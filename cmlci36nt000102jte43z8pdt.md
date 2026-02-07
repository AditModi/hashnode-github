---
title: "EKS Backup with AWS Backup: Finally, a Managed Solution for Kubernetes Disaster Recovery"
datePublished: Fri Nov 28 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmlci36nt000102jte43z8pdt
slug: eks-backup-with-aws-backup-finally-a-managed-solution-for-kubernetes-disaster-recovery
tags: aws

---

I've been running production Kubernetes clusters on EKS for five years, and one conversation has haunted me since day one. It usually happens during quarterly disaster recovery reviews:

**Customer**: "If us-east-1a goes down and takes our EKS cluster with it, how long until we're back online?"

**Me**: "Well... we'd need to rebuild the cluster from scratch, redeploy all applications from CI/CD, restore database backups, reconfigure network policies, service meshes, monitoring..."

**Customer**: "How long?"

**Me**: "Best case? Six hours. Realistically? Maybe twelve if things go wrong."

That's the nightmare scenario no one likes to talk about. EKS manages the control plane, which is great for uptime. But if you lose the cluster configuration—all your Deployments, StatefulSets, ConfigMaps, Secrets, RBAC policies, custom resources—there's no magic "restore" button. You're rebuilding from Git repositories, Terraform configs, and tribal knowledge scattered across documentation and Slack threads.​

The traditional solution has been Velero, the CNCF open-source backup tool. I've deployed it dozens of times. It works, but it's yet another thing to run, monitor, patch, and troubleshoot. S3 bucket configurations, IAM roles, backup schedules, retention policies—all custom infrastructure you maintain forever.

On November 10, 2025, AWS finally shipped what teams like mine have been requesting for years: native EKS integration with AWS Backup. The same centralized backup service you use for RDS, DynamoDB, and EBS now supports full EKS cluster backups—cluster configuration state and persistent volumes, managed as a single unit.

This isn't just "Velero as a managed service." It's deeper integration with AWS's disaster recovery infrastructure: backup vaults with immutability, cross-region/cross-account replication, policy-based lifecycle management, and the ability to restore to a new cluster that AWS Backup provisions for you. No custom scripts, no Velero CRDs to maintain, no S3 bucket configurations to debug at 3am.

Let me show you how it actually works, what it backs up (and what it doesn't), and when you'd still choose Velero.

## Understanding the Backup Architecture

Before we dive into configuration, let's understand what AWS Backup actually captures when it backs up an EKS cluster.

## What Gets Backed Up

An EKS backup creates a **composite recovery point**—a single logical backup containing multiple components:

**1\. EKS Cluster State (Kubernetes Manifests)**

This includes all Kubernetes resources that define your cluster configuration:

* **Workload resources**: Deployments, StatefulSets, DaemonSets, ReplicaSets, Jobs, CronJobs
    
* **Configuration**: ConfigMaps, Secrets
    
* **Storage**: PersistentVolumeClaims, StorageClasses
    
* **Networking**: Services, Ingresses, NetworkPolicies
    
* **RBAC**: Roles, RoleBindings, ClusterRoles, ClusterRoleBindings
    
* **Custom resources**: CRDs and their instances
    
* **EKS-specific configs**: Cluster name, IAM roles, VPC configuration, logging settings, encryption config, add-ons, access entries, managed node groups, Fargate profiles, pod identity associations
    

Essentially, everything you'd see running `kubectl get all --all-namespaces` plus cluster-level configuration.​

**2\. Persistent Storage**

Backups of storage attached via PersistentVolumeClaims, but only for storage types supported by AWS Backup:

* **Amazon EBS volumes**: Via the EBS CSI driver
    
* **Amazon EFS file systems**: Via the EFS CSI driver
    
* **Amazon S3 buckets**: Via the S3 CSI driver (buckets only, not specific prefixes)
    

Each persistent volume becomes a child recovery point within the composite.

## What Does NOT Get Backed Up

This is critical to understand—AWS Backup doesn't capture everything:

❌ **Container images**: Images stored in ECR, Docker Hub, or other registries aren't backed up. You need these to exist when you restore.​

❌ **Infrastructure components**: VPCs, subnets, security groups, NAT gateways, load balancers, Route53 records. These are EKS cluster dependencies, not part of the cluster itself.​

❌ **EKS nodes**: The EC2 instances or Fargate pods running your workloads. On restore, you'll need capacity available (existing nodes or ability to launch new ones).​

❌ **Auto-generated resources**: Kubernetes creates ephemeral resources like Events, Leases, temporary Pods created by Jobs. These aren't backed up.​

❌ **CSI-migrated or in-tree volumes**: Only volumes provisioned via CSI drivers are supported. Legacy in-tree storage provisioners or volumes using CSI migration aren't backed up.​

❌ **FSx for Lustre/NetApp/Windows**: Only EBS, EFS, and S3 via CSI drivers are supported.​

## Composite Recovery Point Structure

Here's what a backup actually looks like:​

```yaml
Composite Recovery Point (Parent)
├── EKS Cluster State Child Recovery Point
│   └── Contains: All Kubernetes manifests, cluster config
│
├── Persistent Volume Child Recovery Point #1
│   └── EBS Snapshot: vol-abc123 (10GB)
│
├── Persistent Volume Child Recovery Point #2
│   └── EFS Backup: fs-def456
│
└── Persistent Volume Child Recovery Point #3
    └── S3 Backup: my-app-bucket
```

Each child recovery point has its own status (`Completed` or `Failed`). The composite status can be:​

* `Completed`: All children backed up successfully
    
* `Partial`: Some children failed or were deleted/disassociated
    
* `Failed`: The backup job failed entirely
    

This structure is important for restores. You can restore the full composite (cluster state + all volumes) or restore individual child recovery points (just the cluster config, or just specific volumes).​

## Prerequisites and Setup

Before you can back up an EKS cluster, you need specific configurations in place.

## EKS Cluster Configuration

Your cluster must use API-based authentication:

```bash
# Check your cluster's authentication mode
aws eks describe-cluster \
  --name my-cluster \
  --query 'cluster.accessConfig.authenticationMode' \
  --output text

# Should return: API or API_AND_CONFIG_MAP
```

AWS Backup needs to create access entries to interact with the Kubernetes API. If your cluster uses `CONFIG_MAP` mode exclusively (the old aws-auth ConfigMap), you need to update it:

```bash
# Update to API_AND_CONFIG_MAP mode (allows both methods)
aws eks update-cluster-config \
  --name my-cluster \
  --access-config authenticationMode=API_AND_CONFIG_MAP
```

This change is non-disruptive—existing ConfigMap-based authentication continues working while enabling API-based access entries.​

## IAM Permissions

AWS Backup needs permissions to access your EKS cluster and storage resources:

```yaml
# IAM role for AWS Backup
resource "aws_iam_role" "backup_eks" {
  name = "AWSBackupEKSRole"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "backup.amazonaws.com"
      }
    }]
  })
}

# Attach managed policy for EKS/EBS/EFS backup
resource "aws_iam_role_policy_attachment" "backup_policy" {
  role       = aws_iam_role.backup_eks.name
  policy_arn = "arn:aws:iam::aws:policy/service-role/AWSBackupServiceRolePolicyForBackup"
}

# If backing up S3 buckets, attach S3 backup policy
resource "aws_iam_role_policy_attachment" "backup_s3_policy" {
  role       = aws_iam_role.backup_eks.name
  policy_arn = "arn:aws:iam::aws:policy/AWSBackupServiceRolePolicyForS3Backup"
}

# Additional permissions for S3 bucket access
resource "aws_iam_role_policy" "s3_bucket_access" {
  name = "EKSBackupS3BucketAccess"
  role = aws_iam_role.backup_eks.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "s3:GetObject",
          "s3:PutObject",
          "s3:DeleteObject",
          "s3:ListBucket"
        ]
        Resource = [
          "arn:aws:s3:::my-app-persistent-bucket",
          "arn:aws:s3:::my-app-persistent-bucket/*"
        ]
      }
    ]
  })
}
```

The managed policy `AWSBackupServiceRolePolicyForBackup` includes permissions for EKS, EBS, and EFS. S3 requires the additional policy because it has more granular permissions requirements.

## Enable EKS in AWS Backup

In the AWS Backup console, you need to opt-in to EKS support:​

```bash
# Via CLI
aws backup put-backup-vault-resource-type \
  --backup-vault-name Default \
  --resource-type EKS

# Or enable in Settings via Console:
# AWS Backup Console → Settings → Service opt-in → Enable EKS
```

This is a one-time global setting per account/region.​

## CSI Drivers for Persistent Volumes

Persistent volumes must use AWS CSI drivers. If you're using legacy in-tree provisioners, you need to migrate:

```bash
# Install EBS CSI driver
eksctl create addon --name aws-ebs-csi-driver --cluster my-cluster

# Install EFS CSI driver
eksctl create addon --name aws-efs-csi-driver --cluster my-cluster

# Install S3 CSI driver (if using S3 for persistent storage)
helm install aws-mountpoint-s3-csi-driver \
  aws-mountpoint-s3-csi-driver/aws-mountpoint-s3-csi-driver \
  --namespace kube-system
```

Verify your StorageClasses use the CSI provisioners:

```bash
kubectl get storageclass

# Example output:
# NAME            PROVISIONER             AGE
# gp3             ebs.csi.aws.com        30d   ✅ Supported
# efs             efs.csi.aws.com        30d   ✅ Supported
# kubernetes.io-aws-ebs (DEPRECATED)     ❌ Not supported
```

Any PVCs using legacy provisioners won't be backed up.​

## Creating Your First Backup

Let's walk through creating both on-demand and scheduled backups.

## On-Demand Backup via Console

This is the quickest way to test:​

1. Open AWS Backup console → **Protected resources** → **Create on-demand backup**
    
2. Select resource type: **EKS**
    
3. Choose your cluster from the dropdown
    
4. Configure backup settings:
    
    * **Backup vault**: Create a new vault or use `Default`
        
    * **IAM role**: Select the role you created earlier
        
    * **Backup window**: Start now or schedule
        
    * **Retention**: How long to keep the backup (e.g., 30 days)
        
5. (Optional) Advanced options:
    
    * **Enable continuous backup**: For point-in-time recovery of EBS volumes
        
    * **Copy to destination**: Cross-region/account copies
        
6. Click **Create on-demand backup**
    

The backup job starts immediately. You can monitor progress in **Backup jobs**.​

## On-Demand Backup via CLI

```bash
# Create on-demand backup
aws backup start-backup-job \
  --backup-vault-name EKS-Backups \
  --resource-arn arn:aws:eks:us-east-1:123456789012:cluster/my-cluster \
  --iam-role-arn arn:aws:iam::123456789012:role/AWSBackupEKSRole \
  --recovery-point-tags Environment=production,Application=webapp

# Monitor backup job
JOB_ID="<backup-job-id-from-previous-command>"
aws backup describe-backup-job --backup-job-id $JOB_ID

# Output:
# {
#   "BackupJobId": "1234ABCD-12AB-34CD-56EF-1234567890AB",
#   "BackupVaultName": "EKS-Backups",
#   "State": "RUNNING",
#   "PercentDone": "45%",
#   "ResourceArn": "arn:aws:eks:us-east-1:123456789012:cluster/my-cluster",
#   "CreationDate": "2026-02-07T20:00:00Z"
# }
```

## Automated Backup Plans

For production, you want scheduled backups with retention policies:​

```yaml
# Backup vault with encryption
resource "aws_backup_vault" "eks_vault" {
  name        = "eks-production-vault"
  kms_key_arn = aws_kms_key.backup.arn
  
  # Lock vault to prevent deletion (compliance)
  force_destroy = false
}

# Backup plan with multiple rules
resource "aws_backup_plan" "eks_plan" {
  name = "eks-daily-backup-plan"
  
  # Daily backup with 30-day retention
  rule {
    rule_name         = "daily_backup"
    target_vault_name = aws_backup_vault.eks_vault.name
    schedule          = "cron(0 3 * * ? *)"  # 3 AM UTC daily
    
    lifecycle {
      delete_after = 30  # Delete after 30 days
    }
    
    copy_action {
      destination_vault_arn = aws_backup_vault.dr_vault.arn
      
      lifecycle {
        delete_after = 90  # Keep DR copies for 90 days
      }
    }
  }
  
  # Weekly backup with longer retention
  rule {
    rule_name         = "weekly_backup"
    target_vault_name = aws_backup_vault.eks_vault.name
    schedule          = "cron(0 4 ? * SUN *)"  # Sundays at 4 AM UTC
    
    lifecycle {
      delete_after = 365  # Keep for 1 year
    }
  }
  
  # Enable advanced backup settings
  advanced_backup_setting {
    backup_options = {
      WindowsVSS = "enabled"
    }
    resource_type = "EC2"
  }
}

# Assign EKS clusters to backup plan using tags
resource "aws_backup_selection" "eks_clusters" {
  name         = "eks-production-clusters"
  plan_id      = aws_backup_plan.eks_plan.id
  iam_role_arn = aws_iam_role.backup_eks.arn
  
  # Select all EKS clusters with specific tags
  selection_tag {
    type  = "STRINGEQUALS"
    key   = "Backup"
    value = "true"
  }
  
  selection_tag {
    type  = "STRINGEQUALS"
    key   = "Environment"
    value = "production"
  }
  
  # Explicitly include specific clusters
  resources = [
    "arn:aws:eks:us-east-1:123456789012:cluster/my-cluster"
  ]
}

# Tag your EKS clusters for automatic backup
resource "aws_eks_cluster" "app" {
  name     = "my-cluster"
  role_arn = aws_iam_role.eks_cluster.arn
  # ... other cluster config
  
  tags = {
    Backup      = "true"
    Environment = "production"
    Application = "webapp"
  }
}
```

This configuration:​

* Creates daily backups at 3 AM, kept for 30 days
    
* Creates weekly backups on Sundays, kept for 1 year
    
* Copies each backup to a DR vault in another region
    
* Automatically backs up any EKS cluster tagged with `Backup=true` and `Environment=production`
    

## Monitoring Backup Jobs

Track backup progress and health:

```bash
# List recent backup jobs
aws backup list-backup-jobs \
  --by-resource-type EKS \
  --max-results 10 \
  --query 'BackupJobs[*].[BackupJobId,State,ResourceArn,CompletionDate]' \
  --output table

# Get detailed backup job info
aws backup describe-backup-job \
  --backup-job-id 1234ABCD-12AB-34CD-56EF-1234567890AB

# List recovery points
aws backup list-recovery-points-by-backup-vault \
  --backup-vault-name eks-production-vault \
  --by-resource-type EKS

# View composite recovery point with child recovery points
aws backup list-recovery-points-by-resource \
  --resource-arn arn:aws:eks:us-east-1:123456789012:cluster/my-cluster
```

Set up CloudWatch alarms for backup failures:

```yaml
resource "aws_cloudwatch_metric_alarm" "backup_failure" {
  alarm_name          = "eks-backup-failure"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "NumberOfBackupJobsFailed"
  namespace           = "AWS/Backup"
  period              = 300
  statistic           = "Sum"
  threshold           = 0
  alarm_description   = "EKS backup job failed"
  treat_missing_data  = "notBreaching"
  
  dimensions = {
    ResourceType = "EKS"
  }
  
  alarm_actions = [aws_sns_topic.alerts.arn]
}
```

## Restoring from Backup

This is where AWS Backup's EKS integration really shines—you can restore to an existing cluster or have AWS Backup create a new cluster for you.

## Restore to Existing Cluster

Use this when you want to recover specific resources or repair a partially damaged cluster:​

```bash
# List available recovery points
aws backup list-recovery-points-by-backup-vault \
  --backup-vault-name eks-production-vault \
  --by-resource-type EKS

# Start restore to existing cluster
aws backup start-restore-job \
  --recovery-point-arn arn:aws:backup:us-east-1:123456789012:recovery-point:composite:eks/my-cluster-20260207 \
  --iam-role-arn arn:aws:iam::123456789012:role/AWSBackupEKSRole \
  --metadata '{
    "eksClusterArn": "arn:aws:eks:us-east-1:123456789012:cluster/my-cluster",
    "restoreType": "EXISTING_CLUSTER",
    "itemsToRestore": "ALL",
    "conflictResolutionStrategy": "OVERWRITE"
  }'
```

Key metadata options:​

* `restoreType`: `EXISTING_CLUSTER` or `NEW_CLUSTER`
    
* `itemsToRestore`: `ALL` (full composite) or specific child recovery point ARNs
    
* `conflictResolutionStrategy`: `OVERWRITE` (replace existing resources) or `SKIP` (keep existing if conflict)
    
* `resourceRestoreOrder`: `DEFAULT` (recommended) or custom order
    

**Important behavior**: Restoring to an existing cluster is non-destructive. It doesn't delete anything already there—it restores the delta between the backup and current state. If a resource exists in the backup but not in the cluster, it's created. If it exists in both, the behavior depends on `conflictResolutionStrategy`.​

## Restore to New Cluster

This is the disaster recovery scenario—your original cluster is gone and you need to rebuild:​

```bash
# Restore and create new cluster
aws backup start-restore-job \
  --recovery-point-arn arn:aws:backup:us-east-1:123456789012:recovery-point:composite:eks/my-cluster-20260207 \
  --iam-role-arn arn:aws:iam::123456789012:role/AWSBackupEKSRole \
  --metadata '{
    "restoreType": "NEW_CLUSTER",
    "clusterName": "my-cluster-restored",
    "vpcId": "vpc-abc123",
    "subnetIds": ["subnet-abc123", "subnet-def456"],
    "securityGroupIds": ["sg-abc123"],
    "kubernetesVersion": "1.28",
    "itemsToRestore": "ALL"
  }'
```

AWS Backup will:​

1. Create a new EKS cluster with the specified networking config
    
2. Wait for the cluster to become active
    
3. Restore the Kubernetes manifests (cluster state)
    
4. Restore persistent volumes
    
5. Apply all resources in the appropriate order
    

This is huge. Previously, with Velero or custom scripts, you had to pre-provision the target cluster, configure IAM, set up networking, install CSI drivers, etc., before you could even start restoring. AWS Backup handles cluster creation as part of the restore process.​

**Critical consideration**: The new cluster inherits configuration from the backup (Kubernetes version, encryption settings, etc.) but you specify networking parameters (VPC, subnets, security groups). Make sure these resources exist in your target account/region.​

## Selective Restore

You don't have to restore everything. You can restore individual components:​

```bash
# List child recovery points within composite
aws backup list-recovery-points-by-parent \
  --parent-recovery-point-arn arn:aws:backup:us-east-1:123456789012:recovery-point:composite:eks/my-cluster-20260207

# Output shows:
# - eks/my-cluster-20260207/config (cluster state)
# - ebs/vol-abc123 (PV #1)
# - efs/fs-def456 (PV #2)

# Restore only specific PersistentVolume
aws backup start-restore-job \
  --recovery-point-arn arn:aws:backup:us-east-1:123456789012:recovery-point:ebs/vol-abc123 \
  --iam-role-arn arn:aws:iam::123456789012:role/AWSBackupEKSRole \
  --metadata '{
    "availabilityZone": "us-east-1a",
    "iops": "3000",
    "volumeType": "gp3"
  }'
```

This creates a new EBS volume from the snapshot. You'd then create a PersistentVolume and PersistentVolumeClaim pointing to this volume:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: restored-pv
spec:
  capacity:
    storage: 100Gi
  accessModes:
    - ReadWriteOnce
  csi:
    driver: ebs.csi.aws.com
    volumeHandle: vol-restored-abc123  # From restore job
  storageClassName: gp3
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: restored-pvc
  namespace: default
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Gi
  volumeName: restored-pv
  storageClassName: gp3
```

## Namespace-Level Restore

You can restore specific namespaces only:​

```bash
aws backup start-restore-job \
  --recovery-point-arn arn:aws:backup:us-east-1:123456789012:recovery-point:composite:eks/my-cluster-20260207 \
  --iam-role-arn arn:aws:iam::123456789012:role/AWSBackupEKSRole \
  --metadata '{
    "eksClusterArn": "arn:aws:eks:us-east-1:123456789012:cluster/my-cluster",
    "restoreType": "EXISTING_CLUSTER",
    "namespacesToRestore": ["production", "staging"],
    "conflictResolutionStrategy": "SKIP"
  }'
```

This restores only resources in the `production` and `staging` namespaces. Useful for recovering accidentally deleted namespaces without affecting the rest of the cluster.​

**Limitation**: Namespace-level restore only works to existing clusters. You can't create a new cluster and restore just specific namespaces—that wouldn't make sense since cluster-wide resources wouldn't be present.

## Monitoring Restore Jobs

```bash
# List restore jobs
aws backup list-restore-jobs \
  --by-resource-type EKS \
  --max-results 10

# Get restore job details
aws backup describe-restore-job \
  --restore-job-id 5678EFGH-56EF-78GH-90IJ-567890ABCDEF

# Check status of child restore jobs
aws backup list-restore-jobs \
  --by-parent-recovery-point-arn arn:aws:backup:us-east-1:123456789012:recovery-point:composite:eks/my-cluster-20260207
```

Restore times vary based on cluster size:​

* Small cluster (10 nodes, 100 pods, 5 PVs): ~15 minutes
    
* Medium cluster (50 nodes, 500 pods, 20 PVs): ~30-45 minutes
    
* Large cluster (200 nodes, 2000 pods, 100 PVs): ~1-2 hours
    

Most time is spent waiting for PersistentVolumes to restore from snapshots and for pods to become healthy.​

## AWS Backup vs. Velero: The Honest Comparison

The question everyone asks: "Should I migrate from Velero to AWS Backup?"

## When to Use AWS Backup

✅ **AWS-only infrastructure**: If you're all-in on AWS and not running Kubernetes anywhere else​

✅ **Compliance requirements**: Need immutable backups, cross-region/account copies, audit trails​

✅ **Centralized backup management**: Want one tool for RDS, DynamoDB, EBS, EFS, S3, and now EKS​

✅ **Operational simplicity**: Don't want to run/maintain Velero infrastructure (controller pods, S3 buckets, IAM policies)

✅ **New to EKS backups**: If you're starting from scratch, AWS Backup is simpler​

✅ **Restore to new cluster**: Value the ability to have AWS provision the target cluster during restore​

## When to Use Velero

✅ **Multi-cloud/hybrid**: Running Kubernetes on EKS, GKE, AKS, or on-premises

✅ **Fine-grained control**: Need granular backup/restore filtering (by label, annotation, resource type)​

✅ **Application-aware backups**: Need pre/post-backup hooks for database consistency​

✅ **Cluster migration**: Regularly migrating workloads between clusters​

✅ **Backup validation**: Need to programmatically validate backup contents before restore​

✅ **Cost sensitivity**: Velero is free (open-source), AWS Backup charges per backup and per GB stored​

## Feature Comparison Table

| **Feature** | **AWS Backup** | **Velero** |
| --- | --- | --- |
| Cluster state backup | ✅ Yes | ✅ Yes |
| Persistent volume backup | ✅ EBS, EFS, S3 only | ✅ Any CSI driver + plugins |
| Immutable backups | ✅ Built-in | ⚠️ Requires S3 Object Lock |
| Cross-region copy | ✅ Automated | ⚠️ Manual with restic |
| Cross-account copy | ✅ Automated | ⚠️ Manual setup |
| Restore to new cluster | ✅ AWS creates cluster | ❌ Must pre-provision |
| Selective restore | ⚠️ Namespace-level | ✅ Resource-level granularity |
| Backup hooks | ❌ Not supported | ✅ Pre/post-backup hooks |
| Multi-cloud support | ❌ AWS only | ✅ Any Kubernetes |
| Cost | $0.05/GB/month + job fees | Free (S3 storage costs only) |
| Operational overhead | ✅ Fully managed | ⚠️ Run Velero controller |
| Compliance certifications | ✅ Inherits AWS Backup certs | ⚠️ Self-managed compliance |

## Migration Path from Velero

If you're currently using Velero and want to evaluate AWS Backup:​

**Phase 1: Parallel backups (1-2 weeks)**

* Enable AWS Backup for your cluster
    
* Keep Velero running
    
* Compare backup size, duration, restore times
    
* Test restores with both tools
    

**Phase 2: Validation (2-4 weeks)**

* Perform disaster recovery drill using AWS Backup
    
* Verify all critical workloads restore correctly
    
* Check for any unsupported features you rely on
    

**Phase 3: Cutover (1 week)**

* Switch to AWS Backup as primary backup tool
    
* Keep Velero backups as fallback for 30 days
    
* Uninstall Velero after confidence period
    

**Don't migrate if**:

* You use Velero backup hooks for application consistency
    
* You need to back up resources selectively by labels
    
* You're running multi-cloud Kubernetes
    
* You have custom Velero plugins for specialized storage
    

## Costs and Limitations

Let's be transparent about what this costs and what doesn't work.

## Pricing Breakdown

AWS Backup for EKS charges for:​

* **Backup storage**: $0.05/GB/month for warm storage, $0.01/GB/month for cold storage
    
* **Restore**: $0.02/GB restored
    
* **Data transfer**: Standard AWS data transfer rates for cross-region copies
    

**Example calculation** for a medium cluster:

* **Cluster state**: 500MB (Kubernetes manifests, configs) = $0.025/month
    
* **Persistent volumes**: 1TB total across all PVCs = $50/month
    
* **Daily backup for 30 days**: ~$50/month (assuming incremental backups for volumes)
    
* **Cross-region copy**: ~$50/month additional for DR copy
    
* **Total**: ~$100/month for backup storage
    

Compare to Velero:

* **Velero**: Free software
    
* **S3 storage**: Same ~$50/month for backup data
    
* **EC2 costs**: ~$10/month for Velero controller (t3.small)
    
* **Total**: ~$60/month
    

AWS Backup is ~40% more expensive, but that doesn't account for:

* Engineering time maintaining Velero
    
* Complexity of cross-region/account copies
    
* Immutability features
    
* Integration with centralized backup reporting
    

For most production environments, the operational simplicity is worth the premium.

## Current Limitations

Be aware of these constraints:

**1\. CSI driver requirement**: Only volumes provisioned via AWS CSI drivers are backed up. Legacy in-tree provisioners aren't supported.​

**2\. No FSx support**: FSx for Lustre, FSx for NetApp ONTAP, FSx for Windows File Server aren't backed up via EKS integration.​

**3\. No S3 prefix support**: You can back up S3 buckets attached as PVs, but not specific prefixes within buckets.​

**4\. Snapshot-only for S3**: S3 bucket backups are snapshots, not continuous backups.​

**5\. No EKS on Outposts**: Outposts clusters aren't supported.​

**6\. No container images**: Images in ECR or Docker Hub must exist when you restore.​

**7\. Restore ordering**: While you can specify custom order, AWS recommends the default order. Complex interdependencies might require manual intervention.​

**8\. Cross-version limitations**: Restoring a Kubernetes 1.28 cluster backup to a 1.30 cluster might fail if APIs changed.​

## Wrapping Up

AWS Backup's EKS integration finally gives Kubernetes users what they've had for other AWS services all along: centralized, policy-driven, enterprise-grade backup and restore. No more custom scripts, no more Velero maintenance, no more wondering if your disaster recovery plan will actually work when you need it.

The feature launched in November 2025, and it's already production-ready. The architecture is solid: composite recovery points intelligently group cluster state and persistent volumes, cross-region/account copies provide true DR capabilities, and immutable vaults protect against ransomware and accidental deletion.

For teams running entirely on AWS, this is the backup solution you should default to. The cost premium over Velero (roughly 40%) is offset by reduced operational burden and better integration with AWS's security and compliance tooling. For multi-cloud environments or teams needing fine-grained restore control, Velero remains the better choice.

Start simple: enable AWS Backup for one non-critical EKS cluster, create a backup, perform a test restore. Once you're confident, expand to production with automated backup plans, cross-region copies, and regular DR drills. The "what if the cluster disappears tomorrow" conversation gets a lot less scary when you have verified, immutable backups and a tested restore procedure.

And for those of us who have spent years maintaining custom backup solutions—we can finally delete that code and let AWS handle it.
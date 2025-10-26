---
title: "Securing AWS RDS Passwords in Terraform: Two Production-Ready Approaches"
datePublished: Fri Oct 17 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmh7cv5o4000002l10tve0yeo
slug: securing-aws-rds-passwords-in-terraform-two-production-ready-approaches
tags: aws, hashicorp

---

If you're managing AWS RDS databases with Terraform, you've likely encountered a critical security dilemma: even when you carefully store passwords in AWS Secrets Manager or HashiCorp Vault, they still end up exposed in plain text within your Terraform state file. This defeats the entire purpose of using a secrets management solution and creates a significant security vulnerability.

Today, we'll explore two production-ready approaches that eliminate this problem entirely, allowing you to provision RDS databases securely without ever storing passwords in Terraform state.

## The Problem: Terraform State Exposes Your Secrets

When you create an AWS RDS instance using Terraform, you typically provide a master password either directly in your configuration or by referencing a secret from a vault. Here's what commonly happens:​

```json
resource "aws_db_instance" "example" {
  identifier        = "mydb"
  engine            = "postgres"
  instance_class    = "db.t3.micro"
  allocated_storage = 20
  
  username = "admin"
  password = data.aws_secretsmanager_secret_version.db_password.secret_string
  
  # other configuration...
}
```

Even though you're pulling the password from Secrets Manager, Terraform writes the actual password value into its **state file in plain text**. Anyone with access to your state file—whether stored in S3, Terraform Cloud, or locally—can read the database password directly. State files are often shared across teams, stored in version control (accidentally), or accessible to CI/CD systems, multiplying the exposure risk.​

## Approach 1: IAM Authentication with Ephemeral Bootstrap Password

This approach eliminates long-lived passwords entirely by leveraging AWS IAM database authentication, where users authenticate using temporary IAM tokens instead of passwords.​

## How It Works

The strategy involves four key steps: create an ephemeral random password that never touches state, use it only during database creation, immediately grant IAM authentication to the master user, and nullify the password so it becomes irrelevant.​

## Implementation

**Step 1: Generate an Ephemeral Random Password**

```json
resource "random_password" "master_password" {
  length  = 30
  special = false
}
```

This `random_password` resource generates a secure password during Terraform execution, but critically, it's not stored in state or output logs.​

**Step 2: Create RDS Instance with IAM Authentication Enabled**

```json
resource "aws_db_instance" "app" {
  identifier     = "myapp-db"
  engine         = "postgres"
  engine_version = "15.4"
  instance_class = "db.t3.micro"
  
  allocated_storage = 20
  storage_encrypted = true
  
  db_name  = "appdb"
  username = "dbadmin"
  password = random_password.master_password.result
  
  # Enable IAM authentication
  iam_database_authentication_enabled = true
  
  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.db.name
  
  backup_retention_period = 7
  skip_final_snapshot    = false
  final_snapshot_identifier = "myapp-db-final-snapshot"
  
  tags = {
    Environment = "production"
    ManagedBy   = "terraform"
  }
}
```

The `password` argument is **write-only**—Terraform uses it to create the database but doesn't read it back or store it in state.​

**Step 3: Create IAM Policy for Database Access**

```json
data "aws_caller_identity" "current" {}

resource "aws_iam_policy" "rds_iam_auth" {
  name        = "rds-iam-authentication"
  description = "Allow IAM authentication to RDS"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "rds-db:connect"
        ]
        Resource = [
          "arn:aws:rds-db:${var.region}:${data.aws_caller_identity.current.account_id}:dbuser:${aws_db_instance.app.resource_id}/dbadmin"
        ]
      }
    ]
  })
}

resource "aws_iam_group" "database_users" {
  name = "DatabaseUsers"
}

resource "aws_iam_group_policy_attachment" "rds_auth" {
  group      = aws_iam_group.database_users.name
  policy_arn = aws_iam_policy.rds_iam_auth.arn
}
```

This policy grants the `rds-db:connect` permission, allowing IAM users or roles to generate authentication tokens.​

**Step 4: Grant IAM Authentication to Database User**

After the database is created, connect using the ephemeral password one time to configure IAM authentication:

```sql
-- Connect using the bootstrap password (one-time use)
psql -h myapp-db.abc123.us-east-1.rds.amazonaws.com -U dbadmin -d appdb

-- Grant IAM authentication capability
GRANT rds_iam TO dbadmin;

-- Nullify the password - from now on, only IAM auth works
ALTER USER dbadmin WITH PASSWORD NULL;
```

You can automate this using a Terraform null resource with a local-exec provisioner, though be cautious about running SQL commands in Terraform.​

**Step 5: Connect Using IAM Authentication**

Now that IAM authentication is enabled, users connect by generating temporary authentication tokens:

```bash
# Generate a 15-minute authentication token
export PGPASSWORD=$(aws rds generate-db-auth-token \
  --hostname myapp-db.abc123.us-east-1.rds.amazonaws.com \
  --port 5432 \
  --username dbadmin \
  --region us-east-1)

# Connect using the token
psql -h myapp-db.abc123.us-east-1.rds.amazonaws.com \
     -U dbadmin \
     -d appdb \
     --set=sslmode=require
```

The token expires after 15 minutes, providing time-limited access without long-lived credentials.​

## Benefits of IAM Authentication

**No password management**: The bootstrap password is used once and immediately nullified—from that point forward, no password exists. **Centralized access control**: Database access is managed through IAM policies you already use for other AWS services. **Automatic encryption**: Network traffic is encrypted via SSL/TLS when using IAM authentication. **Audit trails**: CloudTrail logs show who accessed the database using specific IAM identities, not generic database usernames. **No credential rotation**: Since authentication happens via temporary tokens, there's no need to rotate master passwords periodically.​

## Limitations to Consider

IAM database authentication has a limit of approximately **200 new connections per second** due to token generation overhead. If your application creates connections more frequently, consider using connection pooling or RDS Proxy to reuse connections and mitigate this limitation. IAM authentication works with **MySQL, MariaDB, and PostgreSQL** but not with other database engines.​

## Approach 2: Secrets Manager with Ephemeral Password (No State Exposure)

For applications that require traditional password-based authentication—perhaps due to third-party tools, legacy applications, or engines that don't support IAM authentication—you can still avoid storing passwords in Terraform state using this pattern.​

## How It Works

Generate an ephemeral random password that never enters Terraform state, pass it directly to both the RDS instance and Secrets Manager during creation, and let Secrets Manager handle password rotation going forward.​

## Implementation

**Step 1: Generate Ephemeral Random Password**

```json
resource "random_password" "master_password" {
  length  = 30
  special = true
  
  # Ensure it meets RDS password requirements
  override_special = "!#$%^&*()-_=+[]{}:?"
}
```

**Step 2: Create Secrets Manager Secret**

```json
resource "aws_secretsmanager_secret" "db_password" {
  name = "myapp-db-master-password"
  description = "Master password for myapp RDS database"
  
  recovery_window_in_days = 7
  
  tags = {
    Environment = "production"
    ManagedBy   = "terraform"
  }
}

resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = jsonencode({
    username = "dbadmin"
    password = random_password.master_password.result
    engine   = "postgres"
    host     = aws_db_instance.app.endpoint
    port     = aws_db_instance.app.port
    dbname   = aws_db_instance.app.db_name
  })
}
```

The password flows directly from the `random_password` resource to Secrets Manager without being stored in Terraform state.​

**Step 3: Create RDS Instance**

```json
resource "aws_db_instance" "app" {
  identifier     = "myapp-db"
  engine         = "postgres"
  engine_version = "15.4"
  instance_class = "db.t3.micro"
  
  allocated_storage = 20
  storage_encrypted = true
  
  db_name  = "appdb"
  username = "dbadmin"
  password = random_password.master_password.result
  
  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.db.name
  
  backup_retention_period = 7
  skip_final_snapshot    = false
  final_snapshot_identifier = "myapp-db-final-snapshot"
  
  # Optional: Let RDS manage rotation automatically
  manage_master_user_password = false
  
  tags = {
    Environment = "production"
    ManagedBy   = "terraform"
  }
}
```

Again, the `password` argument is write-only and never stored in state.​

**Step 4: Configure Automatic Password Rotation (Optional)**

```json
resource "aws_secretsmanager_secret_rotation" "db_password" {
  secret_id           = aws_secretsmanager_secret.db_password.id
  rotation_lambda_arn = aws_lambda_function.rotate_db_password.arn
  
  rotation_rules {
    automatically_after_days = 30
  }
}

# You'll need to create a Lambda function that handles rotation
# AWS provides templates for common rotation scenarios
```

Secrets Manager can automatically rotate the password on a schedule, updating both the secret and the database password.​

**Step 5: Grant Application Access to the Secret**

```json
resource "aws_iam_role_policy" "app_read_secret" {
  role = aws_iam_role.app.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "secretsmanager:GetSecretValue"
        ]
        Resource = [
          aws_secretsmanager_secret.db_password.arn
        ]
      }
    ]
  })
}
```

**Step 6: Application Code Retrieves Password**

Your application retrieves the password at runtime from Secrets Manager:

```python
import boto3
import json

def get_database_credentials():
    client = boto3.client('secretsmanager', region_name='us-east-1')
    
    response = client.get_secret_value(SecretId='myapp-db-master-password')
    secret = json.loads(response['SecretString'])
    
    return {
        'host': secret['host'],
        'port': secret['port'],
        'username': secret['username'],
        'password': secret['password'],
        'database': secret['dbname']
    }

# Use in connection
credentials = get_database_credentials()
connection_string = f"postgresql://{credentials['username']}:{credentials['password']}@{credentials['host']}:{credentials['port']}/{credentials['database']}"
```

## Benefits of Secrets Manager Approach

**Universal compatibility**: Works with any database engine and any application framework. **Automatic rotation**: Secrets Manager can rotate passwords automatically without downtime. **No state exposure**: The password never appears in Terraform state—it flows directly from generation to storage. **Centralized secret management**: All secrets live in one place with consistent access patterns. **Version history**: Secrets Manager maintains previous versions, enabling rollback if needed.​

## RDS-Managed Secrets (Alternative Pattern)

AWS also offers native integration where RDS automatically creates and manages the Secrets Manager secret:

```json
resource "aws_db_instance" "app" {
  identifier     = "myapp-db"
  engine         = "postgres"
  engine_version = "15.4"
  instance_class = "db.t3.micro"
  
  allocated_storage = 20
  storage_encrypted = true
  
  db_name  = "appdb"
  username = "dbadmin"
  
  # Let RDS manage the password entirely
  manage_master_user_password = true
  master_user_secret_kms_key_id = aws_kms_key.db_secrets.id
  
  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.db.name
  
  backup_retention_period = 7
  skip_final_snapshot    = false
  final_snapshot_identifier = "myapp-db-final-snapshot"
}

# Reference the RDS-created secret
output "master_user_secret_arn" {
  value = aws_db_instance.app.master_user_secret[0].secret_arn
}
```

With `manage_master_user_password = true`, RDS creates a secret in Secrets Manager automatically, rotates it, and Terraform never sees the password at all. This is the simplest approach but requires RDS to own the entire secret lifecycle.​

## Comparison: When to Use Each Approach

## Choose IAM Authentication When

Your database engine supports it (MySQL, MariaDB, PostgreSQL). Your application creates fewer than 200 new connections per second. You want to eliminate password management entirely. You need granular, IAM-based access control with CloudTrail auditing. Your team is comfortable with IAM-based authentication patterns. You're building new applications or microservices with modern authentication requirements.​

## Choose Secrets Manager When

You're working with database engines that don't support IAM authentication (Oracle, SQL Server, etc.). Your application or third-party tools require traditional password authentication. You need automatic password rotation with minimal code changes. You have legacy applications that can't easily adopt IAM authentication. You want universal compatibility across all database types. Your connection rate exceeds IAM authentication limits.​

## Hybrid Approach: Best of Both Worlds

For maximum flexibility, you can implement both approaches simultaneously:

```json
resource "random_password" "master_password" {
  length  = 30
  special = false
}

resource "aws_db_instance" "app" {
  identifier     = "myapp-db"
  engine         = "postgres"
  engine_version = "15.4"
  instance_class = "db.t3.micro"
  
  allocated_storage = 20
  storage_encrypted = true
  
  db_name  = "appdb"
  username = "dbadmin"
  password = random_password.master_password.result
  
  # Enable IAM authentication
  iam_database_authentication_enabled = true
  
  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.db.name
  
  backup_retention_period = 7
  skip_final_snapshot    = false
  final_snapshot_identifier = "myapp-db-final-snapshot"
}

# Store the password in Secrets Manager as emergency backup
resource "aws_secretsmanager_secret_version" "db_password" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = jsonencode({
    username = "dbadmin"
    password = random_password.master_password.result
    engine   = "postgres"
    host     = aws_db_instance.app.endpoint
    port     = aws_db_instance.app.port
    dbname   = aws_db_instance.app.db_name
  })
}
```

This provides IAM authentication for day-to-day operations while maintaining a password in Secrets Manager for emergency access or third-party tools that can't use IAM.​

## Security Best Practices

Regardless of which approach you choose, follow these additional security practices:

**Encrypt your state file**: If using S3 backend, enable encryption at rest and versioning. **Restrict state access**: Use IAM policies to limit who can read Terraform state. **Enable database encryption**: Always set `storage_encrypted = true` on RDS instances. **Use VPC security groups**: Never expose databases to the public internet. **Enable automated backups**: Set appropriate `backup_retention_period` values. **Monitor access patterns**: Use CloudWatch and CloudTrail to detect anomalous database access. **Implement least privilege**: Grant only necessary permissions to IAM users and roles.​

## Complete Working Example

Here's a complete Terraform configuration demonstrating the IAM authentication approach:

```json
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    random = {
      source  = "hashicorp/random"
      version = "~> 3.5"
    }
  }
}

provider "aws" {
  region = var.region
}

variable "region" {
  type    = string
  default = "us-east-1"
}

# Generate ephemeral password
resource "random_password" "master_password" {
  length  = 30
  special = false
}

# VPC and networking
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
}

resource "aws_subnet" "private_1" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "${var.region}a"
}

resource "aws_subnet" "private_2" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.2.0/24"
  availability_zone = "${var.region}b"
}

resource "aws_db_subnet_group" "db" {
  name       = "myapp-db-subnet-group"
  subnet_ids = [aws_subnet.private_1.id, aws_subnet.private_2.id]
}

resource "aws_security_group" "db" {
  name        = "myapp-db-sg"
  description = "Security group for RDS database"
  vpc_id      = aws_vpc.main.id

  ingress {
    from_port   = 5432
    to_port     = 5432
    protocol    = "tcp"
    cidr_blocks = [aws_vpc.main.cidr_block]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# RDS instance with IAM authentication
resource "aws_db_instance" "app" {
  identifier     = "myapp-db"
  engine         = "postgres"
  engine_version = "15.4"
  instance_class = "db.t3.micro"
  
  allocated_storage = 20
  storage_encrypted = true
  
  db_name  = "appdb"
  username = "dbadmin"
  password = random_password.master_password.result
  
  iam_database_authentication_enabled = true
  
  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.db.name
  
  backup_retention_period = 7
  skip_final_snapshot    = true
  
  tags = {
    Environment = "production"
    ManagedBy   = "terraform"
  }
}

# IAM policy for database access
data "aws_caller_identity" "current" {}

resource "aws_iam_policy" "rds_iam_auth" {
  name        = "rds-iam-authentication"
  description = "Allow IAM authentication to RDS"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "rds-db:connect"
        ]
        Resource = [
          "arn:aws:rds-db:${var.region}:${data.aws_caller_identity.current.account_id}:dbuser:${aws_db_instance.app.resource_id}/dbadmin"
        ]
      }
    ]
  })
}

resource "aws_iam_group" "database_users" {
  name = "DatabaseUsers"
}

resource "aws_iam_group_policy_attachment" "rds_auth" {
  group      = aws_iam_group.database_users.name
  policy_arn = aws_iam_policy.rds_iam_auth.arn
}

# Outputs
output "db_endpoint" {
  value = aws_db_instance.app.endpoint
}

output "db_resource_id" {
  value = aws_db_instance.app.resource_id
}

output "connection_command" {
  value = <<-EOT
    # Generate authentication token:
    export PGPASSWORD=$(aws rds generate-db-auth-token \
      --hostname ${split(":", aws_db_instance.app.endpoint)[0]} \
      --port 5432 \
      --username dbadmin \
      --region ${var.region})
    
    # Connect to database:
    psql -h ${split(":", aws_db_instance.app.endpoint)[0]} \
         -U dbadmin \
         -d appdb \
         --set=sslmode=require
  EOT
}
```

## Conclusion

Storing database passwords in Terraform state is a security anti-pattern that undermines your entire secrets management strategy. Both approaches outlined in this post—IAM authentication and Secrets Manager with ephemeral passwords—eliminate this vulnerability while providing production-grade security.​

IAM authentication represents the modern, cloud-native approach, eliminating passwords entirely and leveraging temporary tokens for access. It's ideal for new applications and teams comfortable with IAM-based patterns.​

Secrets Manager with ephemeral passwords provides universal compatibility while still preventing state exposure. It works with any database engine and integrates seamlessly with applications that require traditional authentication.​

Choose the approach that best fits your technical requirements, team expertise, and operational constraints—or implement both for maximum flexibility. Either way, you'll sleep better knowing your database credentials are truly secure, not lurking in plain text within your Terraform state file.

The days of accepting password exposure in infrastructure code are over. It's time to implement proper secret management—your security team will thank you.
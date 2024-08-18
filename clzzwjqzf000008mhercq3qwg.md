---
title: "Mastering Multi-Account Management with AWS Control Tower and Landing Zones - Part 3: Advanced Security, Compliance, and Automation"
datePublished: Sun Aug 18 2024 18:30:13 GMT+0000 (Coordinated Universal Time)
cuid: clzzwjqzf000008mhercq3qwg
slug: mastering-multi-account-management-with-aws-control-tower-and-landing-zones-part-3-advanced-security-compliance-and-automation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723811271933/459b4e8a-cdfb-439c-8060-067b3070e667.png
tags: aws

---

Table of Contents for Part 3:

1. Introduction
    
2. Advanced Security and Compliance in AWS Control Tower 2.1. Control Types: Detective, Preventive, and Proactive 2.2. Encryption at Scale 2.3. AWS Security Hub and Control Tower Integration 2.4. Service Control Policies (SCPs) for Cost Control 2.5. Tagging Strategies for Resource Management
    
3. Automating Landing Zones with AWS Control Tower Account Factory for Terraform (AFT) 3.1. Introduction to AFT 3.2. AFT Implementation Demo
    
4. Conclusion
    

Welcome to the final part of our comprehensive guide on mastering multi-account management with AWS Control Tower and Landing Zones. In this section, we'll delve into advanced security and compliance features, explore various control types, discuss encryption at scale, and examine the integration between AWS Security Hub and Control Tower. We'll also cover Service Control Policies (SCPs) for cost control and effective tagging strategies.

To cap it all off, we'll provide a demo of automating landing zones using AWS Control Tower Account Factory for Terraform (AFT), showcasing how you can streamline and scale your multi-account management even further.

2. ### Advanced Security and Compliance in AWS Control Tower
    

**2.1. Control Types: Detective, Preventive, and Proactive**

AWS Control Tower implements three types of controls, each serving a specific purpose in maintaining security and compliance:

a) Detective Controls:

* Purpose: Identify non-compliance after it occurs
    
* Example: AWS Config rule to detect public S3 buckets
    
* Implementation: Use AWS Config Rules and AWS Security Hub
    

Best Practices:

* Enable relevant AWS Config Rules across all accounts
    
* Set up alerts for non-compliant resources
    
* Implement automated remediation where possible
    

b) Preventive Controls:

* Purpose: Stop non-compliant actions before they occur
    
* Example: SCP to prevent deletion of CloudTrail trails
    
* Implementation: Use Service Control Policies (SCPs) and IAM policies
    

Best Practices:

* Implement least-privilege access
    
* Use SCPs to enforce organization-wide policies
    
* Regularly review and update preventive controls
    

c) Proactive Controls:

* Purpose: Guide users towards compliance and best practices
    
* Example: AWS Service Catalog to provide pre-approved resources
    
* Implementation: Use AWS Service Catalog and CloudFormation templates
    

Best Practices:

* Create a curated catalog of compliant resources
    
* Educate users on the importance of using approved resources
    
* Regularly update the catalog based on evolving needs and best practices
    

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUebPIoksHMc6Re6OfTc-wEjwEQHyNCiMBm8PyhhJn6dpzaSlh014k5w4fmPL-yebQ6-1SAgAN8PmOf-y-abBaTcKLj8fMU9tdPlNqs-OHJTXqDzhRDvC3tPF_E-J6HbxiJyt5f-HIjHKW2Vhj_-s3K8pXGcW9vKWhU76ZzqNtzpY0YtsQyCX-4=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUdHiMuwJ4bMviPnLKXqmAmoCCv-d0rQzrHps5Kcd6FVYk9YdfTYpBVPr81ZH_JEA6Wm3_oyF4VynyUCV4LGH0V0bfQVm56t9fTKKH9O48kITcH_P0nm0QH-yvrEAMb3n0j-J7LTnStFa_Xe9EQ6ZNoKXDcpss9PtaBG4SjgwGW8dr2Nhx48fG8=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

**2.2. Encryption at Scale**

Implementing encryption at scale is crucial for protecting data across your multi-account environment. AWS Control Tower can help manage encryption consistently:

a) AWS Key Management Service (KMS) Integration:

* Use AWS KMS to manage encryption keys across accounts
    
* Implement automatic key rotation for enhanced security
    

b) Default Encryption for S3 Buckets:

* Enable default encryption for all S3 buckets using AWS KMS keys
    
* Use SCPs to enforce encryption on new buckets
    

c) EBS Volume Encryption:

* Enable default encryption for EBS volumes in all accounts
    
* Use AWS Config Rules to detect and remediate unencrypted volumes
    

d) RDS Encryption:

* Enforce encryption for all RDS instances using AWS KMS keys
    
* Implement a Config Rule to ensure compliance
    

Best Practices:

* Use a centralized KMS key management strategy
    
* Implement key aliases for easier management across accounts
    
* Regularly audit and rotate encryption keys
    
* Use AWS CloudHSM for workloads requiring dedicated hardware security modules
    

**2.3. AWS Security Hub and Control Tower Integration**

The integration of AWS Security Hub with Control Tower provides a comprehensive security posture management solution:

a) Automated Enablement:

* Security Hub is automatically enabled in all accounts managed by Control Tower
    

b) Centralized Dashboard:

* View security findings from all accounts in a single dashboard
    
* Prioritize and track remediation efforts across your organization
    

c) Compliance Standards:

* Enable and monitor compliance with standards like CIS AWS Foundations Benchmark, PCI DSS, and NIST 800-53
    
* Create custom security standards tailored to your organization's needs
    

d) Integration with Third-party Tools:

* Extend Security Hub's capabilities by integrating with partner solutions
    

Best Practices:

* Regularly review Security Hub findings and track remediation progress
    
* Use Security Hub's custom insights to focus on specific security concerns
    
* Implement automated remediation for common issues using AWS Systems Manager Automation
    
* Conduct regular security posture reviews using Security Hub data
    

2.4. Service Control Policies (SCPs) for Cost Control

SCPs are a powerful tool for implementing cost control measures across your organization:

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUe4jzrxm06pWPzvdYEPPEX339XoGfWp0fuopi-wCIe3Xqa8ckjYy528JQocSvfAB3utntvSExtwivQ56VAuAE9rLkyY7ppJ1ISkw0ntcB3n5SJeKyUvvuLv2qAx6lXJ-ZJxTuzo4qApOV5bBY-xhd22BDriMtjK49Y7f7oC0iBOh4MknpFJklI=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

a) Restrict Expensive Services:

* Use SCPs to prevent the use of costly services in non-production environments
    
* Example SCP:
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723810722962/78a2e5fd-0625-4476-b1ea-76a593afdfec.png align="center")
    

b) Enforce Tagging Policies:

* Require cost allocation tags on all resources
    
* Deny resource creation without proper tags
    

c) Limit Regional Usage:

* Restrict resource creation to specific AWS regions to control costs and compliance
    

Best Practices:

* Start with least-privilege SCPs and gradually allow necessary actions
    
* Regularly review and update SCPs based on changing business needs
    
* Use AWS Cost Explorer and AWS Budgets to monitor the effectiveness of your cost control measures
    

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUf9akzKAosWnFyt_I_tzpvg3Nm7p4aVRab6lzE0oZ4M7-mc18jZokkLFYR8XfMrrqOwBanqaqPqukQmlskesl56jybitkFZOOKtwvRC37sZwpYzf4ywkyzFQNBfSqfuq-TSXcat8ImGnvTOIYY_bhg4B7xXd7Q_onN4_3DZsClImbh7IFnMt-4=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

**2.5. Tagging Strategies for Resource Management**

Implementing a comprehensive tagging strategy is crucial for effective resource management and cost allocation:

a) Mandatory Tags:

* Enforce mandatory tags such as Environment, Project, Owner, and CostCenter
    
* Use AWS Organizations Tag Policies to ensure consistent tagging across accounts
    

b) Automated Tagging:

* Implement Lambda functions to automatically tag resources based on creation context
    
* Use AWS Config Rules to detect and remediate improperly tagged resources
    

c) Tag-based Access Control:

* Use resource tags in IAM policies to control access to resources
    

d) Cost Allocation:

* Use tags for detailed cost allocation reports in AWS Cost Explorer
    
* Set up tag-based billing alarms using AWS Budgets
    

Best Practices:

* Develop a clear, organization-wide tagging taxonomy
    
* Educate teams on the importance of proper tagging
    
* Regularly audit and clean up tags
    
* Use tag-based views in AWS Config and AWS Resource Groups for easier resource management
    

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUd0vgyt9ieFUZveIGWPs8WhwrZgnBPz4K9W_e37nOWN62t0vUfgaBHVbU2XOzjCcchjSUWt2NSsYf6TK1ripk0P38-yoXSsOUkKWqU15gAN0GTVgvCL4Tqu7TI1LJT_T33MzzTpzgXH8c5aGbA-lG5tUbJ-N7zQiYGkM6nVWiGhmRVb7C-E1Lc=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

3. ### Automating Landing Zones with AWS Control Tower Account Factory for Terraform (AFT)
    

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUdvifGuMEVmBQNuMXR3wBEe2M9KKz5dYa_W6glwR_w38W3jR1Z1BiP7oAWWUCwmMLIuu02AG0osTm_4Q_luq3srFrtXDQrD1Jbncr_lBNkD7SIFig5xHR91NlfjwB2oikWbxFAzoytOyBUEmHALnPBLumH_mBHp2JXSpNRhSqtMSIj9TYkcxxQ=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

AWS Control Tower Account Factory for Terraform (AFT) is a solution that allows you to automate the creation and bootstrapping of AWS accounts using Terraform. It extends the capabilities of Control Tower's Account Factory, enabling you to define and manage your multi-account environment as code.

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUdJUFvm9S4dYGhnUNrlgb4gGfLfeuQ36EuLV6qU4loBRAXMb-h30wse1_zWxr-KkF1TJQXNuEL19QR0OluHKQvqdJ_LmbecDS56-EKcudwNNDSn31jbtqkE6DFdb7JTeDRub90vNJD15uPBBgWPy_3Xj7SlYj8UCvxYULGQwPv-G-vOlJSiukk=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

Key benefits of AFT:

* Automate account provisioning and customization
    
* Ensure consistency across accounts
    
* Version control your account configurations
    
* Integrate with your existing CI/CD pipelines
    

3.2. AFT Implementation Demo

Let's walk through setting up and using AFT:

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUfK6fnYqbBGsOQ6oFmuINDtzfJdcfoLUDa-PiMGozuEe7Z1p9Sqmsp1Udxkh9RvlkqV2ND32aqNX5VKWaebpdVKWzlm6FU7b4egow8G9RR4YQlUrmL6KBoBlx0OnwghqXOdBgAtqnhB_Bhtiikax5rUfHKDALu9rqdOsefC5eOlieA4p2j2tBQ=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

Step 1: Prerequisites

* Ensure you have AWS Control Tower set up
    
* Install Terraform (version 0.15.0 or later)
    
* Configure AWS CLI with appropriate permissions
    

Step 2: Deploy AFT  
a) Clone the AFT repository:

```bash
git clone https://github.com/aws-ia/terraform-aws-control_tower_account_factory
cd terraform-aws-control_tower_account_factory        
```

b) Configure AFT parameters in `terraform.tfvars`:

```bash
control_tower_parameters = {
  aws_region        = "us-east-1"
  aft_management_account_id = "123456789012"
  ct_management_account_id  = "098765432109"
  log_archive_account_id    = "234567890123"
  audit_account_id          = "345678901234"
}

terraform_distribution = "oss"
terraform_version      = "1.0.0"            
```

c) Initialize and apply Terraform:

```bash
terraform init
terraform apply
```

Step 3: Create an Account Request  
a) Create a new file [`account-request.tf`](http://account-request.tf):

```bash
module "account_request" {
  source = "./modules/aft-account-request"

  control_tower_parameters = {
    AccountEmail = "newaccount@example.com"
    AccountName  = "New Dev Account"
    ManagedOrganizationalUnit = "Sandbox"
    SSOUserEmail     = "admin@example.com"
    SSOUserFirstName = "Admin"
    SSOUserLastName  = "User"
  }

  account_tags = {
    Environment = "Development"
    Owner       = "DevTeam"
  }

  custom_fields = {
    ProjectID   = "DEV-123"
    CostCenter  = "CC-456"
  }
}
```

b) Apply the account request:

```bash
terraform apply
```

Step 4: Customize the New Account  
a) Create account customizations in `aft-account-customizations`:

```bash
resource "aws_s3_bucket" "example" {
  bucket = "my-new-dev-bucket"
  acl    = "private"

  tags = {
    Environment = "Development"
    Project     = var.custom_fields["ProjectID"]
  }
}
```

b) Commit and push your changes to trigger the AFT pipeline

Step 5: Monitor Account Creation

* Check the AFT pipeline in AWS CodePipeline
    
* Verify the new account in AWS Control Tower and AWS Organizations
    

This demo provides a basic implementation of AFT. In a real-world scenario, you would create more complex account structures and customizations based on your organization's needs.

4. ### Conclusion
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723811287790/98a07d17-89e3-440d-972f-270e47cde2f4.png align="center")

In this final part of our series, we've explored advanced security and compliance features in AWS Control Tower, including different control types, encryption at scale, and the integration with AWS Security Hub. We've also discussed strategies for cost control using Service Control Policies and effective tagging.

The Account Factory for Terraform demo showcased how you can automate and scale your landing zone implementation, bringing infrastructure-as-code practices to your multi-account management strategy.

By leveraging these advanced features and automation capabilities, you can create a secure, compliant, and efficiently managed multi-account AWS environment that can grow and adapt with your organization's needs.

Remember that cloud management is an ongoing process. Regularly review your configurations, stay updated with new AWS features, and continuously refine your approach to meet your organization's evolving requirements.
---
title: "Mastering Multi-Account Management with AWS Control Tower and Landing Zones - Part 2: Implementation and Security Best Practices"
datePublished: Sat Aug 17 2024 18:30:32 GMT+0000 (Coordinated Universal Time)
cuid: clzyh4b92000w09meh9xtcxwm
slug: mastering-multi-account-management-with-aws-control-tower-and-landing-zones-part-2-implementation-and-security-best-practices
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723808621663/ce4557e1-f73d-499c-a46d-d361ed8c0d1a.png
tags: aws

---

Table of Contents for Part 2:

1. Introduction
    
2. Understanding AWS Control Tower in Depth  
    2.1. AWS Services Managed by Control Tower  
    2.2. AWS Accounts as Building Blocks  
    2.3. AWS Control Tower Architecture  
    2.4. Organizational Units in Control Tower  
    2.5. Multiple AWS Account Setup
    
3. Implementing AWS Control Tower: A Comprehensive Demo
    
4. Security Best Practices with AWS Control Tower  
    4.1. IAM Identity Center  
    4.2. AWS Security Hub  
    4.3. AWS CloudTrail  
    4.4. AWS Config Rules
    
5. Conclusion
    

Welcome to the second part of our comprehensive guide on mastering multi-account management with AWS Control Tower and Landing Zones. In the first part, we explored the foundations of cloud architecture and introduced the concepts of Landing Zones and AWS Control Tower. Now, we'll dive into the practical aspects of implementing AWS Control Tower and examine the security best practices that come with it.

This section will walk you through the Understanding AWS Control Tower in Depth followed by a detailed demo of setting up AWS Control Tower and explore the key security features it offers. By the end of this part, you'll have a solid understanding of how to implement AWS Control Tower and leverage its security capabilities to maintain a robust and compliant multi-account environment.

2. ### Understanding AWS Control Tower in Depth
    

Before we dive into the implementation, let's explore AWS Control Tower in more detail, understanding its components, architecture, and how it manages multiple AWS services and accounts.

**2.1. AWS Services Managed by Control Tower**

AWS Control Tower provides a centralized way to manage and govern multiple AWS services. Here are the key services that Control Tower integrates with:

**a) Cloud Governance:**

* AWS Organizations: For account management and organizational structure
    
* AWS CloudTrail: For comprehensive logging of API calls
    
* Amazon CloudWatch: For monitoring and observability
    
* Amazon SNS: For notifications and alerts
    
* AWS Lambda: For serverless compute and automation
    

**b) Control Management:**

* AWS Config: For resource inventory and configuration tracking
    
* AWS Security Hub: For security posture management
    
* AWS CloudFormation: For infrastructure as code and resource provisioning
    

**c) Identity and Access Management:**

* AWS IAM: For fine-grained access control
    
* AWS IAM Identity Center: For centralized identity management
    

**d) Data Protection:**

* AWS Key Management Service (KMS): For encryption key management
    
* Amazon S3: For secure object storage
    

**e) Infrastructure Protection:**

* AWS Service Catalog: For approved service and resource deployment
    
* AWS Step Functions: For workflow orchestration
    

By integrating these services, Control Tower provides a comprehensive solution for managing and governing your multi-account AWS environment.  
  

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUfNPR3W5_8Zjh2IHpy_yd08PLi1sLKY2FDozK-a5aD4VQOSLYLWlNHpPQloyRbrzY8nvYEdpFyGOVYvVvozD_nUd75Hx-QHe0NR-yx-4NAPqRzgCimTkVfnLHVztO8E3HMFkqx0PWnQsG8vriaXDWjXsBmtqtASoL7fAuhUyJK9Xb7Mu1jGws0=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

**2.2. AWS Accounts as Building Blocks**

In AWS Control Tower, individual AWS accounts serve as the fundamental building blocks of your organization's cloud infrastructure. This approach offers several benefits:

* Isolation: Each account provides a natural boundary for resources, improving security and reducing blast radius.
    
* Cost Management: Separate accounts allow for easier tracking and allocation of costs.
    
* Compliance: Different accounts can be configured to meet specific compliance requirements.
    
* Scalability: New accounts can be easily provisioned as your organization grows.
    

**2.3. AWS Control Tower Architecture**

The AWS Control Tower architecture consists of several key components:

a) Management Account: This is the root account of your AWS Organization, where Control Tower is set up.

b) Log Archive Account: Centralized storage for logs from all accounts in the organization.

c) Audit Account: Used for security and compliance tooling.

d) Member Accounts: Individual accounts for workloads, departments, or projects.

e) AWS Organizations: Provides the overall structure and hierarchy for your accounts.

f) CloudFormation StackSets: Used to deploy resources and configurations across multiple accounts.

g) Service Catalog: Offers a portfolio of approved AWS resources that can be deployed in member accounts.  
  

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUdUGRWhSKLo_SepHg-kFlEKUlIq9j2H0mmayo7l-Oo4zehOdPchLQml_a4hhOdSfer3PIZWsiw7B-H-azD-_xOJC0iUVM9Yfy8Ooiu4dRcpoDQxCxAGTrKw8DaApoeZKKfTeIeYWrhH3OrvmCizrLxbvPchr8uY_Y2VkGhk5-CUihgNx5cRxNY=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

**2.4. Organizational Units in Control Tower**

Control Tower uses Organizational Units (OUs) to group and manage accounts:

a) Security OU: Contains accounts related to security and compliance functions.

b) Sandbox OU: For development and testing environments with fewer restrictions.

c) Foundational OUs: Created by default, including Core OU and Custom OU.

d) Additional OUs: Can be created to meet specific organizational needs.

**2.5. Multiple AWS Account Setup**

In a typical multi-account setup with AWS Control Tower, you might have:

* Management Account: For overall administration and Control Tower management.
    
* Log Archive Account: Centralized logging repository.
    
* Audit Account: For security and compliance tooling.
    
* Shared Services Account: For resources used across multiple accounts (e.g., Active Directory, DNS).
    
* Production OU:
    
    * Prod Account 1 (e.g., E-commerce application)
        
    * Prod Account 2 (e.g., Customer support system)
        
* Development OU:
    
    * Dev Account 1
        
    * Dev Account 2
        
* Test OU:
    
    * Test Account 1
        
    * Test Account 2
        

This structure allows for clear separation of concerns, improved security, and easier management of different environments and workloads.

3. ### Implementing AWS Control Tower: A Comprehensive Demo
    

Let's walk through the process of setting up AWS Control Tower step by step:

Step 1: Prepare for AWS Control Tower Before you begin, ensure you have:

* An AWS account that will serve as the management account
    
* AWS Organizations enabled in your account
    
* Sufficient permissions to create and manage resources
    

Step 2: Access AWS Control Tower

* Log in to your AWS Management Console
    
* Navigate to the AWS Control Tower service
    

Step 3: Launch AWS Control Tower

* Click on "Set up landing zone"
    
* Review the prerequisites and click "Next"
    

Step 4: Choose your organizational structure

* Select your home Region (where Control Tower resources will be deployed)
    
* Choose to create a new organization or use an existing one
    
* Define your organizational units (OUs) structure
    

Step 5: Configure core accounts

* Management account: This is your current account
    
* Log archive account: Choose to create a new account or use an existing one
    
* Audit account: Choose to create a new account or use an existing one
    
* Provide email addresses for the new accounts (if creating)
    

Step 6: Configure AWS Control Tower features

* Enable or disable AWS CloudTrail
    
* Configure AWS Config settings
    
* Choose whether to enable AWS Security Hub
    

Step 7: Review and create your landing zone

* Review all your configuration choices
    
* Click "Set up landing zone" to begin the deployment
    

Step 8: Monitor the setup process

* The setup process typically takes 60-90 minutes
    
* You can monitor progress in the AWS Control Tower dashboard
    

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUfIpoc34E5tSH-v_6n8_153e1k20_evrxiF_upQuM4X_m3rV4vpx8hJDMgrF-N6QrlsdoTR1tL-GWI-k79owIN7RUtzn-P06QoBDRbcxXGfy4TDHSw92qCPjNV1fMjL_7b-0dCHedj6bKnAyx82orrZxl4pqaHDHCMzC9YPxpGK3YtrtzK1MA=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

Step 9: Explore your new environment Once setup is complete:

* Review the AWS Control Tower dashboard
    
* Explore the Account Factory
    
* Check the implemented guardrails
    
* Verify the created OUs in AWS Organizations
    

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUdgwmVCA3TROE41mbrwvlxO6GerWkI0UsuoWt0yFTBoc6kLxoRy1uy84-Z2S9JQcwkZjY1osINh1EpioB3XNrIkuUrz3hp7tRVsPc2KkUBc7TWvqVyYfkrgppuiZCvE8WmL_JfBgx7xoiy_TLFDQ0rzKt6fc_3T6-Lzgrh-BNjVHvpnqv2ONVs=s2048?key=O4ArTULVlToGY1a7FZOO4Q align="left")

Step 10: Enroll existing accounts (optional) If you have existing AWS accounts:

* Go to "Account Factory" in the Control Tower dashboard
    
* Click "Enroll account"
    
* Follow the prompts to bring existing accounts under Control Tower management
    

Step 11: Create new accounts To create new accounts within your landing zone:

* Go to "Account Factory" in the Control Tower dashboard
    
* Click "Create account"
    
* Fill in the account details and choose the appropriate OU
    
* Control Tower will provision the account based on your landing zone blueprint
    

Step 12: Implement additional guardrails

* Review the list of available guardrails in the Control Tower dashboard
    
* Enable additional guardrails as needed for your organization's requirements
    

This demo provides a high-level overview of the setup process. In a real-world scenario, you would need to carefully consider your organization's specific requirements and tailor the configuration accordingly.

4. ### Security Best Practices with AWS Control Tower
    

AWS Control Tower incorporates several security best practices out of the box. Let's explore some key security features and how to leverage them effectively:

**4.1. IAM Identity Center**

IAM Identity Center (formerly AWS Single Sign-On) is a crucial component of AWS Control Tower, providing centralized access management across your AWS accounts.

Key features:

* Single sign-on to AWS accounts and applications
    
* Integration with existing identity providers (e.g., Azure AD, Okta)
    
* Fine-grained permissions management
    

Best practices: a) Use IAM Identity Center as your primary means of access management

* Configure IAM Identity Center as the central authentication point
    
* Create permission sets that align with job functions
    
* Assign users to groups and map groups to permission sets
    

b) Implement the principle of least privilege

* Grant only the permissions necessary for users to perform their jobs
    
* Regularly review and adjust permissions as roles change
    

c) Enable multi-factor authentication (MFA)

* Enforce MFA for all users, especially those with elevated privileges
    
* Consider using hardware tokens for highly privileged accounts
    

d) Regularly review and audit user access

* Conduct quarterly access reviews
    
* Implement automated alerts for suspicious access patterns
    
* Use AWS Access Analyzer to identify resources shared with external entities
    

**4.2. AWS Security Hub**

AWS Security Hub provides a comprehensive view of your security and compliance status across your AWS accounts.

Key features:

* Aggregated security findings
    
* Automated security checks
    
* Integration with third-party security tools
    

Best practices: a) Enable Security Hub in all accounts

* Use Control Tower to automatically enable Security Hub in new accounts
    
* Retroactively enable Security Hub in any existing accounts
    

b) Customize security standards

* Enable relevant standards (e.g., CIS AWS Foundations Benchmark, PCI DSS)
    
* Create custom insights for organization-specific security requirements
    

c) Regularly review and act on Security Hub findings

* Set up automated notifications for high-severity findings
    
* Implement a process for triaging and addressing security issues
    
* Use Security Hub's integration with AWS Systems Manager to automate remediation where possible
    

d) Integrate third-party security tools

* Connect compatible third-party tools to extend Security Hub's capabilities
    
* Ensure a unified view of security across all your tools and services
    

**4.3. AWS CloudTrail**

CloudTrail records API calls and account activity, providing a crucial audit trail for your AWS environment.

Key features:

* Detailed event history of account activity
    
* Integration with AWS services for automated responses
    
* Long-term storage of log files
    

Best practices: a) Enable CloudTrail in all accounts

* Use Control Tower to automatically enable CloudTrail in new accounts
    
* Ensure both management and data events are logged
    

b) Configure log file integrity validation

* Enable log file integrity validation to detect any tampering with log files
    
* Regularly verify the integrity of your log files
    

c) Set up alerts for specific CloudTrail events

* Create CloudWatch alarms for critical events (e.g., root account usage, security group changes)
    
* Implement automated responses to suspicious activities
    

d) Ensure proper log retention and analysis

* Configure appropriate retention periods for CloudTrail logs
    
* Use AWS Athena or third-party SIEM tools for log analysis
    
* Implement a process for regular log review and incident response
    

**4.4. AWS Config Rules**

AWS Config provides a detailed inventory of your AWS resources and their configurations, allowing you to assess, audit, and evaluate their compliance with your policies.

Key features:

* Continuous monitoring and assessment of resource configurations
    
* Automated remediation of non-compliant resources
    
* Custom rules for organization-specific requirements
    

Best practices: a) Enable AWS Config in all accounts

* Use Control Tower to automatically enable Config in new accounts
    
* Ensure all relevant resource types are recorded
    

b) Implement both AWS-managed and custom Config rules

* Enable relevant AWS-managed rules (e.g., encrypted volumes, restricted common ports)
    
* Create custom rules for organization-specific compliance requirements
    

c) Set up automated remediation actions

* Configure auto-remediation for common issues (e.g., public S3 buckets)
    
* Use AWS Systems Manager Automation documents for complex remediation tasks
    

d) Regularly review and update Config rules

* Conduct periodic reviews of your Config rules
    
* Update rules as your compliance requirements evolve
    

e) Leverage Config data for compliance reporting

* Use AWS Config aggregators to centralize configuration data
    
* Generate compliance reports for audits and internal reviews
    

5. ### Conclusion
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723808571001/1a616a3f-7763-4c7d-b094-f04290ec739a.png align="center")

Implementing AWS Control Tower and following these security best practices provides a solid foundation for managing a secure and compliant multi-account AWS environment. By leveraging IAM Identity Center, Security Hub, CloudTrail, and Config Rules, you can maintain a strong security posture across your entire AWS organization.

Remember that security is an ongoing process. Regularly review your configurations, stay updated with new AWS features, and continuously refine your approach to meet your organization's evolving needs.

In the next and final part of this series, we'll explore advanced topics such as automating landing zones with AWS Control Tower Account Factory for Terraform (AFT) and dive deeper into ensuring ongoing security and compliance in your multi-account environment.
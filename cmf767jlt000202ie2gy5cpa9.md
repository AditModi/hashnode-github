---
title: "Policy as Code: Enforcing Compliance with Open Policy Agent (OPA) & AWS Config"
seoTitle: "Policy as Code: Enforcing Compliance with Open Policy Agent (OPA) & AW"
datePublished: Fri Sep 05 2025 18:30:38 GMT+0000 (Coordinated Universal Time)
cuid: cmf767jlt000202ie2gy5cpa9
slug: policy-as-code-enforcing-compliance-with-open-policy-agent-opa-and-aws-config
tags: aws, security, iac

---

As the complexity of cloud infrastructure increases, ensuring that systems are compliant with security standards, governance policies, and operational best practices becomes crucial. In the traditional world of IT infrastructure, security and compliance checks were often performed manually or in a disconnected manner, leading to inefficiencies, inconsistencies, and vulnerabilities. With **Infrastructure as Code (IaC)** taking center stage, it's possible to automate and enforce these policies as part of the deployment pipeline itself — **this is where Policy as Code (PaC)** comes into play.

This blog post explores how **Policy as Code** enables you to enforce compliance across your infrastructure, ensuring your cloud resources remain secure and aligned with your organization's policies. We’ll cover how to use tools like **Open Policy Agent (OPA)** and **AWS Config** to enforce policies in your **Terraform** configurations and **cloud infrastructure**.

---

### **1\. What is Policy as Code (PaC)?**

**Policy as Code** refers to the practice of defining and enforcing governance policies for your cloud infrastructure using code. The key idea behind PaC is that policies can be automated and validated as part of the CI/CD pipeline, just like your infrastructure configuration. This means that security, compliance, and operational rules are applied continuously throughout the software development lifecycle, rather than as a reactive process.

By integrating **Policy as Code** with your IaC toolchain, you can:

* Automatically enforce security and governance policies.
    
* Ensure that infrastructure configurations adhere to organizational standards.
    
* Minimize human error in policy enforcement.
    
* Create repeatable, auditable processes for compliance checks.
    

---

### **2\. Open Policy Agent (OPA): A Deep Dive into Policy as Code**

**Open Policy Agent (OPA)** is a powerful, open-source policy engine that allows you to define, enforce, and query policies across various systems, including **Terraform**, **Kubernetes**, and cloud-native applications. OPA uses a high-level declarative language called **Rego**, which enables you to write policies for almost any aspect of your infrastructure.

#### **Why Use OPA?**

* **Decouples Policy from Application Code**: OPA separates policy decisions from your applications, providing a centralized place for policy enforcement.
    
* **Flexibility**: OPA supports a wide range of use cases, from validating Terraform configurations to controlling access in Kubernetes.
    
* **Auditability**: With OPA, policies are written in code, making it easier to track, version, and review compliance over time.
    

#### **Enforcing Terraform Policies with OPA**

OPA integrates seamlessly with Terraform to allow policy enforcement directly within the IaC process. By using **OPA with Terraform**, you can validate that resources being deployed meet security and compliance standards, preventing violations early in the deployment pipeline.

For instance, let’s say you have a policy that mandates all AWS S3 buckets to be **private**. You can write a policy in Rego to validate this:

**Example Rego policy:**

```rego
package terraform.aws.s3

deny[msg] {
    input.resource_type == "aws_s3_bucket"
    input.values.bucket == "public-bucket"
    msg = "S3 bucket should not be public"
}    
```

This policy checks that no `aws_s3_bucket` resource has a bucket name equal to `public-bucket`, ensuring that no publicly accessible buckets are created.

You can integrate this policy into your **CI/CD pipeline** to check for violations before deployment:

```bash
opa eval --input terraform-plan.json --data s3_policy.rego "data.terraform.aws.s3.deny"    
```

This evaluates whether the Terraform plan violates the policy. If it does, the pipeline fails, preventing the risky resource from being deployed.

#### **OPA Best Practices**

* **Modular Policies**: Organize policies into reusable modules. This reduces duplication and improves maintainability.
    
* **Version Control**: Just like with Terraform code, store OPA policies in version control to track changes over time.
    
* **Error Handling**: Ensure that OPA policies provide meaningful error messages, so developers understand why a policy violation occurred.
    
* **Testing**: Use tools like **Conftest** (which leverages OPA) to test policies locally before deploying them in production.
    

---

### **3\. AWS Config: Enforcing AWS-Specific Policies**

While **OPA** provides a powerful solution for generic policy enforcement, **AWS Config** is a native AWS service that helps you monitor and enforce compliance across AWS resources. AWS Config allows you to define **AWS Config Rules** that evaluate whether your AWS resources comply with specific policies.

#### **Key Features of AWS Config**

* **Resource Tracking**: AWS Config tracks the configuration history of your AWS resources.
    
* **Compliance Validation**: You can define rules to check whether your resources comply with internal policies (e.g., tagging, encryption, IAM permissions).
    
* **Remediation**: AWS Config allows you to set up automated remediation actions to fix non-compliant resources.
    

#### **Common AWS Config Rules**

* **S3 Bucket Encryption**: Ensure that all S3 buckets have encryption enabled.
    
* **EC2 Instance Type**: Validate that EC2 instances are of an allowed type (e.g., t2.micro).
    
* **IAM Role Permissions**: Ensure IAM roles do not have overly permissive policies.
    

Here’s an example of a **custom AWS Config rule** written in **AWS Lambda** to ensure EC2 instances have a **specific tag**:

```python
import json

def lambda_handler(event, context):
    config_rule_name = 'ec2-instance-tag-rule'
    non_compliant_resources = []

    for record in event['invokingEvent']['configurationItem']:
        resource_type = record['resourceType']
        resource_id = record['resourceId']
        
        if resource_type == 'AWS::EC2::Instance':
            tags = record['configuration']['tags']
            if not any(tag['Key'] == 'Environment' for tag in tags):
                non_compliant_resources.append(resource_id)
    
    if non_compliant_resources:
        return {
            'compliance_type': 'NON_COMPLIANT',
            'annotation': 'The EC2 instances must have an Environment tag.'
        }
    else:
        return {
            'compliance_type': 'COMPLIANT',
            'annotation': 'All EC2 instances have the required tag.'
        }    
```

#### **Best Practices for AWS Config**

* **Custom Rules for Compliance**: Use **AWS Config Lambda Rules** for more granular, custom compliance checks (like ensuring EC2 instances have proper tags, or that security groups don’t allow inbound SSH access).
    
* **Integrate with Notifications**: Integrate **SNS** with AWS Config to alert teams when resources fall out of compliance.
    
* **Automated Remediation**: Use AWS **Systems Manager Automation** to automatically remediate non-compliant resources, e.g., automatically tag untagged resources.
    

---

### **4\. Policy as Code with IaC Pipelines**

Enforcing policies with **OPA** and **AWS Config** is only effective if integrated into your **CI/CD pipeline**. With tools like **GitHub Actions**, **CircleCI**, and **Jenkins**, you can automate the process of policy validation before your code is deployed.

#### **CI/CD Pipeline for Terraform with Policy Checks**

Here’s an example of integrating **OPA** with a **GitHub Actions** pipeline for Terraform:

**.github/workflows/terraform-ci.yml:**

```yaml
name: Terraform CI/CD Pipeline with Policy Checks

on:
  pull_request:
    branches:
      - main

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Set Up Terraform
        uses: hashicorp/setup-terraform@v1
        with:
          terraform_version: 1.0.0

      - name: Run Terraform Init
        run: terraform init

      - name: Run Terraform Plan
        run: terraform plan -out=plan.tfplan

      - name: Run OPA Policy Check
        run: |
          opa eval --input plan.tfplan --data policies/terraform-policy.rego "data.terraform.aws.s3.deny"    
```

This pipeline:

1. **Checks out** the code from the repository.
    
2. **Initializes Terraform** and generates a **plan**.
    
3. Runs an **OPA policy check** to validate if any policy violations exist (e.g., no public S3 buckets).
    

By including these checks as part of your pipeline, you ensure that only compliant resources are deployed.

---

### **5\. Conclusion: Embedding Security and Governance in the Development Lifecycle**

With **Policy as Code** and tools like **OPA** and **AWS Config**, enforcing compliance across your cloud infrastructure becomes automated, repeatable, and auditable. By embedding security and compliance directly into your IaC workflow, you ensure that your infrastructure remains compliant throughout its lifecycle, reducing the risk of misconfigurations and security vulnerabilities.

In this post, we’ve shown how **OPA** enables generic policy enforcement, and **AWS Config** helps with AWS-specific compliance. By integrating these tools into your CI/CD pipeline, you can ensure that policy violations are detected early, before they impact production environments.

In the next post, we’ll explore **IaC Security** and best practices for securing your cloud infrastructure. Stay tuned!
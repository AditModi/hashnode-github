## How I passed the AWS Certified SysOps Administrator Associate Exam (SOA-C01)

## Introduction

- I recently passed the AWS Certified SysOps Administrator Associate Exam . I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified SysOps Administrator - Associate is intended for system administrators in cloud operations roles to validate technical skills.

- Earning AWS Certified SysOps Administrator - Associate
demonstrates experience deploying, managing, and operating workloads on AWS. This certification helps organizations identify and develop talent with critical skills for implementing cloud initiatives.

- This article gives you an overview which content and training material I used to prepare myself for the SOA-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- A minimum of one year of hands-on experience with AWS technology
- Experience deploying, managing, and operating workloads on AWS as well as implementing security controls and compliance requirements
- Familiarity with using both the AWS Management Console and the AWS Command Line Interface (CLI)
- Understanding of the AWS Well-Architected Framework as well as AWS networking and security services

## Exam overview

**Level:** Associate

**Length:** 180 minutes to complete the exam

**Cost:** 150 USD

Visit[Exam pricing](https://aws.amazon.com/certification/policies/before-testing/#Exam_pricing) for additional cost information.

**Format:** 65 questions, either multiple choice or multiple response

**Delivery method:** Pearson VUE and PSI; testing center or online proctored exam

## Exam Outline

- This exam guide includes weightings, test domains, and objectives for the exam. It is not a comprehensive listing of the content on the exam. However, additional context for each of the objectives is available to help guide your preparation for the exam. 

- The following table lists the main content domains and their
weightings. The table precedes the complete exam content outline, which includes the additional context. The percentage in each domain represents only scored content.


| Domain  | % of Exam |
| ------- | --------- |
|Domain 1: Monitoring, Logging, and Remediation |20%|
|Domain 2: Reliability and Business Continuity |16%|
|Domain 3: Deployment, Provisioning, and Automation |18%|
|Domain 4: Security and Compliance |16%|
|Domain 5: Networking and Content Delivery |18%|
|Domain 6: Cost and Performance Optimization |12%|
|TOTAL |100%|

**Domain 1: Monitoring, Logging, and Remediation**
1.1 Implement metrics, alarms, and filters by using AWS monitoring and logging services
- Identify, collect, analyze, and export logs (for example, Amazon CloudWatch Logs, CloudWatch Logs Insights, AWS CloudTrail logs)
- Collect metrics and logs using the CloudWatch agent
- Create CloudWatch alarms
- Create metric filters
- Create CloudWatch dashboards
- Configure notifications (for example, Amazon Simple Notification Service [Amazon SNS], Service Quotas, CloudWatch alarms, AWS Health events)
1.2 Remediate issues based on monitoring and availability metrics
- Troubleshoot or take corrective actions based on notifications and alarms
- Configure Amazon EventBridge rules to trigger actions
- Use AWS Systems Manager Automation documents to take action based on AWS Config rules
**Domain 2: Reliability and Business Continuity**
2.1 Implement scalability and elasticity
- Create and maintain AWS Auto Scaling plans
- Implement caching
- Implement Amazon RDS replicas and Amazon Aurora Replicas
- Implement loosely coupled architectures
- Differentiate between horizontal scaling and vertical scaling
2.2 Implement high availability and resilient environments
- Configure Elastic Load Balancer and Amazon Route 53 health checks
- Differentiate between the use of a single Availability Zone and Multi-AZ deployments (for example, Amazon EC2 Auto Scaling groups, Elastic Load Balancing, Amazon FSx, Amazon RDS)
- Implement fault-tolerant workloads (for example, Amazon Elastic File System [Amazon EFS], Elastic IP addresses)
- Implement Route 53 routing policies (for example, failover, weighted, latency based)
2.3 Implement backup and restore strategies
- Automate snapshots and backups based on use cases (for example, RDS snapshots, AWS Backup, RTO and RPO, Amazon Data Lifecycle Manager, retention policy)
- Restore databases (for example, point-in-time restore, promote read replica)
- Implement versioning and lifecycle rules
- Configure Amazon S3 Cross-Region Replication
- Execute disaster recovery procedures
**Domain 3: Deployment, Provisioning, and Automation**
3.1 Provision and maintain cloud resources
- Create and manage AMIs (for example, EC2 Image Builder)
- Create, manage, and troubleshoot AWS CloudFormation
- Provision resources across multiple AWS Regions and accounts (for example, AWS Resource Access Manager, CloudFormation StackSets, IAM cross-account roles)
- Select deployment scenarios and services (for example, blue/green, rolling, canary)
- Identify and remediate deployment issues (for example, service quotas, subnet sizing, CloudFormation and AWS OpsWorks errors, permissions)
3.2 Automate manual or repeatable processes
- Use AWS services (for example, OpsWorks, Systems Manager, CloudFormation) to automate deployment processes
- Implement automated patch management
- Schedule automated tasks by using AWS services (for example, EventBridge, AWS Config)
**Domain 4: Security and Compliance**
4.1 Implement and manage security and compliance policies
- Implement IAM features (for example, password policies, MFA, roles, SAML, federated identity, resource policies, policy conditions)
- Troubleshoot and audit access issues by using AWS services (for example, CloudTrail, IAM Access Analyzer, IAM policy simulator)
- Validate service control policies and permissions boundaries
- Review AWS Trusted Advisor security checks
- Validate AWS Region and service selections based on compliance requirements
- Implement secure multi-account strategies (for example, AWS Control Tower, AWS Organizations)
4.2 Implement data and infrastructure protection strategies
- Enforce a data classification scheme
- Create, manage, and protect encryption keys
- Implement encryption at rest (for example, AWS Key Management Service [AWS KMS])
- Implement encryption in transit (for example, AWS Certificate Manager, VPN)
- Securely store secrets by using AWS services (for example, AWS Secrets Manager, Systems Manager Parameter Store)
- Review reports or findings (for example, AWS Security Hub, Amazon GuardDuty, AWS Config, Amazon Inspector)
**Domain 5: Networking and Content Delivery**
5.1 Implement networking features and connectivity
- Configure a VPC (for example, subnets, route tables, network ACLs, security groups, NAT gateway, internet gateway)
- Configure private connectivity (for example, Systems Manager Session Manager, VPC endpoints, VPC peering, VPN)
- Configure AWS network protection services (for example, AWS WAF, AWS Shield)
5.2 Configure domains, DNS services, and content delivery
- Configure Route 53 hosted zones and records
- Implement Route 53 routing policies (for example, geolocation, geoproximity)
- Configure DNS (for example, Route 53 Resolver)
- Configure Amazon CloudFront and S3 origin access identity (OAI)
- Configure S3 static website hosting
5.3 Troubleshoot network connectivity issues
- Interpret VPC configurations (for example, subnets, route tables, network ACLs, security groups)
- Collect and interpret logs (for example, VPC Flow Logs, Elastic Load Balancer access logs, AWS WAF web ACL logs, CloudFront logs)
- Identify and remediate CloudFront caching issues
- Troubleshoot hybrid and private connectivity issues
**Domain 6: Cost and Performance Optimization**
6.1 Implement cost optimization strategies
- Implement cost allocation tags
- Identify and remediate underutilized or unused resources by using AWS services and tools (for example, Trusted Advisor, AWS Compute Optimizer, Cost Explorer)
- Configure AWS Budgets and billing alarms
- Assess resource usage patterns to qualify workloads for EC2 Spot Instances
- Identify opportunities to use managed services (for example, Amazon RDS, AWS Fargate, EFS)
6.2 Implement performance optimization strategies
- Recommend compute resources based on performance metrics
- Monitor Amazon EBS metrics and modify configuration to increase performance efficiency
- Implement S3 performance features (for example, S3 Transfer Acceleration, multipart uploads)
- Monitor RDS metrics and modify the configuration to increase performance efficiency (for example, Performance Insights, RDS Proxy)
- Enable enhanced EC2 capabilities (for example, enhanced network adapter, instance store, placement groups)


👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-sysops-associate/AWS-Certified-SysOps-Administrator-Associate_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “Ultimate AWS Certified SysOps Administrator Associate” by Stephane Maarek which is a very good course and covers all the most important aspects of the AWS and its fundamentals so this is a very good start. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/ultimate-aws-certified-sysops-administrator-associate/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi).

3)**📋 AWS Ramp-Up Guides:** Your guides to learning the AWS Cloud.

- AWS Ramp-Up Guides offer a variety of resources to help you build your skills and knowledge of the AWS Cloud. Each guide features carefully selected digital training, classroom courses, videos, whitepapers, certifications, and more. Explore the guides below by role, solution, or industry area.

👉 more details can be found [here](https://aws.amazon.com/training/ramp-up-guides/)

4) **🤝 being part of a Study Groups:** I also recommend you to be part of a study groups. it helps you stay focused, probably having study groups with people studying for the same exam is an added benefit.

Study Groups I was part of:

- Cloud and DevOps Babies:

Cloud and DevOps Babies are a global group of babies with curious minds to learn/decode Cloud, DevOps and Microservices tech stacks. 

👉 more details can be found [here](https://www.linkedin.com/company/devopsandcloudbabies/)

- Tech Study Slack: 
TechStudySlack is a Slack for people studying Tech

👉 more details can be found [here](https://techstudyslack.com/)

5)**✍️ practice tests:** Lastly I recommend all of you to pass these practice exams before attending the real exam. It provides simulated questions that are very similar to the actual exam.

One of the selling points of this practice exam is that each question contains detailed explanations that will help you gain a deeper understanding of AWS services. It not just explains what the correct answer is, but also explains why other answers are wrong. It is extremely helpful to make you recognize the difference between similar services.

- Tutorials Dojo Practice Exams:

👉 more details can be found [here](https://tutorialsdojo.com/courses/aws-certified-sysops-administrator-associate-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Andrew Brown’s AWS SysOps Administrator Associate Exam Course](https://www.freecodecamp.org/news/aws-sysops-adminstrator-associate-certification-exam-course/)

- [Stephane Mareek’s Ultimate AWS Certified SysOps Administrator Associate](https://www.udemy.com/course/ultimate-aws-certified-sysops-administrator-associate/)

- [Andrian Cantrill's AWS Certified SysOps Administrator - Associate](https://learn.cantrill.io/p/aws-certified-sysops-administrator-associate)

- [Ervin Szilágyi’s AWS-SOA-C01 Study Guide Github Repo](https://github.com/Ernyoke/certified-aws-sysops-associate)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
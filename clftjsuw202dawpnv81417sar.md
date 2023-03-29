---
title: "How I passed the AWS Certified Security  — Specialty Exam (SCS-C01)"
seoTitle: "How I passed the AWS Certified Security  — Specialty Exam (SCS-C01)"
seoDescription: "I recently passed the AWS Certified Security  — Specialty Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Bef"
datePublished: Wed Mar 29 2023 10:32:39 GMT+0000 (Coordinated Universal Time)
cuid: clftjsuw202dawpnv81417sar
slug: how-i-passed-the-aws-certified-security-specialty-exam-scs-c01
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1660050497940/Vk2ZiMHjN.png
tags: aws, security, certification

---

## Introduction

- I recently passed the AWS Certified Security  — Specialty Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Security – Specialty is intended for individuals who perform a security role and have at least two years of hands-on experience securing AWS workloads.

- Earning AWS Certified Security – Specialty validates expertise in securing data and workloads in the AWS Cloud. This exam helps organizations identify and develop talent with critical skills for implementing cloud initiatives.

- This article gives you an overview which content and training material I used to prepare myself for the AWS SCS-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- Five years of IT security experience in designing and implementing security solutions and at least two years of hands-on experience in securing AWS workloads
- Working knowledge of AWS security services and features of services to provide a secure production environment and an understanding of security operations and risks
- Knowledge of the AWS shared responsibility model and its application; security controls for workloads on AWS; logging and monitoring strategies; cloud security threat models; patch management and security automation; ways to enhance AWS security services with third-party tools and services; and disaster recovery controls, including BCP and backups, encryption, access control, and data retention
- Understanding of specialized data classifications and AWS data protection mechanisms, data-encryption methods and AWS mechanisms to implement them, and secure internet protocols and AWS mechanisms to implement them
- Ability to make tradeoff decisions with regard to cost, security, and deployment complexity to meet a set of application requirements

## Exam overview

**Level:** Specialty

**Length:** 170 minutes to complete the exam

**Cost:** 300 USD

Visit[Exam pricing](https://aws.amazon.com/certification/policies/before-testing/#Exam_pricing) for additional cost information.

**Format:** 65 questions, either multiple choice or multiple response

**Delivery method:** Pearson VUE and PSI; testing center or online proctored exam

## Exam Outline

- This exam guide includes weightings, test domains, and objectives for the exam. It is not a comprehensive listing of the content on the exam. However, additional context for each of the objectives is available to help guide your preparation for the exam. 

- The following table lists the main content domains and their
weightings. The table precedes the complete exam content outline, which includes the additional context. The percentage in each domain represents only scored content.


| Domain  | % of Exam |
| ------- | --------- |
|Domain 1: Incident Response |12%|
|Domain 2: Logging and Monitoring |20%|
|Domain 3: Infrastructure Security |26%|
|Domain 4: Identity and Access Management |20%|
|Domain 5: Data Protection |22%|
|TOTAL |100%|

**Domain 1: Incident Response**

1.1 Given an AWS abuse notice, evaluate the suspected compromised instance or exposed access keys.
 Given an AWS Abuse report about an EC2 instance, securely isolate the instance as part of a forensic investigation.
 Analyze logs relevant to a reported instance to verify a breach, and collect relevant data.
 Capture a memory dump from a suspected instance for later deep analysis or for legal compliance reasons.
1.2 Verify that the Incident Response plan includes relevant AWS services.
 Determine if changes to baseline security configuration have been made.
 Determine if list omits services, processes, or procedures which facilitate Incident Response.
 Recommend services, processes, procedures to remediate gaps.
1.3 Evaluate the configuration of automated alerting, and execute possible remediation of security related incidents and emerging issues.
 Automate evaluation of conformance with rules for new/changed/removed resources.
 Apply rule-based alerts for common infrastructure misconfigurations.
 Review previous security incidents and recommend improvements to existing systems.

**Domain 2: Logging and Monitoring**

2.1 Design and implement security monitoring and alerting.
 Analyze architecture and identify monitoring requirements and sources for monitoring statistics.
 Analyze architecture to determine which AWS services can be used to automate monitoring and alerting.
 Analyze the requirements for custom application monitoring, and determine how this could be achieved.
 Set up automated tools/scripts to perform regular audits.

2.2 Troubleshoot security monitoring and alerting.
 Given an occurrence of a known event without the expected alerting, analyze the service functionality and configuration and remediate.
 Given an occurrence of a known event without the expected alerting, analyze the permissions and remediate.
 Given a custom application which is not reporting its statistics, analyze the configuration and remediate.
 Review audit trails of system and user activity.
2.3 Design and implement a logging solution.
 Analyze architecture and identify logging requirements and sources for log ingestion.
 Analyze requirements and implement durable and secure log storage according to AWS best practices.
 Analyze architecture to determine which AWS services can be used to automate log ingestion and analysis.
2.4 Troubleshoot logging solutions.
 Given the absence of logs, determine the incorrect configuration and define remediation steps.
 Analyze logging access permissions to determine incorrect configuration and define remediation steps.
 Based on the security policy requirements, determine the correct log level, type, and sources.
Domain 3: Infrastructure Security
3.1 Design edge security on AWS.
 For a given workload, assess and limit the attack surface.
 Reduce blast radius (e.g. by distributing applications across accounts and regions).
 Choose appropriate AWS and/or third-party edge services such as WAF, CloudFront and Route 53 to protect against DDoS or filter application-level attacks.
 Given a set of edge protection requirements for an application, evaluate the mechanisms to prevent and detect intrusions for compliance and recommend required changes.
 Test WAF rules to ensure they block malicious traffic.
3.2 Design and implement a secure network infrastructure.
 Disable any unnecessary network ports and protocols.
 Given a set of edge protection requirements, evaluate the security groups and NACLs of an application for compliance and recommend required changes.
 Given security requirements, decide on network segmentation (e.g. security groups and NACLs)
that allow the minimum ingress/egress access required.
 Determine the use case for VPN or Direct Connect.
 Determine the use case for enabling VPC Flow Logs.
 Given a description of the network infrastructure for a VPC, analyze the use of subnets and gateways for secure operation.
3.3 Troubleshoot a secure network infrastructure.
 Determine where network traffic flow is being denied.
 Given a configuration, confirm security groups and NACLs have been implemented correctly.

3.4 Design and implement host-based security.
 Given security requirements, install and configure host-based protections including Inspector, SSM.
 Decide when to use host-based firewall like iptables.
 Recommend methods for host hardening and monitoring.
Domain 4: Identity and Access Management
4.1 Design and implement a scalable authorization and authentication system to access AWS resources.
 Given a description of a workload, analyze the access control configuration for AWS services and make recommendations that reduce risk.
 Given a description how an organization manages their AWS accounts, verify security of their root user.
 Given your organization’s compliance requirements, determine when to apply user policies and resource policies.
 Within an organization’s policy, determine when to federate a directory services to IAM.
 Design a scalable authorization model that includes users, groups, roles, and policies.
 Identify and restrict individual users of data and AWS resources.
 Review policies to establish that users/systems are restricted from performing functions beyond their responsibility, and also enforce proper separation of duties.
4.2 Troubleshoot an authorization and authentication system to access AWS resources.
 Investigate a user’s inability to access S3 bucket contents.
 Investigate a user’s inability to switch roles to a different account.
 Investigate an Amazon EC2 instance’s inability to access a given AWS resource.
Domain 5: Data Protection
5.1 Design and implement key management and use.
 Analyze a given scenario to determine an appropriate key management solution.
 Given a set of data protection requirements, evaluate key usage and recommend required changes.
 Determine and control the blast radius of a key compromise event and design a solution to contain the same.
5.2 Troubleshoot key management.
 Break down the difference between a KMS key grant and IAM policy.
 Deduce the precedence given different conflicting policies for a given key.
 Determine when and how to revoke permissions for a user or service in the event of a compromise.
5.3 Design and implement a data encryption solution for data at rest and data in transit.
 Given a set of data protection requirements, evaluate the security of the data at rest in a workload and recommend required changes.
 Verify policy on a key such that it can only be used by specific AWS services.
 Distinguish the compliance state of data through tag-based data classifications and automate remediation.
 Evaluate a number of transport encryption techniques and select the appropriate method (i.e. TLS, IPsec, client-side KMS encryption).

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-security-spec/AWS-Certified-Security-Specialty_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “AWS Certified Security Specialty” by Zeal Vora which is a very good course and covers all the most important aspects of the AWS and You will be able to Master the Security aspect of AWS. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-certified-security-specialty/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi). I have some AWS security related projects as well.

3)**📋 AWS Ramp-Up Guides:** Your guides to learning the AWS Cloud.

- AWS Ramp-Up Guides offer a variety of resources to help you build your skills and knowledge of the AWS Cloud. Each guide features carefully selected digital training, classroom courses, videos, whitepapers, certifications, and more. Explore the guides below by role, solution, or industry area. 

👉 more details can be found [here](https://aws.amazon.com/training/ramp-up-guides/)

👉 Focus more on [Security Solution](https://d1.awsstatic.com/training-and-certification/ramp-up_guides/Ramp-Up_Guide_Security.pdf)

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

👉 more details can be found [here](https://tutorialsdojo.com/courses/aws-certified-security-specialty-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Zeal Vora’s AWS Certified Security Specialty Course](https://www.udemy.com/course/aws-certified-security-specialty/)

- [Andrian Cantrill's AWS Certified Security - Specialty Course](https://learn.cantrill.io/p/aws-certified-security-specialty)

- [Brian Lam’s AWS-SCS-C01 Study Guide Github Repo](https://github.com/brianlam38/AWS-Certified-Security-Specialty)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
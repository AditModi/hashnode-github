---
title: "How I passed the AWS Certified DevOps Engineer - Professional Exam (DOP-C01)"
seoTitle: "How I passed the AWS Certified DevOps Engineer - Professional Exam (DO"
seoDescription: "I recently passed the AWS Certified DevOps Engineer - Professional Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the"
datePublished: Sat Apr 29 2023 04:07:23 GMT+0000 (Coordinated Universal Time)
cuid: clk97dhfq00010al48yxw0q18
slug: how-i-passed-the-aws-certified-devops-engineer-professional-exam-dop-c01
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1660064163628/ClvDN8TIo.png
tags: aws, devops, certification

---

## Introduction

- I recently passed the AWS Certified DevOps Engineer - Professional Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified DevOps Engineer - Professional is intended for individuals with two or more years of experience provisioning, operating, and managing AWS environments.

- Earning AWS Certified DevOps Engineer - Professional validates the ability to automate the testing and deployment of AWS infrastructure and applications. This certification helps organizations identify and develop talent with critical skills for implementing cloud initiatives. 

- This article gives you an overview which content and training material I used to prepare myself for the AWS DOP-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- Experience developing code in at least one high-level programming language; building highly automated infrastructures; and administering operating systems
- Understanding of modern development and operations processes and methodologies
- Ability to implement and manage continuous delivery systems and methodologies on AWS
- Ability to implement and automate security controls, governance processes, and compliance validation
- Ability to define and deploy monitoring, metrics, and logging systems on AWS

## Exam overview

**Level:** Professional

**Length:** 180 minutes to complete the exam

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
|Domain 1: SDLC Automation |22%|
|Domain 2: Configuration Management and Infrastructure as Code |19%|
|Domain 3: Monitoring and Logging |15%|
|Domain 4: Policies and Standards Automation |10%|
|Domain 5: Incident and Event Response |18%|
|Domain 6: High Availability, Fault Tolerance, and Disaster Recover |16%|
|TOTAL |100%|

**Domain 1: SDLC Automation**
1.1 Apply concepts required to automate a CI/CD pipeline
 Set up repositories
 Set up build services
 Integrate automated testing (e.g., unit tests, integrity tests)
 Set up deployment products/services
 Orchestrate multiple pipeline stages
1.2 Determine source control strategies and how to implement them
 Determine a workflow for integrating code changes from multiple contributors
 Assess security requirements and recommend code repository access design
 Reconcile running application versions to repository versions (tags)
 Differentiate different source control types
1.3 Apply concepts required to automate and integrate testing
 Run integration tests as part of code merge process
 Run load/stress testing and benchmark applications at scale
 Measure application health based on application exit codes (robust Health Check)
 Automate unit tests to check pass/fail, code coverage
o CodePipeline, CodeBuild, etc.
 Integrate tests with pipeline
1.4 Apply concepts required to build and manage artifacts securely
 Distinguish storage options based on artifacts security classification
 Translate application requirements into Operating System and package configuration (build specs)
 Determine the code/environment dependencies and required resources
o Example: CodeDeploy AppSpec, CodeBuild buildspec
 Run a code build process
1.5 Determine deployment/delivery strategies (e.g., A/B, Blue/green, Canary, Red/black) and how to implement them using AWS services
 Determine the correct delivery strategy based on business needs
 Critique existing deployment strategies and suggest improvements
 Recommend DNS/routing strategies (e.g., Route 53, ELB, ALB, load balancer) based on business continuity goals
 Verify deployment success/failure and automate rollbacks

**Domain 2: Configuration Management and Infrastructure as Code**
2.1 Determine deployment services based on deployment needs
 Demonstrate knowledge of process flows of deployment models
 Given a specific deployment model, classify and implement relevant AWS services to meet requirements
o Given the requirement to have DynamoDB choose CloudFormation instead of OpsWorks
o Determine what to do with rolling updates
2.2 Determine application and infrastructure deployment models based on business needs
 Balance different considerations (cost, availability, time to recovery) based on business requirements to choose the best deployment model
 Determine a deployment model given specific AWS services
 Analyze risks associated with deployment models and relevant remedies
2.3 Apply security concepts in the automation of resource provisioning
 Choose the best automation tool given requirements
 Demonstrate knowledge of security best practices for resource provisioning (e.g., encrypting data bags, generating credentials on the fly)
 Review IAM policies and assess if sufficient but least privilege is granted for all lifecycle stages of a deployment (e.g., create, update, promote)
 Review credential management solutions (e.g., EC2 parameter store, third party)
 Build the automation
o CloudFormation template, Chef Recipe, Cookbooks, Code pipeline, etc.
2.4 Determine how to implement lifecycle hooks on a deployment
 Determine appropriate integration techniques to meet project requirements
 Choose the appropriate hook solution (e.g., implement leader node selection after a node failure) in an Auto Scaling group
 Evaluate hook implementation for failure impacts (if a remote call fails, if a dependent service is temporarily unavailable (i.e., Amazon S3), and recommend resiliency improvements
 Evaluate deployment rollout procedures for failure impacts and evaluate rollback/recovery processes
2.5 Apply concepts required to manage systems using AWS configuration management tools and services
 Identify pros and cons of AWS configuration management tools
 Demonstrate knowledge of configuration management components
 Show the ability to run configuration management services end to end with no assistance while adhering to industry best practices

**Domain 3: Monitoring and Logging**
3.1 Determine how to set up the aggregation, storage, and analysis of logs and metrics
 Implement and configure distributed logs collection and processing (e.g., agents, syslog, flumed, CW agent)
 Aggregate logs (e.g., Amazon S3, CW Logs, intermediate systems (EMR), Kinesis FH – Transformation, ELK/BI)
 Implement custom CW metrics, Log subscription filters
 Manage Log storage lifecycle (e.g., CW to S3, S3 lifecycle, S3 events)
3.2 Apply concepts required to automate monitoring and event management of an environment
 Parse logs (e.g., Amazon S3 data events/event logs/ELB/ALB/CF access logs) and correlate
with other alarms/events (e.g., CW events to AWS Lambda) and take appropriate action
 Use CloudTrail/VPC flow logs for detective control (e.g., CT, CW log filters, Athena, NACL or WAF rules) and take dependent actions (AWS step) based on error handling logic (state machine)
 Configure and implement Patch/inventory/state management using ESM (SSM), Inspector, CodeDeploy, OpsWorks, and CW agents
o EC2 retirement/maintenance
 Handle scaling/failover events (e.g., ASG, DB HA, route table/DNS update, Application Config, Auto Recovery, PH dashboard, TA)
 Determine how to automate the creation of monitoring
3.3 Apply concepts required to audit, log, and monitor operating systems, infrastructures, and applications
 Monitor end to end service metrics (DDB/S3) using available AWS tools (X-ray with EB and Lambda)
 Verify environment/OS state through auditing (Inspector), Config rules, CloudTrail (process and action), and AWS APIs
 Enable, configure, and analyze custom metrics (e.g., Application metrics, memory, KCL/KPL) and take action
 Ensure container monitoring (e.g., task state, placement, logging, port mapping, LB)
 Distinguish between services that enable service level or OS level monitoring
o Example: AWS services that use OS agents (e.g., Inspector, SSM)
3.4 Determine how to implement tagging and other metadata strategies
 Segregate authority based on tagging (lifecycle stages – dev/prod) with Condition context keys
 Utilize Amazon S3 system/user-defined metadata for classification and automation
 Design and implement tag-based deployment groups with CodeDeploy
 Best practice for cost allocation/optimization with tagging

**Domain 4: Policies and Standards Automation**
4.1 Apply concepts required to enforce standards for logging, metrics, monitoring, testing, and security
 Detect, report, and respond to governance and security violations
 Apply logging standards across application, operating system, and infrastructure
 Apply context specific application health and performance monitoring
 Outline standards for delivery models for logs and metrics (e.g., JSON, XML, Data Normalization)

4.2 Determine how to optimize cost through automation
 Prioritize automation effort to reduce labor costs
 Implement right sizing of workload based on metrics
 Assess ways to improve time to market through automating process orchestration and repeatable tasks
 Diagnose outliers to determine use case fit
o Example: Configuration drift
 Measure and automate cost optimization through events
o Example: Trusted Advisor
4.3 Apply concepts required to implement governance strategies
 Generalize governance standards across CI/CD pipeline
 Outline and measure the real-time status of compliance with governance strategies
 Report on compliance with governance strategies
 Deploy governance policies related to self-service capabilities
o Example: Service Catalog, CFN Nag

**Domain 5: Incident and Event Response**
5.1 Troubleshoot issues and determine how to restore operations
 Given an issue, evaluate how to narrow down the unhealthy components as quickly as possible
 Given an increase in load, determine what steps to take to mitigate the impact
 Determine the causes and impacts of a failure
o Example: Deployment, operations
 Determine the best way to restore operations after a failure occurs
 Investigate and correlate logged events with application components
o Example: application source code
5.2 Determine how to automate event management and alerting
 Set up automated restores from backup in the event of a catastrophic failure
 Set up methods to deliver alerts and notifications that are appropriate for different types of events
 Assess the quality/actionability of alerts
 Configure metrics appropriate to an application’s SLAs
 Proactively update limits
5.3 Apply concepts required to implement automated healing
 Set up the correct scaling strategy to enable auto-healing when a failure occurs (e.g., with Auto Scaling policies)
 Use the correct rollback strategy to avoid impact from failed deployments
 Configure Route 53 to ensure cross-Region failover
 Detect and respond to maintenance or Spot termination events
5.4 Apply concepts required to set up event-driven automated actions
 Configure Lambda functions or CloudWatch actions to implement automated actions
 Set up CloudWatch event rules and/or Config rules and targets
 Use AWS Systems Manager or Step Functions to coordinate components (e.g., Lambda, use maintenance windows)
 Configure a build/roll-out process to automatically respond to critical software updates

**Domain 6: High Availability, Fault Tolerance, and Disaster Recovery**
6.1 Determine appropriate use of multi-AZ versus multi-Region architectures
 Determine deployment strategy based on HA/DR requirements
 Determine data replication strategy based on cost and durability requirements
 Determine infrastructure, platform, and services based on HA/DR requirements
 Design for HA/FT/DR based on service availability (i.e., global/regional/single AZ)
6.2 Determine how to implement high availability, scalability, and fault tolerance
 Design deployment strategy to support HA/FT/scalability
 Assess statefulness of application infrastructure components
 Use load balancing to distribute traffic across multiple AZ/ASGs/instance types (spot/M4 vs C4) /targets
 Use appropriate caching solutions to improve availability and performance
6.3 Determine the right services based on business needs (e.g., RTO/RPO, cost)
 Determine cost-effective storage solution for your application
o Example: tiered, archival, EBS type, hot/cold
 Choose a database platform and configuration to meet business requirements
 Choose a cost-effective compute platform based on business requirements
o Example: Spot
 Choose a deployment service/model based on business requirements
o Example: Code Deploy, Blue/Green deployment
 Determine when to use managed service vs. self-managed infrastructure (Docker on EC2 vs. ECS)
6.4 Determine how to design and automate disaster recovery strategies
 Automate failure detection
 Automate components/environment recovery
 Choose appropriate deployment strategy for environment recovery
 Design automation to support failover in hybrid environment
6.5 Evaluate a deployment for points of failure
 Determine appropriate deployment-specific health checks
 Implement failure detection during deployment
 Implement failure event handling/response
 Ensure that resources/components/processes exist to react to failures during deployment
 Look for exit codes on each event of the deployment
 Map errors to different points of deployment

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-devops-pro/AWS-Certified-DevOps-Engineer-Professional_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “AWS Certified DevOps Engineer Professional” by Stephane Maarek which is a very good course and covers all the most important aspects of the AWS and helps us understand the DevOps on AWS with Hands-on Examples. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-certified-devops-engineer-professional-hands-on/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi). I have a list of projects that I worked on for this particular exam, which I have shared [here](https://github.com/AWS-Devops-Projects).

3)**📋 AWS Ramp-Up Guides:** Your guides to learning the AWS Cloud.

- AWS Ramp-Up Guides offer a variety of resources to help you build your skills and knowledge of the AWS Cloud. Each guide features carefully selected digital training, classroom courses, videos, whitepapers, certifications, and more. Explore the guides below by role, solution, or industry area.

👉 more details can be found [here](https://aws.amazon.com/training/ramp-up-guides/)

👉 focus on roles based guide like [DevOps](https://d1.awsstatic.com/training-and-certification/ramp-up_guides/Ramp-Up_Guide_DevOps.pdf).

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

👉 more details can be found [here](https://tutorialsdojo.com/courses/aws-certified-devops-engineer-professional-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Stephane Mareek’s AWS Certified DevOps Engineer Professional Course](https://www.udemy.com/course/aws-certified-devops-engineer-professional-hands-on/)

- [Andrian Cantrill's AWS Certified DevOps Engineer - Professional Course](https://learn.cantrill.io/p/aws-certified-devops-engineer-professional)

- [Ervin Szilágyi’s AWS-DOP-C01 Study Guide Github Repo](https://github.com/Ernyoke/certified-aws-devops-professional)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
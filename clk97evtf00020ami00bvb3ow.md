---
title: "How I passed the AWS Certified Solutions Architect - Professional Exam (SAP-C01)"
seoTitle: "How I passed the AWS Certified Solutions Architect - Professional Exam"
seoDescription: "I recently passed the Solutions Architect - Professional Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way."
datePublished: Sun May 28 2023 04:08:44 GMT+0000 (Coordinated Universal Time)
cuid: clk97evtf00020ami00bvb3ow
slug: how-i-passed-the-aws-certified-solutions-architect-professional-exam-sap-c01
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1660063238161/dgMX-nZ7d.png
tags: software-development, aws, certification

---

## Introduction

- I recently passed the Solutions Architect - Professional Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Solutions Architect - Professional is intended for individuals with two or more years of hands-on experience designing and deploying cloud architecture on AWS.

- Earning AWS Certified Solutions Architect - Professional validates the ability to design, deploy, and evaluate applications on AWS within diverse, complex requirements. This certification helps organizations identify and develop talent with critical skills for implementing cloud initiatives.

- This article gives you an overview which content and training material I used to prepare myself for the AWS SAP-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- Familiarity with AWS CLI, AWS APIs, AWS Cloud Formation templates, the AWS Billing Console, the AWS Management Console, a scripting language, and Windows and Linux environments
- Ability to provide best practice guidance on the architectural design across multiple applications and projects of the enterprise as well as an ability to map business objectives to application/architecture requirements
- Ability to evaluate cloud application requirements and make architectural recommendations for implementation, deployment, and provisioning applications on AWS
- Ability to design a hybrid architecture using key AWS technologies (e.g., VPN, AWS Direct Connect) as well as a continuous integration and deployment process

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
|Domain 1: Design for Organizational Complexity |12.5%|
|Domain 2: Design for New Solutions |31%|
|Domain 3: Migration Planning |15%|
|Domain 4: Cost Control |12.5%|
|Domain 5: Continuous Improvement |29%|
|TOTAL |100%|

**Domain 1: Design for Organizational Complexity**
1.1 Determine cross-account authentication and access strategy for complex organizations.
 Analyze the organizational structure
 Evaluate the current authentication infrastructure
 Analyze the AWS resources at an account level
 Determine an auditing strategy for authentication and access
1.2 Determine how to design networks for complex organizations.
 Outline an IP addressing strategy for VPCs
 Determine DNS strategy
 Classify network traffic and security
 Determine connectivity needs for hybrid environments
 Determine a way to audit network traffic
1.3 Determine how to design a multi-account AWS environment for complex organizations.
 Determine how to use AWS Organizations
 Implement the most appropriate account structure for proper cost allocation, agility, and security
 Recommend a central audit and event notification strategy
 Decide on an access strategy

**Domain 2: Design for New Solutions**
2.1 Determine security requirements and controls when designing and implementing a solution.
 Implement infrastructure as code
 Determine prevention controls for large-scale web applications
 Determine roles and responsibilities of applications
 Determine a secure method to manage credentials for the solutions/applications
 Enable detection controls and security services for large-scale applications
 Enforce host and network security boundaries
 Enable encryption in transit and at rest
2.2 Determine a solution design and implementation strategy to meet reliability requirements.
 Design a highly available application environment
 Determine advanced techniques to detect for failure and service recoverability
 Determine processes and components to monitor and recover from regional service disruptions with regional failover
2.3 Determine a solution design to ensure business continuity.
 Architect an automated, cost-effective back-up solution that supports business continuity across multiple AWS Regions
 Determine an architecture that provides application and infrastructure availability in the event of a service disruption
2.4 Determine a solution design to meet performance objectives.
 Design internet-scale application architectures
 Design an architecture for performance according to business objectives
 Apply design patterns to meet business objectives with caches, buffering, and replicas
2.5 Determine a deployment strategy to meet business requirements when designing and implementing a solution.
 Determine resource provisioning strategy to meet business objectives
 Determine a migration process to change the version of a service
 Determine services to meet deployment strategy
 Determine patch management strategy

**Domain 3: Migration Planning**
3.1 Select existing workloads and processes for potential migration to the cloud.
 Complete an application migration assessment
 Classify applications according to the six Rs (re-host, re-platform, re-purchase, refactor, retire, and retain)
3.2 Select migration tools and/or services for new and migrated solutions based on detailed AWS knowledge.
 Select an appropriate database transfer mechanism
 Select an appropriate data transfer service
 Select an appropriate data transfer target
 Select an appropriate server migration mechanism
 Apply the appropriate security methods to the migration tools
3.3 Determine a new cloud architecture for an existing solution.
 Evaluate business applications and determine the target cloud architecture
 Break down the functionality of applications into services
 Determine target database platforms
3.4 Determine a strategy for migrating existing on-premises workloads to the cloud.
 Determine the desired prioritization strategy of the organization
 Analyze data volume and rate of change to determine a data transfer strategy
 Evaluate cutover strategies
 Assess internal and external compliance requirements for a successful migration

**Domain 4: Cost Control**
4.1 Select a cost-effective pricing model for a solution.
 Purchase resources based on usage requirements
 Identify when to use different storage tiers
4.2 Determine which controls to design and implement that will ensure cost optimization.
 Determine an AWS-generated cost allocation tags strategy that allows mapping cost to business units
 Determine a mechanism to monitor when underutilized resources are present
 Determine a way to manage commonly deployed resources to achieve governance
 Define a way to plan costs that do not exceed the budget amount
4.3 Identify opportunities to reduce cost in an existing architecture.
 Distinguish opportunities to use AWS Managed Services
 Determine which services are most cost-effective in meeting business objectives

**Domain 5: Continuous Improvement for Existing Solutions**
5.1 Troubleshoot solutions architectures.
 Assess an existing application architecture for deficiencies
 Analyze application and infrastructure logs
 Test possible solutions in non-production environment
5.2 Determine a strategy to improve an existing solution for operational excellence.
 Determine the most appropriate logging and monitoring strategy
 Recommend the appropriate AWS offering(s) to enable configuration management automation
5.3 Determine a strategy to improve the reliability of an existing solution.
 Evaluate existing architecture to determine areas that are not sufficiently reliable
 Remediate single points of failure
 Enable data replication, self-healing, and elastic features and services
 Test the reliability of the new solution
5.4 Determine a strategy to improve the performance of an existing solution.
 Reconcile current performance metrics against performance targets
 Identify and examine performance bottlenecks
 Recommend and test potential remediation solutions
5.5 Determine a strategy to improve the security of an existing solution.
 Evaluate AWS Secrets Manager strategy
 Audit the environment for security vulnerabilities
 Enable manual and/or automated responses to the detection of vulnerabilities
5.6 Determine how to improve the deployment of an existing solution.
 Evaluate appropriate tooling to enable infrastructure as code
 Evaluate current deployment processes for improvement opportunities
 Test automated deployment and rollback strategies

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS-Certified-Solutions-Architect-Professional_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “Ultimate AWS Certified Solutions Architect Professional” by Stephane Maarek which is a very good course and covers all the most important aspects of the AWS and also deep dive into some of the AWS related solution architectures. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-solutions-architect-professional/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi). I already have a list of projects which I have shared [here](https://github.com/Cloud-Tech-Projects).

3)**📋 AWS Ramp-Up Guides:** Your guides to learning the AWS Cloud.

- AWS Ramp-Up Guides offer a variety of resources to help you build your skills and knowledge of the AWS Cloud. Each guide features carefully selected digital training, classroom courses, videos, whitepapers, certifications, and more. Explore the guides below by role, solution, or industry area.

👉 more details can be found [here](https://aws.amazon.com/training/ramp-up-guides/)

👉 focus more on role based guide like [architect](https://d1.awsstatic.com/training-and-certification/ramp-up_guides/Ramp-Up_Guide_Architect.pdf).


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

👉 more details can be found [here](https://portal.tutorialsdojo.com/courses/aws-certified-solutions-architect-professional-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Stephane Mareek’s Ultimate AWS Certified Solutions Architect Professional Course](https://www.udemy.com/course/aws-solutions-architect-professional/)

- [Andrian Cantrill's AWS Certified Solutions Architect - Professional Course](https://learn.cantrill.io/p/aws-certified-solutions-architect-professional)

- [Ervin Szilágyi’s AWS-SAP-C01 Study Guide Github Repo](https://github.com/Ernyoke/certified-aws-solutions-architect-professional)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
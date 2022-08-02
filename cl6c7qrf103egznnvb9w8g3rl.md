## How I passed the AWS Certified Cloud Practitioner Exam (CLF-C01)

## Introduction

- I recently passed the AWS Certified Cloud Practitioner Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Cloud Practitioner is intended for anyone who has basic knowledge of the AWS platform.

- Earning AWS Certified Cloud Practitioner validates cloud fluency and foundational AWS knowledge. This exam helps organizations identify and develop talent with critical knowledge related to implementing cloud initiatives.

- This article gives you an overview which content and training material I used to prepare myself for the AWS CLF-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- Six months of exposure to the AWS Cloud
- Basic understanding of IT services and their uses in the AWS Cloud platform
- Knowledge of core AWS services and use cases, billing and pricing models, security concepts, and how cloud impacts your business

## Exam overview

**Level:** Foundational

**Length:** 90 minutes to complete the exam

**Cost:** 100 USD

Visit[Exam pricing](https://aws.amazon.com/certification/policies/before-testing/#Exam_pricing) for additional cost information.

**Format:** 65 questions, either multiple choice or multiple response

**Delivery method:** Pearson VUE and PSI; testing center or online proctored exam

## Exam Outline

- This exam guide includes weightings, test domains, and objectives for the exam. It is not a comprehensive listing of the content on the exam. However, additional context for each of the objectives is available to help guide your preparation for the exam. 

- The following table lists the main content domains and their
weightings. The table precedes the complete exam content outline, which includes the additional context. The percentage in each domain represents only scored content.


| Domain  | % of Exam |
| ------- | --------- |
|Domain 1: Cloud Concepts |26%|
|Domain 2: Security and Compliance |25%|
|Domain 3: Technology |33%|
|Domain 4: Billing and Pricing |16%|
|TOTAL |100%|

**Domain 1: Cloud Concepts**
1.1 Define the AWS Cloud and its value proposition
 Define the benefits of the AWS cloud including:
o Security
o Reliability
o High Availability
o Elasticity
o Agility
o Pay-as-you go pricing
o Scalability
o Global Reach
o Economy of scale
 Explain how the AWS cloud allows users to focus on business value
o Shifting technical resources to revenue-generating activities as opposed to managing infrastructure
1.2 Identify aspects of AWS Cloud economics
 Define items that would be part of a Total Cost of Ownership proposal
o Understand the role of operational expenses (OpEx)
o Understand the role of capital expenses (CapEx)
o Understand labor costs associated with on-premises operations
o Understand the impact of software licensing costs when moving to the cloud
 Identify which operations will reduce costs by moving to the cloud
o Right-sized infrastructure
o Benefits of automation
o Reduce compliance scope (for example, reporting)
o Managed services (for example, RDS, ECS, EKS, DynamoDB)
1.3 Explain the different cloud architecture design principles
 Explain the design principles
o Design for failure
o Decouple components versus monolithic architecture
o Implement elasticity in the cloud versus on-premises
o Think parallel
**Domain 2: Security and Compliance**
2.1 Define the AWS shared responsibility model
 Recognize the elements of the Shared Responsibility Model
 Describe the customer’s responsibly on AWS
o Describe how the customer’s responsibilities may shift depending on the service used (for example with RDS, Lambda, or EC2)
 Describe AWS responsibilities
2.2 Define AWS Cloud security and compliance concepts
 Identify where to find AWS compliance information
o Locations of lists of recognized available compliance controls (for example, HIPPA, SOCs)
o Recognize that compliance requirements vary among AWS services
 At a high level, describe how customers achieve compliance on AWS
o Identify different encryption options on AWS (for example, In transit, At rest)
 Describe who enables encryption on AWS for a given service
 Recognize there are services that will aid in auditing and reporting
o Recognize that logs exist for auditing and monitoring (do not have to understand the logs)
o Define Amazon CloudWatch, AWS Config, and AWS CloudTrail
 Explain the concept of least privileged access
2.3 Identify AWS access management capabilities
 Understand the purpose of User and Identity Management
o Access keys and password policies (rotation, complexity)
o Multi-Factor Authentication (MFA)
o AWS Identity and Access Management (IAM)
• Groups/users
• Roles
• Policies, managed policies compared to custom policies
o Tasks that require use of root accounts
Protection of root accounts
2.4 Identify resources for security support
 Recognize there are different network security capabilities
o Native AWS services (for example, security groups, Network ACLs, AWS WAF)
o 3rd party security products from the AWS Marketplace
 Recognize there is documentation and where to find it (for example, best practices, whitepapers, official documents)
o AWS Knowledge Center, Security Center, security forum, and security blogs
o Partner Systems Integrators
 Know that security checks are a component of AWS Trusted Advisor
**Domain 3: Technology**
3.1 Define methods of deploying and operating in the AWS Cloud
 Identify at a high level different ways of provisioning and operating in the AWS cloud
o Programmatic access, APIs, SDKs, AWS Management Console, CLI, Infrastructure as Code
 Identify different types of cloud deployment models
o All in with cloud/cloud native
o Hybrid
o On-premises
 Identify connectivity options
o VPN
o AWS Direct Connect
o Public internet
3.2 Define the AWS global infrastructure
 Describe the relationships among Regions, Availability Zones, and Edge Locations
 Describe how to achieve high availability through the use of multiple Availability Zones
o Recall that high availability is achieved by using multiple Availability Zones
o Recognize that Availability Zones do not share single points of failure
 Describe when to consider the use of multiple AWS Regions
o Disaster recovery/business continuity
o Low latency for end-users
o Data sovereignty
 Describe at a high level the benefits of Edge Locations
o Amazon CloudFront
o AWS Global Accelerator
3.3 Identify the core AWS services
 Describe the categories of services on AWS (compute, storage, network, database)
 Identify AWS compute services
o Recognize there are different compute families
o Recognize the different services that provide compute (for example, AWS Lambda
compared to Amazon Elastic Container Service (Amazon ECS), or Amazon EC2, etc.)
o Recognize that elasticity is achieved through Auto Scaling
o Identify the purpose of load balancers
 Identify different AWS storage services
o Describe Amazon S3
o Describe Amazon Elastic Block Store (Amazon EBS)
o Describe Amazon S3 Glacier
o Describe AWS Snowball
o Describe Amazon Elastic File System (Amazon EFS)
o Describe AWS Storage Gateway
 Identify AWS networking services
o Identify VPC
o Identify security groups
o Identify the purpose of Amazon Route 53
o Identify VPN, AWS Direct Connect
 Identify different AWS database services
o Install databases on Amazon EC2 compared to AWS managed databases
o Identify Amazon RDS
o Identify Amazon DynamoDB
o Identify Amazon Redshift
3.4 Identify resources for technology support
 Recognize there is documentation (best practices, whitepapers, AWS Knowledge Center, forums, blogs)
 Identify the various levels and scope of AWS support
o AWS Abuse
o AWS support cases
o Premium support
o Technical Account Managers
 Recognize there is a partner network (marketplace, third-party) including Independent Software Vendors and System Integrators
 Identify sources of AWS technical assistance and knowledge including professional services, solution architects, training and certification, and the Amazon Partner Network
 Identify the benefits of using AWS Trusted Advisor
**Domain 4: Billing and Pricing**
4.1 Compare and contrast the various pricing models for AWS (for example, On-Demand Instances, Reserved Instances, and Spot Instance pricing)
 Identify scenarios/best fit for On-Demand Instance pricing
 Identify scenarios/best fit for Reserved-Instance pricing
o Describe Reserved-Instances flexibility
o Describe Reserved-Instances behavior in AWS Organizations
 Identify scenarios/best fit for Spot Instance pricing
4.2 Recognize the various account structures in relation to AWS billing and pricing
 Recognize that consolidated billing is a feature of AWS Organizations
 Identify how multiple accounts aid in allocating costs across departments
4.3 Identify resources available for billing support
 Identify ways to get billing support and information
o Cost Explorer, AWS Cost and Usage Report, Amazon QuickSight, third-party partners, and AWS Marketplace tools
o Open a billing support case
o The role of the Concierge for AWS Enterprise Support Plan customers
 Identify where to find pricing information on AWS services
o AWS Simple Monthly Calculator
o AWS Services product pages
o AWS Pricing API
 Recognize that alarms/alerts exist
 Identify how tags are used in cost allocation

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-cloud-practitioner/AWS-Certified-Cloud-Practitioner_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “Ultimate AWS Certified Cloud Practitioner” by Stephane Maarek which is a very good course and covers all the most important aspects of the AWS and its fundamentals so this is a very good start. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-certified-cloud-practitioner-new/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi).

3)**📋 AWS Ramp-Up Guides:** Your guides to learning the AWS Cloud.

- AWS Ramp-Up Guides offer a variety of resources to help you build your skills and knowledge of the AWS Cloud. Each guide features carefully selected digital training, classroom courses, videos, whitepapers, certifications, and more. Explore the guides below by role, solution, or industry area.

👉 more details can be found [here]
(https://aws.amazon.com/training/ramp-up-guides/)

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

👉 more details can be found [here](https://tutorialsdojo.com/courses/aws-certified-cloud-practitioner-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Andrew Brown’s AWS Certified Cloud Practitioner Study Course](https://www.freecodecamp.org/news/aws-certified-cloud-practitioner-certification-study-course-pass-the-exam/)

- [Stephane Mareek’s Ultimate AWS Certified Cloud Practitioner](https://www.udemy.com/course/aws-certified-cloud-practitioner-new/)

- [ACloud Guru's AWS Certified Cloud Practitioner (CLF-C01))](https://acloudguru.com/course/aws-certified-cloud-practitioner)

- [AWS Certified Cloud Practitioner Exam Guide by Packt](https://github.com/PacktPublishing/AWS-Certified-Cloud-Practitioner-Exam-Guide)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
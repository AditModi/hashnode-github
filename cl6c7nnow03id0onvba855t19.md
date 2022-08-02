## How I passed the AWS Certified Solutions Architect Associate Exam (SAA-C02)

## Introduction

- I recently passed the AWS Certified Solutions Architect - Associate exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Solutions Architect - Associate is intended for anyone with one or more years of hands-on experience designing available, cost-efficient, fault-tolerant, and scalable distributed systems on AWS.

- The AWS Solutions Architect Associate exam is one of the most popular certification exams out right now for anyone trying to break into cloud engineering, architecting, or web solutions. It covers aspects such as technical deployments of AWS systems, AWS best practices, and other skills needed for competency and working within the Amazon AWS ecosystem.

- This article gives you an overview which content and training material I used to prepare myself for the AWS SAA-C02 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- One year of hands-on experience with AWS technology, including using compute, networking, storage, and database AWS services as well as AWS deployment and management services
- Experience deploying, managing, and operating workloads on AWS as well as implementing security controls and compliance requirements
- Familiarity with using both the AWS Management Console and the AWS Command Line Interface (CLI)
- Understanding of the AWS Well-Architected Framework, AWS networking, security services, and the AWS global infrastructure
- Ability to identify which AWS services meet a given technical requirement and to define technical requirements for an AWS-based application

## Exam overview

**Level:** Associate

**Length:** 130 minutes to complete the exam

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
| Domain 1: Design Resilient Architectures | 30% |
| Domain 2: Design High-Performing Architectures | 28% |
| Domain 3: Design Secure Applications and Architectures | 24% |
| Domain 4: Design Cost-Optimized Architectures | 18% |
| TOTAL | 100% |

**Domain 1: Design Resilient Architectures**
1.1 Design a multi-tier architecture solution
- Determine a solution design based on access patterns.
- Determine a scaling strategy for components used in a design.
- Select an appropriate database based on requirements.
- Select an appropriate compute and storage service based on requirements.
1.2 Design highly available and/or fault-tolerant architectures
- Determine the amount of resources needed to provide a fault-tolerant architecture across Availability Zones.
- Select a highly available configuration to mitigate single points of failure.
- Apply AWS services to improve the reliability of legacy applications when application changes are not possible.
- Select an appropriate disaster recovery strategy to meet business requirements.
- Identify key performance indicators to ensure the high availability of the solution.
1.3 Design decoupling mechanisms using AWS services
- Determine which AWS services can be leveraged to achieve loose coupling of components.
- Determine when to leverage serverless technologies to enable decoupling.
1.4 Choose appropriate resilient storage
- Define a strategy to ensure the durability of data.
- Identify how data service consistency will affect the operation of the application.
- Select data services that will meet the access requirements of the application.
- Identify storage services that can be used with hybrid or non-cloud-native applications.
**Domain 2: Design High-Performing Architectures**
2.1 Identify elastic and scalable compute solutions for a workload
- Select the appropriate instance(s) based on compute, storage, and networking requirements.
- Choose the appropriate architecture and services that scale to meet performance requirements.
- Identify metrics to monitor the performance of the solution.
2.2 Select high-performing and scalable storage solutions for a workload
- Select a storage service and configuration that meets performance demands.
- Determine storage services that can scale to accommodate future needs.
2.3 Select high-performing networking solutions for a workload
- Select appropriate AWS connectivity options to meet performance demands.
- Select appropriate features to optimize connectivity to AWS public services.
- Determine an edge caching strategy to provide performance benefits.
- Select appropriate data transfer service for migration and/or ingestion.
2.4 Choose high-performing database solutions for a workload
- Select an appropriate database scaling strategy.
- Determine when database caching is required for performance improvement.
- Choose a suitable database service to meet performance needs.
**Domain 3: Design Secure Applications and Architectures**
3.1 Design secure access to AWS resources
- Determine when to choose between users, groups, and roles.
- Interpret the net effect of a given access policy.
- Select appropriate techniques to secure a root account.
- Determine ways to secure credentials using features of AWS IAM.
- Determine the secure method for an application to access AWS APIs.
- Select appropriate services to create traceability for access to AWS resources.
3.2 Design secure application tiers
- Given traffic control requirements, determine when and how to use security groups and network ACLs.
- Determine a network segmentation strategy using public and private subnets.
- Select the appropriate routing mechanism to securely access AWS service endpoints or internet-based resources from Amazon VPC.
- Select appropriate AWS services to protect applications from external threats.
3.3 Select appropriate data security options
- Determine the policies that need to be applied to objects based on access patterns.
- Select appropriate encryption options for data at rest and in transit for AWS services.
- Select appropriate key management options based on requirements.
**Domain 4: Design Cost-Optimized Architectures**
4.1 Identify cost-effective storage solutions
- Determine the most cost-effective data storage options based on requirements.
- Apply automated processes to ensure that data over time is stored on storage tiers that minimize costs.
4.2 Identify cost-effective compute and database services
- Determine the most cost-effective Amazon EC2 billing options for each aspect of the workload.
- Determine the most cost-effective database options based on requirements.
- Select appropriate scaling strategies from a cost perspective.
- Select and size compute resources that are optimally suited for the workload.
- Determine options to minimize total cost of ownership (TCO) through managed services and serverless architectures.
4.3 Design cost-optimized network architectures
- Identify when content delivery can be used to reduce costs.
- Determine strategies to reduce data transfer costs within AWS.
- Determine the most cost-effective connectivity options between AWS and on-premises environments.

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “Ultimate AWS Certified Solutions Architect Associate 2022” by Stephane Maarek which is a very good course and covers all the most important aspects of the AWS and its fundamentals so this is a very good start. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/)

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

👉 more details can be found [here](https://click.linksynergy.com/deeplink?id=0wsuN4lypZM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Faws-certified-solutions-architect-associate-amazon-practice-exams-saa-c02%2F)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Andrew Brown’s ExamPro AWS Certified Solutions Architect Course](https://www.freecodecamp.org/news/pass-the-aws-certified-solutions-architect-exam-with-this-free-10-hour-course/)

- [Stephane Mareek’s Ultimate AWS Certified Solutions Architect Associate Course](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02)

- [Andrian Cantrill's AWS Certified Solutions Architect - Associate (SAA-C02)](https://learn.cantrill.io/p/aws-certified-solutions-architect-associate-saa-c02)

- [keenanromain’s AWS-SAA-C02 Study Guide Github Repo](https://github.com/keenanromain/AWS-SAA-C02-Study-Guide)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
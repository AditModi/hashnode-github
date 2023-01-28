# How I passed the AWS Certified Database — Specialty Exam (DBS-C01)

## Introduction

- I recently passed the AWS Certified Database — Specialty Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Database – Specialty is intended for individuals with experience and expertise working with on-premises and AWS Cloud-based relational and non-relational databases.

- Earning AWS Certified Database – Specialty validates expertise in recommending, designing, and maintaining optimal AWS database solutions. This exam helps organizations identify and develop talent with critical skills for implementing cloud initiatives.

- This article gives you an overview which content and training material I used to prepare myself for the AWS DBS-C01 Exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- Five years of experience with common database technologies
- Two years of hands-on experience and expertise working with on-premises and AWS Cloud-based relational and NoSQL databases
- Ability to understand and differentiate the key features of AWS database services
- Ability to analyze needs and requirements to design and recommend appropriate database solutions by using AWS services

## Exam overview

**Level:** Specialty

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
|Domain 1: Workload-Specific Database Design |26%|
|Domain 2: Deployment and Migration |20%|
|Domain 3: Management and Operations |18%|
|Domain 4: Monitoring and Troubleshooting |18%|
|Domain 5: Database Security |18%|
|TOTAL |100%|

**Domain 1: Workload-Specific Database Design**
1.1 Select appropriate database services for specific types of data and workloads.
 Differentiate between ACID vs. BASE workloads
 Explain appropriate uses of types of databases (e.g., relational, key-value, document, in-memory, graph, time series, ledger)
 Identify use cases for persisted data vs. ephemeral data
1.2 Determine strategies for disaster recovery and high availability.
 Select Region and Availability Zone placement to optimize database performance
 Determine implications of Regions and Availability Zones on disaster recovery/high availability strategies
 Differentiate use cases for read replicas and Multi-AZ deployments
1.3 Design database solutions for performance, compliance, and scalability.
 Recommend serverless vs. instance-based database architecture
 Evaluate requirements for scaling read replicas
 Define database caching solutions
 Evaluate the implications of partitioning, sharding, and indexing
 Determine appropriate instance types and storage options
 Determine auto-scaling capabilities for relational and NoSQL databases
 Determine the implications of Amazon DynamoDB adaptive capacity
 Determine data locality based on compliance requirements
1.4 Compare the costs of database solutions.
 Determine cost implications of Amazon DynamoDB capacity units, including on-demand vs. provisioned capacity
 Determine costs associated with instance types and automatic scaling
 Design for costs including high availability, backups, multi-Region, Multi-AZ, and storage type options
 Compare data access costs

**Domain 2: Deployment and Migration**
2.1 Automate database solution deployments.
 Evaluate application requirements to determine components to deploy
 Choose appropriate deployment tools and services (e.g., AWS CloudFormation, AWS CLI)
2.2 Determine data preparation and migration strategies.
 Determine the data migration method (e.g., snapshots, replication, restore)
 Evaluate database migration tools and services (e.g., AWS DMS, native database tools)
 Prepare data sources and targets
 Determine schema conversion methods (e.g., AWS Schema Conversion Tool)
 Determine heterogeneous vs. homogeneous migration strategies
2.3 Execute and validate data migration.
 Design and script data migration
 Run data extraction and migration scripts
 Verify the successful load of data

**Domain 3: Management and Operations**
3.1 Determine maintenance tasks and processes.
 Account for the AWS shared responsibility model for database services
 Determine appropriate maintenance window strategies
 Differentiate between major and minor engine upgrades
3.2 Determine backup and restore strategies.
 Identify the need for automatic and manual backups/snapshots
 Differentiate backup and restore strategies (e.g., full backup, point-in-time, encrypting backups cross-Region)
 Define retention policies
 Correlate the backup and restore to recovery point objective (RPO) and recovery time objective (RTO) requirements
3.3 Manage the operational environment of a database solution.
 Orchestrate the refresh of lower environments
 Implement configuration changes (e.g., in Amazon RDS option/parameter groups or Amazon DynamoDB indexing changes)
 Automate operational tasks
 Take action based on AWS Trusted Advisor reports

**Domain 4: Monitoring and Troubleshooting**
4.1 Determine monitoring and alerting strategies.
 Evaluate monitoring tools (e.g., Amazon CloudWatch, Amazon RDS Performance Insights, database native)
 Determine appropriate parameters and thresholds for alert conditions
 Use tools to notify users when thresholds are breached (e.g., Amazon SNS, Amazon SQS, Amazon CloudWatch dashboards)
4.2 Troubleshoot and resolve common database issues.
 Identify, evaluate, and respond to categories of failures (e.g., troubleshoot connectivity; instance, storage, and partitioning issues)
 Automate responses when possible
4.3 Optimize database performance.
 Troubleshoot database performance issues
 Identify appropriate AWS tools and services for database optimization
 Evaluate the configuration, schema design, queries, and infrastructure to improve performance

**Domain 5: Database Security**
5.1 Encrypt data at rest and in transit.
 Encrypt data in relational and NoSQL databases
 Apply SSL connectivity to databases
 Implement key management (e.g., AWS KMS, AWS CloudHSM)

5.2 Evaluate auditing solutions.
 Determine auditing strategies for structural/schema changes (e.g., DDL)
 Determine auditing strategies for data changes (e.g., DML)
 Determine auditing strategies for data access (e.g., queries)
 Determine auditing strategies for infrastructure changes (e.g., AWS CloudTrail)
 Enable the export of database logs to Amazon CloudWatch Logs
5.3 Determine access control and authentication mechanisms.
 Recommend authentication controls for users and roles (e.g., IAM, native credentials, Active Directory)
 Recommend authorization controls for users (e.g., policies)
5.4 Recognize potential security vulnerabilities within database solutions.
 Determine security group rules and NACLs for database access
 Identify relevant VPC configurations (e.g., VPC endpoints, public vs. private subnets, demilitarized zone)
 Determine appropriate storage methods for sensitive data

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-database-specialty/AWS-Certified-Database-Specialty_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “Ultimate AWS Certified Database Specialty” by Stephane Maarek and Riyaz Sayyad which is a very good course and covers all the most important aspects of the AWS and helps you learn RDS, Aurora, DynamoDB, DMS, ElastiCache in depth. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-certified-database-specialty-dbs/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi). here is the list of projects I had done as part of my [exam prep](https://github.com/AWS-Big-Data-Projects).

3)**📋 AWS Ramp-Up Guides:** Your guides to learning the AWS Cloud.

- AWS Ramp-Up Guides offer a variety of resources to help you build your skills and knowledge of the AWS Cloud. Each guide features carefully selected digital training, classroom courses, videos, whitepapers, certifications, and more. Explore the guides below by role, solution, or industry area.

👉 more details can be found [here](https://aws.amazon.com/training/ramp-up-guides/)

👉 focus on the [database solution guide](https://d1.awsstatic.com/training-and-certification/ramp-up_guides/Ramp-Up_Guide_Databases.pdf)

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

👉 more details can be found [here](https://tutorialsdojo.com/courses/aws-certified-database-specialty-practice-exams/)

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

- [Stephane Mareek and Riyaz Sayyed’s Ultimate AWS Certified Database Specialty Course](https://www.udemy.com/course/aws-certified-database-specialty-dbs/)

- [Jayendra Patil's AWS Certified Database – Specialty (DBS-C01) Exam Learning Path](https://jayendrapatil.com/aws-certified-database-specialty-dbs-c01-exam-learning-path/)

- [Borko’s AWS-DBS-C01 Study Guide Github Repo](https://github.com/borkod/AWS-Certified-Database-Specialty-Exam-Guide)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
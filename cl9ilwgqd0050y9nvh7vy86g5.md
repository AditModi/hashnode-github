# How I passed the AWS Certified Data Analytics —Specialty Exam (DAS-C01)

## Introduction

- I recently passed the AWS Certified Data Analytics — Specialty Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Data Analytics – Specialty is intended for individuals with experience and expertise working with AWS services to design, build, secure, and maintain analytics solutions.

-  Earning AWS Certified Data Analytics – Specialty validates expertise in using AWS data lakes and analytics services to get insights from data. This certification helps organizations identify and develop talent with critical skills for implementing cloud initiatives. 

- This article gives you an overview which content and training material I used to prepare myself for the AWS DAS-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- Five years of experience with common data analytics technologies
- Two years of hands-on experience and expertise working with AWS services to design, build, secure, and maintain analytics solutions
- Ability to define AWS data analytics services and understand how they integrate with each other
- Ability to explain how AWS data analytics services fit in the data lifecycle of collection, storage, processing, and visualization

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
|Domain 1: Collection |18%|
|Domain 2: Storage and Data Management |22%|
|Domain 3: Processing |24%|
|Domain 4: Analysis and Visualization |18%|
|Domain 5: |Security 18%|
|TOTAL |100%|

**Domain 1: Collection**
1.1 Determine the operational characteristics of the collection system
 Evaluate that the data loss is within tolerance limits in the event of failures
 Evaluate costs associated with data acquisition, transfer, and provisioning from various sources into the collection system (e.g., networking, bandwidth, ETL/data migration costs)
 Assess the failure scenarios that the collection system may undergo, and take remediation actions based on impact
 Determine data persistence at various points of data capture
 Identify the latency characteristics of the collection system
1.2 Select a collection system that handles the frequency, volume, and the source of data
 Describe and characterize the volume and flow characteristics of incoming data (streaming, transactional, batch)
 Match flow characteristics of data to potential solutions
 Assess the tradeoffs between various ingestion services taking into account scalability, cost, fault tolerance, latency, etc.
 Explain the throughput capability of a variety of different types of data collection and identify bottlenecks
 Choose a collection solution that satisfies connectivity constraints of the source data system
1.3 Select a collection system that addresses the key properties of data, such as order, format, and compression
 Describe how to capture data changes at the source
 Discuss data structure and format, compression applied, and encryption requirements
 Distinguish the impact of out-of-order delivery of data, duplicate delivery of data, and the tradeoffs between at-most-once, exactly-once, and at-least-once processing
 Describe how to transform and filter data during the collection process
**Domain 2: Storage and Data Management**
2.1 Determine the operational characteristics of the storage solution for analytics
 Determine the appropriate storage service(s) on the basis of cost vs. performance
 Understand the durability, reliability, and latency characteristics of the storage solution based on requirements
 Determine the requirements of a system for strong vs. eventual consistency of the storage system
 Determine the appropriate storage solution to address data freshness requirements
2.2 Determine data access and retrieval patterns
 Determine the appropriate storage solution based on update patterns (e.g., bulk, transactional, micro batching)
 Determine the appropriate storage solution based on access patterns (e.g., sequential vs. random access, continuous usage [vs.ad](http://vs.ad/) hoc)
 Determine the appropriate storage solution to address change characteristics of data (appendonly changes vs. updates)
 Determine the appropriate storage solution for long-term storage vs. transient storage
 Determine the appropriate storage solution for structured vs. semi-structured data
 Determine the appropriate storage solution to address query latency requirements
2.3 Select appropriate data layout, schema, structure, and format
 Determine appropriate mechanisms to address schema evolution requirements
 Select the storage format for the task
 Select the compression/encoding strategies for the chosen storage format
 Select the data sorting and distribution strategies and the storage layout for efficient data access
 Explain the cost and performance implications of different data distributions, layouts, and formats (e.g., size and number of files)
 Implement data formatting and partitioning schemes for data-optimized analysis
2.4 Define data lifecycle based on usage patterns and business requirements
 Determine the strategy to address data lifecycle requirements
 Apply the lifecycle and data retention policies to different storage solutions
2.5 Determine the appropriate system for cataloging data and managing metadata
 Evaluate mechanisms for discovery of new and updated data sources
 Evaluate mechanisms for creating and updating data catalogs and metadata
 Explain mechanisms for searching and retrieving data catalogs and metadata
 Explain mechanisms for tagging and classifying data
**Domain 3: Processing**
3.1 Determine appropriate data processing solution requirements
 Understand data preparation and usage requirements
 Understand different types of data sources and targets
 Evaluate performance and orchestration needs
 Evaluate appropriate services for cost, scalability, and availability
3.2 Design a solution for transforming and preparing data for analysis
 Apply appropriate ETL/ELT techniques for batch and real-time workloads
 Implement failover, scaling, and replication mechanisms
 Implement techniques to address concurrency needs
 Implement techniques to improve cost-optimization efficiencies
 Apply orchestration workflows
 Aggregate and enrich data for downstream consumption
3.3 Automate and operationalize data processing solutions
 Implement automated techniques for repeatable workflows
 Apply methods to identify and recover from processing failures
 Deploy logging and monitoring solutions to enable auditing and traceability
**Domain 4: Analysis and Visualization**
4.1 Determine the operational characteristics of the analysis and visualization solution
 Determine costs associated with analysis and visualization
 Determine scalability associated with analysis
 Determine failover recovery and fault tolerance within the RPO/RTO
 Determine the availability characteristics of an analysis tool
 Evaluate dynamic, interactive, and static presentations of data
 Translate performance requirements to an appropriate visualization approach (pre-compute and consume static data vs. consume dynamic data)
4.2 Select the appropriate data analysis solution for a given scenario
 Evaluate and compare analysis solutions
 Select the right type of analysis based on the customer use case (streaming, interactive, collaborative, operational)
4.3 Select the appropriate data visualization solution for a given scenario
 Evaluate output capabilities for a given analysis solution (metrics, KPIs, tabular, API)
 Choose the appropriate method for data delivery (e.g., web, mobile, email, collaborative notebooks)
 Choose and define the appropriate data refresh schedule
 Choose appropriate tools for different data freshness requirements (e.g., Amazon Elasticsearch Service vs. Amazon QuickSight vs. Amazon EMR notebooks)
 Understand the capabilities of visualization tools for interactive use cases (e.g., drill down, drill
through and pivot)
 Implement the appropriate data access mechanism (e.g., in memory vs. direct access)
 Implement an integrated solution from multiple heterogeneous data sources
**Domain 5: Security**
5.1 Select appropriate authentication and authorization mechanisms
 Implement appropriate authentication methods (e.g., federated access, SSO, IAM)
 Implement appropriate authorization methods (e.g., policies, ACL, table/column level permissions)
 Implement appropriate access control mechanisms (e.g., security groups, role-based control)
5.2 Apply data protection and encryption techniques
 Determine data encryption and masking needs
 Apply different encryption approaches (server-side encryption, client-side encryption, AWS KMS, AWS CloudHSM)
 Implement at-rest and in-transit encryption mechanisms
 Implement data obfuscation and masking techniques
 Apply basic principles of key rotation and secrets management
5.3 Apply data governance and compliance controls
 Determine data governance and compliance requirements
 Understand and configure access and audit logging across data analytics services
 Implement appropriate controls to meet compliance requirements


👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-data-analytics-specialty/AWS-Certified-Data-Analytics-Specialty_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “AWS Certified Data Analytics Specialty - Hands On!” by Stephane Maarek & Frank Kane which is a very good course and covers all the most important aspects of the AWS and its fundamentals so this is a very good start. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-data-analytics/)

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

👉 more details can be found [here](https://tutorialsdojo.com/courses/aws-certified-data-analytics-specialty-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Stephane Mareek and Frank Kane’s AWS Certified Data Analytics Specialty - Hands On!](https://www.udemy.com/course/aws-data-analytics/)

- [ACloud Guru's AWS Certified Data Analytics - Specialty Course](https://acloudguru.com/course/aws-certified-data-analytics-specialty)

- [Linux Academy’s AWS-DAS-C01 Study Guide Github Repo](https://github.com/linuxacademy/Content-AWS-Certified-Data-Analytics---Speciality)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
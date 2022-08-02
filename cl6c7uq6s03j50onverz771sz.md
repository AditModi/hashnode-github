## How I passed the AWS Certified Developer Associate Exam (DVA-C01)

## Introduction

- I recently passed the AWS Certified Developer Associate Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Developer – Associate is intended for anyone with one or more years of hands-on experience developing and maintaining an AWS-based application.

- AWS Certified Developer Associate Exam demonstrates your understanding of core AWS services, their uses, and basic AWS architecture best practices as well as your proficiency in development, deployment, and debugging cloud-based applications using AWS.

- This article gives you an overview which content and training material I used to prepare myself for the AWS DVA-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- In-depth knowledge of at least one high-level programming language
- Understanding of core AWS services, uses of the services, and basic AWS architecture best practices, including the AWS Shared Responsibility Model, application lifecycle management, and the use of containers in the development process
- Proficiency in developing, deploying, and debugging cloud-based applications using AWS and writing code for serverless applications
- Ability to identify key features of AWS services and use the AWS service APIs, AWS CLI, and SDKs to write applications
- Ability to apply a basic understanding of cloud-native applications to write code
- Ability to author, maintain, and debug code modules on AWS

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
|Domain 1: Deployment |22%|
|Domain 2: Security |26%|
|Domain 3: Development with AWS Services |30%|
|Domain 4: Refactoring |10%|
|Domain 5: Monitoring and Troubleshooting |12%|
|TOTAL |100%|

**Domain 1: Deployment**
1.1 Deploy written code in AWS using existing CI/CD pipelines, processes, and patterns.
- Commit code to a repository and invoke build, test and/or deployment actions
- Use labels and branches for version and release management
- Use AWS CodePipeline to orchestrate workflows against different environments
- Apply AWS CodeCommit, AWS CodeBuild, AWS CodePipeline, AWS CodeStar, and AWS CodeDeploy for CI/CD purposes
- Perform a roll back plan based on application deployment policy
1.2 Deploy applications using AWS Elastic Beanstalk.
- Utilize existing supported environments to define a new application stack
- Package the application
- Introduce a new application version into the Elastic Beanstalk environment
- Utilize a deployment policy to deploy an application version (i.e., all at once, rolling, rolling with batch, immutable)
- Validate application health using Elastic Beanstalk dashboard
- Use Amazon CloudWatch Logs to instrument application logging
1.3 Prepare the application deployment package to be deployed to AWS.
- Manage the dependencies of the code module (like environment variables, config files and static image files) within the package
- Outline the package/container directory structure and organize files appropriately
- Translate application resource requirements to AWS infrastructure parameters (e.g., memory,
cores)
1.4 Deploy serverless applications.
- Given a use case, implement and launch an AWS Serverless Application Model (AWS SAM) template
- Manage environments in individual AWS services (e.g., Differentiate between Development,
Test, and Production in Amazon API Gateway)
Domain 2: Security
2.1 Make authenticated calls to AWS services.
- Communicate required policy based on least privileges required by application.
- Assume an IAM role to access a service
- Use the software development kit (SDK) credential provider on-premises or in the cloud to access AWS services (local credentials vs. instance roles)
2.2 Implement encryption using AWS services.
- Encrypt data at rest (client side; server side; envelope encryption) using AWS services
- Encrypt data in transit
2.3 Implement application authentication and authorization.
- Add user sign-up and sign-in functionality for applications with Amazon Cognito identity or user pools
- Use Amazon Cognito-provided credentials to write code that access AWS services.
- Use Amazon Cognito sync to synchronize user profiles and data
- Use developer-authenticated identities to interact between end user devices, backend authentication, and Amazon Cognito
Domain 3: Development with AWS Services
3.1 Write code for serverless applications.
- Compare and contrast server-based vs. serverless model (e.g., micro services, stateless nature of serverless applications, scaling serverless applications, and decoupling layers of serverless applications)
- Configure AWS Lambda functions by defining environment variables and parameters (e.g., memory, time out, runtime, handler)
- Create an API endpoint using Amazon API Gateway
- Create and test appropriate API actions like GET, POST using the API endpoint
- Apply Amazon DynamoDB concepts (e.g., tables, items, and attributes)
- Compute read/write capacity units for Amazon DynamoDB based on application requirements
- Associate an AWS Lambda function with an AWS event source (e.g., Amazon API Gateway,
Amazon CloudWatch event, Amazon S3 events, Amazon Kinesis)
- Invoke an AWS Lambda function synchronously and asynchronously
3.2 Translate functional requirements into application design.
- Determine real-time vs. batch processing for a given use case
- Determine use of synchronous vs. asynchronous for a given use case
- Determine use of event vs. schedule/poll for a given use case
- Account for tradeoffs for consistency models in an application design
3.3 Implement application design into application code.
- Write code to utilize messaging services (e.g., SQS, SNS)
- Use Amazon ElastiCache to create a database cache
- Use Amazon DynamoDB to index objects in Amazon S3
- Write a stateless AWS Lambda function
- Write a web application with stateless web servers (Externalize state)
3.4 Write code that interacts with AWS services by using APIs, SDKs, and AWS CLI.
- Choose the appropriate APIs, software development kits (SDKs), and CLI commands for the code components
- Write resilient code that deals with failures or exceptions (i.e., retries with exponential back off and jitter)
Domain 4: Refactoring
4.1 Optimize applications to best use AWS services and features.
- Implement AWS caching services to optimize performance (e.g., Amazon ElastiCache, Amazon API Gateway cache)
- Apply an Amazon S3 naming scheme for optimal read performance
4.2 Migrate existing application code to run on AWS.
- Isolate dependencies
- Run the application as one or more stateless processes
- Develop in order to enable horizontal scalability
- Externalize state
Domain 5: Monitoring and Troubleshooting
5.1 Write code that can be monitored.
- Create custom Amazon CloudWatch metrics
- Perform logging in a manner available to systems operators
- Instrument application source code to enable tracing in AWS X-Ray
5.2 Perform root cause analysis on faults found in testing or production.
- Interpret the outputs from the logging mechanism in AWS to identify errors in logs
- Check build and testing history in AWS services (e.g., AWS CodeBuild, AWS CodeDeploy, AWS CodePipeline) to identify issues
- Utilize AWS services (e.g., Amazon CloudWatch, VPC Flow Logs, and AWS X-Ray) to locate a specific faulty component

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “Ultimate AWS Certified Developer Associate” by Stephane Maarek which is a very good course and covers all the most important aspects of the AWS and its fundamentals so this is a very good start. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉 some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi).

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

👉 more details can be found [here](https://tutorialsdojo.com/courses/aws-certified-developer-associate-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 


## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Andrew Brown’s ExamPro AWS Certified Developer - Associate Course](https://www.youtube.com/watch?v=RrKRN9zRBWs)

- [Stephane Mareek’s Ultimate AWS Certified Developer Associate](https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/)

- [Andrian Cantrill's AWS Certified Developer - Associate](https://learn.cantrill.io/p/aws-certified-developer-associate)

- [Mostafa Abdo’s Certified AWS Associate Developer Notes Github Repo](https://github.com/itsmostafa/certified-aws-developer-associate-notes)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
# Practicing Continuous Integration and Continuous Delivery on AWS | AWS White Paper Summary

* This paper explains the features and benefits of using continuous integration and continuous delivery (CI/CD) along with Amazon Web Services (AWS) tooling in your software development environment. Continuous integration and continuous delivery are best practices and a vital part of a DevOps initiative.

# The challenge of software delivery

* Enterprises today face the challenges of rapidly changing competitive landscapes, evolving security requirements, and performance scalability. Enterprises must bridge the gap between operations stability and rapid feature development. Continuous integration and continuous delivery (CI/CD) are practices that enable rapid software changes while maintaining system stability and security.

* Amazon realized early on that the business needs of delivering features for Amazon.com retail customers, Amazon subsidiaries, and Amazon Web Services (AWS) would require new and innovative ways of delivering software. At the scale of a company like Amazon, thousands of independent software teams must be able to work in parallel to deliver software quickly, securely, reliably, and with zero tolerance for outages.

* By learning how to deliver software at high velocity, Amazon and other forward-thinking organizations pioneered DevOps . DevOps is a combination of cultural philosophies, practices, and tools that increases an organization’s ability to deliver applications and services at high velocity. Using DevOps principles, organizations can evolve and improve products at a faster pace than organizations that use traditional software development and infrastructure management processes. This speed enables organizations to better serve their customers and compete more effectively in the market.

* Some of these principles, such as two-pizza teams and microservices/service-oriented architecture (SOA), are out of the scope of this whitepaper. This whitepaper discusses the CI/CD capability that Amazon has built and continuously improved. CI/CD is key to delivering software features rapidly and reliably.

* AWS now offers these CI/CD capabilities as a set of developer services: AWS CodeStar, AWS CodeCommit, AWS CodePipeline, AWS CodeBuild, AWS CodeDeploy, and AWS CodeArtifact. Developers and IT operations professionals practicing DevOps can use these services to rapidly, safely, and securely deliver software. Together, they help you securely store and apply version control to your application's source code. You can use AWS CodeStar to rapidly orchestrate an end-to-end software release workflow using these services. 

* For an existing environment, AWS CodePipeline has the flexibility to integrate each service independently with your existing tools. These are highly available, easily integrated services that can be accessed through the AWS Management Console, AWS application programming interfaces (APIs), and AWS software development toolkits (SDKs) like any other AWS service.

# What is continuous integration and continuous delivery/deployment?

## Continuous integration

* Continuous integration (CI) is a software development practice where developers regularly merge their code changes into a central repository, after which automated builds and tests are run. CI most often refers to the build or integration stage of the software release process and requires both an automation component (for example a CI or build service) and a cultural component (for example learning to integrate frequently). The key goals of CI are to find and address bugs more quickly, improve software quality, and reduce the time it takes to validate and release new software updates.

## Continuous delivery and deployment

* Continuous delivery (CD) is a software development practice where code changes are automatically built, tested, and prepared for production release. It expands on continuous integration by deploying all code changes to a testing environment, a production environment, or both after the build stage has been completed. Continuous delivery can be fully automated with a workflow process or partially automated with manual steps at critical points. When continuous delivery is properly implemented, developers always have a deployment-ready build artifact that has passed through a standardized test process.

## Continuous delivery is not continuous deployment

* One misconception about continuous delivery is that it means every change committed is applied to production immediately after passing automated tests. However, the point of continuous delivery is not to apply every change to production immediately, but to ensure that every change is ready to go to production.

# Benefits of continuous delivery

* CD provides numerous benefits for your software development team including automating the process, improving developer productivity, improving code quality, and delivering updates to your customers faster.

* Automate the software release process
* Improve developer productivity
* Improve code quality
* Deliver updates faster

# Implementing continuous integration and continuous delivery

* This section discusses the ways in which you can begin to implement a CI/CD model in your organization. This whitepaper doesn’t discuss how an organization with a mature DevOps and cloud transformation model builds and uses a CI/CD pipeline. To help you on your DevOps journey, AWS has a number of certified DevOps Partners who can provide resources and tooling. 

## Building the pipeline

* This section discusses building the pipeline. Start by establishing a pipeline with just the components needed for CI and then transition later to a continuous delivery pipeline with more components and stages. This section also discusses how you can consider using AWS Lambda functions and manual approvals for large projects, plan for multiple teams, branches, and AWS Regions.

### A pathway to continuous integration/continuous delivery

* CI/CD can be pictured as a pipeline (refer to the following figure), where new code is submitted on one end, tested over a series of stages (source, build, staging, and production), and then published as production-ready code. If your organization is new to CI/CD it can approach this pipeline in an iterative fashion. This means that you should start small, and iterate at each stage so that you can understand and develop your code in a way that will help your organization grow.

### Teams

* AWS recommends organizing three developer teams for implementing a CI/CD environment: an application team, an infrastructure team, and a tools team (refer to the following figure). This organization represents a set of best practices that have been developed and applied in fast-moving startups, large enterprise organizations, and in Amazon itself. The teams should be no larger than groups that two pizzas can feed, or about 10-12 people. This follows the communication rule that meaningful conversations hit limits as group sizes increase and lines of communication multiply.

### Testing stages in continuous integration and continuous delivery

The three CI/CD teams should incorporate testing into the software development lifecycle at the different stages of the CI/CD pipeline. Overall, testing should start as early as possible.

The following testing pyramid is a concept provided by Mike Cohn in Succeeding with Agile. It shows the various software tests in relation to their cost and speed at which they run.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xosks9wwwakgwoxmj50c.png)
*CI/CD testing pyramid*

* Unit tests are on the bottom of the pyramid. They are both the fastest to run and the least expensive. Therefore, unit tests should make up the bulk of your testing strategy. A good rule of thumb is about 70 percent. Unit tests should have near-complete code coverage because bugs caught in this phase can be fixed quickly and cheaply.

* Service, component, and integration tests are above unit tests on the pyramid. These tests require detailed environments and therefore, are more costly in infrastructure requirements and slower to run. Performance and compliance tests are the next level. They require production-quality environments and are more expensive yet. UI and user acceptance tests are at the top of the pyramid and require production-quality environments as well.

* All of these tests are part of a complete strategy to assure high-quality software. However, for speed of development, emphasis is on the number of tests and the coverage in the bottom half of the pyramid.

### Pipeline integration with AWS CodeBuild

AWS CodeBuild is designed to enable your organization to build a highly available build process with almost unlimited scale. AWS CodeBuild provides quickstart environments for a number of popular languages plus the ability to run any Docker container that you specify.

With the advantages of tight integration with AWS CodeCommit, AWS CodePipeline, and AWS CodeDeploy, as well as Git and CodePipeline Lambda actions, the CodeBuild tool is highly flexible.

Software can be built through the inclusion of a buildspec.yml file that identifies each of the build steps, including pre- and post- build actions, or specified actions through the CodeBuild tool.

You can view detailed history of each build using the CodeBuild dashboard. Events are stored as Amazon CloudWatch Logs log files.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4fbs7obgdngjqf048749.png)
*CloudWatch Logs log files in AWS CodeBuild* 

### Pipeline integration with Jenkins

* You can use the Jenkins build tool to create delivery pipelines. These pipelines use standard jobs that define steps for implementing continuous delivery stages. However, this approach might not be optimal for larger projects because the current state of the pipeline doesn’t persist between Jenkins restarts, implementing manual approval is not straightforward, and tracking the state of a complex pipeline can be complicated.

* Instead, AWS recommends that you implement continuous delivery with Jenkins by using the AWS Code Pipeline Plugin. This plugin allows complex workflows to be described using Groovy-like domain-specific language and can be used to orchestrate complex pipelines. 

* The AWS Code Pipeline plugin’s functionality can be enhanced by the use of satellite plugins such as the Pipeline Stage View Plugin, which visualizes the current progress of stages defined in a pipeline, or Pipeline Multibranch Plugin, which groups builds from different branches.

* AWS recommends that you store your pipeline configuration in Jenkinsfile and have it checked into a source code repository. This allows for tracking changes to pipeline code and becomes even more important when working with the Pipeline Multibranch Plugin. 

* AWS also recommends that you divide your pipeline into stages. This logically groups the pipeline steps and also enables the Pipeline Stage View Plugin to visualize the current state of the pipeline.

* The following figure shows a sample Jenkins pipeline, with four defined stages visualized by the Pipeline Stage View Plugin. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/t4qlpbuzoa1c8qrxn2n1.png)
*Defined stages of Jenkins pipeline visualized by the Pipeline Stage View Plugin* 

# Deployment methods

* You can consider multiple deployment strategies and variations for rolling out new versions of software in a continuous delivery process. This section discusses the most common deployment methods: all at once (deploy in place), rolling, immutable, and blue/green. AWS indicates which of these methods are supported by AWS CodeDeploy and AWS Elastic Beanstalk.

### **All at once (in-place deployment)**

* All at once (in-place deployment) is a method you can use to roll out new application code to an existing fleet of servers. This method replaces all the code in one deployment action. It requires downtime because all servers in the fleet are updated at once. There is no need to update existing DNS records. In case of a failed deployment, the only way to restore operations is to redeploy the code on all servers again.

## **Rolling deployment**

* With rolling deployment, the fleet is divided into portions so that all of the fleet isn’t upgraded at once. During the deployment process two software versions, new and old, are running on the same fleet. This method allows a zero-downtime update. If the deployment fails, only the updated portion of the fleet will be affected.

## **Immutable and blue/green deployment**

* The immutable pattern specifies a deployment of application code by starting an entirely new set of servers with a new configuration or version of application code. This pattern leverages the cloud capability that new server resources are created with simple API calls.

# Database schema changes

* It’s common for modern software to have a database layer. Typically, a relational database is used, which stores both data and the structure of the data. It’s often necessary to modify the database in the continuous delivery process. Handling changes in a relational database requires special consideration, and it offers other challenges than the ones present when deploying application binaries. Usually, when you upgrade an application binary you stop the application, upgrade it, and then start it again. You don't really bother about the application state, which is handled outside of the application.

* When upgrading databases, you do need to consider state because a database contains much state but comparatively little logic and structure.

* The database schema before and after a change is applied should be considered different versions of the database. You could use tools such as Liquibase and Flyway to manage the versions.

* In general, those tools employ some variant of the following methods:

 * Add a table to the database where a database version is stored.
 * Keep track of database change commands and bunch them together in versioned change sets. In the case of Liquibase, these changes are stored in XML files. Flyway employs a slightly different method where the change sets are handled as separate SQL files or occasionally as separate Java classes for more complex transitions.
 * When Liquibase is being asked to upgrade a database, it looks at the metadata table and determines which change sets to run in order to bring the database up-to-date with the latest version.

# Reference

[Original paper](https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/practicing-continuous-integration-continuous-delivery.pdf)

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
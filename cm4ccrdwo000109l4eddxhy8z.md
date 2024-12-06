---
title: "My Top Pre-re:Invent Picks: Innovations to Watch"
datePublished: Sat Nov 30 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm4ccrdwo000109l4eddxhy8z
slug: my-top-pre-reinvent-picks-innovations-to-watch
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733461675271/2fd6af70-4242-4c18-a8a3-2597e7ddbfb6.png
tags: aws

---

AWS has set the stage for another game-changing re:Invent with exciting pre-event announcements. These updates highlight AWS's focus on innovation and helping customers solve real-world problems. Here are my top picks, why they stand out, and how they address challenges many AWS users face.

#### **1\. ECS Predictive Scaling**

**The Challenge:** Traditional scaling methods react to changes, leading to delayed responses during traffic surges and resource inefficiency during lulls.  
**How It Helps:** Predictive Scaling for Amazon ECS leverages historical data and machine learning to anticipate workload demands and adjust resources accordingly. This proactive approach optimizes compute costs and ensures smooth application performance during demand fluctuations.  
📖 [Learn more about ECS Predictive Scaling](https://aws.amazon.com/blogs/containers/optimize-compute-resources-on-amazon-ecs-with-predictive-scaling/)  
  

![Figure 5. Predictive Scaling policy recommendations](https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2024/11/21/Picture16-2.png align="left")

![Figure 6. Predictive Scaling’s Load and Capacity graph](https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2024/11/21/image-75.png align="left")

![Figure 8. ECS service scaling activities](https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2024/11/21/Picture19.png align="left")

#### **2\. DynamoDB Price Reductions**

**The Challenge:** On-demand pricing in DynamoDB provided flexibility but came at a premium, often pushing users towards provisioned capacity for cost efficiency.  
**How It Helps:** Recent price cuts—up to 50% for on-demand throughput and 67% for global tables—bridge the gap, making on-demand pricing a viable, cost-effective option for dynamic workloads without sacrificing flexibility.  
📖 [Read more about DynamoDB pricing updates](https://aws.amazon.com/blogs/database/new-amazon-dynamodb-lowers-pricing-for-on-demand-throughput-and-global-tables/)

**3\. Aurora Serverless v2 Scale-to-Zero**

**The Challenge:** Previously, Aurora Serverless v2 could not scale to zero, leading to idle costs, which drew criticism about its “serverless” claims.  
**How It Helps:** By scaling capacity down to 0 ACUs, Aurora Serverless v2 now enables true serverless functionality, reducing costs during inactivity and making it perfect for dev/test environments or sporadically used apps.  
📖 [Dive deeper into Aurora Scale-to-Zero](https://aws.amazon.com/blogs/database/introducing-scaling-to-0-capacity-with-amazon-aurora-serverless-v2/)

![](https://d2908q01vomqb2.cloudfront.net/887309d048beef83ad3eabf2a79a64a389ab1c9f/2024/11/19/DBBLOG-4386-img2.png align="left")

**4\. CloudFormation Timeline View**

**The Challenge:** Diagnosing deployment issues in CloudFormation often required digging through logs, slowing resolution.  
**How It Helps:** Similar to Jenkins build pipelines, the Timeline View provides a visual history of deployment events, simplifying the identification of root causes for failures and enhancing the debugging process.  
📖 [Explore the Timeline View in CloudFormation](https://aws.amazon.com/blogs/devops/peek-inside-your-aws-cloudformation-deployments-with-timeline-view/)

![CloudFormation deployment timeline view feature overview](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2024/11/11/Screenshot-2024-10-31-at-11.29.15%E2%80%AFAM-1-1024x599.png align="left")

![CloudFormation deployment timeline view color bars for each resource action](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2024/11/12/color-bar-timeline-graph-300x223.png align="left")

![Deployment timeline view of a stack in rollback complete view 1](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2024/11/11/Deployment-timeline-view-of-a-stack-in-rollback-complete.png align="left")

**5\. Step Functions Variables**

**The Challenge:** Managing and transforming data within workflows often required custom code, complicating orchestration.  
**How It Helps:** Variables in AWS Step Functions simplify workflows by introducing scoping and transformation features akin to programming languages, enabling developers to streamline complex workflows without extra custom logic.  
📖 [Learn more about Step Functions Variables](https://aws.amazon.com/blogs/compute/simplifying-developer-experience-with-variables-and-jsonata-in-aws-step-functions/)

**6\. EKS Auto Mode**

**The Challenge:** Setting up Kubernetes clusters required managing infrastructure components manually, increasing operational complexity.  
**How It Helps:** Amazon EKS Auto Mode automates the provisioning and management of Kubernetes clusters, simplifying operations with a single-click setup for compute, storage, and networking.  
📖 [Discover EKS Auto Mode](https://aws.amazon.com/about-aws/whats-new/2024/12/amazon-eks-auto-mode/)

![After Auto Mode](https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2024/12/03/after-auto-mode.png align="left")

**What’s Next?**

These innovations show how AWS is continuously raising the bar for cloud computing. Whether you’re optimizing costs, improving operational efficiency, or simplifying workflows, there’s something for everyone.

What are your favorites from these updates? Let me know in the comments!
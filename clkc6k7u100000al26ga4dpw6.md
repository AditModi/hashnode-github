---
title: "Serverless Analytics on AWS: Redshift Serverless and Kinesis on-demand (Part - 2)"
seoTitle: "Serverless Analytics on AWS: Redshift Serverless and Kinesis on-demand"
seoDescription: "In Part 1 of our blog series on Serverless Analytics on AWS, we explored the benefits of serverless analytics and delved into Amazon EMR Serverless and Amaz"
datePublished: Sat Nov 26 2022 06:05:51 GMT+0000 (Coordinated Universal Time)
cuid: clkc6k7u100000al26ga4dpw6
slug: serverless-analytics-on-aws-redshift-serverless-and-kinesis-on-demand-part-2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689919234080/9c5f01c1-989e-401d-8c1f-2516f639bad7.webp
tags: aws, serverless, data-analytics

---

In Part 1 of our blog series on Serverless Analytics on AWS, we explored the benefits of serverless analytics and delved into Amazon EMR Serverless and Amazon MSK Serverless. In Part 2, we will continue our journey into the world of serverless analytics and focus on two more powerful services offered by AWS: Amazon Redshift Serverless and Amazon Kinesis on-demand.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e8srdik9dxl2dieqtqbw.png align="left")

## Introduction to Redshift Serverless and Kinesis on-demand

Amazon Redshift is a fully managed data warehousing service that allows organizations to run complex analytics queries and derive insights from their data. Redshift Serverless extends this functionality by enabling users to run analytics queries on-demand without the need to manage traditional Redshift clusters. Amazon Kinesis, on the other hand, is a suite of services for real-time data streaming and processing, and Kinesis on-demand takes this a step further by providing automatic scaling and resource management for Kinesis data streams.

## Getting Started with Amazon Redshift Serverless

Amazon Redshift Serverless provides a cost-effective and flexible option for running analytics queries without the need for managing clusters. It automatically scales compute resources based on query demands, ensuring optimal performance while minimizing costs during idle periods.

**Key Features of Amazon Redshift Serverless:**

* **Automatic Scaling:** Redshift Serverless automatically scales the number of compute nodes based on query requirements, enabling high-performance analytics without manual intervention.
    
* **Pause and Resume:** The ability to pause and resume Redshift clusters allows users to save costs during periods of inactivity, making it an ideal choice for intermittent workloads.
    
* **Data Lake Integration:** Redshift Serverless seamlessly integrates with Amazon S3, enabling users to analyze data directly from S3 without the need to load it into Redshift first.
    
* **Concurrent Queries:** Redshift Serverless supports concurrent queries, allowing multiple users to run analytics simultaneously without compromising performance.
    

## How Amazon Redshift Serverless Works

When a query is submitted to Amazon Redshift Serverless, the service automatically provisions the required compute nodes to process the query. Once the query is completed, the compute nodes are automatically released, and users are only billed for the compute resources used during query execution.

The pause and resume functionality allows users to put their clusters in a dormant state during periods of inactivity, significantly reducing costs.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jm5tkxemeo7awvk4l3vu.png align="left")

## How Amazon Redshift Serverless Differs from Traditional Redshift

In traditional Redshift, users need to provision and manage clusters, which involves upfront planning and fixed resource allocation. This approach may lead to over-provisioning during idle periods, resulting in unnecessary costs.

Amazon Redshift Serverless eliminates the need for cluster management and provides automatic scaling based on query demands, making it a cost-effective and user-friendly option.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/13s17f4drw8as5zzo4c4.png align="left")

## Getting Started with Amazon Kinesis on-demand

Amazon Kinesis on-demand offers a serverless and fully managed solution for real-time data streaming and processing. It automatically handles the provisioning and scaling of resources to accommodate fluctuating data volumes, making it ideal for event-driven or dynamic workloads.

**Key Features of Amazon Kinesis on-demand:**

* **Automatic Scaling:** Kinesis on-demand automatically adjusts the number of shards to handle varying data ingestion rates, ensuring smooth data processing without manual intervention.
    
* **Pay-as-You-Go Pricing:** Users only pay for the resources used during data ingestion and processing, making it a cost-efficient choice for unpredictable workloads.
    
* **Integration with AWS Services:** Kinesis on-demand seamlessly integrates with various AWS services like Lambda, Firehose, and DynamoDB, enabling users to build real-time data processing pipelines effortlessly.
    
* **Enhanced Resilience:** With Kinesis on-demand, data streams are automatically replicated across multiple availability zones for enhanced resilience and durability.
    

## How Amazon Kinesis on-demand Works

When data is ingested into Amazon Kinesis on-demand, the service automatically creates and scales shards to accommodate the incoming data rate. As the data rate fluctuates, the number of shards scales up or down accordingly, ensuring efficient resource utilization and optimal performance.

Users are billed based on the number of shards used during data ingestion and processing.

## How Amazon Kinesis on-demand Differs from Traditional Kinesis

In traditional Kinesis, users need to provision and manage the number of shards in their data streams. This approach requires careful planning and may lead to either over-provisioning or under-provisioning of resources.

Amazon Kinesis on-demand eliminates the need for manual shard management and automatically handles scaling, making it more suitable for event-driven workloads and unpredictable data ingestion rates.

## Key Considerations

As you consider Amazon Redshift Serverless and Amazon Kinesis on-demand for your serverless analytics needs, keep the following key points in mind:

**1\. Data Volume and Workload Patterns:** Analyze your data volume and workload patterns to determine the most suitable serverless option. Each service has its strengths, so choosing the right one will depend on your specific use case.

**2\. Integration with Other AWS Services:** Evaluate how well the serverless analytics service integrates with your existing AWS architecture and other services you might be using for data storage, processing, and visualization.

**3\. Cost Optimization:** While serverless options can provide cost savings, it's essential to continuously monitor and optimize your usage to ensure you get the best value for your analytics workloads.

## Conclusion

Amazon Redshift Serverless and Amazon Kinesis on-demand offer powerful and efficient solutions for serverless analytics on AWS. By leveraging these services, organizations can process and analyze data without the burden of managing infrastructure, benefiting from automatic scaling and cost-effectiveness.

In this two-part blog series, we explored the world of serverless analytics on AWS and the benefits of using serverless options for processing and analyzing data. By adopting serverless analytics, organizations can focus on extracting valuable insights from their data, driving innovation, and making data-driven decisions to stay competitive in today's data-centric world.

And if you haven't yet, make sure to follow me on below handles:

👋 connect with me on [LinkedIn](https://www.linkedin.com/in/adit-n-modi-356606261/) 🤓 connect with me on [Twitter](https://twitter.com/adi_12_modi)🐱‍💻 follow me on [github](https://github.com/AditModi) ✍️ Do Checkout [my blogs](https://aditmodi.com)

Like, share, and follow me 🚀 to stay updated with the latest content and to join a vibrant community of tech enthusiasts. Your support is greatly appreciated!
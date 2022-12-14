# Patterns for Ingesting SaaS Data into AWS Data Lakes | AWS White Paper Summary

Today, many organizations generate and store data in [Software as a service](https://en.wikipedia.org/wiki/Software_as_a_service) (SaaS)-based applications. They want to ingest this data in a central repository (often a data lake) so that they can analyze it though a single pane of glass and derive insights from it. However, because different SaaS applications provide different mechanisms of extracting the data, a single approach does not always work.

This paper outlines different patterns using Amazon Web Services (AWS) services to ingest SaaS data into a data lake on AWS.

## **Are you Well-Architected?**

The [AWS Well-Architected Framework](http://aws.amazon.com/architecture/well-architected/) helps you understand the pros and cons of the decisions you make when building systems on AWS. Using the Framework allows you to learn architectural best practices for designing and operating reliable, secure, efficient, and cost-effective systems in the cloud.

In the [SaaS Lens](https://docs.aws.amazon.com/wellarchitected/latest/saas-lens/saas-lens.html), we enable customers to review and improve their cloud-based architectures and better understand the business impact of their design decisions. We address general design principles as well as specific best practices and guidance in five conceptual areas that we define as the pillars of the Well-Architected Framework.

## **Introduction**

With the growing ecosystem of SaaS-based applications, many organizations face the challenge of collecting all this data into a centralized location for analytics. Often this centralized location is a data lake, and in AWS Cloud, [Amazon Simple Storage Service](http://aws.amazon.com/s3/) (Amazon S3) becomes the centralized storage for this data lake. However, an ingestion mechanism must be in place get all the SaaS data into Amazon S3. This paper explores different options to use in AWS, along with usage patterns and considerations to make when selecting a particular ingestion mechanism.

# Data ingestion mechanisms

In this section, we'll review the following data ingestion mechanisms: Purpose-built integration service, ETL using custom connectors with Apache Spark, data federation using SQL engine, streaming data, and import/export using files.

## Purpose-built integration service

### **Amazon AppFlow: Introduction**

[Amazon AppFlow](http://aws.amazon.com/appflow/) is a fully-managed integration service that enables you to securely transfer data between SaaS applications (such as Salesforce, Marketo, Slack, and ServiceNow) and AWS services (such as Amazon S3 and [Amazon Redshift](http://aws.amazon.com/redshift)). With Amazon AppFlow, you can run data flows at nearly any scale and frequency (on a schedule, in response to a business event in real time, or on demand). You can configure data transformations such as data masking and concatenation of fields, as well as validate and filter data (omitting records that don’t fit a criteria) to generate rich, ready-to-use data as part of the flow itself, without additional steps.

Amazon AppFlow automatically encrypts data in motion, and optionally allows you to restrict data from flowing over the public internet for SaaS applications that are integrated with [AWS PrivateLink](http://aws.amazon.com/privatelink/), reducing exposure to security threats. For a complete list of all the SaaS applications that can be integrated with Amazon AppFlow, refer to [Amazon AppFlow integrations](http://aws.amazon.com/appflow/integrations/).

#### **Architecture overview**

The following diagram depicts the architecture of the solution where data from Salesforce is ingested into Amazon S3 using Amazon AppFlow. Once the data is ingested in Amazon S3, you can use an [AWS Glue crawler](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html) to populate the [AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/components-overview.html#data-catalog-intro) with tables and start consuming this data using SQL in [Amazon Athena](http://aws.amazon.com/athena).

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bdxizrffbh07qrbj1uk8.png)
*Amazon AppFlow-based data ingestion pattern* 

## Extract, transform and load (ETL) using custom connectors with Apache Spark

## **AWS Glue: Introduction**

[AWS Glue](http://aws.amazon.com/glue/) is a serverless data integration service that makes it easy to discover, prepare, and combine data for analytics, machine learning, and application development. AWS Glue provides all the capabilities needed for data integration so that you can start analyzing your data and putting it to use in minutes instead of months.

[AWS Glue custom connectors](http://aws.amazon.com/about-aws/whats-new/2020/12/aws-glue-launches-aws-glue-custom-connectors/) make it easy to discover and integrate with a variety of additional data sources, such as SaaS applications or your custom data sources. With just a few clicks, you can search and select connectors from the [AWS Marketplace](http://aws.amazon.com/marketplace/) and begin your data preparation workflow in minutes. AWS is also releasing a new framework to develop, validate, and deploy your own custom connectors (bring your own connectors (BYOC)).

### **Architecture overview**

The following diagram depicts the architecture of the solution where data from any of the SaaS applications can be ingested into Amazon S3 using AWS Glue. Once the data is ingested in Amazon S3, you can catalog it using AWS AWS Glue Data Catalog, and start consuming this data using SQL in Amazon Athena.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k98ky2flnuwzikwe8wxg.png)
*AWS Glue-based data ingestion pattern* 

## Data federation using SQL engine

## **Amazon Athena: Introduction**

[Amazon Athena](http://aws.amazon.com/athena) is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. Athena is serverless, so there is no infrastructure to manage, and you pay only for the queries that you run.

[Amazon Athena Federated Query](https://docs.aws.amazon.com/athena/latest/ug/connect-to-a-data-source.html) is a new Athena feature that enables data analysts, engineers, and data scientists to run SQL queries across data stored in relational, non-relational, object, and custom data sources. With Amazon Athena Federated Query, customers can submit a single SQL query and analyze data from multiple sources running on-premises or hosted on the cloud. Athena runs federated queries using Data Source Connectors that run on [AWS Lambda](http://aws.amazon.com/lambda).

Customers can use these connectors to run federated SQL queries in Athena across multiple data sources, including SaaS applications. Additionally, using Query Federation SDK, customers can build connectors to any proprietary data source, and enable Athena to run SQL queries against the data source. Because connectors run on Lambda, customers continue to benefit from Athena’s serverless architecture and do not have to manage infrastructure or scale for peak demands.

### **Architecture overview**

Athena Federated query connector allows Athena to connect to SaaS applications like Salesforce, Snowflake, and Google BigQuery. Once a connection is established, you can write SQL queries to retrieve data stored in these SaaS applications. To store data in Amazon S3 data lake, you can use Athena statements [Create Table as Select (CTAS) and INSERT INTO for ETL](https://docs.aws.amazon.com/athena/latest/ug/ctas-insert-into-etl.html), which then store the data in Amazon S3 and create a table in the AWS AWS Glue Data Catalog.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rfqtg28g0x03w01obzl0.png)
*Amazon Athena-based data ingestion pattern* 

## Streaming data

## **Amazon Managed Streaming for Apache Kafka and Amazon Kinesis: Introduction**

[Amazon Managed Streaming for Apache Kafka](http://aws.amazon.com/msk/) (Amazon MSK) makes it easy to ingest and process streaming data in real time with fully-managed Apache Kafka.

[Amazon Kinesis Data Firehose](http://aws.amazon.com/kinesis/data-firehose/) is an ETL service that reliably captures, transforms, and delivers streaming data to data lakes, data stores, and analytics services.

### **Architecture overview**

Often, use cases require the source data to be to immediately made available in the data lake for analytics. Getting the data from the SaaS application in near-real-time lays the foundation for an event-driven architecture. Start with how Apache Kafka helps with this architecture pattern: Apache Kafka has a tool called [Kafka Connect](https://kafka.apache.org/documentation/#connect), which allows for scalable and reliable streaming data between Apache Kafka and other systems. Many SaaS applications allow for events data to be published to Kafka topics using Kafka Connect.

Amazon MSK eliminates the operational overhead, including the provisioning, configuration, and maintenance of highly-available Apache Kafka and Kafka Connect clusters. With [Amazon MSK Connect](http://aws.amazon.com/msk/features/msk-connect/), a feature of Amazon MSK, you can run fully-managed Apache Kafka Connect workloads on AWS. This feature makes it easy to deploy, monitor, and automatically scale connectors that move data between Apache Kafka clusters and external systems. Amazon MSK Connect is fully-compatible with Kafka Connect, enabling you to lift and shift your Kafka Connect applications with zero code changes. With Amazon MSK Connect, you only pay for connectors you are running, without the need for cluster infrastructure.

Once the data is streamed in Kafka, you can either directly send it to Amazon S3 or you can use the [Kinesis-Kafka Connector](https://github.com/awslabs/kinesis-kafka-connector) to extend the pipeline before the data is pushed into Amazon S3. The advantage of using Kinesis Data Firehose in the streaming data pipeline is that you can now transform/normalize the events data, buffer it for a specific interval or a specific file size, and finally convert this data into Parquet file format before it lands in Amazon S3. You can also partition the data in Amazon S3 prefixes based on rules using Kinesis Data Firehose.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/05w67adpkskno31bffrc.png)
*Streaming based-data ingestion pattern* 

## Import/Export using files

Many SaaS applications allow data to be exported into files. You can use any of the file transfer mechanisms listed below to move such files into Amazon S3 for further processing:

[AWS DataSync](http://aws.amazon.com/datasync/) is a secure, online service that automates and accelerates the process of moving data between on premises and AWS storage services. DataSync can copy data between Network File System (NFS) shares, Server Message Block (SMB) shares, Hadoop Distributed File Systems (HDFS), self-managed object storage, AWS Snowcone, Amazon S3 buckets, Amazon Elastic File System (Amazon EFS) file systems, Amazon FSx for Windows File Server file systems, and Amazon FSx for Lustre file systems.

[AWS Transfer Family](http://aws.amazon.com/aws-transfer-family/) securely scales your recurring business-to-business file transfers to Amazon S3 and Amazon EFS using SFTP, FTPS, and FTP protocols.

You can also write your own scripts using [AWS Command Line Interface (CLI)](http://aws.amazon.com/cli/) or [AWS SDK](http://aws.amazon.com/tools/) to transfer the exported files over to Amazon S3 at a regular interval.

# Conclusion

In the ever-growing world of SaaS-based applications, the data produced and stored by these applications will continue to grow exponentially in the future. This paper looked at some of the mechanisms with which you can ingest this data into Amazon S3, and build a central data lake. Based on your use case, conduct a proof of concept with the best fitting mechanism listed in this paper, to make sure all the edge cases can be met and you get a cost effective, agile, and scalable system.

# Reference

[Original paper](https://docs.aws.amazon.com/whitepapers/latest/patterns-for-ingesting-saas-data-into-aws-data-lakes/patterns-for-ingesting-saas-data-into-aws-data-lakes.html)

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
---
title: "AWS Streaming Data Solution for Amazon Kinesis"
seoTitle: "AWS Streaming Data Solution for Amazon Kinesis"
seoDescription: "AWS Streaming Data Solution for Amazon Kinesis"
datePublished: Thu May 04 2023 06:17:39 GMT+0000 (Coordinated Universal Time)
cuid: clh8qjlem024i0anv8nbk6s3u
slug: aws-streaming-data-solution-for-amazon-kinesis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1659181902118/_Rfzvnp7P.png
tags: aws, architecture, kinesis, data-streaming

---

➡️ Option 1: AWS CloudFormation template using Amazon API Gateway, Amazon Kinesis Data Streams, and AWS Lambda. Deploy a solution that automatically configures the AWS services necessary to easily capture, store, process, and deliver streaming data. 

![2022-07-30 (31).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659187256025/r93A84Etg.png align="left")

1) An Amazon API Gateway REST API that acts as a proxy to Amazon Kinesis Data
Streams, adding either an individual data record or a list of data records.

2) An Amazon Cognito user pool is used to control who can invoke REST API
methods.

3) Kinesis Data Streams to store the incoming streaming data.

4) An AWS Lambda function processes the records from the data stream.

5) Errors and failed records that occur during AWS Lambda processing are annotated, and
the events are stored in Amazon Simple Queue Service (Amazon SQS). The queue
stores metadata for failed batch records and Lambda errors, allowing customers to
retrieve these records and determine the next steps to resolve them.

More Details here: 👉

[Link](https://docs.aws.amazon.com/solutions/latest/aws-streaming-data-solution-for-amazon-kinesis/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-kinesis-sol.pdf) 

➡️ Option 2: AWS CloudFormation template using Amazon EC2, Amazon Kinesis Producer Library, Amazon Kinesis Data Streams, Amazon Kinesis Data Analytics, and Amazon CloudWatch. Deploy a solution that automatically configures the AWS services necessary to easily capture, store, process, and deliver streaming data. 

![2022-07-30 (32).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659187266389/g9pHUrYep.png align="left")

1) An Amazon Elastic Compute Cloud (Amazon EC2) instance that uses the
Amazon Kinesis Producer Library (KPL) to generate data.

2) Kinesis Data Streams to store the incoming streaming data.

3) Kinesis Data Analytics processes the incoming records and saves the processed
data in an Amazon Simple Storage Service (Amazon S3) bucket.

4) An Amazon CloudWatch dashboard monitors application health, progress,
resource utilization, events, and errors.
More Details here: 👉

[Link](https://docs.aws.amazon.com/solutions/latest/aws-streaming-data-solution-for-amazon-kinesis/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-kinesis-sol.pdf) 

➡️ Option 3: AWS CloudFormation template using Amazon Kinesis Data Streams, Amazon Kinesis Data Firehose, and Amazon S3. Deploy a solution that automatically configures the AWS services necessary to easily capture, store, process, and deliver streaming data.

![2022-07-30 (33).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659187272430/_oiwZOgv4.png align="left")

1) Kinesis Data Streams stores the incoming streaming data.

2) Kinesis Data Firehose buffers the data before delivering the output to an Amazon S3 bucket. It is a fully managed service that automatically scales to match the throughput of your data and requires no ongoing administration.
3) An Amazon CloudWatch dashboard monitors the data ingestion and buffering. CloudWatch alarms are set on essential metrics for Kinesis Data Firehose.

More Details here: 👉

[Link](https://docs.aws.amazon.com/solutions/latest/aws-streaming-data-solution-for-amazon-kinesis/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-kinesis-sol.pdf)

➡️ Option 4: AWS CloudFormation template using Amazon Kinesis Data Streams, Amazon Kinesis Data Analytics, and Amazon API Gateway. Deploy a solution that automatically configures the AWS services necessary to easily capture, store, process, and deliver streaming data.

![2022-07-30 (34).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659187281130/HPbcgZNrb.png align="left")

1) An Amazon Elastic Compute Cloud (Amazon EC2) instance that uses the
Amazon Kinesis Producer Library (KPL) to generate data.

2) Kinesis Data Streams stores the incoming streaming data.
3)  Kinesis Data Analytics processes the incoming records and asynchronously
invokes an external endpoint.

4) The demo application invokes an AWS Lambda function.

5) The external API can be any integration supported by Amazon API Gateway (for
example, an Amazon SageMaker endpoint).

6) An Amazon CloudWatch dashboard monitors application health, progress,
resource utilization, events, and errors.

More Details here: 👉

[Link](https://docs.aws.amazon.com/solutions/latest/aws-streaming-data-solution-for-amazon-kinesis/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-kinesis-sol.pdf) 

You can download the Implementation Guide from this link 👀👇
[Link](https://d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-kinesis-sol.pdf?did=wp_card&trk=wp_card) 

I hope you will find it useful for your organization! 🤓

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
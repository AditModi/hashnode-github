---
title: "Multi-Model Inference Workflow Orchestration"
seoTitle: "Multi-Model Inference Workflow Orchestra"
seoDescription: "Multi-Model Inference Workflow Orchestra"
datePublished: Fri Jun 02 2023 06:18:42 GMT+0000 (Coordinated Universal Time)
cuid: clie6cn8d062d4fnv860ydyo8
slug: multi-model-inference-workflow-orchestration
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1659182580148/b5ewVxqjm.png
tags: machine-learning

---

➡️ This architecture enables customers to monitor streaming data and compare it in near-real-time to a machinelearned forecast, raising an incident or alarm if actual performance deviates significantly from the forecast.


![2022-07-30 (22).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659186648357/oTeFN5Hmb.png align="left")

1) Data is collected from multiple data sources across the enterprise and the edge using Amazon Kinesis Data Streams’ many SDKs with support for languages like Java, .NET, C++, python, Javascript, and others.

2) Data persists and is sent to Amazon Simple Storage Service (Amazon S3) by Amazon Kinesis Data Firehouse. AWS Lambda can be used to enrich data prior to storage in Amazon S3.

3) Initial data preparation and aggregation is performed using Amazon Athena. Prepared
and aggregated data is stored in Amazon S3.

4) Amazon SageMaker is used to train a forecasting model and create predictions of future behavior. These can be predictions for either statistical descriptions (for example sample counts and standard deviations) or business-oriented aggregations (for example transaction values). The predictions are stored in Amazon S3.

5) As new data arrives, it is aggregated and prepared in near-real-time by Amazon Kinesis Data Analytics. The resulting prepared data is compared to the previously generated forecast.

6) Using another AWS Lambda function, the forecast and actual values are written as
metrics to Amazon CloudWatch.

7) When actual values deviate significantly from the forecast, a CloudWatch alarm triggers an incident in AWS Systems Manager Incident Manager to trigger an investigation or remediation.

You can download the Implementation Guide from this link 👀👇
[Link](https://d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/multi-model-inference-workflow-orchestration-ra.pdf?did=wp_card&trk=wp_card) 

I hope you will find it useful for your organization! 🤓

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
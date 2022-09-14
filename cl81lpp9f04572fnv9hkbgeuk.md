## Data Analytics Lens AWS Well-Architected Framework | AWS White Paper Summary

# Introduction

* This document describes the AWS Well-Architected Data Analytics Lens, a collection of customer-proven best practices for designing well-architected analytics workloads. 

* The Data Analytics Lens contains insights that AWS has gathered from real-world case studies, and helps you learn the key design elements of well-architected analytics workloads along with recommendations for improvement. 

* The document is intended for IT architects, developers, and team members who build and operate analytics systems.

## How to use the lens

* The AWS Well-Architected Framework helps you understand the pros and cons of decisions you make while building systems on AWS.

* By using the framework, you learn architectural best practices for designing and operating reliable, secure, efficient, and cost-effective systems in the cloud. It provides a way for you to consistently measure your architectures against best practices and identify areas for improvement. We believe that having well-architected systems greatly increases the likelihood of business success.

* In this “Lens” we focus on how to design, deploy, and architect your analytics application workloads in the AWS Cloud. For brevity, we have only covered details from the Well-Architected Framework that are specific to analytics workloads. 

* You should still consider best practices and questions that have not been included in this document when designing your architecture. We recommend that you read the AWS Well-Architected Framework.

* This document is intended for those in technology roles, such as chief technology officers (CTOs), architects, developers, and operations team members. 

* After reading this document, you will understand AWS best practices and strategies to use when designing architectures for analytics applications and environment.

# Workload context checklist

☐ C1	Required	Name of the workload
☐ C2	Required	Description that contains the business purposes, key performance indicators (KPIs), and the intended users of the workload
☐ C3	Required	Review owner who leads the lens review
☐ C4	Required	Workload owner who is responsible for maintaining the workload
☐ C5	Required	Business stakeholders who sponsor the workload
☐ C6	Required	Business partners who have a stake in the workload, such as information security, finance, and legal
☐ C7	Recommended	Architecture design document that describes the workload
☐ C8	Recommended	AWS Account IDs associated with the workload
☐ C9	Recommended	Regulatory compliance requirements relevant to the workload (if any)

# Well-Architected design principles

* This section describes the design principles, best practices, and improvement suggestions that are relevant when designing your workload architecture. For brevity, only questions that are specific to analytics workloads are included in the Analytics Lens. 

* We recommend you also read and apply the guidance found in each Well-Architected pillar, which includes foundational best practices for operational excellence, security, performance efficiency, reliability, and cost optimization that are relevant to all workloads.

## Pillars

### Operational excellence

* The Operational Excellence pillar includes the ability to support development and run workloads effectively, gain insight into their operations, and to continually improve supporting processes and procedures to deliver business value.

### Security

* The security pillar encompasses the ability to protect data, systems, and assets to take advantage of cloud technologies to improve your security.

### Reliability

* The reliability pillar includes the ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfiguration or transient network issues. It also includes the ability to understand the full travel path and change history of the data and keep the data safe when storage failure occurs.

### Performance efficiency

* The performance efficiency pillar focuses on the efficient use of computing resources to meet requirements and the maintenance of that efficiency as demand changes and technologies evolve. Remember, performance optimization is not a one-time activity. 

* It is an incremental and continual process of confirming business requirements, measuring the workload performance, identifying under-performing components, and tuning the components to meet your business needs.

### Cost optimization

* The cost optimization pillar includes the continual process of refinement and improvement of a system over its entire lifecycle to optimize cost, from the initial design of your first proof of concept to the ongoing operation of production workloads. 

* It’s a years-long, continual process. Choose the right solution and pricing model. Build cost-aware systems that allow you to achieve business outcomes and minimize costs. To perform cost optimization over time, you need to identify data, infrastructure resources, and analytics jobs that can be removed or downsized.

* Determine the analytics workflow costs at each individual data processing step or individual pipeline branch. The benefit of understanding analytics workflow costs at this granular level will help you decide where to focus engineering resources for development, and perform return on investment (ROI) calculations for the analytics portfolio as a whole.

# Scenarios

* In this section, we cover the six key scenarios that are common in many analytics applications and how they influence the design and architecture of your analytics environment in AWS. 

* We present the assumptions made for each of these scenarios, the common drivers for the design, and a reference architecture for how these scenarios should be implemented.

## Modern data architecture (formerly Lake House)

* A lake house is a modern data architecture that integrates a data lake, a data warehouse, and other purpose-built data stores while enabling unified governance and seamless data movement.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tz03klfbfmnuf47e5ycw.png)
*Modern data architecture reference architecture* 

## Data mesh

* At AWS, we have been talking about the data-driven organization model for years, which consists of data producers and consumers. This model is similar to those used by some of our customers and has been described by Zhamak Dehghani of Thoughtworks, who coined the term data mesh

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xm90ocsinlbavras58j5.png)
*data mesh reference architecture*

## Batch data processing

* Most analytics applications require frequent batch processing, which allows them to process data in batches at varying interval. 

* For example, processing daily sales aggregations by individual store and then writing that data to the data warehouse on a nightly basis can allow business intelligence (BI) reporting queries to run faster. 

* Batch systems must be built to scale for all sizes of data and scale seamlessly to the size of the dataset being processed at various job runs.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hcawzgn90b5pi1tucw2d.png)
*batch data processing reference architecture*

## Streaming ingest and stream processing

* Processing real-time streaming data requires throughput scalability, reliability, high availability, and low latency to support a variety of applications and workloads. 

* Some examples include: streaming ETL, real-time analytics, fraud detection, API microservices integration, fraud detection activity tracking, real-time inventory and recommendations, and click-stream, log file, and IoT device analysis.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2e5hvrxmqs3pf4xnfza9.png)
*streaming ingest and stream processing reference architecture*

## Operational analytics

* Operational analytics refers to inter-disciplinary techniques and methodologies that aim to improve day-to-day business performance in terms of increasing efficiency of internal business processes and improving customer experience and value. 

* Business analysts used the traditional analytics systems to recognize the business trends and identify the decisions. But by using the operational analytics systems, they can initiate such business actions based on the recommendations that the systems provide. They can also automate the execution processes to reduce the human errors. This makes the system go beyond being descriptive to being more prescriptive and even being predictive in nature.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/p1wjhu8ydlj9z3m8dowj.png)
*operational analytics reference architecture*

## Data visualization

* Every day, the people in your organization make decisions that affect your business. When they have the right information at the right time, they can make the choices that move your company in the right direction. 

* Giving the decision-makers the opportunity to explore and interpret information in an interactive visual environment helps democratize data and accelerates data-driven insights that are easy to understand and navigate

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6d2b0j56vrf3ce1vai9h.png)
*data visualization reference architecture*

# Conclusion

* This lens provides architectural guidance for designing and building reliable, secure, efficient, and cost-effective analytics workloads in the cloud. We captured common architectures and overarching analytics design tenets. 

* The document also discussed the well-architected pillars through the Analytics Lens, providing you with a set of questions to consider for new or existing analytics architectures. 

* Applying the framework to your architecture helps you build robust, stable, and efficient systems, leaving you to focus on running analytics workloads and pushing the boundaries of the field to which you’re committed. 

* The analytics landscape is continuing to evolve as the number of tools and processes grows and matures. As this evolution occurs, we will continue to update this paper to help you ensure that your analytics applications are well-architected.

# Reference

[Original paper](https://docs.aws.amazon.com/wellarchitected/latest/analytics-lens/analytics-lens.html)
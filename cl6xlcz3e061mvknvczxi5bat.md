## AWS Cloud Data Ingestion
Patterns and Practices | AWS White Paper Summary

* Today, many organizations want to gain further insight using the vast amount of data they generate or have access to. They may want to perform reporting, analytics and/or machine learning on that data and further integrate the results with other applications across the organization. 

* More and more organizations have found it challenging to meet their needs with traditional on-premises data analytics solutions, and are looking at modernizing their data and analytics infrastructure by moving to cloud. 

* However, before they can start analyzing the data, they need to ingest this data into cloud and use the right tool for the right job. This paper outlines the patterns, practices, and tools used by AWS customers when ingesting data into AWS Cloud using AWS services.

# Introduction

* As companies are dealing with massive surge in data being generated, collected and stored to support their business needs, users are expecting faster access to data to make better decisions quickly as changes occur. Such agility requires that they integrate terabytes to petabytes and sometimes exabytes of data, along with the data that was previously siloed, in order to get a complete view of their customers and business operations.

* To analyze these vast amounts of data and to cater to their end user’s needs, many companies create solutions like data lakes and data warehouses. They also have purpose-built data stores to cater to specific business applications and use cases—for example, relational database systems to cater to transactional systems for structured data or technologies like Elasticsearch to perform log analytics and search operations.

* As customers use these data lakes and purpose-built stores, they often need to also move data between these systems. For example, moving data from the lake to purpose- built stores, from those stores to the lake, and between purpose-built stores.

* As data in these data lakes and purpose-built stores continues to grow, it becomes harder to move all this data around. We call this data gravity.

* Data movement is a critical part of any system. To design a data ingestion pipeline, it is important to understand the requirements of data ingestion and choose the appropriate approach which meets performance, latency, scale, security, and governance needs.

* This whitepaper provides the patterns, practices and tools to consider in order to arrive at the most appropriate approach for data ingestion needs, with a focus on ingesting data from outside AWS to the AWS Cloud. 

* This whitepaper is not a programming guide to handle data ingestion but is rather intended to be a guide for architects to understand the options available and provide guidance on what to consider when designing a data ingestion platform. It also guides you through various tools that are available to perform your data ingestion needs.

* The whitepaper is organized into several high-level sections which highlight the common patterns in the industry. For example, homogeneous data ingestion is a pattern where data is moved between similar data storage systems, like Microsoft SQL Server to Microsoft SQL Server or similar formats like Parquet to Parquet. This paper further breaks down different use cases for each pattern—for example, migration from on- premises system to AWS Cloud, scaling in the cloud for read-only workloads and reporting, or performing change data capture for continuous data ingestion into the analytics workflow.

# Data ingestion patterns

* Organizations often want to use the cloud to optimize their data analytics and data storage solutions. These optimizations can be in the form of reducing costs, reducing the undifferentiated heavy lifting of infrastructure provisioning and management, achieving better scale and/or performance, or using innovation in the cloud.

* Depending upon the current architecture and target Lake House architecture, there are certain common ingestion patterns that can be observed.

 * **Homogeneous data ingestion patterns:** These are patterns where the primary objective is to move the data into the destination in the same format or same storage engine as it is in the source. In these patterns, your primary objectives may be speed of data transfer, data protection (encryption in transit and at rest), preserving the data integrity and automating where continuous ingestion is required. These patterns usually fall under EL piece of extract, transform, load (ETL) and can be an intermediary step before transformations are done after the ingestion.

* This paper covers the following use cases for this pattern:

 * Relational data ingestion between same data engines (for example, Microsoft SQL Server to Amazon RDS for SQL Server or SQL Server on Amazon EC2, or Oracle to Amazon RDS for Oracle.) This use case can apply to migrating your peripheral workload into the AWS Cloud or for scaling your workload to expand on new requirements like reporting.

 * Data files ingestion from on-premises storage to an AWS Cloud data lake (for example, ingesting parquet files from Apache Hadoop to Amazon Simple Storage Service (Amazon S3) or ingesting CSV files from a file share to Amazon S3). This use case may be one time to migrate your big data solutions, or may apply to building a new data lake capability in the cloud.

 * Large objects (BLOB, photos, videos) ingestion into Amazon S3 object storage.

  * **Heterogeneous data ingestion patterns:** These are patterns where data must be transformed as it is ingested into the destination data storage system. These transformations can be simple like changing the data type/format of the data to meet the destination requirement or can be as complex as running machine learning to derive new attributes in the data. 

* This pattern is usually where data engineers and ETL developers spend most of their time to cleanse, standardize, format, and shape the data based on the business and technology requirements. As such, it follows a traditional ETL model. 

* In this pattern, you may be integrating data from multiple sources and may have a complex step of applying transformation. The primary objectives here are same as in homogeneous data ingestion, with the added objective of meeting the business and technology requirements to transform the data.

* This paper covers the following use cases for this pattern:

 * Relational data ingestion between different data engines (for example, Microsoft SQL Server to Amazon Aurora relational database or Oracle to Amazon RDS for MySQL).

 * Streaming data ingestion from data sources like Internet of Things (IoT) devices or log files to a central data lake or peripheral data storage.

 * Relational data source to non-relational data destination and vice versa (for example, Amazon DocumentDB solution to Amazon Redshift or MySQL to Amazon DynamoDB).
 
 * File format transformations while ingesting data files (for example, changing CSV format files on file share to Parquet on Amazon S3).

* The tools that can be used in each of the preceding patterns depend upon your use case. In many cases, the same tool can be used to meet multiple use cases. Ultimately, the decision on using the right tool for the right job will depend upon your overall requirements for data ingestion in the Lake House architecture. An important aspect of your tooling will also be workflow scheduling and automation.


# Conclusion

* With the massive amount of data growth, organizations are focusing on driving greater efficiency in their operations by making data driven decisions. As data is coming from various sources in different forms, organizations are tasked with how to integrate terabytes to petabytes and sometimes exabytes of data that were previously siloed in order to get a complete view of their customers and business operations. 

* Traditional on-premises data analytics solutions can’t handle this approach because they don’t scale well enough and are too expensive. As a result, customers are looking to modernize their data and analytics infrastructure by moving to the cloud.

* To analyze these vast amounts of data, many companies are moving all their data from various silos into a Lake House architecture. A cloud-based Lake House architecture on AWS allows customers to take advantages around scale, innovation, elasticity and agility to meet their data analytics and machine learning needs. 

* As such, ingesting data into the cloud becomes an important step of the overall architecture. This whitepaper addressed the various patterns, scenarios, use cases and the right tools for the right job that an organization should consider while ingesting data into AWS.

# Reference

[Original paper](https://d1.awsstatic.com/whitepapers/aws-cloud-data-ingestion-patterns-practices.pdf?did=wp_card&trk=wp_card)
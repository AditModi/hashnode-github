# Change Data Capture using AWS Database Migration Service


➡️ **Migrate and Replicate**

➡️ This architecture enables customers to migrate databases using on-going replication.


![2022-07-30 (35).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659186974594/7X3IMcAKh.png align="left")

1) Sources for change data capture (CDC) include Oracle, SQL Server, MySQL, PostgreSQL, MongoDB, Amazon Aurora, Amazon DocumentDB, and Amazon RDS.
2) The AWS Schema Conversion Tool makes heterogeneous database migrations predictable by automatically converting the source database schema and a majority of the database code objects to a format compatible with the target database.

3) AWS Database Migration Service helps you with one-time data migration of databases and continuous data replication. AWS Database Migration Service captures changes on the source
database and applies them in a transactionally consistent way to the target.
4) Targets for CDC include Oracle, SQL Server, MySQL, PostgreSQL, MongoDB, Amazon Aurora, Amazon DocumentDB, Amazon RDS, and Amazon S3.

➡️ **Streaming**
➡️ This architecture enables customers to ingest real-time changes from source databases through streaming services.


![2022-07-30 (36).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659186981198/wpPaahtHa.png align="left")

1) Sources for change data capture (CDC) include Oracle, SQL Server, MySQL, PostgreSQL, MongoDB, Amazon Aurora, Amazon DocumentDB, and Amazon RDS.

2) AWS Database Migration Service helps you with one-time data migration of databases and continuous data replication. AWS Database Migration Service captures changes on the source database and applies them in a transactionally consistent way to the target.

3) You can use AWS DMS to stream data to an Amazon Kinesis data stream and Amazon Managed Streaming for Apache Kafka (Amazon MSK) to collect and process large streams of data.

4) You can use purpose-built services for analytics in your data lake or data warehouse.

5) You can visualize and consume data using Amazon QuickSight, Kibana, and Amazon SageMaker Notebooks.

➡️ **Amazon S3 with Extract, Transform, Load (ETL) for Upsert**

➡️ This architecture enables customers to create data pipelines with change data capture to build transactional data lakes


![2022-07-30 (37).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659186988443/9xLmcSliL.png align="left")

1) Sources for change data capture (CDC) include Oracle, SQL Server, MySQL, PostgreSQL, MongoDB, Amazon Aurora, Amazon DocumentDB, and Amazon RDS.

2) AWS Database Migration Service helps you with one-time data migration of databases and continuous data replication. AWS Database Migration Service captures changes on the source
database and applies them in a transactionally consistent way to the target.

3) The target for change data capture is Amazon S3.

4) You can use AWS Glue, Amazon EMR for extract, transform, load (ETL) upsert to Amazon S3 and
Amazon Redshift.

You can download the Implementation Guide from this link 👀👇
[Link](https://d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/change-data-capture-using-aws-dms-ra.pdf?did=wp_card&trk=wp_card) 

I hope you will find it useful for your organization! 🤓

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
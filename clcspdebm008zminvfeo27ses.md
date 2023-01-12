# Best Practices for Migrating from RDBMS to Amazon DynamoDB | AWS White Paper Summary

* Software architects and developers have an array of choices for data storage and persistence. These include not only traditional relational database management systems (RDBMS), but also NoSQL databases, such as Amazon DynamoDB. 

* Certain workloads will scale better and be more cost-effective to run using a NoSQL solution. This whitepaper highlights the best practices for migrating these workloads from an RDBMS to DynamoDB. It also discusses how NoSQL databases like DynamoDB differ from a traditional RDBMS, and proposes a framework for analysis, data modeling, and migration of data from an RDBMS into DynamoDB.

## **Introduction**

* For decades, the RDBMS was the de facto choice for data storage and persistence. Any data driven application, be it an e-commerce website or an expense reporting system, was almost certain to use a relational database to retrieve and store the data required by the application. The reasons for this are numerous and include the following:

 * RDBMS is a mature and stable technology.
 * The query language, SQL, is feature-rich and versatile.
 * The servers that run an RDBMS engine are typically some of the most stable and powerful in the IT infrastructure.
 * All major programming languages contain support for the drivers used to communicate with an RDBMS, as well as a rich set of tools for simplifying the development of database-driven applications.

* These factors, and many others, have supported the wide adoption of the RDBMS. For architects and software developers, there simply wasn’t a reasonable alternative for data storage and persistence – until now.

* The growth of *internet scale* web applications, such as e-commerce and social media, the explosion of connected devices like smart phones and tablets, and the rise of big data have resulted in new workloads that traditional relational databases are not well suited to handle. As systems designed for transaction processing, all RDBMS must support certain fundamental properties.

* These properties are defined by the acronym ACID: Atomicity, Consistency, Isolation, and Durability. Atomicity refers to *all or nothing* operations – a transaction processes completely or not at all. Consistency means that the process of a transaction causes a valid state transition or the transaction is cancelled. Once the transaction is committed, the state of the resulting data must conform to the constraints imposed by the database schema. Isolation requires that concurrent transactions run separately from one another. 

* The isolation property guarantees that if concurrent transactions are run in serial, the end state of the data will be the same. Durability requires that the state of the data, once a transaction processes, be preserved. In the event of power or system failure, the database must be able to recover to the last known state.

* These ACID properties are all desirable, but support for all four requires an architecture that poses some challenges for today’s data intensive workloads. For example, consistency requires a well-defined schema and that all data stored in a database conform to that schema. This is great for ad-hoc queries and read-heavy workloads. 

* For a workload consisting almost entirely of writes, such as the saving of a player’s state in a gaming application, this enforcement of schema is expensive from a storage and compute standpoint. The game developer benefits little by forcing this data into rows and tables that relate to one another through a well-defined set of keys.

* Consistency also requires locking some portion of the data until the transaction modifying it completes and then making the change immediately visible. For a bank transaction, which debits one account and credits another, this is required. This type of transaction is called *strongly consistent*. 

* For a social media application, on the other hand, there really is no requirement that all users see an update to a data feed at precisely the same time. In this latter case, the transaction is *eventually consistent*. It is far more important that the social media application scale to handle potentially millions of simultaneous users even if those users see changes to the data at different times. Scaling an RDBMS to handle this level of concurrency, while maintaining strong consistency, requires upgrading to more powerful (and often proprietary) hardware. This is called *scaling up* or *vertical scaling*, it usually carries an extremely high cost and has an upper scalability limit. 

* The more cost-effective way to scale a database to support this level of concurrency is to add server instances running on commodity hardware. This is called *scaling out* or *horizontal scaling* and it is typically far more cost-effective than vertical scaling.

* NoSQL databases, such as Amazon DynamoDB, address the scaling and performance challenges found with RDBMS. The term *NoSQL* simply means that the database doesn’t follow the relational model espoused by E.F Codd in his 1970 paper *[A Relational Model of Data for Large Shared Data Banks](http://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf)*, which would become the basis for all modern RDBMS. As a result, NoSQL databases vary much more widely in features and functionality than a traditional RDBMS. There is no common query language analogous to SQL, and query flexibility is generally replaced by high I/O performance and horizontal scalability. NoSQL databases don’t enforce the notion of schema in the same way as an RDBMS. NoSQL databases may store semi-structured data, like JSON, they may store related values as column sets, or they may simply store key/value pairs.

* The net result is that NoSQL databases usually trade some of the query capabilities and ACID properties of an RDBMS for a much more flexible data model that scales horizontally. These characteristics make NoSQL databases an excellent choice in situations where use of an RDBMS (like the aforementioned game state example) is resulting in some combination of performance bottlenecks, operational complexity, and rising costs. DynamoDB offers solutions to all these problems, and is an excellent platform for migrating these workloads off of an RDBMS. 

* In addition, DynamoDB supports strong consistency and ACID transactions, so even workloads that require such capabilities, which traditionally were not considered suitable for NoSQL databases, can take advantage of DynamoDB’s scalability, flexible data model, and operational simplicity.

# Overview of Amazon DynamoDB

* [Amazon DynamoDB](http://aws.amazon.com/dynamodb/) is a fully managed, serverless, NoSQL database service running in the AWS cloud. The complexity of running a massively scalable, distributed NoSQL database is managed by the service itself, enabling software developers to focus on building applications rather than managing infrastructure. NoSQL databases are designed for scale, but their architectures are sophisticated, and there can be significant operational overhead in running a large NoSQL cluster. With DynamoDB, instead of having to become experts in advanced distributed computing concepts, the developer need only learn DynamoDB’s straightforward API using the software development kit (SDK) for the programming language of choice.

# Suitable workloads

With the introduction of DynamoDB features like [ACID transactions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transactions.html), [Amazon DynamoDB Accelerator](http://aws.amazon.com/dynamodb/dax/) (DAX), [global tables](http://aws.amazon.com/dynamodb/global-tables/), [Amazon DynamoDB Time to Live](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html) (TTL), and [Amazon DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html), the variety of workloads suitable for DynamoDB has expanded during the recent years. The following table outlines some of the more common use-cases for DynamoDB workloads.

|Application	|Use case|
| ------------- | ------ |
| Adtech	| Capturing browser cookie state User events, clickstream, impressions datastore| 
| Mobile applications	| Storing application data and session state| 
| Gaming applications	| Storing user preferences and application state, Storing players’ game states
| Consumer voting applications	| Reality TV contests, Superbowl commercials| 
| Large scale websites| 	Session state User data used for personalization Access control| 
| Application monitoring	| Storing application log and event data JSON data| 
| Internet of Things	| Sensor data and log ingestion| 
| Retail	| Shopping cart Customer profiles Inventory tracking and fulfillment| 
| Finance	| User transactions Event-driven transaction processing, fraud detection| 

# Unsuitable workloads

Some workloads are unsuitable for DynamoDB, including:

- Ad-hoc queries
- Online analytical processing (OLAP)
- Binary Large Object (Blob) storage

# Key concepts

As described in the previous section, DynamoDB organizes data into tables consisting of items. Each item in a DynamoDB table can define an arbitrary set of attributes, but all items in the table must define a primary key that uniquely identifies the item. This key must contain an attribute known as the partition key and optionally an attribute called the sort key. The following figure shows the structure of a DynamoDB table that defines both a partition and sort key.

# Migrating to DynamoDB from RDBMS

# Migrating to DynamoDB from RDBMS

**[PDF](https://docs.aws.amazon.com/whitepapers/latest/best-practices-for-migrating-from-rdbms-to-dynamodb/best-practices-for-migrating-from-rdbms-to-dynamodb.pdf#migrating-to-dynamodb-from-rdbms)[RSS](https://docs.aws.amazon.com/whitepapers/latest/best-practices-for-migrating-from-rdbms-to-dynamodb/best-practices-for-migrating-from-rdbms-to-dynamodb.rss)**

In the previous section, some of the key features of DynamoDB were discussed, as well as some of the key differences between DynamoDB and a traditional RDBMS. In this section, a strategy for migrating from an RDBMS to DynamoDB that takes into account these key features and differences is proposed. Because database migrations tend to be complex and risky, we advocate taking a phased, iterative approach. As is the case with the adoption of any new technology, it’s also good to focus on the easiest use cases first. It’s also important to remember, as we propose in this section, that migration to DynamoDB doesn’t need to be an all or nothing process. For certain migrations, it may be feasible to run the workload on both DynamoDB and the RDBMS in parallel, and switch over to DynamoDB only when it’s clear that the migration has succeeded and the application is working properly.

The following state diagram expresses the proposed migration strategy:

![https://docs.aws.amazon.com/whitepapers/latest/best-practices-for-migrating-from-rdbms-to-dynamodb/images/migration-phases.png](https://docs.aws.amazon.com/whitepapers/latest/best-practices-for-migrating-from-rdbms-to-dynamodb/images/migration-phases.png)

*Migration phases*

It is important to note that this process is iterative. The outcome of certain states can result in a return to a previous state. Oversights in the data analysis and data-modeling phase may not become apparent until the testing phase. In most cases, it will be necessary to iterate over these phases multiple times before reaching the final data migration state. Each phase is discussed in detail in the following sections.

# Planning phase

The first part of the planning phase is to identify the goals of the data migration. These often include, but are not limited to:

- Increasing application performance
- Lowering costs
- Reducing the load on an RDBMS
- Reducing operational overhead

In many cases, the goals of a migration may be a combination of all of the above. Once these goals have been defined, they can be used to inform the identification of the RDMBS tables to migrate to DynamoDB. Some good candidates for migration are:

- Entity-Attribute-Value tables
- Application session state tables
- User preference tables
- Logging tables

Once the tables have been identified, any characteristics of the source tables that may make migration challenging should be documented. This information will be essential for choosing a sound migration strategy. Let’s take a look at some of the more common challenges that tend to impact the migration strategy:

*Table 3 – Challenges that impact migration strategy*

|Challenge	|Impact on migration strategy|
| ------------- | -------------------------- |
|Writes to the RDBMS source table will continue before and during the migration.	|Consider a migration strategy that involves writing data to both the source and target tables in parallel in addition to bulk importing the existing data. As an alternative migration strategy, you can use the AWS Database Migration Service with ongoing replication to DynamoDB.|
|The amount of data in the source table is in excess of what can reasonably be transferred with the existing network bandwidth.	| Consider exporting the data from the source table to removable disks and using the AWS Import/Export or AWS Snowball services to import the data to a bucket in S3. Import this data into DynamoDB directly from S3. Alternatively, reduce the amount of data that needs to be migrated by exporting only those records that were created after a recent point in time. All data older than that point will remain in the legacy table in the RDBMS.|
|The data in the source table needs to be transformed before it can be imported into DynamoDB.	| Consider using one of the following strategies: Export the data from the source table and transfer it to S3. Consider using a Glue ETL job to perform the data transformation, and import the transformed data into DynamoDB. Use the AWS Database Migration Service with object mapping. Perform the transformation in an application code that will read data from the source database, transform as needed and write to DynamoDB.|
|The primary key structure of the source table is not portable to DynamoDB.	|Identify columns that will make suitable partition keys and sort keys for the imported items. Alternatively, consider adding a surrogate key, such as a universally unique identifier (UUID), to the source table that will act as a suitable partition key.|

Finally, and perhaps most importantly, the backup and recovery process should be defined and documented in the planning phase. If the migration strategy requires a full cutover from the RDBMS to DynamoDB, defining a process for restoring functionality using the RDBMS in the event the migration fails is essential. To mitigate risk, consider running the workload on DynamoDB and the RDBMS in parallel for some length of time. In this scenario, the legacy RDBMS-based application can be disabled only once the workload has been sufficiently tested in production using DynamoDB.

# Data analysis phase

The purpose of the data analysis phase is to understand the composition of the source data, and to identify the data access patterns used by the application. This information is required input for the data-modeling phase. It is also essential for understanding the cost and performance of running a workload on DynamoDB. The analysis of the source data should include:

- An estimate of the number of items to be imported into DynamoDB
- A distribution of the item sizes
- The multiplicity of values to be used as partition or sort keys

DynamoDB pricing contains two main components – read and write capacity and storage. By estimating the number of items that will be imported into a DynamoDB table, and the approximate size of each item, the storage and the capacity requirements for the table can be calculated. Common SQL data types will map to one of three scalar types in DynamoDB: string, number, or binary. The length of the number data type is variable, and strings are encoded using binary UTF-8. Focus should be placed on the attributes with the largest values when estimating item size, as capacity units are given in integral 1KB increments—there is no concept of a fractional capacity unit in DynamoDB. If an item is estimated to be 3.3KB in size, it will require four 1KB write capacity units and one 4KB read capacity unit to write and read a single item, respectively. Since the size will be rounded to the nearest kilobyte, the exact size of the numeric types is unimportant. In most cases, even for large numbers with high precision, the data will be stored using a small number of bytes. Because each item in a table may contain a variable number of attributes, it is useful to compute a distribution of item sizes and use a percentile value to estimate item size. For example, one may choose an item size representing the 95th percentile and use this for estimating the storage and capacity costs. In the event that there are too many rows in the source table to inspect individually, take samples of the source data and use these for computing the item size distribution.

Correctly estimating the required capacity is key to both guaranteeing the required application performance as well as understanding cost. Understanding the distribution frequency of RDBMS column values that could be partition or sort keys is essential for obtaining maximum performance as well. Columns containing values that are not uniformly distributed (that is, some values occur in much larger numbers than others) are not good partition or sort keys because accessing items with keys occurring in high frequency will hit the same DynamoDB partitions, and this has negative performance implications.

The second purpose of the data analysis phase is to categorize the data access patterns of the application. Because DynamoDB does not support joins and isn’t optimized for ad-hoc queries, it is essential to document the ways in which data will be written to and read from the tables. This information is critical for the data-modeling phase, in which the tables, the key structure, and the indexes will be defined. Some common patterns for data access are:

- Write only – items are written to a table and never read by the application.
- Fetches by distinct value – items are fetched individually by a value that uniquely identifies the item in the table.
- Queries across a range of items – items are fetched as groups using a value that identifies multiple items in the table, for example, items that belong to the same user ID.

As discussed in the next section, documentation of an application’s data access patterns using categories such as those described above will drive much of the data-modeling decisions.

# Data modeling phase

**[PDF](https://docs.aws.amazon.com/whitepapers/latest/best-practices-for-migrating-from-rdbms-to-dynamodb/best-practices-for-migrating-from-rdbms-to-dynamodb.pdf#data-modeling-phase)[RSS](https://docs.aws.amazon.com/whitepapers/latest/best-practices-for-migrating-from-rdbms-to-dynamodb/best-practices-for-migrating-from-rdbms-to-dynamodb.rss)**

In this phase, the tables, partition key, sort key, and secondary indexes are defined. The data model produced in this phase must support the data access patterns described in the data analysis phase. The first step in data modeling is to determine the partition key and sort key for a table. The primary key, whether consisting only of the partition key or a composite of the partition and sort key, must be unique for all items in the table. When migrating data from an RDBMS, it is tempting to use the primary key of the source table as the partition key, but in reality, this key is often meaningless to the application. For example, a user table in an RDBMS may define a numeric primary key, but an application responsible for logging in a user will ask for an email address, not the numeric user ID. In this case, the email address is the *natural key* and would be better suited as the partition key in the DynamoDB table, as items can easily be fetched by their partition key values. Modeling the partition key in this way is appropriate for data access patterns that fetch items by distinct value. For other data access patterns, like *write only*, using a randomly generated numeric ID will work well for the partition key. In this case, the items will never be fetched from the table by the application, and as such, the key will only be used to uniquely identify the items, not as a means of fetching data.

RDBMS tables that contain a unique index on two key values are good candidates for defining a primary key using both a partition key and a sort key. Intersection tables used to define many-to-many relationships in an RDBMS are typically modeled using a unique index on the key values of both sides of the relationship. Because fetching data in a many-to-many relationship requires a series of table joins, migrating such a table to DynamoDB would also involve denormalization of the data (discussed in more detail below).

Date values are also often used as sort key. A table counting the number of times a URL was visited on any given day could define the URL as the partition key and the date as the sort key. As with primary keys consisting solely of a partition key, fetching items with a composite primary key requires the application to specify both the partition and sort key values. This needs to be considered when evaluating whether a surrogate key or a natural key would make the better choice for the partition and or sort key.

Because non-key attributes can be added to an item arbitrarily, for tables with a simple primary key, the only attributes that must be specified in a DynamoDB definition are the partition key. For a table with a composite primary key, the partition key and the sort key must be specified. However, if secondary indexes are going to be defined on any non-key attributes, then these must be included in the table definition. Inclusion of non-key attributes in the table definition does not impose any sort of schema on all the items in the table. Aside from the primary key, each item in the table can have an arbitrary list of attributes.

The support for SQL in an RDBMS means that records can be fetched using any of the column values in the table. These queries may not always be efficient – if no index exists on the column used to fetch the data, a full table scan may be required to locate the matching rows. With DynamoDB it is possible to do a full table scan by using either the native query interface or PartiQL, but this is inefficient and will consume substantial read units if the table is large. Instead, items can be fetched from a DynamoDB table by the primary key of the table, or the key of a local or global secondary index defined on the table. Because an index in a non-key column of an RDBMS table suggests that the application commonly queries for data on this value, these attributes make good candidates for local or global secondary indexes in a DynamoDB table. There are [limits](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html) to the number of secondary indexes allowed on a DynamoDB table and, as previously explained, an index consumes additional storage and capacity for writes, so it is important to choose keys for these indexes using attribute values that the application will use most frequently for fetching data.

DynamoDB does not support the concept of a table join, so migrating data from an RDBMS table often requires denormalization of the data. To those used to working with an RDBMS, this will be a foreign and perhaps initially uncomfortable concept. For example, if a relational database contains a User and a UserAddress table, related through the UserID, one would combine the User attributes with the Address attributes into a single DynamoDB table. In the relational database, normalizing the UserAddress information allows for multiple addresses to be specified for a given user. This is a requirement for a contact management or CRM system. However, in DynamoDB, a user table would likely serve a different purpose—perhaps keeping track of a mobile application’s registered users. In this use-case, the multiplicity of users to addresses is less important than scalability and fast retrieval of user records.

It is recommended to use the [NOSQL Workbench for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.html) in order to perform data modeling for your DynamoDB tables. This client graphical user interface (GUI) application simplifies data modeling and visualization for DynamoDB.

# Testing Phase

The testing phase is the most important part of the migration strategy. It is during this phase that the entire migration process will be tested end-to-end. A comprehensive test plan should minimally contain the following:

*Table 9 – Data migration test plan*

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eusjj4qs1n6ez3evps0c.png)
 

Because the migration strategy is iterative, these tests are executed multiple times. For maximum efficiency, consider testing the data migration routines using a sampling from the production data if the total amount of data to migrate is large. The outcome of the testing phase often requires revisiting a previous phase in the process. The overall migration strategy becomes more refined through each iteration of the process. After all the tests execute successfully, it is time for the next and final phase: data migration.

# Data migration phase

In the data migration phase, the full set of production data from the source RDBMS tables is migrated into DynamoDB. By the time this phase is reached, the end-to-end data migration process has been tested and vetted thoroughly. All the steps of the process are carefully documented, so running it on the production data set should be as simple as following a procedure that has been executed numerous times before.

After the data migration is complete, the user acceptance tests defined in the previous phase should be executed one final time to ensure that the application is in a usable state. In the event that the migration fails for any reason, the backup and recovery procedure—which is also thoroughly tested and vetted at this point—can be initiated. When the system is back to a stable state, a root cause analysis of the failure should be conducted and the data migration rescheduled once the root cause is resolved. If all goes well, the application should be closely monitored over the next several days until there is sufficient data indicating that the application is functioning normally.

# Conclusion

Using DynamoDB for suitable workloads can result in lower costs, a reduction in operational overhead, and an increase in performance, availability, and reliability when compared to a traditional RDBMS. In this paper, AWS proposed a strategy for identifying and migrating suitable workloads from an RDBMS to DynamoDB. While implementing such a strategy will require careful planning and engineering effort, the return on investment (ROI) of migrating to a fully managed, serverless, NoSQL solution such as DynamoDB should greatly exceed the upfront cost associated with the effort.

# Reference

[Original paper](https://docs.aws.amazon.com/whitepapers/latest/best-practices-for-migrating-from-rdbms-to-dynamodb/welcome.html)

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
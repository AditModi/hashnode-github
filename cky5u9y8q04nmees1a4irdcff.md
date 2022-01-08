## Understanding what influences data lakes costs

* Across both real-time and batch processing, data flows through different stages. For each stage, there is an option to use managed services from AWS or to use compute, storage, or network services from AWS with third-party or open source software installed on top it.

* In the case of managed services, AWS provides service features like high availability, backups, and management of underlying infrastructure at additional cost. In some cases, the managed services are on-demand serverless based solutions, where the customer is charged only when the service is used.

* In case of third-party or open source software, the user pays AWS for infrastructure components being used and does the management and administration work themselves (in addition to license cost, if applicable).
 
# Ingestion

* Services required for ingestion depend on the source of data, the frequency at which the data should be consumed for processing to derive business value from it, and the data transfer rate.

## Cost factors

* Depending on the services you choose, the primary cost factors are:
 * Storage – These are the costs you pay for storing your raw data as it arrives.
 * Data transfer – These are the costs you pay for moving the data. Costs can be either bandwidth charges, leased line, or offline transfer. For example, Snowball. It is worth noting the analytics services should be deployed in the same Region to avoid unnecessary inter-Region data transfer. This improves performance and reduces costs.
 * Managed service costs – These are the costs you pay (usually per second or per hour) for the service you are using, if you chose a managed service from AWS (for example, AWS IoT or AWS Transfer for SFTP).

## Cost optimization factors

* We recommend that you consider the following actions to reduce cost:
 * Use data compression
 * Reduce run frequency
 * Choose a columnar format
 * Create data lifecycle policies
 * Choose serverless services

# Streaming

* This stage is only applicable for real-time processing. This stage is primarily responsible for ingesting the unbounded stream of data and providing guaranteed delivery for downstream processing.

## Cost factors

* The primary costs of this stage are
 
 * Data transfer – These are the costs you pay for the rate at which data is consumed by the data streaming service.
 * Streaming service costs – These are the costs you pay (usually per second or per hour) for AWS management of Amazon Kinesis or Amazon MSK service that is being used, including instance cost of Amazon MSK.
 * Storage cost – This is the cost of storing data in streaming service until data is consumed by its consumers and processed.

## Cost optimization factors

* We recommend that you consider the following actions to reduce cost:
 * Choose the right tool for the job
 * Choose serverless services
 * Use automatic scaling
 * Use Reserved Instances

# Processing and transformation

* The cost of processing depends on number of factors, such as the size of the data being processed, the complexity of the transformations (which leads to the choice of tool), and how the data is stored (format and compression).

## Cost factors

* The primary cost at this stage includes:
 * Processing unit cost – This is the cost of compute and memory required to process the data. If you use a service like Amazon EMR, the Amazon EC2 instance used to process the data will incur the cost.
 * Managed service costs – These are the costs you pay (usually per second or per hour) for the management of the service. For a serverless service like AWS Glue, this will be billed based on the utilization of the service.
 * Storage cost – This is the cost of storing the processed data.

## Cost optimization factors

* We recommend that you consider the following actions to reduce cost:
 
 * Use data compression
 * Partition data
 * Right size your compute
 * Choose serverless services
 * Use Spot Instances
 * Use Reserved Instances

# Analytics/Storage

* The business value of data lakes is derived using tools in analytics stage. Cost in this stage correlates directly to the amount of data collected and analytics done on the data to derive the required value.

## Cost factors

* The primary cost of this stage includes:
 
 * Processing Unit cost – This is the cost of instances used by the analytics tool being used. Data Warehouse solution like Amazon Redshift or analytics solutions on Amazon EMR or operational analytics on Amazon Elasticsearch are all billed based on the instance type and number of nodes used.
 * Storage cost – The data stored for analysis either on Amazon S3 or on the nodes of the analytics tool itself contribute to this cost.
 * Scanned data cost – This is applicable only for serverless service like Athena and Amazon Redshift Spectrum, where the cost is based on the data scanned by the analytics queries.

## Cost optimization factors

* We recommend that you consider the following actions to reduce cost:
 * Use data compression
 * Reduce run frequency
 * Partition data
 * Choose a columnar format 
 * Choose serverless services
 * Use Spot Instances
 * Use Reserved Instances
 * Right size your instances
 * Use automatic scaling

# Visualization/API access

* Visualization is used to graphically present the enormous amount of data in data lake in a human consumable format. API access refers to allowing developers to integrate your data in their apps to make it meaningful for consumers. The rate at which data is refreshed or request affects the number of queries being fired (and cost) in the analytics stage.

## Cost factors

* The primary cost of this stage includes:
 * Per user license cost – Most of SaaS visualization tools charge per user license cost.
 * Processing unit cost – The cost required to host the visualization software is applicable only for hosted visualization tools.
 * In-memory processing cost – This is the cost of the visualization need to load huge amount of data in memory for quick performance. The amount of data required to be loaded also has a direct impact on cost, for example, Super-fast, Parallel, In-memory Calculation Engine (SPICE) in Amazon QuickSight.
 * Number of requests through an API – As developers integrate with their apps the number of requests made for data will increase.

## Cost optimization factors

* We recommend that you consider the following actions to reduce cost:
 * Choose the right tool for the job
 * Choose serverless services
 * Use automatic scaling 
 * Use Reserved Instances
 * Right size your instances

# Metadata management, data catalog, and data governance

* In addition to the previous data lake stages, you need the following components to make use of the data effectively and securely.
 * Metadata management – Responsible for capturing technical metadata (data type, format, and schema), operational metadata (interrelationship, data origin, lineage) and business metadata (business objects and description)
 * Data catalog – An extension of metadata that includes features of data management and search capabilities
 * Data governance – Tools that manage the ownership and stewardship of data along with access control and management of data usage

## Cost factors

* The primary cost of this stage includes:
 * Processing cost – This is the cost associated with processing required to generate the catalog or metadata of data stored. In governance tools, this cost depends on number of governance rules that is being processed.
 * Storage cost – This is the cost associated with amount of metadata and catalog stored.
 * License cost – If there is any third party involved in providing these services, it will include the cost of that license.

## Cost optimization practices

* We recommend that you consider the following actions to reduce cost:
 * Choose the right tool for the job
 * Choose serverless services
 * Reduce run frequency
 * Partition data 
 * Choose a columnar format

# Cost optimization

* Across stages, the following are general practices to optimize cost of the services used.

## Use data compression

* There are many different formats for compression. You should be selective about which one you choose to make sure it is supported by the services you are using.

* The formats provide different features for example some formats support splitting meaning they support parallelization better, whilst others have better compression but at the cost of performance. Compressing the data, reduces the storage cost and also reduces the cost for scanning data.

* To give an example of how compression can help reduce costs we took a CSV dataset and compressed it using bzip2. This reduced the data from 8 GB to 2 GB a 75% reduction. If we extrapolate this compression ratio to a larger dataset, 100 TB the savings are significant:
 * Without compression: $2,406.40 per month
 * With compression: $614.40 per month This represents a saving of 75%.

## Reduce run frequency

* In this scenario, let’s assume we generate 1 TB of data daily. We could potentially stream this data, because it’s available, or choose to batch it and transfer it in one go. Let’s assume we have 1 Gbps bandwidth available for the transfer from our source system, for example on-premises systems. A single push of 1 TB daily would take around two hours. In this case we’ll allow three hours for some variance in speed.

* Also, in this scenario, we will use AWS SFTP to transfer the data. In the first scenario, we need the AWS SFTP service constantly running. In the second scenario, we only need it running for three hours a day to complete our transfer.
 
 * Running constant transfers as the data is available: $259.96 per month
 * Running a single batch transfer daily: $68.34 per month This represents a saving of just over 73%.

## Partition data

* Partitioning data allows you to reduce the amount of data that is scanned within queries. This helps both reduce cost and improve performance. To demonstrate this, we used the GDELT dataset. It contains around 300 million rows stored in many CSV files. We stored one copy un-partitioned and a second copy partitioned by year, month, day.

```shell
SELECT	count(*)	FROM	"gdeltv2"."non-partioned|partitioned"	WHERE
year	=	2019;
```

Unpartitioned:
 * Cost of query (102.9 GB scanned): $0.10 per query
 * Speed of query: 4 minutes 20 seconds Partitioned:
 * Cost of query (6.49 GB scanned): $0.006 per query
 * Speed of query: 11 seconds
This represents a saving of 94% and improved performance by 95%.

## Choose a columnar format

* Choosing a columnar format such as parquet, ORC or Avro helps reduce disk I/O requirements which can reduce costs and drastically improve performance in data lakes. To demonstrate this, we will again use GDELT. In both examples there is no partitioning but one dataset is stored as CSV and one as parquet.

 
 
CSV:
 * Cost of query (102.9 GB scanned): $0.10 per query
 * Speed of query: 4 minutes 20 seconds Partitioned:
 * Cost of query (1.04 GB scanned): $0.001 per query
 * Speed of query: 12.65 seconds
This represents a saving of 99% and improved performance by 95%.

## Create data lifecycle policies

* With Amazon S3, you can save significant costs by moving your data between storage tiers. In data lakes we generally keep a copy of the raw in-case we need to reprocess data as we find new questions to ask of our data. However, often this data isn’t required during general operations. 

* Once we have processed our raw data for the organization, we can choose to move this to a cheaper tier of storage. In this example we are going to move data from standard Amazon S3 to Amazon S3 Glacier.

* For our example, we will model these costs with 100TB of data.
 * Standard Amazon S3: $2,406.40 per month
 * Amazon S3 Glacier: $460.80 per month This represents a saving of just over 80%.

## Right size your instances

* AWS services are consumed on demand. This means that you pay only for what you consume. Like many AWS services, when you run the job you define how many resources you want and you also choose the instance size you want. 

* You also have ability to stop the instance when it’s not being used and you can spin it up based on specific schedule. For example, transient EMR clusters can be used for scenarios where data has to be processed once a day or once a month.

* Monitor instance metrics with analytic workloads and downsize the instances if they are over provisioned.
 
## Use Spot Instances

* Amazon EC2 Spot Instances let you take advantage of unused Amazon EC2 capacity in the AWS Cloud. Spot Instances are available at up to a 90% discount compared to on- demand prices. For example, using spot helps the Salesforce save up to 40 percent over Amazon EC2 Reserved Instance pricing and up to 80 percent over on-demand pricing. Spot is a great use case for batch when time to insight is less critical.

* In the following example, we have modeled a transient Amazon EMR cluster that takes six hours to complete its job.
 * On-Demand Instance: $106.56 per month
 * Spot Instance (estimated at a conservative 70%): $31.96 per month This represents a saving of just over 70%.

## Use Reserved Instances

* Amazon EC2 Reserved Instances (RI) provide a significant discount (up to 72%) compared to On-Demand pricing and provide a capacity reservation when used in a specific Availability Zone. This can be useful if some services used in the data lake will be constantly utilized.

* A good example of this can be Amazon Elasticsearch. In this scenario we’ll model a 10 node Amazon Elasticsearch cluster (3 master + 7 data) with both on-demand and reserved pricing. In this model, we’ll use c5.4xlarge instances with a total storage capacity of 1 TB.
 * On-demand: $ 7,420.40 per month
 * RI (three years, all up front): $ 3,649.44 per month This represents a saving of just over 50%.

## Choose the right tool for the job

* There are many different ways to interact with AWS services. Picking the right tool for the job, helps reduce cost.
If you are using Kinesis Data Streams and pushing data into it, you can choose between the AWS SDK, the Kinesis Producer Library (KPL), or the Kinesis Agent. Sometimes the option is based on the source of the data and sometimes the developer can choose.
 
* Using the latest KPL enables you to use a native capability to aggregate multiple messages/events into a single PUT unit (Kinesis record aggregation). Kinesis data streams shards support up to 1000 records per second or 1-MB throughput. Record aggregation by KPL enables customers to combine multiple records into a single Kinesis Data Streams record. This allows customers to improve their per shard throughput.

* For example, if you have a scenario of pushing 100,000 messages/sec with 150 byes in each message into a Kinesis data stream, you can use KPL to aggregate them into 15 Kinesis data stream records. The following is the difference in cost between using (latest version) KPL and not.
 * Without KPL (latest version): $4,787.28 per month
 * With KPL (latest version): $201.60 per month This represents a saving of 95%.

## Use automatic scaling

* Automatic scaling is the ability to spin resources up and down based on the need. Building application elasticity enables you to incur cost only for your fully utilized resources.

* Building EC2 instances using an Auto Scaling group can provide elasticity for Amazon EC2 based services where applicable. Even for serverless services like Kinesis where shards are defined per stream during provisioning, AWS Application Auto Scaling provides ability to automatically add or remove shards based on utilization.

* For example, if you are using Kinesis streams to capture user activity for an application hosted in specific Region the streaming information might vary between day and night. During day times, when the user activity is higher you might need more shards compared to night times when the user activity would be very low. Being able to configure AWS Application Auto Scaling based on your utilization enables you to optimize your cost for Kinesis.

* Data flowing at rate of 50,000 records/sec with each record having 2 K bytes, will require 96 shards. If the data flow reduces to 1000 records/sec for eight hours during night, it would only need two shards.
 * Without AWS Application Auto Scaling: $1068.72 per month
 * With AWS Application Auto Scaling (downsizing): $725.26 per month
 * This represents a saving of 32%.

## Choose serverless services

* Serverless services are fully managed services that incur costs only when you use them. By choosing a serverless service, you are eliminating the operational cost of managing the service.

* For example, in scenarios where you want to run a job to process your data once a day, using serverless service incurs a cost only when the job is run. In comparison, using a self-managed service incurs a cost for the provisioned instance that is hosting the service.

* Running a Spark job in AWS Glue to process a CSV file stored in Amazon S3 requires 10 minutes of 6 DPU’s and cost $0.44. To execute the same job on Amazon EMR, you need at least three m5.xlarge instances (for high availability) running at a rate of $0.240 per hour.
 * Using a serverless service: $13.42 price per month
 * Not using a serverless service: $527.04 price per month This represents a saving of 97%.

* Similar savings are applicable between running Amazon Kinesis vs Amazon MSK and between Amazon Athena vs Amazon EMR.

* While we are not covering cost optimization in any detail, we have indicated the macro factors that you should consider during each stage to keep costs down. To understand cost optimization in detail, see the Analytics Lens for the AWS Well-Architected Framework.

---

Hope this guide helps you Understand what influences data lakes costs.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.

{% user aditmodi %}

---

[Reference Notes](https://d1.awsstatic.com/whitepapers/cost-modeling-data-lakes.pdf?did=wp_card&trk=wp_card)
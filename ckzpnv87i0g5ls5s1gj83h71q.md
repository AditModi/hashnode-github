## Predictive Analytics Architecture on AWS

* AWS includes the components needed to enable pipelines for predictive analytics workflows. There are many viable architectural patterns to effectively compute predictive analytics. In this section, we discuss some of the technology options for building predictive analytics architecture on AWS. 

Figure 1 shows one such conceptual architecture.

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645022393920/77Y2-qPzI.png)
*Figure 1 — Conceptual reference architecture*

## Data Sources and Data Ingestion

* Data collection and ingestion is the first step and one of the most important technical architecture components to the overall predictive analytics architecture. At a high level, the main source data required for M&E analytics can be classified into the following categories.
 * **Dimension data** — Provides structured labeling information to numeric measures. Dimension data is mainly used for grouping, filtering, and labeling of information. Examples of dimension data are customer master data, demographics data, transaction or subscription data, content metadata, and other reference data. These are mostly structured data stored in relational databases, such as CRM, Master Data Management (MDM), or Digital Asset Management (DAM) databases.
 * **Social media data** — Can be used for sentiment analysis. Some of the main social data sources for M&E are Twitter, YouTube, and Facebook. The data could encompass content ratings, reviews, social sharing, tagging, and bookmarking events.
 * **Event data** — In OTT and online media, examples of event data are audience engagement behaviors with streaming videos such as web browsing patterns, searching events for content, video play/watch/stop events, and device data. These are mostly real-time click-streaming data from websites, mobile apps, and OTT players.
 * **Other relevant data** — Includes data from aggregators (Nielson, comScore, etc.), advertising response data, customer contacts, and service case data.

* There are two main modes of data ingestion into AWS: batch and streaming.

### Batch Ingestion

* In this mode, data is ingested as files (e.g., database extracts) following a specified schedule. Data ingestion approaches include the following.
 
 * **Third-party applications** — These applications have connector integration with Amazon Simple Storage Service (Amazon S3) object storage that can ingest data into Amazon S3 buckets. The applications can either take source files or extract data from the source database directly and store them in Amazon S3. There are commercial products (e.g., Informatica, Talend) and open-source utilities (e.g., Embulk) that can extract data from databases and export the data into an Amazon S3 bucket directly.
 * **Custom applications using AWS SDK/APIs** — Custom applications can use AWS SDKs and the Amazon S3 application programming interface (API) to ingest data into target Amazon S3 buckets. The SDKs and API also support multipart upload for faster data transfer to Amazon S3 buckets.
 * **AWS Data Pipeline** — This service facilitates moving data between different sources, including AWS services. AWS Data Pipeline launches a task runner that is a Linux-based Amazon Elastic Compute Cloud (Amazon EC2) instance, which can run scripts and commands to move data on an event-based or scheduled basis.
 * **Command line interface (CLI)** — Amazon S3 also provides a CLI for interacting and ingesting data into Amazon S3 buckets.
 * **File synchronization utilities** — Utilities such as rsynch and s3synch can keep source data directories in sync with Amazon S3 buckets, as a way to move files from source locations to Amazon S3 buckets.

### Streaming Ingestion

* In this mode, data is ingested in streams (e.g., clickstream data). Architecturally, there must be a streaming store that accepts and stores streaming data at scale, and in real time. Additionally, data collectors that collect data at the sources are needed to send data to the streaming store.

 * **Streaming stores** — There are various options for the streaming stores. Amazon Kinesis Streams and Amazon Kinesis Firehose are fully managed streaming stores. Streams and Firehose also provide SDKs and APIs for programmatic integration. Alternatively, open-source platforms such as Kafka can be installed and configured on EC2 clusters to manage streaming data ingestion and storage.
 * **Data collectors** — These can be web, mobile, or OTT applications that send data directly to the streaming store, or collector agents running next to the data sources (e.g., clickstream logs) that send data to the streaming store in real time. There are several options for the data collectors. Flume and Flentd are two open-source data collectors that can collect log data and send data to streaming stores. An Amazon Kinesis agent can be used as the data collector for Streams and Firehose.
One common practice is to ingest all the input data into staging Amazon S3 buckets or folders first, perform further data processing, and then store the data in target Amazon S3 buckets or folders.
Any data processing related to data quality (e.g., data completeness, invalid data) should be handled at the sources when possible, and is not discussed in this document. During this stage, the following data processing might be needed.
 * **Data transformation** — This could be transformation of source data to the defined common standards. For example, breaking up a single name field into first name, middle name, and last name fields.
 * **Metadata extraction and persistence** — Any metadata associated with input files should be extracted and stored in a persistent store. This could include file name, file or record size, content description, data source information, and date or time information.
 * **Data enrichment** — Raw data can be enhanced and refined with additional information. For example, you can enrich source IP addresses with geographic data.
 * **Table schema creation and maintenance** — Once the data is processed into a target structure, you can create the schemas for the target systems.

### File Formats

* The various file formats have tradeoffs regarding compatibility, storage efficiency, read performance, write performance, and schema extensibility. In the Hadoop ecosystem there are many variations of file-based data stores. The following are some of the more common ones in use.
 
 * **Comma Separated Values (CSV)** — CSV, typically the lowest common denominator of file formats, excels at providing tremendous compatibility between platforms. It’s a common format for going into and out of the Hadoop ecosystem. This file type can be easily inspected and edited with a text editor, which provides flexibility for ad hoc usage. One drawback is poor support for compression, so the files tend to take up more storage space than some other available formats. You should also note that CSV sometimes has a header row with column names. Avoid using this with machine learning tools because it inhibits the ability to arbitrarily split files.
 * **JavaScript Object Notation (JSON)** — JSON is similar to CSV in that text editors can consume this format easily. JSON records can be stored using a delimiter such as a newline character, as a demarcation to split large data sets across multiple files. However, JSON files include some additional metadata whereas CSV files typically do not when used in Hadoop. JSON files with one record should be avoided because this would generally result in too many small files.
 * **Apache Parquet** — A columnar storage format that is integrated into much of the Hadoop ecosystem, Parquet allows for compression schemes to be specified on a per-column level. This provides the flexibility to take advantage of compression in the right places without the penalty of wasted CPU cycles compressing and decompressing data that doesn’t need compressing. Parquet is also flexible for encoding columns. Selecting the right encoding mechanism is also important to maximize CPU utilization when reading and writing data. Because of the columnar format, Parquet can be very efficient when processing jobs that only require reading a subset of columns. However, this columnar format also comes with a write penalty if your processing includes writes.
 * **Apache Avro** — Avro can be used as a file format or as an object format that is used within a file format, such as Parquet. Avro uses a binary data format requiring less space to represent the same data in a text format. This results in lower processing demands in terms of I/O and memory. Avro also has the advantage of being compressible, further reducing the storage size and increasing disk read performance. Avro includes schema data and data that is defined in JSON, while still being persisted in a binary format. The Avro data format is flexible and expressive, allowing for schema evolution and support for more complex data structures such as nested types.
 * **Apache ORC** — Another column-based file format designed for high speed within Hadoop. For flat data structures, ORC has the advantage of being optimized for reads that use predicates in WHERE clauses in Hadoop ecosystem queries. It also compresses quite efficiently with compression schemes such as Snappy, Zlib, or GZip.
 * **Sequence files** — Hadoop often uses sequence files as temporary files during processing steps of a MapReduce job. Sequence files are binary and can be compressed to improve performance and reduce required storage volume. Sequence files are stored row based, with sync ending markers enabling splitting. However, any edits will require the entire file to be rewritten.


## Data Store

* For the data store portion of your solution, you need storage for the data, derived data lake schemas, and a metadata data catalogue. As part of that, a critical decision to make is the type or types of data file formats you will process. Many types of object models and storage formats are used for machine learning.
Common storage locations include databases and files.

* From a storage perspective, Amazon S3 is the preferred storage option for data science processing on AWS. Amazon S3 provides highly durable storage and seamless integration with various data processing services and machine learning platforms on AWS.

### Data Lake Schemas

* Data lake schemas are Apache HIVE tables that support SQL-like data querying using Hadoop-based query tools such as Apache HIVE, Spark SQL, and Presto. Data lake schemas are based on the schema-on-read design, which means table schemas can be created after the source data is already loaded into the data store. 

* A data lake schema uses a HIVE metastore as the schema repository, which can be accessed by different query engines. In addition, the tables can be created and managed using the HIVE engine directly.

### Metadata Data Catalogue

* A metadata data catalogue contains information about the data in the data store. It can be loosely categorized into three areas: technical, operational, and business.
 
 * Technical metadata refers to the forms and structure of the data. In addition to data types, technical metadata can also contain information about what data is valid and the data’s sensitivity.
 * Operational metadata captures information such as the source of the data, time of ingestion, and what ingested the data. Operational metadata can show data lineage, movement, and transformation.
 * Business metadata provides labels and tags for data with business-level attributes to make it easier for someone to search and browse data in the data store.

* There are different options to process and store metadata on AWS. One way is to trigger AWS Lambda functions by using Amazon S3 events to extract or derive metadata from the input files and store metadata in Amazon DynamoDB.

## Processing by Data Scientists

* When all relevant data is available in the data store, data scientists can perform offline data exploration and model selection, data preparation, and model training and generation based on the defined business objectives. 

* The following solutions were selected because they are ideal for handling the large amount of data M&E use cases generate.

### Interactive Data Exploration

* To develop the data understanding needed to support the modeling process, data scientists often must explore the available datasets and determine their usefulness. This is normally an interactive and iterative process and requires tools that can query data quickly across massive amounts of datasets. It is also useful to be able to visualize the data with graphs, charts, and maps. Table 1 provides a list of data exploration tools available on AWS, followed by some specific examples that can be used to explore the data interactively.

Table 1: Data exploration tool options on AWS





|Query Style	|Query Engine	|User Interface Tools	|AWS Services|
| -------------- |---------------|---------------| ------------|
|SQL	|Presto	|AirPal JDBC/ODBC Clients Presto CLI	|EMR|
|       |Spark SQL	|Zeppelin Spark Interactive Shell |EMR|
|       |Apache HIVE	|Apache HUE HIVE Interactive Shell |EMR|
|Programmatic	|R/SparkR (R)	|RStudio R Interactive Shell|	|EMR|
|       |Spark(PySpark, Scala)	|Zeppelin Spark Interactive Shell	|EMR|


#### Presto on Amazon EMR

* The M&E datasets can be stored in Amazon S3 and are accessible as external HIVE tables. An external Amazon RDS database can be deployed for the HIVE metastore data. Presto running in an Amazon EMR cluster can be used to run interactive SQL queries against the datasets. Presto supports ANSI SQL so you can run complex queries, as well as aggregation against any dataset size, from gigabytes to petabytes.

* Java Database Connectivity (JDBC) and Open Database Connectivity (ODBC) drivers support connections from data visualization tools such as Qlikview, Tableau, and Presto for rich data visualization. Web tools such as AirPal provide an easy-to-use web front end to run Presto queries directly.

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645022396132/JopBOIKgq.png)
Figure 2 — Data exploration with Presto

#### Apache Zeppelin with Spark on EMR

* Another tool for data exploration is Apache Zeppelin notebook with Spark. Spark is a general-purpose cluster computing system. It provides high-level APIs for Java, Python, Scala, and R. Spark SQL, an in-memory SQL engine, can integrate with HIVE external tables using HiveContext to query the dataset.

* Zeppelin provides a friendly user interface to interact with Spark and visualize data using a range of charts and tables. Spark SQL can also support JDBC/ODBC connectivity through a server running Thrift.

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645022398461/tNq2pBSdG.png) 
*Figure 3 — Data exploration with Zeppelin*

#### R/SparkR on EMR

* Some data scientists like to use R/RStudio as the tool for data exploration and analysis, but feel constrained by the limitations of R, such as single-threaded execution and small data size support. SparkR provides both the interactive environment, rich statistical libraries, and visualization of R. Additionally, SparkR provides the scalable, fast distributed storage and processing capability of Spark. 

* SparkR uses DataFrames as the data structure, which is a distributed collection of data organized into named columns. DataFrames can be constructed from wide array of data sources, including HIVE tables.

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645022400305/79bLbeMk5.png)  
*Figure 4 — Data exploration with Spark + R*

### Training Data Preparation

* Data scientists will need to prepare training data to support supervised and unsupervised model training. Data is formatted, transformed, and enriched for the modeling purpose. As only the relevant data variable should be included in the model training, feature selection is often performed to remove unneeded and irrelevant attributes that do not contribute to the accuracy of the predictive model. 

* Amazon ML provides feature transformation and feature selection capability that simplifies this process. Labeled training datasets can be stored in Amazon S3 for easy access by machine learning services and frameworks.

### Interactive Model Training

* To generate and select the right models for the target business use cases, data scientists must perform interactive model training against the training data. Table 2 provides a list of use cases with potential products that can you can use to create your solution, followed by several example architectures for interactive model training.

Table 2 — Machine learning options on AWS

|M&E Use Case	|ML Algorithms	|ML Software	|AWS Services|
| -------------- |---------------|---------------| ------------|
|Segmentation	|Clustering (e.g., k- Means)	|Spark ML Mahout R	|EMR|
|Recommendation	|Collaborative Filtering (e.g., Alternating Least Square)	|Spark ML Apache Mahout	|EMR|
|	|Neural Network	|MXNet	|Amazon EC2/GPU|
|Customer Churn	|Classification (e.g., Logistic Regression) |Managed Service	|Amazon Machine Learning|
|		|  |Spark ML Apache Mahout R	|EMR|
|Sentiment Analysis	|Classification(e.g., Logistic Regression) |Managed Service	|Amazon Machine Learning|
	|Classification(e.g., Support Vector Machines, NaïveBayes)	|Spark ML Mahout R	|EMR|
	|Neural Network	|MXNet Caffe Tensorflow TorchTheano	|Amazon EC2/GPU|


#### Amazon ML Architecture

* Amazon ML is a fully managed machine learning service that provides the quickest way to get started with model training. Amazon ML can support long tail use cases, such as churn and sentiment analysis, where logistic regression (for classification) or linear regression (for the prediction of a numeric value) algorithms can be applied. The following are the main steps of model training using Amazon ML.
 
1.	**Data source creation** — Label training data is loaded directly from the Amazon S3 bucket where the data is stored. A target column indicating the prediction field must be selected as part the data source creation.
2.	**Feature processing** — Certain variables can be transformed to improve the predictive power of the model.
3.	**ML model generation** — After the data source is created, it can be used to train the machine learning model. Amazon ML automatically splits the labeled training set into a training set (70%) and an evaluation set (30%). Depending on the selected target column, Amazon ML automatically picks one of three algorithms (binary logistic regression, multinomial logistic regression, or linear regression) for the training.
4.	**Performance evaluation** — Amazon ML provides model evaluation features for model performance assessment and allows for adjustment to the error tolerance threshold.

* All trained models are stored and managed directly within the Amazon ML service and can be used for both batch and real-time prediction.

#### Spark ML/Spark MLlib on Amazon EMR Architecture

* For the use cases that require other machine learning algorithms, such as clustering (for segmentation) and collaborative filtering (for recommendation), Amazon EMR provides cluster management support for running Spark ML.

* To use Spark ML and Spark MLlib for interactive data modeling, data scientists have two choices. They can use Spark shell by SSH’ing onto the master node of the EMR cluster, or use data science notebook Zeppelin running on the EMR cluster master node.

* Spark ML or Spark MLlib supports a range of machine learning algorithms for classification, regression, collaborative filtering, clustering, decomposition, and optimization. Another key benefit of Spark is that the same engine can perform data extraction model training and interactive query.

* A data scientist will need to programmatically train the model using languages such as Java, Python, or Scala. Spark ML provides a set of APIs for creating and tuning machine learning pipelines. The following are the main concepts to understand for pipelines.
 
 * **DataFrame** — Spark ML uses a DataFrame from Spark SQL as an ML dataset. For example, a DataFrame can have different columns corresponding to different columns in the training dataset that is stored in Amazon S3.
 * **Transformer** — An algorithm that can transform one DataFrame into another DataFrame. For instance, an ML model is a Transformer that transforms a DataFrame with features into a DataFrame with predictions.
 * **Estimator** — An algorithm that can fit on a DataFrame to produce a transformer.
 * **Parameter** — All transformers and estimators share a common API for specifying parameters.
 * **Pipeline** — Chains multiple Transformers and Estimators to specify an ML workflow.

* Spark ML provides two approaches for model selection: cross-validation and validation split.

* With **cross-validation**, the dataset is split into multiple folds that are used as separate training and test datasets. Two-thirds of each fold are used for training and one-third of each fold is used for testing. This approach is a well-established method for choosing parameters and is more statistically sound than heuristic tuning by hand. However, it can be very expensive as it cross-validates over a grid of parameters.

* With **validation split**, the dataset is split into a training asset and a test data asset. This approach is less expensive, but when the training data is not sufficiently large it won’t produce results that are as reliable as using cross- validation.

* Spark ML supports a method to export models in the Predictive Model Markup Language (PMML) format. The trained model can be exported and persisted into an Amazon S3 bucket using the model save function. The saved models can then be deployed into other environments and loaded for generating prediction.

#### Machine Learning on EC2/GPU/EMR Architectures

* For use cases that require different machine learning frameworks that are not supported by Amazon ML or Amazon EMR, these frameworks can be installed and run on EC2 fleets. An AMI is available with preinstalled machine learning packages, including MXNet, CNTK, Caffe, Tensorflow, Theano, and Torch. Additional machine learning packages can be added easily to EC2 instances.

* Other machine learning frameworks can also be installed on Amazon EMR via bootstrap actions to take advantage of the EMR cluster management. Examples include Vowpal Wabbit, Skytree, and H2O.

## Prediction Processing and Serving

* One architecture pattern for serving predictions quickly using both historic and new data is the lambda architecture. The components for this architecture include a batch layer, speed layer, and serving layer all working together to enable up-to-date predictions as new data flows into the system.

* Despite its name, this pattern is not related to the AWS Lambda service. 

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645022402095/Wlyuw_mzg.png) 
*Figure 5 — Lambda architecture components*

* The following is a brief description for each portion of the pattern shown in Figure 7.

 * **Event data** — Event-level data is typically log data based on user activity. This could be data captured on websites, mobile devices, or social media activities. Amazon Mobile Analytics provides an easy way to capture user activity for mobile devices. The Amazon Kinesis Agent makes it easy to ingest log data such as web logs. Also, the Amazon Kinesis Producer Library (KPL) makes it easy to programmatically ingest data into a stream.
 * **Streaming** — The streaming layer ingests data as it flows into the system. A popular choice for processing streams is Amazon Kinesis Streams because it is a managed service that minimizes administration and maintenance. Amazon Kinesis Firehose can be used as a stream that stores all the records to a data lake, such as an Amazon S3 bucket.
 * **Data lake** — The data lake is the storage layer for big data associated with event-level data generated by M&E users. The popular choice in AWS is Amazon S3 for highly durable and scalable data.
 * **Speed layer** — The speed layer continually updates predictive results as new data arrives. This layer processes less data than the batch layer so the results may not be as accurate as the batch layer. However, the results are more readily available. This layer can be implemented in Amazon EMR using Spark Streaming.
 * **Batch layer** — The batch layer processes machine learning models using the full set of event-level data available. This processing can take longer but can produce higher fidelity predictions. This layer can be implemented using Spark ML in Amazon EMR.
 * **Serving layer** — The serving layer responds to predictions on an ongoing basis. This layer arbitrates between the results generated by the batch and speed layers. One way to accomplish this is by storing predictive results in a NoSQL database such as DynamoDB. With this approach predictions are stored on an ongoing basis by both the batch and speed layers as they are processed.

---

Hope this guide gives you an Introduction to Predictive Analytics Architecture on AWS.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.

{% user aditmodi %}

---

[Reference Guide](https://d1.awsstatic.com/whitepapers/Analytics/ME%20Advanced%20Analytics%20on%20AWS.pdf?did=wp_card&trk=wp_card)
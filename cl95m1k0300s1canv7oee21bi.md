# Database Caching Strategies Using Redis | AWS White Paper Summary

* In-memory data caching can be one of the most effective strategies for improving your overall application performance and reducing your database costs.

* You can apply caching to any type of database, including relational databases (such as Amazon Relational Database Service (Amazon RDS)) or NoSQL databases (such as Amazon DynamoDB, Amazon DocumentDB (with MongoDB compatibility), and Amazon Keyspaces (for Apache Cassandra)).

* One of the benefits of caching is that it’s an easier option to implement, and it dramatically improves the speed and scalability of your application. Caching can also apply to objects (for instance, objects stored in Amazon Simple Storage Service), as this paper will explore.

* This whitepaper describes some of the caching strategies and implementation approaches that address the limitations and challenges associated with disk-based databases.

# Database challenges

* When you’re building distributed applications that require low latency and scalability, disk-based databases can pose a number of challenges. A few common ones include the following:

 * **Slow processing queries:** There are a number of query optimization techniques and schema designs that help boost query performance. However, the data retrieval speed from disk plus the added query processing times generally put your query response times in double-digit millisecond speeds, at best. This assumes that you have a steady load and your database is performing optimally.

 * **Cost to scale:** Whether the data is distributed in a disk-based NoSQL database or a vertically-scaled relational database, scaling for extremely high reads can be costly. It also can require several database read replicas to match what a single in-memory cache node can deliver in terms of requests per second.

 * **The need to simplify data access:** Although relational databases provide an excellent means to data model relationships, they aren’t optimal for data access. There are instances where your applications may want to access the data in a particular structure or view, to simplify data retrieval and increase application performance.

* Before implementing database caching, many architects and engineers spend great effort trying to extract as much performance as they can from their databases. However, there is a limit to the performance that you can achieve with a disk-based database, and it’s counterproductive to try to solve a problem with the wrong tools. For example, a large portion of the latency of your database query is dictated by the physics of retrieving data from disk.

# Types of database caching

* A database cache supplements your primary database by removing unnecessary pressure on it, typically in the form of frequently-accessed read data. The cache itself can live in several areas, including in your database, in the application, or as a standalone layer.

* The following are the three most common types of database caches:

 * Database-integrated caches
 * Local caches
 * Remote caches

# Caching patterns

* When you are caching data from your database, there are caching patterns for Redis and Memcached that you can implement, including proactive and reactive approaches. The patterns you choose to implement should be directly related to your caching and application objectives.

* Two common approaches are cache-aside or lazy loading (a reactive approach) and write-through (a proactive approach). A cache-aside cache is updated after the data is requested. A write-through cache is updated immediately when the primary database is updated. With both approaches, the application is essentially managing what data is being cached and for how long.

# Cache Validity

* You can control the freshness of your cached data by applying a time to live (TTL) or expiration to your cached keys. After the set time has passed, the key is deleted from the cache, and access to the origin data store is required along with reaching the updated data.

* Two principles can help you determine the appropriate TTLs to apply and the types of caching patterns to implement. First, it’s important that you understand the rate of change of the underlying data. Second, it’s important that you evaluate the risk of outdated data being returned back to your application instead of its updated counterpart.

* For example, it might make sense to keep static or reference data (that is, data that is seldom updated) valid for longer periods of time with write-throughs to the cache when the underlying data gets updated.

* With dynamic data that changes often, you might want to apply lower TTLs that expire the data at a rate of change that matches that of the primary database. This lowers the risk of returning outdated data while still providing a buffer to offload database requests.

* It’s also important to recognize that, even if you are only caching data for minutes or seconds versus longer durations, appropriately applying TTLs to your cached keys can result in a huge performance boost and an overall better user experience with your application.

* Another best practice when applying TTLs to your cache keys is to add some time jitter to your TTLs. This reduces the possibility of heavy database load occurring when your cached data expires. Take, for example, the scenario of caching product information. If all your product data expires at the same time and your application is under heavy load, then your backend database has to fulfill all the product requests. Depending on the load, that could generate too much pressure on your database, resulting in poor performance. By adding slight jitter to your TTLs, a randomly-generated time value (for example, TTL = your initial TTL value in seconds + jitter) would reduce the pressure on your backend database and also reduce the CPU use on your cache engine as a result of deleting expired keys.

# Evictions

* Evictions occur when cache memory is overfilled or is greater than the maxmemory setting for the cache, causing the engine--selecting keys to evict in order to manage its memory. The keys that are chosen are based on the eviction policy you select.

* By default, Amazon ElastiCache for Redis sets the volatile-lru eviction policy to your Redis cluster. When this policy is selected, the least recently used keys that have an expiration (TTL) value set are evicted. Other eviction policies are available and can be applied in the configurable maxmemory-policy parameter.

* The following table summarizes eviction policies:

| Eviction Policy | Description | 
| --------------- | ----------- | 
| allkeys-lru	  | The cache evicts the least recently used (LRU) keys regardless of TTL set. | 
| allkeys-lfu	  | The cache evicts the least frequently used (LFU) keys regardless of TTL set. | 
| volatile-lru    | The cache evicts the least recently used (LRU) keys from those that have a TTL set.| 
| volatile-lfu	  | The cache evicts the least frequently used (LFU) keys from those that have a TTL set.| 
| volatile-ttl	 | The cache evicts the keys with the shortest TTL set.| 
| volatile-random	| The cache randomly evicts keys with a TTL set.| 
| allkeys-random	| The cache randomly evicts keys regardless of TTL set.| 
| no-eviction	| The cache doesn’t evict keys at all. This blocks future writes until memory frees up.| 


* A good strategy in selecting an appropriate eviction policy is to consider the data stored in your cluster and the outcome of keys being evicted.

* Generally, least recently used (LRU-based policies are more common for basic caching use cases. However, depending on your objectives, you might want to use a TTL or random-based eviction policy that better suits your requirements.

* Also, if you are experiencing evictions with your cluster, it is usually a sign that you should scale up (that is, use a node with a larger memory footprint) or scale out (that is, add more nodes to your cluster) to accommodate the additional data. An exception to this rule is if you are purposefully relying on the cache engine to manage your keys by means of eviction, also referred to an LRU cache.7

* Amazon ElastiCache for Redis also supports a frequency-based LFU (Least Frequently Used) eviction policy for evicting keys, in addition to the existing time-based LRU policy. The new LFU policy, which is based on frequency of access, provides a better cache hit ratio by keeping frequently used data in-memory; it traces access counter for each object and evicts keys according to the counter. Every time the object is touched, it reduces the counter after a period called the decay period. This means data used rarely is evicted while the data used often has a higher chance of remaining in the memory.

# Relational Database Caching Techniques

# Additional Caching with Redis

* Redis is traditionally used in a database cache setting. However, it can also be used to cache output from other services, or full objects from storage services such as Amazon Simple Storage Service (Amazon S3).

* The low latency and high throughput of Amazon ElastiCache for Redis, coupled with the large item storage availability, make it a great choice for further optimizing throughput and scalability to new and existing applications.

## Object Caching with Amazon S3

* Amazon S3 is the persistent store for applications such as data lakes, media catalogs, and website-related content. These applications often have latency requirements of 10 ms or less, with frequent object requests on 1–10% of the total stored S3 data. Many customers achieve this by directly writing to S3. 

* Some applications, such as media catalog updates, require high frequency reads and consistent throughput. For such applications, customers often complement S3 with Redis, to reduce the S3 retrieval cost and to improve performance.

* By using Amazon ElastiCache for Redis, applications can maintain a consistent and low-latency throughput, sustained at less than 5 ms, when serving this content outside of S3 at scale. 

* Serving heavily-requested objects via Amazon ElastiCache for Redis in this manner can enable you to meet performance goals, while also reducing retrieval and transfer costs.

# Amazon ElastiCache and Self-Managed Redis

* Redis is an open source, in-memory data store that has become the most popular key/value engine in the market. Much of its popularity is due to its support for a variety of data structures as well as other features, including Lua scripting support and Pub/Sub messaging capability. Other added benefits include high availability topologies with support for read replicas and the ability to persist data.

* Amazon ElastiCache offers a fully-managed service for Redis. This means that all the administrative tasks associated with managing your Redis cluster (including monitoring, patching, backups, and automatic failover), are managed by Amazon. This lets you focus on your business and your data instead of your operations.

* Other benefits of using Amazon ElastiCache for Redis over self-managing your cache environment include the following:

 * An enhanced Redis engine that is fully compatible with the open source version but that also provides added stability and robustness.

 * Easily modifiable parameters, such as eviction policies, buffer limits, etc.

 * Ability to scale and resize your cluster to terabytes of data.

 * Hardened security that lets you isolate your cluster within Amazon Virtual Private Cloud (Amazon VPC).9

## Redis Engine Support

* You can use Amazon ElastiCache for Redis to build HIPAA-compliant applications. To help do this, you can enable at-rest encryption, in-transit encryption, and Redis AUTH when you create a Redis cluster using ElastiCache for Redis versions 3.2.6 or later.

* Amazon ElastiCache for Redis version 6.x also support a feature called Role-Based Access Control (RBAC), which can be used instead of authenticating users with the Redis AUTH command. With RBAC, users can be created with specific permissions by using an access string. Users can also be assigned to user groups aligned with a role that is specific to your use cases. More information on RBAC can be found at: Authenticating Users with Role-Based Access Control documentation.

* In versions 5.0.3 and later, Amazon ElastiCache for Redis adds dynamic network processing to enhance I/O handling. By utilizing the extra CPU power available in nodes with four or more vCPUs, Amazon ElastiCache transparently delivers an increase of up to 83% in throughput and up to 47% reduction in latency per node. That is in addition to the significant performance improvements delivered with the introduction of R5 and M5 instances on the service. You can now benefit from the enhanced I/O handling to further boost application performance and reduce costs.

* Amazon ElastiCache for Redis improves throughput and reduces latency by leveraging more cores for processing I/O and dynamically adjusting to the workload. These improvements work best for applications that require a large number of concurrent connections to the Redis server. No client-side changes are needed.

* Because the newer Redis versions provide a better and more stable experience, Redis versions 2.6.13, 2.8.6, and 2.8.19 are deprecated when using the Amazon ElastiCache console. We recommend against using these Redis versions. If you need to use one of them, work with the AWS CLI or Amazon ElastiCache API. The latest version information can be found on the Supported ElastiCache for Redis Versions page.

## Available Instance Types

* Amazon ElastiCache supports multiple instance node types. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts.

* Furthermore, you can launch both ElastiCache for Redis and Memcached on Graviton2 M6g and R6g instance families. The latest Graviton2 instance types offer you ultra-low latency and high throughput, and you can now enjoy a price/performance improvement of up to 45% over previous generation instances. Graviton2 instances are now the default choice for ElastiCache customers.

* You can launch general-purpose burstable T*-Standard cache nodes in Amazon ElastiCache. These nodes provide a baseline level of CPU performance with the ability to burst CPU usage at any time until the accrued credits are exhausted. A CPU credit provides the performance of a full CPU core for one minute.

## AWS Nitro System

* The AWS Nitro System is the underlying platform for the next generation of EC2 instances that enables AWS to innovate faster, further reduce cost for customers, and deliver added benefits like increased security and new instance types.

* AWS has completely re-imagined its virtualization infrastructure. Traditionally, hypervisors protect the physical hardware and bios, virtualize the CPU, storage, and networking, and provide a rich set of management capabilities. With the Nitro System, we are able to break apart those functions and offload them to dedicated hardware and software.

* The Nitro System delivers practically all of the compute and memory resources of the host hardware to your instances, resulting in better overall performance.

* Additionally, dedicated Nitro Cards enable high speed networking, high speed EBS, and I/O acceleration. To benefit from the AWS Nitro System in Amazon ElastiCache for Redis, please choose a cache type from the list of supported instances: either M5 (cache.m5.xlarge or higher) or R5 (cache.r5.xlarge or higher), or later.

# Redis Cluster Modes: Enabled and Disabled

* Cluster Mode for Amazon ElastiCache for Redis is a feature that enables you to scale your Redis cluster without any impact on the cluster performance. While you initiate a scale-out operation by adding a specified number of new shards to a cluster, you also initiate scale-up or scale-down operation by selecting a new desired node type, Amazon ElastiCache for Redis a new cluster synchronizing the new nodes with the previous ones. Amazon ElastiCache for Redis supports three cluster configurations (Redis (single node), Redis (cluster mode disabled), and Redis (cluster mode enabled)), depending on your reliability, availability, and scaling requirements.

* With Cluster Mode enabled, your Redis cluster can now scale horizontally (in or out) in addition to scaling vertically (up and down). In a Redis cluster with cluster mode disabled, you can have up to five read replicas in your replication group. Adding or removing replicas incurs no downtime to your application. In a Redis cluster with cluster mode enabled, clusters can have up to ninety shards by default (which can be increased if requested) and up to five read replicas in each node group.

* Amazon ElastiCache for Redis with Cluster Mode enabled will help you to architect a cluster with unpredictable network and storage requirements, or with a write-heavy workload. This horizontal scalability is achieved by preparing a plan that results in an even distribution of the key spaces, which distributes the hash slots to the available shards within the cluster, and thus by spreading the workload over a greater number of nodes. By default, the hash slots get evenly distributed between shards, but customers can also configure a custom hash slot. It is recommended to resize your cluster during off-peak hours.

## Reader Endpoint

* Amazon ElastiCache for Redis cluster with multiple nodes and with cluster mode disabled provides the reader endpoint to direct all your read traffic to a single cluster level endpoint. For more information on reader endpoints, see Finding Replication Group Endpoints.

* This reader endpoint splits your incoming read connection requests evenly between all read replicas. This reduces the need for the clients to direct traffic to an individual replica, and simplifies configuration to look up and access cached data. Reader endpoint also helps your Amazon ElastiCache for Redis cluster to load balance all read traffic, as well as to achieve High Availability by placing read replicas in different AWS Availability Zones (AZ).

* Reader endpoints works with ElastiCache for Redis clusters with cluster-mode disabled. For more information, see Finding Replication Group Endpoints.

# Amazon ElastiCache for Redis Global Datastore

* Amazon ElastiCache for Redis Global Datastore is a feature that has been introduced to handle two primary use cases: the ability to have global low latency reads and to support Disaster Recovery scenarios. For more information, see Replication across AWS Regions using global datastores.

* With the Amazon ElastiCache for Redis you can now architect a global application with extremely low latency local reads by reading from a geo-local Global Datastore, and also maintain cluster resiliency at the same time. In the unlikely event of the primary region degrading, it can failover to a secondary region.

* Each Global Datastore is a collection of one and more clusters that replicates to one another: A Primary (Active) Cluster (that accepts writes which are replicated to all clusters) and a Secondary (Passive) Cluster (that only accepts read requests and replicates data updates from a Primary cluster). Applications with media contents can now write to an active cluster and the same content can be read in the local regions.

* Amazon ElastiCache for Redis Global Datastore lets you create a reliable, secure and fully-managed cross-region replication that enables you to easily promote your secondary cluster to primary. During cross-region replication, Global Datastore can be set up on new cluster as well as existing cluster, provided the cluster is running the latest Redis engine version (5.0.6 or later). Scalability is built in to Global Datastore, with regional clusters that can be scaled both vertically and horizontally by modifying Global Datastore without any interruption.

* Amazon ElastiCache Global Datastore enables encryption in transit for cross-region communication in addition to encryption at rest.

# Sizing Best Practices Related to Workloads

* With Redis version 5.0.5, you can scale up or scale down your Amazon ElastiCache for Redis cluster online without any downtime. This enables your cluster to stay online and respond to incoming requests while scaling.

* In order to increase read and write capacity, you can scale up your cluster by selecting a larger node type, and alternatively read and write capacity can be reduced by selecting a smaller node. Amazon ElastiCache can resize your cluster dynamically without any downtime.

* You can also scale out read capacity by adding read replicas and write capacity by adding a specified number of new shards to a cluster.

* To ensure uninterrupted scaling, you need to follow these best practices:

* To scale up, you need to ensure sufficient number of ENI (Elastic Network Interface) availability.

* To scale down, you need to ensure smaller nodes have adequate memory to absorb the incoming traffic.

* Perform scaling activities when traffic towards your cluster is at a minimum. Although the scaling process is designed to remain online, this will help to synchronize data to newer nodes.

* Always test your application in a development environment, where possible.

# Conclusion

* Modern applications can’t afford poor performance. Today’s users have low tolerance for slow-running applications and poor user experiences. When low latency and scaling databases are critical to the success of your applications, it’s imperative that you use database caching.

* Amazon ElastiCache provides two managed in-memory key value stores that you can use for database caching. With zero-downtime scalability, Global datastore for regional low-latency endpoints, and a simplified approach to running a Clustered, distributed cache without the administrative tasks associated with it, Amazon ElastiCache for Redis is the primary choice for engineering teams and customers.

# Reference

[Original paper](https://docs.aws.amazon.com/whitepapers/latest/database-caching-strategies-using-redis/welcome.html?did=wp_card&trk=wp_card)

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
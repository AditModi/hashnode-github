## Tips for Right Sizing your EC2 instances and RDS DB instances

* This article offers tips to help you right size your EC2 instances and RDS DB instances.

# Right Size Using Performance Data

* Analyze performance data to right size your EC2 instances. Identify idle instances and ones that are underutilized. Key metrics to look for are CPU usage and memory usage. Identify instances with a maximum CPU usage and memory usage of less than 40% over a four-week period. These are the instances that you will want to right size to reduce costs.

* For compute-optimized instances, keep the following in mind:

 * Focus on very recent instance data (old data may not be actionable).

* Focus on instances that have run for at least half the time you’re looking at.

* Ignore burstable instance families (T2 instance types) because these families are designed to typically run at low CPU percentages for significant periods of time.

* For storage-optimized instances (I2 and D2 instance types), where the key feature is high data IOPS, focus on IOPS to see whether instances are overprovisioned. Keep the following in mind for storage-optimized instances:

 * Different size instances have different IOPS ratings, so tailor your reports to each instance type. Start with your most commonly used storage-optimized instance type.

 * Peak NetworkIn and NetworkOut values are measured in bytes per minute. Use the following formula to convert these metrics to megabits per second:

     Maximum NetworkIn (or NetworkOut) x 8 (bytes to bits) /1024/1024/ 60 = Number of Mbps

 * Take note of how I/O and CPU percentage metrics change during the day and whether there are peaks that need to be accommodated.

* Right size against memory if you find that maximum memory utilization over a four-week period is less than 40%. AWS provides sample scripts for monitoring memory and disk space utilization on your EC2 instances running Linux. You can configure the scripts to report the metrics to Amazon CloudWatch.

* When analyzing performance data for Amazon RDS DB instances, focus on the following metrics to determine whether actual usage is lower than instance capacity:

 * Average CPU utilization

 * Maximum CPU utilization

 * Minimum available RAM

 * Average number of bytes read from disk per second

 * Average number of bytes written to disk per second

# Right Size Based on Usage Needs

* As you monitor current performance, identify the following usage needs and patterns so that you can take advantage of potential right-sizing options:

 * **Steady state** – The load remains at a relatively constant level over time, and you can accurately forecast the likely compute load. For this usage pattern, you might consider Reserved Instances, which can provide significant savings.

 * **Variable, but predictable** – The load changes, but on a predictable schedule. AWS Auto Scaling is well suited for applications that have stable demand patterns with hourly, daily, or weekly variability in usage. You can use this feature to scale Amazon EC2 capacity up or down when you experience spiky traffic or predictable fluctuations in traffic.

 * **Dev/test/production** – Development, testing, and production environments are typically used only during business hours and can be turned off during evenings, weekends, and holidays. (You’ll need to rely on tagging to identify dev/test/production instances.)

 * **Temporary** – For temporary workloads that have flexible start times and can be interrupted, you can consider placing a bid for an Amazon EC2 Spot Instance instead of using an On-Demand Instance.

# Right Size by Turning Off Idle Instances

* The easiest way to reduce operational costs is to turn off instances that are no longer being used. If you find instances that have been idle for more than two weeks, it’s safe to stop or even terminate them. Before terminating an instance that’s been idle for two weeks or less, consider:

 * Who owns the instance?

 * What is the potential impact of terminating the instance?

 * How hard will it be to re-create the instance if you need to restore it?

* Stopping an EC2 instance leaves any attached EBS volumes operational. You will continue to be charged for these volumes until you delete them. If you need the instance again, you can easily turn it back on. Terminating an instance, however, automatically deletes attached EBS volumes and requires effort to re-provision should the instance be needed again. If you decide to delete an EBS volume, consider storing a snapshot of the volume so that it can be restored later if needed.

* Another simple way to reduce costs is to stop instances used in development and production during hours when these instances are not in use and then start them again when their capacity is needed. Assuming a 50-hour work week, you can save 70% by automatically stopping dev/test/production instances during non-business hours. Many tools are available to automate scheduling, including Amazon EC2 Scheduler, AWS Lambda, and AWS Data Pipeline, as well as third-party tools such as CloudHealth and Skeddly.

# Right Size by Selecting the Right Instance Family

* You can right size an instance by migrating to a different model within the same instance family or by migrating to another instance family. When migrating within the same instance family, you only need to consider vCPU, memory, network throughput, and ephemeral storage. A good, general rule for EC2 instances is that if your maximum CPU and memory usage is less than 40% over a four-week period, you can safely cut the machine in half. For example, if you were using a c4.8xlarge EC2, you could move to a c4.4xlarge, which would save $190 every 10 days.

* When migrating to a different instance family, make sure the current instance type and the new instance type are compatible in terms of virtualization type, network, and platform:

 * **Virtualization type** – The instances must have the same Linux AMI virtualization type (PV AMI versus HVM) and platform (EC2-Classic versus EC2-VPC). For more information, see Linux AMI Virtualization Types.

 * **Network** – Some instances are not supported in EC2-Classic and must be launched in a virtual private cloud (VPC). For more information, see Instance Types Available Only in a VPC.

 * **Platform** – If your current instance type supports 32-bit AMIs, make sure to select a new instance type that also supports 32-bit AMIs (not all EC2 instance types do). To check the platform of your instance, go to the Instances screen in the Amazon EC2 console and choose **Show/Hide Columns, Architecture**.

* When you resize an EC2 instance, the resized instance usually has the same number of instance store volumes that you specified when you launched the original instance. You cannot attach instance store volumes to an instance after you’ve launched it, so if you want to add instance store volumes, you will need to migrate to a new instance type that contains the higher number of volumes.

# Right Size Your Database Instances

* You can scale your database instances by adjusting memory or compute power up or down as performance and capacity requirements change. The following are some things to consider when scaling a database instance:

 * Storage and instance type are decoupled. When you scale your database instance up or down, your storage size remains the same and is not affected by the change.

 * You can separately modify your Amazon RDS DB instance to increase the allocated storage space or improve the performance by changing the storage type (such as General Purpose SSD to Provisioned IOPS SSD).

 * Before you scale, make sure you have the correct licensing in place for commercial engines (SQL Server, Oracle), especially if you Bring Your Own License (BYOL).

 * Determine when you want to apply the change. You have an option to apply it immediately or during the maintenance window specified for the instance.

---

Hope this guide provides you with Tips for Right Sizing your EC2 instances and RDS DB instances.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.

{% user aditmodi %}

---

[Reference Guide](https://docs.aws.amazon.com/whitepapers/latest/cost-optimization-right-sizing/tips-for-right-sizing-your-workloads.html)

Photo by <a href="https://unsplash.com/@alevisionco?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">alevision.co</a> on <a href="https://unsplash.com/s/photos/crowded-alley?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  
  
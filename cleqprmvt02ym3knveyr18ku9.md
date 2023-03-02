# AWS Streaming Data Solution for Amazon MSK

➡️ Option 1: AWS CloudFormation template using Amazon Managed Streaming for Apache Kafka (Amazon MSK). Deploy a solution that automatically configures the AWS services necessary to easily capture, store, process, and deliver streaming data using Amazon MSK. To deploy this solution using the available AWS CloudFormation template, select Deploy with AWS.


![2022-07-30 (27).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659187128703/o3UmQ-W0O.png align="left")

1) An Amazon MSK cluster.

2) An Amazon EC2 instance that contains the Apache Kafka client libraries required to
communicate with the Amazon MSK cluster. This client machine is located on the
same VPC as the cluster, and it can be accessed via AWS Systems Manager Session
Manager.

More Details here: 👉
[Link](https://docs.aws.amazon.com/solutions/latest/aws-streaming-data-solution-for-amazon-msk/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-msk-sol.pdf) 

➡️ Option 2: AWS CloudFormation template using Amazon MSK and AWS Lambda
Deploy a solution that automatically configures the AWS services necessary to easily capture, store, process, and deliver streaming data using Amazon MSK. To deploy this solution using the available AWS CloudFormation template, select Deploy with AWS.

![2022-07-30 (28).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659187135056/QyUsPrZRw.png align="left")

1) An AWS Lambda function that processes process records in a Kafka topic. The default
function is a Node.js application that logs the received messages, but it can be
customized to meet your business needs.

More Details here: 👉
[Link](https://docs.aws.amazon.com/solutions/latest/aws-streaming-data-solution-for-amazon-msk/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-msk-sol.pdf) 

➡️ Option 3: AWS CloudFormation template using Amazon MSK, AWS Lambda, and Amazon Kinesis Firehose. Deploy a solution that automatically configures the AWS services necessary to easily capture, store, process, and deliver streaming data using Amazon MSK. To deploy this solution using the available AWS CloudFormation template, select Deploy with AWS.

![2022-07-30 (29).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659187140560/1VFOEUghY.png align="left")

1) An AWS Lambda function that processes process records in a Kafka topic.

2) An Amazon Kinesis Data Firehose delivery stream that buffers data before delivering it
to the destination.
3) An Amazon S3 bucket that stores all original events from the Amazon MSK
cluster.

More Details here: 👉

[Link](https://docs.aws.amazon.com/solutions/latest/aws-streaming-data-solution-for-amazon-msk/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-msk-sol.pdf) 

Option 4: AWS CloudFormation template using Amazon MSK, Amazon Kinesis Data Analytics, and Amazon S3. Deploy a solution that automatically configures the AWS services necessary to easily capture, store, process, and deliver streaming data using Amazon MSK. To deploy this solution using the available AWS CloudFormation template, select Deploy with AWS.

![2022-07-30 (30).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659187147265/V7YJGAsfE.png align="left")

1) An Amazon Kinesis Data Analytics application that reads events from an
existing topic in an Amazon MSK cluster.

2) An Amazon S3 bucket that stores the output of the demo application.

More Details here: 👉

[Link](https://docs.aws.amazon.com/solutions/latest/aws-streaming-data-solution-for-amazon-msk/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-msk-sol.pdf)

You can download the Implementation Guide from this link 👀👇
[Link](https://d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/aws-streaming-data-solution-for-amazon-msk-sol.pdf?did=wp_card&trk=wp_card)  

I hope you will find it useful for your organization! 🤓

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
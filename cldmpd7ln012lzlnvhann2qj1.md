# SQL-Based ETL with Apache Spark on Amazon EKS

➡️ This solution provides declarative data processing support, codeless ETL capabilities, and workflow orchestration automation to help your business users access their data and create meaningful insights without the need for manual IT processes. To deploy this solution using the available AWS CloudFormation template, select Deploy with AWS.


![2022-07-30 (25).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659186818307/_X2CdOIl3.png align="left")

1) A customizable and flexible workflow management layer (the Orchestration on Amazon EKS group) includes the Argo Workflows plug-in. This plug-in provides a web-based tool to orchestrate your ETL jobs without the need to write code.

2) A secure data processing workspace is configured to unify data workloads in the same Amazon Elastic Kubernetes Service (Amazon EKS) cluster. This workspace contains a second web-based tool, JupyterHub, for interactive job builds and testing. You can either develop Jupyter notebook using a declarative approach to specify ETL tasks or programmatically write your ETL steps using PySpark. This workspace also provides Spark job automations that are managed by the Argo Workflows tool.

3) A set of security functions are deployed in the solution. Amazon Elastic Container Registry (Amazon ECR) maintains and secures a data processing framework Docker image. The AWS Identity and Access Management (IAM) roles for service accounts (IRSA) feature on Amazon EKS provides token authorization with finegrained access control to other AWS services. Jupyter fetches login credentials from AWS Secrets Manager into Amazon EKS on-the-fly. Amazon CloudWatch
monitors applications on Amazon EKS using the activated CloudWatch Container Insights feature.

4) The analytical workloads on the Amazon EKS cluster outputs data results to an
Amazon S3 data lake. A data schema entry (metadata) is created in an AWS Glue Data
Catalog via Amazon Athena.

More Details here: 👉
[Link](https://d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/sql-based-etl-with-apache-spark-on-amazon-eks-sol.pdf?did=wp_card&trk=wp_card) 

You can download the Implementation Guide from this link 👀👇
[Link](https://docs.aws.amazon.com/solutions/latest/sql-based-etl-with-apache-spark-on-amazon-eks/welcome.html?campaign=sol-rad-cta&trk=https//d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/sql-based-etl-with-apache-spark-on-amazon-eks-sol.pdf)

I hope you will find it useful for your organization! 🤓

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.
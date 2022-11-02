# Modernize Applications with Microservices Architecture

➡️ Integrate Amazon EKS with VMware Cloud on AWS


![2022-07-30 (21).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659186576584/F9nudgOpx.png align="left")

1) The Elastic Network Interface is automatically attached to the EC2 bare metal (ESXi) hosts in VMware Cloud on AWS during the software defined data center (SDDC) provisioning.

2) Provision fully managed Amazon EKS clusters for different environments (dev/test/production).

3) Use tools such as AWS App2Container (A2C) to accelerate refactoring/rearchitecting applications into containerized microservices, and use Amazon EKS to manage and automate the testing and deployment of container workloads.

4) Transform and containerize legacy systems to modern applications with minimum disruptions, while keep the existing database tier running on VMware Cloud on AWS to avoid the complexity and delay for database migrations.

5) Network Load Balancer integrates with Kubernetes Ingress controller, providing a secure and consistent approach for exposing applications.

6) Amazon Route 53 resolves incoming requests to the Network Load Balancer in the primary AWS Region.

➡️ Use AWS DevOps tools with Amazon EKS

7) Dev team commits code to an AWS CodeCommit repository, which triggers AWS CodePipeline to start processing the code changes through the pipeline.

8) AWS CodeBuild packages the code changes and dependencies and builds a Docker image. The new Docker image is pushed to Amazon Elastic Container Registry (Amazon ECR).

9) AWS CodeBuild uses Kubectl command line tool to invoke Kubernetes API and update the image tag for the microservice deployment.

10) Kubernetes performs a rolling update of the pods in the application deployment as per the new docker image specified in Amazon ECR.

You can download the Implementation Guide from this link 👀👇

[https://d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/modernize-applications-with-microservices-using-amazon-eks-ra.pdf?did=wp_card&trk=wp_card](https://d1.awsstatic.com/architecture-diagrams/ArchitectureDiagrams/modernize-applications-with-microservices-using-amazon-eks-ra.pdf?did=wp_card&trk=wp_card)

I hope you will find it useful for your organization! 🤓
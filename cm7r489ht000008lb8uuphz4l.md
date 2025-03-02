---
title: "Endpoints: Gateway & Interface Endpoints"
datePublished: Thu Feb 20 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm7r489ht000008lb8uuphz4l
slug: endpoints-gateway-and-interface-endpoints
tags: aws

---

Welcome to the third post in our **AWS Networking Ninja Series**! In the last blog, we discussed **public and private subnets** and how to enable internet connectivity for your VPC resources using **Internet Gateways (IGW)** and **NAT Gateways/Instances**. Now, we’re going to dive into one of the most powerful and often underutilized features of AWS networking: **VPC Endpoints**.

In this post, we will focus on the two main types of VPC Endpoints: **Gateway Endpoints** and **Interface Endpoints**, exploring how they can help you connect your VPC securely to AWS services without needing public IPs or using the internet. We'll break down when and why you might use these endpoints, along with practical examples and best practices.

By the end of this post, you’ll have a thorough understanding of **VPC Endpoints**, and how to design network architectures that reduce latency and enhance security.

---

### **What are VPC Endpoints?**

A **VPC Endpoint** is a network component that allows private connectivity from a VPC to supported AWS services without requiring an internet gateway, NAT device, VPN connection, or Direct Connect connection. Essentially, VPC Endpoints allow you to keep traffic within the AWS network rather than routing it through the public internet.

There are **two types** of VPC Endpoints:

1. **Gateway Endpoints**
    
2. **Interface Endpoints**
    

Let’s take a deeper dive into each of these types.

---

### **1\. Gateway Endpoints**

A **Gateway Endpoint** is used to enable private connections from your VPC to certain AWS services that support it. With this type of endpoint, traffic to supported services like **Amazon S3** and **DynamoDB** remains within the AWS network, never touching the public internet.

#### **How It Works**:

* **Gateway Endpoints** use the **VPC route table** to route traffic destined for supported services directly to the endpoint. You don’t need to configure public IPs or NAT gateways for these types of connections.
    
* The traffic between your VPC and the service is encrypted in transit, ensuring data security.
    

#### **Use Cases for Gateway Endpoints**:

* **Amazon S3**: If you have an S3 bucket for storing data, you can use a Gateway Endpoint to access it from your VPC without going through the internet. This enhances security by eliminating any external traffic.
    
* **DynamoDB**: For applications that use **DynamoDB**, using a Gateway Endpoint ensures that DynamoDB requests remain entirely within the AWS network.
    

#### **Key Features**:

* Traffic doesn’t leave AWS, so it's more secure and private.
    
* Supports services like **Amazon S3** and **DynamoDB** (with more services supported in the future).
    
* No data transfer charges when accessing these services using Gateway Endpoints.
    
* Easy to set up via the VPC console with minimal configuration.
    

#### **Example: Creating a Gateway Endpoint for S3**:

1. Navigate to the VPC console.
    
2. Under **Endpoints**, click **Create Endpoint**.
    
3. Select **Gateway** as the endpoint type.
    
4. Choose **S3** as the service to connect to.
    
5. Specify the route table to associate the endpoint with.
    
6. Review and create the endpoint.
    

Once the endpoint is created, you’ll update your VPC’s route tables to route traffic destined for [**s3.amazonaws.com**](http://s3.amazonaws.com) through the endpoint.

---

### **2\. Interface Endpoints**

An **Interface Endpoint** is a private connection to an AWS service or a customer-owned service hosted in a VPC. Unlike Gateway Endpoints, Interface Endpoints use **Elastic Network Interfaces (ENIs)** in your VPC to provide a secure and scalable connection to services over **private IP addresses**.

#### **How It Works**:

* Interface Endpoints create an **ENI** in your subnet, with private IP addresses that can be used to communicate with the service.
    
* The communication between your VPC and the AWS service is done over the **AWS private network**, meaning no internet gateway is required.
    
* Services like **Amazon EC2 API**, **AWS Secrets Manager**, and **AWS Systems Manager** support Interface Endpoints.
    

#### **Use Cases for Interface Endpoints**:

* **AWS Systems Manager**: Securely manage EC2 instances in your VPC without having to expose them to the internet.
    
* **Amazon EC2 API**: Interface Endpoints allow you to interact with EC2 APIs (e.g., for Auto Scaling, instances management) without leaving your private network.
    
* **AWS Secrets Manager**: Access secrets stored in Secrets Manager securely, avoiding any exposure to public traffic.
    

#### **Key Features**:

* Enables private connections to many AWS services over **private IP addresses**.
    
* Supports both **AWS-managed services** (e.g., EC2, S3) and **customer-owned services**.
    
* Uses **ENIs** in your VPC for secure access to AWS services.
    
* You can use **VPC security groups** to control access to the Interface Endpoint.
    

#### **Example: Creating an Interface Endpoint for EC2**:

1. Go to the VPC console.
    
2. Select **Endpoints** and then click **Create Endpoint**.
    
3. Choose **Interface** as the endpoint type.
    
4. Select [**com.amazonaws.region.ec**](http://com.amazonaws.region.ec)**2** as the service name.
    
5. Specify the VPC and the subnet in which the endpoint ENI should reside.
    
6. Set the security groups to control access to the endpoint.
    
7. Review and create the endpoint.
    

Once the interface endpoint is set up, you’ll be able to interact with EC2 APIs using private IP addresses, avoiding the need for public internet routing.

---

### **Gateway vs. Interface Endpoints: When to Use Each**

* **Gateway Endpoints**: Use Gateway Endpoints when connecting to services like **Amazon S3** and **DynamoDB**, which are optimized for this type of connection. These are ideal when you need to **access storage** and **key-value databases** in a cost-effective and secure manner.
    
* **Interface Endpoints**: Use Interface Endpoints when connecting to services that require **more granular control**, such as **EC2 API**, **Secrets Manager**, or any other service that supports Interface Endpoints. This allows for greater flexibility and more services to be accessed via private IP addresses.
    

---

### **Best Practices for Using VPC Endpoints**

1. **Security**: Use **security groups** to control the inbound and outbound traffic for Interface Endpoints. For Gateway Endpoints, manage access via **IAM policies** to ensure only authorized resources can access S3 or DynamoDB.
    
2. **Cost Optimization**: While Gateway Endpoints are **free**, Interface Endpoints are billed based on usage. Ensure you monitor and optimize your endpoints to avoid unnecessary charges.
    
3. **Route Table Management**: For Gateway Endpoints, ensure the correct **route tables** are associated with the endpoint. This ensures that traffic is directed to the appropriate service.
    
4. **Monitoring and Logging**: Use **CloudWatch Logs** to monitor the usage and performance of your VPC Endpoints. Set up logging for API calls using **AWS CloudTrail** for auditing purposes.
    

---

### **Conclusion**

In this blog post, we’ve explored the world of **VPC Endpoints**, covering **Gateway** and **Interface Endpoints**. By leveraging these endpoints, you can securely and privately connect your VPC to essential AWS services like **Amazon S3**, **DynamoDB**, **EC2**, and more, all without traversing the public internet.

#### **Key Takeaways**:

* **Gateway Endpoints** allow private connections to **S3** and **DynamoDB** via VPC route tables.
    
* **Interface Endpoints** create private connections via **ENIs** to a wide variety of AWS services.
    
* Both types of endpoints enhance security, reduce latency, and optimize traffic routing in your AWS VPC.
    

In the next post of our series, we’ll explore **Hybrid Connectivity**, focusing on how to securely connect your on-premises networks to your AWS cloud using **VPN** and **AWS Direct Connect**.

Stay tuned, and happy networking! 🚀

Let me know your thoughts in the comment section 👇 And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on** [**LinkedIn**](https://www.linkedin.com/in/adit-modi-2a4362191/) 🤓 **connect with me on** [**Twitter**](https://twitter.com/adi_12_modi) 🐱‍💻 **follow me on** [**github**](https://github.com/AditModi) ✍️ **Do Checkout** [**my blogs**](https://aditmodi.com)

Like, share and follow me 🚀 for more content.

---
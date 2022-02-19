## General design principles for hybrid networking

* This article outlines two key scenarios that are common in hybrid networking and how they influence the design and architecture of your workloads on AWS. It also presents the assumptions that were made for each scenario, the common drivers for the design, and a reference architecture of how these scenarios are implemented.

* This article covers the following two hybrid networking scenarios:

 * Getting started with hybrid connectivity using Site-to-Site VPN
 * Designing a reliable, dedicated global hybrid networking setup using fiber connectivity for critical workloads

## Getting started with hybrid connectivity using Site-to-Site VPN

* The easiest way to get started with hybrid connectivity is to establish site-to-site VPN over the internet. AWS Site-to-Site VPN extends your data center or branch office to the cloud using IP Security (IPsec) tunnels. 

* You can configure routing using Border Gateway Protocol (BGP) over the IPsec tunnel or configure static routes. Traffic in the tunnel is encrypted with AES128 or AES256 and uses Diffie-Hellman groups for key exchange, providing Perfect Forward Secrecy.

* Each AWS Site-to-Site VPN connection consists of two VPN tunnel endpoints for redundancy. For high-availability, it’s important to terminate a VPN tunnel to both of the endpoints.  

* Each tunnel terminates in a different Availability zone within the AWS global network, but must also terminate on the same equipment on-premises.  It’s also important that you have a similar highly-available configuration set up at the on-premises equipment and terminate the VPN on two different physical devices in your data center.

* AWS Site-to-Site VPN supports terminating IPSEC tunnels to both virtual private gateway and AWS Transit Gateway at the AWS end. When terminating a VPN on a virtual private gateway, you can access the VPC that the gateway is attached to. 

* For every other VPC that you want to connect to, you must create a separate VPN tunnel to a separate virtual private gateway attached to that VPC. With AWS Transit Gateway you get connectivity to thousands of VPCs over a pair of VPN tunnels. 

* Additionally, Transit Gateway supports Equal Cost Multipath (ECMP routing strategy, allowing you to load balance traffic across multiple VPN tunnels for high-availability and bandwidth aggregation.

* When leveraging Transit Gateway, you can optionally enable acceleration for your Site-to-Site VPN connection. An accelerated Site-to-Site VPN connection uses AWS Global Accelerator to route traffic from your on-premises network to an AWS edge location that is closest to your customer gateway device. 

* AWS Global Accelerator optimizes the network path, using the congestion-free AWS global network to route traffic to the endpoint that provides the best application performance.

* When using Site-to-Site VPN you are charged for each VPN connection-hour that your VPN connection is provisioned and available. Data transfer out on AWS Site-to-Site VPN incurs data transfer out charges. For more information, refer to the EC2 On-Demand pricing page. 

* When using Transit Gateway, in addition to the AWS Site-to-Site VPN costs, you also pay for transit gateway VPN attachment. For more information, refer to AWS Transit Gateway pricing. 

* To summarize, terminating VPN at TGW gives you a lot more flexibility into the number of VPCs you can connect to over single tunnel and provides added functionality like ECMP, accelerated VPN and hence is a recommended default starting point for your architectures. 

* That being said, for some unique use cases involving large data transfers, leveraging the VGW termination endpoint can be cheaper and hence can be a viable alternative.

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645285961479/AaU0po_3T.png)
*Site-to-Site VPN reference architecture* 

# Designing a reliable, dedicated hybrid networking setup using fiber connectivity for critical workloads

* Network latency over the internet varies because it is constantly changing how data gets from point A to point B. While accelerated VPN helps with network latency, the last mile from an AWS edge location to your data center is still over the internet. 

* To achieve consistent end-to-end network performance, you can leverage AWS Direct Connect to enable consistent, low latency, high-bandwidth dedicated fiber connectivity between your data centers and AWS. 

* Direct Connect provides dedicated connections at bandwidths of 1 Gbps, 10 Gbps, and 100 Gbps. Hosted connections are provided by AWS Direct Connect Partners using pre-established network links between themselves and AWS and are available from 50 Mbps up to 10 Gbps.

* AWS Direct Connect is available at over 100 locations around the world. Building highly resilient, fault-tolerant connections are key to a well-architected system when connecting to AWS Direct Connect locations. 

* AWS recommends connecting from multiple data centers for physical location redundancy as well as establishing multiple connections at a direct connect location for device redundancy. When designing WAN connections, investigate using redundant hardware and telecommunications providers with redundant paths. 

* A best practice is to use dynamic routing with Direct Connect, Active/Active connections for automatic load balancing, and failover across redundant network connections. 

* Additionally, provision sufficient network capacity to ensure that the failure of one network connection does not overwhelm and degrade redundant connections. To achieve the best availability, you can leverage the resiliency architecture for AWS Direct Connect as shown in the following diagram.

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645285963447/GlnkIyRcQ.png)
*AWS Direct Connect maximum resiliency architecture*

* If you need more than 100 Gbps of bandwidth, you can provision a link aggregation group (LAG) bundle with AWS Direct Connect. A LAG is a logical interface that uses the Link Aggregation Control protocol (LACP) to aggregate multiple connections at a single Direct Connect location, allowing you to treat them as a single, managed connection. 

* You can have a maximum of two 100G connections, or four connections with a port speed less than 100G in a LAG. You can create a LAG from existing connections, or you can provision new connections. However, a LAG only includes ports on the same AWS device. 

* AWS doesn’t support multi-chassis LAG, this means all of your Direct Connect connections terminate on the same hardware on the AWS side. A LAG is not recommended for a high-availability strategy.

* Once the physical connectivity is established at the Direct Connect location, you can create virtual interfaces which are logical connections on top of physical Direct Connect connections that enable access to AWS resources. These virtual interfaces are tagged with 802.1Q VLANs and require the use of Border Gateway Protocol (BGP).

* AWS Direct Connect provides the following virtual interfaces:


* **Public virtual interfaces** – Provide global connectivity to public AWS resources, including AWS

   public service endpoints public Amazon EC2 IP addresses, and public Elastic Load Balancing addresses.

 * **Private virtual interfaces** – Provide connectivity to the private IP range of your VPC.

   When you use private virtual interfaces, your VPC becomes a logical layer-3 extension of your network. For information about pricing, refer to the AWS Direct Connect pricing.

 * **Transit virtual interfaces** – Enables connectivity to Transit Gateway(s). While this virtual interface enables scaling you pay for the cost associated with AWS Direct Connect and Transit Gateway. For more information about pricing, refer to the AWS Transit Gateway pricing.

   * In the digital world of today, most customers are establishing global presence. There is a need to deploy resources within a large number of VPCs across multiple AWS Regions and connect to them from datacenters spread across geographies. 

   * By leveraging AWS Direct Connect gateway, a global resource that allows you to use your Direct Connect connections to connect to resources in VPCs in most AWS Regions, you can more easily connect to your resources. 

   * To enable global connectivity, you can create and associate a Direct Connect gateway with the virtual private gateway of the VPC you want connectivity into, and then create a private virtual interface to the Direct Connect gateway. 

   * You can associate up to 10 virtual private gateways (each attached to a VPC) in different AWS Regions, directly to a Direct Connect gateway. Alternatively, you can create a transit virtual interface and attach a total of three transit gateways (each attached to thousands of VPCs) across different AWS Regions to a Direct Connect gateway.

   * For standard use cases, we recommend starting with a transit virtual interface. However, if your data transfer volume is high, for example on-premises data backup to a VPC, or if you have 100 Gbps connections and want full 100 Gbps bandwidth to a VPC, we recommend using a private virtual interface. Additionally, you can use a hybrid approach for multiple use cases, as showing in the following diagram.

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645285965749/-6X1Y8G9q.png)
*Global Direct Connect reference architecture – high resiliency*  

---

Hope this guide gives you an Introduction to General design principles for hybrid networking.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.

{% user aditmodi %}

---

[Reference Guide](https://docs.aws.amazon.com/wellarchitected/latest/hybrid-networking-lens/general-design-principles-for-hybrid-networking.html)
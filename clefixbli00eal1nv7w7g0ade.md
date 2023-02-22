# How I passed the AWS Certified Advanced Networking — Specialty Exam (ANS-C01)

## Introduction

- I recently passed the AWS Certified Advanced Networking — Specialty Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Advanced Networking - Specialty is intended for individuals who perform complex networking tasks with five years of hands-on experience architecting and implementing network solutions.

- Earning AWS Certified Advanced Networking - Specialty validates expertise in designing and maintaining network architecture for the breadth of AWS services. This certification helps organizations identify and develop talent with critical skills for implementing cloud initiatives.

- This article gives you an overview which content and training material I used to prepare myself for the AWS ANS-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- Professional experience using AWS technology, AWS security best practices, AWS storage options and their underlying consistency models, and AWS networking nuances and how they relate to the integration of AWS services.
- Knowledge of advanced networking architectures and interconnectivity options [e.g., IP VPN, multiprotocol label switching (MPLS), virtual private LAN service (VPLS)].
- Familiarity with the development of automation scripts and tools. This should include the design, implementation, and optimization of the following: Routing architectures (including static and dynamic); multi-region solutions for a global enterprise; highly available connectivity solutions (e.g., AWS Direct Connect, VPN).
- Knowledge of CIDR and sub-netting (IPv4 and IPv6); IPv6 transition challenges; and generic solutions for network security features, including AWS WAF, intrusion detection systems (IDS), intrusion prevention systems (IPS), DDoS protection, and economic denial of service/sustainability (EDoS).

## Exam overview

**Level:** Specialty

**Length:** 170 minutes to complete the exam

**Cost:** 300 USD

Visit[Exam pricing](https://aws.amazon.com/certification/policies/before-testing/#Exam_pricing) for additional cost information.

**Format:** 65 questions, either multiple choice or multiple response

**Delivery method:** Pearson VUE and PSI; testing center or online proctored exam

## Exam Outline

- This exam guide includes weightings, test domains, and objectives for the exam. It is not a comprehensive listing of the content on the exam. However, additional context for each of the objectives is available to help guide your preparation for the exam. 

- The following table lists the main content domains and their
weightings. The table precedes the complete exam content outline, which includes the additional context. The percentage in each domain represents only scored content.


| Domain  | % of Exam |
| ------- | --------- |
|Domain 1: Network Design |30%|
|Domain 2: Network Implementation |26%|
|Domain 3: Network Management and Operation |20%|
|Domain 4: Network Security, Compliance, and Governance |24%|
|TOTAL |100%|

**Domain 1: Network Design**
1.1: Design a solution that incorporates edge network services to optimize user performance and traffic management for global architectures.
• Design patterns for the usage of content distribution networks (for example, Amazon CloudFront)
• Design patterns for global traffic management (for example, AWS Global Accelerator)
• Integration patterns for content distribution networks and global traffic management with other services (for example, Elastic Load Balancing, Amazon API Gateway)
1.2: Design DNS solutions that meet public, private, and hybrid requirements.
• DNS protocol (for example, DNS records, timers, DNSSEC, DNS delegation, zones)
• DNS logging and monitoring
• Amazon Route 53 features (for example, alias records, traffic policies, resolvers, health checks)
• Integration of Route 53 with other AWS networking services (for example, Amazon VPC)
• Integration of Route 53 with hybrid, multi-account, and multi-Region options
• Domain registration
1.3: Design solutions that integrate load balancing to meet high availability, scalability, and security requirements.
• How load balancing works at layer 3, layer 4, and layer 7 of the OSI model
• Different types of load balancers and how they meet requirements for network design, high availability, and security
• Connectivity patterns that apply to load balancing based on the use case (for example, internal load balancers, external load balancers)
• Scaling factors for load balancers
• Integrations of load balancers and other AWS services (for example, Global Accelerator, CloudFront, AWS WAF, Route 53, Amazon Elastic Kubernetes Service [Amazon EKS], AWS Certificate Manager [ACM])
• Configuration options for load balancers (for example, proxy protocol, cross-zone load balancing, session affinity [sticky sessions], routing algorithms)
• Configuration options for load balancer target groups (for example, TCP, GENEVE, IP compared with instance)
• AWS Load Balancer Controller for Kubernetes clusters
• Considerations for encryption and authentication with load balancers (for example, TLS termination, TLS passthrough)
1.4: Define logging and monitoring requirements across AWS and hybrid networks.
• Amazon CloudWatch metrics, agents, logs, alarms, dashboards, and insights in AWS architectures to provide visibility
• AWS Transit Gateway Network Manager in architectures to provide visibility
• VPC Reachability Analyzer in architectures to provide visibility
• Flow logs and traffic mirroring in architecture to provide visibility
• Access logging (for example, load balancers, CloudFront)

1.5: Design a routing strategy and connectivity architecture between on-premises networks and the AWS Cloud.
• Routing fundamentals (for example, dynamic compared with static, BGP)
• Layer 1 and layer 2 concepts for physical interconnects (for example, VLAN, link aggregation group [LAG], optics, jumbo frames)
• Encapsulation and encryption technologies (for example, Generic Routing Encapsulation [GRE], IPsec)
• Resource sharing across AWS accounts
• Overlay networks

1.6: Design a routing strategy and connectivity architecture that include multiple AWS
accounts, AWS Regions, and VPCs to support different connectivity patterns.
• Different connectivity patterns and use cases (for example, VPC peering, Transit Gateway, AWS PrivateLink)
• Capabilities and advantages of VPC sharing
• IP subnets and solutions accounting for IP address overlaps

**Domain 2: Network Implementation**
2.1: Implement routing and connectivity between on-premises networks and the AWS Cloud.
• Routing protocols (for example, static, dynamic)
• VPNs (for example, security, accelerated VPN)
• Layer 1 and types of hardware to use (for example, Letter of Authorization [LOA] documents, colocation facilities, Direct Connect)
• Layer 2 and layer 3 (for example, VLANs, IP addressing, gateways, routing, switching)
• Traffic management and SD-WAN (for example, Transit Gateway Connect)
• DNS (for example, conditional forwarding, hosted zones, resolvers)
• Security appliances (for example, firewalls)
• Load balancing (for example, layer 4 compared with layer 7, reverse proxies, layer 3)
• Infrastructure automation
• AWS Organizations and AWS Resource Access Manager (AWS RAM) (for example, multiaccount Transit Gateway, Direct Connect, Amazon VPC, Route 53)

• Test connectivity (for example, Route Analyzer, Reachability Analyzer)
• Networking services of VPCs

2.2: Implement routing and connectivity across multiple AWS accounts, Regions, and VPCs to support different connectivity patterns.

• Inter-VPC and multi-account connectivity (for example, VPC peering, Transit Gateway, VPN, third-party vendors, SD-WAN, multiprotocol label switching [MPLS])
• Private application connectivity (for example, PrivateLink)
• Methods of expanding AWS networking connectivity (for example, Organizations, AWS RAM)
• Host and service name resolution for applications and clients (for example, DNS)
• Infrastructure automation
• Authentication and authorization (for example, SAML, Active Directory)
• Security (for example, security groups, network ACLs, AWS Network Firewall)
• Test connectivity (for example, Route Analyzer, Reachability Analyzer, tooling)

2.3: Implement complex hybrid and multi-account DNS architectures.
• When to use private hosted zones and public hosted zones
• Methods to alter traffic management (for example, based on latency, geography, weighting)
• DNS delegation and forwarding (for example, conditional forwarding)
• Different DNS record types (for example, A, AAAA, TXT, pointer records, alias records)
• DNSSEC

How to share DNS services between accounts (for example, AWS RAM)
• Requirements and implementation options for outbound and inbound endpoints

2.4: Automate and configure network infrastructure.
• Infrastructure as code (IaC) (for example, AWS Cloud Development Kit [AWS CDK], AWS CloudFormation, AWS CLI, AWS SDK, APIs)
• Event-driven network automation
• Common problems of using hardcoded instructions in IaC templates when provisioning cloud networking resources

**Domain 3: Network Management and Operations**

3.1: Maintain routing and connectivity on AWS and hybrid networks

• Industry-standard routing protocols that are used in AWS hybrid networks (for example, BGP over Direct Connect)
• Connectivity methods for AWS and hybrid networks (for example, Direct Connect gateway, Transit Gateway, VIFs)
• How limits and quotas affect AWS networking services (for example, bandwidth limits, route limits)
• Available private and public access methods for custom services (for example, PrivateLink, VPC peering)
• Available inter-Regional and intra-Regional communication patterns

3.2: Monitor and analyze network traffic to troubleshoot and optimize connectivity patterns.

• Network performance metrics and reachability constraints (for example, routing, packet size)
• Appropriate logs and metrics to assess network performance and reachability issues (for example, packet loss)
• Tools to collect and analyze logs and metrics (for example, CloudWatch, VPC Flow Logs, VPC Traffic Mirroring)
• Tools to analyze routing patterns and issues (for example, Reachability Analyzer, Transit Gateway Network Manager)

3.3: Optimize AWS networks for performance, reliability, and cost-effectiveness.

• Situations in which a VPC peer or a transit gateway are appropriate
• Different methods to reduce bandwidth utilization (for example, unicast compared with multicast, CloudFront)
• Cost-effective connectivity options for data transfer between a VPC and on-premises environments
• Different types of network interfaces on AWS
• High-availability features in Route 53 (for example, DNS load balancing using health checks with latency and weighted record sets)

• Availability of options from Route 53 that provide reliability
• Load balancing and traffic distribution patterns
• VPC subnet optimization
• Frame size optimization for bandwidth across different connection types

**Domain 4: Network Security, Compliance, and Governance**
4.1: Implement and maintain network features to meet security and compliance needs and requirements.
• Different threat models based on application architecture
• Common security threats
• Mechanisms to secure different application flows
• AWS network architecture that meets security and compliance requirements

4.2: Validate and audit security by using network monitoring and logging services.
• Network monitoring and logging services that are available in AWS (for example, CloudWatch, AWS CloudTrail, VPC Traffic Mirroring, VPC Flow Logs, Transit Gateway Network Manager)
• Alert mechanisms (for example, CloudWatch alarms)
• Log creation in different AWS services (for example, VPC flow logs, load balancer access logs, CloudFront access logs)
• Log delivery mechanisms (for example, Amazon Kinesis, Route 53, CloudWatch)
• Mechanisms to audit network security configurations (for example, security groups, AWS Firewall Manager, AWS Trusted Advisor)

4.3: Implement and maintain confidentiality of data and communications of the network.
• Network encryption options that are available on AWS
• VPN connectivity over Direct Connect
• Encryption methods for data in transit (for example, IPsec)
• Network encryption under the AWS shared responsibility model
• Security methods for DNS communications (for example, DNSSEC)

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-advnetworking-spec/AWS-Certified-Advanced-Networking-Specialty_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “AWS Certified Advanced Networking - Specialty” by Zeal Vora which is a very good course and covers all the most important aspects of the AWS and helps you Understand and Implement various complex AWS networking architectures. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-certified-advanced-networking-specialty/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉some of the AWS networking related projects I implemented are shared [here](https://github.com/Cloud-Tech-Projects).some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi). 

3)**📋 AWS Ramp-Up Guides:** Your guides to learning the AWS Cloud.

- AWS Ramp-Up Guides offer a variety of resources to help you build your skills and knowledge of the AWS Cloud. Each guide features carefully selected digital training, classroom courses, videos, whitepapers, certifications, and more. Explore the guides below by role, solution, or industry area.

👉 more details can be found [here](https://aws.amazon.com/training/ramp-up-guides/)

👉 focus on Networking & Content Delivery related [solution guide](https://d1.awsstatic.com/training-and-certification/ramp-up_guides/Ramp-Up_Guide_Networking-Content-Delivery.pdf)

4) **🤝 being part of a Study Groups:** I also recommend you to be part of a study groups. it helps you stay focused, probably having study groups with people studying for the same exam is an added benefit.

Study Groups I was part of:

- Cloud and DevOps Babies:

Cloud and DevOps Babies are a global group of babies with curious minds to learn/decode Cloud, DevOps and Microservices tech stacks. 

👉 more details can be found [here](https://www.linkedin.com/company/devopsandcloudbabies/)

- Tech Study Slack: 
TechStudySlack is a Slack for people studying Tech

👉 more details can be found [here](https://techstudyslack.com/)

5)**✍️ practice tests:** Lastly I recommend all of you to pass these practice exams before attending the real exam. It provides simulated questions that are very similar to the actual exam.

One of the selling points of this practice exam is that each question contains detailed explanations that will help you gain a deeper understanding of AWS services. It not just explains what the correct answer is, but also explains why other answers are wrong. It is extremely helpful to make you recognize the difference between similar services.

- Tutorials Dojo Practice Exams:

👉 more details can be found [here](https://portal.tutorialsdojo.com/courses/aws-certified-advanced-networking-specialty-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Zeal Vora’s AWS Certified Advanced Networking - Specialty Course](https://www.udemy.com/course/aws-certified-advanced-networking-specialty/)

- [Stephane Mareek and Chetan Agarwal’s AWS Certified Advanced Networking Specialty Course](https://www.udemy.com/course/aws-certified-advanced-networking-specialty-ans/)

- [Andrian Cantrill's AWS Certified Advanced Networking - Specialty Course](https://learn.cantrill.io/p/aws-certified-advanced-networking-specialty)

- [Ervin Szilágyi’s AWS-ANS-C01 Study Guide Github Repo](https://github.com/Ernyoke/certified-aws-advanced-networking-specialty)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪
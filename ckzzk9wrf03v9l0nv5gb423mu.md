## 10 best practices to build a secure IoT deployment

* IoT implementations can have some very unique challenges not present in traditional IT deployments. For example, deploying a consumer IoT device, such as what iRobot has done using AWS to handle scale and spikes, can introduce a new classification of threats to be addressed. Industrial deployments of IoT (IIoT) devices (such as how SKF and Volkswagen have used AWS IoT to optimize its production processes, reduce costs, and provide a better experience to its customers) offer another unique set of security considerations. 

* Lastly, operational technology (OT) or SCADA-based IoT deployments, such as Enel using AWS IoT to get electricity to their customers can require more thought around reliability and anomaly detection. And this is not an exhaustive list.

* For these use cases there are some common security best practices that can be addressed using AWS services. How enterprises choose to invest in each of these will be based on their risk model.

## The following are 10 best practices to build a secure IoT deployment.




### 1.Conduct a formal security risk assessment using a common framework (such as MITRE ATT&CK). Use this to inform system design.

* Whether you’re deploying consumer devices, industrial workloads, or operational technologies, it is important to first evaluate the risks and threats associated with your deployment. For example, one common threat to IoT devices listed in the MITRE ATT&CK framework is a Network Denial of Service (T1498). 

* A denial-of-service (DoS) attack against an IoT device can be defined as disallowing status or command and control communication to and from an IoT device and its controllers. 

* In the case of a consumer IoT device, such as a smart bulb, not having the ability to communicate status or receive updates from a central control place could create problems, but would likely not necessarily have dramatic consequences. 

* However, in an OT system managing a water treatment facility, losing the ability to receive commands to open or shut key valves could create a larger impact to people and the environment. 

* So, it’s important to look at the impact of various common threats, how they apply to different IoT use cases, and ways to mitigate them. Key steps include:
 * Identify, manage, and track gaps and vulnerabilities. Create and maintain an up- to-date threat model that can be monitored against.
 * Segment systems based on their risk assessment. Some IoT and IT systems may share the same risks, so use a predefined zoning model with appropriate controls between them.
 * Follow a micro segmentation approach to isolate the impact of an event.
 * Use appropriate security mechanisms to control information flow between network segments.
 * Regularly identify and review security event minimization opportunities as your IoT system evolves.

#### Supporting AWS resources

* When building your environment inside of AWS, foundational services such as Amazon Virtual Private Cloud (VPC), VPC security groups (SGs), and network access control lists (network ACLs) should be used to implement the micro segmentation. 

* AWS recommends using multiple accounts, which helps to isolate IoT applications, data, and business processes across your environment and use AWS Organizations for better manageability and centralized insight. Additional information can be found in the security pillar of AWS Well-Architected Framework and Organizing Your AWS Environment Using Multiple Accounts whitepaper.

**2.Maintain an asset inventory of all IoT assets, including IT assets required to maintain IoT operations. Categorize them by safety, criticality, ability to patch, and other actionable criteria.**

* A critical aspect of a good security program is having visibility into your system. It’s also important that you create visibility with actionable outcomes in mind, so you can automate operations and maintenance of these devices after deployment.
 * Create and maintain an asset inventory for all IoT assets along with their major characteristics that you may want to action upon. This includes things such as deployed certificates and software or hardware versions.
 * Segment devices into categories or apply appropriate tags to be able to manage them programmatically. Focus on actionable data such criticality of the devices, location, whether the device can or should be updated, or important contact and owner information.

#### Supporting AWS resources

* AWS provides the following services to help you create and maintain a connected asset inventory:
 * **AWS IoT Device Management –** For devices connected to AWS IoT.
 * **AWS Systems Manager –** For cloud and on-premises computers.
 * Security Pillar of AWS Well-Architected and IoT Lens

**3.Provision IoT devices and systems with unique identities and credentials. Apply authentication and access control mechanisms at each system interface.**

* Strong identity controls are key to operational excellence. However, IoT implementation considerations around physical control of devices range widely. Therefore, not only is it important to ensure devices receive unique identities and credentials, but also that those credentials are appropriately protected on the device, and monitoring and automated remediation plans are put in place when there’s deviation from expected standards.
 * Assign unique identities to IoT devices such as X.509 certificates to each device. Monitor that the identity does not change on devices or that certificates are not reused.
 * Create mechanisms to facilitate the generation, distribution, rotation, and revocation of credentials.
 * When appropriate, use hardware-protected modules such as TPMs for storing credentials and performing authentication operations.
 * Avoid hardcoding credentials or storing secrets that are not unique to the device on IoT devices.

#### Supporting AWS resources

* AWS provides the following assets, services, and capabilities to help you identify, sort, and secure your IoT assets:
 * Security and identity for AWS IoT
 * Device manufacturing and provisioning with X.509 certificates in AWS IoT Core – Goes over various mechanisms to securely provision identities to your IoT devices.
 * AWS Certificate Manager Private Certificate Authority – For provisioning your own certificates.
 * Amazon Cognito – A service that provides authentication, authorization, and user management for your web and mobile apps.
 * AWS Identity and Access Management (IAM) – A service that enables you to manage access to AWS services and resources securely.
 * Device authentication and authorization for AWS IoT Greengrass
 * AWS Secrets Manager – A service that can be used to securely store and manage secrets in the cloud and encrypts the secrets using AWS Key Management Service (AWS KMS).
 * AWS KMS – Allows you to easily create and control the keys used for cryptographic operations in the cloud.
 * Security Pillar of AWS Well-Architected and IoT Lens

**4.Define appropriate update mechanisms for software and firmware updates.**

*  Whether it’s deploying patches to individual packages, updating local firmware, or wholesale replacing the software on an IoT device, patching is critical during the IoT device’s lifecycle. 

* Although different use cases will have different tradeoffs, common things to consider include rolling out patches gradually to catch defects and ensuring all devices of the same type aren’t brought down simultaneously, being responsive to vulnerabilities, and ensuring the patch delivery mechanism can’t be used by unauthorized actors. Some additional considerations include:
 * Begin with having a mechanism to push software and firmware to devices in the field to patch security vulnerabilities and improve device functionality.
 * Apply and verify digital signatures on distributed deployment artifacts.
 * Verify the integrity of the software on the device before starting to run it ensuring that it comes from a reliable source (signed by the vendor) and that it is obtained in a secure manner.
 * Monitor status of deployments throughout your ecosystem and investigate any failed or stalled deployments.
 * Use rolling patches using asset tags or other segmentation mechanism based on the impact of a latent issue.
 * Include patch status in your inventory of the deployed devices.
 * Use version control mechanisms to prevent unauthorized actors from forcing firmware or software downgrades.
 * Maintain notification mechanisms to immediately alert the appropriate stakeholders when security updates are required or fail.
 * Create mechanisms to identify, isolate into a different network segment, or replace IoT devices that are outside of compliance.
 * Create detection and response mechanisms to handle unauthorized changes in deployed software or firmware.

#### Supporting AWS resources

* AWS provides the following capabilities and services to help you organize and maintain a continuous development and deployment pipeline:
 * FreeRTOS over-the-air updates
 * OTA updates of AWS IoT Greengrass Core software
 * AWS IoT Jobs – Defines a set of remote operations that you send to and run on one or more devices connected to AWS IoT. 
 * AWS Systems Manager Patch Manager – Automates the process of patching managed instances with both security related and other types of updates, such as operating systems and applications.
 * Security Pillar of AWS Well-Architected and IoT Lens

**5.Encrypt persistent data at rest.**

* For devices such as sensors or cameras, information stored on deployed devices may seem innocuous, but when physical control of a device is not guaranteed that information can be a target for unauthorized actors. Whether in the consumer space like cached videos on cameras, industrial application with proprietary machine learning (ML) models, or even some configuration data for operational environments, the best course of action is to encrypt all data (even transitive data) stored at rest when possible. Some additional considerations include:
 * Identify and classify data collected throughout your IoT ecosystem and learn their corresponding business use case.
 * Categorize data based on the earlier risk analysis, including impact to other stakeholders.
 * Identify opportunities to stop collecting unused data or reducing granularity and retention time, then implement improvements.
 * Ensure integrity of data used to operate devices through cryptographic mechanisms.
 * Apply access controls using least privilege principle to encryption keys, and monitor and audit data access.
 * When necessary, follow least privilege and need-to-know principles when granting access to third parties.
 * Consider privacy and transparency expectations of your customers and corresponding legal requirements.

#### Supporting AWS resources

* AWS provides the following assets and services to help you secure IoT data at the edge and cloud:
 * AWS Shared Responsibility Model – For security and compliance.
 * AWS Data Privacy
 * AWS Privacy Notice
 * AWS Compliance programs and offerings
 * AWS Compliance Solutions Guide
 * AWS Key Management Service (AWS KMS) – Can be used to create and control the keys used for cryptographic operations in the cloud.
 * Security Pillar of AWS Well-Architected and IoT Lens

**6.Encrypt all data in transit, including sensor and device data, administration, provisioning, and deployments.**

* Nearly all modern IoT devices have the power to perform encryption of network traffic, so take advantage of that and protect both the data plane and control plane communications. This not only ensures confidentiality of the data, but also the integrity of monitoring signals. 

* For protocols that can’t be encrypted, consider if a second device closer to the IoT asset can accept the communication and convert it to something more secure to then send outside the local perimeter. Some additional considerations include:
 * Protect the confidentiality and integrity of inbound and outbound network communication channels that you use for data transfers, monitoring, administration, provisioning, and deployments by selecting modern internet native cryptographic network protocols.
 * If possible, limit the number of protocols implemented within a given environment and disable default network services that are unused.
 * If over-the-air updates are implemented, network-related vulnerabilities that affect the integrity of the over-the-air process should be addressed first.
 * If possible, implement mechanisms to identify when an insecure network environment is being used. For example, if the certificate used for TLS encryption doesn’t match a known certificate on the device such as in a man-in-the-middle event.

#### Supporting AWS resources
 
* AWS provides the following assets, capabilities, and services to help you encrypt your networks:
 * AWS IoT SDKs – Help you securely and quickly connect your devices to AWS IoT.
 * FreeRTOS libraries – Provide additional functionality to the FreeRTOS kernel and its internal libraries.
 * AWS Certificate Manager Private Certificate Authority – Provision your own certificates.
 * Security best practices for AWS IoT SiteWise
 * Security Pillar of AWS Well-Architected and IoT Lens

**7.Secure both the IoT environment and supporting IT environments to the same level of criticality following a well-documented standard. This is especially true for gateways that serve as boundaries between systems.**

* Often, IoT systems still have a dependency on traditional IT systems to operate. Whether that’s for identity and authorization, billing, monitoring and remediation, or maintenance, having these systems become unavailable to the IoT system can cause cascading failures. 

* Therefore, you should use the risk assessment and asset inventory to document these critical dependencies and architect all relevant systems to the same level of resiliency and security. Some ways to do this include:
 * Plan and manage security lifecycle of devices.
 * Consistently harden internet-connected network resources such as edge gateways.
 * Avoid hardcoding or storing credentials and secrets locally on devices.
 * Use device certificates and temporary credentials instead of long-term credentials to access AWS cloud services.
 * Limit the number of listening ports on IoT devices, and ensure access only from authorized systems.
 * Create allow lists for access with a management mechanism similar to that of software updates. 
 * Disable unused sensors, actuators, services, or software on the IoT device.
 * Establish secure connections to cloud services, and monitor these connections.

#### Supporting AWS resources

* AWS provides the following assets, capabilities, and services to help secure cloud connected network resources and securely manage on-premises computing resources:
 * NIST Guide to General Server Security – For general guidance on security devices (such as edge gateways).
 * AWS IoT Greengrass hardware security
 * Working with secrets at the Edge.
 * AWS IoT SiteWise Gateway – Securely configuring edge gateways.
 * AWS Systems Manager – Provides you with a centralized and consistent way to gather operational insights and carry out routine management tasks.
 * AWS IoT Device Management – A service that allows you to securely register, organize, monitor, and remotely manage IIoT devices at scale throughout their lifecycle.
 * AWS IoT secure tunneling – Accesses IIoT devices behind restricted firewalls at remote sites for troubleshooting, configuration updates, and other operational tasks.
 * Plant network to Amazon Virtual Private Cloud connectivity options
 * AWS IoT Greengrass - Connect on port 443 or through a network proxy
 * Security Pillar of AWS Well-Architected and IoT Lens

**8.Deploy security auditing and monitoring mechanisms across your IoT environment and relevant IT systems.**

* As we’ve discussed, it’s important to ensure the proper configuration of IoT devices when they are put into production and that they are updated. But, it’s also important to monitor their behavior and security posture on an ongoing basis.
 * Deploy auditing and monitoring mechanisms to continuously collect and report activity metrics and logs. 
 * Monitor on-device and related off-device activities such as network traffic and entry points, process implementation, and system interactions for any unexpected behavior.
 * Continuously check that your security controls and systems are intact by explicitly testing them.
 * Implement a monitoring solution to create a traffic baseline, and monitor anomalies and adherence to the baseline.
 * Collect security logs and analyze them in real time using automated tooling.
 * Monitor availability of your IoT devices in real time, where technically feasible.

#### Supporting AWS resources

* AWS provides the following capabilities and services to help you monitor your security at varying levels:
 * AWS IoT Device Defender – Monitors and audits your fleet of IoT devices.
 * Monitor AWS IoT with CloudWatch Logs – Centralizes the logs from all of your systems, applications, and AWS services that you use, in a single, highly scalable service.
 * Log AWS IoT API Calls with AWS CloudTrail – Provides a record of actions taken by a user, a role, or an AWS service in AWS IoT.
 * Monitoring with AWS IoT Greengrass Logs
 * AWS Config – Assess, audit, and evaluate the configurations of your AWS resources.
 * Amazon GuardDuty – Continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts and workloads.
 * AWS Security Hub – Automates AWS security checks and centralizes security alerts.
 * Security Pillar of AWS Well-Architected and IoT Lens
 
**9.Create incident response playbooks, and build automation as your security response matures.**

* Management systems must build continuous health checks before the devices get shipped. It’s also important to create incident response playbooks for when those checks identify anomalies, and, as processes mature, automate the containing of events and returning to a known good state. 

* Although it may seem daunting, this doesn’t have to happen all at the same time. This is a process that will continue throughout the lifecycle of the IoT environment, with the complexity and maturity of the program growing over time.
Maintain and regularly exercise a security incident response plan to test monitoring functionality.
 * Collect security logs and analyze them in real time using automated tooling. Build playbooks in response to unexpected findings.
 * Create an incident response playbook with clearly understood roles and responsibilities.
 * Test incident response procedures on a periodic basis.
 * As procedures become more stable, automate their implementation but maintain human interaction. As the automated procedures are validated, automate what triggers their implementation.

#### Supporting AWS resources

* AWS provides the following assets and services to help you monitor your security and create incident response playbooks:
 * AWS Security Incident Response Guide
 * AWS Systems Manager – Provides a centralized and consistent way to gather operational insights and carry out routine management tasks.
 * Security Pillar of AWS Well-Architected and IoT Lens

**10.Create and test business continuity and recovery plans.**

* During an event, different IoT systems could behave in different ways. Before those events occur, you must define parameters relevant to your use case (should a system fail open or fail shut, does the system attempt recovery automatically or require human
intervention, do you need to enable or disable manual controls) and then test those rigorously. 

* Again, use the risk assessment and criticality assignments performed early in this process to ensure you apply the right amount of scrutiny and resources to this phase. Don’t forget about defining when to return to the baseline state in your recovery plans.
 * Define important parameters (such as overall availability) for your stakeholders.
 * Define the resilience requirements for the system and analyze failure modes to ensure adherence.
 * Test recovery plans periodically and adapt them according to lessons learned from tests and actual security incidents.
 * Perform threat and risk assessment of supporting IT systems and develop written procedures on how to return to the normal, well-defined, state of operation tailored to the assessment’s results.
 * Include third-party aspects (such as network communications, software, and support).
 * Use resiliency features at the edge to support data resiliency and backup needs.
 * Use cloud services for backup and business continuity.

#### Supporting AWS resources

* AWS provides the following assets and services to help you create and test business continuity and recovery plans:
 * AWS IoT Lens for AWS Well-Architected Framework – A document that covers commonly encountered IoT use cases and identified key solution elements to ensure that your workload architecture uses established best practices.
•	Resilience in AWS IoT Greengrass
•	Backup and Restore Use Cases with AWS
•	CloudEndure Disaster Recovery
•	AWS Backup
 
* These general best practices apply across all IoT deployments, but as mentioned previously, different industries will have different threat and risk models. In the next section we will dive into examples across these industries and demonstrate prescriptive approaches that are more targeted.

---

Hope this guide helps you understand the 10 best practices to build a secure IoT deployment.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.

{% user aditmodi %}

[Reference Notes](https://d1.awsstatic.com/whitepapers/Security/Securing_IoT_with_AWS.pdf?did=wp_card&trk=wp_card)

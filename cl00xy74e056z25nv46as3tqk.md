## Disaster Recovery of On-Premises Applications to AWS | AWS White Paper Summary

* This whitepaper outlines the best practices for planning, implementing, and maintaining disaster recovery for on-premises applications using AWS. It lays out the differences between disaster recovery and other resilience strategies, and describes the steps of building a disaster recovery plan. It also offers different approaches for mitigating risks and meeting recovery time objectives (RTO), and meeting recovery point objectives (RPO) by using AWS as the disaster recovery site.

# Introduction

**What is disaster recovery?**

* Disaster recovery is the process of preparing for and recovering from a disruptive event.

**What is a disaster?**

* In the context of a company’s IT environment, a disaster is an event that partially or completely disrupts the operations of one or more applications. A disaster normally requires human intervention to fail over to secondary copies of applications in order to maintain their functionality.

* The four main categories of a disaster:

 * **Human errors** – Unintentional actions leading to a security breach such as inadvertent misconfiguration of the software or a database

 * **Malicious attacks** – Unauthorized actions that affect a victim’s system such as a denial-of-service (DoS) or ransomware attack

 * **Natural disasters** – Environmental factors that cause a system failure such as earthquakes or floods

 * **Technical failures** – A malfunction of software, hardware, or a facility such as a power failure or a network connectivity failure

* There are several factors that need to be considered when planning your response to a specific disaster:

 * **Expected duration of the disaster** – How soon will the application recover and how likely is the disaster to resolve on its own?

 * **Size of impact (also known as blast radius)** – Which applications are affected and to what extent is their functionality impaired?

 * **Geographic impact** – May be regional, national, continental, or global.

 * **Tolerance of downtime** – How significant is the impact of the application not functioning?

**Why disaster recovery?**

* A properly planned and implemented disaster recovery solution helps mitigate the following issues that can be caused by a disaster:

 * **Direct and indirect financial loss** – The impact of direct financial loss is mostly relevant for applications that are critical for any revenue-generating processes. For example, external-facing IT systems that are provided to customers for a fee or internal IT systems that process data relevant for revenue generation. Indirect financial loss includes, for example, customers switching to a competing product and the cost of work needed to resume normal operation after the disaster is over.

 * **Reputational damage** – In addition to financial loss as described previously, downtime caused by unexpected incidents can significantly harm a company’s reputation. A short recovery period aided by a disaster recovery solution can help avoid irreversible damage to the corporate image.

 * **Failure to abide by compliance standards** – Multiple compliance standards, including System and Organization Controls (SOC), the Payment Card Industry (PCI) Data Security Standard, and the Health Insurance Portability and Accountability Act (HIPAA), require a disaster recovery plan. Some standards even add very specific requirements, such as minimal physical distance between the source site and the disaster recovery site.

## How does disaster recovery help business continuity?

* Disaster recovery is a component of the overall business continuity strategy of an organization. Business continuity is the ability of the organization and all the supporting applications to run critical business functions at all times, including during emergency events.

* To achieve business continuity, you must implement various types of resilience mechanisms. Resilience is the ability of an application to recover from an outage, either automatically or with human intervention.

* Disaster recovery (sometimes called business continuity/disaster recovery, BC/DR, or DR) is an important part of your resilience strategy and determines how you respond when a disaster strikes. This response varies between applications and should be based on your organization's business objectives for each application. These objectives should specify (among other things) the strategy for minimizing loss of data and reducing downtime when your applications are not available for use. This approach helps your organization maintain operations as part of business continuity planning (BCP).

## Recovery objectives

* As part of disaster recovery planning, you need to define a recovery time objective (RTO) and recovery point objective (RPO) for each application based on impact analysis and risk assessment.

* **Recovery time objective (RTO)** is the maximum acceptable delay between the interruption of an application and the restoration of its service. This objective determines what is considered an acceptable time window for an application to be unavailable.

* **Recovery point objective (RPO)** is the maximum acceptable gap between the data in the disaster recovery site and the latest data stored in the application when the disaster strikes. This objective determines what is considered the maximum amount of time acceptable for interruption/loss of data that can be caused by a disaster.

![Image description](https://cdn.hashnode.com/res/hashnode/image/upload/v1645704461921/G19FbY-xH.png)
*Recovery objectives*

RTO and RPO for each application depend on many factors (such as service level agreements (SLA) and external compliance requirements), but there are some common standards. Common figures for mission-critical applications (tier-1 applications) include an RTO of 15 minutes and a near-zero RPO. For important applications that are not mission critical (tier-2 applications), the RTO is typically 4 hours and the RPO is 2 hours. For all other applications (tier-3 applications), a typical RTO is 8 to 24 hours and RPO is 4 hours. 

# Using AWS for disaster recovery of on-premises applications

## AWS services related to disaster recovery

AWS can be used as the infrastructure running your disaster recovery site. Additionally, AWS offers a number of services that can help you replicate your source applications into the AWS infrastructure and recover them in the case of a disaster.

## Shared responsibility

Disaster recovery is a shared responsibility between AWS and you, the customer. It is important that you understand how disaster recovery and availability, as part of resiliency, operate under this shared model.

## Business impact analysis and risk assessment

* One of the first steps to perform when planning the implementation of a disaster recovery solution is a business impact analysis for all relevant applications. A business impact analysis should quantify the business impact of a disruption to each application. 

* It should identify the impact your internal and external customers will face from not being able to use your applications and the effect that will have on your business with regards to cost, reputation, and compliance. 

* The analysis should help you determine how quickly the application needs to be made available (RTO) and how much data loss can be tolerated (RPO). However, recovery objectives should not be defined without also considering the likelihood of disruption and the cost of recovery when calculating the business value of providing disaster recovery for an application.

* The business impact of a disaster may not be constant. For example, the impact might be dependent on the timing of the disaster — disruption to your payroll system is likely to have a very high impact to the business just before employees are supposed to be paid, but it may have a low impact just after employees have been paid.

* With all of this information, you can document the threat, risk, and impact of different disaster scenarios and the associated recovery options. This information should be used to choose the best disaster recovery strategy and tools for each application and to match the risk and impact (financial, among other types) of a disaster for each application.

* The following table is a sample risk analysis for disaster recovery planning for determining maximum TCO.

Table 1 - Sample risk analysis for disaster recovery planning

Risk analysis for application <APPLICATION NAME>

|Disaster type	|Likelihood (per year)	|Consequences	|Risk (likelihood x consequences)|
|---------------|-----------|--------|---------|
|Weather	|10% (based on past statistics)	|$100,000	|$10,000|
|Power outage	|5% (based on past statistics)	|$150,000 (includes equipment that would need to be replaced)	|$7,500|
||||$17,500|

* After you have determined your maximum TCO, identify whether there is a disaster recovery solution that has a lower TCO than the cost estimated in the risk analysis (taking the probability of a disaster into account). If such a solution exists, then it makes sense financially to use that solution for that application.

* In addition to the financial calculations, the RTO and RPO for each application need to be considered during this process. The more critical the application is, the more aggressive its RTO and RPO requirements, resulting in a higher TCO of an overall disaster recovery solution. Therefore, solutions that don’t offer the needed RTO and RPO should not be considered, even if they make sense based on the raw financial calculation.

#### How to match a disaster recovery solution to an application

* There are a variety of disaster recovery solutions on the market. Currently available disaster recovery solutions have the following main differences:

 * **Source application support limitations** – Different disaster recovery products and services support a different subset of operating systems, central processing unit (CPU) architectures, applications, and hypervisors. For applications that are not supported by a disaster recovery solution, the solution will either not be able to correctly (if at all) copy the data to the disaster recovery site or run a working copy of that application in the disaster recovery site when needed (the latter sometimes can only be discovered during a disaster recovery drill).

 * **Target infrastructure support limitations** – Different disaster recovery solutions have different requirements for the disaster recovery site infrastructure. For example, not all target infrastructures support all of the operating systems of the source nor can they run with the required performance.

 * **RPO** – Choose a solution that provides the RPO required for the application.

 * **RTO** – Choose a solution that offers your target RTO.

 * **TCO** – Usually the more features and capabilities a disaster recovery service has, the higher its TCO will be. For example, more aggressive RTOs and RPOs would increase the TCO.

 * **Compliance** – Applications that are covered by compliance certifications normally require their disaster recovery solutions to be covered by the requirements of the certifications.

 * **Ease of operation** – Choose a solution that does not require significant effort or uncommon skills to operate and manage during normal operation (outside of drills and disasters).

 * **Ease of disaster recovery drills** – Choose a solution that reduces the cost and simplifies the process for disaster recovery drills. This will encourage you to conduct the drills more frequently and improve your readiness for disasters.

 * **Level of automation** – Disaster recovery solutions that offer a higher level of automation (for example, they have application programming interfaces (APIs) for integration with other solutions) can be used to extend their capabilities and meet more of your disaster recovery needs.

* This list of technical criteria should be used when evaluating which disaster recovery solution best meets your needs. Keep in mind that different applications (or groups of applications) are likely to have different requirements. It’s also important to pay attention to the small print and to confirm that the product will work as expected in real life scenarios.

* Lastly, as a general best practice, narrow down your disaster recovery solutions to the ones you actually need. Operationally, each new solution will have separate overhead, an added learning curve, and additional implementation and maintenance efforts (such as monitoring the solution, upgrading versions, and applying security patches). Therefore, AWS recommends selecting a small number of disaster recovery solutions to cover all the required applications.

* After you select the disaster recovery solution for each application, you need to plan the implementation project. Implementing disaster recovery for a large number of applications may be a lengthy and complex project.

* To simplify and accelerate the project, include the following in your planning process:

 * **Mapping** – Map the applications, the resources within the applications, and what needs to be recovered for the application to run. AWS recommends preparing a complete list of such resources, grouped by applications. These resources include servers, appliances (such as NAS (network-attached storage) appliances and firewalls), and networks. As a part of this mapping, dependencies within and between applications need to be established and documented.

For each application, go through the risk analysis, define the disaster recovery tier, and determine the RTO and RPO.

 * **Timeline** – For both the planning and the implementation stages, a realistic timeline needs to be defined based on the number of applications and their complexity and also the skillset and experience of the people involved in the project.

 * **People** – The number of people that will be allocated for each stage of the project (including planning and implementation), who these people are, and the skillsets, roles, and responsibilities of each person.

 * **Budget** – How much the entire project is estimated to cost. The budget should take into account the cost of the licenses of the AWS services used as well as other costs, such as data transfer from the source site to AWS.

 * **Solution** – Choose solutions for each disaster recovery tier. Since this paper focuses on using AWS for disaster recovery, the prescribed suggestions are as follows:

   * For servers, use CloudEndure Disaster Recovery. Edge cases for which CloudEndure Disaster Recovery cannot be used include unsupported operating systems, non-ACID applications (such as MyISAM-backed MySQL databases), applications using shared storage that multiple nodes write to in parallel (such as Oracle RAC), systems that have distributed databases with multiple nodes that need to be in sync with each other (such as a Hadoop Cluster), and servers that the CloudEndure Agent can’t be installed on (for example, third-party appliances).

   * For NAS appliances, such as NetApp, use DataSync whenever possible.

   * For components of source applications that are not supported by either CloudEndure Disaster Recovery or DataSync, an application-level solution needs to be used. For example, Oracle has application-level solutions for replicating Oracle RAC databases, NAS appliances can be replicated by periodically copying all the changed files to AWS, and Hadoop clusters may require duplicating each node with another one running in AWS. Application-level disaster recovery solutions have multiple disadvantages, including scope (only relevant for specific applications or application components), cost, and ability to replicate over long distances. Therefore, we recommend trying other disaster recovery solution options prior to opting for an application-level solution.

 * **Success criteria** – For a disaster recovery implementation project, success is normally signified by a disaster recovery drill that achieves a failover according to the requirements of the disaster recovery plan.

 * **Team resources** – Assign team members who are skilled in the resources you mapped for replication. For example, if you have a mix of Linux and Windows operating systems, then you need people who understand both operating systems.

 * **Timelines** – Assign timelines to the implementation phase and to at least a single successful drill per application that will conclude the implementation. The disaster recovery implementation is done when all of the applications have had at least one successful drill.


### How to respond in the event of a disaster

#### Performing a failover

* An actual failover is very similar to a drill. The two main differences are that during an actual failover, users will be redirected to the disaster recovery site and that a failback may be needed when the disaster is over. In the event of planned or unplanned downtime, begin the failover process by using the CloudEndure User Console to launch recovery machines in your disaster recovery target infrastructure.

1. Launch machine in recovery mode to note in the event log that it was an actual failover launch.

2. Perform the actual failover (directing traffic to your target instances) using the tool or the means you use for directing traffic. For a Domain Name System (DNS) redirect, AWS recommends using Amazon Route 53 Application Recovery Controller.

3. After a successful failover, and after the downtime is over, you can prepare for failback.

#### Performing a failback and returning to normal operations

* After performing a successful failover, data from changes that occurred while the disaster recovery site was active may need to merged back into the source applications, or all the data may need to be copied back in case the data of the source site cannot be recovered after the disaster is over.

* To failback to the source servers (or to new servers, if the source ones are no longer available after the disaster is over), follow these steps:

 1. Start by replicating the data from the disaster recovery site back to the source site (when using CloudEndure Disaster Recovery, utilize the user console to open the Project Actions menu and choose the Prepare for Failback option).

 2. Continue using the disaster recovery site while the data is being replicated.

 3. Choose a failback window that minimizes the impact on users (because during failback a short amount of downtime may occur).

 4. During the failback window, go to the disaster recovery site and stop the application.

 5. Make sure that all the data finishes replicating to the source site.

 6. Start the application in the source site.

 7. Make sure the application launches correctly.

 8. Once you have verified that the application is running properly, redirect users back to the application in the source site.

# Conclusion

* A properly planned and implemented disaster recovery solution is an important part of your resilience strategy and is crucial to mitigating the financial loss and reputational damage that a disaster can cause. This whitepaper outlined the various considerations of how to choose a disaster recovery solution and how AWS can be utilized in such scenarios.

* Customers with on-premises workloads have a variety of options when it comes to using AWS for disaster recovery. This whitepaper covered the various services that can help customers replicate their source applications into AWS and recover them in the case of a disaster, including CloudEndure Disaster Recovery, AWS DataSync, and AWS Route 53. 

* This whitepaper also outlined how to best match a disaster recovery solution to an application and guided customers through the entire disaster recovery implementation process, from mapping applications to performing drills, and finally to performing a disaster recovery failover and failback.


# Reference

[Original paper](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-of-on-premises-applications-to-aws/disaster-recovery-of-on-premises-applications-to-aws.html?did=wp_card&trk=wp_card)
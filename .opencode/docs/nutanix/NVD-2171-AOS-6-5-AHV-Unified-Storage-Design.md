# **Hybrid Cloud with AHV Unified Storage Design** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Hybrid Cloud with AHV Unified Storage Design 

## **Contents** 

**1. Executive Summary.................................................................................5 2. Hybrid Cloud Unified Storage Design Software Versions....................8 3. Nutanix Terminology................................................................................9 4. Storage Infrastructure Design for Hybrid Cloud with Unified Storage.....................................................................................................11** Scalability Design for Hybrid Cloud with Unified Storage.................................................................15 Resilience Design for Hybrid Cloud with Unified Storage................................................................ 17 File Server and Object Store Design for Hybrid Cloud with Unified Storage....................................19 Cluster Design for Hybrid Cloud with Unified Storage..................................................................... 20 Storage Design for Hybrid Cloud with Unified Storage.................................................................... 24 Network Design for Hybrid Cloud with Unified Storage....................................................................25 Management Components for Hybrid Cloud with Unified Storage...................................................27 Monitoring for Hybrid Cloud with Unified Storage............................................................................ 28 Security and Compliance for Hybrid Cloud with Unified Storage..................................................... 32 Datacenter Infrastructure for Hybrid Cloud with Unified Storage..................................................... 39 **5. Backup and Disaster Recovery for Hybrid Cloud with Unified Storage.....................................................................................................42** Disaster Recovery Design for Hybrid Cloud with Unified Storage....................................................47 Backup Design for Hybrid Cloud with Unified Storage.....................................................................51 **6. Ordering.................................................................................................. 53** Sizing Considerations........................................................................................................................54 Primary and Secondary Nutanix Files Clusters Bill of Materials...................................................... 54 Primary and Secondary Objects Storage Clusters Bill of Materials................................................. 55 Nutanix Professional Services.......................................................................................................... 57 **7. Test Plan................................................................................................. 58** 

**8. References.............................................................................................. 59 About Nutanix.............................................................................................60 List of Figures.............................................................................................................................................61** 

Hybrid Cloud with AHV Unified Storage Design 

## 1. Executive Summary 

Enterprises today face increasing pressure to modernize their IT infrastructure and storage solutions while maintaining oversight, compliance, and performance. To address this challenge, this Nutanix Validated Design (NVD) enables organizations to deploy a robust, hybrid cloud infrastructure with unified storage that is simple to deploy and operate. 

The hybrid cloud with unified storage NVD assists organizations with deploying a consolidated storage ecosystem using Nutanix Unified Storage while offering rich data services such as analytics, life cycle management, cybersecurity, and strong data protection. The architecture supports a scalable, resilient, and secure unified storage solution with two datacenters for high availability and disaster recovery. 

**Note:** In this design, each datacenter resides in its own availability zone (AZ). This document denotes location exclusively by AZ. 

You can add this flexible and scalable design as a module to the core Nutanix OnPremises Hybrid Cloud with AHV Design, or you can deploy it as a standalone unified storage solution. If you deploy the unified storage NVD on its own, you might still need to reference parts of the core hybrid cloud NVD, as this document omits some information to avoid repetition. 

Key features: 

- The full-stack integration combines Nutanix AOS, AHV, Prism Central, Objects Storage, and Files Storage into a unified storage solution. 

- Files Storage Smart DR supports up to 80 standard shares and 20 distributed shares per file server pairing. 

- With a frequency of less than 10 minutes, you can replicate up to 5 distributed shares or 20 standard shares. 

- Deploying Prism Central in a two-site, scale-out configuration creates a highly available and distributed management plane. 

© 2026 Nutanix, Inc. All rights reserved  | **5** 

Hybrid Cloud with AHV Unified Storage Design 

- The design can support an object store size of around 218 TiB (accounting for n + 1), a single share size of around 205 TiB usable (accounting for n + 1), and 8,000 concurrent client connections. 

- Enterprise-grade business continuity and disaster recovery (BCDR) targets 99.999 percent availability. 

- The BCDR tiers provide recovery point objective (RPO) options of 1 minute, 1 hour, and 24 hours and a recovery time objective (RTO) of 1 hour, which includes client-side cache operations that are outside the influence of Files Storage Smart DR. 

Advantages: 

- A validated, preintegrated solution simplifies hybrid cloud with unified storage deployment. 

- Modular design provides flexible scalability and resilience for the management and control plane so that you can grow your infrastructure incrementally. 

- Applying vendor-validated best practices promotes reliability and supportability. 

- Automation and standard VM sizing prepare your environment for operational consistency, predictable performance, and easier management. 

## Benefits: 

- Faster time to value: A validated design with a comprehensive bill of materials accelerates deployment and reduces trial and error. 

- Improved IT agility: You can rapidly provision and scale workloads in response to business needs. 

- Reduced risk: Built-in backup and disaster recovery protect applications and the entire infrastructure to provide business continuity. 

- Lower total cost of ownership: Efficient resource utilization and automation reduce operational overhead. 

Nutanix rigorously tests and documents solutions in the NVD program to align with best practices and support faster, more resilient deployments, giving IT teams a trusted foundation for building enterprise-grade private cloud environments. 

_Table: Document Version History_ 

© 2026 Nutanix, Inc. All rights reserved  | **6** 

Hybrid Cloud with AHV Unified Storage Design 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|January 2023|Original publication.|
|1.1|February 2023|Updated the Data Reduction|
|||Options section.|
|2.0|February 2024|Updated for Nutanix Files|
|||4.4 and Nutanix Objects 4.3.|
|||Updated the Nutanix Files|
|||disaster recovery architecture.|
|||Added the Test Plan section.|
|2.1|March 2024|Updated for Nutanix Objects|
|||4.3.0.2.|
|2.2|September 2024|Updated the Ordering section.|
|2.3|November 2024|Updated AOS release cycle|
|||descriptions.|
|2.4|February 2026|Updated document structure|
|||and the Executive Summary|
|||and Ordering sections.|



© 2026 Nutanix, Inc. All rights reserved  | **7** 

Hybrid Cloud with AHV Unified Storage Design 

## 2. Hybrid Cloud Unified Storage Design Software Versions 

The following table summarizes the complete package of software that Nutanix validated for functionality and interoperability with this solution. 

_Table: Software Versions Used in Validation Testing_ 

|**Component**|**Software Version**|
|---|---|
|Nutanix Prism Central|pc.2023.4|
|Nutanix AOS|6.5.4|
|Nutanix AHV|20220304.441|
|Objects Service|4.3.0.2|
|Objects Manager|4.3.0.2|
|Microservices Platform (MSP)|2.4.4.1|
|Nutanix Files|4.4.0.1|
|Files Manager|4.4.0.1|
|File Server Module|4.4.0.1|
|F5 BIG-IP|16.1.2-0.0.18|
|HYCU|4.6.0-3452|



© 2026 Nutanix, Inc. All rights reserved  | **8** 

Hybrid Cloud with AHV Unified Storage Design 

## 3. Nutanix Terminology 

This document uses the following terms to refer to different elements of the Nutanix hybrid cloud solution. 

## **Cluster** 

A cluster is the management boundary of the storage provided to a group of workloads. 

## **Region** 

Regions are the geographic areas where you deploy datacenters. Different regions are far enough away from each other that they are unlikely to be impacted by the same natural disasters, power grid outages, and other events as other regions. Nutanix defines a region as a datacenter location where round-trip latency is greater than 5 ms but less than 100 ms. 

## **Availability zone** 

Availability zones (AZs) are physically and logically separated datacenters or datacenter rooms with independent power sources, networks, and cooling connected with an extremely low-latency network. With Nutanix, Prism Central manages these components. 

## **Management domain** 

A management domain is a logical construct that refers to components such as Nutanix Cloud Manager, application domain Prism Central instances, Foundation or Foundation Central, Nutanix Central, and vCenter Server that are deployed on dedicated Nutanix clusters located in a separate security zone. 

## **Application domain** 

An application domain is a logical construct that refers to the Prism Element clusters in a single AZ and the Prism Central instance that they're registered to. 

## **Pod** 

A pod is a group of resources managed by the Prism Central instances in the two management clusters, and isn't bound by physical location. 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

Hybrid Cloud with AHV Unified Storage Design 

## **Block** 

A block is a Nutanix cluster or a pair of clusters that are located in different AZs. 

## **Fault domain** 

Fault domains are groups of VMs that share a common power source, network infrastructure, server rack, Nutanix cluster, or datacenter location. 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

Hybrid Cloud with AHV Unified Storage Design 

## 4. Storage Infrastructure Design for Hybrid Cloud with Unified Storage 

The following lists provide core storage infrastructure design requirements, risks, and constraints. 

Storage infrastructure design requirements by component: 

- Management: 

   - › Deploy a unified management plane at the right scale to manage all clusters and workloads in the environment. 

   - › Configure management to integrate with Active Directory for authentication. 

   - › Use Active Directory–based groups for access control. 

- Monitoring: 

   - › Enable platform fault monitoring and use email to send alerts. 

   - › Enable Nutanix Data Lens anomaly detection, ransomware protection, and monitoring and use email to send alerts. 

   - › Monitor performance metrics in the Nutanix Files and Nutanix Objects control planes. 

   - › Monitor resources critical to Nutanix Files and Nutanix Objects operations (for example, CPU, memory, storage, and network resources); resource usage that exceeds configured limits generates an alert. 

   - › Use email as the primary channel for event monitoring alerts. 

- Connectivity: Support the SMB and NFS protocols and the S3 API. 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

Hybrid Cloud with AHV Unified Storage Design 

- Capacity and performance: 

   - › Ensure that usable storage capacity does not exceed 200 TiB. 

   - › Support a working set size of 1 TiB. 

   - › Colocate applications and data to reduce unnecessary WAN traffic, minimize application latency, and reduce network congestion. 

- Business continuity and disaster recovery (BCDR): 

   - › Achieve recovery point objectives (RPOs) of 1 minute, 1 hour, and 24 hours. 

   - › Achieve a recovery time objective (RTO) of 1 hour. 

   - › Provide nightly incremental backup, file-level and share-level granular restore, and self-service restore. 

- Infrastructure: Minimize cost of storing cold data. 

- Security and analytics: 

   - › Provide ransomware protection. 

   - › Provide network-level security to isolate protocol services for virus and worm threats. 

   - › Enforce segregation of duties so storage administrators don't have management access for workloads that aren't storage. 

   - › Provide storage analytics for performance and capacity management, audit reporting, anomaly detection, and ransomware policies. 

Storage infrastructure design risks by component: 

- Monitoring: If Prism Central becomes unavailable for any reason, the platform can no longer send alerts. 

To mitigate this risk, configure each Prism Element instance to send alerts as well. As this approach results in duplicate alerts during normal operations, send Prism Element alerts to a different mailbox that you can monitor when Prism Central is unavailable. 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

Hybrid Cloud with AHV Unified Storage Design 

- Management: 

   - › If Prism Central becomes unavailable in an AZ, the Nutanix Objects service in that AZ continues to function, but object store creation, upgrade, and scale-out operations are not available. 

   - › If the affected Prism Central instance is managing the Nutanix Files clusters, Smart Disaster Recovery (Smart DR) replication continues to work, but you can't make changes to file server data protection policies. 

   - › If the Prism Central instance managing the Nutanix Files clusters becomes unavailable and the file server must fail over to the other AZ, you can use Prism Central in the other AZ to orchestrate recovery. 

Storage infrastructure design constraints by component: 

- Nutanix Files clusters: The number of file server VMs (FSVMs) per cluster in a fully scaled Nutanix Files deployment doesn't exceed 16. The number of FSVMs doesn't exceed the number of physical nodes in the cluster. 

- Nutanix Objects clusters: 

   - › The number of load balancer VMs doesn't exceed four. 

   - › The number of worker VMs doesn't exceed the number of physical nodes in the cluster. 

- BCDR: 

   - › A file server can have only one tiering profile. In a scenario where both AZs contain active file servers, each must replicate to a target file server that doesn't already have a tiering profile (a passive file server). Because you're replicating to a file server that isn't already involved in a tiering relationship, the tiering profile can successfully fail over to the passive file server without conflict. 

   - › Inline reads of tiered data files are possible after failover, but data tiering and recall operations are blocked. 

   - › Smart DR supports a maximum of 80 standard shares and 20 distributed shares per file server pairing. With a frequency of less than 10 minutes, you can replicate up to 5 distributed shares or 20 standard shares. 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

Hybrid Cloud with AHV Unified Storage Design 

- Monitoring: 

   - › SMTP is an available channel in the environment that can receive event monitoring alerts. 

   - › Files Storage and Objects Storage natively integrate into the platform's SMTP capability. 

   - › Syslog, which is supported by both Files Storage and Objects Storage, captures logs but doesn't generate alerts on events. 

The conceptual pod design has the following features: 

- Two active-active datacenters in separate AZs. 

- Two physical storage clusters in each AZ: one hosts Nutanix Objects, and the other hosts Nutanix Files. 

- An instance of Prism Central hosted on the Nutanix Objects cluster in each AZ. 

   - › The Prism Central instance in AZ1 manages both the Nutanix Objects and Nutanix Files clusters in AZ1. 

   - › The Prism Central instance in AZ2 manages both the Nutanix Objects and Nutanix Files clusters in AZ2. 

- Smart tiering of cold file data between the Nutanix Files and Nutanix Objects clusters in each AZ. 

- Bidirectional replication between the Files Storage clusters in each AZ. 

- Bidirectional replication between the Objects Storage clusters in each AZ. 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

Hybrid Cloud with AHV Unified Storage Design 

Figure 1: Conceptual Storage Design 

## **Scalability Design for Hybrid Cloud with Unified Storage** 

Scalability is one of the core concepts of the Nutanix platform and, in the context of unified storage, refers to the ability to increase storage space and storage processing capacity to meet both current and future SMB, NFS, and S3 workload demands. A welldesigned cluster meets current requirements while providing a path to support future growth. 

This Nutanix Validated Design (NVD) allows vertical and horizontal scaling within the boundaries set by running storage workloads in a single rack per availability zone (AZ) across two AZs. If the workload grows, you can add nodes and storage capacity to the cluster. This design has a maximum of 8 nodes per Files Storage cluster and per Objects Storage cluster, for a total of 16 nodes per AZ; if you must scale beyond that number, you can deploy additional nodes in an adjacent rack. Such expansion is beyond the scope of the current design. However, if you must expand the Objects Storage namespace beyond 8 nodes, deploy a new Nutanix cluster in another rack and use the Objects Storage multicluster functionality to add it to the existing Objects Storage namespace. With the Smart Tiering integration, expanding the Objects Storage deployment in this way also makes more storage capacity available to the Files Storage namespace. 

**Note:** If the infrastructure changes in one AZ, you must upgrade the other AZ accordingly to ensure that a failover can complete successfully. Identical configurations are required between AZs. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

Hybrid Cloud with AHV Unified Storage Design 

This NVD provides separate, dedicated clusters in each AZ for Nutanix Files and Nutanix Objects workloads, so you can expand allocated vCPU and memory for both service types concurrently without the risk of CPU or memory sharing between the Nutanix Files and Nutanix Objects services. Because the most common storage use cases tend to require storage density, this design uses a hybrid disk configuration with a mixture of SSDs and high-capacity HDDs. 

The Nutanix model used in this design puts eight workload nodes in 16 rack units (RU) (four nodes in 8 RU for Files Storage and four nodes in 8 RU for Objects Storage). The design uses a single rack per AZ, with redundant top-of-rack network switches. This approach reduces operational complexity, but the large form factor of these storagedense nodes limits the number that can fit in a single rack. Power and cooling limitations might introduce further constraints. For more information on hardware and networkrelated limitations, see Nutanix On-Premises Hybrid Cloud with AHV Design. 

_Table: Scalability Design Decisions_ 

|**Design Option**|**Validated Selection**|
|---|---|
|Node memory population|Use 128 GB for Nutanix Objects nodes and|
||256 GB for Nutanix Files nodes.|
|Node drive type|Use a mix of SSDs and HDDs.|
|Node drive population|Fully populate the node drive.|
|Cluster placement|Use one rack per AZ.|
|Establish scalability boundaries|Use X-Ray to confirm load per node.|



At the software level, configuration maximums also constrain solution scalability. For more information on the latest limits for Files Storage and Objects Storage, see Files Storage configuration maximums (Nutanix Portal credentials required) and Objects Storage configuration maximums (Nutanix Portal credentials required). 

_Table: Configuration Maximums or Maximum System Values_ 

|**Entity**|**Architectural Maximum**|**Number Included in Unified**|
|---|---|---|
|||**Storage NVD**|
|FSVMs per file server|16|4|
|Concurrent client connections|64,000|8,000|



© 2026 Nutanix, Inc. All rights reserved  | **16** 

Hybrid Cloud with AHV Unified Storage Design 

|**Entity**|**Architectural Maximum**<br>**Number Included in Unified**<br>**Storage NVD**|
|---|---|
|Single share size<br>Worker VMs per object store<br>Load balancers per object<br>store<br>Object store size|5 PB (usable)<br>205.2 TiB (usable) (accounts<br>for n + 1)<br>Same as the maximum node<br>count<br>3<br>4<br>2<br>Limited only by the physical<br>cluster capacity<br>218.47 TiB (usable) (accounts<br>for n + 1)|



**Note:** Capacity values in this table don't account for potential savings from compression and erasure coding. 

This design uses the Smart Tiering feature to facilitate scaling file data capacity beyond the limits of the local physical Files Storage cluster. The Nutanix Data Lens service provides access to Smart Tiering. After you connect the file servers to the Nutanix Data Lens service, you can define the tiering endpoint for each active file server and create a tiering policy. 

Smart Tiering operates on file data based on last access time, moving cold file data out to an object storage endpoint and leaving a stub behind in the share's active file system. The tiering endpoint can be either a public cloud or an on-premises object store. In this NVD, the endpoint is the Nutanix Objects cluster residing in the same AZ as the file server. 

_Table: Smart Tiering Policy Decisions_ 

|**Policy**|**Timeframe**|**Reads within**|**Capacity Threshold**|
|---|---|---|---|
|||**Timeframe**||
|Tier|6 months|0|70%|
|Recall|1 day|3|N/A|



## **Resilience Design for Hybrid Cloud with Unified Storage** 

Nutanix provides many resilience features, including storage replication, snapshots, degraded node detection, and self-healing. These capabilities increase the resilience of 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

Hybrid Cloud with AHV Unified Storage Design 

unified storage workloads. Nutanix layers these software features on hardware designed to be resilient (for example, with redundant physical components and power supplies, many of which are hot-swappable or otherwise easily serviceable). Running workloads in a virtualized environment adds another kind of resilience, as you can perform many maintenance operations without application downtime. A resilient network fabric that can sustain individual link or node failures without significant impact completes the architecture. 

File servers with three or more FSVMs achieve high availability during node failure and Nutanix Files upgrades at the file server level. Resources under the control of a failed FSVM or an FSVM that's being upgraded temporarily move to another FSVM. For singleFSVM instances, hypervisor high availability provides protection against node failure events. 

Objects Storage provides built-in resilience in the event of worker VM failure. If a worker VM fails, the load balancers automatically redirect clients to surviving worker VMs. Any in-flight requests to the failed worker VM might fail, causing the client to resend the request. To ensure that the system doesn't direct clients to a failed load balancer, each availability zone (AZ) has a global server load balancer (GSLB) deployed in front of the object store. The GSLB used in this Nutanix Validated Design (NVD) is F5 BIGIP. All client lookups of the object store namespace are handled by the GSLB, which forwards the client requests to the Objects Storage load balancers in the local AZ. The GSLB detects when a load balancer VM fails and ceases to forward client requests to that particular load balancer until it comes back into service. The GSLB also makes disaster recovery failovers between AZs seamless by detecting a full AZ-level failure and forwarding all client requests to the load balancer VMs in the surviving AZ. 

The workload cluster sizing allows for n + 1 failure redundancy. Monitoring and alerting ensure that any issues result in an alert; consistently monitoring workload growth ensures that sufficient headroom is available at any time. 

_Table: Resilience Design Decisions_ 

|**Design Option**|**Validated Selection**|
|---|---|
|Full redundancy of all components|Ensure the full redundancy of all components|
||in the AZ.|
|Established resilience boundaries|Use X-Ray to find resilience constraints.|



© 2026 Nutanix, Inc. All rights reserved  | **18** 

Hybrid Cloud with AHV Unified Storage Design 

## **File Server and Object Store Design for Hybrid Cloud with Unified Storage** 

To achieve high performance for both the Files Storage and Objects Storage workloads, this design minimizes overallocation of physical resources where possible. If Files Storage and Objects Storage are located on the same physical cluster (a supported configuration), sharing certain resources (for example, CPU cores and resources for the Nutanix Controller Virtual Machine (CVM)) between the environments is unavoidable. However, this Nutanix Validated Design (NVD) provides separate physical clusters for Files Storage and Objects Storage, removing any risk of contention between the two storage service types. 

The amount of compute resources that you can allocate to the file server VMs (FSVMs) in Files Storage is configurable. This design replicates smart-tiered file servers between AZs. Because each file server can have only one tiering profile (see the previous list of storage infrastructure design constraints), you must deploy two file server instances on each physical Files Storage cluster (one active and one passive) so that there are two FSVMs on every physical node. The base deployment allocates 6 vCPUs and 32 GB of RAM to each FSVM because the compute specification of the underlying physical nodes is dual 12-core CPUs and FSVMs can potentially achieve better performance when they remain within the boundaries of a NUMA node. 

If each of the coexisting FSVMs has 6 vCPUs, they can coexist on the same 12-core NUMA node without having to share cores. For more information on the number of client connections a given FSVM configuration can support, see SMB Performance (Nutanix customer account required). The other NUMA node in these dual-socket nodes is dedicated to the Nutanix CVM. 

You can add compute resources nondisruptively to the FSVMs to support more client sessions or deliver greater throughput. Scaling up the FSVMs in this way introduces a degree of core sharing; however, even with both FSVMs at maximum size, the sharing doesn't exceed 2:1. As one of the two FSVMs only receives incoming replication traffic from the other AZ, sharing is unlikely to have significant effects. You can also scale FSVMs down for full flexibility. The following table details the minimum and maximum FSVM sizes. To scale, you can either expand the existing FSVM compute resources (scaling up) or add more FSVMs (scaling out). 

_Table: Supported FSVM Configurations_ 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

Hybrid Cloud with AHV Unified Storage Design 

|**FSVM Size**|**Minimum**|**Maximum**|
|---|---|---|
|Virtual CPU|4|24|
|Virtual memory|12 GB|512 GB|
|Maximum concurrent|500|4,000|
|connections per FSVM|||
|FSVMs per cluster (regardless|3|16|
|of FSVM configuration)|||



Objects Storage has one fixed size for the worker VM and one fixed size for the load balancer VM, both detailed in the following table. To scale Objects Storage, add more worker VMs (scaling out). 

_Table: Supported Objects Storage VM Configurations_ 

|**Item**|**Worker VM**|**Load Balancer VM**|
|---|---|---|
|Virtual CPU|10|2|
|Virtual memory|32 GB|4 GB|
|Maximum per cluster|The total number of|4|
||nodes in the AOS cluster||



This design deploys a single Prism Central VM (small size) allocated with 6 vCPUs on each Objects Storage cluster. This configuration results in a modest amount of core sharing with one of the Objects Storage worker VMs. However, only one node is sharing; even with a load balancer VM on the same node, oversubscription doesn't exceed 2:1. 

## **Cluster Design for Hybrid Cloud with Unified Storage** 

This design includes one dedicated physical cluster for Objects Storage and another for Files Storage in each availability zone (AZ), with the Prism Central management layer located on the Objects Storage cluster. The design doesn't include dedicated physical clusters for management because the additional costs they incur aren't warranted. You can alternatively use the Prism Central instances from the core hybrid cloud Nutanix Validated Design (NVD) (Management Components for On-Premises Hybrid Cloud) to manage the Files Storage and Objects Storage clusters. 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

Hybrid Cloud with AHV Unified Storage Design 

This NVD uses one region with two separate AZs. Both AZs host active workloads, and each AZ provides a replication target for the other’s data. A Nutanix Prism Central instance in the respective local AZ manages each Objects Storage and Files Storage deployment. 

## _Table: Cluster Design Decisions_ 

|**Design Option**|**Validated Selection**|
|---|---|
|Number of regions|Use one region.|
|Number of AZs|Use two AZs.|
|Number of datacenters|Use two datacenters: one per AZ.|
|Mixed workloads or dedicated workload per|Use a dedicated workload per cluster.|
|cluster||
|Minimum workload cluster size|Use at least four nodes per workload cluster|
||(Nutanix Files and Nutanix Objects are on|
||separate clusters).|
|Maximum workload cluster size for this design|Nutanix Files: Use eight nodes (single rack|
||constraint). Nutanix Objects: Use eight nodes|
||(single rack constraint).|
|Cluster node redundancy|Use n + 1 for redundancy (safely usable|
||storage capacity).|
|Maximum usable nodes for storage capacity|Consume at most three usable nodes for|
||storage to ensure that you can always rebuild|
||data with replication factor 2.|
|Maximum usable nodes for storage processing|Configure all four nodes to optimize the|
|compute|investment; the loss of a node might cause|
||performance degradation.|
|Workload clusters in one rack or split across|Use one rack per AZ for both Files Storage|
|multiple racks|and Objects Storage clusters.|
|Cluster replication factor|Use replication factor 2.|
|Cluster high availability configuration|Guarantee high availability.|
|Percentage of client connections supported|Support 100 percent of client connections;|
|during disaster recovery failover|you might see some performance degradation|
||when hardware resources experience|
||increased demand.|



© 2026 Nutanix, Inc. All rights reserved  | **21** 

Hybrid Cloud with AHV Unified Storage Design 

|**Design Option**|**Validated Selection**|
|---|---|
|Maximum usable resource capacity per cluster<br>to allow for disaster recovery failover|With workloads split evenly between AZs,<br>50 percent of the storage resources at each<br>AZ can serve data locally while the other 50<br>percent accommodates incoming replication<br>from the other AZ.|



## **Hybrid Cloud with Unified Storage Platform Selection and Capacity Management** 

The following table provides details regarding hardware platform selection. 

_Table: Platform Selection_ 

|**Hardware or Service**|**Nutanix Files**|**Nutanix Objects**|
|---|---|---|
|Node type|NX-8155-G8|NX-8155-G8|
|Node count|4|4|
|Rackspace (per node)|2 RU|2 RU|
|Processor|2 Intel Xeon Silver 4310 12-|2 Intel Xeon Silver 4310 12-|
||core 120 W, 2.1 GHz (Ice|core 120 W, 2.1 GHz (Ice|
||Lake)|Lake)|
|RAM|8 × 32 GB 3,200 MHz DDR4|4 × 32 GB 3,200 MHz DDR4|
||RDIMM (256 GB total)|RDIMM (128 GB total)|
|SSD|4 × 7.68 TB|2 × 3.84 TB|
|HDD|8 × 18 TB|10 × 18 TB|
|NIC|25 GbE Dual SFP+|25 GbE Dual SFP+|
|Support|3Y Production|3Y Production|



In this design, each Files Storage cluster can grow from a minimum of four nodes (8 RU) with three nodes of usable storage capacity to a maximum of eight nodes (16 RU) with seven nodes of usable storage capacity. Likewise, each Objects Storage cluster can grow from a minimum of four nodes (8 RU) with three nodes of usable storage capacity to a maximum of eight nodes (16 RU) with seven nodes of usable storage capacity. You can expand both the Files Storage and Objects Storage clusters in single-node increments up to the maximum. In both cases, each additional node provides increased throughput and more storage capacity to the environment. If you reach the maximum number of nodes prescribed by this NVD and need more, you can either deploy a 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

Hybrid Cloud with AHV Unified Storage Design 

new cluster in a new rack or expand the existing cluster by deploying new nodes in a neighboring rack. This storage NVD, however, stops at eight nodes for each storage service, all in a single rack. 

The proportion of usable storage decreases as the cluster size decreases because the system reserves one node per cluster for maintenance and failure. Moreover, the reduction in usable capacity after the n + 1 allowance can be significant when using storage-dense nodes. Coupled with the fact that expanding an existing file server namespace is significantly more transparent than creating an entirely new namespace (each new Files Storage cluster results in a new namespace), these factors mean that if you must expand the Files Storage cluster beyond eight nodes, scale it into a new rack. 

Objects Storage, unlike Files Storage, has multicluster support, so an object store's namespace can span multiple physical clusters. However, the same AOS Storage n + 1 overheads apply. Additionally, you can only add new worker VMs in the initial AOS cluster. Therefore, to scale performance, if you must expand the Objects Storage deployment beyond eight nodes, add more nodes (in a new rack) to the existing underlying AOS Storage cluster rather than create a new cluster. 

**Note:** Although scaling beyond eight nodes per workload is beyond the scope of the current design, using eight nodes is neither an architectural nor a practical limit for either Files Storage or Objects Storage. 

In situations where you must scale capacity to accommodate growth in file data, you might need to scale the Nutanix Mine Integrated Backup cluster that is backing up the Files Storage environment proportionately. For more information, see Backup Design for On-Premises Hybrid Cloud. 

## **Hybrid Cloud with Unified Storage Cluster Resilience** 

Replication factor 2 protects against the loss of a single component in case of failure or maintenance. During a failure or maintenance scenario, Nutanix rebuilds any data that falls out of compliance much faster than traditional RAID data protection methods. Rebuild performance increases linearly as the cluster grows. 

In the Nutanix architecture, rapid recovery in the event of failure is the standard, and no single points of failure exist. You can configure the cluster to maintain either two or three copies of data; to maintain three copies, you need at least five nodes. 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

Hybrid Cloud with AHV Unified Storage Design 

## **Storage Design for Hybrid Cloud with Unified Storage** 

Nutanix uses a distributed, shared-nothing architecture for storage. For more information on Nutanix storage constructs, see Nutanix Hybrid Cloud Compute and Storage in Nutanix Platform Architecture for Datacenters. For information on node types, counts, and physical configurations, see Cluster Design for On-Premises Hybrid Cloud in the Nutanix On-Premises Hybrid Cloud with AHV Design. 

Creating a cluster automatically creates a number of storage containers. When you create a new file server, the system automatically creates a storage container dedicated to that file server. The name of the container conforms to the following format: `Nutanix_<fileserver_name>_ctr` . When you deploy an object store, the system creates two storage containers: one for data and the other for metadata. These containers use the following naming convention: 

- Data container: `objectsd<uniqueidentifier>` 

- Metadata container: `objectsm<uniqueidentifier>` 

To increase the effective capacity of the Files Storage and the Objects Storage clusters, this design enables erasure coding with the default strip size on the Files Storage and Objects Storage containers. 

**Note:** Erasure coding requires a minimum of four nodes. 

Deduplication isn't enabled for either the Files Storage or Objects Storage containers. Unless the application writing to Objects Storage is already compressing the data, enable inline compression on the Objects Storage containers. Files Storage manages inline compression at the file server level on a share-by-share basis (enabled by default); delayed compression is also enabled at the container level when you deploy a file server. The following data reduction settings apply across clusters in both availability zones (AZs). 

- `Nutanix_<file_server_name>_ctr` 

   - › Compression: On (60-minute delay) 

   - › Deduplication: Off 

   - › Erasure Coding: On 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

Hybrid Cloud with AHV Unified Storage Design 

- Nutanix Objects containers 

   - › Compression: On 

   - › Deduplication: Off 

   - › Erasure Coding: On 

## _Table: Storage Design Decisions_ 

|**Design Option**|**Validated Selection**|
|---|---|
|Sizing a cluster|For Nutanix Files, deploy four large SSDs|
||per node to provide enough usable hot tier|
||capacity to support the file server’s working|
||set. For Nutanix Objects, deploy two medium|
||SSDs per node to support Nutanix Objects|
||metadata (object data payload is directly|
||written to and read from the HDD tier).|
|Node type vendors|Use all Nutanix NX nodes.|
|Node and disk types|Use identical node types equipped with similar|
||disks.|
|Sizing for node redundancy for storage|Size all clusters for n + 1 failover capacity.|
|Fault tolerance and replication factor settings|Configure the cluster for fault tolerance 1 and|
||configure the container for replication factor 2.|
|Inline compression|Enable inline compression at the container|
||level for Nutanix Objects and at the file server|
||level for Nutanix Files. Exception: In both|
||cases, if data is already compressed at the|
||client level, turn off compression on the|
||Nutanix side.|
|Deduplication|Don't enable deduplication.|
|Erasure coding|Enable erasure coding (the default setting for|
||Nutanix Objects).|



## **Network Design for Hybrid Cloud with Unified Storage** 

A Nutanix cluster can tolerate multiple simultaneous failures because it maintains a set redundancy factor. However, this level of resilience requires a highly available network 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

Hybrid Cloud with AHV Unified Storage Design 

connecting a cluster's nodes. For more information on deploying a network to achieve the necessary resilience levels, see Network Design for On-Premises Hybrid Cloud in the Nutanix On-Premises Hybrid Cloud with AHV Design. Files Storage and Objects Storage have the following additional network requirements for a four-node deployment. 

**Note:** In both cases, the storage network addresses must be on the same network as CVM eth0. Files Storage 4.1 and later versions support segmented iSCSI traffic, but this Nutanix Validated Design (NVD) didn't test it. Objects Storage doesn't support segmented iSCSI traffic. 

The following table details IP address requirements covering eight FSVMs per AZ (four FSVMs per file server, with two file servers). 

_Table: Nutanix Files IP Address Requirements_ 

|**Network Type**|**Minimum IP Addresses**|**Minimum IP**|
|---|---|---|
||**per File Server**|**Addresses per AZ**|
|Client|4|8|
|Storage|5|10|



When scaling out the environment, allow for one more client network address and one more storage network address for every FSVM you add. 

Objects Storage runs as a containerized service on a Kubernetes microservices platform, which provides benefits such as increased velocity of new features. Several of the required storage IP addresses are for functions related to the underlying microservices platform. You must manage these networks. 

The design has the following IP address requirements covering three worker VMs and two load balancer VMs per AZ: 

- Public: Minimum of 2 IP addresses 

- Storage: Minimum of 10 IP addresses 

When scaling out the environment, allow for one more storage network address for every worker VM added. You can't scale out load balancers after deployment, so you don't need additional IP addresses. 

This design includes disaster recovery replication. With both Nutanix Files and Nutanix Objects, replication traffic travels across the client or public network interfaces to the 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

Hybrid Cloud with AHV Unified Storage Design 

corresponding target system on the destination AZ’s client or public network. Neither service requires stretched layer 2 networking. 

The Nutanix Prism Central instances must connect to the following networks: 

- The storage or client networks of the file servers they manage 

- The storage network and the public network for the object stores they manage 

For more information on the specific functions that require the IP addresses in the preceding tables, see the appropriate user guide. 

## **Management Components for Hybrid Cloud with Unified Storage** 

Management components such as Prism Central, Active Directory, DNS, and NTP are critical services that must be highly available. Prism Central is the global control plane for Nutanix; its unified storage management responsibilities include: 

- Object store deployment and management 

- Nutanix Objects Identity and Access Management (IAM) user and administrator management 

- Files Storage Smart DR management 

- Centralized monitoring and management for both Files Storage and Objects Storage 

You can deploy Prism Central in either a single-VM or scaled-out (three-VM) configuration. When you design your management components, decide how many Prism Central instances you need. This Nutanix Validated Design (NVD) deploys one Prism Central instance in the Objects Storage cluster in each availability zone (AZ) for a total of two Prism Central instances. The AZ1 Nutanix Prism Central instance manages the local Objects Storage and Files Storage deployments in AZ1. The AZ2 Nutanix Prism Central instance manages the local Objects Storage and Files Storage deployments in AZ2. 

By deploying Prism Central instances dedicated to Nutanix storage services, you don’t need to rely on role-based access control (RBAC) to restrict the resources that storage administrators can access and manage. However, you can use the dedicated management clusters in the core hybrid cloud NVD to manage Files Storage and Objects Storage as an alternative. Files Storage and Objects Storage provide RBAC to restrict management access where needed. 

© 2026 Nutanix, Inc. All rights reserved  | **27** 

Hybrid Cloud with AHV Unified Storage Design 

In this NVD, the Nutanix Files and Nutanix Objects workload clusters run AOS 6.5.4. 

_Table: Management Component Design Decisions_ 

|**Design Option**|**Validated Selection**|
|---|---|
|Management cluster architecture|Use one Prism Central VM in the Nutanix|
||Objects cluster at each AZ.|
|Prism Central deployment structure|Deploy a single-VM Prism Central instance.|
||Because storage services don't rely heavily on|
||Prism Central, they don't warrant the resources|
||required for scale-out.|
|Prism Central deployment size|Use a small Prism Central deployment size:|
||one VM with 6 vCPUs, 26 GB of RAM, and 500|
||GiB of storage.|
|Prism Central deployment locations|Deploy one Prism Central instance in each AZ.|
|Prism Central container name|Use the default container name.|
|Active Directory authentication|Use Active Directory authentication.|
|Connection to Active Directory|Use SSL or TLS for Active Directory.|



## **Monitoring for Hybrid Cloud with Unified Storage** 

Monitoring in the Nutanix Validated Design (NVD) falls into two categories: event monitoring and performance monitoring. Each category addresses different needs and different issues. 

In a highly available environment, you must monitor events to maintain high service levels. When faults occur, the system must raise alerts in a timely manner so that administrators can take remediation actions as soon as possible. This NVD configures the Nutanix platform's built-in capability to generate Files Storage–related alerts. The Objects Manager UI generates Objects Storage–related alerts, which also appear in Nutanix Prism Central. 

In addition to keeping the platform healthy, maintaining steady resource usage is also essential to delivering a high-performing environment. Performance monitoring continuously captures and stores metrics that are essential when you must troubleshoot performance issues. A comprehensive monitoring approach must track metrics for the following areas: 

© 2026 Nutanix, Inc. All rights reserved  | **28** 

Hybrid Cloud with AHV Unified Storage Design 

- Files Storage 

- Objects Storage 

- Hyperconverged platform 

- Network environment 

- Physical environment 

By tracking metrics in these areas, the Nutanix platform can also provide capacity monitoring across the stack. Most enterprise environments inevitably grow, so you must understand resource utilization and the rate of expansion to anticipate changing capacity demands and avoid any business impact caused by a lack of resources. 

You can use syslog if you must forward Files Storage or Objects Storage log data to third-party systems. Objects Storage also supports NATS and Kafka for sending event notifications. 

This design uses Nutanix Data Lens, which provides file and object analytics as a service, to gain insights into file usage trends, auditing events including anomalies and ransomware activity, and the overall composition of the file estate (broken down by file or object type, size, age, operation type, and so on). The information that the Nutanix Data Lens service returns can be very useful in future decision making related to the Files Storage and Objects Storage environments. Nutanix Data Lens can also raise alerts when it observes suspicious activity. Because Nutanix Data Lens is software-as-aservice running in the public cloud, it requires internet access. 

In this NVD, Prism Central performs most of the event monitoring at the underlying infrastructure level. Objects Storage–specific alerts appear in the Objects Storage management console in Prism Central (the Alerts tab presents all notifications and events), while Nutanix Files–specific alerts and events appear in the Files Manager component in Prism Central. We use SMTP-based email alerts to provide infrastructurelevel notifications for Files Storage and Objects Storage. For more information on the workaround for configuring Objects Storage email alerts, see Nutanix KB-10474 (Nutanix Support Portal account required). 

**Note:** This NVD uses syslog for log collection; for more information, see the Security and Compliance section. Alerts from Prism Central go to a primary email alert recipient that's always monitored. 

© 2026 Nutanix, Inc. All rights reserved  | **29** 

Hybrid Cloud with AHV Unified Storage Design 

To cover situations where Prism Central might be unavailable, each Nutanix cluster in this NVD (the two Files Storage clusters and two Objects Storage clusters) sends out notifications using SMTP as well. The individual Nutanix clusters send alerts to a different receiving mailbox that's only monitored when Prism Central isn't available. The resilient Nutanix Data Lens service also provides SMTP alerting that you configure for both the primary and secondary email recipients. 

Figure 2: Unified Storage Monitoring Design 

Prism Central monitors cluster performance in key areas such as CPU, memory, network, and storage utilization. The Objects Storage management module monitors Objects Storage–specific performance and usage rates, exposing metrics such as requests per second, time to first byte, throughput, and capacity consumption. Many of the reported metrics are available at both the object store level and the bucket level. 

The Files Management Console monitors performance and usage information for Nutanix Files, reporting on metrics such as file latency, throughput, IOPS, capacity consumption, open connections, and file count. Many of the metrics are available at both the file server level and the share level. 

In all cases Nutanix captures these metrics by default, so you don't need to do much configuration. 

The network switches that connect the cluster also play an important role in cluster performance. For more information on monitoring network switches, review Monitoring 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

Hybrid Cloud with AHV Unified Storage Design 

for On-Premises Hybrid Cloud in the Nutanix On-Premises Hybrid Cloud with AHV Design. 

Nutanix Data Lens provides analytics-driven insights into file and object activity that can help you understand the composition of the file environment. Each Files Storage and Objects Storage cluster must have Pulse dial-home support enabled for the file servers and object stores to be visible to the Nutanix Data Lens service. The file servers and object stores at each of the AZs can then have the service explicitly enabled in the Nutanix Data Lens portal. Following Pulse and Nutanix Data Lens configuration, you can set up email alerts in both the anomaly detection and ransomware modules and flag suspicious activity in real time. 

The following table provides descriptions of the monitoring design decisions. 

_Table: Monitoring Design Decisions Specific to Unified Storage_ 

|**Design Option**|**Validated Selection**|
|---|---|
|Nutanix Objects performance monitoring|Use Objects Manager (a module in Prism|
||Central).|
|Nutanix Files performance monitoring|Use Files Management Console.|
|Nutanix Objects storage utilization monitoring|Use Objects Manager (a module in Prism|
||Central).|
|Nutanix Files storage utilization monitoring|Use Files Management Console.|
|Cluster storage runway|Use Prism to predict future cluster and storage|
||container space utilization.|
|Objects Storage alerts|Objects Manager (a module in Prism Central)|
||surfaces alert and event information.|
|Files Storage alerts|Prism Central and Prism Element both|
||surface Files Storage–related alert and|
||event information and email it according to|
||Prism Central and Prism Element SMTP|
||configuration settings.|
|Nutanix Data Lens alerts|Nutanix Data Lens surfaces alerts relating to|
||anomaly detection and ransomware attempts|
||and sends them according to the configuration|
||settings in the respective modules.|



© 2026 Nutanix, Inc. All rights reserved  | **31** 

Hybrid Cloud with AHV Unified Storage Design 

## **Security and Compliance for Hybrid Cloud with Unified Storage** 

Nutanix recommends a defense-in-depth strategy for layering security throughout any enterprise datacenter solution, which also applies to unified storage services. This design section focuses on validating the layers specific to Files Storage and Objects Storage that Nutanix can directly oversee at the control and data plane levels. 

Because Active Directory is required for SMB authentication, this design uses it to apply user and group permissions. For more information on configuring the AOS and network components and integrating them with Active Directory authentication, see Security and Compliance for On-Premises Hybrid Cloud in the Nutanix On-Premises Hybrid Cloud with AHV Design. 

For SMB file client authentication and permissions management, Nutanix recommends leaving the share permission settings at **Full Control** and managing access with NTFS permissions. Apply NTFS permissions using Active Directory–based security groups. With NFS, Files Storage supports Active Directory and LDAP for directorybased authentication. Files Storage also supports leaving the environment unmanaged, using either system authentication or no authentication. This NVD tested system authentication. 

With S3, the administrator generates client access keys for Nutanix Objects using the IAM service. You can generate these keys either for individuals or for all members of an Active Directory security group simultaneously. You can then assign granular, API-level permissions to users on a per-bucket basis. 

You can integrate Files Storage and Objects Storage event logging into the syslog infrastructure as described in the core Hybrid Cloud NVD. 

Network Design for On-Premises Hybrid Cloud in the Nutanix On-Premises Hybrid Cloud with AHV Design describes how microsegmentation protects AHV VMs by controlling which network traffic can travel between VMs or groups of VMs as defined by categories. This storage design uses the same microsegmentation technology to protect the Files Storage and Objects Storage services. This design's policies allow only the necessary inbound communication over the required ports, as detailed in the following table. Inbound ports are restricted to the FSVMs and to the Objects Storage load balancer VMs. For more information on microsegmentation, see the core Hybrid Cloud NVD. For additional port requirement details, see the Port Reference guide. 

© 2026 Nutanix, Inc. All rights reserved  | **32** 

Hybrid Cloud with AHV Unified Storage Design 

## _Table: Inbound Ports to Nutanix Files FSVMs_ 

|**Port Number**|**Description**|**Source**|**Destination**|**Transfer**|**Service**|
|---|---|---|---|---|---|
|||||**Protocol**||
|22|SSHD|CVM|FSVM|TCP or UDP|SSH|
|2027|CVM to FSVM|CVM|FSVM|TCP|Insights|
||management|||||
|2090|CVM to|CVM|FSVM|TCP|Ergon|
||file server|||||
||management|||||
||and task|||||
||status|||||
|2100|Cluster|CVM|FSVM|TCP|Genesis|
||configuration|||||
|7502|Access|CVM|FSVM|TCP|minerva_nvm|
||services|||||
||running on|||||
||Nutanix Files|||||
|9440|REST API|CVM, Prism|FSVM|TCP or UDP|SSH, Mercury|
||calls and|Central||||
||Prism Central|||||
||access|||||
|7515|Smart DR|FSVM|Remote FSVM|TCP|Replicator|
||replication|||||
|111|NFS port|NFS client|FSVM|TCP or UDP|NFS|
||mapper|||||
|2049|NFSv4|NFS client|FSVM|TCP|NFS|
||support|||||
|7508|Statd output|NFS client|FSVM|TCP|NFS (statd)|
||port for boot,|||||
||reboot, and|||||
||recovery|||||
||functions|||||



© 2026 Nutanix, Inc. All rights reserved  | **33** 

Hybrid Cloud with AHV Unified Storage Design 

|**Port Number**|**Description**<br>**Source**<br>**Destination**<br>**Transfer**<br>**Protocol**<br>**Service**|
|---|---|
|20048<br>20049<br>20050<br>445|Mountd<br>access and<br>service<br>request<br>monitoring<br>port<br>NFS client<br>FSVM<br>TCP<br>NFS (mountd)<br>Karbon<br>NFS client<br>FSVM<br>TCP<br>Karbon<br>Lockd port<br>for locking<br>requests<br>NFS client<br>FSVM<br>TCP<br>NFS (lockd)<br>FSVM to<br>SMB client<br>communication<br>SMB client<br>FSVM<br>TCP<br>Active<br>Directory or<br>SMB|



_Table: Inbound Ports to Nutanix Objects Load Balancers_ 

|**Port Number**|**Description**|**Source**|**Destination**|**Transfer**|**Service**|
|---|---|---|---|---|---|
|||||**Protocol**||
|22|SSHD|Prism Central|Storage|TCP|SSH|
||||network|||
|80|HTTP|External S3|Public|TCP|HTTP|
||endpoint|clients|network (load|||
||to access||balancer)|||
||Nutanix|||||
||Objects|||||
|443|HTTPS|External S3|Public|TCP|HTTPS|
||endpoint|clients|network (load|||
||to access||balancer)|||
||Nutanix|||||
||Objects|||||
|9901|Prism Central|Prism Central|Storage|TCP|Envoy|
||checks health||network|||
||of envoy|||||



© 2026 Nutanix, Inc. All rights reserved  | **34** 

Hybrid Cloud with AHV Unified Storage Design 

|**Port Number**|**Description**<br>**Source**<br>**Destination**<br>**Transfer**<br>**Protocol**<br>**Service**|
|---|---|
|5553<br>7100|Prism Central<br>communicates<br>with IAM<br>service<br>Prism Central<br>Public<br>network (load<br>balancer)<br>TCP<br>IAM<br>Nutanix<br>Objects<br>replication<br>Storage<br>network<br>Public network<br>of the target<br>object store<br>TCP<br>Nutanix<br>Objects<br>replication|



To protect against deletion, encryption, or other malicious modification of object data, Objects Storage supports write once, read many (WORM). Cohasset has validated the Nutanix WORM implementation against industry-recognized security standards. For more information, see Nutanix Objects: SEC, FINRA, and CFTC Compliance Assessment by Cohasset. 

The core Hybrid Cloud NVD includes WORM as part of the backup repository to ensure immutable backups. You can also apply WORM to other object workloads if required; WORM is an optional part of this storage NVD. Files Storage supports share-level WORM. This NVD didn't test Files Storage WORM capabilities. 

Objects Storage supports versioning, which provides access to previous states (versions) of an object before it was changed. Unlike with WORM, previous versions aren't immutable and can be deleted. However, because the previous versions provide restore points that can prove useful in the event of data tampering, this design uses versioning. Nutanix Data Lens also requires versioning enablement on the Smart Tiering endpoint bucket. 

This design uses Nutanix Data Lens analytics (delivered as a service) to provide additional levels of security for the Files Storage and Objects Storage environments. Nutanix Data Lens adds security protection through the following capabilities: 

## **Auditing** 

Searchable audit trail of all file or object operations performed by each user. Nutanix Data Lens also tracks Files Storage client workstation IP addresses. 

© 2026 Nutanix, Inc. All rights reserved  | **35** 

Hybrid Cloud with AHV Unified Storage Design 

## **Anomalous event detection** 

Tracks administrator-defined behavioral patterns that signify potentially malicious activity. Email alerts are configurable. 

## **Ransomware protection** 

Signature-based detection uses the Nutanix Files ransomware file blocking mechanism to identify potential ransomware attacks and block files with a ransomware extension from carrying out malicious operations. The ransomware file blocking mechanism uses a curated list of more than 4,000 signatures that frequently appear in ransomware attacks. 

Event-pattern-based ransomware protection looks for audit events in near real time to identify potential ransomware attacks. Configuring automatic remediation allows you to block file users and clients suspected to be the source of a ransomware attack from accessing shares, preventing the ransomware attack from further infecting the files. 

When it detects probing activity (characterized by denial of access) and unexpected bucket policy changes among users, Objects Storage places those users on a watchlist. Over the next 30 days, Objects Storage generates alerts if users on the watchlist perform actions that might indicate attempts to delete, manipulate, or exfiltrate data. Optionally, you can set up policies to detect these event types for any user, not just those on the watchlist. 

You can configure email alerts on the ransomware page of the Nutanix Data Lens UI to notify staff of ransomware attacks as they occur. 

Reports let you know whether file shares have self-service restore snapshots enabled. Using self-service restore snapshots is a best practice, and this design enables them for all shares. 

A similar advisory exists for Objects Storage and highlights whether buckets have WORM protection and versioning enabled. 

_Table: Security Design Decisions Specific to Unified Storage_ 

|**Design Option**|**Validated Selection**|
|---|---|
|Objects Storage client connection security|Sign with an internal certificate authority.|



© 2026 Nutanix, Inc. All rights reserved  | **36** 

Hybrid Cloud with AHV Unified Storage Design 

|**Design Option**|**Validated Selection**|
|---|---|
|Certificates<br>Data-at-rest encryption (DaRE)<br>Client-server traffic encryption<br>File share permissions<br>Security analytics<br>Data immutability and versioning<br>Microsegmentation|Provision certificates with a yearly expiration<br>date and rotate accordingly.<br>Turn off DaRE; don't deploy a key<br>management server.<br>Enable HTTPS for Nutanix Objects; don't<br>enable SMB3 encryption or krb5p encryption.<br>Leave share permission setting at Full Control<br>and manage access with NTFS permissions.<br>Enable Pulse to establish link to Nutanix Data<br>Lens and permit analytics for the file-server<br>and object instances in AZ1 and AZ2. Define<br>anomalies and enable ransomware protection<br>with email alerting.<br>Enable versioning policy on deployed buckets.<br>Optionally enable WORM on buckets not<br>associated with Nutanix Files Smart Tiering.<br>Use microsegmentation to restrict traffic to the<br>Nutanix Files FSVMs and Nutanix Objects load<br>balancers to the required ports.|



## **Nutanix Prism and Object Store Certificates** 

With Objects Storage, self-signed Secure Socket Layer (SSL) certificates are generated by default. For control plane security in Nutanix Prism, the core Hybrid Cloud NVD describes replacing the default self-signed certificates with certificates signed by an internal certificate authority from a Microsoft public key infrastructure. You can choose alternative tools such as openssl for certificate generation and signing. These certificates secure communications between Objects Storage components and Nutanix Prism. 

Generate additional sets of certificates in the same way, using the same certificate authority, and apply them to the object store in each AZ to provide strong security for S3 client connections using the HTTPS protocol. S3 clients that interact with Objects Storage should have the trusted certificate authority chain preloaded. 

To facilitate disaster recovery, configure each object store with the fully qualified domain name (FQDN) of the other object store in addition to its own. This configuration allows each object store to respond to client requests intended for the other store. Therefore, 

© 2026 Nutanix, Inc. All rights reserved  | **37** 

Hybrid Cloud with AHV Unified Storage Design 

to ensure strong security for S3 client connections in a disaster recovery scenario, apply certificates for both FQDNs to each of the object stores. 

**Note:** Certificate management is an ongoing activity, and you must rotate certificates periodically. The NVD signs all certificates for one year of validity. 

## **Encryption for Hybrid Cloud with Unified Storage** 

AOS Storage can perform data-at-rest encryption (DaRE) at the cluster level; however, as the NVD doesn't have a stated requirement that warrants enabling it, this design doesn't use it on either the Files Storage or Objects Storage clusters. However, both Files Storage and Objects Storage support DaRE if it's required. You can also enable DaRE nondisruptively after cluster creation and data population. After you enable DaRE, existing data is encrypted in place, and all new data is written in an encrypted format. 

**Note:** To enable DaRE, you must also deploy an encryption key management solution such as the key management functionality built into Prism Central or a third-party key management system. 

Objects Storage supports HTTPS for secure, encrypted client communications according to the certificate considerations described previously. This design uses HTTPS encryption because it doesn't incur a significant performance penalty. 

Files Storage supports SMB3 encryption for SMB3 client-server traffic and Kerberos krb5p encryption for NFSv4 client-server traffic. This design doesn't include these encryption types because they incur a performance penalty, and this NVD doesn't have a requirement that warrants enabling them. 

## **Role-Based Access Control for Objects Storage and Files Storage** 

Objects Storage and Files Storage use the RBAC feature in Nutanix Prism Central. Files Storage and Objects Storage have out-of-the-box management roles, but you can also create custom roles. Use the RBAC capability if different Nutanix Unified Storage administrators have different responsibilities. You must enable IAM authentication in Nutanix Prism Central for Files Storage and Objects Storage RBAC to be available. IAM authentication is enabled by default in the Nutanix Prism Central version tested in this NVD. 

**Note:** RBAC in Objects Storage and Files Storage specifically defines administrator privileges and has nothing to do with managing user permissions. 

© 2026 Nutanix, Inc. All rights reserved  | **38** 

Hybrid Cloud with AHV Unified Storage Design 

Although the Objects Storage and Files Storage RBAC feature is available, it’s not part of this storage design. Because the Nutanix Prism Central instances that manage Files Storage and Objects Storage don’t manage any other Nutanix services, storage administrators are inherently limited to managing only storage. 

RBAC for Nutanix Data Lens is simple and straightforward. Assign user roles in the Admin Center at my.nutanix.com for users and administrators. 

## **Datacenter Infrastructure for Hybrid Cloud with Unified Storage** 

This design assumes that datacenters in the hosting region can sustain two AZs without intraregional fate-sharing—in other words, failures in one datacenter's physical plant or supporting utilities don't affect the other datacenter. 

This NVD recommends using a single rack that contains a Files Storage cluster and an Objects Storage cluster (four nodes for each cluster). You can expand both clusters in the rack to a total of eight nodes each. If you need additional storage expansion, you can go beyond the scope of this design and add more racks as needed, depending on topof-rack network switch density and the datacenter's power, weight, and cooling density capabilities per square foot. Subject to meeting these environmental requirements, you can expand existing Files Storage and Objects Storage clusters into neighboring racks. Consider scaling the backup clusters (described in the core Hybrid Cloud NVD) in proportion to the data growth in the Files Storage environment. 

For more information on the specific node models selected for this NVD, see the Platform Selection and Capacity Management section. In this design's physical rack space, one generic 42 RU rack contains 16 RU of systems with 3 RU reserved for two data switches and one out-of-band switch, leaving 23 RU of space available. Adding eight more nodes to the rack (four for Files Storage, four for Objects Storage) consumes an additional 16 RU, leaving 7 RU available. 

For network ports, the eight nodes in this storage NVD consume 8 ports on each of the two data switches. With 48 port switches, two Inter-Switch Links (ISLs), and two uplinks to the upstream network, this configuration leaves 36 ports available per data switch. Adding eight more nodes to the rack (four for Files Storage, four for Objects Storage) consumes 8 more ports per switch, leaving 28 unused ports available per switch. 

At a minimum, plan for the following power, cooling, and weight requirements: 

© 2026 Nutanix, Inc. All rights reserved  | **39** 

Hybrid Cloud with AHV Unified Storage Design 

- Power: 6,584 VA 

- Thermal: 22,448 Btu per hour 

- Weight: 476 lb 

Assume at least double these values for a fully loaded rack including network switches. The following figure shows the initial density for this design. 

Figure 3: Unified Storage Rack Layout 

© 2026 Nutanix, Inc. All rights reserved  | **40** 

Hybrid Cloud with AHV Unified Storage Design 

Datacenter selection is beyond the scope of this design; have a conversation about fully loaded racks with datacenter management before the initial deployment. Planning to properly support the environment's long-term growth might change where in the facility you set up the equipment. 

© 2026 Nutanix, Inc. All rights reserved  | **41** 

Hybrid Cloud with AHV Unified Storage Design 

## 5. Backup and Disaster Recovery for Hybrid Cloud with Unified Storage 

This storage NVD uses Files Storage Smart DR and Objects Storage streaming replication to provide a business continuity and disaster recovery (BCDR) solution to protect against different types of events. This section defines the overall high-level disaster recovery and backup designs. For more information on the backup storage design, see Backup and Disaster Recovery for On-Premises Hybrid Cloud in the Nutanix On-Premises Hybrid Cloud with AHV Design. 

This design provides the following recovery point objective (RPO) levels for data protection: 

- Gold Tier RPO: 1 minute 

- Silver Tier RPO: 1 hour 

- Bronze Tier RPO: 24 hours 

The solution provides an RTO of 1 hour, which includes client-side cache operations that are outside the influence of Files Storage Smart DR. 

This design doesn't provide a specific RPO for Objects Storage because this replication is streaming—data replicates as soon as the system writes the object on the source bucket. Depending on factors such as object size, network speed, and bandwidth, streaming replication might be nearly synchronous. 

To protect workloads against security threats like ransomware attacks, this NVD copies data to an external backup system that provides immutability. The backup target is a Nutanix Mine Integrated Backup cluster in each AZ, described in depth in the core Hybrid Cloud NVD. 

Unified Storage NVD BCDR requirements: 

- Place file shares with different levels of criticality in separate replication policies. 

- Configure Nutanix Files self-service restore snapshot schedules for share-level protection. 

© 2026 Nutanix, Inc. All rights reserved  | **42** 

Hybrid Cloud with AHV Unified Storage Design 

- Provide an RPO of 1 min for critical file data. 

- Provide an RPO of 1 hour for file data. 

- Provide an RPO of 24 hours for noncritical file data. 

- Support full failover (including networking). 

- Nutanix Files: Support automated DNS update and Service Principal Name transfer. 

- Nutanix Objects: Manage client redirection with a third-party GSLB (F5 BIG-IP). 

- Provide maximum automation and orchestration for failover and failback. 

- Provide read-only access of replicated data for testing purposes. 

- Simplify disaster recovery exercise, reducing human interaction to a minimum during disaster recovery. 

- Support the following disaster recovery events: 

   - › Datacenter outage. 

   - › Single cluster outage. 

   - › Ransomware attack. 

   - › Top-of-rack switch outage. 

   - › Single VLAN outage. 

   - › Human error. 

   - › Software bug. 

   - › Performance degradation caused by infrastructure (Nutanix cluster or network) or hardware components. 

- Ensure that tiered files remain accessible after AZ failover. 

- Ensure that file tiering activity resumes after AZ failback. 

- Choose a backup solution that supports Nutanix Files backup and restore using API, ideally CFT. 

- Choose a backup solution that supports Nutanix Files file-level backup and restore. 

© 2026 Nutanix, Inc. All rights reserved  | **43** 

Hybrid Cloud with AHV Unified Storage Design 

- Choose a backup solution that supports S3-compatible storage as a backup target. 

- Choose a target backup storage system that supports ransomware protection. 

- Choose a target backup storage system that supports WORM. 

- Choose a backup solution that supports replication to a secondary location. 

- Choose a backup solution that supports archiving to S3-compatible storage, including public cloud providers. 

**Note:** You must confirm every assumption in the following list. 

## Unified Storage NVD BCDR assumptions: 

- Disaster recovery avoidance causes minimal storage service downtime. 

- Customer provides redundant WAN connectivity between AZs. 

- Supporting infrastructure elements like DNS, Active Directory, and IP Address Management (IPAM) are available in both AZs. 

- Solution doesn't provide partial failover capabilities. 

## _Table: Unified Storage NVD BCDR Risks_ 

|**Risk Description**|**Impact**|**Likelihood**|**Mitigation**|
|---|---|---|---|
|Full outage of active|Large|Unlikely|Fail over to remote AZ.|
|AZ||||
|WAN link outage|Large|Unlikely|Provide redundant|
||||WAN connection.|
|Ransomware attack|Large|Likely|Set up|
||||antiransomware|
||||policies in Nutanix|
||||Data Lens. Implement|
||||backup solution with|
||||immutability. Replicate|
||||data to remote AZ.|
|Top-of-rack|Large|Unlikely|Use two top-of-|
|switch outage or|||rack switches for|
|misconfiguration|||redundancy.|



© 2026 Nutanix, Inc. All rights reserved  | **44** 

Hybrid Cloud with AHV Unified Storage Design 

|**Risk Description**|**Impact**|**Likelihood**|**Mitigation**|
|---|---|---|---|
|Single Nutanix cluster|Medium|Unlikely|Replicate data and fail|
|outage|||over to remote AZ.|
|Single VLAN outage or|Medium|Unlikely|Replicate data and fail|
|misconfiguration|||over to remote AZ.|
|Human error|Large|Likely|Introduce automation.|
||||Replicate data and fail|
||||over to remote AZ.|
|Performance|Large|Unlikely|Replicate data and fail|
|degradation caused|||over to remote AZ.|
|by infrastructure or||||
|hardware components||||
|(Nutanix clusters,||||
|network)||||



## _Table: Unified Storage NVD BCDR Constraints_ 

|**Constraint Description**|**Comment**|
|---|---|
|Use Nutanix Mine Integrated Backup for file|Although this NVD uses HYCU as the Nutanix|
|backup.|Mine Integrated Backup partner, solutions are|
||also available in partnership with Commvault,|
||Arcserve, and Veeam.|
|Use Nutanix Smart DR for Files Storage|Smart DR is the solution of choice to automate|
|disaster recovery automation.|file share failover.|
|Use GSLB for automated Nutanix Objects|The GSLB integrates with DNS to direct clients|
|client redirection during disaster recovery|to the live object store. Local and remote|
|events.|FQDNs applied to object stores in each AZ|
||ensure that the object store in the local AZ|
||can respond to S3 requests targeted at the|
||remote AZ’s object store. Apply the appropriate|
||certificates on each Nutanix Objects cluster.|



© 2026 Nutanix, Inc. All rights reserved  | **45** 

Hybrid Cloud with AHV Unified Storage Design 

|**Constraint Description**|**Comment**|
|---|---|
|Use identical names for source and target<br>buckets.|Bucket names must be identical to preserve<br>the fileserver-to-bucket tiering relationship in a<br>disaster recovery failover event. This naming<br>consistency, combined with the configuration<br>of the source object store’s FQDN as a<br>secondary FQDN on the disaster recovery<br>object store, ensures that the file server’s<br>tiering profile can locate the disaster recovery<br>bucket.|



## _Table: Unified Storage NVD BCDR Design Decisions_ 

|**Design Option**|**Validated Selection**|
|---|---|
|Automated disaster recovery failover and|Use Smart DR workflows for Nutanix Files. Use|
|testing|a GSLB for Nutanix Objects and apply a local|
||and a remote FQDN to each object store.|
|Support RPOs of 1 min, 1 hour, and 24 hours|Control Nutanix Files Smart DR with replication|
||policies and Nutanix Objects streaming|
||replication with replication rules.|
|Maximum number of shares for Smart DR|Use a maximum of 100 shares (20 distributed,|
|(Files Storage) and buckets for streaming|80 standard) and configure up to a 1-min RPO|
|replication (Objects Storage)|with 5 distributed and 20 standard shares.|
||Objects Storage replication does not have|
||limits.|
|Nutanix local and remote snapshot retention|Keep 12 hourly snapshots and 7 daily|
|policies|snapshots for file shares at both local and|
||remote AZs.|
|Nutanix local and remote versioning policies|Enable versioning for Objects Storage buckets|
||at both local and remote AZs.|



© 2026 Nutanix, Inc. All rights reserved  | **46** 

Hybrid Cloud with AHV Unified Storage Design 

|**Design Option**|**Validated Selection**|
|---|---|
|Relationship maintenance between Nutanix<br>Files Smart Tiering and Objects Storage in a<br>disaster recovery failover event<br>Workload backup within AZs or across AZs<br>RPO for backup policies<br>Backup policies per HYCU backup controller<br>Storage solution for backup repository<br>Number of S3 buckets to use as the backup<br>repository<br>Advanced features to enable on S3 storage|Use two file server instances per AZ: one<br>passive (receiving data replicated from the<br>alternate AZ) and one active. Only active file<br>servers can have an associated tiering profile.<br>After both the file share and object bucket<br>layers fail over, the file share’s reference to<br>the bucket remains valid because the Objects<br>Storage instances have secondary FQDNs<br>and the GSLB seamlessly directs files to the<br>surviving object store.<br>To optimize the backup window and save WAN<br>bandwidth, Nutanix Mine Integrated Backup<br>clusters back up file shares that are in the local<br>AZ.<br>Set a 24-hour RPO on backup policies.<br>Configure one HYCU policy.<br>Use Nutanix Mine Integrated Backup version 3<br>as the backup repository.<br>Use one object store with one bucket as the<br>backup repository.<br>Enable WORM and set it for 365 days.|



## **Disaster Recovery Design for Hybrid Cloud with Unified Storage** 

Nutanix Prism Central is the management and control plane for Files Storage and Objects Storage disaster recovery capabilities. Files Storage takes advantage of the Smart DR feature to use replication jobs to define replication frequencies on a share-byshare basis. Objects Storage lets you configure streaming replication between a source and a target Objects Storage instance on a bucket-by-bucket basis. HYCU backup policies configured for each share use the CFT API for incremental backups of the Files Storage environment. 

© 2026 Nutanix, Inc. All rights reserved  | **47** 

Hybrid Cloud with AHV Unified Storage Design 

Figure 4: Unified Storage BCDR Conceptual Design 

This NVD provides comprehensive disaster recovery protection for file shares and buckets across both AZs in a single region. Applications can use underlying infrastructure to provide disaster recovery resilience based on three protection levels with bidirectional replication between AZs. The design provides disaster recovery to the file servers and object store instances. 

Disaster recovery testing, failover, and failback are fully orchestrated and require only minimal human involvement. Each Nutanix Files cluster is registered to the Nutanix Prism Central instance in its respective AZ. The same is true for each Nutanix Objects cluster. 

The design uses four file servers, two in each AZ. In each AZ, one file server is passive (no connected clients, only receiving data replicated from the alternate site) while the other actively serves clients in the AZ. This configuration preserves the share-to-bucket Smart Tiering relationship in the event of a failover between AZs. Because each file server can have only one tiering profile, only the active file servers have Smart Tiering configured. If both the Nutanix Files and Nutanix Objects services fail over, the file share's reference to its bucket remains valid because of two key aspects of the design: 

- The Objects Storage instances have remote FQDNs that allow them to respond to S3 requests from the file server intended for the object store in the other AZ. 

© 2026 Nutanix, Inc. All rights reserved  | **48** 

Hybrid Cloud with AHV Unified Storage Design 

- The GSLB seamlessly directs reads of tiered data initiated by the file server to the disaster recovery object store. 

Figure 5: Unifed Storage BCDR Logical Design 

The NVD provides three protection tiers for Files Storage. Objects Storage uses continuous replication, where RPO depends on the object size, bandwidth, and latency between AZs. 

## _Table: Nutanix Files Disaster Recovery Protection Tiers_ 

|**Tier**|**RPO**|**RTO**|
|---|---|---|
|Gold|1 minute|1 hour|
|Silver|1 hour|1 hour|
|Bronze|1 day|1 hour|



## _Table: Nutanix Objects Disaster Recovery Protection Tiers_ 

|**Tier**|**RPO**|**RTO**|
|---|---|---|
|Continuous|Real time (variable)|1 hour|



© 2026 Nutanix, Inc. All rights reserved  | **49** 

Hybrid Cloud with AHV Unified Storage Design 

## _Table: Nutanix Files Replication Policy Configuration_ 

|**Policy Name**|**Source Cluster**|**Target Cluster**|**RPO**|
|---|---|---|---|
|AZ01-AZ02-Bronze-01|AZ01-FS-01|AZ02-FS-02|1 day|
|AZ01-AZ02-Silver-01|AZ01-FS-01|AZ02-FS-02|1 hour|
|AZ01-AZ02-Gold-01|AZ01-FS-01|AZ02-FS-02|1 minute|
|AZ02-AZ01-Bronze-01|AZ02-FS-03|AZ01-FS-04|1 day|
|AZ02-AZ01-Silver-01|AZ02-FS-03|AZ01-FS-04|1 hour|
|AZ02-AZ01-Gold-01|AZ02-FS-03|AZ01-FS-04|1 minute|



_Table: Nutanix Objects Replication Policy Configuration_ 

|**Policy Name**|**Source Cluster**|**Target Cluster**|**RPO**|
|---|---|---|---|
|Applied on source|AZ01-OBJ-01|AZ02-OBJ-02|Continuous replication|
|bucket||||
|Applied on source|AZ02-OBJ-02|AZ01-OBJ-01|Continuous replication|
|bucket||||



You can run Nutanix Files failover operations at the file server level from Prism Central in your recovery AZ. If Prism Central isn't available, you can run the same workflow using the CLI on the target file server. All shares in replication policies fail over together when you recover a file server on a target cluster. 

A GSLB controls Nutanix Objects failover. This storage NVD tested F5 BIG-IP as the GSLB. The F5 GSLB acts as a DNS server and forwards requests to the appropriate Nutanix Objects instance based on availability. With both Nutanix Files and Nutanix Objects, clients automatically redirect to the target site because the namespaces move as a part of the disaster recovery orchestration. This approach also ensures that Smart Tiering relationships between the Nutanix Files and Nutanix Objects layers remain intact. 

© 2026 Nutanix, Inc. All rights reserved  | **50** 

Hybrid Cloud with AHV Unified Storage Design 

Figure 6: F5 BIG-IP Configuration for Objects Storage 

## **Backup Design for Hybrid Cloud with Unified Storage** 

The core Hybrid Cloud NVD provides a backup option for workloads running in both AZs. This design, which extends to file data, optimizes the backup solution to back up workloads that run locally to the backup cluster. This design uses Nutanix Mine Integrated Backup infrastructure, described in the core Hybrid Cloud NVD, to back up file data. Several backup vendors can very efficiently back up Files Storage using the Changed File Tracking (CFT) API. This NVD uses HYCU as the backup application because HYCU supports the CFT API and provides a simplified, efficient, and wellintegrated backup capability for Files Storage. 

© 2026 Nutanix, Inc. All rights reserved  | **51** 

Hybrid Cloud with AHV Unified Storage Design 

In Files Storage you apply a backup policy to source shares and exports. The FSVMs that make up the Files Storage instance are stateless and not backed up. HYCU integrates with the Nutanix Files CFT API to ensure efficient incremental backup of shares as data changes. HYCU also supports backing up shares that are either the source or the target for Smart DR replication. This storage NVD uses Smart DR to replicate Nutanix Files data between AZ1 and AZ2 instead of HYCU-based replication. HYCU policies then back up the local copy of each file share, whether it's a source or target for Smart DR replication. 

Figure 7: Unified Storage Backup Architecture Logical Design 

Objects Storage backup is out of scope for this NVD, which protects Objects Storage data using remote replication and disaster recovery capabilities. Instead of backing up the object stores, this NVD protects object data by replicating buckets between the Objects Storage clusters in each AZ. Bucket replication is the conventional approach to protecting object stores, which often grow too large to be backed up using traditional methods. 

© 2026 Nutanix, Inc. All rights reserved  | **52** 

Hybrid Cloud with AHV Unified Storage Design 

## 6. Ordering 

This bill of materials reflects the validated and tested hardware, software, and services that Nutanix recommends to achieve the outcomes described in this document. Consider the following points when you build your orders: 

- All software licensing is based on storage capacity. 

- Nutanix Professional Services or an affiliated partner selected by Nutanix provides all services. 

- Nutanix based the functional testing described in this document on NX series models with similar configurations to validate the interoperability of software and services. 

**Note:** Because available hardware, software, and services can change without notice, contact Nutanix Sales when ordering to ensure that you have the correct product codes. 

If you must make substitutions in the specific list of validated and tested hardware, software, or services, note the following guidance: 

- If a specific hardware configuration is unavailable, you can choose a similar option that meets or exceeds the recommended specification. 

Contact Nutanix Sales or use Nutanix Sizer to determine alternate 

configurations and identify additional OEM options. 

- You can make hardware substitutions to suit your preferences; however, such changes might result in a solution that does not deliver the exact outcomes described in this NVD. 

- You must avoid software product code substitutions except in the following instances: 

   - › You must use different quantities to maintain software licensing compliance. 

   - › You prefer a higher license tier or support level for the same software product code. 

- Adding any software or workloads that aren't specified in this design to the environment (including additional Nutanix products) might affect the validated density 

© 2026 Nutanix, Inc. All rights reserved  | **53** 

Hybrid Cloud with AHV Unified Storage Design 

calculations and result in a solution that doesn't follow the recommended Nutanix configuration. 

## **Sizing Considerations** 

This NVD is based on one 4-node Files Storage cluster and one 4-node Objects Storage cluster in each AZ for BCDR. You can use backup clusters in the core Hybrid Cloud NVD to back up the file data. 

A 4-node cluster is the minimum size, but you can increase the Files Storage and Objects Storage clusters incrementally up to 8 nodes each while remaining in a single rack. If you need even more storage capacity, you can expand the clusters into adjacent racks. Files Storage can use up to 32 nodes of storage capacity and up to 16 nodes of compute resources for client I/O processing. Objects Storage can use up to 48 nodes in a single cluster (both compute and storage) with the option to incorporate storage from up to four other physical clusters into the namespace. 

## **Primary and Secondary Nutanix Files Clusters Bill of Materials** 

The following section shows the bills of materials for the primary and secondary Files Storage clusters. 

_Table: Primary and Secondary Files Storage Clusters: Hardware_ 

|**Item**|**Specification**|
|---|---|
|Platform|NX-NX8155-G8|
|Configuration|One node|
|Type|Hybrid|
|Support Level|Production|
|NRDK Support|No|
|NR Node Support|No|



_Table: Primary and Secondary Nutanix Files Clusters: Per-Node Hardware Configuration_ 

© 2026 Nutanix, Inc. All rights reserved  | **54** 

Hybrid Cloud with AHV Unified Storage Design 

|**Component**|**Description**|**Quantity**|
|---|---|---|
|Processor|Intel Xeon-Silver 4310|2|
||processor (2.1 GHz, 12-core,||
||120 W, Ice Lake)||
|Memory|32 GB (3,200 MHz DDR4|8|
||RDIMM)||
|HDD|18 TB|8|
|SSD|7.68 TB|4|
|Network adapter|25 GbE, 2-port (NVIDIA|1|
||Mellanox ConnectX-5)||



Use the following software for the primary and secondary Nutanix Files clusters: 

- Nutanix Unified Storage Pro subscription (contains Nutanix Files) 

- Nutanix Unified Storage Security Add-On Software 

- Nutanix Cloud Manager (NCM) Starter 

- Nutanix Data Lens 

Nutanix recommends the following Nutanix Professional Services subscriptions for the primary and secondary Nutanix Files clusters: 

- Infrastructure Deploy - On-Prem NCI Cluster 

- Nutanix Unified Storage Deployment 

## **Primary and Secondary Objects Storage Clusters Bill of Materials** 

The following section shows the bills of materials for the primary and secondary Objects Storage clusters. 

_Table: Primary and Secondary Objects Storage Clusters: Hardware_ 

|**Item**|**Specification**|
|---|---|
|Platform|NX-NX8155-G8|
|Configuration|One node|
|Type|Hybrid|



© 2026 Nutanix, Inc. All rights reserved  | **55** 

Hybrid Cloud with AHV Unified Storage Design 

|**Item**|**Specification**|
|---|---|
|Quantity<br>Support Level<br>NRDK Support<br>NR Node Support|8<br>Production<br>No<br>No|



_Table: Primary and Secondary Nutanix Objects Clusters: Per-Node Hardware Configuration_ 

|**Component**|**Description**|**Quantity**|
|---|---|---|
|Processor|Intel Xeon-Silver 4310|2|
||processor (2.1 GHz, 12-core,||
||120 W, Ice Lake)||
|Memory|32 GB (3,200 MHz DDR4|4|
||RDIMM)||
|HDD|18 TB|10|
|SSD|3.84 TB|2|
|Network adapter|25 GbE, 2-port (NVIDIA|1|
||Mellanox ConnectX-5)||



Use the following software for the primary and secondary Nutanix Objects clusters: 

- Nutanix Unified Storage Pro subscription (contains Nutanix Objects) 

- Nutanix Unified Storage Security Add-On Software 

- NCM Starter 

- Nutanix Data Lens 

Nutanix recommends the following Nutanix Professional Services subscriptions for the primary and secondary Nutanix Objects clusters: 

- Infrastructure Deploy - On-Prem NCI Cluster 

- Nutanix Unified Storage Deployment 

© 2026 Nutanix, Inc. All rights reserved  | **56** 

Hybrid Cloud with AHV Unified Storage Design 

## **Nutanix Professional Services** 

To ensure a seamless experience across design, migration planning, implementation, and optimization of the Nutanix platform for both virtualized and containerized applications, we strongly recommend engaging Nutanix Professional Services. 

Reach out to your Nutanix Account Team to discuss the recommended services tailored to your environment. Using these expert services helps reduce complexity and risk at every stage of your journey. 

© 2026 Nutanix, Inc. All rights reserved  | **57** 

Hybrid Cloud with AHV Unified Storage Design 

## 7. Test Plan 

The test plan for the hybrid cloud with unified storage NVD validates a successful implementation (spreadsheet automatically downloads when you click the link). Compare the result for each test plan item with the **Expected Result** column, then select the correct response from the dropdown menu in the **Result** column. 

© 2026 Nutanix, Inc. All rights reserved  | **58** 

Hybrid Cloud with AHV Unified Storage Design 

## 8. References 

For more information on the components that make up on-premises hybrid cloud deployments on Nutanix, see the following references: 

- Nutanix Platform Architecture for Datacenters 

- Nutanix On-Premises Hybrid Cloud with AHV Design 

- Nutanix Files 

- Nutanix Objects 

- Physical Networking Best Practices 

© 2026 Nutanix, Inc. All rights reserved  | **59** 

Hybrid Cloud with AHV Unified Storage Design 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **60** 

Hybrid Cloud with AHV Unified Storage Design 

## **List of Figures** 

Figure 1: Conceptual Storage Design..........................................................................................................15 Figure 2: Unified Storage Monitoring Design...............................................................................................30 Figure 3: Unified Storage Rack Layout........................................................................................................40 Figure 4: Unified Storage BCDR Conceptual Design.................................................................................. 48 Figure 5: Unifed Storage BCDR Logical Design..........................................................................................49 Figure 6: F5 BIG-IP Configuration for Objects Storage...............................................................................51 Figure 7: Unified Storage Backup Architecture Logical Design...................................................................52 


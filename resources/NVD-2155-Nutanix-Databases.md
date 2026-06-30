# **Nutanix Database Service Design** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Code samples and snippets that appear in this content are unofficial, are unsupported, and will require extensive modification before use in a production environment. As such, the code samples and snippets are provided AS IS and are not guaranteed to be complete, accurate, or up-to-date. Nutanix makes no representations or warranties of any kind, express or implied, as to the operation or content of the code samples or snippet. Nutanix expressly disclaims all other guarantees, warranties, conditions and representations of any kind, either express or implied, and whether arising under any statute, law, commercial use or otherwise, including implied warranties of merchantability, fitness for a particular purpose, title and non-infringement therein. 

This content reflects an experiment in a test environment. Results, benefits, savings, or other outcomes described depend on a variety of factors including use case, individual requirements, and operating environments, and this publication should not be construed as a promise or obligation to deliver specific outcomes. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Nutanix Database Service Design 

## **Contents** 

**1. Nutanix Database Service Design Executive Summary.......................5 2. Nutanix Database Service Design Software Versions..........................9 3. Nutanix Terminology..............................................................................10 4. Nutanix Platform Architecture..............................................................12 5. Nutanix Database Service.....................................................................13 6. Nutanix Database Solution Usage Scenarios..................................... 16** Provisioning a Single-Instance Database.........................................................................................16 Provisioning a Clustered Database Instance....................................................................................18 Provisioning a Development and Test Database..............................................................................19 **7. Core Infrastructure Design for Nutanix Database Service.................21** Scalability Design for Nutanix Database Service............................................................................. 22 Resilience Design for Nutanix Database Service.............................................................................23 VM Design for Nutanix Database Service........................................................................................23 Cluster Design for Nutanix Database Service..................................................................................24 Storage Design for Nutanix Database Service.................................................................................26 Management Components for Nutanix Database Service............................................................... 32 Monitoring for Nutanix Database Service.........................................................................................33 Security and Compliance for Nutanix Database Service..................................................................42 **8. Databases on Nutanix........................................................................... 43** Configuring Nutanix Database Service.............................................................................................44 SQL Server Implementation Best Practices..................................................................................... 44 Oracle Implementation Best Practices..............................................................................................46 PostgreSQL Implementation Best Practices.....................................................................................47 

**9. Software Patching with Nutanix Database Service............................ 52** Microsoft SQL Server Database Software Patching.........................................................................52 Patch Oracle Database Software..................................................................................................... 54 Patch PostgreSQL Database Software............................................................................................ 56 

**10. Nutanix AHV High Availability............................................................ 58** Unplanned SQL Server VM Migration.............................................................................................. 58 Planned SQL Server VM Migration.................................................................................................. 60 

**11. Nutanix Database Service Time Machine Backup and Recovery..................................................................................................62** Validating Time Machine Data.......................................................................................................... 62 Verifying Microsoft SQL Server Backup and Restore...................................................................... 63 Oracle Database Restore................................................................................................................. 64 Validating PostgreSQL Backup and Restore....................................................................................66 

**12. Ordering Nutanix Database Service Deployments........................... 67** Sample Bill of Materials....................................................................................................................68 Nutanix Professional Services.......................................................................................................... 68 

**13. Test Plans for Nutanix Database Service Design............................. 70 14. References and Resources for Nutanix Database Service Design......................................................................................................71 About Nutanix.............................................................................................72 List of Figures.............................................................................................................................................73** 

Nutanix Database Service Design 

## 1. Nutanix Database Service Design Executive Summary 

Enterprise database deployments aren't a one-size-fits-all proposition. Requirements range from many small database servers with mixed operating systems and database software vendors to large, mission-critical datastores and from web-scale environments where the redundancy is built into the software to environments where infrastructure provides availability and disaster recovery. This Nutanix Validated Design (NVD) offers components you can combine, including validation steps, to ensure that your deployment meets best practice standards and is ready to support your business. 

The Nutanix Database Service (NDB) solution orchestrates and simplifies database deployments by incorporating database best practices so that you don't have to perform tasks manually at the infrastructure layer. Some of the NDB tasks related to infrastructure include compute creation, storage management, affinity rules, and storage mapping to database servers. 

Key features: 

- Simplified database management with NDB 

- Choice of hypervisor and hardware vendors with the latest Intel or AMD processors and latest storage technologies 

- Enterprise-grade performance with software-defined architecture 

- Reduced total cost of ownership compared to the public cloud 

- Hybrid multicloud capabilities 

Advantages: 

- A validated, preintegrated solution simplifies large-scale NDB deployment. 

- Modular design provides flexible scalability and resilience for the management and control planes so that you can grow your database environment incrementally. 

© 2026 Nutanix, Inc. All rights reserved  | **5** 

Nutanix Database Service Design 

- A validated reference design reduces implementation risk and decreases the time to value for customer deployments. 

- Applying vendor-validated best practices promotes reliability and supportability. 

- Automation and standard VM sizes prepare your environment for operational consistency, predictable performance, and easier management. 

- Database life cycle operations—such as provisioning, patching, backups, and scaling —continue to function even if a control plane VM or cluster is unavailable. 

Benefits: 

- Faster time to value: A validated design with a comprehensive bill of materials accelerates deployment and reduces trial and error. 

- Improved IT agility: You can rapidly provision and scale workloads in response to business needs. 

- Reduced risk: Built-in backup and disaster recovery protect applications and the entire database ecosystem to provide business continuity. 

- Lower total cost of ownership: Efficient resource utilization and automation reduce operational overhead. 

Nutanix rigorously tests and documents solutions in the NVD program to align with best practices and support faster, more resilient deployments, giving IT teams a trusted foundation for building enterprise-grade private cloud environments. 

If you plan to deploy databases without NDB, see the best practice guide for your specific database engine: 

- Oracle 

- Microsoft SQL Server 

- PostgreSQL 

- MySQL 

- MongoDB 

_Table: Document Version History_ 

© 2026 Nutanix, Inc. All rights reserved  | **6** 

Nutanix Database Service Design 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|May 2022|Original publication.|
|1.1|June 2022|Updated the Hardware and|
|||NVD Test Cluster Details|
|||sections.|
|1.2|July 2022|Updated the Appendix section.|
|1.3|September 2022|Aligned with Hybrid Cloud:|
|||AOS 5.20 with AHV On-|
|||Premises Design.|
|2.0|November 2022|Added PostgreSQL content.|
|2.1|December 2022|Updated the Infrastructure|
|||Design and Performance|
|||Results sections.|
|2.2|June 2023|Updated the Executive|
|||Summary, Infrastructure|
|||Design, Nutanix Database|
|||Service, and Performance|
|||Results sections.|
|3.0|October 2023|Updated for AOS 6.5.3 and|
|||NDB 2.5.2. Added the Network|
|||Microsegmentation and|
|||Monitoring sections. Removed|
|||the Performance Results,|
|||SQL Server HammerDB|
|||Script File, SQL Server|
|||Performance Measurement,|
|||and PostgreSQL HammerDB|
|||Script File sections.|
|3.1|November 2023|Updated the Application|
|||Connectivity Requirements|
|||and Patch PostgreSQL|
|||Database Software sections.|
|3.2|September 2024|Updated the Sample Bill of|
|||Materials section.|
|3.3|November 2024|Updated AOS release cycle|
|||descriptions.|



© 2026 Nutanix, Inc. All rights reserved  | **7** 

Nutanix Database Service Design 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|3.4<br>4.0<br>4.1<br>4.2<br>4.3|March 2025<br>Added the NDB Alert<br>Management section.<br>May 2025<br>Updated for AOS 6.10 and<br>NDB 2.8.<br>February 2026<br>Updated document structure.<br>March 2026<br>Updated the Databases on<br>Nutanix section.<br>May 2026<br>Updated the Scalability Design<br>for Nutanix Database Service<br>section.|



© 2026 Nutanix, Inc. All rights reserved  | **8** 

Nutanix Database Service Design 

## 2. Nutanix Database Service Design Software Versions 

The following table summarizes the software that Nutanix validated for functionality and interoperability with this solution. 

_Table: Software Versions Used in Validation Testing_ 

|**Component**|**Software Version**|
|---|---|
|Nutanix AOS|6.10|
|Nutanix Database Service|2.8|
|PostgreSQL|16.6|
|RHEL|8.10|



© 2026 Nutanix, Inc. All rights reserved  | **9** 

Nutanix Database Service Design 

## 3. Nutanix Terminology 

This document uses the following terms to refer to different elements of the Nutanix Database Service design solution. 

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

© 2026 Nutanix, Inc. All rights reserved  | **10** 

Nutanix Database Service Design 

## **Block** 

A block is a Nutanix cluster or a pair of clusters that are located in different AZs. 

## **Fault domain** 

Fault domains are groups of VMs that share a common power source, network infrastructure, server rack, Nutanix cluster, or datacenter location. 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

Nutanix Database Service Design 

## 4. Nutanix Platform Architecture 

The Nutanix hyperconverged solution combines storage, compute, networking, and virtualization into an industry-proven x86 platform for hosting database workloads. 

For details on the Nutanix platform architecture, hyperconverged infrastructure, and software components, see Nutanix On-Premises Hybrid Cloud with AHV Design and Nutanix Platform Architecture for Datacenters. 

Figure 1: Nutanix Platform for Databases Architecture 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

Nutanix Database Service Design 

## 5. Nutanix Database Service 

Nutanix Database Service (NDB) is a software suite that automates and simplifies database administration, bringing simplicity and invisible operations to database provisioning and life cycle management. With one-click database provisioning, copy data management, database protection, and patching, NDB enables database administrators to provision, clone, and refresh their databases to any point in time. The NDB APIfirst architecture can easily integrate with your preferred self-service tools, and every operation has a unique ID that's fully visible for auditing. 

Figure 2: Nutanix Database Service Features 

The Nutanix Database Service (NDB) management plane operates on one or more VMs running in a Nutanix cluster with at least three Nutanix nodes (physical servers). The default NDB management plane deployment in a single Nutanix cluster consists of one VM running front-end services (API, agent, web service) and the back-end service (database or repository). The following figure presents a logical implementation of the 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

Nutanix Database Service Design 

Nutanix database as a service (DBaaS) offering, including the NDB management VM, the logical components, databases, and one Nutanix cluster. 

Figure 3: Nutanix Database Service Architecture 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

Nutanix Database Service Design 

NDB is available as a VM shipped as a QCOW2 or OVA image preloaded with a web server, a Postgres repository to hold the database metadata, and the NDB agent. 

The NDB agent is deployed on the NDB server and on each of the database server VMs provisioned by NDB. It contains the tools required to perform each task (for example, snapshot, provision, and clone). 

You can connect to NDB through the web interface, CLI, or REST APIs. NDB uses an API-first architecture. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

Nutanix Database Service Design 

## 6. Nutanix Database Solution Usage Scenarios 

With a high-level understanding of the Nutanix platform and Nutanix Database Service (NDB), you can plan database deployments on Nutanix. The following deployment scenarios are validated in this Nutanix Validated Design (NVD) and apply to Oracle, Microsoft SQL Server, and PostgreSQL databases: 

- Single-instance database 

- Clustered database (Oracle RAC, SQL Server Always On availability group, or PostgreSQL High Availability) 

- Development and test database maintenance 

## **Provisioning a Single-Instance Database** 

This single-instance workflow deploys one or more single-server database instances in a greenfield environment. The first step is creating a new database server VM template image and using that template image to provision a database server VM using Nutanix Database Service (NDB). 

For the Nutanix Validated Design (NVD) testing, we provisioned a database server VM using NDB with a software profile generated from a newly created VM located in the Nutanix cluster. The validation includes Oracle, SQL Server, and PostgreSQL databases. To use NDB to provision a single-server database instance, follow these steps: 

**1.** Create a new database VM containing database software. 

The database type determines the software (Windows Server 2022 and SQL Server 2022, RHEL 8.10, Oracle Database 19c RU 24, Oracle Grid Infrastructure 19c RU 24, or PostgreSQL 16.6 with RHEL 8.10). NDB captures the OS disks and the disks with the database software from the source VM when creating the software profile. For more information, see 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

Nutanix Database Service Design 

Creating a Windows VM on AHV with Nutanix VirtIO and Creating a VM (AHV). 

**2.** Install any other required software on the OS drive. 

**3.** Run a check script before registering the database. 

A check script isn't required for SQL Server on Windows. For more information related to Oracle or PostgreSQL databases, see Database Server VM Registration Prerequisite Checks. 

**4.** Register the newly created VM with NDB. 

For more information on database server VM registration, see Oracle Database Server VM Registration, SQL Server Database Server VM Registration, or PostgreSQL Database Server VM Registration. 

**5.** Create a software profile from the newly registered VM. 

For more information on creating a software profile, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

**6.** Create compute and network profiles to provision databases. 

For more information on creating a compute profile, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. For more information on creating a network profile, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

**7.** Complete the appropriate parameter-definition step based on your database software: 

   - Create a database parameter profile for Oracle. 

   - Create a database parameter profile and Windows domain profile for SQL Server. 

   - Create a database parameter profile for PostgreSQL. 

**8.** Provision a database using the NDB profiles. 

For more information on provisioning a database, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL databases. 

**9.** Sign in to the new VM. 

**10.** Verify that the VM was configured as expected. 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

Nutanix Database Service Design 

## **Provisioning a Clustered Database Instance** 

This clustered database workflow deploys clustered database instances in a greenfield environment. The first step is creating a new database server VM template image (containing clustered software components) and generating a clustered software profile from that template image. Using the clustered software profile, we provisioned a clustered database instance using Nutanix Database Service (NDB). 

The validation includes Oracle, SQL Server, and PostgreSQL databases. 

To use NDB to provision a clustered database instance, follow these steps: 

**1.** Create a new database VM containing database software. 

The database type determines the software (Windows Server 2022 and SQL Server 2022; RHEL 8.10, Oracle Database 19c RU 24, Oracle Grid Infrastructure 19c RU 24, or PostgreSQL 16.6 with RHEL 8.10). Patroni high availability components are haproxy 1.5.18, patroni 3.0.2, and etcd 3.3.11. NDB captures the OS disks and disks that the database software is installed on from the source VM when creating the software profile. For more information, see AHV: Create Windows VM and AHV: Create VM (Linux). 

**2.** Install any other required software on the OS drive. 

**3.** Run a check script before registering the database. 

A check script isn't required for SQL Server on Windows. For more information on database server VM registration prerequisite checks, see Nutanix documentation for Oracle or PostgreSQL. 

**4.** Register the newly created VM with NDB. 

For more information on database server VM registration, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

**5.** Create a software profile from the newly registered VM. 

For more information on creating a software profile, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

© 2026 Nutanix, Inc. All rights reserved  | **18** 

Nutanix Database Service Design 

**6.** Create compute and network profiles to provision the database. 

For more information on creating a compute profile, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

For more information on creating a network profile, see Nutanix documentation for Oracle RAC Database, Windows Clusters, or PostgreSQL High Availability Instance. 

**7.** Complete the appropriate parameter-definition step based on your database software: 

   - Create a database parameter profile for Oracle. 

   - Create a database parameter profile and a Windows domain profile for SQL Server. 

   - Create a database parameter profile for PostgreSQL. 

**8.** Provision a database server VM using the NDB profiles. 

For more information on provisioning a database server VM using NDB profiles, see Nutanix documentation for Oracle RAC Database, SQL Server Availability Database, or PostgreSQL High Availability Instance. 

**9.** Sign in to the new database VM. 

**10.** Verify that the cluster and the database were configured as expected. 

## **Provisioning a Development and Test Database** 

Nutanix Database Service (NDB) simplifies the process of provisioning development and test databases by creating a production database that NDB registers and manages. The time machine associated with the production database has a service-level agreement (SLA) defined for backup frequency and retention. The snapshots created by the time machine maintain nonproduction copies from the production database for testing, development, and troubleshooting purposes, including automatically refreshing the copies at regular intervals. 

To use NDB to provision a development and test database, follow these steps: 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

Nutanix Database Service Design 

## **1.** Register the production database VM with NDB. 

For more information on database server VM registration, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

**2.** Create compute and network profiles to create nonproduction databases. 

For more information on creating a compute profile, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

For more information on creating a network profile, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

**3.** Complete the appropriate step based on your database software: 

   - Create a database parameter profile for Oracle. 

   - Create a database parameter profile and a Windows domain profile for SQL Server. 

   - Create a database parameter profile for PostgreSQL. 

**4.** Validate the time machine for the production database. 

For more information, see Time Machine Behavior and Functionality. 

**5.** Create a clone from a source database and set a schedule for automatic refresh. 

For more information on creating single-node database clones, see Nutanix documentation for Oracle, SQL Server, or PostgreSQL. 

**6.** Define a schedule to automatically or manually refresh the clones. 

For more information, see Refreshing Database Clones (Manual). 

**7.** Sign in to the clone VM. 

**8.** Connect to the clone database. 

**9.** Verify the source data. 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

Nutanix Database Service Design 

## 7. Core Infrastructure Design for Nutanix Database Service 

To begin the design process, collect your application requirements. Use your requirements to select hardware and apply best practices throughout your cluster. The following guidelines apply to selecting hardware and configuring clusterwide settings for a production database service. If you aren't deploying a mission-critical production application, you can change attributes like the CPU type, vCPU-to-CPU overcommit ratio, network configuration, and amount of free storage to meet your needs. 

Use the following guidelines when selecting hardware for production databases: 

- Select a processor with a high clock speed and 12 or more cores per socket on dualsocket servers. Nutanix recommends selecting a CPU with a clock rate of at least 2.7 GHz. 

- Use a balanced memory configuration. A balanced memory configuration delivers the highest bandwidth between the processor and memory and the best performance for CPU-intensive applications. The number of DIMMs required for balanced memory varies by model. 

- Ensure that you have sufficient memory to allocate a minimum of 64 GB to the Nutanix Controller VM (CVM). 

- Select a minimum of dual 25 GbE or faster network interface. 

- Use all NVMe or a mix of NVMe and SSD storage. 

For more information on OEM and third-party hardware vendors, see Supported Hardware Platforms and Public Clouds. 

## **Storage hardware** 

Use all NVMe or a mix of NVMe and SSD storage hardware for all databases on Nutanix. A mix of NVMe and SSD offers a high price-performance ratio, making it the best option for a variety of workloads. 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

Nutanix Database Service Design 

Don't use HDD storage for clusters running databases. You can use hybrid clusters with a mix of SSD and HDD storage for backup or archive targets to reduce costs. 

## **CPU** 

For production database VMs, don't overcommit CPU. If your entire cluster isn't dedicated to production database VMs, you can use affinity rules to keep critical VMs isolated on nodes and increase vCPU overcommit on cluster nodes that run other services (like development, support, and backup database instances). 

Nutanix isn't responsible for database license compliance. It's your responsibility to ensure compliance with database engine licensing requirements. 

## **Memory** 

Configure your infrastructure with balanced memory and ensure sufficient memory for the CVM. A balanced memory configuration can greatly improve application performance because it uses all memory channels to transfer data between memory and CPU. 

- In Intel G9 configurations, balanced memory means that you have 16 or 32 DIMMs populated in a two-socket server. 

- The Nutanix CVM requires 64 GB of memory (32 GB more than the default) to support a high-performance database workload. Increasing CVM memory can improve random I/O performance by providing more space to cache metadata. 

For more information, see Nutanix Physical Memory Configuration. 

## **Network** 

Each node must have at least two 25 GbE or faster uplinks, and all nodes in a Nutanix cluster must be connected to the same pair of switches. 

For more information, see AHV Networking Best Practices. 

## **Scalability Design for Nutanix Database Service** 

Scalability is one of the core concepts of the Nutanix platform and refers to the ability to increase storage and compute capacity to meet current and future workload demands. A well-designed cluster meets current requirements while providing a path to support future growth. 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

Nutanix Database Service Design 

Although storing logs on volume groups is viable for small environments, it becomes inefficient at scale—particularly when managing over 200 time machines. Objects Storage can support up to 300 time machines with a log catchup frequency of 15 minutes, making it an ideal back-end for log-intensive database environments. For more information, see NDB Control Plane Configuration and Scalability. 

## **Resilience Design for Nutanix Database Service** 

Nutanix provides many resilience features, including storage replication, snapshots, block awareness, degraded node detection, and self-healing. These capabilities increase the resilience of all workloads, even if the application itself has limited resilience options. Nutanix layers these software features on hardware designed to be resilient (for example, with redundant physical components and power supplies, many of which are hot-swappable or otherwise easily serviceable). Running workloads in a virtualized environment adds another kind of resilience; you can perform many maintenance operations without application downtime. A resilient network fabric that can sustain individual link, node, or block failures without significant impact completes the architecture. 

All components are physically redundant. The physical components include the top-ofrack switches, the nodes and their internal parts, and the datacenter itself in case of a disaster. 

To protect workloads to meet or exceed service-level agreements (SLAs), this NVD separates the workload clusters from the management cluster. The workload cluster sizing provides n + 1 failure redundancy. Monitoring and alerting ensure that any issues result in an alert; consistently monitoring workload growth ensures that sufficient headroom is available at any time. 

## **VM Design for Nutanix Database Service** 

To support a wide range of database workload scenarios, this design establishes three standard VM sizes to facilitate consistent deployment, automation, sizing, and capacity planning for the environment. You can combine any number of VMs of any size up to the maximums that Nutanix designed this architecture to support. All VMs deploy with UEFI and Secure Boot enabled. 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

Nutanix Database Service Design 

Nutanix recommends keeping the VM name and guest OS host name the same. This approach streamlines operational and support requirements and minimizes confusion when you identify systems in the environment. 

## **Cluster Design for Nutanix Database Service** 

This design incorporates three distinct cluster types: 

- Management: Critical infrastructure and environment management workloads 

- Workload: Dedicated environment for running database workloads 

- Nutanix Objects: Backend storage for time machine log backups 

Nutanix recommends deploying a cluster that can fully protect your data and maintain cluster high availability in the case of a node failure. Use an n + 1 configuration, where n is the number of nodes in the cluster. Nodes must have sufficient CPU, memory, and storage capacity to maintain redundancy when a node failure occurs. 

Figure 4: Sample Four-Node Cluster Hosting SQL Server Databases on Nutanix 

For all database clusters, start with four nodes, the smallest possible n + 1 cluster. Ensure that you maintain n + 1 as you add nodes to the cluster. When you have at least four nodes, enable additional configuration options. Enable high availability reservation using Prism to ensure that you have sufficient CPU and memory to fail all the VMs from any node over to other nodes in the event of a node failure. 

To ensure that you have sufficient storage capacity, configure Prism so that it generates a warning when the cluster reaches the threshold for resilient capacity (the storage level at which you can still fully recover the data even if a failed node doesn't rejoin the cluster). 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

Nutanix Database Service Design 

## _Table: Nutanix Validated Design Test Cluster Details_ 

|**Item**|**Specification**|
|---|---|
|Manufacturer|Nutanix|
|Model|NX-8170-G9|
|Nodes in the cluster|4|
|AOS version|6.10|
|AHV version|20230302.102001|
|CPU model|Intel Xeon Gold 6442Y @ 2.60 GHz|
|Base frequency|2.60 GHz|
|Maximum turbo frequency|4.00 GHz|
|CPUs per node|2|
|Threads per core|2|
|Cores per socket|24|
|Sockets per node|2|
|NUMA node per CPU|2|
|Installed memory per node|1.48 TiB|
|NIC speed|100 Gbps|
|Storage|SSD-PCIe: 12 disks|
|CVM vCPU|16|
|CVM memory|64 GB|



## **Nutanix Database Service Cluster Resilience** 

Replication factor 2 protects against the loss of a single component in case of failure or maintenance. During a failure or maintenance scenario, Nutanix rebuilds any data that falls out of compliance much faster than traditional RAID data protection methods, and rebuild performance increases linearly as the cluster grows. 

The Nutanix architecture rapidly recovers in the event of failure and has no single points of failure. You can configure the cluster to maintain three copies of data; however, for database server virtualization, Nutanix recommends that you distribute application and 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

Nutanix Database Service Design 

VM components across multiple clusters to provide greater resilience at the application level. 

While Nutanix clusters provide platform-level resilience, Nutanix Database Service (NDB) adds service-level resilience for the control plane. NDB monitors critical services and periodically backs up key datasets to enable recovery to the latest stable state in the event of a failure. For more information on how NDB handles service failures and recovery scenarios, see NDB Service Resiliency in the Nutanix Database Service Administration Guide. 

## **Storage Design for Nutanix Database Service** 

Nutanix uses a distributed, shared-nothing architecture for storage. For more information on Nutanix storage constructs, see Nutanix Hybrid Cloud Compute and Storage in the Nutanix Platform Architecture for Datacenters. 

Creating a cluster automatically creates the following storage containers: 

- NutanixManagementShare: Used for Nutanix features like Files and Objects and other internal storage needs 

This storage container doesn't store workload vDisks. 

- SelfServiceContainer: Used by the Nutanix Cloud Management (NCM) Self-Service Portal and automation services 

- Default-Container-XXXX: Used by VMs to store vDisks for user VMs and applications 

To increase the effective capacity of the cluster, the design enables inline compression according to Nutanix best practices. For the Default-Container-XX, NutanixManagement Share, and SelfService Container, enable compression and disable deduplication and erasure coding across both the primary and replica clusters. 

Nutanix Database Service (NDB) creates striped logical volumes based on the database size specified during provisioning. 

Place database data and logs in a storage container with compression enabled, zero delay, and no deduplication or erasure coding (default). Enabling inline compression saves space and can improve the performance of database workloads running on Nutanix. Don't use deduplication, which adds processing overhead and doesn't reduce space usage for databases. Erasure coding can save storage space, but it consumes 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

Nutanix Database Service Design 

significant CPU time to generate parity and checksums, so only use it for archives or other storage needs that don't require high performance. 

When creating file systems that require high performance, spread each file system over multiple vDisks. NDB automatically implements this best practice. 

For more detailed information, see Data Efficiency. 

## **Network Design for Nutanix Database Service** 

In a cloud architecture, Ethernet handles client communication, transfers within databases, and storage traffic. High-performance networking is key to good database performance on Nutanix. 

Networking best practices: 

- Enable LACP in an active-active configuration. 

- Check network configurations and related recommendations for each database software VM and type of deployment. 

- Create VLANs for use by Nutanix Database Service (NDB). 

For more information on Nutanix networking best practices, see the following documents: 

- Physical Networking Best Practices 

- Nutanix AHV Networking Best Practices 

- Nutanix Database Service Administration Guide 

This Nutanix Validated Design (NVD) configures the cluster network so that each port provides 100 Gbps. Nutanix recommends using redundant 25 Gbps (or faster) connections to ensure adequate bandwidth. 

This NVD uses the standard 1,500-byte MTU, which delivers excellent performance and stability. For more information on networking in AHV, see Nutanix AHV Networking Best Practices. 

We used the Spectrum_NDB_Managed VLAN (VLAN ID: 253) to manage Oracle, SQL Server, and PostgreSQL database VM host IP addresses with an NDB-managed IP address range. 

© 2026 Nutanix, Inc. All rights reserved  | **27** 

Nutanix Database Service Design 

## **Network Microsegmentation with Nutanix Database Service** 

The Nutanix Flow Network Security software solution enables VM- and application-based microsegmentation for traffic visibility and control. This Nutanix Validated Design (NVD) uses Flow Network Security to protect the database environment from network attacks, create strict traffic controls that segment the network, and gain visibility into application network behavior for all AHV hosts managed by a single Prism Central instance. Flow Network Security uses categories created in Prism Central and applies the security policies on the respective components. 

This NVD covers the following use cases: 

- Segment the network and enable application microsegmentation between standalone Nutanix Database Service (NDB) instances and database server VMs. 

- Segment internal networks based on database engine type, application type, or department. 

- Visualize and discover network traffic to NDB instances and traffic from NDB to the database server VMs. 

- Categorize incoming connections to the database server VMs and configure a leastprivilege inbound policy (no traffic allowed). 

- Secure the connectivity ports between the NDB instances and the database server VMs. 

We made the following Flow Network Security design decisions: 

- Limit inbound and outbound security policies based on the requirements. 

- Create unique AppType and DatabaseType categories for each application and database. 

- Create address groups to define the corporate network for easy rule creation. 

- Use port 22 (SSH). 

- Use ports 2379 and 8679 (Patroni and etcd). 

© 2026 Nutanix, Inc. All rights reserved  | **28** 

Nutanix Database Service Design 

## **Database Application Connectivity Requirements** 

Before segmenting the network, define the database AppTypes and AppTiers. For example, set Nutanix Database Service (NDB) as the AppType and define the AppTier as Web or Application when using NDB. When using PostgreSQL, set PostgreSQL as the AppType and define the AppTier as Database. Create the smallest number of application types and application tiers to uniquely identify and group your applications. 

Create a category to organize the required database VMs based on database engine type (such as SQL Server, Oracle, PostgreSQL) and tier (like production, development, and quality assurance). You can create categories at the source and destination entities. 

For each application policy, determine whether the application's required inbound traffic comes from a VM in the Nutanix environment or from an external source. Then you must determine the required traffic between application tiers and decide whether to allow traffic within the same tier. Finally, you must decide whether to allow outbound traffic for the application. 

Define the following database ports as services in Prism Central: 

- The ports allowed between the NDB instance and the source 

- The destination ports between NDB and the database VMs 

For applications that require granular traffic control, create application tiers for different VMs. For example, NDB and Prism Element can be in different application tiers. Configure rules to allow traffic to and from individual application tiers. 

For more information on ports and protocols between NDB and database VMs, see the Software Type: Ports and Protocols page and the NDB Network Requirements section of the Nutanix Database Service Administration Guide. 

© 2026 Nutanix, Inc. All rights reserved  | **29** 

Nutanix Database Service Design 

Figure 5: Securing an Application 

The following tables provide example security policies between NDB and multiple types of database server VMs. Modify the name and specific addresses, categories, and ports based on the databases you protect. 

_Table: NDB and PostgreSQL Security Policy_ 

|**Purpose**|**Source**|**Traffic**|**Destination**|**Port / Protocol**|
|---|---|---|---|---|
|Allow corp clients|AddrCorpClient|Inbound|AppType: NDB;|HTTPS 443|
|to NDB|||AppTier: Web||
|Allow database|AddrDatabasevms|Inbound|AppType: NDB;|HTTPS 443|
|VMs to NDB|||AppTier: Web||
|Server|||||
|Allow NDB to|AppType: NDB;|Outbound|AppType:|SSH 22; Patroni:|
|database VMs|AppTier: Web||PostgreSQL|2379, 8679|
||||AppTier: DB||



_Table: NDB and Oracle Security Policy_ 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

Nutanix Database Service Design 

|**Purpose**|**Source**|**Traffic**|**Destination**|**Port / Protocol**|
|---|---|---|---|---|
|Allow corp clients|AddrCorpClient|Inbound|AppType: NDB;|HTTPS 443|
|to NDB|||AppTier: Web||
|Allow database|AddrDatabasevms|Inbound|AppType: NDB;|HTTPS 443|
|VMs to NDB|||AppTier: Web||
|Server|||||
|Allow NDB to|AppType: NDB;|Outbound|AppType:|SSH 22|
|database VMs|AppTier: Web||OracleAppTier:||
||||DB||



_Table: NDB and SQL Server Security Policy_ 

|**Purpose**|**Source**|**Traffic**|**Destination**|**Port / Protocol**|
|---|---|---|---|---|
|Allow corp clients|AddrCorpClient|Inbound|AppType: NDB;|HTTPS 443|
|to NDB|||AppTier: Web||
|Allow database|AddrDatabasevms|Inbound|AppType: NDB;|HTTPS 443|
|VMs to NDB|||AppTier: Web||
|Allow NDB to|AppType: NDB;|Outbound|AppType: SQL|WINRM 5985|
|database VMs|AppTier: Web||Server AppTier:||
||||DB||



In this Nutanix Validated Design (NVD), the management traffic and the database traffic are on the same VLAN. If your management and database traffic use separate VLANs, create different destination outbound rules. 

This NVD represents address groups with the following addresses of corporate servers and clients to allow differentiated access from database server VMs. Replace these placeholders with addresses specific to your deployment. 

_Table: Address Groups_ 

|**Name**|**Addresses**|**Purpose**|
|---|---|---|
|AddrCorpAll|xx.xx.xx.xx/8|Identify all corporate IP|
|||addresses.|
|AddrCorpClient|xx.xx.xx.xx/16|Identify all IP addresses that|
|||belong to corporate client|
|||devices.|



© 2026 Nutanix, Inc. All rights reserved  | **31** 

Nutanix Database Service Design 

|**Name**|**Addresses**|**Purpose**|
|---|---|---|
|AddrCorpServer|xx.xx.xx.xx/24|Identify all IP addresses that|
|||belong to corporate server|
|||devices.|



The following figure presents a sample security policy configuration in Prism Central. It shows the traffic between the NDB instance and the database server VMs. 

Figure 6: Prism Central Security Policy Example 

## **Management Components for Nutanix Database Service** 

The management cluster is the control hub of the Nutanix environment, hosting foundational services that enable infrastructure orchestration, monitoring, and centralized management across all workload domains. 

Management components such as Prism Central, Active Directory, DNS, and NTP must be highly available. Prism Central is responsible for VM management, replication, 

© 2026 Nutanix, Inc. All rights reserved  | **32** 

Nutanix Database Service Design 

application orchestration, microsegmentation, and other monitoring and analytics functions. You can deploy Prism Central in either a single-VM or scale-out (three-VM) configuration. 

For more information on VM resource assignment when deploying Prism Central, see Nutanix On-Premises Hybrid Cloud with AHV Design. 

For more information on installing Prism Central, see the Prism Central Deployment section of the Prism Central Infrastructure Guide. 

## **Monitoring for Nutanix Database Service** 

Monitoring in this Nutanix Validated Design (NVD) falls into two categories: event monitoring and performance monitoring. Each category addresses different needs and issues. 

In a highly available environment, you must monitor events to maintain higher service levels. When faults occur, the system raises alerts promptly so that administrators can remediate as soon as possible. This NVD configures the Nutanix platform's built-in capability to generate alerts in case of failure. 

In addition to keeping the platform healthy, maintaining a healthy level of resource usage is also essential to the delivery of a high-performing environment. Performance monitoring continuously captures and stores metrics that are essential for troubleshooting application performance. A comprehensive monitoring approach tracks metrics related to the following areas: 

- Applications and databases 

- Operating system 

- Hyperconverged platform 

- Network environment 

- Physical environment 

By tracking metrics in these areas, the Nutanix platform can also provide capacity monitoring across the stack. Most enterprise environments inevitably grow, so you must understand resource utilization and the rate of expansion to anticipate changing capacity demands and avoid any business impact caused by a lack of resources. 

© 2026 Nutanix, Inc. All rights reserved  | **33** 

Nutanix Database Service Design 

In this NVD, Prism Central performs most of the event monitoring. To cover situations where Prism Central might be unavailable, each Nutanix cluster in this NVD sends out notifications using SMTP as well. The individual Nutanix clusters send alerts to a different receiving mailbox that's only monitored when Prism Central isn't available. 

Figure 7: Hybrid Cloud On-Premises Monitoring Conceptual Design 

Prism Central monitors and captures cluster performance in key areas such as CPU, memory, network, and storage utilization by default. When a Prism Central instance manages a cluster, Prism Central transmits all Nutanix Pulse data so it doesn't originate from individual clusters. When you enable Pulse, it detects known issues affecting cluster stability and automatically opens support cases. 

© 2026 Nutanix, Inc. All rights reserved  | **34** 

Nutanix Database Service Design 

Figure 8: Hybrid Cloud Performance Metrics Systems 

The network switches that connect the cluster also play an important role in cluster performance. A separate monitoring tool that's compatible with deployed switches can capture switch performance metrics. For example, an SNMP tool can regularly poll counters from the switches. 

We made the following monitoring design decisions: 

- Prism Central monitors Nutanix platform performance. 

- A separate tool that performs SNMP polling to the switches monitors network switch performance. 

© 2026 Nutanix, Inc. All rights reserved  | **35** 

Nutanix Database Service Design 

- On the database cluster with AOS 6.10.x, leave the Prism Element storage utilization warning threshold at 75 percent (the default value). 

- For the Prism Element health check, leave the host CPU utilization warning threshold at 75 percent (the default value). 

- SMTP alerting: 

   - › Use SMTP alerting; use an enterprise SMTP service as the primary SMTP gateway for Prism Element and Prism Central. 

   - › Configure the source email address to be `clustername@<yourdomain>.com` to uniquely identify the source of emails. For Prism Central, use the Prism Central host name in place of `clustername` . 

   - › Configure the Prism Central recipient email address to be `primaryalerts@<yourdomain>.com` . 

   - › Configure the Prism Element recipient email address to be `secondaryalerts@<yourdomain>.com` . 

- Configure daily Nutanix Cluster Check reports to run at 6:00 AM local time and send them by email to the primary alerting mailbox. 

- Configure Nutanix Pulse to send telemetry data to Nutanix. 

Prism Central provides a notification mechanism and allows you to configure and monitor database infrastructure alerts and events. You can also create custom user-defined alerts and events based on your requirements. 

- Create custom alert policies to monitor database infrastructure entities (such as VMs, hosts, and clusters). 

- Group database VMs into categories to assign custom alert policies. 

- Define a single alert policy for VMs, hosts, or clusters that share common criteria. 

© 2026 Nutanix, Inc. All rights reserved  | **36** 

Nutanix Database Service Design 

- Monitor common metrics, including the following: 

   - › CPU usage 

   - › Memory usage 

   - › Controller average read and write I/O latency 

   - › Controller read and write IOPS 

For a full list of metrics, see the Alert Metrics section of the Prism Central Alerts and Events Reference Guide. 

For more information on Alert Policies, see the Alert Policies (Prism Central) section of the Prism Central Alerts and Events Reference Guide. 

## **Nutanix Database Service Alert Management** 

Nutanix Database Service (NDB) uses an alert management system to notify you if any operation fails. Each NDB operation, such as provisioning a database or creating a clone, is linked to an alert policy. If an operation fails and its associated policy is enabled, NDB generates an alert. 

NDB supports both push and pull mechanisms for delivering alerts. For push notifications, alerts can be sent through SMTP. For pull-based access, you can retrieve alerts using the NDB REST API. NDB maintains its own alert management system and doesn't pass the alerts through Prism Element or Prism Central. 

For more information on sending NDB alerts to an email recipient, see the Alert Notifications section of the NDB Administration Guide. 

A playbook allows you to define a trigger for an action or a series of actions. A trigger can be an event that occurs in the system, such as an alert or a request. The resulting actions can be VM actions, communication actions, alerts, or reports. You can combine playbooks with alerts or events to perform corrective actions for database infrastructure. 

You can create playbooks based on predefined alert triggers, or you can create a manual trigger. For more information, see Playbook Triggers. 

## **Microsoft SQL Server Monitoring with Nutanix Cloud Manager Intelligent Operations** 

The Nutanix Cloud Manager Intelligent Operations functionality provides application monitoring and deep insights into Microsoft SQL Server performance metrics. You can 

© 2026 Nutanix, Inc. All rights reserved  | **37** 

Nutanix Database Service Design 

use it to discover, monitor, and collect SQL Server instance details with Nutanix and third-party data collectors. 

For more information, see the Application Instance Details View (SQL Server) section of the Intelligent Operations Guide. 

## **Database Monitoring Options** 

The Analysis dashboard in Prism Central offers a wide range of monitoring options at many levels, from the entire cluster to individual virtual disks. It shows the historical development of a variety of metrics and can provide a quick overview of cluster performance. 

You can access the Analysis dashboard from the Prism Central main dashboard by entering `Analysis` in the search bar. In older versions of Prism Central, you can access the Analysis dashboard from the Operations section of the main menu. 

The following tools are examples of other common database monitoring options that you can choose from. 

This Nutanix Validated Design (NVD) hasn’t validated these tools. It's your responsibility to ensure compliance with database software licensing and the tools you use. Nutanix can't offer advice on whether a specific license or a specific number of licenses guarantees that you can pass a database engine vendor licensing audit. Consult the database vendor for licensing guidelines. 

## **Oracle Enterprise Manager** 

Oracle Enterprise Manager offers a centralized suite for monitoring, management, and integration tailored for Oracle databases. It provides visibility into database performance, facilitating advanced diagnostics. Oracle Enterprise Manager includes automatic tuning and diagnostic tools. You can extend the platform with various plugins for additional functionalities. 

Choose Oracle Enterprise Manager if your organization predominantly uses Oracle databases and requires comprehensive native support, seamless integration, and a diverse array of database-specific features. 

## **SolarWinds Database Performance Analyzer** 

SolarWinds Database Performance Analyzer (DPA) offers real-time monitoring and diagnostic capabilities customized for Oracle, Postgres, SQL Server, MariaDB, 

© 2026 Nutanix, Inc. All rights reserved  | **38** 

Nutanix Database Service Design 

and MySQL databases. It specializes in SQL query analysis and provides tuning recommendations. DPA's interface offers customizable dashboards. 

Choose DPA if your organization needs a cross-platform monitoring tool that delivers real-time diagnostics and SQL query optimization. 

## **Prometheus** 

Prometheus is an open-source monitoring solution designed for cloud-native environments. It offers extensive customization possibilities through a robust ecosystem of exporters and integrations. Prometheus supports autodiscovery and dynamic service monitoring. 

Choose Prometheus if your organization operates in a cloud-native environment and needs a customized, scalable monitoring solution. 

## **MongoDB Atlas Monitoring** 

MongoDB Atlas Monitoring is a fully managed, cloud-based solution that provides real-time monitoring capabilities for MongoDB clusters. Automated scaling and backup features are available. The tool integrates seamlessly with MongoDB Atlas database hosting. 

Choose MongoDB Atlas Monitoring when using MongoDB Atlas as your database hosting platform. 

## **Ops Manager** 

Ops Manager offers monitoring, backup, and automation features tailored for MongoDB. It provides both on-premises and cloud deployment options. Custom alerting and scaling policies are fully supported. Automation capabilities are available for backup and recovery tasks. 

Choose Ops Manager when you need a monitoring and management tool (whether on-premises or cloud-based) that offers extensive customization options to meet specific requirements. 

## **Percona Monitoring and Management** 

Percona Monitoring and Management (PMM) is an open-source monitoring and management solution. It provides real-time monitoring and in-depth query analytics for various database engines, including MongoDB, PostgreSQL, and MySQL. PMM 

© 2026 Nutanix, Inc. All rights reserved  | **39** 

Nutanix Database Service Design 

supports custom alerting and query performance analysis. It's suitable for hybrid and on-premises deployments. 

Choose PMM if you prefer open-source solutions and require robust monitoring and query analysis for various database systems. 

## **SQL Server Management Studio** 

Microsoft SQL Server Management Studio (SSMS) is a tool for managing and monitoring SQL Server instances. It offers a user interface for real-time monitoring, queries, and performance tuning. SMSS supports custom scripts and extensions for advanced management tasks. 

Choose SSMS to manage and monitor individual SQL Server instances when additional costs are a concern. 

## **SQL Diagnostic Manager for SQL Server** 

SQL Diagnostic Manager offers monitoring and alerting tailored for SQL Server environments. It provides real-time performance analysis and tracks historical data. You can customize alerting and reporting features in SQL Diagnostic Manager. It supports automation for routine maintenance tasks. 

Choose SQL Diagnostic Manager if you need in-depth monitoring, advanced alerting, and automation for SQL Server environments. 

## **pgAdmin** 

pgAdmin is an open-source solution that offers an interface for PostgreSQL database management and monitoring. 

Choose pgAdmin if you need basic PostgreSQL monitoring and management capabilities for a small organization. 

## **EnterpriseDB PostgreSQL Enterprise Manager** 

EnterpriseDB PostgreSQL Enterprise Manager (PEM) offers PostgreSQL monitoring, alerting, and performance tuning. It supports both open-source PostgreSQL and EnterpriseDB Postgres Advanced Server. You can use it for advanced query performance analysis and historical data tracking. 

Choose EnterpriseDB PEM if you need advanced monitoring and performance tuning for both open-source and commercial PostgreSQL deployments. 

© 2026 Nutanix, Inc. All rights reserved  | **40** 

Nutanix Database Service Design 

## **Monitoring Cluster Storage IOPS** 

To create a chart that tracks storage IOPS for one cluster, follow these steps: 

**1.** From the Analysis dashboard in Prism Central, click **Add Chart** . 

**2.** Select **Cluster** from the **Entity** dropdown menu. 

**3.** Select a cluster from the cluster dropdown menu. 

**4.** Enter **IOPS** in the **Metric** field. 

Entering a query in the **Metric** field filters the dropdown menu. 

**5.** Select the following metrics for the chart: 

   - Controller IOPS 

   - Controller Read IOPS 

   - Controller Write IOPS 

**6.** Enter a name for the chart in the **Chart Name** field. 

**7.** Click **Add** . 

## **Monitoring Cluster CVM CPU Usage** 

To create a chart that tracks the Controller VM (CVM) CPU usage for a cluster, follow these steps: 

**1.** From the Analysis dashboard in Prism Central, click **Add Chart** . 

**2.** Select **VM** from the **Entity** dropdown menu. 

**3.** Choose the cluster CVMs to be monitored. 

**4.** Select **CPU Usage (%)** from the **Metric** dropdown menu. 

**5.** Enter a name for the chart in the **Chart Name** field. 

**6.** Click **Add** . 

To monitor database VM CPU usage, follow the same steps but select database VMs instead of CVMs in the entity section. 

## **Monitoring VM Storage Latency** 

To create a chart that tracks the storage latency for a database VM, follow these steps: 

**1.** From the Analysis dashboard in Prism Central, click **Add Chart** . 

**2.** Select **VM** from the **Entity** dropdown menu. 

**3.** Select the database VM to be monitored. 

© 2026 Nutanix, Inc. All rights reserved  | **41** 

Nutanix Database Service Design 

**4.** Enter **Latency** in the **Metric** dropdown menu. 

**5.** Select the following metrics: 

   - Storage Controller Read Latency 

   - Storage Controller Write Latency 

**6.** Enter a name for the chart in the **Chart Name** field. 

**7.** Click **Add** . 

## **Security and Compliance for Nutanix Database Service** 

Nutanix recommends a defense-in-depth strategy for layering security throughout any enterprise database solution. This design section focuses on validating the layers that Nutanix can directly oversee at the control and data plane levels. For more information on network-based security, see the Network Design for Nutanix Database Service at Scale section, and for additional details, see Nutanix Hybrid Cloud Security Layer. 

Nutanix recommends isolating the management and backup clusters from the rest of the network with firewalls. Backup clusters are often prime targets for compromise. When designing your network security architecture, remember that the backup and workload clusters have significant traffic between them. In addition, Nutanix management IPMI interfaces must only be directly accessible from the management domain. 

All Nutanix control plane endpoints use Active Directory–hosted LDAPS. Active Directory itself is redundant across the management clusters in both availability zones (AZs). Only administrative accounts map to admin roles, which are controlled through a named Active Directory group. 

© 2026 Nutanix, Inc. All rights reserved  | **42** 

Nutanix Database Service Design 

## 8. Databases on Nutanix 

To deploy databases on Nutanix, fulfill the infrastructure requirements and follow the best practices for optimal performance. Although this Nutanix Validated Design (NVD) covers database management using Nutanix Database Service (NDB), you can deploy databases manually without NDB and still achieve the required goals. 

## **NDB-Managed Networks in NDB** 

This NVD configures an NDB-managed IP address pool on each of the testing VLANs. Each network (VLAN) can have static IP address management provided by NDB or DHCP provided by an external server. 

NDB can use VLANs configured in AHV to provision database server VMs. NDB can manage a pool of IP addresses that are statically assigned to database VMs as they're provisioned. For single-node database server VMs, you can use an NDB-managed IP address range or an external DHCP server. For clustered database provisioning, use NDB for IP address management. 

## **NDB Networks** 

NDB uses network profiles to associate VLANs with database servers. This NVD validates the VLANs on the Nutanix cluster, adds those VLANs to NDB, and enables NDB IP address management. Use an NDB-managed network with an assigned static IP address range to provision SQL Server clustered databases and Oracle RAC databases. For more information, see NDB Network Management. 

## **NDB Compute** 

This NVD broadly defines three VM sizes: small, medium, and large. These VM sizes are examples based on data from Nutanix Sizer, which uses VM requirements as input to generate an appropriate hardware configuration. 

_Table: Example VM Configurations for Database Validation_ 

|**VM Size**|**vCPUs**|**Memory**|
|---|---|---|
|Small|2|32|
|Medium|4|64|



© 2026 Nutanix, Inc. All rights reserved  | **43** 

Nutanix Database Service Design 

|**VM Size**|**vCPUs**<br>**Memory**|
|---|---|
|Large|8<br>128|



These example VM sizes can meet some database requirements. For your environment, ensure that your VM size matches your application requirements before implementation. 

## **Configuring Nutanix Database Service** 

To configure and use Nutanix Database Service (NDB) to manage databases, follow these steps: 

**1.** Download NDB from the Nutanix Support portal. 

**Note:** This NVD uses NDB version 2.8. For more information on downloading NDB, see the NDB Downloads page (Nutanix credentials required). 

**2.** Upload the image to the cluster using Prism Element. 

**3.** Create an NDB VM. 

For more information, see Installing NDB on AHV. 

## **4.** Perform the initial NDB configuration. 

For more information, see NDB Initial Configuration. 

**5.** Finish setting up NDB using the **Welcome to NDB** wizard. 

For more information, see Get Started with NDB. 

## **SQL Server Implementation Best Practices** 

Set the SQL Server maximum memory setting to a value that leaves memory free for other processes running on the system. For examples and calculations for setting the SQL Server maximum memory parameter, see Microsoft SQL Server Design. 

Keeping the SQL Server memory requirement within the vNUMA boundary can provide optimal performance. For large databases, you might need to make the SQL Server memory larger than the vNUMA boundary. For more information on vNUMA for SQL Server, see Microsoft SQL Server on Nutanix Best Practices. 

© 2026 Nutanix, Inc. All rights reserved  | **44** 

Nutanix Database Service Design 

This Nutanix Validated Design (NVD) configures the SQL Server maximum memory settings using NDB SQL Server database instance profiles. 

_Table: Example SQL Server Database Instance Profiles_ 

|**Size**|**Memory**|**Test Database Size**|**SQL Server Max**|
|---|---|---|---|
||||**Memory Setting**|
|Small|32 GB|100 GB|27,148 MB|
|Medium|64 GB|300 GB|59,416 MB|
|Large|128 GB|500 GB|123,952 MB|



## **SQL Server Storage Configuration** 

For databases smaller than 2 TB, Nutanix Database Service (NDB) configures four data disks for each database and calculates the number of data files for each SQL Server database based on the number of vCPUs configured for the VM. NDB configures log files to be 20 percent of the database size and the initial tempdb size to be 10 percent of the database size by default. 

Figure 9: SQL Server Database Example Disk File System 

NDB creates the database disk drives and attaches them to the database server VM as mount points under the `C:\NTNX\ERA_DATABASES\` directory. 

For SQL Server Always On cluster database servers, all the nodes contain a similar disk layout. 

Nutanix best practices are a starting point for any SQL Server deployment. Analyze and calculate your storage requirements and adjust the storage environment, disk layout, number of data files, temp files, and log files accordingly. For more information on the 

© 2026 Nutanix, Inc. All rights reserved  | **45** 

Nutanix Database Service Design 

logical and physical disk designs for typical SQL Server environments, see Microsoft SQL Server on Nutanix Best Practices. 

## **Oracle Implementation Best Practices** 

This Nutanix Validated Design (NVD) assigns 10 percent of system memory for the operating system and file cache. Of the remaining memory, 80 percent is assigned to the System Global Area (SGA) and 20 percent to the Program Global Area (PGA). This NVD configures temp tablespace, undo tablespace, and the processes parameter to accommodate these settings. Adapt these settings to fit your application requirements. 

Nutanix Database Service (NDB) spreads the DATA and RECO (redo and archive files) disk groups across multiple vDisks for optimal performance. NDB supports both Oracle Automatic Storage Management (ASM) and local file systems. It also supports ASMFD (if the operating system supports ASMFD), ASMLIB, and udev rules for ASM disk mappings. 

When provisioning a database, NDB calculates the required number of vDisks for DATADG and RECODG disk groups based on the database size. This NVD configures all Oracle databases used for this validation to accommodate 2 TiB of data, which is considered a large database. The number of vDisks per disk group depends on the database size requested. For a 2 TiB database, NDB creates eight vDisks for DATADG (2,048 GB ÷ 8 = 256 GB per disk) and four vDisks for RECODG. 

The storage configuration is independent of the vCPU and memory of the database VMs. Small, medium, or large database VMs in this validated design refer to the amount of vCPU and memory. 

_Table: Number of Disks Created by NDB Based on Database Size_ 

|**Disk Groups**|**Small or Medium (≤ 500 GB)**|**Large (≥ 501 GB)**|
|---|---|---|
|CRSDG|3 vDisks|3 vDisks|
|DATADG|4 vDisks|8 vDisks|
|RECODG|2 vDisks|4 vDisks|



**Note:** The CRSDG disk group only exists for Oracle RAC databases. 

© 2026 Nutanix, Inc. All rights reserved  | **46** 

Nutanix Database Service Design 

Figure 10: Logical and Physical Design for Oracle Database 

This NVD configures separate VLANs for the Oracle public network and Oracle private interconnect for cache fusion. After you configure the networks in NDB, NDB-managed networks assign the IP addresses for public, private, and virtual networks. 

For more information, see AHV Networking Best Practices. 

## **PostgreSQL Implementation Best Practices** 

We assigned 10 percent of system memory for the operating system and file cache, 25 percent for the `shared_buffers` , and 75 percent for the `effective_cache_size` . Adapt these settings to fit your application requirements. 

We used the following software versions in this Nutanix Validated Design (NVD): 

© 2026 Nutanix, Inc. All rights reserved  | **47** 

Nutanix Database Service Design 

- PostgreSQL 16.6 

- Nutanix Database Service 2.8 

## **Preparing PostgreSQL Software Profile Version** 

You can create a software profile from a previously registered Postgres database server VM, or you can create a new database software VM that you register as a new software profile. 

To create a new database server VM with Nutanix Database Service (NDB), follow these steps: 

**1.** Use Prism to create a VM with a supported operating system. 

For more information on supported OS options, see the NDB release notes (Nutanix credentials required). 

**2.** Install a supported version of PostgreSQL. 

You can install PostgreSQL binaries in any directory; specify the path to the binaries when you create the software profile. For more information on supported versions, see the latest NDB release notes. 

**3.** Customize the VM as required. 

For more information, see PostgreSQL on Nutanix Best Practices. 

**4.** Install any nondefault packages your environment uses. 

**5.** Ensure that your VM meets NDB configuration requirements: 

   - **a.** Enable the `postgresql` user for `NOPASSWD sudo` . 

   - **b.** Configure software repositories for NDB OS updates. 

   - **c.** Add any scripts that must run before or after deployment for NDB configuration. 

## **PostgreSQL Configuration** 

The following table captures the PostgreSQL configuration used in this Nutanix Validated Design (NVD). 

_Table: Example PostgreSQL Database Instance Profiles_ 

|**Size**|**Memory**|**Test**|**Shared Buffers**|**Effective**|
|---|---|---|---|---|
|||**Database Size**||**Cache Size**|
|Small|64 GB|100 GB|16 GB|48 GB|



© 2026 Nutanix, Inc. All rights reserved  | **48** 

Nutanix Database Service Design 

|**Size**|**Memory**|**Test**|**Shared Buffers**|**Effective**|
|---|---|---|---|---|
|||**Database Size**||**Cache Size**|
|Medium|128 GB|300 GB|32 GB|96 GB|
|Large|256 GB|500 GB|64 GB|192 GB|



These example VM sizes can meet some database requirements. For your environment, ensure that your VM size matches your application requirements before implementation. 

## **PostgreSQL Storage Configuration** 

This Nutanix Validated Design (NVD) uses the optimized storage configuration provided by Nutanix Database Service (NDB). NDB creates striped Linux logical volumes based on the database size specified during provisioning. The database software and logs are placed in the `data` directory, and the database tablespaces are placed in the `tsdata` directory. 

© 2026 Nutanix, Inc. All rights reserved  | **49** 

Nutanix Database Service Design 

Figure 11: Storage Design for a Medium PostgreSQL Database 

Example `data` and `tsdata` directories on an NDB PostgreSQL deployment include the following: 

- `/pgsql/<Database Name>/data` : the PGDATA location 

© 2026 Nutanix, Inc. All rights reserved  | **50** 

Nutanix Database Service Design 

- `/pgsql/<Database Name>/tsdata` : the data tablespace location 

## _Table: Number of Disks Created by NDB Based on Database Size_ 

|**Disk Group**|**Small (<1 TB)**|**Medium (>1 TB)**|**Large (>2 TB)**|
|---|---|---|---|
|data|2 vDisks|3 vDisks|3 vDisks|
|tsdata|3 vDisks|3 vDisks|5 vDisks|



## **PostgreSQL Image Customization During Provisioning** 

You can use Nutanix Database Service (NDB) pre and post commands to customize your database deployment. Pre and post commands run as the NDB drive owner, defined when you provision the database VM. This NVD has an NDB drive owner with the username **postgres** and uses a post command script to modify `pg_hba.conf` to configure the IP addresses that could access this PostgreSQL instance. The post command script `postgresql_post.bash` was included in the VM image used to create the software profile to ensure the script was available during deployment. 

© 2026 Nutanix, Inc. All rights reserved  | **51** 

Nutanix Database Service Design 

## 9. Software Patching with Nutanix Database Service 

Nutanix Database Service (NDB) automates the patching process for the OS and database software. 

To patch the OS on Linux, NDB issues the update command for the package manager used. For example, if you use RHEL 7.x or higher, NDB runs `yum` . If you use Centos 8.x or higher, NDB runs `dnf` . For OS patching, the software profile must include the proper software repository configuration such that a `yum` update (in the case of Centos 7) performs the required actions. 

## **Microsoft SQL Server Database Software Patching** 

Nutanix Database Service (NDB) supports patching for Microsoft SQL Server database instances. Patching for SQL Server is validated on VMs provisioned by NDB (greenfield deployments). 

To patch SQL Server database server VMs, create a software profile version by uploading a SQL Server update executable in NDB. You can then use the SQL Server update to patch other database server VMs or provision a new database server VM with the updated software profile. 

Patches are applied in a rolling upgrade. Rolling upgrade patching updates the secondary database replica first. After it patches the secondary replica, the rolling upgrade fails the availability group over to the (earlier) secondary replica. After it also patches the last (initial primary) node, the rolling upgrade fails the availability group over again to restore the availability group roles and configuration to its state before the update. This behavior is the default for database server cluster patching in NDB. 

## **Preparing a SQL Server Software Profile Version** 

To create a software profile version, follow these steps: 

**1.** In the dropdown list of the main menu, select **Profiles** . 

© 2026 Nutanix, Inc. All rights reserved  | **52** 

Nutanix Database Service Design 

**2.** Go to Software and open the software profile that you already created. 

For example, open the MSSQL_GoldVM software profile. 

## **3.** Click **Create** . 

**4.** In the Create Software Profile Version dialog, configure the following items: 

   - **Name** : Enter a name for the software profile version. 

   - **Description** : Enter a description for the software profile version. 

   - **Patch File Location** : 

      - › If your patch file is stored on the local computer, upload the patch file (.exe) from the local computer. 

      - › If your patch file is stored in a file share, type the location of the file share that contains the patch file and provide the file share credentials in the **User Name** and **Password** fields. 

   - **Patch Notes** : Enter a note to provide additional information about the patch file. 

**5.** Click **Create** . 

Patch files are available for download from the Microsoft Download Center and the Microsoft Update Catalog site. You can provide any SQL Server service pack or cumulative update. 

**6.** Monitor the progress of the operation. 

NDB displays the software profile version that you added to the software profile in the list. 

## **Patching a SQL Server Single-Instance Database** 

To apply updates from the available software profile versions to a provisioned, registered database server VM, follow these steps: 

**1.** In the dropdown list of the main menu, select **Database Server VMs** . 

**2.** Go to **List** . 

**3.** Select the database server VM for the software profile version update. 

**4.** In the Database Server VM Summary dialog, check the Software Profile Version widget. 

The Software Profile Version widget displays the current version, recommended version, and status of the software profile version. 

© 2026 Nutanix, Inc. All rights reserved  | **53** 

Nutanix Database Service Design 

## **5.** Click **Update** . 

**Note:** The Update option only appears when a new software profile version is available. 

**6.** In the Update Database Server VM dialog, configure the following items: 

   - Update to Software Profile Version: Select the software profile version (for example, CU15) from the dropdown menu. 

   - Start Update: Select **Now** . 

   - Pre-Post Commands: Leave this field blank. 

   - Provide the database server VM name. 

**7.** Click **Update** . 

A dialog box indicates that the operation to update a database has started. 

**8.** In the NDB Operations page, monitor the update progress. 

**9.** Verify the upgrade by running the `select @@version` command again on the database server VM. 

**10.** Ensure that the database hosted on the VM is healthy and active after the upgrade. 

**Note:** The patching process for Microsoft SQL Server database server clusters using NDB is the same. 

## **Patch Oracle Database Software** 

For Oracle database updates, Nutanix Database Service (NDB) uses out-of-place patching. This method allows you to install updates on your template image or one database host, copy Oracle or GRID home, install the updates on one or more target servers, and start the services. Patching for Oracle databases is validated on VMs provisioned by NDB (greenfield deployments). 

## **Preparing an Oracle Software Profile Version** 

To patch an existing database with Nutanix Database Service (NDB), generate a new version of the software profile that you used to create the database: 

© 2026 Nutanix, Inc. All rights reserved  | **54** 

Nutanix Database Service Design 

**1.** Provision a database VM using an existing software profile version. 

We recommend this method over patching an existing source database or clone database because it minimizes the impact on these databases. 

Provision the database from an Oracle 19.23 RU software profile. 

**2.** Manually patch the grid infrastructure and database software as described in the Oracle documentation accompanying the patch set. 

**3.** In the software profile section of NDB, select the name of the software profile that you must create a new version for. 

**4.** Click **Create** . 

**5.** Enter a name for the new software profile version. 

**6.** Select the VM that you just patched to 19.24. 

**7.** (Optional) In the **Notes** section, enter additional information about the new software profile version. 

**8.** Select the new version in the software profile's overview screen. 

**9.** Click **Update** . 

**10.** Set the status to **Published** . 

**11.** Select the checkbox for the acknowledgment. 

**12.** (Optional) Update the **Notes** section. 

## **Patching an Oracle Single-Instance Database** 

After you publish the software profile version, the database VMs that can be patched with the new software profile version show an **Update Available** notification in the **Software Profile Version** widget. 

To update an Oracle single-instance database VM, follow these steps: 

**1.** In the **Software Profile Version** widget, click **Update** . 

**2.** Choose the new software profile version from the dropdown list. 

**3.** Select whether to patch the database VM now or later. 

NDB patches the Oracle software on the chosen database VM. 

You can confirm that the patch was successful using NDB, opatch lsinventory, or the database dictionary. 

The following output shows the single-instance database software version: 

```
Before:
```

© 2026 Nutanix, Inc. All rights reserved  | **55** 

Nutanix Database Service Design 

```
Patch description: "Database Release Update : 19.23.0.0.240416 (36233263)"
After:
Patch description: "Database Release Update : 19.24.0.0.240716 (36582781)"
```

```
You can get the oracle patch information from DBA_REGISTRY_SQLPATCH:
```

```
  PATCH_ID PATCH_TYPE ACTION STATUS  DESCRIPTION
           SOURCE_VERSION TARGET_VERSION
---------- ---------- ------ -------
 ----------------------------------------------------- --------------
 --------------
  36233263 RU      APPLY  SUCCESS Database Release Update : 19.23.0.0.240416
 (36233263) 19.1.0.0.0  19.23.0.0.0
  36582781 RU      APPLY  SUCCESS Database Release Update : 19.24.0.0.240716
 (36582781) 19.23.0.0.0 19.24.0.0.0
```

The patching process for Oracle Database server clusters using NDB is the same. You can perform the process in either a rolling or nonrolling fashion. 

## **Patch PostgreSQL Database Software** 

Nutanix Database Service (NDB) 2.8 supports automated minor version upgrades for PostgreSQL. This Nutanix Validated Design (NVD) tested the upgrade from PostgreSQL 16.6 to 16.8. 

Nutanix recommends using the source code or unzip method to install PostgreSQL, rather than using Linux package managers such as YUM or DNF. If you use a Linux software package manager to install the database engine, patching through NDB isn’t supported, including both minor upgrades using a new software profile and patching from outside NDB. 

## **Preparing a PostgreSQL Software Profile Version** 

To prepare a software profile to patch PostgreSQL, follow these steps: 

**1.** Register a RHEL 8.10 VM with PostgreSQL version 16.6 installed: 

```
$ cat /etc/redhat-release
Red Hat Enterprise Linux release 8.10 (Ootpa)
```

**2.** Set the VM name to **PostgreSQL16.6** . 

**3.** Create a software profile using this VM. 

**4.** Set the software profile name to **PostgreSQL16.6-SI** . 

**5.** Sign in to the PostgreSQL16.6 VM. 

**6.** Use the source code method to upgrade to PostgreSQL 16.8. 

© 2026 Nutanix, Inc. All rights reserved  | **56** 

Nutanix Database Service Design 

**7.** Create a software profile update version for the PostgreSQL16.8-SI profile using the PostgreSQL16.6 VM with the upgraded database software. 

## **Patching a PostgreSQL Single-Instance Database** 

To update a PostgreSQL single-instance database, follow these steps: 

**1.** Click the database server VM, select the instance, and under Software Profile Version, click **Update** . 

**2.** Sign in to the VM. 

**3.** Verify that PostgreSQL 16.8 is running. 

The patching process for PostgreSQL database server clusters using Nutanix Database Service (NDB) is the same. 

© 2026 Nutanix, Inc. All rights reserved  | **57** 

Nutanix Database Service Design 

## 10. Nutanix AHV High Availability 

Nutanix performed two high availability validations to measure the effect on a running database workload: 

- Unplanned outage: a hardware or software failure that results in the loss of a cluster node. 

- Planned outage: the workload management process for a planned upgrade that requires you to restart a cluster node. 

For more information on high-availability scenarios and database recoverability options, see the Availability and Resilience section in the documentation for your database: 

- SQL Server 

- Oracle 

- PostgreSQL 

You can configure the Nutanix Database Service (NDB) management plane for high availability. For more information, see NDB High Availability. 

Nutanix simulated SQL Server database migration on a test cluster to validate AHV high availability. You can apply this process to any other database VMs. 

## **Unplanned SQL Server VM Migration** 

When unplanned outages occur, AHV automatically restarts VMs affected by a failed node on a surviving node in the cluster. This test timed how long it took for a SQL Server database server VM with six vCPUs and 32 GB of memory to automatically resume on another host by pinging from a different VM and running a benchmark task from a client VM that we monitored live. We enabled high availability for the cluster in Prism Element and simulated an outage to one of the nodes by putting it in maintenance mode. 

The Prism Tasks page indicated that the migration finished in less than 16 seconds. 

© 2026 Nutanix, Inc. All rights reserved  | **58** 

Nutanix Database Service Design 

Figure 12: Unplanned Migration Task in Prism 

The database server VM started accepting connections, and we reached the VM after 16 seconds (it only missed one beat during the ping test). 

During the migration process, the workload maintained full session continuity with no transaction failures observed. The workload with continuous insertion fluctuated during the migration but stabilized within a minute of the live migration finishing. Nutanix advises using high-speed networking infrastructure to ensure optimal performance—even for applications operating in a single VM. 

© 2026 Nutanix, Inc. All rights reserved  | **59** 

Nutanix Database Service Design 

Figure 13: Monitor the Unplanned Migration Process 

For more information, see KB-7949. 

## **Planned SQL Server VM Migration** 

To maintain application availability during cluster maintenance, Nutanix performs rolling upgrades. Nutanix AHV supports hypervisor upgrades by live-migrating running VMs to another node in the cluster before upgrading a node. This Nutanix Validated Design (NVD) measured the effect of a live migration, simulating a rolling upgrade on a SQL Server database VM with six vCPUs and 32 GB of memory. We started with a database server VM running a workload that continuously runs a benchmark and live-migrated the database server to another node. 

The Prism Tasks page indicated that the live migration finished in less than 19 seconds. 

The database server VM started accepting connections, and we reached the VM after 19 seconds (it only missed one beat during the ping test). 

During the migration process, the workload maintained full session continuity with no transaction failures observed. Although the transactions exhibited a small dip, performance stabilized a few seconds after the migration. Nutanix recommends using fast networks to ensure optimal performance, even when an application operates in a single VM. 

© 2026 Nutanix, Inc. All rights reserved  | **60** 

Nutanix Database Service Design 

Figure 14: Monitor the Planned Migration Process 

For more information, see KB-7949. 

© 2026 Nutanix, Inc. All rights reserved  | **61** 

Nutanix Database Service Design 

## 11. Nutanix Database Service Time Machine Backup and Recovery 

Nutanix Database Service (NDB) uses a time machine to create backups in the form of application-consistent snapshots of a database and copies of database transaction logs. Service-level agreements (SLAs) define backup frequency and how long data is kept. For example, if you set Continuous Duration to 10 days, transaction logs are retained for 10 days from when the first daily snapshot is taken. 

Snapshots are available locally in the cluster, so it takes only a few seconds to restore to the last snapshot. Based on the recovery point chosen, NDB restores the vDisks using the appropriate snapshot and applies the transaction logs to bring the database to a consistent state. 

NDB time machine backups protect your data from application- and user-related errors, but they don't protect against disasters like a site failure. A comprehensive disaster recovery strategy includes a copy of the data stored in a separate location. 

This validation includes database backup and recovery performed using the NDB time machine. 

## **Validating Time Machine Data** 

To validate using time machine data for backup and recovery, follow these steps: 

**1.** Create a database. 

**2.** Populate the database. 

**3.** Perform a time machine snapshot. 

**4.** Delete a table. 

**5.** Perform an in-place restore of the database using snapshots and logs. 

© 2026 Nutanix, Inc. All rights reserved  | **62** 

Nutanix Database Service Design 

## **Verifying Microsoft SQL Server Backup and Restore** 

To validate the backup and recovery of a Microsoft SQL Server database using SQL Server Management Studio (SSMS), follow these steps: 

**1.** In SSMS, create two tables. 

**2.** Insert data into the two tables. 

The following output shows the timestamps for creating the tables and adding the data. 

`11:00 - Created two tables 11:01 - insert primary data - 12000 rows have been inserted into the tblauthors table successfully 11:02 - Inserted secondary data set - verified 20000 rows have been inserted to tblbooks` 

**3.** Verify that the time machine schedule creates a snapshot. 

Figure 15: Snapshot Creation with Time Machine 

**4.** Modify the database by inserting 2,000 additional rows into the `tblbooks` table: 

`11:17 - insert 2000 records into the tblbooks table. The tblbooks table now contains a total of 22000 rows.` 

**5.** Restore the database to its previous state: 

   - **a.** Navigate to the time machine. 

   - **b.** Select the time machine you want to use for recovery. 

This Nutanix Validated Design (NVD) example uses the time machine NVD_backuptest2_TM. 

**6.** Click **Actions** . 

**7.** 

   - Select **Restore Source Database** . 

**8.** In the Restore Source Database dialog, select **Point in Time** and select a time that is shortly before you modified the database. 

© 2026 Nutanix, Inc. All rights reserved  | **63** 

Nutanix Database Service Design 

**9.** Under **Restore Location** , select **Original Location** . 

**10.** Click **Restore** . 

Nutanix Database Service uses the earlier snapshot and applies all the logs until right before the change to recover the database. 

**11.** Connect to the VM. 

**12.** Verify that the table has 20,000 rows. 

## **Oracle Database Restore** 

This validation provisions a new database using NDB a few days before attempting a restore. It applies a custom service-level agreement (SLA), NVD_ORACLE_PROD, to back up the database and database transaction log files. The NVD_ORACLE_PROD SLA uses the following retention lengths: 

- Continuous log retention: 30 days 

- Daily snapshot retention: 7 days 

- Weekly snapshot retention: 2 weeks 

- Monthly snapshot retention: 2 months 

- Quarterly snapshot retention: 1 quarter 

For more information on configuring a time machine, see NDB Time Machine Management. 

The test scenario used an Oracle single-instance database with a user schema that has a table with data to test the restore functionality by deleting the entire schema. 

## **Deleting the Oracle Schema** 

To delete an Oracle schema, follow these steps: 

**1.** Use a SQL select statement to query the DBA_SEGMENTS and DBA_TABLES metadata table to identify the size of the table and number of rows in the table. 

In this example, we have a table with 617 records and 65,536 bytes: 

```
SQL> select dt.OWNER, ds.SEGMENT_NAME, ds.TABLESPACE_NAME, ds.BYTES,
 dt.NUM_ROWS
from dba_tables dt, dba_segments ds
where dt.owner = ds.owner
```

© 2026 Nutanix, Inc. All rights reserved  | **64** 

Nutanix Database Service Design 

```
and dt.table_name = ds.segment_name
and ds.segment_name = 'TIMER';
OWNER          SEGMENT_NAME   TABLESPACE_NAME           BYTES   NUM_ROWS
-------------------- -------------------- ------------------------------
 ---------- ----------
CASETESTER      TIMER       USERS                65536    617
```

**2.** Drop the user and all its objects: 

```
SQL> drop user CASETESTER cascade;
```

**3.** Confirm that the schema is dropped. 

## **Restoring the Oracle Schema** 

To test how quickly Nutanix Database Service (NDB) can restore the table, follow these steps: 

**1.** Sign in to NDB. 

**2.** From the main menu, click **Databases** . 

**3.** Click **Sources** . 

**4.** Select the source database. 

**5.** From the Databases homepage, click **Restore** . 

**6.** In the Restore Source Database dialog, select the **Point in Time** option and select the desired restore point. 

This test case uses a time before the CASETESTER schema was dropped from the database. 

**Note:** You can only run a point-in-time restore (PITR) if a snapshot and database transaction logs exist. 

**7.** For Restore Location of Data Files, enter the original location of the data files. 

**Note:** NDB must have a backup of the log you select to facilitate a point-in-time restore operation. If the log is highlighted in yellow, the logs are available but not backed up. 

To manually trigger a log backup, select the **Archive/Online Redo Logs** checkbox and click **Run** . 

**8.** Enter the name of the database. 

**9.** Click **Restore** . 

The restore process begins and the source database becomes unavailable. **10.** Monitor the status from the **Operations** tab. 

© 2026 Nutanix, Inc. All rights reserved  | **65** 

Nutanix Database Service Design 

## **11.** Confirm that you restored the schema from DBA_SEGMENTS and DBA_TABLES: 

```
select dt.OWNER, ds.SEGMENT_NAME, ds.TABLESPACE_NAME, ds.BYTES,
 dt.NUM_ROWS
from dba_tables dt, dba_segments ds
where dt.owner = ds.owner
and dt.table_name = ds.segment_name
and ds.segment_name = 'TIMER';
OWNER          SEGMENT_NAME   TABLESPACE_NAME           BYTES   NUM_ROWS
-------------------- -------------------- ------------------------------
 ---------- ----------
CASETESTER      TIMER       USERS                65536    617
```

## **Validating PostgreSQL Backup and Restore** 

To validate the PostgreSQL backup and restore process, follow these steps: 

**1.** Validate the database size: 

```
SELECT pg_size_pretty(pg_database_size('tpcc'));
pg_size_pretty
----------------
292 GB
```

**2.** After the Nutanix Database Service (NDB) time machine automatically creates the restore point based on the schedule that you defined when you provisioned the VM, check the last recoverable time and the last snapshot. 

**3.** Drop the database. 

**4.** Restore the database to the restore time available in the time machine. 

**5.** Verify that you restored the database: 

```
SELECT pg_size_pretty(pg_database_size('tpcc'));
pg_size_pretty
---------------
292 GB
```

© 2026 Nutanix, Inc. All rights reserved  | **66** 

Nutanix Database Service Design 

## 12. Ordering Nutanix Database Service Deployments 

This sample bill of materials reflects the validated and tested hardware, software, and services that Nutanix recommends to achieve the outcomes described in this document. Consider the following points when you build your orders: 

- All software is based on core licensing whenever possible. 

- Nutanix Professional Services or an affiliated partner selected by Nutanix provides all services. 

- Nutanix based the functional testing described in this document on NX series models with similar configurations to validate the interoperability of software and services. 

**Note:** Some components aren't available in all regions; your actual quote depends on availability. 

If you need to make substitutions in the specific list of validated and tested hardware, software, or services, note the following guidance: 

- Nutanix recommends that you purchase the exact hardware configuration reflected in the bill of materials whenever possible. If a specific hardware configuration is unavailable, choose a similar option that meets or exceeds the recommended specification. 

- You can make hardware substitutions to suit your preferences; however, such changes might result in a solution that doesn't follow the recommended Nutanix configuration. 

- Avoid software product code substitutions except in the following scenarios: 

   - › You need different quantities to maintain software licensing compliance. 

   - › You prefer a higher license tier or support level for the same software product code. 

- Adding any software or workloads that aren't specified in this design to the environment (including additional Nutanix products) might affect the validated density 

© 2026 Nutanix, Inc. All rights reserved  | **67** 

Nutanix Database Service Design 

calculations and result in a solution that doesn't follow the recommended Nutanix configuration. 

- Professional Services substitutions to accommodate customer preferences aren't possible. 

## **Sample Bill of Materials** 

This validation used the following software and hardware components: 

- Nutanix Database Service (NDB) Platform Software License & Mission Critical Software Support Service for 1 CPU Core 

- NX-8170-G9 

- Nutanix software: 

   - › Foundation (hypervisor-agnostic installer) 

   - › Controller VM 

   - › Prism Management 

   - › Starter License Entitlement 

- Intel Xeon Gold 6442Y @ 2.60 GHz 

- 64 GB of memory, 4,800 MT/s DDR5 Synchronous Registered (Buffered) 

- SSD-PCIe 3.36 TiB Samsung MZQL23T8HCLS-00A07 

- Mellanox 100, 40, 25 GbE, 2-port NIC (Mellanox CX6) 

- 24/7 Mission-Critical Level Hardware Support for Nutanix HCI appliance 

- C13/C14, 15A, 4 ft power cord 

## **Nutanix Professional Services** 

With the following Nutanix Professional Services, Nutanix can implement this Nutanix Validated Design (NVD) as designed, built, and tested: 

- NDB Deployment and Database Migration - Starter Edition 

© 2026 Nutanix, Inc. All rights reserved  | **68** 

Nutanix Database Service Design 

- NDB-Enabled Database Automation Pro Service 

**Note:** Small, medium, and large configurations can use this service. 

- Database Modernization, NDB Deployment 

To ensure a seamless experience across design, migration planning, implementation, and optimization of the Nutanix platform for both virtualized and containerized applications, we strongly recommend engaging Nutanix Professional Services. 

Reach out to your Nutanix Account Team to discuss the recommended services tailored to your environment. Using these expert services helps reduce complexity and risk at every stage of your journey. 

© 2026 Nutanix, Inc. All rights reserved  | **69** 

Nutanix Database Service Design 

## 13. Test Plans for Nutanix Database Service Design 

The test plans for the Nutanix Database Service Design Nutanix Validated Design (NVD) validate a successful implementation (spreadsheet automatically downloads when you click the link). Compare the result for each test plan item with the Expected Result column. 

© 2026 Nutanix, Inc. All rights reserved  | **70** 

Nutanix Database Service Design 

## 14. References and Resources for Nutanix Database Service Design 

For more information on the components that make up the Nutanix Database Service Design, see the following references: 

- Microsoft SQL Server Database Workloads on Nutanix 

- Microsoft SQL Server on Nutanix Best Practices 

- Oracle on Nutanix Best Practices 

- PostgreSQL on Nutanix Best Practices 

- Nutanix On-Premises Hybrid Cloud with AHV Design 

© 2026 Nutanix, Inc. All rights reserved  | **71** 

Nutanix Database Service Design 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **72** 

Nutanix Database Service Design 

## **List of Figures** 

Figure 1: Nutanix Platform for Databases Architecture............................................................................... 12 Figure 2: Nutanix Database Service Features.............................................................................................13 Figure 3: Nutanix Database Service Architecture........................................................................................14 Figure 4: Sample Four-Node Cluster Hosting SQL Server Databases on Nutanix......................................24 Figure 5: Securing an Application................................................................................................................30 Figure 6: Prism Central Security Policy Example........................................................................................32 Figure 7: Hybrid Cloud On-Premises Monitoring Conceptual Design..........................................................34 Figure 8: Hybrid Cloud Performance Metrics Systems................................................................................35 Figure 9: SQL Server Database Example Disk File System....................................................................... 45 Figure 10: Logical and Physical Design for Oracle Database..................................................................... 47 Figure 11: Storage Design for a Medium PostgreSQL Database................................................................50 Figure 12: Unplanned Migration Task in Prism............................................................................................59 Figure 13: Monitor the Unplanned Migration Process................................................................................. 60 Figure 14: Monitor the Planned Migration Process..................................................................................... 61 Figure 15: Snapshot Creation with Time Machine.......................................................................................63 


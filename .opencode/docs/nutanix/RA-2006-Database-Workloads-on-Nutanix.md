# **Microsoft SQL Server Database Workloads on Nutanix** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

This content reflects an experiment in a test environment. Results, benefits, savings, or other outcomes described depend on a variety of factors including use case, individual requirements, and operating environments, and this publication should not be construed as a promise or obligation to deliver specific outcomes. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Microsoft SQL Server Database Workloads on Nutanix 

## **Contents** 

## **1. Microsoft SQL Server Database Workloads on Nutanix** 

**Introduction...............................................................................................5** 

Microsoft SQL Server Database Workloads on Nutanix Architectural Elements and Services...........7 

## **2. Nutanix Architecture Overview...............................................................8** 

Physical Layer.....................................................................................................................................8 Virtualization Layer............................................................................................................................11 Nutanix Prism Management Layer................................................................................................... 12 Business Continuity Layer................................................................................................................ 12 Database Layer.................................................................................................................................13 Security and Compliance Layer........................................................................................................13 Management Cluster.........................................................................................................................14 Quality Assurance Cluster................................................................................................................ 14 

## **3. Microsoft SQL Server on Nutanix........................................................ 15** 

Storage Platform for Microsoft SQL Server on Nutanix................................................................... 16 Availability and Resilience for Microsoft SQL Server on Nutanix.....................................................18 Scalability for Microsoft SQL Server on Nutanix.............................................................................. 20 

## **4. Performance Verification Configuration for Microsoft SQL Server** 

**on Nutanix...............................................................................................23** 

## **5. Performance Testing for Microsoft SQL Server on Nutanix.............. 27** 

Performance Validation Results for Microsoft SQL Server on Nutanix.............................................29 Validation for Microsoft SQL Server Always On Availability Groups on Nutanix.............................. 35 Performance Validation Results Summary for SQL Server on Nutanix............................................41 

## **6. Sample Bill of Materials for Microsoft SQL Server on Nutanix** 

**Solution....................................................................................................42** 

## **7. Recommendations and Requirements................................................ 44** 

Licensing............................................................................................................................................46 

**8. References.............................................................................................. 47 About Nutanix.............................................................................................48 List of Figures.............................................................................................................................................49** 

Microsoft SQL Server Database Workloads on Nutanix 

## 1. Microsoft SQL Server Database Workloads on Nutanix Introduction 

Enterprise database deployments don't follow a one-size-fits-all model. Requirements range from many small database servers with mixed operating systems and database software vendors to large, mission-critical datastores where redundancy is built into the software to environments where infrastructure provides availability and disaster recovery. This document focuses on the components required to run Microsoft SQL Server databases in a Nutanix environment. For an in-depth architectural overview, see the Nutanix Hybrid Cloud Reference Architecture. 

Key topics: 

- Architectural concepts required to run databases on Nutanix 

- Management options for running databases on Nutanix 

- Requirements and recommendations for setup, configurations, and best practices for databases on Nutanix 

- Validated use cases, configurations, and performance comparisons between hardware and AOS software versions 

This solution covers the features and functionality available in the following software versions: 

## _Table: Applicable Software Versions_ 

|**Software**|**Version**|**Description**|
|---|---|---|
|Nutanix AOS|6.5 and 6.10|Supported release|
|Nutanix AHV|v.20220304.342 and|Bundled with AOS version|
||v.20230302.102001|6.10|
|Windows|2022|Windows Server 2022|
|||Standard|
|SQL Server|2022|Enterprise Edition: Core-based|
|||Licensing|



© 2026 Nutanix, Inc. All rights reserved  | **5** 

Microsoft SQL Server Database Workloads on Nutanix 

## _Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|August 2021|Original publication.|
|1.1|December 2021|Added reference to the|
|||Database Migration tech note.|
|2.0|February 2022|Microsoft SQL Server|
|||Always On availability group|
|||validation. Updated product|
|||naming.|
|3.0|September 2022|Reconfigured DBaaS for|
|||End Users Design Example|
|||section. Significant text|
|||updates throughout.|
|4.0|July 2023|Updated for AOS 6.5.|
|||Provided a comparison|
|||between G7 and G8 clusters.|
|||Added PostgreSQL test|
|||results.|
|5.0|April 2024|Updated to add Microsoft SQL|
|||Server 2022 validation and|
|||to focus exclusively on SQL|
|||Server.|
|5.1|November 2024|Updated AOS release cycle|
|||descriptions.|
|6.0|January 2025|Updated for AOS 6.10 and NX|
|||G9 hardware.|
|6.1|February 2025|Updated graphs in the|
|||Performance Testing for|
|||Microsoft SQL Server on|
|||Nutanix section.|
|6.2|February 2025|Updated the|
|||Recommendations and|
|||Requirements section.|
|6.3|September 2025|Updated document structure.|



© 2026 Nutanix, Inc. All rights reserved  | **6** 

Microsoft SQL Server Database Workloads on Nutanix 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|6.4|March 2026<br>Updated the Performance<br>Testing for Microsoft SQL<br>Server on Nutanix section.|



## **Microsoft SQL Server Database Workloads on Nutanix Architectural Elements and Services** 

This solution uses the following architectural elements: 

- Nutanix block: The physical chassis that contains one to four Nutanix nodes 

- Nutanix cluster: A logical group of Nutanix nodes providing compute (CPU and memory) and storage according to availability, capacity, and performance requirements 

- Nutanix nodes: Physical servers that run Nutanix AHV as the hypervisor 

- Nutanix storage pool: A group of physical storage devices from the Nutanix nodes in the Nutanix cluster; NVMe, SSD, and HDD 

- Nutanix container: A logical segment of the storage pool that contains one or more virtual machines (VMs) or files 

- Prism Central: Nutanix cluster administration interface that runs in a separate VM or in multiple VMs and monitors and manages multiple Nutanix clusters through a single web console; not required but makes Nutanix multicluster management easier 

- Prism Element: Nutanix cluster administration interface that includes an HTML5-based UI, an API, and a command-line interface (CLI) 

© 2026 Nutanix, Inc. All rights reserved  | **7** 

Microsoft SQL Server Database Workloads on Nutanix 

## 2. Nutanix Architecture Overview 

Nutanix converges compute, storage, networking, and virtualization into a simple platform, replacing the separate servers, storage systems, and storage area networks (SANs) in conventional datacenter architectures. Each node in a Nutanix cluster includes compute, memory, and storage and runs an industry-standard hypervisor and a virtual storage controller called the Controller VM (CVM). 

## Figure 1: Nutanix Hybrid Cloud On-Premises Architecture 

## **Physical Layer** 

Nutanix AOS pools storage and distributes operating functions across all nodes in the cluster for performance, scalability, and resilience. 

© 2026 Nutanix, Inc. All rights reserved  | **8** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 2: Nutanix Hyperconverged Infrastructure Overview 

Nutanix provides flexibility for hardware platform selection. You can select from the following options: 

- Nutanix NX appliances 

- OEM appliances from leading vendors, including Cisco, Dell, Lenovo, HPE, IBM, and Fujitsu 

- Other third-party servers from a wide range of vendors; contact Nutanix Support for the detailed list 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 3: Overview of Nutanix Physical Layer 

## **Compute** 

The Controller VM (CVM) runs Nutanix AOS and serves I/O operations to all the VMs running on that host, so you must include enough compute CPU and memory to support each CVM. 

Nutanix Foundation assigns a certain number of vCPUs to the CVMs based on node hardware (such as storage devices and CPU type) when you initially set the nodes up with Nutanix. The vCPU setting from Foundation is typically sufficient when you correctly size your hardware. However, we recommend sizing the CVM with 16 vCPUs for storage-intensive database workloads; the minimum is 12 vCPUs for the CVM. The CVM doesn't always use all the vCPU you assign it daily. For more information on how Foundation determines the number of vCPUs to assign to a CVM, see Controller VM (CVM) Field Specifications. 

We recommend configuring the CVM with 64 GB of memory. 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

Microsoft SQL Server Database Workloads on Nutanix 

## **Storage** 

Nutanix nodes support a range of storage configurations: Hybrid nodes that combine SSDs for performance and HDDs for capacity and all-flash nodes that support SATA, SAS-based SSDs, and NVMe SSDs (3D NAND, Intel Optane). 

## **Networking** 

Fast, low-latency, and highly available networking is critical for the resilience and performance of database workloads. We recommend 25 GbE or faster network interfaces for database deployments. For more information, see Physical Networking Best Practices. 

## **Virtualization Layer** 

The virtualization layer controls VM access to compute, network, and storage resources. This solution has two virtualization constructs: the hypervisor and the Nutanix cluster. 

The hypervisor hosts VMs. Although Nutanix supports Nutanix AHV, VMware ESXi, and Microsoft Hyper-V, this document only covers Nutanix AHV. 

A Nutanix cluster is a group of three or more physical nodes working as a single entity. A hypervisor cluster is a storage management boundary for a group of VMs. When you use Nutanix AHV as the hypervisor, you create the Nutanix cluster and the hypervisor cluster at setup. With the other supported hypervisors, you create the Nutanix cluster separately from the hypervisor cluster. 

You can build a Nutanix cluster to support mixed workloads or have dedicated clusters for each workload type in a block-and-pod design. Dedicated cluster designs can include any of the following cluster types: 

- Management clusters run datacenter management VMs, including the following workloads: 

   - › Nutanix Prism Central 

   - › VMware vCenter 

   - › Active Directory domain controllers 

   - › Other management workloads such as DNS, DHCP, IPAM, NTP, Syslog, or SMTP 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

Microsoft SQL Server Database Workloads on Nutanix 

- Workload clusters run application VMs, including virtual desktops, databases, web servers, and other application services. You can mix different types of compute clusters and provide separate compute pools to address different service-level agreements (SLAs) for availability and performance. 

- Storage clusters are dedicated to data services. You can deploy storage clusters for object-, file- (NFS, SMB), or block-level storage. 

## **Nutanix Prism Management Layer** 

Nutanix Prism combines multiple aspects of datacenter management into a single consumer-grade design that delivers complete infrastructure and virtualization management, operational insights, and troubleshooting, reducing the need for separate management tools. Nutanix Prism consists of three products: 

- Prism Element enables management and monitoring at the cluster level for all infrastructure (compute, storage, networks) and virtualization. 

- Prism Central manages and monitors multiple Prism Element clusters from a central interface. 

- Nutanix Cloud Manager (NCM) Intelligent Operations adds advanced capabilities to the Nutanix Prism platform, including performance anomaly detection, capacity planning, custom dashboards, reporting, advanced search capabilities, and task automation. 

For more information on each product's benefits, see Nutanix Prism, the Prism Central Guide, and NCM Intelligent Operations Tiers on the Nutanix Support Portal. 

## **Business Continuity Layer** 

You can use the Nutanix platform's redundancy for power and hardware components to configure resilience for entire datacenters. Nutanix offers software-based backup, restore, and disaster recovery options that remove the need for specialized hardware components. Nutanix can provide native protection through the hypervisor, third-party software, or a combination of these methods. 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

Microsoft SQL Server Database Workloads on Nutanix 

## **Database Layer** 

You can run any database engine on Nutanix if the target hypervisor supports the VM guest OS unless otherwise stated by the database engine vendor. The following list provides a few examples of currently supported database engines: 

- Db2 

- HBase 

- IRIS (Caché) 

- MariaDB 

- Microsoft SQL Server 

- MongoDB 

- MySQL 

- Oracle 

- PostgreSQL 

- SAP HANA 

- SingleStore 

This solution focuses on Microsoft SQL Server. 

## **Security and Compliance Layer** 

Nutanix takes a security-first approach that includes a secure platform, extensive automation, and a robust partner ecosystem. We built security into every step of the software development process, from design and development to testing and hardening. 

Nutanix helps you achieve compliance with the following international security standards: 

- Section 508 

- FIPS 140-2 Level 1 

- National Institute of Standards and Technology (NIST) 800-53 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

Microsoft SQL Server Database Workloads on Nutanix 

- Trade Agreements Act (TAA) 

With Nutanix, you can protect your environment from many threat vectors, including active, automated, internal, and external threats. The Nutanix security approach doesn't require setting hundreds of configuration options to achieve a secure environment. However, configuration options are available if you need to add an extra layer of security based on business or technical requirements. 

## **Management Cluster** 

A management cluster hosts additional architectural services, Prism Central, VMware vCenter, and the Nutanix Database Service (NDB) management plane VMs. Design and implement the management cluster to meet the highest uptime requirements and the lowest recovery point objective (RPO) and recovery time objective (RTO) defined for the services consuming the management cluster services. The management cluster doesn't host user VMs or services. This document's focus workload includes the database server VMs hosted in Nutanix workload clusters. 

For a small-scale deployment, designing, purchasing, and operating a separate cluster for a limited set of management VMs is usually not cost-effective. Contact the Nutanix team for advice on the best action. 

## **Quality Assurance Cluster** 

A quality assurance cluster ensures that certain changes don't affect a production system. For example, if a software upgrade goes wrong, you don't want it to affect the production system. With a quality assurance cluster, you can validate changes without risking production workloads. 

Although a quality assurance cluster isn't feasible or necessary for every installation, it adds a layer of environmental resilience. It is especially worth considering when availability and resilience are paramount. 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

Microsoft SQL Server Database Workloads on Nutanix 

## 3. Microsoft SQL Server on Nutanix 

Nutanix offers several methods for running SQL Server databases: You can run VMs with a database engine installed on the Nutanix platform; provision, manage, protect, clone, and refresh your databases using Nutanix Database Service (NDB); use the Nutanix Kubernetes Engine (NKE) to simplify provisioning and operating Kubernetes clusters; or deploy Microsoft SQL Server Linux containers on NKE. Base your decision on your requirements and conditions. 

Databases are one of the most valuable asset categories in a company's IT infrastructure because they hold crucial data for every business aspect. For this reason, databases must maintain data integrity, availability, and performance at all times. Nutanix is an excellent choice as the basis for a database environment because it offers the following advantages: 

- Simplifies management with a single management pane, so you don't need to worry about SAN management tasks such as multipathing, zoning, or masking 

- Uses high availability and data redundancy to withstand software and hardware failures, which means your databases stay running even if the underlying hardware fails 

- Provides low I/O latency and predictable performance while supporting a wide range of transactional and analytical database work 

- Enables you to start small and scale performance and capacity as your needs grow 

- Offers integrated snapshots, remote replication, and metro-level availability to protect the data 

A typical Nutanix database deployment includes additional components such as database management, business continuity, and monitoring tools. 

You can also migrate existing databases to Nutanix. For an outline of the high-level processes for migrating SQL Server databases to Nutanix, see Database Migration Strategies on Nutanix. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

Microsoft SQL Server Database Workloads on Nutanix 

## **Storage Platform for Microsoft SQL Server on Nutanix** 

The Nutanix platform is a scale-out architecture that distributes disks and I/O across a shared-nothing cluster that scales linearly. Nutanix supports various storage devices, such as Optane, NVMe, and SSD. We recommend using an all-flash configuration for databases, including the following options: 

- SATA SSD 

- NVMe SSD 

- Hybrid of NVMe and SATA SSD 

- Premium hybrid of Optane and 3D NAND SSD 

Using Nutanix Objects or Nutanix Files on separate hybrid storage (SSD and HDD) can reduce long-term storage costs for backups and archived data. 

We recommend creating multiple virtual disks (vDisks) for each file system that requires low latency or high storage throughput. The optimal number and size of the vDisks depend on the size and activity of the database and the best practice guidance from the underlying database engine vendor. 

You can use the following methods to provision vDisks for VMs: 

- Use Nutanix native vDisks to simplify VM administration and add disks to VMs using the appropriate hypervisor management tool (Nutanix Prism for AHV). 

If you require shared storage (Microsoft SQL Server failover cluster instance), use a volume group (VG) instead of native vDisks. 

- Add Nutanix VGs (logical constructs that contain one or more vDisks) directly to AHVbased VMs in the same cluster using the hypervisor management tool, then VMs outside the cluster or physical servers access VGs using in-guest iSCSI. With VGs, 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

Microsoft SQL Server Database Workloads on Nutanix 

you can configure a cluster with shared disk access across multiple VMs. Nutanix offers two types of VGs: 

- › Default: This type of VG provides the best data locality because it doesn't loadbalance vDisks across nodes in a Nutanix cluster. All vDisks in the VG have a single Controller VM (CVM) providing their I/O. 

   - For example, in a four-node Nutanix cluster that includes a VG with eight vDisks attached to a VM, a single CVM owns all the vDisks, and all I/O to the eight vDisks goes through this CVM. 

- › Volume Group with Load Balancing (VGLB): A VGLB distributes ownership of the vDisks across all the CVMs in the cluster. 

Each environment has different requirements based on its unique workload patterns. Always test and compare the performance of native vDisks, default VGs, and VGLBs to see which choice best fits your environment. 

The following Nutanix vDisk configuration example applies to AHV environments. 

Figure 4: SQL Server vDisk Layout 

_Table: General Guidance for Database VM vDisk Configuration_ 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

Microsoft SQL Server Database Workloads on Nutanix 

|**Number of vDisks**|**Purpose**|
|---|---|
|At least 4|Database data files|
|At least 1|Database log files|
|At least 4|Database backup files(for local backups, if|
||required)|
|At least 2|TempDB|
|At least 1|TempDB log files|



For more information on SQL environment configuration recommendations, see Microsoft SQL Server on Nutanix Best Practices. 

## **Availability and Resilience for Microsoft SQL Server on Nutanix** 

You can use database software and infrastructure tools to protect your application; the best selection for your environment depends on your requirements. We recommend using database-level resilience where possible because the database vendor knows best how to protect your database data. However, your RPO and RTO requirements, software capabilities, and licensing might necessitate a combination of application and infrastructure resilience. 

SQL Server on Nutanix offers the following resilience options for different failure scenarios: 

- Physical server failure: 

   - › Nutanix AOS 

   - › Hypervisor high availability 

   - › Database engine: Microsoft SQL Server failover clustering and SQL Server Always On availability groups (AG) 

- VM failures: Database engine: Microsoft SQL Server clustering (AG or failover cluster instance) 

© 2026 Nutanix, Inc. All rights reserved  | **18** 

Microsoft SQL Server Database Workloads on Nutanix 

- Storage failure: 

   - › AOS: AHV redirects VM disk I/O to a surviving Controller VM (CVM) during CVM failure 

Replication factor (2 or 3) ensures that data is available for VMs if one or two Nutanix nodes fail. 

- › Protection domain 

- › Database engine: SQL Server AG 

For more information, see Infrastructure Resilience. 

- Site failure with remote backup: 

   - › Hypervisor high availability when you use Metro Availability or synchronous replication with automatic failover (not supported with Nutanix Database Service) 

   - › Database engine: SQL Server AG 

   - › Cluster storage replication (synchronous, asynchronous, NearSync) 

- Administrative failure (intentional and unintentional logical errors including data deletion, storage container deletion, malware, or sabotage): 

   - › Backup and restore 

   - › Storage snapshots (protection domains) 

   - › Database engine: SQL Server AG 

Whether the replicated data protects against intentional and unintentional logical errors depends on the data replication delay. 

- Database failure: 

   - › Backup and restore 

   - › Database engine capabilities: High availability, disaster recovery availability, and schema and data replication 

Always test the solution's availability and resilience before going live to determine whether it meets your business requirements. Also, keep your disaster recovery and business continuity plan updated. 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

Microsoft SQL Server Database Workloads on Nutanix 

The following table lists the database technologies offering maximum availability and database granularity (point-in-time recovery) while avoiding data loss. 

_Table: Database Vendor Disaster Recovery Capabilities_ 

|**Area**|**SQL Server**|
|---|---|
|High availability|Windows Server Failover Clustering (WSFC),|
||SQL Server AGs|
|Disaster recovery|SQL Server AGs|
|Schema or data replication|SQL Server Replication, SQL Server log|
||shipping|
|Data resilience|SQL Server AGs, SQL Server log shipping|



## **Scalability for Microsoft SQL Server on Nutanix** 

The physical resources required to host database workloads are similar across database solutions; the difference is in where you host management services and how you protect your database data. A well-designed solution has the initial resources required to deliver availability and performance for your database workloads and provides a path to support future workloads. 

To determine the best Nutanix cluster size, consider the following items: 

- Business continuity, including backup windows, RPO, and RTO 

- Manageability, including maintenance of windows 

- Security zones 

- Failure domains 

- Environment purposes, such as development, production, stage, test, or quality assurance 

- Hardware capability, such as Nutanix nodes and network equipment 

- Datacenter capabilities, including total power availability, power per rack availability, cooling, and rack space 

- Application and database engine licensing 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

Microsoft SQL Server Database Workloads on Nutanix 

- Operating system licensing 

Database software can offer both vertical and horizontal scaling options. The best method for scaling your environment depends on your workload. Nutanix supports all the scalability options supported by the database vendors. 

## **Nutanix Cluster Scalability** 

One of the key benefits of the Nutanix platform is scalability. You can add compute and storage to a Nutanix solution as needed to build a perfect match for any deployment type: 

- Greenfield, a fresh system where you might not know the resource requirements 

- Brownfield, an existing system where you know the resource requirements and can estimate future requirements 

- On-premises solutions where you own the resources required to run the solution 

- On-premises solutions where you rent the resources required to run the solution 

You can scale up a node by adding more memory, storage, or nodes to the cluster when you require more resources. Depending on your implementation model and availability domain specification, you might need to add more than one node to your cluster when required to increase resources. 

The following list provides example situations where you need to add more than one node at a time to the solution: 

- You implemented the cluster across racks, and each rack requires equal resources. 

Each rack is a failure domain. 

- You added a new Nutanix node type to the cluster. 

- You implemented multiple Nutanix clusters to provide backup or disaster recovery to each other, and they require equal capacity. 

You can find Nutanix configuration maximums on the Nutanix Portal. 

Following are some of the scalability cluster configuration items you need to be aware of when supporting a growing solution: 

- Maximum number of nodes per cluster 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

Microsoft SQL Server Database Workloads on Nutanix 

- Node configuration: 

   - › Memory slots populated 

   - › Disk drives populated 

- Smallest scalability increment (for example, one node) 

- Failure domain (for example, one rack) 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

Microsoft SQL Server Database Workloads on Nutanix 

## 4. Performance Verification Configuration for Microsoft SQL Server on Nutanix 

Solution performance depends on many factors, including hardware, software, and workload, so providing data for all possible scenarios is impossible. Considering these constraints, our objective throughout this document is to provide general guidelines on the solution's performance under benchmark conditions. We carried out a software benchmark comparison between Nutanix AOS versions 6.5 and 6.10 on G9 hardware and a hardware benchmark comparison between G8 and G9 using AOS 6.10 on a greenfield configuration. 

We used the following technologies to develop this solution. 

Nutanix software: 

- AOS Version 6.5 and 6.10 

- AHV Version v.20220304.342 and v.20230302.102001 

Database software: 

- Microsoft Windows Server 2022 Standard Edition 

- Microsoft SQL Server 2022 Core Edition 

Benchmark utility: HammerDB v4.6 

Hardware: 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

Microsoft SQL Server Database Workloads on Nutanix 

- G8 cluster specification 

   - › Server model: NX-8170-G8 (4 servers) 

   - › CPU: 2 × Intel Xeon Gold 6354, 18 cores per socket @ 3.0 GHz 

   - › Memory: 1 TB (32 × 32 GB DDR4 DIMMs @ 3,200 MHz) 

   - › Network cards: 

      - 2 × Intel Ethernet Controller X710 dual-port 10 GbE (not used) 

      - 1 × NVIDIA Mellanox MT2892 Family [CX-6] dual-port 100 GbE 

   - › Network connection: 2 × 100 GbE LACP active-active 

   - › Disks: 8x NVMe 3.84 TB Samsung MZQL23T8HCLS-00A07 

- G9 cluster specification 

   - › Server model: NX-8150-G9 (4 servers) 

   - › CPU: 2 × Intel Xeon Gold 6442Y CPU 24 cores @ 2.6 GHz 

   - › Memory: 1TB 

   - › Network cards: CX-6 - 2 × 100 GBE Active Active 

   - › Disks: 20x NVME 3.84 TB Samsung MZQL23T8HCLS-00A07 

Solution validation: We created a storage container with compression enabled with zero delay. Deduplication and erasure coding were disabled for database workloads. 

In this solution, we configured a single virtual switch in the cluster using NVIDIA Mellanox CX-6 with 100 GbE cards. For simplicity, we used an active-active leaf-spine network topology, which requires at least two spine switches, two leaf switches, and LACP at the switch end. Every leaf connects to every spine using 100 GbE uplink ports and connects to every node using a 100 GbE virtual switch. 

The following figure outlines the general AHV host network configuration and physical switch connections: 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 5: AHV Host Network Physical Configuration 

We configured the SQL Server VMs according to Microsoft SQL Server on Nutanix Best Practices, and we highly recommend that you follow this guide for any SQL Server environment. 

We used the following SQL Server VM configuration: 

- vCPU: 16 

- Memory: 64 GB for the VM, with 55 GB assigned to the SQL Server 2022 instance 

- Disk layout: 

   - › 2 vDisks for SQL Server data 

   - › 1 vDisk exclusively for UserLogs 

   - › 2 vDisks for TempDB data 

   - › 1 vDisk for the SQL Server system 

   - › 1 vDisk for the TempDB log 

- Data files: 4 per vDisk (8 total) in one filegroup 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

Microsoft SQL Server Database Workloads on Nutanix 

- SQL compatibility mode: SQL 2022 

- Query store: Disabled 

- Soft-NUMA: Enabled 

- Instant file initialization (IFI): Enabled 

- OS: Windows 2022 Standard Edition 

- Database: SQL Server 2022 Core Edition 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

Microsoft SQL Server Database Workloads on Nutanix 

## 5. Performance Testing for Microsoft SQL Server on Nutanix 

To validate the SQL Server solution, we used the HammerDB benchmark tool to run TPROC-C, an OLTP-like workload, against the database servers and monitored the performance of the database engine. HammerDB is a database workload generator that can benchmark database engines using industry-standard tests based on the specifications published by the Transaction Processing Performance Council (TPC). We used HammerDB to perform transactions without delay, driving the maximum number of transactions and IOPS. Real-world scenarios have user delay, but this approach lets us observe the performance of the system under the most demanding workload. During benchmark testing, we captured the transactions per minute and the new orders per minute to verify performance. 

We used the following parameters for the HammerDB TPROC-C benchmark configuration: 

- Number of virtual users: 400 

- Think time or delay time: 1 ms 

- Number of transactions per user: 1,000,000 

- Number of warehouses: 5,000 

- Use all warehouses: True 

- Ramp-up time: 5 minutes 

- Steady runtime: 30 minutes 

- Number of runs: 5 

- Results collected: Removed outlier values (lowest and highest) and averaged the remaining three runs 

The following figure shows the layout of the components for the HammerDB benchmark runs. 

© 2026 Nutanix, Inc. All rights reserved  | **27** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 6: SQL Server Physical Client-Server Design 

We had separate clusters hosting the SQL Server system under test (SUT) and the HammerDB clients. In a typical client-server concept, these clusters have a 1:1 ratio, as shown in the previous figure. 

© 2026 Nutanix, Inc. All rights reserved  | **28** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 7: SQL Server SUT with Storage View 

## **Performance Validation Results for Microsoft SQL Server on Nutanix** 

We tested the following scenarios: 

- Performance of a single database VM 

- Scale-out operation of database VMs 

- Scale-up operation of a database VM 

- Performance of single synchronous and asynchronous Always On availability group (AG) configurations 

- AG scale-out operations 

© 2026 Nutanix, Inc. All rights reserved  | **29** 

Microsoft SQL Server Database Workloads on Nutanix 

For each of the test scenarios, we ran separate test iterations to evaluate the performance effect of using AOS 6.10 over AOS 6.5 and NX-8150-G9 over NX-8170-G8 servers. 

We then compared the results of each test to derive the performance comparison for Nutanix AOS version changes as a greenfield configuration and with hardware changes, as described in the following sections. 

## **Single VM Test for SQL Server on Nutanix** 

We first compared the Nutanix platform (AOS) version results for the single SQL Server VM benchmark between AOS versions 6.5 and 6.10 hosted on NX-8150-G9 servers to verify the software performance comparison. These results demonstrate the performance you can achieve in ideal conditions with no other workloads running in the cluster. 

Figure 8: Single SQL Server VM, AOS 6.5 vs. AOS 6.10 

A comparison of the Nutanix platforms showed a 63 percent performance gain for the same workload with the latest AOS version due to the efficiencies made in the core data path in AOS 6.10. 

We ran a separate set of tests using a Single SQL Server database VM with AOS 6.10 hosted on NX-8150-G9 servers. We compared the results against the same AOS version 6.10 hosted on earlier generation NX-8170-G8 servers to verify the hardware performance comparison. 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 9: Single SQL Server VM, G8 vs. G9 on AOS 6.10 

When we compared the performance of hardware benchmarks on G8 and G9 with AOS 6.10, we found that the benchmark achieved an 11 percent performance gain despite the G9 hardware being configured with lower clock speed CPUs than the G8. 

## **VM Scale-Out Test for SQL Server on Nutanix** 

For the scale-out benchmark, we started with one SQL Server VM running HammerDB TPROC-C and scaled out one VM at a time to four VMs, with one VM per node and one SQL Server database per VM. Scaling out tests the impact of running concurrent workloads on a cluster. 

Figure 10: VM Scale-Out Comparison, AOS 6.5 vs. AOS 6.10 

© 2026 Nutanix, Inc. All rights reserved  | **31** 

Microsoft SQL Server Database Workloads on Nutanix 

Comparing AOS 6.5 and 6.10 on G9 hardware, we observed consistent linear improvement in performance as we added more VMs to the cluster. With AOS 6.10, we achieved more than 63 percent performance gain during each scale-out benchmark due to efficiencies made in the core data path in AOS 6.10. 

Figure 11: VM Scale-Out Comparison, G8 vs. G9 on AOS 6.10 

Analyzing the results of the hardware comparison benchmark, we found that as the load in the cluster increased, the environment could sustain the performance gain and linearly increase throughout as the number of VMs increased. 

## **Memory Scale-Up Test for SQL Server on Nutanix** 

If you need more capacity or performance and scaling out is not an option due to technology or licensing limitations, consider scaling up instances by adding vCPU, memory, or storage to a VM. 

To validate the scale-up topology, we ran a HammerDB TPROC-C benchmark against a SQL Server VM configured with 16 vCPUs. For each run, we changed the VM's memory allocation to test with 32 GB, 64 GB, 128 GB, 256 GB, and 512 GB. We allocated SQL instance memory resources during each test according to Microsoft SQL Server on Nutanix Best Practices. 

© 2026 Nutanix, Inc. All rights reserved  | **32** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 12: SQL Server Memory Scale-Up Comparison, AOS 6.5 vs. AOS 6.10 

Allocating more memory to the system allows the database workloads to cache the active data set and provide better optimization by reducing I/O contentions in the system. Analyzing the results of the Nutanix platform benchmark, we found significant optimization at the low end of the memory allocation (32 GB, 64 GB, and 128 GB), with optimization of 24 percent, 66 percent, and 53 percent, respectively. The efficiencies made in the core data path in AOS 6.10 provide better optimization and value. At the high end of the memory allocation, we observed 8 percent (256 GB) and 6 percent (512 GB) optimization for memory allocation. Even though the system has cached the entire data set in the memory and is already running without any contention at these levels, the benchmark shows that the system achieves further efficiencies with AOS 6.10. 

Figure 13: SQL Server Memory Scale-Up Comparison, G8 vs. G9 on AOS 6.10 

The hardware performance analysis showed that G9 hardware outperformed in all scenarios despite the G9 host having a lower clock speed CPU than the G8 environment. We did not observe any improvements in the low-end memory allocation of the 32 

© 2026 Nutanix, Inc. All rights reserved  | **33** 

Microsoft SQL Server Database Workloads on Nutanix 

GB system because most of the transactions were I/O bound at the low-end memory allocation. However, the system was able to sustain itself, highlighting the importance of sizing the system appropriately to support your performance requirements. 

## **vCPU Scale-Up Test for SQL Server on Nutanix** 

In this test iteration, we ran HammerDB TPROC-C benchmarks in a SQL Server VM with 64 GB of memory to verify the CPU scale-up capabilities. We used different CPU amounts (8, 16, and 32 vCPU) to validate CPU scale-up. 

Figure 14: SQL Server vCPU Scale-Up Comparison, AOS 6.5 vs. AOS 6.10 

As with memory scale-up, configuring the database system with the appropriate amount of CPU determines how efficiently the system can function to serve the database requests; however, in each of the scenarios, we verified that AOS 6.10 outperformed AOS 6.5. Delivering efficient I/O management with AOS 6.10 allowed the system to free up resources to drive 19 percent better performance, even at the low end of CPU allocation. When the system was configured with more resources, the system was able to drive 55 percent more transactions with 16 CPU allocations and 70 percent more transactions with 32 CPU configurations compared to AOS 6.5. 

The benchmark also demonstrates that adding more CPUs to the system doesn't guarantee incremental performance. For optimal system performance, investigate bottlenecks and increase system resources appropriately. 

© 2026 Nutanix, Inc. All rights reserved  | **34** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 15: SQL Server vCPU Scale-Up Comparison, G8 vs. G9 on AOS 6.10 

The hardware comparison benchmarks show that systems configured on G9 hardware outperformed G8 configurations throughout the scenarios. We observed that the system configured with 8 CPUs provided a 2 percent improvement despite the G9 CPU having a lower clock speed than the G8 CPUs. Meanwhile, systems configured with 16 and 32 CPUs provided 5% and 14% gain, respectively, highlighting the importance of sizing the system appropriately. 

## **Validation for Microsoft SQL Server Always On Availability Groups on Nutanix** 

We deployed two Always On availability group (AG) instances on the same Windows test cluster configured on the four-node Nutanix cluster. We used the same VM and database configurations described in the configuration sections. 

© 2026 Nutanix, Inc. All rights reserved  | **35** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 16: SQL Server AG Benchmark Design 

For the first round, we tested the primary SQL Server database VM (Replica 1) and Replica 2. Subsequently, we added two more SQL Server VMs: Replica 3 and Replica 4. 

We created a storage container for the SQL Server VMs from the existing storage pool with replication factor 2, compression enabled, and delay at zero. We created the vDisks from this storage container, which had sharding enabled by default. 

The primary SQL Server database VM (Replica 1) and the subsequent replicas had identical configurations, according to Microsoft SQL Server on Nutanix Best Practices. 

You can set up SQL Server AG in synchronous or asynchronous configurations. We carried out separate test iterations for synchronous and asynchronous replica performance tests, which provided us with much larger data points. 

_Table: SQL Server AG Test Configuration_ 

|**Item**|**Configuration**|
|---|---|
|Number of replicas|4 (1 primary and 3 secondary)|
|Automated backup preference|Secondary|
|Required synchronized secondaries to commit|0 for asynchronous replica Always On|
||configuration; 1 for synchronous replica Always|
||On configuration|
|Availability mode|Asynchronous commit or synchronous commit|
|Seeding mode|Manual|



© 2026 Nutanix, Inc. All rights reserved  | **36** 

Microsoft SQL Server Database Workloads on Nutanix 

|**Item**|**Configuration**|
|---|---|
|Secondary role (allow connections)|No|



We connected to the primary replica of the SQL Server Always On cluster to test workloads and performed the initial set of tests on a two-node configuration. Then, we performed several scale-out test scenarios to evaluate the performance. 

## **Two-Node SQL Server Always On Availability Group Tests on Nutanix** 

We tested synchronous and asynchronous SQL Server Always On availability group (AG) configurations. 

We conducted the following verification using the same HammerDB TPROC workload we used to verify the single instance configuration. During the verification, we configured the AG using synchronous replication with two database nodes. We derived the following benchmark results by running a HammerDB workload on the primary replica and taking an average of three benchmark runs. 

Figure 17: SQL Server Always On Availability Group Synchronous Comparison, AOS 6.5 vs. AOS 6.10 

Analysis of the benchmark data on two database nodes using synchronous AG replication showed that, with the efficiencies made in the core data path on AOS 6.10, we achieved a 66 percent performance gain over the previous AOS version for the same workload execution. 

© 2026 Nutanix, Inc. All rights reserved  | **37** 

Microsoft SQL Server Database Workloads on Nutanix 

Figure 18: SQL Server Always On Availability Group Synchronous Comparison, G8 vs. G9 on AOS 6.10 

The hardware comparison showed that systems configured on G9 hardware outperformed systems configured on G8 hardware by 3 percent. This performance gain was achieved despite the G9 CPUs having a lower clock speed than the G8 CPUs. 

Following the synchronous configuration testing, we gathered the following results by conducting the same HammerDB benchmarks, except with asynchronous availability group configuration. 

Figure 19: SQL Server Always On Availability Group Asynchronous Comparison, AOS 6.5 vs. AOS 6.10 

© 2026 Nutanix, Inc. All rights reserved  | **38** 

Microsoft SQL Server Database Workloads on Nutanix 

The result showed that AOS 6.10 outperformed AOS 6.5 by 62 percent, providing a significant performance boost for the same configuration in the G9 environment. This again highlights significant core data path optimization made in AOS 6.10 to optimize the I/O performance. 

Figure 20: SQL Server Always On Availability Group Asynchronous Comparison, G8 vs. G9 on AOS 6.10 

The hardware comparison showed that systems configured on G9 hardware outperformed systems configured on G8 hardware by 3 percent. This performance gain was achieved despite the G9 CPUs having a lower clock speed than the G8 CPUs. 

Benchmark data also highlights that synchronous and asynchronous replication provided almost the same throughput in the AOS 6.10 environment. Synchronous replication has always been resource-intensive and has traditionally had lower performance. AOS 6.10 reduces the synchronization waits and performs similarly to the asynchronous replication configuration. 

## **Scale-Out SQL Server Always On Availability Group Tests on Nutanix** 

We scaled out the Always On cluster from two replicas to four replicas (one replica on each node) and executed the HammerDB workload against the primary replica to evaluate the performance scalability. 

As all the tests were conducted on the same cluster with high-bandwidth, low-latency connectivity, we achieved similar test results for both synchronous and asynchronous configurations. While we carried out the following tests using a synchronous availability 

© 2026 Nutanix, Inc. All rights reserved  | **39** 

Microsoft SQL Server Database Workloads on Nutanix 

group configuration, the results in the following figure apply to both asynchronous and synchronous configurations. 

Figure 21: SQL Server Always On Scale-Out Tests Comparison, AOS 6.5 vs. 6.10 

Nutanix platform comparison results show that AOS 6.10 significantly outperformed the environment configured using AOS 6.5, providing a performance gain of over 63 percent for all the configurations we validated. 

Figure 22: SQL Server Always On Scale-Out Comparison, G8 vs. G9 on AOS 6.10 

© 2026 Nutanix, Inc. All rights reserved  | **40** 

Microsoft SQL Server Database Workloads on Nutanix 

The G9 hardware environment outperformed the G8 environment, providing 2 percent, 7 percent, and 7 percent performance gains over the G8 two-, three-, and four-node AG environments, despite the G9 CPUs' lower clock speed than G8 CPUs. 

## **Performance Validation Results Summary for SQL Server on Nutanix** 

The following points summarize the results of the performance tests that we ran for the SQL Server on Nutanix solution: 

- Our benchmarks show that using AOS 6.10 significantly optimizes I/O performance. 

- Scale-out and scale-up benchmarks show that a Nutanix environment can achieve linear scalability as you add more databases. Nutanix also offers the advantage of starting with a small cluster and adding nodes to expand the cluster capacity as the environment grows. 

For more information, see Supported Hardware Platforms and Public Clouds. 

© 2026 Nutanix, Inc. All rights reserved  | **41** 

Microsoft SQL Server Database Workloads on Nutanix 

## 6. Sample Bill of Materials for Microsoft SQL Server on Nutanix Solution 

The following section provides a sample bill of materials. You can define the number of nodes based on the configuration size: 

- Small: 4 nodes 

- Medium: 6 nodes 

- Large: 8 nodes 

Hardware: 

- NX-8170-G8 hardware platform, one node: 

   - › 2 × Intel(R) Xeon(R) Gold 6354 CPU 18 cores @ 3.00 GHz 

   - › 1 TB memory 

   - › 8 × NVMe 3.84 TB Samsung MZQL23T8HCLS-00A07 

   - › 2 × 100 GbE active-active network 

- NX-8150-G9 hardware platform, one node: 

   - › 2 × Intel Xeon Gold 6442Y CPU 24 cores @ 2.6 GHz 

   - › 1 TB Memory 

   - › 20 × NVMe 3.84 TB Samsung MZQL23T8HCLS-00A07 

   - › 2 × 100 GbE active-active network 

## Software and services: 

© 2026 Nutanix, Inc. All rights reserved  | **42** 

Microsoft SQL Server Database Workloads on Nutanix 

- Nutanix software: 

   - › Nutanix Foundation: Hypervisor Agnostic Installer 

   - › Controller VM 

   - › Prism Management 

   - › Starter License Entitlement 

- Subscription, Nutanix Database Service (NDB) Platform Software License & Mission Critical Software Support Service for 1 CPU Core 

- Mission-critical level hardware support for Nutanix HCI appliance available at any time 

© 2026 Nutanix, Inc. All rights reserved  | **43** 

Microsoft SQL Server Database Workloads on Nutanix 

## 7. Recommendations and Requirements 

The following table lists the minimum and recommended hardware for running databases on Nutanix and provides configuration recommendations. 

This table doesn't account for capital expenditures. 

_Table: Minimum and Recommended Hardware for Databases on Nutanix_ 

|**Component**|**Minimum**|**Recommended**|
|---|---|---|
|BIOS|Keep up to date|Keep up to date.|
|Power management through|AHV enables high|—|
|hypervisor|performance automatically.||
|CPU type|Intel Gold|Intel Gold or Platinum|
|CPU core count|12|16 and above|
|CPU speed|Intel 2.7 GHz|2.8 GHz and above|
|CPU C-states|Disabled|Disabled|
|CPU: NUMA and|Enabled|Enabled|
|hyperthreading|||
|CPU: Intel (VMX & VT-x) and|Enabled|Enabled|
|AMD (AMD-V)|||
|Memory|Balanced configuration. See|Optimal performance|
||Nutanix Physical Memory|configuration for Intel CPUs is|
||Configurationfor Intel CPUs.|detailed inNutanix Physical|
|||Memory Configuration.|
|Storage|All-flash SSD|All-flash NVMe or NVMe +|
|||Optane|
|Number of I/O slots|8|12 or 24|
|Network topology|Traditional aggregation and|Leaf-spine|
||access layer||
|Network speed|Dual > 25 Gbps|Dual 25 Gbps or dual 100|
|||Gbps|



© 2026 Nutanix, Inc. All rights reserved  | **44** 

Microsoft SQL Server Database Workloads on Nutanix 

|**Component**|**Minimum**<br>**Recommended**|
|---|---|
|Network connection<br>Network placement|Connect all Nutanix nodes to<br>the same pair of switches if<br>possible.<br>Recommend LACP<br>Place the CVM and hypervisor<br>host in a dedicated network.<br>Do not run databases or<br>other user workloads on the<br>network.<br>—|



Nutanix cluster configuration varies based on which database engine you run in the Nutanix cluster and on the type of workload the database engine hosts. 

_Table: Recommended Nutanix Cluster Configuration_ 

|**Component**|**Configuration**|**Comment**|
|---|---|---|
|Failover capacity|n + 1|n + 1 or n + 2 depending on|
|||your requirements; monitor|
|||usage to ensure enough|
|||capacity for an n + 1 scenario.|
|Minimum nodes per Nutanix|Use at least 4 nodes (n + 1)|—|
|cluster|||
|Maximum nodes per Nutanix|Use at most 16 nodes|—|
|cluster|||
|Number of storage pools|1|—|
|Number of containers|1|You can only use one Nutanix|
|||container if you use NDB.|
|Container replication factor|2|Use replication factor 3 if|
|||required to meet your SLA.|
|Container configuration|Inline compression|Recommendations|
|||in the database|
|||engine best practices|
|||documents supersede this|
|||recommendation. Don’t enable|
|||compression if the database|
|||data isn’t compressible.|



© 2026 Nutanix, Inc. All rights reserved  | **45** 

Microsoft SQL Server Database Workloads on Nutanix 

|**Component**|**Configuration**<br>**Comment**|
|---|---|
|Nutanix cluster encryption<br>vCPU-to-pCPU ratio|N/A<br>Encrypt the cluster based on<br>customer requirements.<br>1:1 for business-critical<br>applications with maximum<br>performance; 2:1 or 4:1 for<br>generic database server<br>workload; 4:1 or 6:1 for<br>development, stage, test, and<br>QA environments<br>These ratios represent<br>common configurations;<br>configure your environment’s<br>ratios based on your or<br>vendor's requirements.<br>Use the same ratio across<br>environments if development,<br>test, stage, and QA require<br>the same performance as the<br>production workloads.|



The Virtual Hardware Design section of the Microsoft SQL Server on Nutanix Best Practices guide includes the configurations required by Microsoft SQL Server and the OS. 

## **Licensing** 

You can choose from licensing levels for Nutanix software, including Starter, Pro, and Ultimate. Verify which license you need for the features and functionalities included in your solution. For an overview of the licenses and options available on each, see Nutanix Cloud Platform Software Options. 

Consult the appropriate vendor and their licensing guides to ensure compliance with database engine licensing requirements. Database licensing is outside of Nutanix responsibility. 

© 2026 Nutanix, Inc. All rights reserved  | **46** 

Microsoft SQL Server Database Workloads on Nutanix 

## 8. References 

- Nutanix Database Service Documentation 

- Database Migration 

- Nutanix Maximum System Values 

- Nutanix AHV Networking Best Practices 

- Nutanix Volumes Best Practices 

- Nutanix Cloud Platform Software Options 

- Microsoft SQL Server on Nutanix Best Practices 

© 2026 Nutanix, Inc. All rights reserved  | **47** 

Microsoft SQL Server Database Workloads on Nutanix 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **48** 

Microsoft SQL Server Database Workloads on Nutanix 

## **List of Figures** 

Figure 1: Nutanix Hybrid Cloud On-Premises Architecture........................................................................... 8 Figure 2: Nutanix Hyperconverged Infrastructure Overview.......................................................................... 9 Figure 3: Overview of Nutanix Physical Layer.............................................................................................10 Figure 4: SQL Server vDisk Layout.............................................................................................................17 Figure 5: AHV Host Network Physical Configuration...................................................................................25 Figure 6: SQL Server Physical Client-Server Design.................................................................................. 28 Figure 7: SQL Server SUT with Storage View............................................................................................ 29 Figure 8: Single SQL Server VM, AOS 6.5 vs. AOS 6.10...........................................................................30 Figure 9: Single SQL Server VM, G8 vs. G9 on AOS 6.10.........................................................................31 Figure 10: VM Scale-Out Comparison, AOS 6.5 vs. AOS 6.10...................................................................31 Figure 11: VM Scale-Out Comparison, G8 vs. G9 on AOS 6.10.................................................................32 Figure 12: SQL Server Memory Scale-Up Comparison, AOS 6.5 vs. AOS 6.10.........................................33 Figure 13: SQL Server Memory Scale-Up Comparison, G8 vs. G9 on AOS 6.10.......................................33 Figure 14: SQL Server vCPU Scale-Up Comparison, AOS 6.5 vs. AOS 6.10............................................ 34 Figure 15: SQL Server vCPU Scale-Up Comparison, G8 vs. G9 on AOS 6.10.......................................... 35 Figure 16: SQL Server AG Benchmark Design...........................................................................................36 Figure 17: SQL Server Always On Availability Group Synchronous Comparison, AOS 6.5 vs. AOS 6.10...37 Figure 18: SQL Server Always On Availability Group Synchronous Comparison, G8 vs. G9 on AOS 6.10.38 Figure 19: SQL Server Always On Availability Group Asynchronous Comparison, AOS 6.5 vs. AOS 6.10.38 Figure 20: SQL Server Always On Availability Group Asynchronous Comparison, G8 vs. G9 on AOS 6.10.......................................................................................................................................................... 39 Figure 21: SQL Server Always On Scale-Out Tests Comparison, AOS 6.5 vs. 6.10...................................40 Figure 22: SQL Server Always On Scale-Out Comparison, G8 vs. G9 on AOS 6.10................................. 40 


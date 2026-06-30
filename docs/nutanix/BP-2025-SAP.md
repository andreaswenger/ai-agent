# **SAP on Nutanix Best Practices** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. Nutanix, Inc. is not affiliated with VMware by Broadcom or Broadcom. VMware and the VMware product names recited herein are registered or unregistered trademarks of Broadcom in the United States and/ or other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

SAP on Nutanix Best Practices 

## **Contents** 

**1. Executive Summary.................................................................................4 2. SAP on Nutanix Overview.......................................................................6** Benefits of Running SAP on Nutanix AHV.........................................................................................7 **3. SAP on Nutanix Best Practices............................................................10 4. Platform Guidelines for SAP on Nutanix.............................................20 5. SAP on Nutanix Application Layer Design......................................... 23 6. Database Best Practices for SAP on Nutanix.....................................25 7. References and Resources...................................................................30 About Nutanix.............................................................................................32** 

SAP on Nutanix Best Practices 

## 1. Executive Summary 

Nutanix delivers the performance, scalability, and availability that SAP Basis and database administrators require for business-critical transactional and analytical workloads while reducing management and operational complexity. As an SAP Global Technology Partner, Nutanix provides SAP users with many advantages: 

- Localized I/O and flash for index and key database files, enabling low-latency operations 

- Infrastructure consolidation that can eliminate underused application silos and consolidate multiple workloads onto a single, efficient platform 

- Nondisruptive upgrades and scalability, including one-click node addition without system downtime 

- Nutanix VM-level data protection and disaster recovery to automate backups 

- Unrivaled operational simplicity; no complicated configuration, provisioning, or mapping with disks, RAID, and LUNs 

- Rapid one-click cloning of SAP VMs with no wasted storage capacity from the operation 

- One-click hypervisor conversion for greater flexibility and lower total cost of ownership 

- A turnkey validated framework that dramatically reduces the time to deploy your SAP applications 

- Mission-critical availability with a self-healing foundation, VM-centric data protection, and support for enterprise backup solutions 

- Flexibility to choose from SAP-supported hypervisors, such as the Nutanix AHV software 

- Reduced total cost of ownership from infrastructure right-sized for your SAP workload 

Deploying and operating SAP applications in your environment is complex. If you're unsure about the details of your Nutanix infrastructure, contact Nutanix Services. 

© 2026 Nutanix, Inc. All rights reserved  | **4** 

SAP on Nutanix Best Practices 

**Note:** Access to the SAP Knowledge Base is required to access some of the SAP notes linked in this document. 

Key topics: 

- Overview of the Nutanix solution for delivering SAP on AHV, VMware vSphere, and Microsoft Hyper-V 

- Overview of the Nutanix solution for delivering SAP HANA 

- Benefits of SAP on Nutanix, and AHV in particular 

- Best practices for SAP on AHV, vSphere, and Hyper-V on Nutanix 

- Sizing guidance for scaling SAP deployments on Nutanix 

- Design and configuration considerations when designing and building SAP on Nutanix distributed storage 

## _Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|September 2017|Original publication.|
|2.0|January 2021|Major updates throughout.|
|2.1|April 2022|Minor updates throughout.|
|2.2|January 2024|Added content from the SAP|
|||on Nutanix tech note.|
|2.3|February 2026|Updated document structure.|



© 2026 Nutanix, Inc. All rights reserved  | **5** 

SAP on Nutanix Best Practices 

## 2. SAP on Nutanix Overview 

Most organizations use SAP software to deliver business-critical services to their employees, partners, or customers, so it's vital to run SAP software on supported platforms. SAP selected Nutanix as a Global Technology Partner and certified Nutanix Cloud Platform for SAP NetWeaver applications and relational databases. Customers and partners have the choice between AHV and ESXi as a hypervisor. 

At the heart of SAP solutions is the SAP Enterprise Resource Planning (ERP) application, which is complemented by SAP Customer Relationship Management (CRM), SAP Supplier Relationship Management (SRM), SAP Product Lifecycle Management (PLM), and SAP Supply Chain Management (SCM), among other products. SAP now delivers this comprehensive suite of modules in a simplified form called S/4. For traditional SAP systems, the back-end databases can be SAP HANA, SAP Sybase ASE, Oracle Database, or Microsoft SQL Server. However, for SAP S/4, the back-end database must be SAP HANA. 

SAP HANA has specific version restrictions for the hypervisor and AOS Storage software. For more information, see the appropriate hypervisor-specific SAP HANA document: 

- SAP HANA on Nutanix AHV Best Practices 

- SAP HANA on VMware vSphere Best Practices 

With SAP on Nutanix, you can deploy any application mix at any scale, all on a single platform. The Nutanix Prism solution offers an efficient and elegant management interface that enables uncompromising simplicity with one-click infrastructure management, remediation, and operational insights. You can run SAP application servers and database VM workloads simultaneously while isolating databases on dedicated hosts for licensing purposes. Nutanix offers high IOPS and low latency, which means that database computing and storage requirements drive deployment density, rather than concerns about I/O or resource bottlenecks. Nutanix distributed storage can easily handle the throughput and transaction requirements of the most demanding transactional and analytical databases. Our testing shows that it's better to increase the number of database VMs on the Nutanix platform to take full advantage of its performance 

© 2026 Nutanix, Inc. All rights reserved  | **6** 

SAP on Nutanix Best Practices 

capabilities than to scale large numbers of database instances or schemas in a single VM. 

For general support and configuration information for running SAP workloads on Nutanix, see SAP note 2428012 (SAP account required). For more information on support for SAP on Linux in virtualized environments, see SAP note 1122387 (SAP account required). 

## **Benefits of Running SAP on Nutanix AHV** 

Nutanix is the only hyperconverged platform certified for SAP end to end, which means that support covers not only AHV, the distributed storage fabric, and hardware, but the entire infrastructure stack. Compute, storage, and virtualization are all fully certified and supported for SAP workloads. 

For most businesses, achieving optimal IT infrastructure isn't a goal; the purpose of infrastructure is to enable the business. The invisible Nutanix infrastructure, which includes AHV, elevates IT staff from infrastructure work, enabling them to use their talents and creativity to make the business more competitive, efficient, and profitable. AHV also reduces legacy hypervisor costs, such as those associated with software licensing, ongoing administration, business disruption, and hardware. 

## **Support** 

Nutanix Support boasts an industry-leading 90+ Net Promoter Score and supports business-critical applications such as Oracle, SQL, and Exchange. Nutanix Support arranges and actively participates in conference calls between vendors until issues are resolved. 

## **Simplicity** 

Deploying SAP on AHV is user-friendly and fast, saving hours on complicated performance management policies or storage and VM design. AHV is an enterprise-grade hypervisor that is high-performing and consistent to use. Additionally, every Nutanix node includes Prism, an elegant and intuitive management interface. Built-in guidance and self-healing remove the burden of constantly tuning the environment. With a few clicks, you can upgrade your entire virtualization environment—including the OS, firmware, and hypervisor—regardless 

© 2026 Nutanix, Inc. All rights reserved  | **7** 

SAP on Nutanix Best Practices 

of geography. Nutanix offers comparable ease of use when using ESXi to run SAP workloads. 

## **Scalability** 

Standalone hypervisor management solutions require a scale-up architecture. Disparate scale-up management methods also require considerable design effort to eliminate single points of failure and minimize downtime risk while allowing maximum agility. This approach contrasts with the scale-out capability that virtualization natively enables for the compute layer. Nutanix consumer-grade design means that users can deploy applications within hours of receiving Nutanix nodes. Adding nodes to the cluster is quick and intuitive as well, with automated self-discovery and one-click automatic host configuration using existing policies. 

## **Security** 

Conventional hypervisors must interact with hardware and software products from many manufacturers, and security vulnerabilities emerge where the products intersect. In contrast, we tuned, tested, and hardened AHV exclusively for Nutanix hyperconverged infrastructure. This approach yields a product with a dramatically reduced security area. The security baseline can help you cover the full stack, and you can use comprehensive Nutanix analytics capabilities to help ensure that your platform is compliant. With Nutanix, you can protect your SAP applications under an industry-leading secure platform ecosystem. 

## **Resilience** 

AHV virtualization, like the Nutanix hyperconverged data plane, is scale-out, self-healing, and highly available out of the box. Because Prism is part of every node, the management capabilities are highly redundant; they continue even if a node fails. Because Nutanix has a fully integrated stack, it doesn't experience incompatibility failures, thus reducing the risk of downtime. Nondisruptive, rolling one-click upgrades also ensure continuous uptime. 

## **Analytics** 

AHV doesn't require additional software licensing to extract analytics from the environment, and you don't need to load the data into a separate database. Analytics are built into Prism and incorporate not only virtualization but also storage and compute, providing visibility across the entire stack. Nutanix achieves datadriven efficiency with extensive automation and rich, system-wide monitoring, 

© 2026 Nutanix, Inc. All rights reserved  | **8** 

SAP on Nutanix Best Practices 

combined with REST-based programmatic interfaces that integrate with datacenter management tools. AHV feeds all system, audit, intrusion detection, and selfremediation logs to the central log host, which allows real-time situational awareness during forensic support and root-cause analysis. 

For more information on the benefits of Nutanix Cloud Platform, see Nutanix Business Value. 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

SAP on Nutanix Best Practices 

## 3. SAP on Nutanix Best Practices 

For SAP on Nutanix, the majority of best practice configurations and optimizations occur at the database and OS levels, rather than on Nutanix Cloud Platform software. 

**Note:** You must have SAP Support Portal credentials to access SAP notes. 

## General best practices: 

- Follow the guidelines in these SAP notes: 

   - › 2428012: SAP on Nutanix 

   - › 2686722: SAP HANA virtualized on Nutanix AOS 

   - › 1056052: Windows: VMware vSphere configuration guidelines 

   - › 1122388: Linux: VMware vSphere configuration guidelines 

   - › 1246467: Hyper-V configuration guidelines 

- Use one of the database vendors supported by Nutanix and SAP, such as Sybase ASE, SQL Server (VMware and Hyper-V only), Oracle, and IBM Db2. 

- When using SAP HANA as a database, follow the detailed guidance provided in the following best practice documents: 

   - › SAP HANA on Nutanix AHV Best Practices 

   - › SAP HANA on VMware vSphere Best Practices 

- For SAP-related issues, open an SAP support ticket. 

   - › If SAP support determines that the problem is related to the hypervisor, they assign the ticket to the respective vendor. 

   - › You can also log a support call with Nutanix, VMware, or Microsoft support for hypervisor issues. 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

SAP on Nutanix Best Practices 

- For VMware, you can open an SAP support ticket under the following categories: 

   - › BC-OP-NT-ESX (Windows on VMware ESX) 

   - › BC-OP-LNX-ESX (Linux on VMware ESX) 

- When using AHV as the hypervisor, apply the guidance from SAP note 2656072: Steps required to run virtualized SAP applications on Nutanix AHV to allow for correct metrics collection. 

- Apply the guidance from SAP note 1409604: Virtualization on Windows: Enhanced monitoring or SAP note 1102124: SAPOSCOL on Linux: Enhanced function. 

The guidance in these notes enables you to view VMware performance counters in the following transactions: ST06: SAP NetWeaver 7.2 or later; OS07: SAP NetWeaver 7.01, 7.02, 7.1, and 7.11; and OS07N: SAP NetWeaver 6.40 and 7.0. These actions are necessary to obtain SAP support, and they require VMware configuration to perform the following operations: 

   - › Activate the host accessory functions on ESXi host (set Misc.GuestLibAllowHostInfo). 

   - › Activate the accessory functions for the VM (set tools.guestlib.enableHostInfo). 

- Use the latest processor generations, which include hardware-assisted CPU virtualization, memory management unit (MMU) virtualization, and I/O MMU virtualization features, to assist virtualization and improve performance. 

Intel Turbo Boost improves performance for single-threaded applications and processes. 

- After sizing VMs with the memory and virtual CPUs required for the workload, administer SAP application instances in VMs in the same way that you administer them with physical infrastructure; the standard SAP administration tasks and procedures apply. 

- Follow parameter settings for SAP application servers as specified in SAP notes on VMware per physical deployments. 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

SAP on Nutanix Best Practices 

- For performance best practices specific to vSphere, see the appropriate guide: 

   - › Performance Best Practices for VMware vSphere 6.7 

   - › Performance Best Practices for VMware vSphere 6.7 Update 2 

   - › Performance Best Practices for VMware vSphere 7.0 

Guest OS best practices: 

- For AHV, only use the Linux operating systems that SAP lists on the Product Availability Matrix (PAM) (SAP Support account required). 

**Note:** Windows OS isn't supported on AHV. 

- Install the latest version of VMware Tools in the guest operating system. 

- Minimize VM time drift by following the guidelines in SAP note 989963: Linux: VMware timing problem and VMware KB article 1006427. 

- From Linux 2.6 onward, set the Linux kernel I/O scheduler to `NOOP` . 

- You can continue to use the Linux OS Logical Volume Manager (LVM) to manage disks for convenience and flexibility, but use the correct extent and stripe based on your database requirements. 

- Disable read-ahead for database LVM logical volumes. 

CPU best practices: 

- Enable hyperthreading if it is available. 

Hyperthreading might provide a reduced benefit for CPU-intensive batch jobs compared to OLTP workloads. For more information on this topic, see SAP note 1612283: Hardware Configuration Standards and Guidance. 

- Assign a minimum of 8 GB per provisioned vCPU, fully reserved. 

This minimum assignment applies to both database and application servers. However, database servers typically need more memory depending on requirements and testing results. 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

SAP on Nutanix Best Practices 

- Configure virtual nonuniform memory access (vNUMA) sockets for wide VMs that must cross NUMA nodes (for example, database VMs). 

A VM is wide when it has more vCPUs than the NUMA node. NUMA is a design method that allows microprocessors in a multiprocessing system to share memory locally, improving performance and scalability. Local memory access times on NUMA-based systems are many times faster than remote because the memory controller is directly connected to one processor. 

- The number of vSockets configured and available directly impacts the likelihood that access occurs locally rather than remotely (fewer vSockets means higher likelihood of local access). 

- Because ESXi and Hyper-V can provide NUMA-aware scheduling, VM configurations affect OS behavior. 

- If workload monitoring shows that the SAP application isn't using all the vCPUs, the extra vCPUs might cause scheduling constraints, especially under high workload, so minimize the number of vCPUs in the VM. 

- In typical deployments, don't set CPU reservations. 

SAP tests on vCPU overcommit show graceful degradation in performance, which you can overcome by rebalancing the workloads across an ESXi cluster (using VMware vSphere vMotion). 

**Note:** This solution assumes spare CPU capacity in the cluster. 

- When configuring SAP VMs, see SAP note 1612283: Hardware Configuration Standards and Guidance, which recommends sizing SAP VMs within a NUMA node boundary, if possible. 

- Consider NUMA boundaries before hot-adding vCPUs, as this action can disable vNUMA. 

The effect of hot-adding vCPUs is particularly relevant to database VMs, which can be NUMA-wide. Generally, you size databases using vCPUs that can handle peak workloads with an additional buffer, so hot-add might not be an urgent use case. However, if you need to hot-add vCPUs beyond a NUMA boundary, the loss in vNUMA benefits depends on the workload and NUMA optimization algorithms specific to the database vendor and version of 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

SAP on Nutanix Best Practices 

VMware vSphere. VMware recommends determining the NUMA optimization benefits based on your workload before setting the hot-add vCPU function— this analysis allows you to decide if the performance tradeoff is warranted. For more information, see VMware KB article 2040375. 

Memory best practices: 

- Follow service-level agreements (SLAs) for memory reservations; for production systems under strict performance, SLAs set memory reservations equal to the VM size (VMware and Hyper-V only). 

- Don't use memory oversubscription for any productive SAP workload. 

SAP doesn't support memory oversubscription for any productive SAP workload, so Nutanix doesn't support using it on any supported hypervisor. 

- Follow SAP guidelines for using large pages. 

   - › VMware vSphere enables large page support by default, and Linux and Windows support large pages. 

   - › Using large pages can potentially increase translation lookaside buffer (TLB) access efficiency and improve program performance. 

   - › Large pages can cause memory to be allocated to a VM more quickly. 

   - › Some SAP applications don't support large pages, whether those applications are running natively or inside a VM. 

      - Not supported for the Advanced Business Application Programming (ABAP) stack (see SAP note 1312995: Enabling Large Page Support for the View Memory Model) 

      - Supported for NetWeaver Java on Linux (see SAP note 1681501: Configure an SAP JVM to use large pages on Linux) 

      - Not supported for Java in Business Objects 

- Follow the same SAP notes for a physical deployment to configure the size of the OS swap space in the VM. 

This guideline is independent of the hypervisor solution used. SAP or SAP Support must address recommendations on OS swap sizing for SAP. 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

SAP on Nutanix Best Practices 

- Set memory reservations. 

   - › If you don't set memory reservations, VMware vSphere Distributed Resource Scheduler (DRS) load balancing recommendations might be suboptimal for SAP systems with large memory requirements. 

   - › AHV reserves all memory per VM by default, so it doesn't need special consideration. 

Storage best practices: 

- Use one of the common storage protocols supported by Nutanix, such as NFS and iSCSI. 

Each of these storage protocols achieves acceptable performance. 

**Note:** Use iSCSI in-guest storage where you require Windows Failover Clustering. 

- Use standard VMDK on a standard container or datastore for VMware. 

- Spread database files across multiple vDisks. 

   - › Separate logs from data in separate vDisks. 

   - › Assign the database log vDisk to a separate PVSCSI adapter. 

- Don't create multiple storage pools or containers in a Nutanix environment because separation doesn't affect performance. 

A single storage container is sufficient for your environment; any additional containers are only required for management purposes. 

- Use thin disks; using eager-zeroed thick disks over thin disks provides no performance advantage in a Nutanix environment. 

**Note:** Oracle RAC requires eager-zeroed thick disks in order to enable Clusterware features. 

- Enable inline compression (delay=0) in a Nutanix storage container for improved SAP workload performance and space savings. 

Depending on the database features you're using, you might see no space savings from the compression applied. 

- Disable deduplication and erasure coding for SAP environments. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

SAP on Nutanix Best Practices 

- For I/O-intensive SAP workloads in VMware, you can increase the queue depth to increase performance. 

Newer versions of ESXi increase the queue depth by default, so you don't need to make additional changes. 

- In VMware, disable Storage I/O Control and Storage DRS, which offer no benefit in a Nutanix environment. 

Nutanix has built-in noisy neighbor prevention due to the web-scale design and storage controller on each Nutanix node. 

- Use Nutanix snapshots to create crash-consistent, point-in-time copies of systems to avoid VM stun or pause behavior in the hypervisor. 

**Note:** Local snapshots aren't backups. 

- Use a single Nutanix container, which is represented as a VMware datastore. 

- Choose the hardware model based on compute, storage, and licensing requirements: 

   - › Ideally, keep the working set in SSD and the database size within node capacity. 

A working set refers to the amount of data that a process or workflow uses in a given period, or commonly accessed (hot) data in the overall persistent storage capacity. The concept of the working set implies that some data is used actively while the remainder is at rest. 

   - › For SAP HANA, use an all-flash or all-flash NVMe cluster. 

   - › Select a model that can fit the entire database on a single node. For databases that are too large to fit on a single node, ensure ample bandwidth between nodes. 

   - › Use higher-memory node models for I/O-heavy workloads. 

   - › Use a node that has twice the memory size of the largest single VM. 

   - › Use a node that fits your organization's licensing constraints. 

- Keep Nutanix Controller VMs (CVMs) in the vSphere Cluster Root rather than in a secondary resource pool. 

- Use a Nutanix all-flash node for your database workloads if requirements make the data-tiering model unsuitable. 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

SAP on Nutanix Best Practices 

- Don't perform a VMware snapshot delete operation while a batch job is running because the delete operation could cause the batch job to stop. 

When you delete a VMware snapshot (for example, during backup operations for a VM running a database in a three-tier setup), the VM might be stunned for a period. The stun can cause application server disconnections, but SAP application servers are configured to automatically reconnect. For more information, see SAP note 98051: Database Reconnect: Architecture and function. 

- Size the SSD tier to hold the application working sets for optimal performance in a Nutanix environment. 

To estimate the size of a database server working set (active data), one general guideline is to multiply the peak monthly growth of the database by three. The result is a useful data point that works well in the field for existing customers. However, this estimate might not work for all SAP deployments, as certain SAP systems have more read-heavy or write-heavy requirements based on the business the system supports. SAP also provides useful information for calculating working set size through a program called the SAP EarlyWatch Alert. 

## Networking best practices: 

- On VMware, use the VMXNET family of paravirtualized network adapters. 

The paravirtualized network adapters in the VMXNET family implement an optimized network interface that passes network traffic between the VM and the physical network interface cards with minimal overhead. 

- On Windows, enable receive-side scaling (RSS) if available. 

- On VMware, use E1000E for legacy SAP applications on older operating systems only. 

- For SAP HANA workloads with strict latency requirements, use NICs and physical switches with remote direct memory access (RDMA) enabled. 

Sizing and architecture best practices: 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

SAP on Nutanix Best Practices 

- As with physical deployments, obtain the SAPS (SAP Application Performance Standard) rating of the SAP system, which SAP publishes on its benchmarks page. 

The SAPS rating for a VM depends on the core you schedule the vCPUs on. The SAPS rating per core, in turn, depends on the server and CPU specifications. 

- Scale out application servers in VMs that fit within a NUMA node (see the NUMA guidelines in the CPU best practices). 

- Use multiple smaller application servers instead of one large application server (see SAP note 9942). 

This best practice reduces CPU context switching between work processes and generates less overhead. It's also better to have one VM per application server to manage workload distribution and resilience. 

- Add an extra 10 percent to the SAPS calculations for virtualization and hypervisor overhead. 

This buffer ensures that compute resources have room to compensate for any virtualization overhead. 

- For new and existing SAP environments, follow SAP's standard application sizing guidance (SAP account required). 

- Use best practice guidance from Nutanix, VMware, and Microsoft to size VM configurations. 

- Follow SAP best practice documentation and SAP Notes for specific information about how to size and configure the virtualized environment for the chosen hypervisor (AHV, ESXi, or Hyper-V). 

- Design Nutanix clusters based on high availability and database licensing requirements. 

- Observe the design and configuration recommendations general to Nutanix storage while also accounting for SAP workload characteristics such as capacity, performance, and redundancy requirements. 

- Don't overcommit CPU for production environments. 

© 2026 Nutanix, Inc. All rights reserved  | **18** 

SAP on Nutanix Best Practices 

- Don't overcommit memory for any environment. 

- When designing around dependencies, don't ignore network design architecture. 

Transactional systems rely on low response times, while analytical systems emphasize I/O throughput. 

For SAP HANA, sizing involves additional considerations: 

- Always deploy SAP HANA on certified original equipment manufacturer (OEM) hardware. 

Refer to SAP's HCI certification matrix for approved solutions. Contact the manufacturer for certified hardware and certified installation partners for your HANA systems. This step is critical to ensure proper support. 

- Account for the limitations on CPU sockets, memory ratio, and production VMs per host as stated in SAP Note 2686722: SAP HANA virtualized on Nutanix AOS. 

- Don't expect the CPU socket assigned to the Nutanix CVM to run any HANA production workloads. 

Other workloads can run on that socket if enough resources are available. 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

SAP on Nutanix Best Practices 

## 4. Platform Guidelines for SAP on Nutanix 

Nutanix certified and provides support for SAP workloads on all Nutanix platform types. However, some node types are better suited for the server types or VMs that compose an SAP implementation. 

For SAP application servers, hybrid NX-3000 and NX-8000 series are both good choices, depending on SAP requirements and sizing. These series also work well for small and medium-sized database servers. For large database servers, we recommend using NX-8000 because of its enhanced cold-tier performance and overall optimization for database workloads. Nutanix offers all-flash (SSD) options in both platforms for databases that require very high performance and consistent latency across very large active datasets. You can also configure mixed all-flash and hybrid clusters to efficiently run application servers alongside databases with high performance demands. 

For SAP HANA, work with certified original equipment manufacturer (OEM) partners to procure the right servers and services for installation and support. 

Nutanix, VMware, and Microsoft support and have certified Nutanix for AHV, ESXi, and Hyper-V, respectively. However, SAP doesn't support running SAP on Windows guest operating systems on AHV or Linux guests on Hyper-V (SAP Note 1122387). SAP released hypervisor- and OS-specific configuration guidance as SAP Notes to support virtualized SAP systems. These notes are available on SAP's support portal home page, but you must have a valid SAP Service Marketplace (SMP) account to access the content. Consider the recommendations in the SAP Notes carefully to avoid operational issues. 

When deploying SAP applications on AHV, see the following SAP Notes: 

- Note 2428012: SAP on Nutanix 

- Note 2686722: SAP HANA Virtualized on Nutanix AOS 

- Note 2656072: Steps Required to Run Virtualized SAP Applications on Nutanix AHV 

When deploying SAP applications on ESXi, see the following SAP Notes: 

- Note 1492000: General Support Statement for Virtual Environments 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

SAP on Nutanix Best Practices 

- Note 2161991: VMware vSphere Configuration Guidelines 

- Note 1501701: Single Computing Unit Performance and Sizing 

- Note 1482272: Key Monitoring Metrics for SAP on VMware vSphere (up to version 5.5 u2) 

- Note 2266266: Key Monitoring Metrics for SAP on VMware vSphere (version 5.5 u3 and higher) 

- Note 1173954: Support of Oracle for VMware (see also VMware's white paper on Oracle certification, support, and licensing) 

When deploying SAP applications on Hyper-V, see the following SAP Notes: 

- Note 1492000: General Support Statement for Virtual Environments 

- Note 1246467: Hyper-V Configuration Guideline 

- Note 1570141: Key Monitoring Metrics for SAP on Hyper-V 

- Note 1329848: Oracle Support for Microsoft Hyper-V 

When deploying SAP applications using Linux, see the following SAP Notes: 

- Note 1122387: Linux: SAP Support in Virtualized Environments 

- Note 171356: SAP Software on Linux: General Information 

- Note 540787: CPU Affinity and CPU Priority under Linux 

- Note 989963: Linux: VMware Timing Problem 

- Note 1552925: Linux: High Availability Cluster Solutions 

- Note 1681501: Configure an SAP JVM to Use Large Pages on Linux 

- Note 724140: Extended Memory, Release for Operating System 

- Note 1102124: SAPOSCOL on Linux: Enhanced Function 

When deploying SAP applications using Windows, see the following SAP Notes: 

- Note 1409608: Virtualization on Windows 

- Note 1409604: Virtualization on Windows: Enhanced Monitoring 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

SAP on Nutanix Best Practices 

- Note 1612283: Hardware Configuration Standards and Guidance 

- Note 1374671: High Availability in Virtual Environment on Windows 

- Note 1678705: Installation Scenarios for a Standalone ASCS Instance 

- Note 1580509: Windows Editions Supported by SAP 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

SAP on Nutanix Best Practices 

## 5. SAP on Nutanix Application Layer Design 

To benefit fully from virtualizing an SAP deployment, we recommend a three-tier architecture over a two-tier architecture. Three-tier implementations provide operational efficiency and help you increase security, resilience, and scalability. You can deploy separate application servers as needed with little manual effort, and SAP mechanisms efficiently distribute the user load across the application servers. 

Because application servers and database servers have completely different resource consumption patterns, deploying them separately improves operations. With a three-tier implementation, administrators can move database or application VMs to faster hardware without any downtime using live migration, and administrators can manage these VMs independently. This approach helps create a more resilient design. 

Three-tier implementations make it easier to monitor resource usage because they eliminate guesswork during resource contention. Two-tier architectures might appear easier to deploy, but they can be much harder to operate and tune later. Additionally, the isolated access patterns in three-tier implementations can help you ensure better security. Users only access the application servers, and only application servers and administrators have access to the database servers. 

Regarding application server sizing, we recommend using smaller application servers with lower vCPU counts to avoid the following disadvantages: 

- Larger instances allow for more work processes per instance, which can cause work process context switching and result in higher overheads. 

- Bottlenecks might occur on shared resources within one instance, including in the dispatcher, the gateway, network connections, and buffers. 

- Problems occurring within an instance can affect a greater number of sessions. 

- Larger instances experience decreased handling during monitoring and error analysis —for example, larger instances require more time to analyze user traces and performance statistics. 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

SAP on Nutanix Best Practices 

- Operating systems can have problems handling more requests, especially around the number of connections and file handlers, which can cause performance issues. 

For example, we recommend using four VMs as four application servers with 4 vCPUs each, rather than one application server with 16 vCPUs. The latter ultimately uses the same resources, but splitting up the application servers delivers better performance and resilience. For information on using smaller and more agile application server or dialog instance VMs, see SAP Note 9942. 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

SAP on Nutanix Best Practices 

## 6. Database Best Practices for SAP on Nutanix 

When choosing a database to run with SAP on Nutanix, consider the following points on database licensing: 

- Database VMs can be wide, depending on the sizing requirements. 

- If the SAP user license covers the database license at runtime, you can run the application server and database server machines in the same cluster without any database licensing impact. 

- If you obtain the database license separately from the database vendor (original equipment manufacturer (OEM)), depending on the database vendor's virtual licensing policies, you can maximize your return on investment on database licensing costs with a dedicated cluster for database VMs, or you can restrict database VMs to dedicated hosts using antiaffinity policies. 

For more information on how to use effective database licensing with Nutanix, contact your Nutanix account manager. 

- If you purchase the database license from the vendor (such as Oracle), you must license the cores of all hosts running Oracle, after which you can run an unlimited number of Oracle VMs on those hosts. 

**Note:** Oracle doesn't support partially licensed hosts. 

While general SAP workloads on Nutanix have flexible options for hardware, hypervisor, and guest platforms, the technical requirements for HANA, based on SAP's guidelines, are more specific. 

You must fulfill two key requirements to run SAP HANA on Nutanix: 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

SAP on Nutanix Best Practices 

- Use SAP HANA on Nutanix with one of the specific certified models of Lenovo HX, DELL XC, HPE DX, and Fujitsu XF hardware solutions. 

The hardware's original equipment manufacturer (OEM) provides and supports the certified solution for you. 

- Use AHV or ESXi. 

The Nutanix platform for SAP HANA offers the following advantages: 

- A single interface (Nutanix Prism) for managing all your infrastructure and applications, including VMs running HANA databases 

- Simplified architecture for a true on-premises cloud landscape for your mission-critical applications 

- The latest proven technology with a turnkey solution stack that's fully certified and supported out of the box 

- Integrated support for the entire stack 

- Shorter time to deliver cloud services for mission-critical applications (including SAP HANA) 

For more information on supported configurations for SAP HANA on Nutanix, see SAP note 2686722: SAP HANA virtualized on Nutanix AOS. 

See the following resources for Sybase Adaptive Server Enterprise (ASE): 

- See SAP note 1706801: SYB: Sybase ASE released for virtual systems. 

- Follow the guidelines in SAP Sybase Adaptive Server Enterprise on VMware vSphere: Essential Deployment Tips. 

- See the guidelines for Sybase ASE in Architectural Guidelines and Best Practices for Deployments of SAP HANA on VMware vSphere. 

See the following resources and best practices for SAP on Oracle: 

- See SAP note 1173954: Support of Oracle for VMware. 

- For an overview of Oracle support and licensing on VMware, see VMware's Oracle virtualization resources. 

- See the latest VMware Oracle Support Policy. 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

SAP on Nutanix Best Practices 

- For recommendations concerning maximizing I/O performance that also apply to virtual environments, see SAP note 793113: FAQ: Oracle I/O configuration. 

- SAP supports Oracle Data Guard. See SAP note 105047: Support for Oracle functions in the SAP environment. 

- For information on Oracle performance on SAP, see SAP note 618868: FAQ: Oracle performance. 

- If you suspect slow I/O performance, check the vSphere I/O latency metrics. 

- Set huge pages following the guidelines from SAP note 1672954. 

- SAP supports Oracle RAC. See SAP note 527843: Oracle RAC support in the SAP environment. 

   - › VMware supports RAC configurations. 

   - › For more information, see Oracle on Nutanix Best Practices. 

- For a consistent online backup of the Oracle database, the following options are available: 

   - › Use the Oracle Recovery Manager (RMAN) utility (this option doesn't require a storage-based snapshot). 

   - › Place the Oracle database in backup mode, then take a snapshot (VMware or storage array level). 

- For Oracle on Windows, you can use Microsoft Volume Shadow Copy Service (VSS) to perform online database backup, which is database consistent (see Performing Database Backup and Recovery with VSS). 

**Note:** Oracle Automatic Storage Management (ASM) doesn't support this process. 

See the following resources and best practices for SAP on SQL Server: 

© 2026 Nutanix, Inc. All rights reserved  | **27** 

SAP on Nutanix Best Practices 

- SQL Server is supported on VMware with four options: 

   - › Microsoft Premier contract 

   - › Microsoft Server Virtualization Validation Program(Windows Server 2008 and later) 

   - › Server original equipment manufacturer (OEM) 

   - › VMware Global Support Services (GSS) and TSANet. 

- For supported configurations, see the guidelines on Microsoft clustering with vSphere. 

   - › For Nutanix, Microsoft clustering with iSCSI support with persistent SCSI reservation requires at least AOS 5.10. 

- SAP supports databases protected by SQL Server Always On (see SAP note 1772688: SQL Server Alway sOn and SAP applications). 

   - › Always On minimizes recovery time objectives (RTOs) and recovery point objectives (RPOs) in case of primary SQL Server failure. For more information, see Running SAP Applications on the Microsoft Platform. 

   - › The process for configuring Always On in VMs is comparable to the process for physical deployments. 

- Place data and log files in separate vDisks, preferably connected through separate SCSI adapters. 

- Review the Microsoft guidance around SAP systems on SQL Server in the Microsoft Tech Community article Best Practices for Maintaining SQL Server SAP Systems. 

- For SQL Server configuration parameters, follow the SAP notes regarding physical instances: 

   - › 1237682: Configuration Parameters for SQL Server 2008 

   - › 1702408: Configuration Parameters for SQL Server 2012 

   - › 2779607: Configuration Parameters for SQL Server 2019 

   - › These notes indicate how much extra memory the OS requires beyond SQL Server. For memory sizing, treat the VM as if it were a physical server and calculate memory allocation in the VM accordingly. 

© 2026 Nutanix, Inc. All rights reserved  | **28** 

SAP on Nutanix Best Practices 

- For instructions on how to investigate SQL Server I/O performance in the guest OS, see SAP note 987961: FAQ: SQL Server I/O performance. 

- If you suspect slow I/O performance, check the vSphere I/O latency metrics. 

- For backup considerations for SAP SQL Server databases, see SAP note 1878886: Backup Strategies for SQL Server. 

**Note:** Don't use simple recovery mode for SAP databases. 

- Use Nutanix solutions that integrate with Microsoft VSS (for database consistency) for environments with high restore service level agreements (SLAs)—these solutions work on VMware much like they do on physical infrastructure. 

- Most of the recommendations provided in Microsoft SQL Server on Nutanix Best Practices are also valid for SAP environments. 

See the following resources and best practices for SAP on Db2: 

- IBM supports Db2 on VMware (see SAP note 1130801: DB6: Virtualization of IBM Db2 for Linux, UNIX, and Windows). 

- IBM supports subcapacity licensing on VMware (see IBM's subcapacity licensing FAQ, item 11). 

- IBM uses the Processor Value Unit (PVU) metric for licensing. 

For VMware, the IBM policy is as follows: "Each vCPU is equal to one processor core for PVU licensing. We license to the lower of the sum of vCPUs or full (physical) capacity of the server." Licensing examples: 

- › 1 × 8 vCPU VM running Db2 on a 16-core ESXi host: 8 cores for PVU licensing 

- › 3 × 8 vCPU VMs or 1 × 32 vCPU VM on a 16-core ESXi host (vCPU overcommit): 16 cores for PVU licensing 

- › 1 × 16 vCPU VM (running Db2) on an ESXi cluster with 2 × 16-core ESXi hosts (extra host added for vSphere High Availability failover): 16 cores for PVU licensing 

© 2026 Nutanix, Inc. All rights reserved  | **29** 

SAP on Nutanix Best Practices 

## 7. References and Resources 

Nutanix AHV: 

- AHV Best Practice Guide 

- SAP on Nutanix Solution Brief 

- Nutanix KB 8805: How to Change AHV Hypervisor Identity 

VMware vSphere: 

- Architecture Guidelines and Best Practices for Deployments of SAP HANA on VMware vSphere 

- Monitoring Business Critical Applications with VMware vCenter Operations Manager 

- Architecting Oracle Workloads on VMware Hybrid Multi-Clouds 

- Performance Best Practices for VMware vSphere 7.0 

- SAP Sybase Adaptive Server Enterprise on VMware vSphere: Essential Deployment Tips 

- VMware vSphere Resource Management 

Microsoft Hyper-V: 

- SAP on Microsoft Hyper-V 

- Hyper-V Getting Started Guide 

- Performance Tuning Guidelines for Windows Server 2022 

- Step-by-Step Guide for Testing Hyper-V and Failover Clustering 

- Microsoft Tech Community 

- SAP on Windows Community 

- Documentation Related to SAP on Hyper-V 

- SAP on Windows Server 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

SAP on Nutanix Best Practices 

- Microsoft Virtualization 

- Microsoft Industry 

## Databases: 

- SAP HANA on AHV Best Practice Guide 

- SAP HANA on vSphere Best Practice Guide 

- Oracle on Nutanix Best Practice Guide 

- Microsoft SQL Server Best Practice Guide 

© 2026 Nutanix, Inc. All rights reserved  | **31** 

SAP on Nutanix Best Practices 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **32** 


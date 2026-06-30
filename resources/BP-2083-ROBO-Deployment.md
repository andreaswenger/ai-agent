# **ROBO Deployment and Operations Best Practices** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Code samples and snippets that appear in this content are unofficial, are unsupported, and will require extensive modification before use in a production environment. As such, the code samples and snippets are provided AS IS and are not guaranteed to be complete, accurate, or up-to-date. Nutanix makes no representations or warranties of any kind, express or implied, as to the operation or content of the code samples or snippet. Nutanix expressly disclaims all other guarantees, warranties, conditions and representations of any kind, either express or implied, and whether arising under any statute, law, commercial use or otherwise, including implied warranties of merchantability, fitness for a particular purpose, title and non-infringement therein. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

ROBO Deployment and Operations Best Practices 

## **Contents** 

**1. Executive Summary.................................................................................4 2. ROBO Cluster Architecture Decisions.................................................. 6** Witness VM Requirements for ROBO Deployments.......................................................................... 7 Remote Site Data Seeding................................................................................................................. 8 **3. ROBO Deployment Hypervisor Considerations....................................9 4. Nutanix Prism for ROBO Deployments............................................... 10** Prism Central with Nutanix Cloud Manager Intelligent Operations.................................................. 11 Nutanix Central..................................................................................................................................11 **5. Multisite ROBO Operations with Nutanix Prism.................................13** Upgrade Management in Nutanix Prism...........................................................................................13 **6. Nutanix Disaster Recovery for ROBO Deployments.......................... 15** Local and Remote Snapshot Storage Sizing....................................................................................16 Bandwidth Requirement for Replication........................................................................................... 17 Single-Node Backup Target.............................................................................................................. 18 **7. ROBO Deployment Disk and Node Failures....................................... 20 8. Best Practices for ROBO Cluster Storage and Prism Central........... 22 About Nutanix.............................................................................................24 List of Figures.............................................................................................................................................25** 

ROBO Deployment and Operations Best Practices 

## 1. Executive Summary 

Nutanix provides a powerful converged compute and storage system that offers oneclick simplicity and high availability for remote and branch office (ROBO) locations. This document describes best practices for deploying and operating ROBO locations on Nutanix, including guidance on choosing the right Nutanix cluster for seeding data to overcome slow remote network links. 

Nutanix Cloud Platform's self-healing design reduces operational and support costs, such as unnecessary site visits and overtime. With Nutanix, you can proactively schedule projects and site visits on a regular cadence rather than working around emergencies. Prism Central, our end-to-end infrastructure management tool, streamlines remote cluster operations through one-click upgrades and provides simple orchestration for multiple cluster upgrades. Nutanix makes deploying and operating ROBO locations as easy as deploying to the public cloud, but with control and security on your terms. 

This document covers the following topics: 

- Overview of the Nutanix solution for managing multiple remote offices 

- Nutanix cluster architecture for remote sites 

- Network sizing for management and disaster recovery traffic 

- Best practices for remote management with Nutanix Cloud Manager Intelligent Operations 

- How to design for failure at remote sites 

_Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|June 2017|Original publication.|
|1.1|March 2018|Added information on one- and|
|||two-node clusters.|
|1.2|February 2019|Updated Nutanix overview.|



© 2026 Nutanix, Inc. All rights reserved  | **4** 

ROBO Deployment and Operations Best Practices 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|1.3<br>1.4<br>1.5<br>1.6<br>1.7<br>1.8<br>1.9<br>2.0<br>3.0<br>3.1<br>3.2|April 2019<br>Updated witness<br>requirements.<br>February 2020<br>Updated the Best Practices<br>Checklist section.<br>December 2020<br>Model updates.<br>January 2022<br>Updated the Remote Office<br>and Branch Office Deployment<br>section.<br>October 2022<br>Updated product naming.<br>October 2023<br>Removed the Cloud Connect<br>section.<br>December 2023<br>Minor editorial updates.<br>July 2024<br>Updated the Witness<br>Requirements, Initial<br>Installation and Sizing, Prism<br>Central Management, and<br>Prism Central Best Practices<br>sections and added the<br>Nutanix Central section.<br>July 2025<br>Updated for AOS 7.0 and 7.3.<br>September 2025<br>Updated document structure.<br>February 2026<br>Updated the ROBO Cluster<br>Architecture Decisions section.|



© 2026 Nutanix, Inc. All rights reserved  | **5** 

ROBO Deployment and Operations Best Practices 

## 2. ROBO Cluster Architecture Decisions 

Picking the right solution always involves trade-offs. While a remote site isn't a datacenter, uptime is still a crucial concern. Financial constraints and physical layout also affect what counts as the best architecture for your environment. Nutanix offers a variety of clusters for remote locations. You can select single- and dual-socket cluster options and options that can reduce licensing costs. 

## **Three-node clusters** 

Although a three-node system might cost more money up front, it's the gold standard for ROBO locations. Three-node clusters provide excellent data protection by always committing two copies of your data, which means that your data is safe even during failures. Three-node clusters also rebuild your data within 60 seconds of a node going down. Distributed storage rebuilds the data on the downed node without any user intervention. 

A self-healing three-node Nutanix cluster also prevents unnecessary trips to remote sites. We recommend designing these systems with enough capacity to handle an entire node going down, which allows the loss of multiple hard drives, one at a time. Because this solution doesn’t rely on RAID, the cluster can lose and heal drives, one after the next, until available space runs out. 

For three-node clusters at sites with high availability requirements or sites that are difficult to visit, we recommend configuring one node and one disk cluster fault tolerance. One node and one disk fault tolerance can withstand the simultaneous failure of one node and one disk in another node. Three-node clusters can scale up to eight nodes with 1 Gbps networking and up to any scale when using 10 Gbps and higher networking. With our reliability and availability, you can focus on expanding your business rather than wasting resources on emergency site visits. 

## **Two-node clusters** 

Two-node clusters offer reliability for smaller sites that must be cost-effective and run with tight margins. These clusters use a witness only in failure scenarios to coordinate rebuilding data and automatic upgrades. You can deploy the witness offsite up to 500 ms away for ROBO locations and 200 ms when you use Metro 

© 2026 Nutanix, Inc. All rights reserved  | **6** 

ROBO Deployment and Operations Best Practices 

Availability. With ESXi, multiple clusters can use the same witness VM for two-node and Metro clusters. With AHV, you must use different witness VMs for two-node and Metro Availability clusters. AHV Metro Availability uses the witness service that is integrated into Prism Central or running as a separate VM. 

## **One-node clusters** 

One-node clusters are a perfect fit if you have low availability requirements and need strong overall management for multiple sites. One-node clusters provide resilience against the loss of a hard drive while still offering great remote management. Nutanix supports one-node clusters with AHV and ESXi only. 

Nutanix Cloud Infrastructure – Edge (NCI-Edge) is an alternative to the core-based NCI licensing option and offers a per-VM pricing model appropriate for edge and remote office and branch office (ROBO) use cases. You can use the NCI-Edge platform license for small clusters running up to 25 concurrently powered-on VMs, with each VM limited to 96 GB of memory. NCI-Edge provides the same hybrid cloud infrastructure capabilities and benefits as NCI in a pricing model suitable for small edge deployments. 

**Note:** You must run the license on a dedicated software-licensed edge cluster with no core-based licensing. You cannot mix NCI license types in one cluster. 

For more information, see the Nutanix Cloud Infrastructure (NCI-Edge) offer overview. 

For all two- and three-node clusters, Nutanix recommends n + 1 nodes to ensure sufficient space for rebuilding. Remove an additional 5 percent so that the system isn't full on rebuild. For single-node clusters, reserve 55 percent of usable space to recover from the loss of a disk. 

The NX-1000 series is built for ROBO environments. For more information on all available sizing options, see the dynamic sizing sheet. 

## **Witness VM Requirements for ROBO Deployments** 

You can deploy the witness as a separate VM or use the witness service integrated in Prism Central. The witness VM requires the following minimum specifications: 

- 2 vCPU 

- 6 GB of memory 

- 25 GB of storage 

© 2026 Nutanix, Inc. All rights reserved  | **7** 

ROBO Deployment and Operations Best Practices 

The witness VM must reside in a separate failure domain, which means that you must have independent power and network connections from each of the two-node clusters. We recommend locating the witness VM in a third physical site with dedicated network connections to sites one and two to avoid a single point of failure. 

Communication with the witness happens over port TCP 9440; open this port for the Controller VMs (CVMs) on any two-node clusters using the witness. 

Network latency between each two-node cluster and the witness VM must be less than 500 ms for ROBO locations. 

The witness VM can reside on any supported hypervisor and run on either Nutanix or non-Nutanix hardware. You can register multiple (different) two-node cluster pairs to a single witness VM. One witness VM can support up to 100 ROBO clusters or sites. 

## **Remote Site Data Seeding** 

When a remote site has a limited network connection back to the main datacenter, you might need to seed data to overcome network speed deficits. Seeding involves using a separate device to ship the data to a remote location. Instead of replication taking weeks or months, depending on the amount of data you must protect, you can copy the data locally to a separate Nutanix node and ship it to your remote site. 

Nutanix checks the snapshot metadata before seeding the device to prevent unnecessary duplication. Nutanix can apply its native data protection to a seed cluster by placing VMs in a protection domain and replicating them to a seed cluster. A protection domain is a collection of VMs that have a similar recovery point objective (RPO). You must ensure, however, that the seeding snapshot doesn’t expire before you can copy the data to the destination. 

© 2026 Nutanix, Inc. All rights reserved  | **8** 

ROBO Deployment and Operations Best Practices 

## 3. ROBO Deployment Hypervisor Considerations 

Nutanix supports multiple hypervisors to meet your enterprise's needs. The three main considerations for choosing the right hypervisor for your environment are supportability, operations, and licensing costs. 

Supportability can include support for your applications, the training your staff needs for daily activities, and break-fix support. When it comes to supportability, the path of least resistance often shapes hypervisor selection. Early on, when organizations could afford to use virtualization, ESXi was a prime candidate. Because many environments run ESXi, the Nutanix 1000 series offers a mixed-hypervisor deployment consisting of two ESXi nodes and one AHV storage node. The mixed-hypervisor deployment option provides the same benefits as a full three-node cluster but removes the CPU licensing required by some hypervisor licensing models. 

Operationally, Nutanix aspires to build infrastructure that's invisible to the people using it. We recommend that customers who want a fully integrated solution select AHV as their hypervisor. With AHV, virtual machine (VM) and data placement happen automatically without any required settings. Nutanix also hardens systems by default to meet security requirements and provides the automation necessary to maintain that security. Nutanix supplies Security Technical Implementation Guidelines (STIGs) in machine-readable code for both AHV and the storage controller. 

Nutanix offers cross-hypervisor disaster recovery to replicate VMs from AHV to ESXi or ESXi to AHV to avoid switching hypervisors in the main datacenter. In the event of a disaster, administrators can restore their AHV VM to ESXi for quick recovery or replicate the VM to the remote site with easy workflows. 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

ROBO Deployment and Operations Best Practices 

## 4. Nutanix Prism for ROBO Deployments 

Nutanix Prism provides central access for administrators to configure, monitor, and manage virtual environments. Powered by advanced data analytics, heuristics, and rich automation, Nutanix Prism offers unprecedented simplicity by combining several aspects of datacenter management into a single, consumer-grade solution. Using innovative machine learning technology, Nutanix Prism can mine large volumes of system data easily and quickly and generate actionable insights for optimizing all aspects of virtual infrastructure management. Nutanix Prism is a part of every Nutanix deployment and has two core components: Prism Element and Prism Central. 

## **Prism Element** 

Prism Element is a service built into the platform for every Nutanix cluster deployed. With it, you can fully configure, manage, and monitor Nutanix clusters running any hypervisor. 

Because Prism Element manages only the cluster it’s part of, each deployed Nutanix cluster has a unique Prism Element instance for management. As you deploy multiple Nutanix clusters, you must be able to manage all of them from a single Nutanix Prism instance, so Nutanix introduced Prism Central. 

## **Prism Central** 

Prism Central offers an organizational view into a distributed Nutanix deployment, with the ability to attach all remote and local Nutanix clusters to a single Prism Central deployment. This global management experience offers a single place to monitor performance, health, and inventory for all Nutanix clusters. 

The standard version of Prism Central offers all the features of Prism Element under one umbrella, with a single sign-on for your entire Nutanix environment, and makes day-to-day management easier by making all your applications accessible with the entity explorer. The entity explorer offers customizable tagging for applications so that even if they are dispersed among different sites, you can analyze their aggregated data in one central location. 

For more information, see the Best Practices for ROBO Cluster Storage and Prism Central section of this document. 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

ROBO Deployment and Operations Best Practices 

## **Prism Central with Nutanix Cloud Manager Intelligent Operations** 

Prism Central is available in a standard version included with every Nutanix deployment and as a separately licensed Nutanix Cloud Manager Intelligent Operations version that enables several advanced features. Nutanix Cloud Manager Intelligent Operations has additional features to manage large deployments and prevent emergencies and unnecessary site visits: 

- Customizable dashboards 

- Capacity runway to safeguard against exhausting resources 

- Capacity planning to safely reclaim resources from old projects and just-in-time forecasting for new projections 

- Advanced search to streamline access to features with minimal training 

- Simple multicluster upgrades 

## **Nutanix Central** 

Nutanix Central is a software as a service (SaaS) unified cloud console for viewing and managing your Nutanix environments and services deployed on-premises or on Nutanix Cloud Clusters (NC2). Nutanix customers use multiple Prism Central instances to manage clusters deployed in their environments that can span on-premises datacenters and NC2 on public cloud providers. Although Prism Central can manage multiple clusters, each Prism Central instance represents a separate management domain, meaning that customers must frequently switch between multiple consoles to manage their domains. This approach consumes unnecessary time and resources and leads to productivity loss due to siloed domain management and metrics views. 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

ROBO Deployment and Operations Best Practices 

Figure 1: Nutanix Central Unified Cloud Console 

Nutanix Central simplifies and streamlines the management of multiple Prism Central instances by providing a single management console. By registering your Prism Central domains to Nutanix Central, you can accomplish the following: 

- Access and manage multiple Prism Central domains through one panel. 

- View a unified dashboard with cluster-level domain metrics, including capacity usage and alert summary statistics. 

- Seamlessly navigate to individual Prism Central domains through the unified cloud console. 

- Discover Nutanix portfolio products and preferred partner apps, deploy them in the Prism Central domain of your choice through a domain-specific marketplace, and easily manage them through My Apps. 

- Use an app switcher to seamlessly move between apps and domains. 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

ROBO Deployment and Operations Best Practices 

## 5. Multisite ROBO Operations with Nutanix Prism 

When dealing with multiple sites, we recommend using a naming standard. However, naming standards typically don't last because of their rigidity, human error, and business changes. To add more management flexibility, tag VMs with one or more labels. When the entity explorer displays results, it represents your labels with a symbol; it also offers labels as an additional method for filtering results. You can use labels to tag VMs that belong to a single application, business owner, or customer. 

Prism Central also lets you tag a cluster with one or more tags, which is helpful for ROBO environments because Nutanix Prism manages at site-level granularity. You can then use the entity explorer in Nutanix Prism to perform operations or actions on multiple entities at the same time. For example, you can specify tags such as **medium ROBO sites in New York** and run the upgrade task with a single click. 

## **Upgrade Management in Nutanix Prism** 

Nutanix Prism makes it easy to keep remote sites up to date with one-click upgrades. For environment-specific service-level agreements, choose from two upgrade modes: simultaneous mode or staggered mode. 

## **Simultaneous mode** 

Simultaneous mode is important when you must upgrade quickly—for example, when performing a critical update or a security update that you must push to all ROBO sites and clusters quickly. Simultaneous mode upgrades all the clusters immediately, in parallel. 

## **Staggered mode** 

Staggered mode allows rolling, sequential upgrades of ROBO sites as a batch job, without any manual intervention—one site upgrades only after the previous site upgrades successfully. This feature is advantageous because it limits exposure to an issue (if one emerges) to one site rather than multiple sites. This safeguard 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

ROBO Deployment and Operations Best Practices 

is especially valuable for centralized administrators and others managing multiple ROBO sites. Staggered mode also lets you choose a custom order for the upgrade sequence. 

The following best practices help ensure that you meet your maintenance window: 

- If WAN links are congested, predownload your upgrade packages near the end of your business day. 

- Perform preupgrade checks before attempting the upgrade. 

When running preupgrade checks, you have the following options: 

- Run the checks from the Cluster Health area in the UI. 

- Log on to a CVM and run Nutanix Cluster Check from the Nutanix command-line interface (nCLI): 

```
nutanix@cvm$ ncc health_checks run_all
```

If the check reports a status other than `PASS` , resolve the reported issues before you proceed. If you can't resolve the issues, contact Nutanix Support for assistance. 

If you must upgrade multiple clusters, use Prism Central. 

**Note:** Create cluster labels for clusters in similar business units. If you must meet an upgrade window, you can upgrade all the selected clusters to run in parallel. We recommend running one upgrade first before continuing to all the clusters. 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

ROBO Deployment and Operations Best Practices 

## 6. Nutanix Disaster Recovery for ROBO Deployments 

Nutanix Disaster Recovery offers an entity-centric automated approach to protect and recover applications and is integrated with Prism Central. It uses categories to group the guest VMs and automate the protection of the guest VMs as the application scales. Application recovery is more flexible with network mappings, an enforceable VM start sequence, and interstage delays. You can also validate and test application recovery without affecting your production workloads. Asynchronous, NearSync, and synchronous replication schedules ensure that an application and its configuration details synchronize to one or more recovery locations for a smoother recovery. For more information, see Data Protection and Disaster Recovery Best Practices. 

Starting with AOS 7.3 and Prism Central 7.3, you can use Nutanix Multicloud Snapshot Technology (NMST) in a variety of disaster recovery deployment models. NMST supports replication to AWS S3, Azure Blob Storage, and Nutanix Objects, and offers a costeffective approach to disaster recovery. With NMST, you can protect multiple sites using async and NearSync replication to send one copy of your VM or volume group to site A and another copy to site B. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

ROBO Deployment and Operations Best Practices 

Figure 2: Nutanix Multicloud Snapshot Technology 

With this strategy, you can replicate snapshots to S3 buckets or Nutanix Objects directly from an on-premises cluster. In the event of a failover, you can quickly deploy an on-demand Nutanix cluster in AWS or Azure or on-premises for recovery using the replicated snapshots. This approach uses low-cost object storage for storing and recovering workloads as required. 

The Prism Central instance hosting the NMST service has 1-to-1 mapping with the configured S3 bucket or Nutanix Objects bucket. NMST supports a bucket configured with up to 300 TB of capacity and can protect over 1,000 entities (VMs or volume groups). NMST also supports a low recovery time objective of 1 hour. 

NMST deploys a total of four VMs to run the service: 

- Three VMs to run the microservices, each with 8 vCPUs, 16 GB of RAM, and 3 TB of storage (thin provisioned) 

- One VM for the load balancer that has 2 vCPUs, 4 GB of RAM, and 40 GB of storage 

## **Local and Remote Snapshot Storage Sizing** 

To size storage for local snapshots at the remote site, account for the rate of change in your environment and how long you plan to keep your snapshots on the cluster. Reduced 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

ROBO Deployment and Operations Best Practices 

snapshot frequency might increase the rate of change due to the greater chance of common blocks changing before the next snapshot. 

To find the space needed for local snapshots to meet your RPO, use the following formula. As you decrease the RPO for asynchronous replication, you might need to account for an increased rate of transformed garbage. Transformed garbage is space the system allocated for I/O optimization or assigned but that no longer has associated metadata. If you're replicating only once each day, you can remove `(change rate per` 

`frequency × # of snapshots in a full Curator scan × 0.1)` from the following formula. A full Curator scan runs every six hours. 

```
snapshot reserve = (frequency of snapshots × full change rate per frequency)
 + (change rate per frequency × # of snapshots in a Curator scan × 0.1)
```

You can look at your backups and compare their incremental differences to find the change rate. 

Using the local snapshot reserve formula and assuming for demonstration purposes that the change rate is 35 GB of data every six hours and that we keep 10 snapshots, we get a 363 GB snapshot reserve: 

```
snapshot reserve = (frequency of snapshots × change rate per frequency) +
(change rate per frequency × # of snapshots in a full curator scan × 0.1)
= (10 × 35,980 MB) + (35,980 MB × 1 × 0.1)
= 359,800 + (35,980 × 1 × 0.1)
= 359,800 + 3,598
= 363,398 MB
= 363 GB
```

Remote snapshots use the same process, but you must include the first full copy of the protection domain plus delta changes based on the set schedule. 

```
snapshot reserve = (frequency of snapshots × change rate per frequency) +
 (change rate per frequency × # of snapshots in a full Curator scan × 0.2) +
 total size of the source protection domain
```

To minimize the storage space you need at the remote site, use 130 percent of the protection domain as an average. 

## **Bandwidth Requirement for Replication** 

You must have enough available bandwidth to keep up with the replication schedule. If you are still replicating when the next snapshot is scheduled, the current replication job finishes first. The newest outstanding snapshot then starts to replicate the newest data to 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

ROBO Deployment and Operations Best Practices 

the remote side first. To help replication run faster with limited bandwidth, seed data on a secondary cluster at the primary site before you ship that cluster to the remote site. 

To calculate the required throughput, you must know your RPO. If you set the RPO to one hour, you must be able to replicate the changes within that time. 

Assuming that you know your change rate based on incremental backups or local snapshots, you can calculate the bandwidth you need. The next example uses a 15 GB change rate and a one-hour RPO: 

```
Bandwidth needed = (RPO change rate × (1 – compression on wire savings %)) /
 RPO
Example:
(15 GB × (1 – 0.3)) / 3,600 s
(15 GB × 0.7) / 3,600 s
10.5 GB / 3,600 s
(10.5 × 1,000 MB) / 3,600 s (changing the unit to MBps)
(10,500 MB) / 3,600 s
10,500 MB / 3,600 = 2.92 MBps
Bandwidth needed = 23.33 Mbps
```

We didn't use deduplication in the calculation, partly so that the dedupe savings can serve as a buffer in the overall calculation and partly because the one-time cost for deduped data going over the wire has less impact after the data is present at the remote site. We assumed an average of 30 percent bandwidth savings for compression on the wire. You can perform the calculation online using the WolframAlpha computational knowledge engine. 

If you have problems meeting your replication schedule, either increase your bandwidth or increase your RPO. To allow more RPO flexibility, you can run different schedules on the same protection domain. For example, set one daily replication schedule and create a separate schedule to take local snapshots every few hours. 

## **Single-Node Backup Target** 

With Nutanix, you can use an NX-1175S or NX-8155 appliance as a single-node backup target for an existing Nutanix cluster. Because this target has different resources than the original cluster, you primarily use it to provide backup for a small set of VMs. This utility gives small and medium-sized businesses and ROBO locations a fully integrated backup option. 

Follow these best practices for using a single-node backup target: 

- Keep all protection domains, combined, under 30 VMs total. 

© 2026 Nutanix, Inc. All rights reserved  | **18** 

ROBO Deployment and Operations Best Practices 

- To speed restores, limit the number of VMs in each protection domain. 

- Limit backup retention to a three-month policy. 

We recommend seven daily, four weekly, and three monthly backups. 

- Map a single-node backup target to only one physical cluster. 

- Set the snapshot schedule to six hours or more. 

- Turn off deduplication. 

Nutanix one- and two-node clusters follow the same best practices as the single-node backup target because of limited resources on the nodes. The only difference for oneand two-node clusters is that all protection domains have no more than five VMs per node. 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

ROBO Deployment and Operations Best Practices 

## 7. ROBO Deployment Disk and Node Failures 

The Hades service, which runs in the CVM, enables Nutanix storage to detect accumulating disk errors (for example, I/O errors or bad sectors). It simplifies the breakfix procedures for disks and automates several tasks that previously required manual user actions. Hades helps fix failing devices before they become unrecoverable. 

Nutanix also has a unified component called Stargate that receives and processes data. The system sends all read and write requests to the Stargate process running on the node. Stargate marks a disk offline when three consecutive I/O errors occur when writing an extent, taking the disks offline well before a catastrophic disk failure, as increasing I/O errors happen first as a disk drive ages. Hades then automatically removes the disk from the data path and runs smartctl checks against it. If the checks pass, Hades marks the disk online and returns it to service. If the smartctl checks fail or if Stargate marks a disk offline three times in one hour (regardless of the smartctl check results), Hades removes the disk from the cluster, and the following sequence occurs: 

**1.** Hades marks the disk for removal in the cluster's Zeus configuration. 

**2.** The system unmounts the disk. 

**3.** The disk's red LED turns on to provide a visual indication of the failure. 

**4.** The cluster automatically begins to create new replicas of the data stored on the disk. 

The system marks the disk as tombstoned to prevent the cluster from using it again without manual intervention. 

Marking a disk offline triggers an alert, and the system immediately removes the offline disk from the storage pool. Curator then identifies all extents stored on the failed disk and the distributed storage fabric and makes additional copies of the associated replicas to restore the desired replication factor. By the time administrators learn of the disk failure from Prism Element, SNMP trap, or email notification, distributed storage is already healing the cluster. 

The distributed storage data rebuild architecture provides faster rebuild times than traditional RAID data protection schemes, with no performance impact on workloads. RAID groups or sets usually have a small number of disks. When a RAID set performs a rebuild operation, it typically selects one disk as the rebuild target. The other disks 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

ROBO Deployment and Operations Best Practices 

in the RAID set must divert enough resources to quickly rebuild the data on the failed disk. This process can lead to performance penalties for workloads served by the degraded RAID set. Distributed storage allocates remote copies found on any individual disk to the remaining disks in the Nutanix cluster. As a result, distributed storage replication operations are background processes with no impact on cluster operations or performance. Moreover, Nutanix storage accesses all disks in the cluster at any given time as a single, unified pool of storage resources. 

Every node in the cluster participates in replication, which means that as the cluster size grows, disk failure recovery time decreases. Because distributed storage allocates the data needed to rebuild a disk throughout the cluster, more disks contribute to the rebuild process and accelerate the additional replication of affected extents. 

Nutanix maintains consistent performance during the rebuild operations. For hybrid systems, Nutanix rebuilds cold data to cold data so that large hard drives do not flood the SSD caches. For all-flash systems, Nutanix protects user I/O by implementing quality of service for back-end I/O. 

In addition to a many-to-many rebuild approach to data availability, the distributed storage data rebuild architecture ensures that all healthy disks are always available. Unlike most traditional storage arrays, Nutanix clusters don't need hot spare or standby drives. Because data can rebuild to any of the remaining healthy disks, you don't need to reserve physical resources for failures. After the data is healed, you can lose the next drive or node. 

A Nutanix cluster must have at least three nodes. Minimum configuration clusters provide the same protections as larger clusters, and a three-node cluster can continue normally after one node and one disk in a different node fail if you use one node and one disk cluster fault tolerance. Configure the storage container with a replication factor of 3 to use one node and one disk fault tolerance. When a disk fails, AOS rebuilds the data right away. Reserve enough space to ensure that every node can recover from a single drive failure: 

```
Fault-tolerant capacity = Total physical capacity – Size of largest drive on
 each node
```

For example, if your three-node cluster has 20 TB total capacity and the largest drive is 1 TB on each node, reserve 17 TB for the cluster: `20 TB – 3 TB = 17 TB` . 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

ROBO Deployment and Operations Best Practices 

## 8. Best Practices for ROBO Cluster Storage and Prism Central 

Follow these best practices for cluster storage capacity: 

- Size for n + 1 nodes for usable space, including an additional 5 percent for system overhead. 

- Configure three-node clusters in remote areas with one node and one disk cluster fault tolerance. 

Follow these Prism Central best practices for networking, installation and sizing, statistics gathering, and cluster registration and licensing: 

## **Network** 

Prism Central uses TCP port 9440 to communicate with the CVMs in a Nutanix cluster. If your network or servers have a firewall enabled, open port 9440 between the CVMs and the Prism Central VM to allow access. 

Always deploy with DNS. Prism Central occasionally does a request on itself; if it can't resolve the DNS name, some cluster statistics might not be present. 

If you use LDAP or LDAPS for authentication, open port 3268 (for LDAP) or 3269 (for LDAPS) on the firewall. 

## **Initial installation and sizing** 

**Extra-small environments:** For fewer than 500 VMs, size Prism Central with 4 vCPU, 18 GB of memory, and 100 GiB of storage. No scale-out option is available. 

**Small environments:** For fewer than 2,500 VMs, size Prism Central with 6 vCPU, 28 GB of memory, and 500 GiB of storage. A scale-out option is available. 

**Large environments:** For up to 12,500 VMs, size Prism Central with 10 vCPU, 46 GB of memory, and 2,500 GiB of storage. A scale-out option is available. 

**Extra-large environments:** For up to 12,500 VMs, size Prism Central with 14 vCPU, 62 GB of memory, and 2,500 GiB of storage. A scale-out option is available. Includes pre-allocated resources for the Atlas Network Controller (ANC) service. 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

ROBO Deployment and Operations Best Practices 

**Note:** Upgrading from a smaller installation to a larger one isn’t as simple as changing CPU and memory. In some cases, you must add more storage and edit the configuration files. Contact Nutanix Support before changing Prism Central sizes. 

## **Statistics** 

Prism Central keeps 13 weeks of raw metrics and 53 weeks of hourly metrics. 

Nutanix Support can help you keep statistics over a longer period if needed. However, after you change the retention time, only stats written after the change have the new retention time. 

## **Cluster registration and licensing** 

Each node registered to and managed by Nutanix Cloud Manager Intelligent Operations requires you to apply for an Intelligent Operations license through the Prism Central web console. For example, if you register and manage 10 Nutanix nodes (regardless of the individual node or cluster license level), you must apply for 10 Intelligent Operations licenses through the Prism Central web console. 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

ROBO Deployment and Operations Best Practices 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

ROBO Deployment and Operations Best Practices 

## **List of Figures** 

Figure 1: Nutanix Central Unified Cloud Console........................................................................................12 Figure 2: Nutanix Multicloud Snapshot Technology.....................................................................................16 


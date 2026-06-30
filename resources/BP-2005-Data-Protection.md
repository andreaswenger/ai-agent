# **Data Protection and Disaster Recovery Best Practices** 

## Legal 

© 2025 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Data Protection and Disaster Recovery Best Practices 

## **Contents** 

**1. Executive Summary.................................................................................5 2. Web-Scale Data Protection Overview.................................................... 9 3. Protection Domain–Based Disaster Recovery....................................11** Failover Migration vs. Activation.......................................................................................................12 **4. Nutanix Snapshots.................................................................................14** Full Local Snapshot Sizing............................................................................................................... 20 Asynchronous Replication Sizing......................................................................................................21 Lightweight Snapshot Sizing.............................................................................................................21 NearSync Sizing................................................................................................................................22 **5. Replication Topologies..........................................................................24** Nutanix Multicloud Snapshot Technology.........................................................................................27 Seeding Data for Replication to a New Site.....................................................................................31 **6. Local Backup with Snapshots..............................................................33** Nutanix Volume Shadow Copy Service Requirements and Best Practices......................................34 **7. Remote Site Backup and Disaster Recovery......................................37** Remote Site IP Address Best Practices...........................................................................................38 Remote Container Best Practices.....................................................................................................39 Full Snapshot and Asynchronous Replication Scheduling............................................................... 40 Lightweight Snapshot and NearSync Scheduling.............................................................................41 Cross-Hypervisor Disaster Recovery Best Practices........................................................................42 Single-Node Backup Target Best Practices......................................................................................42 **8. Disaster Recovery Orchestration.........................................................44** Paired Availability Zones...................................................................................................................45 Protection Policies for Nutanix Disaster Recovery........................................................................... 46 Recovery Plans for Nutanix Disaster Recovery............................................................................... 47 

**9. Self-Service File Restore.......................................................................50 10. Third-Party Backup Products............................................................. 51 11. Metro Availability for AHV and ESXi..................................................52 12. Instant Recovery Data Protection for AHV-Based VMs....................54** On-Demand VM Instant Recovery Option........................................................................................54 Protection Domain VM Data Instant Recovery Option..................................................................... 56 **13. Nutanix Data Protection and Disaster Recovery Best Practices.....57 About Nutanix.............................................................................................64 List of Figures.............................................................................................................................................65** 

Data Protection and Disaster Recovery Best Practices 

## 1. Executive Summary 

Nutanix Cloud Platform can deliver storage, compute, and virtualization services for any application. Designed for supporting multiple virtualized environments, including Nutanix AHV, VMware ESXi, and Microsoft Hyper-V, Nutanix invisible infrastructure provides many ways to achieve your recovery point objectives (RPOs). 

Enterprises are increasingly vulnerable to data loss and downtime during disasters because they rely on virtualized applications and infrastructure that their legacy data protection and disaster recovery solutions can no longer adequately support. This best practice guide provides the optimal configuration for achieving data protection using the native Nutanix disaster recovery capabilities and the disaster recovery orchestration features available on-premises and in public cloud providers like Amazon Web Services (AWS). 

Whatever your use case, you can protect your applications with ease. Nutanix Prism facilitates management to configure the shortest recovery time objectives (RTOs) possible so that you can build complex disaster recovery workflows at a moment’s notice. With Prism Central, you can apply protection policies across all your managed clusters. Based on your RPO, you can activate recovery plans to validate, test, migrate, and fail over seamlessly. Recovery plans can protect availability zones on-premises, in the public cloud with Nutanix Cloud Clusters (NC2), and with managed service providers. 

As application requirements change and grow, Nutanix can adapt to business needs. Nutanix is uniquely positioned to protect and operate in environments with minimal administrative effort because of its web-scale architecture and commitment to enterprise cloud operations. 

Best practice guidance for implementing data protection solutions on Nutanix servers running AOS 6.10 covers the following topics: 

- Scalable metadata 

- Backup 

- Crash-consistent versus application-consistent snapshots 

© 2025 Nutanix, Inc. All rights reserved  | **5** 

Data Protection and Disaster Recovery Best Practices 

- Protection domains 

- Protection policies 

- Recovery plans 

- Scheduling snapshots and asynchronous replication 

- Sizing disk space for local snapshots and replication 

- Scheduling lightweight snapshots (LWS) and NearSync replication 

- Sizing disk space for LWS and NearSync replication 

- Determining bandwidth requirements 

- File-level restore 

## _Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|December 2014|Original publication.|
|2.0|March 2016|Updated best practices|
|||throughout.|
|2.1|June 2016|Updated the Backup and|
|||Disaster Recovery on Remote|
|||Sites section.|
|2.2|July 2016|Updated bandwidth sizing|
|||information.|
|2.3|December 2016|Updated for AOS 5.0.|
|2.4|May 2017|Updated information on sizing|
|||SSD space on a remote|
|||cluster.|
|3.0|December 2017|Updated for AOS 5.5.|
|3.1|September 2018|Updated the Nutanix overview|
|||and the Remote Site Setup|
|||section.|
|4.0|December 2018|Updated for AOS 5.10 and Xi|
|||Leap.|



© 2025 Nutanix, Inc. All rights reserved  | **6** 

Data Protection and Disaster Recovery Best Practices 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|4.1<br>4.2<br>4.3<br>5.0<br>5.1<br>5.2<br>5.3<br>5.4<br>5.5<br>5.6<br>5.7<br>5.8<br>5.9<br>6.0<br>7.0|February 2019<br>Updated the Sizing Space<br>section and Leap product<br>details.<br>August 2019<br>Updated for AOS 5.11.<br>October 2019<br>Updated for AOS 5.11.1 and<br>Nutanix Files support for<br>NearSync replication.<br>May 2020<br>Updated for AOS 5.17.<br>September 2020<br>Updated for AOS 5.18.<br>December 2020<br>Updated for AOS 5.19 and<br>Leap multisite replication.<br>March 2021<br>Refreshed content.<br>February 2022<br>Updated the Third-Party<br>Backup Products section.<br>May 2022<br>Updated product names and<br>references to configuration<br>maximums throughout.<br>August 2022<br>Updated the Third-Party<br>Backup Products section.<br>October 2023<br>Removed the PowerShell<br>Scripts section.<br>May 2024<br>Updated the Executive<br>Summary section.<br>June 2024<br>Updated the Disaster<br>Recovery Orchestration and<br>Nutanix VSS Support with<br>Nutanix Guest Tools sections.<br>April 2025<br>Updated the Virtual Networks<br>in Nutanix DRaaS section.<br>July 2025<br>Updated for AOS 6.10.|



© 2025 Nutanix, Inc. All rights reserved  | **7** 

Data Protection and Disaster Recovery Best Practices 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|7.1<br>7.2<br>7.3<br>7.4|August 2025<br>Updated the Nutanix VSS<br>Support with Nutanix Guest<br>Tools and Storage and Volume<br>Shadow Copy Service Best<br>Practices sections.<br>September 2025<br>Updated document structure.<br>September 2025<br>Updated the Self-Service File<br>Restore section.<br>December 2025<br>Updated the Remote Site<br>Backup and Disaster Recovery<br>section.|



© 2025 Nutanix, Inc. All rights reserved  | **8** 

Data Protection and Disaster Recovery Best Practices 

## 2. Web-Scale Data Protection Overview 

One of the key architectural differentiators for Nutanix is the ability to scale. Nutanix isn't bound by the same limitations as dual-controller architectures or federations relying on special hardware like NVRAM or custom ASICs for performance. When it comes to snapshots and disaster recovery, scaling metadata is a key part of delivering performance while ensuring availability and reliability. Each Nutanix node is responsible for a subset of the overall platform's metadata. All nodes in the cluster serve and manipulate metadata entirely through software, eliminating traditional bottlenecks. 

Because each node has its own virtual storage controller and access to local metadata, replication scales with the system. Every node participates in replication to reduce hotspots throughout the cluster. 

Nutanix uses two different forms of snapshots: full snapshots for asynchronous replication (when the RPO is 60 minutes or greater) and lightweight snapshots (LWS) for NearSync replication (when the RPO is between 1 minute and 15 minutes). Full snapshots keep system resource usage low when you use many snapshots over an extended period. The LWS feature reduces metadata management overhead and increases storage performance by decreasing the high number of storage I/O operations that long snapshot chains can cause. 

Figure 1: Scalable Replication 

© 2025 Nutanix, Inc. All rights reserved  | **9** 

Data Protection and Disaster Recovery Best Practices 

In asynchronous replication, every node can replicate four files, up to an aggregate of 100 MBps at one time. Thus, in a four-node configuration, the cluster can replicate 400 MBps or 3.2 Gbps. As you grow the cluster, the virtual storage controllers keep replication traffic distributed. In many-to-one deployments, as when remote branch offices communicate with a main datacenter, the main datacenter can use all its available resources to handle the increased replication load from the branch offices. When the main site is scalable and reliable, administrators don't have multiple replication targets to maintain, monitor, and manage. You can protect VMs and volume groups with asynchronous replication. 

NearSync offers unbound throughput. All writes go to SSD, so to avoid filling the performance tier, AOS automatically allocates 7 percent or 360 GB per node (whichever is less) of each node's SSD capacity for NearSync. NearSync, which covers both VMs and volume groups, is supported for bidirectional replication between two clusters. 

Nutanix also provides cross-hypervisor disaster recovery natively with asynchronous replication. Existing vSphere clusters can target AHV-based clusters as their disaster recovery and backup targets. With VM mobility, you can place your workloads on the platform that best meets their needs. 

Nutanix Cloud Infrastructure provides two native methods to provide disaster recovery for your workloads. While both methods use the same underlying snapshot and replication technology, the granularity at which they protect and recover, where they are managed, and their full set of capabilities are different. 

© 2025 Nutanix, Inc. All rights reserved  | **10** 

Data Protection and Disaster Recovery Best Practices 

## 3. Protection Domain–Based Disaster Recovery 

A protection domain is a group of VMs or volume groups that you can snapshot locally and replicate to one or more clusters when you configure a remote site. Nutanix supports several protection domain–based disaster recovery solutions, including using remote sites, full snapshots and asynchronous replication, lightweight snapshots (LWS) and NearSync replication, cross-hypervisor disaster recovery, and single-node backup targets. 

Use Prism Element to set up protection domains and remote sites and manage storage and network mapping. To fail over a protection domain from one site to another, you must sign in to the target failover cluster where data is replicated and click **Activate** (disaster recovery). For more information, see the Data Protection and Recovery with Prism Element guide. 

Best practices for implementing protection domain–based disaster recovery: 

- Protection domain names must be unique across sites. 

- VMware Site Recovery Manager and Metro Availability protection domains are limited to 10,000 files. 

- Group VMs with similar RPO requirements. 

- NearSync can only have one schedule, so place NearSync VMs in their own protection domain in groups of 10 or fewer. 

For more information on protection domain limits, see the Nutanix Configuration Maximums page. 

You can create a consistency group for VMs and volume groups that are part of a protection domain and must be snapshotted in a crash-consistent manner. 

Best practices for implementing consistency groups: 

© 2025 Nutanix, Inc. All rights reserved  | **11** 

Data Protection and Disaster Recovery Best Practices 

- Keep consistency groups as small as possible. To ensure that dependent applications or service VMs are recovered in a consistent state, collect them into a consistency group (for example, put a web server and database in the same consistency group). 

- For all hypervisors, try to limit consistency groups to fewer than 10 VMs following these best practices. Although we tested consistency groups with up to 50 VMs, it’s more efficient to have smaller consistency groups. 

- Each consistency group using application-consistent snapshots can contain only one VM. 

- When you provide disaster recovery for VDI using Omnissa Horizon View Composer or Citrix Machine Creation Services, place each protected VM in its own consistency group (including the template image) inside a single protection domain. 

## **Failover Migration vs. Activation** 

Nutanix protection domains offer two options for handling VMs and volume groups during failover: migrate and activate. 

Use the migrate option when you know that both the production site and the remote site are healthy. This option is only available on the active production site. Migrate shuts down the VM or volume group, takes another snapshot, and replicates the VM or volume group to the selected remote site. This scenario is known as a planned failover or disaster avoidance. 

The activate option for restoring a VM is only available on the remote site. When you select activate, the system uses the last snapshot on the remote side to bring up the VM, regardless of whether the active production site is healthy. You can’t sync any outstanding changes to the VM from the production site. 

If you activate a protection domain because the primary site is down but the primary site comes back up after the failover, it might cause a split-brain scenario. To resolve this situation, deactivate the protection domain on the former primary site. The following command is hidden from the nCLI because it deletes the VMs, but it resolves the split while keeping the existing snapshots: 

```
ncli> pd deactivate_and_destroy_vms name=<protection_Domain_Name>
```

© 2025 Nutanix, Inc. All rights reserved  | **12** 

Data Protection and Disaster Recovery Best Practices 

You can test a VM at the remote site without breaking replication using the restore or clone functionality. 

Use the local snapshot browser on the inactive production domain at the remote site, and choose the restore option to clone a VM to the datastore. Add a prefix to the VM’s path. 

Inactive protection domains still consume space in existing snapshots, so remove any unused protection domains to reclaim that space. 

To remove a protection domain from a cluster, follow these steps: 

**1.** Remove existing schedules. 

**2.** Remove both local and remote snapshots. 

**3.** Remove the VMs from the active protection domain. 

**4.** Delete the protection domain. 

Best practices for failover planning: 

- When you activate protection domains on the remote site, use intelligent placement for Hyper-V and DRS for ESXi clusters. 

Intelligent placement evenly spreads out the VMs on startup during a failover. AOS powers on VMs uniformly at start time. 

- Install NGT on machines using volume groups. 

- Configure the data services cluster IP address on the remote cluster. 

© 2025 Nutanix, Inc. All rights reserved  | **13** 

Data Protection and Disaster Recovery Best Practices 

## 4. Nutanix Snapshots 

Fine-grained snapshots are the foundation of Nutanix data protection and disaster recovery. VM-centric snapshots provide data protection at the granularity of an individual virtual hard drive and can be configured to be taken and stored locally, replicated to another cluster, or both. Nutanix AOS distributed storage provides elegant snapshot functionality using a redirect-on-write algorithm. vDisks at the Nutanix layer back the files that AOS presents to VMs. Each vDisk in the system is hosted, or owned, by a Nutanix node's Controller VM (CVM). vDisks are made of blocks, which are 1 MB chunks of virtual address space. Pithos is the internal vDisk configuration manager that is responsible for vDisk (AOS file) configuration data. A Nutanix vDisk can be in one of the following modes: 

- Mutable (read/write): You can perform both reads and writes to and from the vDisk. 

- Immutable (read-only): You can't make any changes to the vDisk. Effectively, it's in read-only mode. 

- SnapshotImmutable (read-only): You can't make any changes to the vDisk. This mode is used during backup and disaster recovery situations. 

During the snapshot operation, the system puts the original vDisk in read-only mode and creates at least one new vDisk. The newly created vDisk and its metadata structure include internal references to the original vDisk; the two disks have a primary-secondary relationship. After taking a snapshot, AOS can copy metadata from the original vDisk to the live vDisk if needed. As the application continues to run, the system redirects new writes and updates to existing data to the current mutable vDisk. The original data in the snapshot remains unchanged, and the system shares the unchanged data across the snapshots and active VM. AOS handles the snapshot process transparently, so no changes to how applications and the virtualization stack access the VM occur. 

Snapshots represent a significant advancement in the traditional backup process. The storage system generates a snapshot by creating a full or virtual copy of the metadata or index of the stored data, rather than copying the entire pool of stored data. Because snapshots copy only the metadata or index, they can be nearly instantaneous, have minimal performance impact, and require little incremental space. IT organizations can 

© 2025 Nutanix, Inc. All rights reserved  | **14** 

Data Protection and Disaster Recovery Best Practices 

take snapshot-based backups more frequently to improve their RPO. Backup vendors and analysts widely acknowledge snapshots as the most practical option for faster recovery. 

The LWS feature further improves on snapshot technology by using markers and episodes to achieve an RPO between 15 minutes and 1 minute instead of creating full snapshots. LWS reduces metadata management overhead and increases storage performance by decreasing the high number of storage I/O operations that long snapshot chains can cause. You don’t have to set a policy to determine whether the system uses vDisk snapshots or LWS; Nutanix AOS automatically transitions between the two forms of replication based on the RPO and available bandwidth. If the network can't handle the low RPO, replication changes to vDisk snapshots. When the network can meet the requirements for NearSync again, AOS returns to using LWS. In oversubscribed networks, NearSync can provide almost the same level of protection as synchronous replication while avoiding network latency on the running workload. 

Another consideration with snapshots is the granularity of data you can protect. Granularity determines the snapshot's space overhead. When you take a snapshot of a large block, a change to a small portion of that block creates a full new block with mostly duplicate data, causing the snapshot to be much larger than the amount of data changed. Smaller block sizes result in more data shared between snapshots and greater space efficiency. Per-VM or per-volume group snapshots enable instant recovery. Depending on the workload and associated service-level agreements, you can tune the snapshot schedule and retention periods to meet RPOs. With the intuitive snapshot browser, you can perform restore and cloning operations instantly on the local cluster. 

The following example describes the data and metadata structure for VM snapshots and how they evolve during the process. In this example, we have one VM (VM1) with one vDisk attached. This vDisk is one mount point in the VM1 OS; for example, / in a VM based on the Linux OS or C:\ in a VM based on the Windows OS. 

At the starting point, the VMs vDisk is in read/write mode (R/W in the following figures). 

© 2025 Nutanix, Inc. All rights reserved  | **15** 

Data Protection and Disaster Recovery Best Practices 

Figure 2: Snapshot: Starting Point 

The system takes a snapshot, creating vDisk_id 22 in Pithos, the AOS vDisk configuration manager. 

Figure 3: Snapshot: Initial Snapshot 

All read operations are issued in the context of vDisk_id 22, but they’re served from the metadata of vDisk_id 10. 

When the VM writes new data (D in the following image), it places this data in vDisk_id 22, which is now allocated on disk. 

© 2025 Nutanix, Inc. All rights reserved  | **16** 

Data Protection and Disaster Recovery Best Practices 

Figure 4: Snapshot: First Write 

When the VM overwrites data A with new data, the most recent copy of A goes to vDisk_id 22 with data D. 

Figure 5: Snapshot: Partial Data Overwrite 

Depending on the nature of the overwrite—overwriting the entire block or partially overwriting the block—AOS might need to complete more than just a write operation. 

© 2025 Nutanix, Inc. All rights reserved  | **17** 

Data Protection and Disaster Recovery Best Practices 

If the VM overwrites the entire block, it only needs to write the new data. If the VM only partially overwrites the block, AOS must copy the metadata that isn’t overwritten from vDisk_id 10 to vDisk_id 22 so that vDisk_id 22 can address the entire block. 

Taking another snapshot creates vDisk_id 33 in Pithos. 

Figure 6: Snapshot: Second Snapshot 

When the VM writes new data (E, F, and G in the following image), it places this data in vDisk_id 33, which is now allocated on disk. 

© 2025 Nutanix, Inc. All rights reserved  | **18** 

Data Protection and Disaster Recovery Best Practices 

Figure 7: Snapshot: Write to Second Snapshot 

To limit the number of metadata sources that AOS must read per vDisk to find the data requested by the VM, AOS performs snapshot metadata maintenance. After a snapshot maintenance task, the original vDisk has the most recent changes to its original data, the first snapshot has the most recent changes to its own data and a copy of data A from the original vDisk, and the second snapshot has the most recent changes to its own data and copies of data A, B, and C from the original vDisk and data D from the first snapshot. 

© 2025 Nutanix, Inc. All rights reserved  | **19** 

Data Protection and Disaster Recovery Best Practices 

Figure 8: Snapshot: Maintenance Tasks 

## **Full Local Snapshot Sizing** 

To size space for local snapshots, you must account for the rate of change in your environment and how long you plan to keep your snapshots on the cluster. Reduced snapshot frequency might increase the rate of change because common blocks are more likely to change before the next snapshot. 

As you lower the RPO for asynchronous replication, you might need to account for an increased rate of transformed garbage, which is space that was allocated for I/O optimization or space that was assigned but the metadata no longer refers to. 

To find the space needed to meet your RPO, use the following formula: 

`snapshot reserve = (frequency of snapshots × change rate per frequency) + (change rate per frequency × # of snapshots in a full curator scan × 0.1)` 

A full curator scan runs every six hours. You can compare the incremental differences between your backups to find the change rate. You can also take a conservative 

© 2025 Nutanix, Inc. All rights reserved  | **20** 

Data Protection and Disaster Recovery Best Practices 

approach and start with a low snapshot frequency and a short expiry policy, then gauge the size difference between backups before consuming too much space. 

To demonstrate using the local snapshot reserve formula, we assume that the change rate is 35 GB of data every six hours and that we keep ten snapshots: 

```
snapshot reserve = (frequency of snapshots × change rate per frequency) +
(change rate per frequency × # of snapshots in a full curator scan × 0.1)
= (10 × 35,980 MiB) + (35,980 MiB × 1 × 0.1)
= 359,800 + (35,980 × 1 × 0.1)
= 359,800 + 3,598
= 363,398 MiB
= 363 GiB
```

## **Asynchronous Replication Sizing** 

Sizing for asynchronous replication uses the same process, but you must include the first full copy of the protection domain plus delta changes based on the set schedule. 

Remote snapshot reserve formula: 

```
snapshot reserve = (frequency of snapshots × change rate per frequency) +
(change rate per frequency × # of snapshots in a full curator scan × 0.2)
+ total size of the source protection domain
```

For the minimum amount of space needed on the remote side, 130 percent of the protection domain is a good average to work from. 

If the remote target also runs a workload, incoming replication uses the performance tier. If you use a hybrid cluster, size for the additional hot data. You can also skip the performance tier by creating a separate container for incoming replication and following the steps in the Remote Container section. 

## **Lightweight Snapshot Sizing** 

Sizing for LWS is similar to sizing for full snapshots in that you must account for change rate and retention time. During the LWS retention time, you don't have to size for transformed garbage space, but LWS does use additional SSD space that you must size for. By default, 7 percent of each node's SSD space forms the LWS reserve, which is an additional factor. 

If we use the workload from the full snapshot example, but change the RPO to one minute, we see that LWS data exists for 75 minutes plus one extra snapshot. Because 

© 2025 Nutanix, Inc. All rights reserved  | **21** 

Data Protection and Disaster Recovery Best Practices 

the frequency rate in this scenario is high, it’s important to account for overwrites in the change rate. Because all data goes through the oplog, it’s compressed; the type and amount of compression varies by workload. Using inline compression, we typically see a 2:1 compression rate. 

If your change rate is 35 GB of data every six hours, add 5 GB to account for overwrites. Using 40 GB every six hours has a change rate of approximately 114 MB per minute. If the cluster is running with replication factor 2, you must account for the total physical space. 

LWS reserve: 

```
= (frequency of snapshots × change rate per frequency) × oplog compression ×
 replication factor
= (75 × 114 MB per minute) × 0.50 × 2
= 8,550 MB
= 8.6 GB
```

If you run a 3460 hybrid system with two 1.9 TB SSDs, the following calculation shows your cluster LWS reserve: 

```
= ((total SSD capacity per node – (CVM + metadata + oplog + cache overhead))
 × number of nodes) × LWS reserve percentage
= (3,539 GiB – (120 GiB + 30 GiB + 200 GiB + 40 GiB)) × 4 × 0.07
= 3,149 GB × 4 × 0.07
= 881.72 GiB
= ~945 GB
```

Apply the 7 percent of the SSD space used by the extent store after you account for the rest of the system. Because the LWS cluster reserve is 945 GB, this system has room for the workload and additional business-critical applications. 

A telescopic schedule has a total of six hourly, seven daily, four weekly, and one monthly snapshots. You only need to account for additional garbage space for the daily snapshots because of the higher frequency. Using the previous change rate, you can calculate each separate schedule and add them all together. Because full snapshots occur less often than LWS, you don't have to be concerned with overwrites and can use the original 35 GB change rate for every six hours. 

## **NearSync Sizing** 

NearSync uses the same process as the snapshot process outlined previously, but you must include the first full copy of the protection domain and delta changes based on the set schedule. 

© 2025 Nutanix, Inc. All rights reserved  | **22** 

Data Protection and Disaster Recovery Best Practices 

For the minimum amount of space needed at the remote site, start with 130 percent of the protection domain plus the required LWS reserve space. 

Best practices for using NearSync: 

- Use drives with the same or greater capacity at the remote site compared to those in your primary cluster to ensure enough LWS reserve space. 

- Don't enable deduplication on the source container for NearSync. 

For most small and medium businesses, a daily change of 140 GB is considered high. The following NearSync example highlights the difference between keeping a lot of snapshots and keeping only the 10 snapshots in the full snapshot example. 

```
Hourly change rate: 5,833 MB
Daily change rate: 140,000 MB
Weekly change rate: 980,000 MB
Monthly change rate: 3,920,000 MB
```

```
snapshot overall capacity reserve = (hourly schedule + daily schedule +
 weekly schedule + monthly schedule)
```

```
For the hourly schedule:
(frequency of snapshots × change rate per frequency) + (change rate per
 frequency × # of snapshots in a full curator scan × 0.1)
The remaining schedules:
= (frequency of snapshots × change rate per frequency)
snapshot overall capacity reserve = ((6 × 5,833)+(5,833 × 6 × 0.1)) + (7 ×
 140,000) + (4 × 980,000) + (2 × 3,920,000)
= 34,998 + 3,500 + 980,000 + 3,920,000 + 8,257,538
= 13,196,036 MB
= ~13 TB
```

© 2025 Nutanix, Inc. All rights reserved  | **23** 

Data Protection and Disaster Recovery Best Practices 

## 5. Replication Topologies 

Replication is the process of copying and maintaining data objects in different locations to ensure that multiple redundant copies of the data exist for disaster recovery. Nutanix supports multiple replication topologies: One-to-one, many-to-one, single-node backup, and Nutanix Multicloud Snapshot Technology (NMST). 

## **One-to-one replication** 

Bidirectional replication of VMs and volume groups between two sites is necessary in environments where all sites must support active traffic. Consider a two-site example. Site B is the data protection target for selected workloads running on site A, while site A serves as the target for designated workloads running on site B. For the most current information regarding supported RPOs, see the Compatibility and Interoperability Matrix (Nutanix credentials required). 

© 2025 Nutanix, Inc. All rights reserved  | **24** 

Data Protection and Disaster Recovery Best Practices 

Figure 9: One-to-One Replication Topology 

## **Many-to-one replication** 

In many-to-one or hub-and-spoke architectures, you can replicate workloads running on sites A and B to a central site C. Centralizing replication to a single site can improve operational efficiency for geographically dispersed environments. Remote and branch office (ROBO) use cases are a classic many-to-one architecture. 

© 2025 Nutanix, Inc. All rights reserved  | **25** 

Data Protection and Disaster Recovery Best Practices 

Figure 10: Many-to-One Replication Topology 

## **Single-node replication target** 

Nutanix supports single-node backup as a cost-efficient solution for providing full native backups. Using the same underlying disaster recovery technology, the single node can be either on-site or remote. Nutanix protects data on the node from single drive failure and provides native backup end to end. Single-node systems can also run VMs for testing or remote branch offices. 

## **Nutanix Multicloud Snapshot Technology** 

NMST replicates native Nutanix AOS snapshots to object storage, AWS S3, and Azure Blob Storage. NMST Engine runs on Prism alongside Prism Central. This feature allows you to offload snapshots that you don’t regularly access or applications that have higher recovery time objectives (RTOs) to optimize 

© 2025 Nutanix, Inc. All rights reserved  | **26** 

Data Protection and Disaster Recovery Best Practices 

performance and capacity in the primary storage infrastructure. You can then recover workloads using a zero-compute (on-demand deployment of a Nutanix cluster on-premises or in Azure) or pilot-light (on-demand expansion of NC2 deployment) model, based on your needs. 

## **Nutanix Multicloud Snapshot Technology** 

Nutanix Multicloud Snapshot Technology (NMST) is particularly suitable if you don’t have a secondary location for disaster recovery or are looking for a low-cost alternative to running a secondary datacenter. It can also satisfy regulatory requirements for third-site data copies or requirements for an independent recovery site for testing or investigation. 

Coupled with Nutanix Cloud Clusters (NC2), NMST provides a low-cost recovery site on demand using public cloud resources. Managed service providers (MSPs) can also use it to provide a disaster recovery service to their customers in their facilities. 

Figure 11: Public Cloud and Nutanix Objects as a Replication Destination 

You can also use NMST's multisite capabilities to provide an additional layer of protection to workloads that are already protected between two Nutanix clusters. 

© 2025 Nutanix, Inc. All rights reserved  | **27** 

Data Protection and Disaster Recovery Best Practices 

Figure 12: NMST as a Secondary Disaster Recovery Target 

The extra level of protection offered by a third site satisfies certain data protection requirements or regulations. 

For more information, see Disaster Recovery Using Multicloud Snapshot Technology. 

You can deploy NMST one of three ways: 

© 2025 Nutanix, Inc. All rights reserved  | **28** 

Data Protection and Disaster Recovery Best Practices 

- Full disaster recovery setup: Source and destination with Prism Central and NMST 

   - › Pairs two Prism Central instances using the Availability Zone menu, as in other Nutanix Disaster Recovery setups 

   - › Restores workloads using Nutanix Disaster Recovery plans 

   - › Sets a private datacenter or NC2 on AWS or Azure as the destination cluster 

Figure 13: NMST Full Disaster Recovery Setup 

© 2025 Nutanix, Inc. All rights reserved  | **29** 

Data Protection and Disaster Recovery Best Practices 

- Snapshot only: Source with Prism Central and NMST, no destination cluster 

   - › Sends snapshot directly to supported object storage 

   - › In the event of a failure, deploys a new cluster using NC2 or sets up a new Prism Central or NMST instance at another site to restore workloads 

**Note:** Without compute at the destination, you cannot use Nutanix Disaster Recovery plans. Recover the VM manually using the VM Recovery Point menu in Prism Central. 

Figure 14: NMST Snapshot-Only Setup 

© 2025 Nutanix, Inc. All rights reserved  | **30** 

Data Protection and Disaster Recovery Best Practices 

- Many to one: Source runs workloads, destination runs Prism Central and NMST 

   - › Requires additional configuration if using Flow Virtual Networking to ensure that source nodes are excluded from Flow Virtual Networking setup 

   - › Can restore workloads using Nutanix Disaster Recovery plans 

Figure 15: NMST Many-to-One Setup 

## **Seeding Data for Replication to a New Site** 

You must have enough available bandwidth to keep up with the replication schedule. If you're still replicating when the next snapshot is scheduled, the current replication job finishes first. The newest outstanding snapshot then starts to retrieve the newest data to the remote side first. To help replication run faster when you have limited bandwidth, you can seed data on a secondary cluster at the primary site before shipping that cluster to the remote site. 

To seed data for a new site, follow these steps: 

**1.** Set up a secondary cluster with local IP addresses at the primary site. 

**2.** Enable compression on the remote site in the production domain. 

**3.** Set the initial retention time to three months. 

© 2025 Nutanix, Inc. All rights reserved  | **31** 

Data Protection and Disaster Recovery Best Practices 

**4.** Reconfigure the secondary cluster with remote IP addresses. 

**5.** Turn off the secondary cluster. 

**6.** Ship the secondary cluster to the remote site. 

**7.** Turn on the remote cluster. 

**8.** Update the remote site on the primary cluster to the new IP address. 

If you can’t seed the protection domain at the local site, you can create the remote cluster as a normal install and turn on compression over the wire. Manually create a onetime replication with a retention time set to three months. We recommend this retention time setting because it takes extra time to replicate the first data set across the wire. 

To figure out how much throughput you need, you must know your RPO. If you set the RPO to one hour, you must be able to replicate the changes within that time. 

Assuming you know your change rate based on incremental backups or local snapshots, you can calculate the bandwidth needed. The next example uses a change rate of 15 GB and an RPO of one hour. We don't use deduplication in the calculation, partly so the dedupe savings can serve as a buffer in the overall calculation and because the one-time cost for deduped data going over the wire has less impact after the data is present at the remote site. We assume an average of 30 percent bandwidth savings for compression on the wire. 

Bandwidth sizing: 

```
Bandwidth needed = (RPO change rate × (1 – compression on wire savings %)) /
 RPO
Example:
(15 GB × (1 - 0.3)) / 3,600 sec
(15 GB × 0.7) / 3,600 sec
10.5 GB / 3,600 sec
(10.5 × 1,000 MB) / 3,600 sec (changing to MBps)
(10,500 MB) / 3,600 sec
10,500 MB / 3,600 = 2.92 MBps
Bandwidth needed = 23.33 Mbps
```

If you keep missing your replication schedule, either increase your bandwidth or your RPO. For more RPO flexibility, you can run different schedules on the same production domain; for example, have one daily replication schedule and create a separate schedule to take local snapshots every two hours. 

© 2025 Nutanix, Inc. All rights reserved  | **32** 

Data Protection and Disaster Recovery Best Practices 

## 6. Local Backup with Snapshots 

Nutanix native snapshots provide production-level data protection without sacrificing performance. Nutanix uses a redirect-on-write algorithm to dramatically improve system efficiency for snapshots. Native snapshots operate at the VM level, and our crashconsistent snapshot implementation is the same across hypervisors. Implementation varies for application-consistent snapshots because of differences in the hypervisor layer. Nutanix can create local backups and recover data instantly to meet a wide range of data protection requirements. 

Best practices for local backup with snapshots: 

- Place all VM files on Nutanix storage. If you make non-Nutanix storage available to store files (VMDKs), the storage must have the same file path on both the source and destination clusters. 

- Remove all external devices, including ISOs and floppy devices. 

VM snapshots are by default crash-consistent, which means that the vDisks captured are consistent with a single point in time. The snapshot represents the on-disk data as if the VM crashed or the power cord was pulled from the server—it doesn't include anything that was in memory when the snapshot was taken. Today, most applications can recover well using crash-consistent snapshots. 

Application-consistent snapshots capture the same data as crash-consistent snapshots, with the addition of all data in memory and all transactions in process. Because of their extra content, application-consistent snapshots are the most involved and take the longest to perform. 

While most organizations find crash-consistent snapshots to be sufficient, Nutanix also supports application-consistent snapshots. The Nutanix application-consistent snapshot uses the Nutanix Volume Shadow Copy Service (VSS) to quiesce the file system for ESXi and AHV before taking the snapshot. You can configure which type of snapshot each protection domain maintains. 

You can configure local backups using snapshots in both protection domains and protection policies. 

© 2025 Nutanix, Inc. All rights reserved  | **33** 

Data Protection and Disaster Recovery Best Practices 

ESXi- and AHV-based snapshots call the Nutanix VSS provider from the Nutanix Guest Agent, which is one component of Nutanix Guest Tools (NGT). The VMware Tools suite talks to the guest VM’s Nutanix VSS writers. Application-consistent snapshots quiesce all I/O, complete all open transactions, and flush the caches. Nutanix VSS freezes write I/O while the native Nutanix snapshot takes place so that all data and metadata are written consistently. After the snapshot takes place, Nutanix VSS releases the system and allows queued writes to proceed. Application-consistent snapshots don’t snapshot the OS memory during this process. 

## **Nutanix Volume Shadow Copy Service Requirements and Best Practices** 

Requirements for Nutanix VSS snapshots: 

- You must have NGT installed. 

- You must configure an external cluster IP address. 

- If you use ESXi, guest VMs must be able to reach the external cluster IP address on port 2074. Guest VMs running on AHV use the serial port to communicate. 

- The TCP port 23578 in the guest VM must be accessible if you use the Nutanix VSS service. 

- Guest VMs must have an empty IDE CD-ROM for installing and configuring NGT. 

- Guest VMs must use ESXi or AHV. 

- Virtual disks must use the SCSI bus type. 

- For NearSync support, you must have NGT version 1.3 installed. 

Nutanix VSS isn’t supported with NearSync, for volume groups, or if the VM has any delta disks (hypervisor snapshots). 

© 2025 Nutanix, Inc. All rights reserved  | **34** 

Data Protection and Disaster Recovery Best Practices 

- Nutanix VSS snapshots are only available for these supported versions: 

   - › Windows 7 and later 

   - › Windows Server 2008 R2 and later 

   - › CentOS 6.5 and 7.0 

   - › Red Hat Enterprise Linux (RHEL) 6.5 and 7.0 

   - › Oracle Linux 6.5 and 7.0 

   - › SUSE Linux Enterprise Server (SLES) 11 SP4 and 12 

- Microsoft VSS must be running on the guest VM (Windows). 

- The guest VM must support the use of Nutanix VSS writers. 

- You can’t include volume groups in a protection domain configured for Metro Availability. 

- You can’t include volume groups in a protected VStore. 

- You can’t use Nutanix native snapshots to protect VMs that have VMware fault tolerance enabled. 

For ESXi, if NGT isn’t installed, the process fails back using VMware Tools. Because the VMware Tools method creates and deletes an ESXi-based snapshot whenever it creates a native Nutanix snapshot, it generates more I/O stress. To eliminate this stress, we strongly recommend installing NGT. 

Best practices for implementing VSS snapshots: 

- Schedule application-consistent snapshots during off-peak hours. 

NGT takes less time to quiesce applications than VMware Tools, but application-consistent snapshots still take longer than crash-consistent snapshots. 

- Increase cluster heartbeat settings when using Windows Server Failover Cluster (WSFC). 

© 2025 Nutanix, Inc. All rights reserved  | **35** 

Data Protection and Disaster Recovery Best Practices 

- To avoid accidental cluster failover when you perform a vMotion, follow VMware best practices to increase heartbeat probes: 

   - › Change the tolerance of missed heartbeats from the default of 5 to 10. Increase the number to 20 if your servers are on different subnets. 

   - › If you run Windows Server 2012, adjust the RouteHistoryLength to double the CrossSubnetThreshold value. 

Microsoft failover settings adjusted for using Nutanix VSS: 

```
(get-cluster).SameSubnetThreshold = 10
(get-cluster).CrossSubnetThreshold = 20
(get-cluster).RouteHistoryLength = 40
```

Hyper-V on Nutanix supports VSS only through third-party backup applications, not snapshots. Because the Microsoft VSS framework requires a full share backup for every virtual disk in the share, Nutanix recommends limiting the number of VMs on any container using VSS backup. 

Best practices for VSS on Hyper-V: 

- Create different containers for VMs that need VSS backup support. 

- Don't exceed 50 VMs on each container. 

- Create a separate large container for crash-consistent VMs. 

© 2025 Nutanix, Inc. All rights reserved  | **36** 

Data Protection and Disaster Recovery Best Practices 

## 7. Remote Site Backup and Disaster Recovery 

With Nutanix you can set up remote sites, and you can choose to use those remote sites for simple backup or for both backup and disaster recovery. 

Remote sites are a logical construct. You must configure any AOS cluster—either physical or based in the cloud—as a remote site from the perspective of the source cluster before you use it as the destination for storing snapshots. Similarly, on this secondary cluster, you must configure the primary cluster as a remote site before snapshots from the secondary cluster start replicating to it. 

Configuring the backup option on Nutanix allows you to use its remote site as a replication target. This option means you can back up data to this site and retrieve snapshots from it to restore locally, but failover protection (that is, running failover VMs directly from the remote site) isn't enabled. Backup supports using multiple hypervisors; as an example, an enterprise might have ESXi in the main datacenter but use Hyper-V at a remote location. With the backup option configured, the Hyper-V cluster could use storage on the ESXi cluster for backup. Using this method, Nutanix can also back up to AWS from Hyper-V or ESXi. You also use the backup option when you back up to Azure. 

Configuring the disaster recovery option allows you to use the remote site for both backup and disaster recovery. In this arrangement, failover VMs can run directly from the remote site. The disaster recovery option requires that both sites either support cross-hypervisor disaster recovery or have the same hypervisor. Nutanix provides cross-hypervisor disaster recovery between ESXi and AHV clusters. Currently, Hyper-V clusters can only provide disaster recovery to other Hyper-V-based clusters. 

AOS supports network mapping for disaster recovery migrations moving to and from AHV. Whenever you delete or change the network attached to a VM specified in the network map, modify the network map accordingly. 

For data replication to succeed, configure forward (DNS A) and reverse (DNS PTR) DNS entries for each ESXi management host on the DNS servers used by the Nutanix cluster. 

You can customize several options when you set up a remote site, including the address, whether the proxy is enabled, the SSH tunnel, capabilities, bandwidth throttling, the 

© 2025 Nutanix, Inc. All rights reserved  | **37** 

Data Protection and Disaster Recovery Best Practices 

remote container, and network mapping. Protection domains inherit all remote site properties during replication. 

Max bandwidth is set to throttle traffic between sites when no network device can limit replication traffic. With the max bandwidth option, you can set different settings throughout the day and assign a max bandwidth policy when your sites are busy with production data. You can also disable the policy when the sites aren't as busy. Max bandwidth doesn't imply a maximum observed throughput. 

When you talk with your networking teams, note that this setting is in megabytes per second, not megabits per second. NearSync doesn't currently honor maximum bandwidth thresholds. 

## **Remote Site IP Address Best Practices** 

Use the external cluster IP address as the address for the remote site. The external cluster IP address is highly available, as it creates a virtual IP address for all the virtual storage controllers. You can configure the external cluster IP address in Nutanix Prism under cluster details. 

Best practices for setting the remote site IP address: 

- Keep both sites at the same AOS version. 

- If both sites require compression, both must have the compression feature licensed and enabled. 

- Open the following ports between both sides: 

   - › 2009 TCP 

   - › 2020 TCP 

   - › 9440 TCP 

   - › 53 UDP 

- If you use the SSH tunnel, also open 22. 

- Use the external cluster IP address for the source and destination. 

Cloud Connect uses a port between 3000—3099, but that setup occurs automatically. 

© 2025 Nutanix, Inc. All rights reserved  | **38** 

Data Protection and Disaster Recovery Best Practices 

- Allow all CVM IP addresses to pass replication traffic between sites with the ports listed. 

To simplify firewall rules, you can enable a remote site proxy to redirect all egress remote replication traffic through one node. This proxy is different from the Prism proxy. When you enable the proxy, replication traffic goes to the remote site proxy, which then forwards it to other nodes in the cluster. This arrangement significantly reduces the number of firewall rules you must set up and maintain. Use the proxy in conjunction with the external address. 

## **SSH Tunnel** 

An SSH tunnel is a point-to-point connection—one node in the primary cluster connects to a node in the remote cluster. By enabling the proxy option, you force replication traffic to go over this node pair. You can use the SSH tunnel between Cloud Connect and physical Nutanix clusters when you can't set up a virtual private network (VPN) between the two clusters. We recommend using an SSH tunnel as a fail-back option rather than a VPN. 

Best practices for using an SSH tunnel: 

- To use an SSH tunnel, enable the proxy. 

- Open port 22 between external cluster IP addresses. 

- Only use SSH tunnel for testing—not production. 

- Use a VPN between remote sites or a Virtual Private Cloud (VPC) with AWS. 

## **Remote Container Best Practices** 

VStore name mapping identifies the container on the remote cluster used as the replication target. When you establish the VStore mapping, we recommend that you create a new, separate remote container with no VMs running on it on the remote side. This configuration helps the hypervisor administrator recognize the failed-over VMs quickly and apply policies on the remote side easily in case of a failover. 

Best practices for remote containers: 

- Create a new remote container as the target for the VStore mapping. 

© 2025 Nutanix, Inc. All rights reserved  | **39** 

Data Protection and Disaster Recovery Best Practices 

- If you're backing up many clusters to one destination cluster and the source containers have similar advanced settings, use only one destination container. 

- Enable MapReduce compression if licensing permits. 

- If you use vCenter Server to manage both the primary and remote sites, don't use storage containers with the same name on both sites. 

If the aggregate incoming bandwidth required to maintain the current change rate is less than 500 Mbps, we recommend skipping the performance tier. This setting saves your flash for other workloads while also saving on SSD write endurance. Skip the performance tier from the Nutanix command-line interface (nCLI): 

```
ncli ctr edit sequential-io-priority-order=DAS-SATA,SSD-SATA,SSD-PCIe
 name=<container-name>
```

You can reverse this command at any time. 

## **Full Snapshot and Asynchronous Replication Scheduling** 

Set the snapshot schedule to be equal to your desired RPO. In practical terms, your RPO determines how much data you can afford to lose in the event of a failure. The failure might be due to a hardware malfunction, human error, or environmental issues. Taking a snapshot every 60 minutes for a server that changes infrequently or when you don't need a low RPO takes resources from more critical services. 

You set your RPO from the local site. If you set a schedule to take a snapshot every hour, bandwidth and available space at the remote site determine if you can achieve the RPO. In constrained environments, limited bandwidth might cause the replication to take longer than the one-hour RPO, increasing the RPO. You can size bandwidth and capacity to avoid this scenario. 

You can create multiple schedules for a protection domain using full snapshots, and you can have multiple protection domains. For example, you can take seven daily snapshots, four weekly snapshots, and three monthly snapshots to cover a three-month retention policy. This policy manages metadata on the cluster more efficiently than a daily snapshot with a 180-day retention policy. 

Best practices for scheduling snapshots: 

© 2025 Nutanix, Inc. All rights reserved  | **40** 

Data Protection and Disaster Recovery Best Practices 

- Stagger replication schedules across protection domains. If you have a protection domain starting at the top of the hour, stagger the protection domains by half of the most common RPO. The goal is to spread out replication impact on performance and bandwidth. 

- Configure snapshot schedules to retain the lowest number of snapshots while still meeting the retention policy. 

Remote snapshots implicitly expire based on how many snapshots exist and how frequently you take them. For example, if you take daily snapshots and keep a maximum of five, the first snapshot expires on the sixth day. At that point, you can't recover from the first snapshot because the system deletes it automatically. 

In case of a prolonged network outage, Nutanix always retains the last snapshot to ensure that you don't ever lose all the snapshots. You can modify the retention schedule from the nCLI by changing the `min-snap-retention-count` parameter. This value specifies the number of snapshots to retain, even if all the snapshots reach the expiry time. This setting works at the protection-domain level. 

## **Lightweight Snapshot and NearSync Scheduling** 

Nutanix offers NearSync replication with a telescopic schedule (time-based retention). When you set the RPO between 1 and 15 minutes, you can save your snapshots for n number of weeks or months. After you select NearSync, you can't add any more schedules. 

The following table presents the default telescopic schedule to save recovery points for one month. 

_Table: Default Telescopic Schedule for One Month_ 

|**Type**|**Frequency**|**Retention**|
|---|---|---|
|Minute increments|Every minute|15 minutes|
|Hourly|Every hour|6 hours|
|Daily|Every 24 hours|7 days|
|Weekly|Every week|4 weeks|
|Monthly|Every month|1 month|



© 2025 Nutanix, Inc. All rights reserved  | **41** 

Data Protection and Disaster Recovery Best Practices 

For more information on the latest requirements and limitations, see Requirements of Data Protection with NearSync Replication. 

Limit the number of VMs to 10 or fewer per protection domain. If you can, maintain one VM per protection domain to help you transition back to NearSync if you run out of LWS reserve storage. 

## **Cross-Hypervisor Disaster Recovery Best Practices** 

Nutanix provides cross-hypervisor disaster recovery for migrating between ESXi and AHV clusters. The migration works with one click and uses the Prism data protection workflow. After you install the mobility drivers through NGT, VMs can move freely between the hypervisors. 

Best practices for cross-hypervisor disaster recovery: 

- Configure the CVM external IP address. 

- Obtain the mobility driver from NGT. 

- Don't migrate VMs with delta disks (hypervisor-based snapshots) using SATA disks. 

- Ensure that protected VMs have an empty IDE CD-ROM attached. 

- Ensure that network mapping is complete. 

Cross-hypervisor disaster recovery has the following limitations: 

- Doesn't preserve SCSI controllers (Nutanix uses LSI logic on failback) 

- No support for UEFI boot 

- Doesn't preserve vSphere High Availability (HA) and Distributed Resource Scheduler (DRS) settings 

- No support for hypervisor-based snapshots, VMware VSS, or linked clones 

## **Single-Node Backup Target Best Practices** 

Nutanix offers the NX-1155 or NX-1175 appliance as a single-node backup target for an existing Nutanix cluster. Because this target has different resources than the original 

© 2025 Nutanix, Inc. All rights reserved  | **42** 

Data Protection and Disaster Recovery Best Practices 

cluster, its primary use case is to provide backup for a small set of VMs. This utility gives a fully integrated backup option to small and midsize businesses for ROBO protection. 

Best practices for using a single-node backup target: 

- Limit all protection domains combined to fewer than 30 VMs. 

- Limit backup retention to a three-month policy. 

We recommend a policy that includes seven daily, four weekly, and three monthly backups. 

- Only map an NX-1155 or NX-1175 to one physical cluster. 

- Set the snapshot schedule to at least six hours. 

- Turn off deduplication. 

© 2025 Nutanix, Inc. All rights reserved  | **43** 

Data Protection and Disaster Recovery Best Practices 

## 8. Disaster Recovery Orchestration 

With Prism Central, you can monitor and manage multiple clusters. Supported AOS versions provide protection policies and recovery plans in Prism Central, offering a way to orchestrate operations around disaster avoidance (planned failover), disaster recovery (unplanned failures), migrations, and testing. You can use categories with protection policies and recovery plans to apply orchestration policies for ESXi and AHV from a central location, ensuring consistency across all your sites and clusters. 

To help provide management flexibility for these protection policies and recovery plans, Nutanix uses a construct called availability zones. When on-premises, an availability zone includes all the Nutanix clusters managed by one Prism Central instance. An availability zone can also represent a region in the public cloud. For disaster recovery, availability zones exist in pairs—on-premises to on-premises, on-premises to cloud using NC2, or on-premises to managed service providers. 

The following best practices are for Nutanix Disaster Recovery, which uses policy-based constructs in Prism Central. For more information, see the Nutanix Disaster Recovery Guide. 

Ensure that you have the following infrastructure in place before deploying Nutanix Disaster Recovery: 

- Deploy Prism Central. 

   - › For on-premises environments, deploy two Prism Central instances or one Prism Central instance at a separate site for high availability. 

   - › Nutanix Disaster Recovery for ROBO sites only requires one Prism Central, at the main site. When you replicate between two clusters managed from the same Prism Central instance, the minimum RPO is one hour or more and cross-hypervisor disaster recovery isn't supported. 

- Deploy Prism Central on a subnet that doesn't fail over. 

- Place the CVM and hypervisor IP addresses on a separate subnet from those used by VMs. 

© 2025 Nutanix, Inc. All rights reserved  | **44** 

Data Protection and Disaster Recovery Best Practices 

- The test network for on-premises disaster recovery orchestration requires a nonroutable VLAN. 

## **Paired Availability Zones** 

Paired availability zones synchronize the following disaster recovery configuration entities: 

- Protection policies 

- Recovery plans 

- Categories used in protection policies and recovery plans 

Issues (like network connectivity loss between paired availability zones) or user actions (such as unpairing availability zones, and then pairing those availability zones again) can affect entity synchronization. Pairing previously unpaired availability zones triggers an automatic synchronization event. 

If you don't update entities before you resolve a connectivity issue or pair the availability zones again, the synchronization behavior resumes. If you update entities in either or both availability zones before you resolve such issues or pair unpaired availability zones again, you can't synchronize the entities. In such a scenario, you can force the entities in one availability zone to synchronize with the paired availability zone. This forced synchronization overwrites entities at the paired availability zone. 

Observe the following recommendations to avoid inconsistencies and the resulting synchronization issues: 

- During network connectivity issues, don't update an entity at both availability zones in a pair. You can safely make updates at any one location. After the connectivity issue is resolved, force synchronization from the updated availability zone. Failure to adhere to this recommendation results in synchronization failures. 

- You can safely create new entities in either or both availability zones if you don't assign the same name to entities in both availability zones. After you resolve the connectivity issue, force synchronization from the availability zone where you created the entities. 

© 2025 Nutanix, Inc. All rights reserved  | **45** 

Data Protection and Disaster Recovery Best Practices 

- If one of the availability zones becomes unavailable or if a service in the paired availability zone is down, force synchronization from the paired availability zone after you resolve the issue. 

## **Protection Policies for Nutanix Disaster Recovery** 

A protection policy automates the creation and replication of snapshots. When you configure a protection policy for creating local snapshots, you specify the RPO, retention policy, and the entities to protect. To automate snapshot replication to a remote location, you can also specify the remote location. Nutanix Disaster Recovery uses entity-centric disaster recovery. 

Requirements for using protection policies: 

- A VM can only belong to a protection domain or a protection policy, not both. 

- If you don't use Nutanix AHV IPAM and need to retain your IP addresses, install NGT on the VMs you must protect. 

- You can associate a VM with a maximum of one protection policy. 

Best practices for using protection policies: 

- Apply protection policies using categories. 

- For VMs that haven't been replicated before, create a container with the same name on both sides. If the VM was in a protection domain before, it continues to use the same container. 

- All protection policies that use NearSync must have fewer than 200 VMs. 

## **Cross-Hypervisor Disaster Recovery for Nutanix Disaster Recovery** 

AOS supports cross-hypervisor disaster recovery. Cross-hypervisor disaster recovery preserves the following elements of the ESXi configuration on failback when your onpremises environment is healthy again: 

- Port group (based on network mapping) 

- Virtual hardware version 

- Network adapter types 

© 2025 Nutanix, Inc. All rights reserved  | **46** 

Data Protection and Disaster Recovery Best Practices 

- Disk adapter type 

- VMware Tools state 

- Amount of vCPU 

- Number of cores per vCPU 

- Static MAC address 

- Disk provisioning (thick or thin) 

- OS type 

Cross-hypervisor disaster recovery has the following limitations: 

- Doesn’t preserve SCSI controllers (Nutanix uses LSI logic on failback) 

- No support for UEFI boot 

- Doesn’t preserve vSphere High Availability (HA) and Distributed Resource Scheduler (DRS) settings 

- No support for hypervisor-based snapshots, VMware VSS, or linked clones 

## **Recovery Plans for Nutanix Disaster Recovery** 

A recovery plan orchestrates restoring protected VMs at a backup location. Recovery plans can recover all specified VMs at once or, using what is essentially a runbook functionality, use power-on sequences with optionally configurable interstage delays to recover applications gracefully and in the required order. 

Recovery plans also allow protected VMs to run custom embedded scripts. With NGT, you can use a custom script to perform a variety of customizations, like changing desktop wallpaper or even updating existing management software after the failover. On failover, the recovery plan provides a variety of options to change or maintain VM IP addresses. The plan shows you the last four digits of the VM's new IP address beforehand so you can take action on it. 

Ensure that you meet the requirements for recovery plans: 

- To enable disaster recovery orchestration, you must set up a Prism Element external data services IP address. 

© 2025 Nutanix, Inc. All rights reserved  | **47** 

Data Protection and Disaster Recovery Best Practices 

- Prism Central must run on a Nutanix cluster with the external data services IP address. 

**Note:** If you add a VM to multiple recovery plans and perform failover simultaneously on those recovery plans, each recovery plan creates an instance of the VM at the recovery location. You must manually clean up the additional instances. 

Create your recovery plans based on the following best practices: 

- For on-premises availability zones, create a nonroutable network for testing failovers. 

- Run the Validate workflow after you make changes to recovery plans. 

- After you run the Test workflow, run the Clean-Up workflow instead of manually deleting VMs. 

- Keep asynchronous or NearSync replication VMs in separate recovery plans from synchronous replication VMs. Synchronous recovery plans don't support planned failover. 

For more information on the configuration maximums for Nutanix Disaster Recovery in multiple scenarios, see Nutanix Configuration Maximums. 

## **Recovery Plan Networking** 

Virtual networks in on-premises Nutanix clusters are virtual subnets bound to a single VLAN. At physical locations, including the recovery location, you must create these virtual subnets manually, with separate virtual subnets created for production and test purposes. You must create these virtual subnets before you configure recovery plans. 

When you configure a recovery plan, map the virtual subnets at the source location to the virtual subnets at the recovery location. 

Offset-based IP address mapping tracks the last octet of VM IP addresses so that you can automatically maintain or change them based on the subnet configuration in the recovery plan. If you keep the source and destination subnets the same, you can maintain the IP address. If you change the destination subnet to a new value, the new subnet keeps the last four digits. 

To manually assign static IP addresses to your VMs on failover, you can use custom IP address mapping to do so as part of the recovery plan. 

© 2025 Nutanix, Inc. All rights reserved  | **48** 

Data Protection and Disaster Recovery Best Practices 

Recovery plans created at one location synchronize with the paired location and work bidirectionally. After VMs fail over from the primary location to the recovery location, you can use the same recovery plan to return the VMs to the original primary location using a failback mechanism. After you create a recovery plan, validate and test it to make sure the system can recover smoothly if it needs to fail over. 

© 2025 Nutanix, Inc. All rights reserved  | **49** 

Data Protection and Disaster Recovery Best Practices 

## 9. Self-Service File Restore 

Self-service file restore is available for ESXi and AHV. The goal is to offer self-service file-level restore with minimal support from infrastructure administrators. After you install NGT, you can open a web browser and go to `http://localhost:5000` to browse snapshots sorted by today, last week, last month, and user-defined criteria. Self-service works with both protection domains and Nutanix Disaster Recovery. 

You must meet the following requirements: 

- Use a compatible OS for guest VMs. For more information, see the Nutanix Guest Tools OS Compatibility and Interoperability Matrix (Nutanix credentials required). 

- Remove JRE 1.8 or later if installed with a previous release. 

- Configure a cluster external IP address. 

- Add the VM to a protection domain. 

- Use the default disk.EnableUUID = true for the VM in advanced settings for ESXi. 

- Configure the VM with a SATA-based CD-ROM for installation. For more information, see the Nutanix Guest Tools Requirements section of the Prism Central Infrastructure Guide. 

- Detach the mounted disk after you restore your files. 

**Note:** For Linux VMs, logical volumes spanning multiple disks aren't supported. 

© 2025 Nutanix, Inc. All rights reserved  | **50** 

Data Protection and Disaster Recovery Best Practices 

## 10. Third-Party Backup Products 

Nutanix provides its own hypervisor-agnostic, changed-region tracking API that vendors can access using a REST API. Currently, Arcserve, Cohesity, Commvault, Druva, HYCU, Rubrik, Veeam, and Veritas NetBackup provide backup support specifically for AHV. Nutanix systems can integrate with any backup vendor that supports vStorage APIs for Data Protection for ESXi. Nutanix also offers a VSS provider for Hyper-V backup software vendors. For more information on certified third-party solutions, see the Nutanix Elevate Technology Alliance Partner Program page. For a complete list of all supported backup software, see the Nutanix Compatibility and Interoperability Matrix for Data Protection. 

Hypervisor-based snapshots left on the VM when you take a hardware-based snapshot might cause the recovery to fail. You must manually turn on the VM. For more information, see VMware KB 1025279. 

Backup software interacting with AHV uses scoped production domains, also known as backup snapshots. When the backup software requests a snapshot of a VM that isn't protected under any user-created production domain, temporarily protect the VM by placing it in a scoped production domain. Issue a snapshot of this production domain, then remove the VM from the production domain. With backup snapshots, you can also back up VMs and use disaster recovery orchestration. The only possible issue is that while the snapshot operation on the scoped production domain is in progress, the userinitiated VM protection might fail in the regular production domain. 

Best practices for using backups with replication: 

- Avoid using hypervisor-based snapshots. 

- Try to schedule hypervisor snapshots or user-created, hardware-based snapshots at times outside of the backup window. 

For optimization and scaling recommendations for third-party backup solutions, see the best practice guides on an enhanced disk-to-disk backup architecture on the Nutanix Support Portal. 

© 2025 Nutanix, Inc. All rights reserved  | **51** 

Data Protection and Disaster Recovery Best Practices 

## 11. Metro Availability for AHV and ESXi 

Metro Availability works in conjunction with existing Nutanix data management features, including compression, erasure encoding, deduplication, and tiering. Metro Availability can enable compression over the network for synchronous replication traffic between sites, reducing the total bandwidth required. For AHV, the control path and management plane are in Prism Central. 

Failover between sites can be manual or automatic with the use of a witness. While the connection between Site 1 and Site 2 must have a round-trip time (RTT) of 5 milliseconds or less, the witness between sites can have an RTT of up to 200 ms. The witness is a VM deployed in a separate failure domain. The witness uses only a small amount of resources and must reside on a different network than Site 1 and Site 2. The witness helps protect against network partitions and primary and secondary site failures. 

Metro Availability with AHV is a continuous availability solution that synchronously replicates data at VM granularity ensuring that a real-time copy exists on the remote site. Synchronous storage replication at VM granularity across independent Nutanix clusters supports the continuous availability of the VM using the protection policy and recovery plan constructs. During a disaster or planned maintenance, VMs can fail over from a primary site to a secondary site, guaranteeing nearly 100 percent uptime for applications. 

AHV Metro Availability currently supports a one-to-one relationship between sites for synchronous replication but can also be replicated to a third site using asynchronous, or near-synchronous replication. This provides an RPO of 1-15 minutes or 1 hour or more to the third site. 

ESXi Metro Availability is also a continuous availability solution. It provides a unified namespace across a container stretched between Nutanix clusters. Synchronous storage replication across independent Nutanix clusters supports the stretched container using the protection domain and remote site constructs. You enable synchronous replication at the container level, and all VMs and files stored in that container replicate concurrently to another Nutanix cluster. 

Each protection domain maps to one container. You can create multiple protection domains to enable different policies, including bidirectional replication, in which each 

© 2025 Nutanix, Inc. All rights reserved  | **52** 

Data Protection and Disaster Recovery Best Practices 

Nutanix cluster replicates synchronously to another. Metro Availability currently supports a one-to-one relationship between protection domains, meaning that each container can replicate to one other remote site. Containers have two primary roles while enabled for Metro Availability: active and standby. Active containers replicate data synchronously to standby containers. 

© 2025 Nutanix, Inc. All rights reserved  | **53** 

Data Protection and Disaster Recovery Best Practices 

## 12. Instant Recovery Data Protection for AHVBased VMs 

The instant recovery option provides a way to rapidly return to an exact time and state for both VMs and Nutanix volume groups. AHV has two built-in methods for quickly bringing back VM data: 

- On-demand VM instant recovery 

- Protection domain or entity-centric VM instant recovery 

A VM can belong to only one protection domain. Because a protection domain can replicate to remote Nutanix clusters, its name must be unique across the system. Use Prism Central to establish entity-centric workflows, which exist at the vDisk level instead of the container level. 

Nutanix AOS manages the instant recovery protection option by VM, volume group, multiple VMs, multiple volume groups, or a mix of both VMs and volume groups. 

You can manage these instant recovery methods using Prism Element, Prism Central, REST API, or the CLI, and both methods use crash-consistent snapshots. 

Because the system stores the data used to provide the instant recovery option in the same physical infrastructure that hosts the VMs and volume groups themselves, the instant recovery option isn't a valid fully functional backup and recovery solution. 

## **On-Demand VM Instant Recovery Option** 

With the on-demand option, you can take VM snapshots, restore snapshots, and clone a new VM from an existing snapshot. We recommend taking a snapshot before starting potentially sensitive administrative tasks or cloning an existing VM. 

In Prism Element, you can manage on-demand VM instant recovery from the **VM** > **Table** view. For more information, see Creating a VM Snapshot Manually. 

In Prism Central, you can manage on-demand VM instant recovery by selecting the VM view and then selecting the VM. 

© 2025 Nutanix, Inc. All rights reserved  | **54** 

Data Protection and Disaster Recovery Best Practices 

To use the on-demand VM recovery point, select the VM from the **VM** > **Table** view and click **VM Snapshots** . 

The VM Snapshots tab provides the following options for each VM snapshot: 

- Details: Provide details about the VM configuration. 

   - › Snapshot creation time 

   - › vCPU 

   - › Number of cores per vCPU 

   - › Memory 

   - › Disks 

   - › Volume groups 

   - › Network adapters (NICs) 

- Clone: Create new VMs based on the snapshot. 

   - › Number of clones (default is one) 

**Note:** If you create more than one clone, you can select a starting index number. 

- › Name (default is [VM name]-1) 

- › vCPU 

- › Number of cores per vCPU 

- › Memory 

- › Network adapters (NICs) 

**Note:** You can't configure disks and volume groups during the clone operation. 

- Restore: Restore the VM to the snapshot state. 

- Delete: Delete the snapshot and merge all changes into the original VM disks. 

© 2025 Nutanix, Inc. All rights reserved  | **55** 

Data Protection and Disaster Recovery Best Practices 

## **Protection Domain VM Data Instant Recovery Option** 

We continue to improve snapshots by incorporating LWS to provide nearly synchronous replication (NearSync). The LWS feature can achieve an RPO between 15 minutes and 20 seconds by using markers instead of creating full snapshots. If the system can't fulfill the low RPO, the system automatically switches to the vDisk snapshot approach and then returns to LWS when possible. 

In Nutanix Prism, you can manage the protection domain instant recovery option by selecting **Async DR** from **Data Protection** > **Table** . For more information on creating a protection domain, see Configuring Data Protection with Asynchronous Replication. 

To use the protection VM and volume group data, choose one of the following options: 

- Overwrite an existing VM or volume group. 

- Create a new entity. 

For more information on recovering from the snapshot, see Restoring an Entity from a Protection Domain. 

© 2025 Nutanix, Inc. All rights reserved  | **56** 

Data Protection and Disaster Recovery Best Practices 

## 13. Nutanix Data Protection and Disaster Recovery Best Practices 

These best practices cover the following areas of data protection and disaster recovery on Nutanix: 

- Storage 

- Volume Shadow Copy Service (VSS) 

- Protection domain–based disaster recovery 

- Disaster recovery orchestration 

- Self-service file restores 

Follow these best practices for storage: 

- Store all VM files on Nutanix storage. If non-Nutanix storage stores files externally, it must have the same file path on both sides. 

- Remove all external devices, including ISOs or floppy devices. 

Follow these best practices for VSS: 

© 2025 Nutanix, Inc. All rights reserved  | **57** 

Data Protection and Disaster Recovery Best Practices 

- Nutanix native VSS snapshots: 

   - › Configure an external cluster IP address. 

   - › If you use ESXi, guest VMs must be able to reach the external cluster IP address on port 2074. Guest VMs running on AHV use the serial port to communicate. 

   - › Guest VMs must have an empty IDE CD-ROM for attaching NGT. 

   - › Guest VMs must use ESXi or AHV. 

   - › Virtual disks must use the SCSI bus type. 

   - › VSS must be running in the guest VM. 

   - › The guest VM needs to support the use of VSS writers. 

   - › Schedule application-consistent snapshots during off-peak hours or ensure that additional I/O performance is available. If you take a VSS snapshot during peak usage, the delta disk from the hypervisor-based snapshot might become large. When you delete the hypervisor-based snapshot, collapsing it takes additional I/O; account for this additional I/O to avoid affecting performance. 

- Hyper-V VSS provider: 

   - › Only use VSS support for backup. 

   - › Create different containers for VMs that need VSS backup support. Limit the number of VMs on each container to 50. 

   - › Create a separate large container for crash-consistent VMs. 

Follow these best practices for protection domain–based disaster recovery: 

- Give protection domains unique names across sites. 

- Group VMs with similar RPO requirements. 

- VMware Site Recovery Manager and Metro Availability protection domains are limited to 50 VMs. 

For current limits, see the Nutanix Configuration Maximums page. 

- Linked clone VMs (typically nonpersistent View desktops) aren't supported with NearSync. 

© 2025 Nutanix, Inc. All rights reserved  | **58** 

Data Protection and Disaster Recovery Best Practices 

- Remove unused protection domains to reclaim space. 

- If you must activate a protection domain rather than migrate it, deactivate the old primary protection domain when the site comes back up. 

- Consistency groups: 

   - › Keep consistency groups as small as possible. 

   - › Keep dependent applications or service VMs in one consistency group to ensure that they are recovered in a consistent state (for example, App and DB). 

- Configure forward (DNS A) and reverse (DNS PTR) DNS entries for each ESXi management host on the DNS servers used by the Nutanix cluster. 

- Remote sites: 

   - › Use the external cluster IP address as the address for the remote site. 

   - › Use a disaster recovery proxy to limit firewall rules. 

   - › Use max bandwidth to limit replication traffic. 

   - › When activating protection domains, use intelligent placement for Hyper-V and DRS for ESXi clusters on the remote site. Intelligent placement evenly spreads out the VMs at start time during a failover. AOS starts VMs uniformly at start time. 

   - › If you use vCenter Server to manage both the primary and remote sites, don't use storage containers with the same name on both sites. 

- Remote containers: 

   - › Create a new remote container as the target for the VStore mapping. 

   - › When you back up many clusters to one destination cluster, use only one destination container if the source containers have similar advanced settings. 

   - › Enable MapReduce compression if licensing permits. 

   - › If the aggregate incoming bandwidth required to maintain the current change rate is less than 500 Mbps, skip the performance tier to save flash capacity and increase device longevity. 

© 2025 Nutanix, Inc. All rights reserved  | **59** 

Data Protection and Disaster Recovery Best Practices 

- Whenever you delete or change the network attached to a VM specified in the network map, modify the network map accordingly. 

- Scheduling: 

   - › To spread out replication impact on performance and bandwidth, stagger replication schedules across protection domains. 

   - › If you have a protection domain starting at the top of the hour, stagger the protection domains by half of the most common RPO. 

   - › Configure snapshot schedules to retain the smallest number of snapshots while still meeting the retention policy. 

- Cross-hypervisor disaster recovery: 

   - › Configure the CVM external IP address. 

   - › Obtain the mobility driver from NGT. 

   - › Don't migrate VMs with delta disks (hypervisor-based snapshots), using SATA disks, or using volume groups. 

   - › Ensure that protected VMs have an empty IDE CD-ROM attached. 

   - › Complete network mapping. 

- Single-node backups: 

   - › Limit all protection domains combined to fewer than 30 VMs. 

   - › Limit backup retention to a three-month policy. 

   - › Schedule seven daily, four weekly, and three monthly backups. 

   - › Only map an NX-1155 to one physical cluster. 

   - › Snapshot schedule must be at least six hours. 

   - › Turn off deduplication. 

Follow these best practices for disaster recovery orchestration: 

- Deploy Prism Central to each on-premises site. 

- Deploy Prism Central on a subnet that doesn't fail over. 

© 2025 Nutanix, Inc. All rights reserved  | **60** 

Data Protection and Disaster Recovery Best Practices 

- Place CVM and hypervisor IP addresses on a separate subnet from the subnets used by the VMs. 

- Use a nonroutable VLAN for the test network. 

- Follow the Nutanix Disaster Recovery configuration maximums. 

- If one of the availability zones becomes unavailable or if a service in the paired availability zone is down, resolve the issue and force synchronization from the paired availability zone. 

- Protection policies: 

   - › If you don't use Nutanix AHV IPAM and must retain your IP addresses, install NGT on the VMs to be protected. 

   - › Use categories to apply protection policies. 

   - › For Nutanix Disaster Recovery on-premises, create the same container name on both sides. 

If the container name doesn't match on both sides, data replicates by default to the SelfServiceContainer. 

- Recovery plans: 

   - › For on-premises availability zones, create a nonroutable network for testing failovers. 

   - › Run the Validate workflow after making changes to recovery plans. 

   - › After you run the Test workflow, run the Clean-Up workflow instead of manually deleting VMs. 

© 2025 Nutanix, Inc. All rights reserved  | **61** 

Data Protection and Disaster Recovery Best Practices 

- Network mapping: 

   - › Set up administrative distances on VLANs for subnets that completely fail over. 

      - If you don't set up administrative distances, shut down the VLAN on the source side after failover if the VPN connection is maintained between the two sites. 

      - If you're failing over to a new subnet, set up the subnet beforehand so you can test the routing. 

   - › Use the same prefix length for network mappings at the source and the destination. 

   - › If you don't use Nutanix IPAM, install the NGT software package to maintain a static address. 

   - › To maintain a static address for Linux VMs that don't use Nutanix IPAM, install the NetworkManager command-line tool (nmcli) version 0.9.10.0 or later on the VMs. 

   - › Use NetworkManager to manage the network for the Linux VMs. 

      - To enable NetworkManager on a Linux VM, set the value of the NM_CONTROLLED field to `yes` in the interface configuration file (for example, in CentOS, the file is `/etc/sysconfig/network-scripts/ifcfg-eth0` ). 

      - After you set the field, restart the network service on the VM. 

- Sizing: 

   - › Size local and remote snapshot storage using the application’s change rate. 

   - › Size the performance tier for hybrid clusters to accommodate incoming data or bypass the performance tier and write directly to disk. 

- Bandwidth: 

   - › Seed locally for replication if WAN bandwidth is limited. 

   - › Set a high initial retention time for the first replication when you seed. 

Follow these best practices for self-service file restores: 

© 2025 Nutanix, Inc. All rights reserved  | **62** 

Data Protection and Disaster Recovery Best Practices 

- Guest VMs must use the following versions: 

   - › Windows 2008, Windows 7, or later 

   - › CentOS 6.5 or later and 7.0 or later 

   - › Red Hat 6.5 or later and 7.0 or later 

   - › OEL 6.5 or later 

   - › SLES 11 or later 

- Install VMware tools. 

- Install JRE 1.8 or later. 

- Configure a cluster external IP address. 

- Add the VM to a protection domain. 

- Use the default `disk.EnableUUID = true` for the VM in advanced settings. 

- Configure a CD-ROM for the VM. 

- Detach the mounted disk after restoring your files. 

© 2025 Nutanix, Inc. All rights reserved  | **63** 

Data Protection and Disaster Recovery Best Practices 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2025 Nutanix, Inc. All rights reserved  | **64** 

Data Protection and Disaster Recovery Best Practices 

## **List of Figures** 

Figure 1: Scalable Replication....................................................................................................................... 9 Figure 2: Snapshot: Starting Point...............................................................................................................16 Figure 3: Snapshot: Initial Snapshot............................................................................................................16 Figure 4: Snapshot: First Write.................................................................................................................... 17 Figure 5: Snapshot: Partial Data Overwrite................................................................................................. 17 Figure 6: Snapshot: Second Snapshot........................................................................................................ 18 Figure 7: Snapshot: Write to Second Snapshot.......................................................................................... 19 Figure 8: Snapshot: Maintenance Tasks......................................................................................................20 Figure 9: One-to-One Replication Topology.................................................................................................25 Figure 10: Many-to-One Replication Topology.............................................................................................26 Figure 11: Public Cloud and Nutanix Objects as a Replication Destination.................................................27 Figure 12: NMST as a Secondary Disaster Recovery Target......................................................................28 Figure 13: NMST Full Disaster Recovery Setup..........................................................................................29 Figure 14: NMST Snapshot-Only Setup...................................................................................................... 30 Figure 15: NMST Many-to-One Setup......................................................................................................... 31 


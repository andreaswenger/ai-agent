# **Nutanix Files Storage** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Nutanix Files Storage 

## **Contents** 

**1. Executive Summary.................................................................................5 2. Nutanix Files Storage Architecture........................................................7** File Server Virtual Machine Architecture............................................................................................ 8 Nutanix Files Storage Exports and Shares...................................................................................... 14 Nutanix Files Storage Load Balancing and Scaling......................................................................... 22 Nutanix Files Storage High Availability.............................................................................................24 Active Directory and SMB Operations with Nutanix Files Storage...................................................27 Unified Namespaces with Nutanix Files Storage............................................................................. 33 NFSv4 Support in Nutanix Files Storage......................................................................................... 34 NFSv3 Support in Nutanix Files Storage......................................................................................... 36 Multiprotocol Shares in Nutanix Files Storage................................................................................. 37 Nutanix Files Storage User and Directory Quotas........................................................................... 40 Selective File Blocking......................................................................................................................41 Files Manager....................................................................................................................................41 Nutanix Data Lens and Nutanix Files Storage Smart Tier............................................................... 42 Hypervisor-Specific Support for Nutanix Files Storage.................................................................... 45 Nutanix Files Storage Kubernetes Integration..................................................................................46 

**3. Backup and Disaster Recovery with Nutanix Files Storage.............. 47** Nutanix Files Storage with Asynchronous Protection Domains and Consistency Groups................48 Nutanix Files Storage Cluster Migration, Failure, and Restoration...................................................48 Nutanix Files Storage Cloning.......................................................................................................... 49 Metro Availability for Nutanix Files Storage......................................................................................50 Nutanix Files Storage Smart Disaster Recovery..............................................................................51 Nutanix Files Storage Data Synchronization....................................................................................54 

**4. Nutanix Files Storage in the Public Cloud..........................................56** Nutanix Files Storage with Native AWS or Azure Services..............................................................57 **5. Third-Party Integration with Nutanix Files Storage............................ 59** 

Third-Party Antivirus Solutions for Nutanix Files Storage................................................................ 59 Third-Party File Operations Monitoring with Nutanix Files Storage..................................................61 Third-Party Backup Solutions for Nutanix Files Storage.................................................................. 62 

**6. Document Version History....................................................................65 About Nutanix.............................................................................................68 List of Figures.............................................................................................................................................69** 

Nutanix Files Storage 

## 1. Executive Summary 

The Nutanix Files Storage solution provides a software-defined, scale-out file storage repository for unstructured data, such as video surveillance, home directories, user profiles, departmental shares, application logs, backups, and archives. Files Storage is ideal for persistent storage that supports modern applications using container orchestration frameworks like Kubernetes. Files Storage also provides high-performance capabilities well-suited to the demands of the entire AI and machine learning life cycle. Files Storage handles data-intensive workloads by providing the high throughput and low latency necessary to feed massive datasets to GPUs for model training and inference. 

You can deploy Files Storage on an existing or standalone cluster. Unlike standalone network-attached storage (NAS) appliances, Files Storage consolidates VM and file storage, eliminating the need to create an infrastructure silo. You can manage Files Storage with the Nutanix Prism management system, alongside VM and Kubernetes services, which unifies and simplifies management. Integration with Active Directory enables support for quotas, access-based enumeration (ABE), and self-service restores with the Windows previous version feature. Files Storage also supports native remote replication and file server cloning, which lets you back up files off-site and run antivirus scans and machine learning without affecting production. 

Files Storage can run on a dedicated cluster or a cluster running user VMs. You can deploy Files Storage in the public cloud with Nutanix Cloud Clusters (NC2) services in Microsoft Azure or AWS. You can also run Files Storage in AWS or Azure using native services, decoupled from NC2. 

Files Storage is hypervisor agnostic and supports both ESXi and AHV. It includes native high availability and uses Nutanix storage for intracluster data resilience. Nutanix storage also provides data efficiency techniques such as erasure coding. 

Files Storage is complimented by Nutanix Data Lens, which gives you visibility and an active defense for your data. Data Lens includes full audit trails, anomaly detection, ransomware protection, data age analytics, permissions visibility, custom reporting, and more. Data Lens provides these capabilities for Nutanix Unified Storage and Amazon S3. Key topics: 

© 2026 Nutanix, Inc. All rights reserved  | **5** 

Nutanix Files Storage 

- Implementing and operating Files Storage in your datacenter 

- Overview of the Nutanix architecture with Files Storage 

- Load balancing of standard and distributed shares (SMB) and exports (NFS) 

- High availability 

- Data protection and replication 

- Quotas and permission management 

- Antivirus 

- Backup 

© 2026 Nutanix, Inc. All rights reserved  | **6** 

Nutanix Files Storage 

## 2. Nutanix Files Storage Architecture 

Files Storage provides Server Message Block (SMB) and Network File System (NFS) file services to clients. For on-premises and Nutanix Cloud Clusters deployments, Files Storage runs as a virtualized software layer on Nutanix Cloud Infrastructure (NCI), a software platform that combines compute, storage, and networking resources from multiple servers into a single pool and provides the virtualization and storage services for Files Storage. Files Storage instances are a set of file server VMs (FSVMs) that represent a unique namespace and management domain. Files Storage requires at least three FSVMs per instance, running on three nodes, to satisfy a quorum for high availability. 

Figure 1: Files Storage Architecture 

© 2026 Nutanix, Inc. All rights reserved  | **7** 

Nutanix Files Storage 

## **File Server Virtual Machine Architecture** 

The file server VM (FSVM) incorporates all the security and hardening that goes into the Nutanix Controller VM (CVM). All the FSVMs have the same minimum configuration: four vCPUs and 12 GB of memory. You can add vCPUs or memory to scale up and add FSVMs to the cluster to scale out. For each file server, the number of FSVMs must be less than or equal to the number of nodes in the Nutanix cluster; however, you can create multiple file server deployments, which can share nodes. Starting with Files Storage 5.0, each Files cluster can support up to 32 FSVMs. Single-FSVM deployments intended for one- and two-node Nutanix clusters are also supported and can be used in larger clusters. 

Files Storage supports SMB and NFS from the same file server and enables simultaneous SMB and NFS access to the same share and data, commonly referred to as multiprotocol support. SMB and NFS share some common libraries, allowing a modular approach. InsightsDB is a NoSQL database that maintains the statistics and alerts for Files Storage. Zookeeper is a centralized service that maintains configuration information, such as domain, share or export, and IP address information. The Minerva service talks to the local CVM and sends heartbeats to share health information and help with failover. 

Figure 2: Data Path Architecture of Nutanix Files Storage 

© 2026 Nutanix, Inc. All rights reserved  | **8** 

Nutanix Files Storage 

Each FSVM stores file server data on multiple file systems that store share-specific data. The individual file system offers the snapshot capability to provide the Windows previous version feature to clients. By using separate file systems for each share or export, Files Storage can scale to support billions of files in one cluster. 

Figure 3: FSVM Internal Communication on One Node 

The previous diagram shows one FSVM running on a node, but you can put multiple FSVMs on a node for multitenancy. 

## **File Server Virtual Machine Networking** 

The file server VM (FSVM) has two network interfaces: the storage interface and the client interface. The FSVM service that communicates with the Controller VMs (CVMs) uses the storage interface, which also provides access to Nutanix Volumes 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

Nutanix Files Storage 

Block Storage iSCSI vDisks in volume groups. The storage interface helps manage deployment, failover, and maintenance and enables control over one-click upgrades. Integration with the CVM lets the FSVM determine if a storage fault occurred and if so, whether you must take action. The FSVM service sends a heartbeat to its local CVM service each second, indicating its state. 

For maximum performance with the least network routing overhead, we recommend that you place the FSVM storage interface on the same network VLAN as the Nutanix CVM iSCSI network. By default, the iSCSI VLAN is the management interface (eth0 of the CVM). You can also create a segmented iSCSI network for isolation purposes. 

The client interface allows clients to connect to SMB shares and NFS exports hosted on an FSVM. A client can connect to any FSVM client network interface to access file data. If a different FSVM provides the data, the client connection automatically redirects to the correct FSVM interface. If an FSVM fails, the client network address for the failed FSVM moves to another to preserve data access. 

You can configure up to 10 client VLANs for a file server. You might require multiple client networks for various reasons, including the following: 

- Network isolation for different tenants for security purposes 

- More compute efficiency by using a single file server with multiple isolated client networks, instead of deploying multiple file servers 

- Routing traffic from multiple client networks to a single client network, which can lead to more complex routing and firewall management 

- Using multiple client networks to avoid IP addressing limits for an environment 

The first client network that you configure during file server deployment becomes the primary network. The primary network communicates with Active Directory. You can add client networks after the initial deployment and modify the primary network if needed. 

You can present common file shares across these different VLANs or you can restrict share access to specific VLANs based on your security requirements. 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

Nutanix Files Storage 

Figure 4: Multiple Client Networks with Common and Unique Shares 

For AHV, you can configure client networks across the VLANs of multiple virtual switches (vSwitches) on the same file server. The multi-vSwitch support requires at least two virtual switches, and each vSwitch must have at least one unmanaged network with VLAN ID = 0. To use multiple client networks, you must deploy the file server using Prism Element. 

## **File Server Virtual Machine Storage Architecture** 

Each file server VM (FSVM) uses three separate vDisks: 

- A 25 GB boot disk that contains the boot image 

- A 45 GB disk ( `/home/nutanix` ) that contains the logs and software state 

- A 45 GB disk for Cassandra data 

Each FSVM also has a volume group that helps maintain auditing events and persistent handles for SMB Transparent Failover. 

A Files Storage cluster is a single namespace that includes a collection of file systems used to support the SMB and NFS shares. The file systems support dynamic metadata, which enables you to store an unlimited number of files. The file system also supports 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

Nutanix Files Storage 

variable block length allocations up to the default size of 64 KB. The variable block length matches the size of the file; for example, a 1 KB file allocates 1 KB on the file system. Any file over 64 KB allocates in 64 KB increments. 

While the default size is 64 KB, you can specify the maximum allocation size to improve performance. For environments with more random access patterns, you can choose a maximum allocation size of 16 KB. For environments with sequential patterns, you can choose 1 MB. These file systems use Nutanix volume groups as storage. For more information on the random or sequential file system options, see Files Storage Performance on Nutanix (requires Nutanix credentials). 

Volume groups offer high availability for file systems and allow them to scale out. A volume group is a collection of logically related vDisks (or volumes) attached to the guest using iSCSI. Cluster Controller VMs (CVMs) balance the ownership of vDisks in a volume group. When an FSVM is down for maintenance or a fault occurs, one of the surviving FSVMs takes over volume group ownership and continues servicing requests. 

Volume groups can contain up to 17 vDisks. The volume groups include 2 metadata vDisks, a separate intent log (SLOG) vDisk for synchronous random writes, and up to 14 data vDisks. The initial file system pool created on the volume group uses seven drives and supports 40 TiB of data. The two metadata disks are pinned to SSD storage. 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

Nutanix Files Storage 

Figure 5: Initial Volume Group File System Pool 

After this file system pool reaches 80 percent space utilization, Files Storage automatically expands it using a scale-up and scale-out approach, which varies depending on the Files Storage version. Scaling up increases the file system pool size by allocating more space on the existing vDisks. Scaling out adds vDisks to the pool. 

The maximum volume group sizes by Files Storage version are as follows: 

- 2.x to 3.2: 40 TiB 

- 3.2 to 3.6: 140 TiB 

- 3.7 to 4.4: 280 TiB 

- 5.x: 1,008 TiB 

**Note:** You must create the file servers with Files Storage 3.0 or later to use the maximum size of 1,008 TiB. 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

Nutanix Files Storage 

## **Nutanix Files Storage Exports and Shares** 

SMB shares and NFS exports have two configurations: distributed and standard. 

- SMB shares: 

   - › Distributed (previously called home) 

   - › Standard (previously called general) 

- NFS exports: 

   - › Distributed (previously called sharded) 

   - › Standard (previously called nonsharded) 

A standard share is an SMB share or NFS export hosted by a single file server VM (FSVM) and volume group. Standard shares and exports can store files in the root of the share. 

**Note:** You can store files in the root of distributed NFS shares. Distributed SMB shares don't allow files in the root. 

A distributed share spreads the workload by distributing the hosting of top-level directories across all the FSVMs that make up the file server, which also simplifies administration. Files Storage uses InsightsDB to maintain the directory mapping for each responsible FSVM. FSVMs use distributed file system (DFS) referrals for SMB and junctions for NFSv4 to ensure that clients can connect to the correct top-level directories. 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

Nutanix Files Storage 

Figure 6: Distributed Directory Shares 

Standard shares and exports don't distribute top-level directories. A single file server always owns the files and subfolders for standard shares and exports. The following diagram illustrates three standard shares on the same file server. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

Nutanix Files Storage 

Figure 7: Three Standard Shares on the Same File Server 

## **Distributed Home Share Use Case** 

Distributed shares work well for workloads like home directories, user profiles, and applications that distribute data across folders at the same directory level. For home shares and exports, Files Storage automatically spreads the workload over multiple file server VMs (FSVMs) per user. If a user creates a share called `\\FS-1\DS1` that contains the top-level directories `\Folder1` , `\Folder2` , and `\Folder3` , those directories can be on FSVM-1, on FSVM-2, on FSVM-3, and so on. The FSVMs use a string hashing algorithm based on the directory names to distribute the top-level directories. 

Distributed shares and exports begin with five volume groups for each FSVM (for example, 15 for a three-node cluster). Files Storage distributes the volume groups to different FSVMs in the file server cluster. When you have a large number of users, multiple volume groups improve load balancing across the FSVMs. 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

Nutanix Files Storage 

This distribution can accommodate a large number of users or directories in a single share or export. The scaling limits of more traditional designs can force administrators to create multiple shares or exports in which, for example, one set of users whose last names begin with A through M run from one controller and users whose names begin with N through Z run from another controller. This design limitation leads to management overhead headaches and unnecessary Active Directory complexity. For these reasons, Files Storage must have one SMB distributed share for the entire cluster. If you need more than one home directory share, you can create additional shares. 

The top-level directories act as a reparse point—essentially a shortcut. Consequently, you must create directories at the root of the share for optimal load balancing. We recommend that you set permissions at the share or export root before you deploy user folders. This step allows newly created top-level directories to inherit permissions, so you don't have to adjust them after the fact using the Files Storage Microsoft Management Console (MMC) plug-in. 

## **Volume Group and Storage Container Architecture** 

Files Storage supports up to 10 volume groups per file server VM (FSVM). After you reach this limit, new shares and exports use existing volume groups of the same type. For example, if you deploy a distributed share (15 volume groups) and 15 standard shares (creating 15 additional volume groups) on a three-node physical cluster, each FSVM hosts 10 volume groups: 5 for the distributed share and 5 for the standard shares. In this situation, the next share created uses an existing volume group because each FSVM is serving the maximum of 10 volume groups. 

Every file server maps one-to-one to a storage container. Postprocess compression and erasure coding are on by default to save capacity. We don't recommend deduplication. Files Storage supports the fault tolerance (FT1, FT2) and replication factor supported by the underlying AOS storage cluster. 

Files Storage 5.1 and later versions support the one node, two drive (1N/2D) replication scheme, which allows up to two simultaneous drive failures or one node failure. You can combine 1N/2D with erasure coding and wider strip sizes (up to 12:2 with Files Storage 5.2) to achieve a more efficient storage configuration compared to replication factor 3. 

**Note:** Nutanix recommends using 1N/2D when a Files cluster contains 100 or more drives of 12 TB or more. 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

Nutanix Files Storage 

You can control inline compression on a share-by-share basis. When you create a share, compression is on by default. We recommend having inline compression at the share level and postprocess compression at the storage-container level. Depending on the version, Files Storage might enforce the container-level recommendations, so Nutanix recommends allowing Files to manage these settings unless otherwise noted. 

**Note:** The high write volume associated with containerized profiles such as Microsoft FSLogix might negate the space savings provided by erasure coding. Carefully consider CPU usage, cluster size and storage, potential space savings, and all other data in your Files Storage deployment before you turn on erasure coding for desktop virtualization or user profile management solutions. 

## **Mounting NFS Distributed Share Directories** 

Distributed share directories with NFSv4 introduce some unique behaviors. To balance performance across file server VMs (FSVMs), each top-level directory that you create becomes an automatically generated export. Files Storage mounts the export on demand when someone accesses the directory. Because these directories are exports rather than standard directories, it takes a few steps to remove a top-level directory. 

To mount new distributed top-level directories, follow these steps: 

**1.** In the NFS client, create a distributed share and mount it as `/projects` . 

```
# df /projects
Filesystem 1K-blocks Used Available Use% Mounted on
1.1.1.10:/projects 1073741824 839455744 234286080 79% /projects
```

See that mount on Linux. 

## **2.** Create project directories. 

```
mkdir /projects/project1
mkdir /projects/project2
```

**3.** Access the project directories. 

```
ls /projects/project1
ls /projects/project2
```

Accessing the directory, using `ls` in this case, mounts the automatically created top-level directory export. If you run `df` again you see two additional mount points. 

```
# df | grep project
1.1.1.10:/projects 1073741824 839455744 234286080 79% /projects
1.1.1.11:/projects/project1 1073741824 839455744 234286080 79% /projects/
project1
```

© 2026 Nutanix, Inc. All rights reserved  | **18** 

Nutanix Files Storage 

```
1.1.1.12:/projects/project2 1073741824 839455744 234286080 79% /projects/
project2
```

These additional mount points allow a different FSVM to serve each export. 

This behavior introduces additional steps when you delete a top-level directory. You can delete the project2 directory from any NFS client with the export mounted if you sign in as a user with the appropriate permissions. 

## **Deleting NFS Distributed Share Directories** 

To delete a distributed share export, follow these steps: 

**1.** Delete the contents of the `project2` share. 

```
rm -rf /projects/project2/*
```

**2.** Unmount the `project2` share. 

```
umount /projects/project2
```

**3.** Delete the `project2` directory. 

```
rmdir /projects/project2
```

When you delete the top-level directory, it becomes inaccessible to other clients mounting the export. Processes that access the export after you delete it receive a Stale File Handle error. 

Distributed shares with NFSv3 clients behave differently than those with NFSv4 clients. NFSv3 connections don't automatically mount the top-level directories of distributed shares. Because the top-level directories aren't submounted, you can rename and delete them without the additional steps required for NFSv4. 

## **Nested Shares for Nutanix Files Storage** 

Files Storage supports nested shares. Using a nested share, you can create a folder in an existing standard or distributed share and turn that folder into a directly accessible share. Nested shares are generally used to provide unique share-level permissions to folders in an existing file system. You can also use them to match existing environments for migration purposes. Files Storage versions 4.4 and later support up to 5,000 nested shares per file server. 

Both SMB and NFS protocols support nested shares. Nested shares inherit some attributes from the parent share, but you can modify other attributes. 

Nested share inherited attributes: 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

Nutanix Files Storage 

- Protocol type 

- Maximum size 

- Self-service restore 

- Quota policy 

Nested share modifiable attributes: 

- SMB access-based enumeration (ABE) 

- NFS authentication and access 

- NFS advanced settings 

You can create a nested share in the **Basics** tab of the **Create Share or Export** menu by providing its path in the **Share Path (Optional)** field. 

## **Connected Shares for Nutanix Files Storage** 

The connected shares function mounts shares and exports as subfolders in other shares and exports. You can create a folder at any level of the folder hierarchy and mount either a standard or distributed share into that folder. The parent share hosting the folder can be a standard, distributed, or nested share. 

Figure 8: Submounting a Distributed Share into a Standard Share 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

Nutanix Files Storage 

Submounting shares and exports provides several benefits: 

- You can use a distributed share at any level of a directory structure. Some applications have hardcoded folder paths where they store data or create directories. If you submount, you can establish a distributed share at the level that matches these application requirements. 

- A folder adopts the size limit of the submounted shares, which is one method to apply folder-level quotas. 

- A folder adopts the self-service restore setting for a submounted share, which enables folder-level snapshots. 

## **Write Once, Read Many Shares for Nutanix Files Storage** 

Write once, read many (WORM) shares offer retention policies to ensure that files aren't inadvertently deleted. WORM shares are provided by failing file operations, which can cause file modification or deletion. Not all applications work on a WORM share. Only applications that don't rely on operations that modify or delete files work normally. 

WORM shares automatically commit files into a WORM state, specified by configuring a cool-off interval for the share. When a regular file in the share hasn't been changed —that is, the change time (or ctime) remains unchanged—for the duration of the cooloff interval, it transitions into a locked state. From the locked state, the file goes into retention, also called commit. The file remains in retention until the end of its retention period, if one is specified; otherwise, the file is retained for the share's default retention period. When the retention period ends, the file transitions into an expired state. The purpose of the expired state is to facilitate the file's deletion. 

You can only enable share-level retention (WORM) while creating a new share. 

WORM shares created with compliance mode block privileged users from deleting shares in the retention period. You can also set a legal hold option protecting expired files from deletion. You cannot turn off compliance mode. 

## **Nutanix Files Storage Directory Layout** 

Files Storage can store millions of files in a single share and billions of files across a multinode cluster with multiple shares. To achieve good response times for environments with high file and directory counts, think about directory design. Placing millions of 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

Nutanix Files Storage 

files or directories in a single directory slows file enumeration, which might affect some applications. 

For performance-sensitive applications, we recommend that you limit directory width— the number of files or directories in the root of a folder—to 100,000 objects. Increasing file server VM (FSVM) memory to cache metadata can help improve performance for environments with high object counts where directory enumeration is common. 

We also recommend that you limit the number of top-level directories in distributed shares. The recommended number of top-level directories depends on the memory assigned to the FSVMs in the cluster. Don't permit your directory count to exceed 3,000 times the FSVM memory of one node. For example, for a three-node cluster with 12 GB of memory assigned to each FSVM, the maximum top-level directory count is 3,000 × 12 = 36,000. 

## **Nutanix Files Storage Load Balancing and Scaling** 

Initial load balancing for a Files Storage cluster is based on the number of standard shares or the number of top-level directories you have in a distributed share. A standard share resides on a single volume group owned by one file server VM (FSVM) at a time. A distributed share is a collection of volume groups containing top-level directories. If some standard shares or top-level directories encounter a large workload and the volume groups supporting those workloads share an FSVM, a performance bottleneck can occur. Files Storage helps load-balance workloads automatically when it detects potential performance problems by redistributing volume groups to different FSVMs for better load balancing across nodes. 

Load balancing can occur in the following situations: 

- An administrator removes an FSVM from the cluster. 

- The distribution of shares or top-level directories becomes poorly balanced during normal operation because of changing client usage patterns or suboptimal initial placement. 

- Increased user demand necessitates adding a new FSVM and its volume groups are initially empty. 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

Nutanix Files Storage 

Files Storage addresses the second and third situations by maintaining usage statistics and patterns to detect per-FSVM load (in terms of CPU and memory utilization) and per–volume group load (in terms of user connections and latency of operations). Files Storage uses these statistics to make a load balancing recommendation, but the administrator must accept the recommendation before Files Storage carries out the action. Nutanix calls this feature one-click performance optimization. 

When Files Storage recommends a performance optimization, it provides the following choices: 

- Scale up: Improve performance by adding vCPU and memory to the existing FSVMs. 

- Rebalance: Eliminate performance bottlenecks by redistributing the workload across the existing FSVMs. 

- Scale out: Improve performance by adding new FSVMs to the existing file server. 

Load balancing through volume group redistribution doesn't always improve performance. For example, if clients target a low-level share directory that can't be further distributed among FSVMs, performance doesn't improve. In such cases, Files Storage supports scaling up by adding vCPU and memory to the FSVMs. Scaling up is seamless to users. We recommend a scale-up operation for performance optimization if SMB connection limits reach 95 percent utilization over a two-hour time window. 

A brief outage can happen during volume group migration and FSVM scale-out if you don't use shares with continuous availability enabled. The file share or export requires a client reconnect after migration and scaling out. Most clients try to reconnect for 50–60 seconds, which limits the overall impact. 

Load balancing occurs for each vDisk in a volume group. Volumes Block Storage requires the administrator to configure an iSCSI data service IP address. The data service IP address is a highly available virtual IP address used as an iSCSI discovery portal. Each vDisk in a volume group represents its own iSCSI target that any Controller VM (CVM) in the cluster can own. Volumes Block Storage uses iSCSI redirection to place and automatically load balance these sessions as needed with a feature called the Acropolis Dynamic Scheduler (ADS). 

For more information, see Nutanix Volumes Best Practices. 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

Nutanix Files Storage 

## **Nutanix Files Storage High Availability** 

Nutanix designed Files Storage to recover from a range of service disruptions, including when a local Controller VM (CVM) or file server VM (FSVM) restarts or fails. If a CVM goes offline because of failure or planned maintenance, any active sessions against that CVM are disconnected, triggering the iSCSI client to sign in again. The new authentication occurs through the external data services IP address, which redirects the session to a healthy CVM. 

Figure 9: High Availability for File Server Volume Groups 

When the failed CVM returns to operation, the iSCSI session fails back. In the case of a failback, the system signs the FSVM off and redirects it to the appropriate CVM. 

When a physical node fails completely, Files Storage uses leadership elections and the local Minerva CVM service to recover. The FSVM sends heartbeats to its local Minerva CVM service once per second, indicating its state. The Minerva CVM service keeps track of this information and can act during a failover. 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

Nutanix Files Storage 

In environments that have only one FSVM, Files Storage clusters rely on hypervisorbased high availability to maintain services. If the physical node that owns the FSVM fails, the FSVM restarts on another physical node. Client connections are disrupted until the FSVM restarts. 

To reinstate control, Files Storage follows this process: 

**1.** Stop SMB and NFS services. 

**2.** Disconnect the volume group. 

**3.** Release the IP address and share and export locks. 

**4.** Register the volume group with FSVM-1. 

**5.** Present new shares and exports to FSVM-1 with eth1. 

When an FSVM fails, the Minerva CVM service unlocks the files from the failed FSVM and releases the external address from eth1. The failed FSVM's resources then appear on a running FSVM. The internal Zookeeper instances store this information to send it to other FSVMs, if necessary. 

Figure 10: FSVM Ownership with Single Share and Volume Group Example 

When an FSVM is unavailable, the remaining FSVMs volunteer for ownership of the shares and exports associated with the failed FSVM. The FSVM that takes ownership 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

Nutanix Files Storage 

of the volume group informs the CVM that the volume group reservation has changed. If the FSVM that attempts to take control of the volume group is already the leader for a different volume group it has volunteered for, it relinquishes leadership for the new volume group immediately. This arrangement ensures distribution of volume groups even if multiple FSVMs fail. 

Figure 11: FSVM-1 Failure 

The Files Storage Zookeeper instance tracks the original FSVM's ownership using the storage IP address (eth0), which doesn't float from node to node. Because FSVM-1's client IP address from eth1 is now on FSVM-2, client connections persist. The volume group and its shares and exports are reregistered and locked to FSVM-2 until FSVM-1 can recover and a grace period elapses. 

The failover process typically takes between 20 and 180 seconds, and client operations can be affected during this time. SMB shares enabled for continuous availability and hard-mounted NFS shares experience I/O delays but maintain connections during highavailability events. 

If an FSVM is offline for 30 minutes or longer, the Files Storage cluster redistributes core cluster services (such as Zookeeper) owned by the offline node. The cluster needs at 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

Nutanix Files Storage 

least three online FSVMs to move services, which helps the Files Storage cluster rebuild itself smaller and survive subsequent node failures. 

When FSVM-1 comes back online and finds its shares and exports locked, it assumes that a high-availability event has occurred. After the grace period expires, FSVM-1 regains control of the volume group through the Minerva CVM service. When the failed FSVM comes back online, the cluster automatically switches back to the larger size. 

## **Active Directory and SMB Operations with Nutanix Files Storage** 

Files Storage SMB works with Active Directory. To deploy a cluster, you must have domain privileges to create the machine account and the DNS entries for DFS referrals. The file server doesn't store these credentials. 

Files Storage versions 3.6 and later simplify the permissions required to add domains when you create a file server. The following delegated domain user permissions are required: 

- Create computer objects. 

- Read servicePrincipalName. 

- Write servicePrincipalName. 

Files Storage can also create the required DNS entries automatically during file server creation. Automated DNS entry creation requires Microsoft Windows DNS and a user account with DNS admin permissions. 

The maximum number of client connections depends primarily on how much memory the FSVM has. The following table provides Nutanix configuration recommendations as of Files Storage version 5.0. 

_Table: Supported Active Client Connections_ 

|**FSVM Memory**|**Supported Client**|**Supported Client**|
|---|---|---|
||**Connections per FSVM**|**Connections for 4-**|
|||**FSVM File Server Cluster**|
|12 GB|500|2,000|
|16 GB|1,000|4,000|
|24 GB|1,500|6,000|



© 2026 Nutanix, Inc. All rights reserved  | **27** 

Nutanix Files Storage 

|**FSVM Memory**|**Supported Client**<br>**Connections per FSVM**<br>**Supported Client**<br>**Connections for 4-**<br>**FSVM File Server Cluster**|
|---|---|
|32 GB<br>40 GB<br>60 GB<br>64–512 GB|2,000<br>8,000<br>2,750<br>11,000<br>3,250<br>13,000<br>4,000<br>16,000|



You can continue deploying additional file server VMs (FSVMs) if you have free nodes; you can also deploy multiple file servers. 

Files Storage uses DFS referrals to direct clients to the FSVM that owns the targeted share or top-level directory. The following process takes place when a client sends a file access request. 

**1.** When the user accesses their files, a DNS request might be triggered to resolve the file server name. 

**2.** Using DNS round robin, DNS replies with an FSVM IP address. 

In this example, the IP address for FSVM-1 returns first. 

**3.** The client sends a create or open request to FSVM-1. 

**4.** The folder doesn't exist on this file server, so a STATUS_PATH_NOT_COVERED is returned. 

**5.** The client then requests a DFS referral for the folder. 

**6.** FSVM-1 looks up the correct mapping in the file server's Zookeeper and refers the client to FSVM-2. 

**7.** A DNS request goes out to resolve FSVM-2. 

**8.** The DNS request returns the IP address of FSVM-2. 

**9.** The client receives access to the correct folder. 

© 2026 Nutanix, Inc. All rights reserved  | **28** 

Nutanix Files Storage 

Figure 12: DNS Request for SMB 

## **Managing Nutanix Files Storage Shares** 

You can manage Files Storage shares the same way that you manage traditional file servers. For standard shares and top-level directories in distributed shares, you can assign permissions with native tools such as Windows Explorer. NTFS-level permissions, also called Windows access control lists (ACLs), manage all file and folder access. 

The distributed share has some special requirements because of how Nutanix uses DFS referrals. DFS referrals have a single namespace even though the data contained in the share can be spread out over many file server VMs (FSVMs). Typically, to delete a user's home directory, you select the top-level directory and delete the entire directory subtree. However, when using a distributed export or share, you can't just delete the top-level directory because it's a separate share created internally by Files Storage. Because you can't directly delete a top-level directory, removing it requires additional steps. 

You can choose between several options for renaming or deleting distributed share folders: 

- Identify which FSVM hosts the folder and rename or delete the folder directly using the FSVM as the UNC path. 

© 2026 Nutanix, Inc. All rights reserved  | **29** 

Nutanix Files Storage 

- Use the Files Storage MMC snap-in to manage top-level directories. Any file server administrator can perform the MMC operations; you don't need to assign privileges manually. Establish a connection to the Files Storage namespace and perform the following SMB share management tasks: 

   **1.** Create, delete, rename, and change permissions for top-level directories. 

   **2.** Change NTFS permissions for shares. 

- Files Storage 5.1 has a file server offload mechanism facilitated by a SmartOps share that lets you optimize large-scale file deletion operations. 

You can also delete top-level directory content on distributed shares without relying on the MMC snap-in for Files Storage. 

**Note:** Use the MMC snap-in for Files Storage when you modify NTFS permissions at the root of a distributed share. The MMC snap-in for Files Storage ensures that permissions propagate to the top-level directories based on inheritance. 

You can modify share-level permissions with the Windows-native Shared Folders MMC. Launch the Shared Folders MMC and point it to the Files Storage instance under the Another computer option. By default, all SMB shares have share-level ACLs set to Everyone with Full Control. Files Storage 4.4 also allows you to control share-level ACLs during share creation and editing. 

**Note:** We recommend leaving share permissions at Full Control and managing access with NTFS permissions. 

You can also manage open file locks and user sessions with the Windows Shared Folders MMC. 

## **Access-Based Enumeration** 

Access-based enumeration (ABE) is a Windows feature (SMB protocol) that filters the list of available files and folders on the file server to include only those the requesting user can access. This filter saves time for the user and helps the administrator prevent users from accessing files not meant for them. 

ABE doesn't control security permissions, and running ABE has associated overhead. Every time a user requests a browse operation, ABE must filter out objects the user doesn't have permission for. Even if the user has permission to access all contents of the share, ABE still runs, which uses additional CPU cycles and increases latency. Don't turn 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

Nutanix Files Storage 

on ABE for home directories (using a distributed share). Most users receive their home mapping when they sign in and always have access to their own content, so we don't recommend ABE for home directory use cases. 

## **SMB Signing and Encryption for Nutanix Files Storage** 

SMB signing is a feature of the SMB protocol that enables a digital signature against each network packet. Digital signing helps verify the packet's origin and authenticity and prevent tampering, such as eavesdropping attacks. 

Files Storage supports SMB signing and honors the signing request as configured from the SMB client. SMB signing is enabled but not enforced by default. The client's SMB signing configuration negotiates with the file server to determine how to use signing. You can enforce signing for all clients by setting signing as **Mandatory** . For more information on configuring signing as mandatory, see Security Hardening in the Files Storage User Guide. 

The SMB protocol uses AES-CMAC to compute signatures for SMB signing, which might affect performance on the SMB share where you enabled signing. The signature computation uses the Intel processor AES-NI instruction set, and the Intel processor hardware acceleration helps reduce the overhead associated with SMB signing. 

Files Storage also supports in-flight encryption of SMB data between clients and shares. SMB encryption isn't on by default with Files Storage. You can turn encryption on as needed on a share-by-share basis. Clients must support encryption to access shares with encryption enabled. Files Storage offloads encryption processing using the Intel processor AES-NI instruction set to limit the performance impact. 

Enabling SMB signing or encryption can decrease the maximum possible performance for SMB shares. 

## **SMB Client Reconnection and Failover** 

Durable handles help SMB clients to survive temporary client-side connection losses after opening files, allowing transparent client reconnection within a timeout window. Files Storage supports durable handles for SMB clients with no configuration required. SMB 2.x and SMB 3.0 clients can use durable handles to reconnect transparently if a clientside network interruption occurs. The system doesn't maintain durable handles through unplanned file server events such as FSVM failures. Starting with Files Storage 4.2, the 

© 2026 Nutanix, Inc. All rights reserved  | **31** 

Nutanix Files Storage 

system maintains durable handles during file server upgrades, but it does so by best effort and provides no guarantees. 

SMB Transparent Failover is a feature of the SMB 3.0 protocol that enables fully nondisruptive operations against SMB shares. Transparent failover is often called continuously available file shares. File server–side failures or upgrade events persist file handles so that clients transparently reconnect to another FSVM without affecting applications. Files Storage versions 3.7.1 and later support SMB 3.0 Transparent Failover. Continuously available shares are intended for solutions that require nondisruptive operations, like Citrix App Layering and FSLogix. 

**Note:** SMB Transparent Failover enforces synchronous write operations, which might decrease the maximum possible performance for SMB shares. 

## **Distributed File System Namespaces for Nutanix Files Storage** 

DFS Namespaces can logically organize shared folders located on different servers or in different geographic locations into a single namespace. These shared folders appear to the user as a unified, hierarchical directory that they can navigate using any Windows client. The server names and their locations are completely hidden from the user, enabling large scale-out architectures. 

The most common architecture is an Active Directory–integrated DFS Namespace, where the namespace is hosted on two or more domain controllers (namespace servers) and the file data is stored on member servers. Files Storage supports the use of DFS Namespaces when you use Files Storage as a member server and Files Storage shares as folder targets in the namespace. You can use either distributed shares or standard shares in the namespace. 

DFS Namespaces are also commonly used in active-active replication scenarios. DFS Namespaces allow multiple file servers hosting the same data to support a common folder. DFS Namespaces provide site affinity based on Active Directory for users to connect to the file server acting as a folder target. You must have a replication engine to maintain the data when active-active scenarios are present. Nutanix supports the use of Peer Software as the replication engine for active-active scenarios. For more information on the Peer Software and Nutanix integration, see the PeerGFS and Files Storage datasheet. 

© 2026 Nutanix, Inc. All rights reserved  | **32** 

Nutanix Files Storage 

## **Unified Namespaces with Nutanix Files Storage** 

A Files Storage instance (file server) represents an individual namespace and path for clients. A file server can only consume the compute and storage available in the Nutanix AOS cluster on which it runs; it cannot consume resources across clusters, which limits how much compute and storage a namespace can have without using a third party like DFS Namespace. 

Files Storage versions 5.1 and later can natively span a namespace across multiple Nutanix file servers running on separate physical clusters, and Files Storage 5.2 introduces support to include third-party file servers. These feature have several benefits: 

- Scaling a namespace beyond the limits of a single file server 

- Matching the structure of an existing environment, which limits the need to change existing client paths 

- Managing cluster sizes and failure domains under one federation 

- Simplifying client mobility between environments during a phased migration 

A namespace has a core file server and member file server. A core file server represents the name, essentially the path, for the namespace. The core is the initial point of connectivity and redirects clients to member file servers as needed. The core also coordinates namespace metadata replication across the core file server and members. The member file servers participate in the namespace, with each of their shares presented under the federation. Each member also replicates information to the core file server when changes such as rename, update, create, and delete operations occur. 

Smart Disaster Recovery (Smart DR) is supported for core and Nutanix member file servers in a namespace. A file server cannot replicate within its own namespace; the target file server must reside in a separate namespace. The namespace fails over as a part of a planned or unplanned operation, meaning that all core and member file servers fail over together. Each file server must have replication policies to peer file servers in the same target namespace. You can switch the direction of a Smart DR policy, meaning that the share switches replication direction and becomes read/write in the target unified namespace. 

Unified namespace redirections use the following workflow: 

© 2026 Nutanix, Inc. All rights reserved  | **33** 

Nutanix Files Storage 

**1.** The client submits a DNS query. 

**2.** The client requests a share from the core file server on the unified namespace. 

**3.** The core file server refers the client to the correct member file server on the unified namespace. 

**4.** The client requests the share from the member file server. 

Figure 13: Unified Namespace Redirection 

## **NFSv4 Support in Nutanix Files Storage** 

Files Storage supports NFSv4, which is more stateful than NFSv3 and includes advanced features such as strongly mandated security and DFS-like referrals. Most recent distributions of Linux, Solaris, and AIX use NFSv4 as the default client protocol. 

Files Storage supports Active Directory, LDAP, and unmanaged access to NFSv4 exports. To make the transition from NFSv3 easier, Files Storage doesn't require administrators to configure Active Directory or LDAP. You can use AUTH_SYS or AUTH_NONE authentication. AUTH_SYS authenticates at the client, just like NFSv3. 

By enabling Active Directory support, you can use three different levels of Kerberos authentication. Each of the following options uses Kerberos version 5: 

© 2026 Nutanix, Inc. All rights reserved  | **34** 

Nutanix Files Storage 

- krb5: Data encryption standard (DES) symmetric key encryption and an MD5 one-way hash for Files Storage credentials 

- krb5i: In addition to krb5; uses MD5-based MAC on every request and response 

- krb5p: In addition to krb5 and krb5i; makes the connection between client and server private by applying DES encryption 

When you deploy Files Storage for NFS, you can select Active Directory, LDAP, or leave it unmanaged. 

Files Storage provides support for both POSIX (Unix) mode bits and NFSv4 access control lists (ACLs). NFSv4 ACL support was introduced with Files Storage 5.3. NFSv4 ACLs provide several benefits over traditional mode bits, including broader permission types beyond read, write, and execute, like delete, attributes, and more. NFSv4 ACLs can be applied to multiple users and groups and are not limited to owner, group, and other semantics, such as mode bits. NFSv4 ACLs also support inheritance, so access controls can automatically propagate to new files and subdirectories. 

The following steps illustrate what happens in the background when a client sends a file access request using NFSv4. These steps are similar to the referral process for SMB. 

**1.** The client sends a DNS request for the file server name. 

**2.** Using DNS round robin, a DNS reply returns with a file server VM (FSVM) address. In this example, the IP address for FSVM-1 returns first. 

**3.** The client sends a create or open request to FSVM-1. 

**4.** If the mount doesn't exist on this file server, it returns NFS4ERR_MOVED. 

**5.** The client requests a GETATTR(FS_LOCATIONS). 

**6.** FSVM-1 looks up the correct mapping in the file server's Zookeeper and refers the client to FSVM-2. 

**7.** A DNS request goes out to resolve FSVM-2. 

**8.** The DNS request returns the IP address of FSVM-2. 

**9.** The client receives access to the correct mount point. 

Files Storage provides multiple optimizations for the NFS stack, enhancing performance with higher throughput and low-latency responsiveness: 

- Nconnect establishes multiple parallel TCP connections for a single NFS mount, which distributes file I/O across CPU cores. This approach reduces client-side queuing to 

© 2026 Nutanix, Inc. All rights reserved  | **35** 

Nutanix Files Storage 

improve response times and aggregate performance, especially in multi-threaded and high-concurrency environments. 

- NFS over Remote Directory Memory Access (RDMA) bypasses the traditional TCP/ IP address stack to enable direct, zero-copy memory transfers between the client and storage. This method significantly reduces I/O latency and lowers CPU utilization, leading to higher throughput and greater system efficiency. 

- GPUDirect Storage creates a direct data path between GPUs and RDMA-capable storage, completely bypassing the CPU to eliminate intermediate data copies. This method results in an ultra-low latency pipeline that accelerates AI and machine learning tasks while freeing up CPU resources for other processes. 

## **NFSv3 Support in Nutanix Files Storage** 

Files Storage supports NFSv3. NFSv3 is enabled on each file server by default, but you can manually turn it off. All NFS exports have NFSv3 turned on or off based on this file server setting. Files Storage supports LDAP and unmanaged exports with NFSv3 but doesn't support Active Directory and Kerberos for NFSv3. 

**Note:** You must mount exports with the TCP protocol if clients use NFSv3. 

Files Storage with NFSv3 includes support for both distributed and standard exports. Unlike NFSv4, NFSv3 doesn't support DFS-like referrals. Clients aren't redirected to the file server VMs (FSVMs) that host a given distributed top-level directory or standard export; they connect to the first FSVM resolved using DNS. An internal remote procedure call (RPC) manages any file access requests to exports and top-level directories not hosted by the client-connected FSVM. This RPC sends or receives data between the client-connected FSVM and the FSVM that owns the required exports and volume groups. The following diagram illustrates this process. 

© 2026 Nutanix, Inc. All rights reserved  | **36** 

Nutanix Files Storage 

Figure 14: DNS Request for NFSv3 

## **Multiprotocol Shares in Nutanix Files Storage** 

You can create file shares that are accessible from both SMB and NFS clients, referred to as multiprotocol shares. Multiprotocol shares allow simultaneous read access to the underlying file data from either protocol. Write access can also occur from either protocol, but not simultaneously to the same file. Authentication support for multiprotocol shares includes all protocols and authentication options available with Files Storage (such as Active Directory for SMB and LDAP, AUTH_SYS, AUTH_NONE, and Active Directory support for NFS). 

© 2026 Nutanix, Inc. All rights reserved  | **37** 

Nutanix Files Storage 

Figure 15: Multiprotocol Shares 

Multiprotocol introduces the concept of a native protocol (either SMB or NFS) for a file share. You specify the native protocol and non-native protocol when you create the share. 

You manage all access control for a multiprotocol share using the native protocol. For SMB, the native protocol is Windows access control lists (ACLs), and for NFS, it's Unix mode bits. When non-native protocol access occurs, Files Storage maps user access to the permission applied with the native protocol. A mapping must exist between the user accounts using the non-native protocol and the user accounts with permission applied through the native protocol. 

If you use Active Directory for SMB and NFS with Kerberos, you don't need any explicit mappings because both have the same users and groups. You can use Identity Management for Unix (RFC 2307) to map NFS users to your Active Directory users with attributes. For LDAP or unmanaged accounts or for Active Directory without Kerberos, you must create a mapping between the users and groups in Active Directory. You can use Prism or the nCLI to manage Files Storage user mappings. 

You can configure a default index to map all non-native users and groups to a specific native user or group. You can also use a rule-based mapping for Active Directory and LDAP users—specifically, a template where the SMB name matches the NFS username. 

© 2026 Nutanix, Inc. All rights reserved  | **38** 

Nutanix Files Storage 

Additionally, you can configure explicit mapping, which overrides rule-based mapping. Explicit mapping consists of two mapping subcategories: one-to-one mapping lists and wildcard mapping. You can use the one-to-one mapping list to manually enter or upload a CSV file that maps users or groups across protocols. Use wildcards for many-toone mapping. You can also deny share access for a specific user or group. For more information on creating user mappings, see the User Mapping section of the Files Storage User Guide. 

The following table provides the user mapping requirements based on the directory service and authentication type. 

## _Table: User Mapping Requirements_ 

|**SMB Directory**|**NFS Directory**|**Export**|**Supported**|**User Mapping**|
|---|---|---|---|---|
|**Service**|**Service**|**Authentication**||**Required**|
|||**Type**|||
|Active Directory|Active Directory|Kerberos5*|Yes|No|
|Active Directory|Active Directory|System|Yes|Yes (Name-to-ID|
|||||mapping)|
|Active Directory|Active Directory|None|Yes (Primary NFS|Yes (Name-to-ID|
||||only)|mapping)|
|Active Directory|Active Directory +|Kerberos5*|Yes|No|
||RFC 2307||||
|Active Directory|Active Directory +|System|Yes|Yes (Name-to-ID|
||RFC 2307|||mapping)|
|Active Directory|Active Directory +|None|Yes (Primary NFS|Yes (Name-to-ID|
||RFC 2307||only)|mapping)|
|Active Directory|LDAP|System|Yes|Yes (Name-to-|
|||||Name mapping)|
|Active Directory|LDAP|None|Yes (Primary NFS|Yes (Name-to-|
||||only)|Name mapping)|
|Active Directory|Unmanaged|System|Yes|Yes (Name-to-ID|
|||||mapping)|
|Active Directory|Unmanaged|None|Yes (Primary NFS|Yes (Name-to-ID|
||||only)|mapping)|



_* Kerberos 5, 5i, and 5p._ 

© 2026 Nutanix, Inc. All rights reserved  | **39** 

Nutanix Files Storage 

## **Nutanix Files Storage User and Directory Quotas** 

Files Storage supports the capability of creating user and directory quotas. Directory quotas were introduced in the Files Storage 5.3 release. With user quotas, you can configure a default, user, and group quota for any share. The default level is the quota limit for every user unless the administrator specifies otherwise. A user-level quota policy sets a specific amount of storage for a single user. For example, if an administrator allocates 1 GB, the user can't use more than 1 GB. A group-level quota policy extends a user policy to include all users for an entire Active Directory group, where each user can use the assigned quota value. For example, if the administrator sets a group's quota to 10 GB, each member of that group can use 10 GB. 

The following list shows the order of precedence for policies when dealing with user quotas for a share: 

**1.** User policy 

**2.** Group policy (the group policy with the highest quota wins) 

**3.** Default user policy 

Directory quotas provide a storage limit for one or more directory paths as specified by an administrator. If only one directory is specified in a policy, the quota is applied to that path. If multiple directories are listed, the quota is shared and aggregated across the paths. It is also possible to have separate quotas on directories within the same folder structure, referred to as hierarchical quotas. For example, you can have a quota on /dir1 and a different quota on subdirectory /dir1/subdir1. Note that enforcing both aggregated and hierarchical folder quotas is not supported. 

You can assign quotas for SMB, NFS, or multiprotocol-enabled shares. In the case of multiprotocol shares, administrators apply quotas using the native protocol. User or group policies are enforced for the non-native protocol based on the user mapping. 

You can configure Nutanix Prism to send email alerts to the user (for user quotas) and to other recipients (for user and folder quotas) using the same engine that sends cluster alerts. Designated users receive a warning email message when a quota reaches 90 percent of the limit and an alert email message when it reaches 100 percent. You can optionally customize the email notification template. 

Enforcement types determine if a user or group can continue to use the share after they consume their user or directory quota. A hard enforcement type prevents the user from 

© 2026 Nutanix, Inc. All rights reserved  | **40** 

Nutanix Files Storage 

writing on the share when they reach their quota limit. A soft enforcement type allows a user to write even if they exceed the quota limit. Under either enforcement type, users over their quota receive an email notification every 24 hours until they resolve the issue. 

## **Selective File Blocking** 

You can define a list of file names and file extensions to block from storage on SMB, NFS, and multiprotocol-enabled shares. You can define a list of files to block at the server level and at the share level. These entries can include wildcards for both the file name and file extension. For example, you can block the file pattern `encrypt*.*xt` . 

In the **Create a share/export** dialog box, you can define which file types to block with a comma-separated list of file names and extensions. File types blocked at the server level apply to all shares. When you define a list of file types at the share level, the share-level setting overrides the server-level setting. 

When you attempt to create or rename a file with a blocked pattern, an access denied message appears. You can read, edit, or delete existing files that were created before a blocked file type policy. You can also use Data Lens to discover if any unwanted file types exist on a file server. With Data Lens, you can search for files that match the patterns you need to block. 

When you use Data Lens to manage your ransomware file name patterns, you can't see the patterns from within the file server or share specific blocking lists. 

## **Files Manager** 

Prism Central and Files Storage support an integrated service called Files Manager, which discovers all Files Storage instances running on clusters registered in Prism Central. Files Manager also provides views on all alerts and events across your file server farm. You can view the file server configurations and launch the Files Storage console to manage the file servers. From Files Manager, you can deploy and scale file servers, configure Smart Disaster Recovery (Smart DR) and data synchronization, and apply role-based access control (RBAC) to fine-tune what users can create and control. 

With RBAC, a super admin user in Prism Central can delegate file server management to nonadmin users. You can control which file servers a user is allowed to view or manage, along with the operations they are allowed to perform, such as creating or deleting file 

© 2026 Nutanix, Inc. All rights reserved  | **41** 

Nutanix Files Storage 

servers, creating shares, or editing file server attributes. For more information on Files Storage RBAC capabilities and configuration, see the Files Manager User Guide. 

Apply RBAC policies through Prism Central. RBAC policies aren't available when managing Files Storage with Prism Element. 

## **Nutanix Data Lens and Nutanix Files Storage Smart Tier** 

Nutanix Data Lens is a software as a service (SaaS) or on-premises data security solution offering ransomware resilience and data analytics for Nutanix Unified Storage and Amazon S3. Data Lens offers global data visibility to proactively assess and mitigate data security risks by identifying anomalous activity, auditing user behavior, and adhering to compliance requirements while enabling efficient data life cycle management. Data Lens also provides robust ransomware protection, permissions monitoring, and tiering management. Data Lens (SaaS) centralizes information from all your clusters connected to Nutanix Pulse across various datacenter locations. Using cloud resources eliminates scaling constraints, allowing you to analyze an unlimited number of files and objects and unlimited amounts of data. If your organization has dark sites that can't connect to the public cloud, SaaS-prohibitive policies, or is in a regulated industry with strict data residency and security requirements, you can run Data Lens (On-Prem) on Nutanix clusters in your environment. All metadata and audit logs remain within your infrastructure with no external data transmission, helping to strengthen your digital sovereignty. For more information, see Nutanix Data Lens. 

Files Storage provides a native tiering framework called Smart Tier to improve storage efficiency, simplify administration, and decrease costs for large network-attached storage (NAS) environments. Smart Tier can also help hybrid multicloud environments handle long-term unstructured data retention, save money, or use a virtually limitless pool of local storage for your Files Storage workloads. With Smart Tier, you can move cold or rarely used data to lower-cost storage while maintaining a single namespace. You can tier data to any qualified S3 API–compliant target, including on-premises solutions or the public cloud. Smart Tier also supports Azure Blob Storage. 

The Files Storage tiering engine uses APIs over HTTPS to accept tiering and recall requests and to move data to Azure Blob or S3-compliant targets. Multiple targets are validated for tiering: 

- Nutanix Objects 

© 2026 Nutanix, Inc. All rights reserved  | **42** 

Nutanix Files Storage 

- Azure Blob 

   - › Hot 

   - › Cool 

- Amazon S3 

   - › Standard 

   - › Infrequent Access 

   - › Glacier Instant Retrieval 

- Wasabi Cloud Storage 

- Google Cloud Storage 

- OVHcloud Object Storage 

You can tier files to these targets using standard or advanced tiering methods. 

## **Standard** 

Standard tiering uses the Files Storage console local to the file server and requires no additional software or licensing. Standard tiering also supports manual or automatic tiering operations, inline reads of tiered data, setting a capacity threshold, defining the age and size of the files to tier, and manual recall operations at the share level. 

## **Advanced** 

Data Lens manages advanced tiering, which supports the same functions as standard tiering but includes filtering by file type and owner and more granular recall operations at the file, folder path, or share level. Additionally, advanced tiering provides automatic recall based on access frequency along with share-level data age and analytics. 

When you tier files, Files Storage maintains the file metadata while it moves the data to the target. When data reaches the configured age and the file server is at its capacity threshold, Smart Tier moves file data to the target. Smart Tier marks the previously consumed space as free, creating additional space in the share for new or recalled data. Tiered files appear as regular files in their folder paths. SMB shares have an offline attribute set, which in Windows Explorer shows an X beside the file icon. 

© 2026 Nutanix, Inc. All rights reserved  | **43** 

Nutanix Files Storage 

You can see the file size (size on disk), which represents just the metadata, and the actual file size, which represents the tiered data. You can retrieve the data inline (or read) from tiered files at any time, but you can't write to a tiered file directly. You must either recall the file or make a separate copy to edit it. 

Figure 16: Smart Tier Architecture Example with Data Lens 

Smart Tier maintains a file stub in the SMB or NFS share path where clients can perform inline reads to access the data. You can also recall files automatically, based on access patterns, or with manual recall operations. You can configure tiering policies and set age- and capacity-based thresholds, manual or automatic tiering schedules, and recall settings. 

## **Configuring Smart Tier Using Data Lens** 

To configure Smart Tier, follow these steps: 

**1.** Define a tiering location. 

The tiering location must be a supported S3-compliant target. 

© 2026 Nutanix, Inc. All rights reserved  | **44** 

Nutanix Files Storage 

## **2.** Configure the following components for your location: 

- Target URL over HTTPS 

- Bucket name 

- Access and secret keys 

- Retention period 

The retention period determines when Files Storage deletes a tiered object after removing the stub from the file system. 

   - Certificate (optional) 

**3.** Define a capacity threshold and when-to-tier policy. 

The capacity threshold represents the percent of allocated space consumed before the system considers tiering. You also define whether the system performs the tiering policy manually or on an automated basis and the time windows in which automated tiering occurs. 

## **4.** Define the formal tiering policy items: 

- Age of the data (based on the last file read or write operation) 

- Minimum file size (64 KB or larger) 

- Shares to exclude from the policy 

- Automatic recall settings 

When defining the automatic recall settings, you must determine whether to recall files automatically or manually and under what conditions (specifically, the number of times the file is accessed over a period). If you choose to recall files manually, Data Lens lets you search for and choose individual files, folder paths, or shares to recall. 

## **Hypervisor-Specific Support for Nutanix Files Storage** 

Nutanix supports ESXi and AHV for Files Storage. For ESXi support, you need vCenter credentials to deploy Files Storage and to create DRS rules to keep the file server VMs (FSVMs) on different nodes. You must register vCenter with the Nutanix cluster instances where you deploy Files Storage. The deployment process generates the DRS rules for AHV automatically. 

© 2026 Nutanix, Inc. All rights reserved  | **45** 

Nutanix Files Storage 

Files Storage supports data-at-rest encryption (DARE) with self-encrypting drives (SEDs) or software-defined DARE using Nutanix AOS. 

For AHV, you enable software encryption at the cluster level. For ESXi, you enable software encryption at the cluster level or the storage container level. To enable storage container–level software encryption with Files Storage, you must use nCLI: 

```
<ncli> storage-container edit enable-software-encryption=true
 name=<Files_Container_Name> force=true
```

## **Nutanix Files Storage Kubernetes Integration** 

Files Storage provides persistent, shareable storage for containerized applications with its Container Storage Interface (CSI) compliant driver, which enables any Kubernetes orchestrator certified by the Cloud Native Computing Foundation (CNCF) to dynamically provision and manage persistent volumes. While the standard CSI driver supports foundational capabilities like creating storage classes and persistent volume claims (PVCs) from NFS shares, its functionality is significantly enhanced through integration with Nutanix Data Services for Kubernetes (NDK). This integration addresses the challenge of protecting dynamically created Kubernetes persistent volumes, which can't be protected by replication technologies like Files Storage Smart Disaster Recovery. By integrating Files Storage with NDK, Nutanix delivers a unified, application-centric data services solution that allows for advanced, enterprise-grade data management and protection, managed directly from the container orchestrator, including snapshot management, replication policy administration, disaster recovery operations like failover, failback, and cloning volumes from snapshots. 

© 2026 Nutanix, Inc. All rights reserved  | **46** 

Nutanix Files Storage 

## 3. Backup and Disaster Recovery with Nutanix Files Storage 

Following modern data protection methodologies, Nutanix provides quick restore access using Self-Service Restore (SSR) and site recovery with Smart Disaster Recovery (Smart DR) and Nutanix-based snapshots. You can enable SSR at any time for SMB or NFS shares. The Windows previous version feature in each folder exposes SSR for SMB shares. SSR for NFS shares is exposed as a hidden snapshot directory in each folder. SSR allows you to view snapshots of shares while the share is in use. The share snapshots are read-only, point-in-time copies. 

You can view and restore removed or overwritten files, which means that you can choose a share snapshot from the same file at different times during the file's history. You can configure multiple snapshot policies at the file-server level and apply them to one or more shares. With Files Storage, you can take snapshots every 15 minutes, every 24 hours, and every day, week, and month on a fixed schedule. The default snapshot policy includes the following snapshots: 

- Hourly (24 per day) 

- Once daily for seven days 

- Four weekly 

- Three monthly 

Starting with Files Storage 5.2, you can configure a maximum of 400 total snapshots in an SSR policy. 

You can change the snapshot frequency to suit your requirements, including shorter intervals for same-day protection against accidental deletions. You can enable SSR during or after share creation. After share creation, you can change the current settings using the share update workflow feature. SSR supports both standard and distributed SMB shares. 

© 2026 Nutanix, Inc. All rights reserved  | **47** 

Nutanix Files Storage 

Files Storage also supports one-click, share-level SSR snapshot restoration. With sharelevel restores, you can revert an entire share to a previous point-in-time. 

## **Nutanix Files Storage with Asynchronous Protection Domains and Consistency Groups** 

Nutanix provides integrated, automated disaster recovery between Nutanix clusters. You can protect Files Storage cluster with Prism and use the same asynchronous replication with protection domains and consistency groups that you use with any other Nutanix cluster. 

A protection domain is a defined group of entities (VMs and volume groups) that you back up locally on a cluster and can replicate to one or more remote sites. A consistency group is a subset of the entities in a protection domain. Consistency groups are configured to snapshot a group of VMs or volume groups in a crash-consistent manner. 

Use AOS node sizing guidelines when you determine your recovery point objective (RPO) requirements. Files Storage supports both asynchronous (RPO of one hour or longer) and NearSync (RPO down to one minute) schedules. 

When you create a file server, Nutanix Prism automatically sets up a corresponding protection domain, which it annotates with the Files Storage cluster name. Prism also creates multiple consistency groups in a protection domain, including a group that includes all file server VMs (FSVMs). 

After you protect Files Storage, all future operations on it (such as adding or removing FSVMs or adding or deleting volume groups) automatically update the corresponding consistency group in the protection domain. 

## **Nutanix Files Storage Cluster Migration, Failure, and Restoration** 

If a Files Storage cluster fails, restore Files in a remote cluster by initiating the Activate workflow, which restores from the last good snapshot. If you move your file services because you must shut the cluster down, as with a planned outage, the Migrate workflow shuts down all the file server VMs (FSVMs) and takes a final snapshot for replication. Run the Migrate workflow from the Nutanix cluster that owns the active 

© 2026 Nutanix, Inc. All rights reserved  | **48** 

Nutanix Files Storage 

protection domain. You can initiate both the Activate and Migrate workflows from the Data Protection menu in Prism. 

After you run either the Activate or Migrate workflow, you must activate the file server instance, which you can do from the File Server menu in Prism. Activating the file server might require you to configure network VLANs on the replica site before Files Storage becomes operational again. These VLANs can be in subnets that are the same as or different from the source site. The recovery process is like creating the file server, but in this context, you can change networks and IP addresses if necessary. 

## **Nutanix Files Storage Cloning** 

Because Files Storage cloning doesn't affect the original Files cluster, it offers improved support for the following use cases: 

- Backups at the primary and secondary sites 

- Disaster recovery at the secondary site 

- File server recovery from a specific point in time 

- File server creation at the primary or remote site for testing or development 

- File server clone copies 

Figure 17: Files Storage Cloning Use Cases 

Files uses Nutanix native snapshots to clone entire file servers. The clone is a thin copy that consumes minimal storage space. File server clones reside on the same container 

© 2026 Nutanix, Inc. All rights reserved  | **49** 

Nutanix Files Storage 

as the original and maintain the original security permissions. During the clone process, you can specify new IP addresses and give the cloned file server a new name. 

Files Storage 5.1 introduces file share cloning, where you create a space-efficient copy of a share. The clone uses existing or on-demand Self-Service Restore (SSR) snapshots to create a nearly instantaneous read/write copy. Share cloning enables multiple use cases: 

- Data processing workflows requiring read or write access to a copy of data: For development and quality assurance life cycles, this use case might mitigate risk for testing against real-world datasets. 

- Read and write testing against a Smart Disaster Recovery copy: This use case ensures that your data is completely accessible without having to fail over the data or namespace between sites. 

- Kubernetes environments using the Nutanix Container Storage Interface (CSI) Driver: This use case enables you to share a copy of data across different environments. 

Create share clones from an existing snapshot or a new snapshot at the time of cloning. You can create multiple clones from the same snapshot or multiple clones using different snapshots against the same share. A clone inherits most of its share properties from the source, including settings like compression, encryption, and continuous availability. You can also change most settings after you clone the share. 

## **Metro Availability for Nutanix Files Storage** 

Metro Availability with Files Storage is a synchronous replication solution, ensuring that file server writes are committed to both a source and target Nutanix cluster before acknowledgement, providing a recovery point objective (RPO) of zero. Metro Availability requires that the roundtrip network latency between clusters not exceed 5 ms. You can also automate failover between clusters using a witness: When a cluster on either site detects a failure, it can obtain ownership of the file servers through the witness and automatically restart file servers from the failed cluster. 

© 2026 Nutanix, Inc. All rights reserved  | **50** 

Nutanix Files Storage 

Figure 18: Files Storage in a Metro Configuration 

For additional resilience, you can combine Metro Availability with file server replication capabilities, like Smart Disaster Recovery, to create a three-site replication solution. For example, you can configure synchronous replication between cluster 1 and cluster 2 and asynchronous file server replication between the file server on cluster 1 and a file server on cluster 3. Both ESXi and AHV support Metro Availability with Files Storage. Metro Availability uses AOS-based replication, so follow AOS node sizing and resource requirement guidelines for synchronous replication. 

## **Nutanix Files Storage Smart Disaster Recovery** 

Files Storage supports the core snapshot and remote replication capabilities of Nutanix AOS. Using AOS has several benefits, including simplified and consolidated administration that aligns with the core hyperconverged infrastructure environment. Using AOS-based replication does have some limitations, but Files Storage Smart Disaster Recovery (Smart DR) helps address these challenges around granularity, node density, and the active-passive nature of share access and failover orchestration while maintaining the same simple and consolidated administrative experience. You can configure and manage Smart DR from Files Manager in Prism Central. 

© 2026 Nutanix, Inc. All rights reserved  | **51** 

Nutanix Files Storage 

Smart DR is an intelligent, simple, and effective way to replicate between Files Storage instances, either on-premises or running in the cloud. Smart DR changes several key areas of Files Storage remote replication. First, the AOS clusters no longer manage the replication engine using native Nutanix protection domains; instead, Files Storage manages replication directly. As with Files Storage Self-Service Restore (SSR), Smart DR takes snapshots at the share (or file-system) level, replicating block-level incremental changes between source and target. 

Replication occurs between active file servers running on their respective Nutanix clusters. Shares that function as replication targets are available in a read-only state. Replicating between active file servers helps reduce failover times, reducing your recovery time objective (RTO) and simplifying the use of replicated data for backup consolidation or reporting. 

Because Files Storage manages replication, the node-density limits specific to AOS snapshots and replication don't apply. You can use our most storage-dense nodes with the benefits of native remote replication, and you can configure your replication schedule to be as short as one minute. 

Replication policies define the share or group of shares that you must replicate. The policy also defines the source and target file servers and the replication frequency. You can create multiple policies and specify a default policy for any newly created shares. 

Figure 19: Files Storage Smart Disaster Recovery 

© 2026 Nutanix, Inc. All rights reserved  | **52** 

Nutanix Files Storage 

For the file systems that support the shares performing replication, you can set policies on a share-by-share basis to manage your RPO at the share level instead of the file server level. The source and target file servers can be on AOS clusters registered to different Prism Central instances. 

Files Manager in Prism Central provides service-level agreement (SLA) monitoring and job replication status at several levels. You can use the Policies page to quickly understand the replication status for each policy. You can also view each replication job to monitor its completion percentage, start and end times, amount of data synchronized, and average network bandwidth usage. 

## **Nutanix Files Storage Failover, Failback, and Self-Service Restore** 

You can manage planned or unplanned failover and failback from Prism Central. During failover operations, Prism Central orchestrates the required updates to the DNS and Active Directory service principal names (SPNs) to move the file server instance name from the source to the target. 

With a planned failover, you can choose to begin replicating in the opposite direction automatically. Replicating after failover helps maintain service-level agreements (SLAs) and recovery point objectives (RPOs) during your failover testing or disaster avoidance operations. You can also choose to switch policy directions without failing over the file server namespace. By switching the policy direction, you can reverse the replication relationship and read/write status of the shares between file servers. 

In addition to the standard failover and failback operations, Smart Disaster Recovery supports replicating snapshots between the source share and its target. The target share retention schedule doesn't have to match the source retention schedule. For example, if you configure the source file server to maintain the last 2 hourly snapshots, you can configure the target file server to maintain the last 10 hourly snapshots, providing a longer retention window. 

Alternatively, you can match retention schedules to ensure that the same Self-Service Restore (SSR) copies exist in both sites for any failover event. SSR schedules must have the same frequency type between the source and target. For example, if your daily snapshots are scheduled on your source, you must schedule daily snapshots on your target for them to be replicated and retained. 

© 2026 Nutanix, Inc. All rights reserved  | **53** 

Nutanix Files Storage 

## **Nutanix Files Storage Data Synchronization** 

Data synchronization through Smart Sync provides folder path–level asynchronous replication between Files Storage instances. You can synchronize data using data consolidation and the VDI Sync feature. 

Data consolidation allows you to replicate from multiple file server paths to a single target path for many-to-one replication. Both the source and target path are readable and writable. Changes in a source file overwrite changes to the same file on the target. You can specify replication down to one-minute intervals. 

Figure 20: Data Consolidation Overview 

You can exclude directories or file extension patterns from replication, allowing you to store only what you need on your target system. You can also choose not to replicate delete operations if your goal is to consolidate data on a target while freeing space on a source system. 

For consolidation use cases, you can configure data synchronization through Files Manager in Prism Central to copy files from multiple sites to a single location for retention or centralized backup. 

© 2026 Nutanix, Inc. All rights reserved  | **54** 

Nutanix Files Storage 

VDI Sync replicates user profiles between file servers, enabling users to access their data locally based on where their VDI session resides. If a user travels between sites, they can access their user profile local to the site they're at. VDI Sync is an active-active solution, meaning that both the source and target of replication are readable and writable and changes are replicated in both directions. 

Figure 21: VDI Sync Overview 

VDI Sync asynchronously replicates data between a source and target distributed share across two file servers. When a user connects to their profile on one of the file servers, the logon session is detected. That same user isn't allowed to connect to the peer file server. When the user disconnects their session and final replication is complete, they can connect to the other file server. Site-dependent connectivity is driven by the VDI broker configuration. 

VDI Sync is not intended for collaboration use cases, where multiple users access and update replicated copies of the same data. It is intended for data accessed by the same user. 

© 2026 Nutanix, Inc. All rights reserved  | **55** 

Nutanix Files Storage 

## 4. Nutanix Files Storage in the Public Cloud 

Files Storage is software-defined and can be deployed on-premises and in the public cloud. Public cloud deployments are common for several use cases, including using file shares as primary storage, localizing information for data processing in the cloud (perhaps on a temporary basis), and hybrid cloud disaster recovery. You can run Files Storage on Nutanix Cloud Clusters (NC2) on both AWS and Azure. Files Storage can also run independent of NC2 using native AWS or Azure services. 

Files Storage deployed on NC2 is nearly identical to on-premises deployments. Files Storage takes advantage of NCI, which is deployed on bare-metal servers on AWS or Azure. You manage Files Storage on NC2 the same way that you manage on-premises environments with Prism. Each bare-metal node has internal storage (CPUs, disks, memory, and NICs), and you can add storage through AWS Elastic Block Store (EBS) to provide denser storage deployments. 

Files Storage on NC2 on Azure has some unique requirements: You must deploy Files from Prism Central and use virtual private cloud (VPC) networking. Both the client and storage networks reside in the same VPC. You can place the client and storage networks in separate overlays within this common VPC. 

The Files Storage architecture is slightly different when deployed in a VPC, which is true in NC2 on Azure and on-premises deployments. You only use the storage network for management communication with Files Manager and Prism Central; the storage traffic does not go over the internal or storage network. The volume groups used to support shares are attached directly to the file server VMs (FSVMs), so they appear to the VM as local drives and not as iSCSI devices, decreasing the effect on layer 2 networking performance between the Files Storage storage interface and the Controller VMs (CVMs) in the underlaying network outside the VPC. 

The best use cases for Files Storage in an NC2 environment are if you must use NC2 and have spare capacity or need an operating model similar to that of other applications running on NC2. 

© 2026 Nutanix, Inc. All rights reserved  | **56** 

Nutanix Files Storage 

## **Nutanix Files Storage with Native AWS or Azure Services** 

Files Storage 5.1 and later versions support files services running in AWS, decoupled from NCI and Nutanix Cloud Clusters (NC2). Files Storage on AWS is based on native cloud services like EC2 for compute, EBS for hot data, and S3 as a capacity storage tier. 

Files Storage 5.2 and later versions support files services running in Azure, decoupled from NCI and NC2. Files Storage on Azure is based on native cloud services like Azure VMs for compute, Azure Managed Disks for hot data, and Azure Blob as a capacity storage tier. Perform all management operations, including installation, configuration, and upgrades, from Prism. For installation, use your existing AWS and Azure accounts with the appropriate identity and access management (IAM) configuration. Deploy the file server VMs (FSVMs) with instance types of your choosing in a supported region, and select the appropriate VPC network. Nutanix Unified Storage Pro licensing is fully portable; use it for Files in AWS or Azure the same way you do for on-premises deployments. 

Figure 22: Files Storage in AWS 

Several storage efficiency features are included to reduce cloud infrastructure costs. All share data is compressed inline while being written to the initial EBS or Azure Managed Disk tier of storage. The allocated volumes start small and incrementally grow only as you need more hot storage. To limit the size of these volumes, Files Storage automatically tiers data to S3 for AWS instances and to Azure Blob for Azure instances 

© 2026 Nutanix, Inc. All rights reserved  | **57** 

Nutanix Files Storage 

to provide a more cost effective capacity storage tier. Users can read and write directly to and from tiered data. 

The cloud-based FSVMs use EBS general purpose SSD volumes, specifically the gp3 volume type for AWS. For Azure, the managed disks are premium SSD v2. Highperformance use cases have an optional profile, not available by default in AWS, that supports larger EC2 instance types that use the EBS io2 Block Express volume type. Files Storage creates a standard S3 bucket by default for the capacity tier of storage in AWS. Azure deployments use the Azure Blob hot tier by default. 

To help support the hybrid cloud disaster recovery use case, you can use Smart Disaster Recover (Smart DR) with Files Storage in AWS and Azure. Smart DR with Files Storage in AWS or Azure can have asymmetric configurations: The number of FSVMs do not have to match between the source and target Files instances that are the replication partners to the Files instance running natively in the cloud. In on-premises Smart DR deployments, the source and target must have the same number of FSVMs. Core aspects of Smart DR are supported, including bidirectional replication, failover and failback, switching policy directions, and multiple Prism Central instances. 

Files Storage running natively in the cloud supports all file server–level features, including in-flight encryption, write once, read many (WORM), and multiprotocol. Certain AOS-level features such as erasure coding are not available, but data-at-rest encryption (DaRE) is supported at the EBS or Azure Managed Disk layer, functions provided by AWS and Azure respectively. Data redundancy is also provided by the respective cloud service. 

© 2026 Nutanix, Inc. All rights reserved  | **58** 

Nutanix Files Storage 

## 5. Third-Party Integration with Nutanix Files Storage 

You can use third-party products with the Files Storage API for the following solutions: 

## **Antivirus protection** 

Nutanix currently supports third-party vendors that use Internet Content Adaptation Protocol (ICAP) servers. To protect users from malware and viruses, you must address both the client and the file server. 

## **File operations monitoring** 

File operations monitoring is a native Files Storage API used to forward all SMB and NFS operations run by clients to a registered target repository. Partner software can register with the Files Storage instance to capture file activity events. Partner software can also make REST calls to Files Storage to create a policy that defines the file notification types to receive and the shares to receive them from. 

## **Backup** 

You can restore backups taken using the Files Storage API to other vendors. A changed-file tracking (CFT) API for third-party backup vendors decreases backup times because it doesn't perform a metadata scan across your file server, which might contain millions of files and directories. Backups can occur across multiple file server VMs (FSVMs) in parallel. 

## **Third-Party Antivirus Solutions for Nutanix Files Storage** 

The Internet Content Adaptation Protocol (ICAP), which is supported by a wide range of security vendors and products, is a standard protocol that allows you to integrate file and web servers with security products. Nutanix chose this method so that you can select an antivirus solution that works best for your specific environment. 

ICAP-supported antivirus solutions follow these steps: 

**1.** An SMB client submits a request to open or close a file. 

© 2026 Nutanix, Inc. All rights reserved  | **59** 

Nutanix Files Storage 

**2.** The file server determines if the file must be scanned based on the metadata and virus scan policies. If the file must be scanned, the file server sends the file to the ICAP server and issues a scan request. 

**3.** The ICAP server scans the file and reports the scan results to the file server. 

**4.** The file server takes an action based on the scan results: 

   - If the file is infected, the file server quarantines it and returns an access denied message to the SMB client. 

   - If the file isn't infected, it returns the file handle to the SMB client. 

Figure 23: ICAP Workflow 

The ICAP service runs on each Files Storage file server and can interact with more than one ICAP server in parallel to scale out the antivirus server. The scale-out nature of Files Storage and one-click optimization significantly mitigate antivirus scanning performance overhead. If scanning affects Files Storage file server VM (FSVM) performance, one-click optimization recommends either increasing the virtual CPU resources or scaling out the FSVMs. This feature also helps both the ICAP server and Files scale out, ensuring fast responses from the customer's antivirus vendor. 

We recommend configuring two or more ICAP servers for production. 

Files Storage sets scanning defaults across the entire file server. You can enable scan on write and scan on read. Scan on write begins when the file is closed, and scan on read occurs when the file is opened. You can also exclude certain file types and sizes. Share scan policies can override any defaults set for the file server. 

For each ICAP server, Files Storage creates no more than 10 parallel connections per FSVM and randomly distributes the file scanning among all the ICAP servers. With heavier workloads, which might involve many scan requests and use all connections, 

© 2026 Nutanix, Inc. All rights reserved  | **60** 

Nutanix Files Storage 

you can give the scan servers more processing power to scan more files. As soon as the current scan finishes, the scan server picks up the next file from the queue, which keeps the number of active connections at 10. 

After Files Storage quarantines a file, the administrator can rescan the file, remove it from quarantine, or delete it. You can search quarantined files if you must restore a file quickly. 

If your antivirus vendor doesn't support ICAP, you can scan the shares by installing an antivirus agent on a Windows machine, then mount all the shares from the file server. With this approach, you can schedule scans for periods of low usage. At the desktop or client level, you can set your antivirus solution to scan on write or scan only when files are modified. You can configure high-security environments to scan inline for both reads and writes. 

For the latest list of qualified ICAP server vendors with Files Storage, see the Compatibility and Interoperability Matrix on the Nutanix Support portal. 

## **Third-Party File Operations Monitoring with Nutanix Files Storage** 

File operations monitoring has two major areas for third-party vendors: 

- File activity 

- Audit 

Partner software uses a web client and communicates with the Files Storage file server using HTTPS requests. The HTTPS communication relies on SSL authentication. The HTTP server runs with either its unique self-signed SSL certificate or a Transport Layer Security (TLS) connection with the partner, exchanges the keys between the two parties, and sends messages to the partner server over this secure channel. 

Third-party software can use different protocol communication methods such as syslog, Google, or Kafka protobuf. You can use the Nutanix file operations monitoring API to forward events to a syslog server for any future auditing needs. The following vendors are integrated with this operations monitoring API: 

© 2026 Nutanix, Inc. All rights reserved  | **61** 

Nutanix Files Storage 

- Peer Software: Includes the file operations monitoring API to support active-active deployments with real-time replication and file locking between heterogenous environments. 

For more information, see the PeerGFS and Files Storage datasheet. 

- Netwrix: Provides comprehensive visibility into changes and data access across Files Storage instances; offers an add-on for Files Storage. 

For more information, see the Netwrix website. 

- Varonis: Version 8.6 offers support for Files Storage versions 3.7.1 and later, including Varonis' entire platform, DatAdvantage, DatAlert and DatAlert Analytics, DataPrivilege, Data Classification Engine, DatAnswers, Automation Engine, and Data Transport Engine. 

## **Third-Party Backup Solutions for Nutanix Files Storage** 

Traditional NAS backup approaches like Network Data Management Protocol (NDMP) are limited. Problems like complexity, performance (including the need for periodic full backups), and scale led Nutanix to develop a more modern backup solution: a changed file tracking (CFT) API for third-party backup vendors. 

To produce CFT backups, Files Storage and a third-party backup vendor follow these steps: 

**1.** The backup server takes a snapshot. 

**2.** The backup server requests the differences between the current and previous snapshot. 

**3.** The Files Storage server sends the differences as a share. 

**4.** The backup server mounts the share and reads the changed files. 

**5.** The backup server writes the changed data to the backup target. 

© 2026 Nutanix, Inc. All rights reserved  | **62** 

Nutanix Files Storage 

Figure 24: CFT Backup Process 

Backup software can specify multiple shares and their respective snapshot information. Files Storage returns the list of URLs that map to the number of client streams that can start in parallel. Because shares are distributed evenly across the file server VMs (FSVMs) based on load, the backup software can use all the FSVMs to drive throughput. Multiple software vendors are integrated with the Files Storage CFT API: 

- HYCU Backup and Recovery 

- Commvault, version 11, service pack 15 (as of CFT API release 3.5) 

- Veritas Netbackup 9.1 

- Veeam 12 

You can back up Files Storage shares with software that doesn't support CFT. One option is to run the backup application on a Windows machine and map the UNC path of the share as a drive to be backed up. Some vendors provide support for backing up file shares without mounting to a guest VM. These applications can read directly from the UNC path. For the full list of validated backup applications, see the Compatibility and Interoperability Matrix. 

© 2026 Nutanix, Inc. All rights reserved  | **63** 

Nutanix Files Storage 

**Note:** Don't back up the FSVMs. FSVMs are stateless, and data is stored in Nutanix volume groups. Back up Files Storage from the share level. You can protect FSVMs using Nutanix protection domains. 

Because the system spreads different standard shares across the cluster, back up multiple shares at the same time with multiple subclients. The distributed share allows you to configure multiple data readers to drive throughput. 

In a test scenario for backing up using Commvault, we tested 400 users spread out on three FSVMs, placing the data on a home share. The Commvault backup job completed with the following results: 

- Average throughput: 559.92 GB per hour 

- Elapsed time: 5 minutes and 26 seconds 

- Transfer time: 4 minutes and 19 seconds 

- Number of files transferred: 57,147 

- Failures: 0 (files and folders) 

- Skipped files: 0 

- Application size: 40.26 GB 

- Data transferred on network: 118.95 

- Compression: 49.90 percent 

We found that adding more readers for the backup job can increase performance. The bottleneck was the media agent, which was the backup destination for the files. The media agent was virtualized and configured with only eight vCPUs. Increasing the media agent's vCPUs shortened the backup time. 

© 2026 Nutanix, Inc. All rights reserved  | **64** 

Nutanix Files Storage 

## 6. Document Version History 

## _Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|December 2016|Original publication.|
|1.1|May 2017|Updated Backup and Disaster|
|||Recovery section.|
|1.2|September 2017|Updated for version 2.2|
|||features.|
|2.0|February 2018|Updated for version 3.0.|
|2.1|April 2018|Solution overview update.|
|2.2|August 2018|SMB share and NFS export|
|||updates.|
|3.0|October 2018|Updated for version 3.1 and|
|||updated product naming.|
|4.0|January 2019|Updated for version 3.2 and|
|||AOS 5.9 and later.|
|5.0|April 2019|Updated for version 3.5 and|
|||AOS 5.10 and later.|
|5.1|August 2019|Updated for version 3.5.1.|
|5.2|October 2019|Updated for version 3.6.|
|5.3|October 2020|Updated for version 3.7.|
|6.0|April 2021|Updated for Files 3.8, File|
|||Analytics 3.0, and Files|
|||Manager 2.0.|



© 2026 Nutanix, Inc. All rights reserved  | **65** 

Nutanix Files Storage 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|7.0<br>7.1<br>7.2<br>8.0<br>8.1<br>9.0<br>9.1<br>9.2<br>10.0<br>10.1<br>10.2<br>10.3|October 2021<br>Updated for Files 4.0 and Data<br>Lens. Added Smart Tier and<br>SSR Interoperability sections.<br>Updated Shares and Exports,<br>Notifications, Selective File<br>Block, and Files Operations<br>Monitoring sections. Updated<br>Files Smart DR figure.<br>June 2022<br>Updated for Files 4.1 and Data<br>Lens.<br>January 2023<br>Updated for Files 4.2 and Data<br>Lens.<br>November 2023<br>Updated for Files 4.3, 4.4, and<br>Data Lens.<br>December 2023<br>Updated the Volume Group<br>and Storage Container<br>Architecture section.<br>August 2024<br>Updated for Files 5.0.<br>September 2024<br>Updated erasure coding<br>guidance in the Volume<br>Group and Storage Container<br>Architecture section.<br>October 2024<br>Updated the SMB Signing<br>section.<br>March 2025<br>Updated for Nutanix Files 5.1.<br>April 2025<br>Updated the Nutanix Files with<br>Native AWS Services section.<br>April 2025<br>Updated the Nutanix Files<br>on Nutanix Cloud Clusters<br>section.<br>June 2025<br>Updated the Volume Group<br>and Storage Container<br>Architecture section.|



© 2026 Nutanix, Inc. All rights reserved  | **66** 

Nutanix Files Storage 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|10.4<br>10.5<br>11.0<br>12.0<br>12.1|July 2025<br>Updated erasure coding<br>guidance in the Volume<br>Group and Storage Container<br>Architecture section.<br>August 2025<br>Updated document structure.<br>September 2025<br>Updated for Nutanix Files 5.2.<br>January 2026<br>Updated for Files Storage 5.3<br>and on-premises Data Lens.<br>April 2026<br>Updated the File Server<br>Virtual Machine Storage<br>Architecture and File Server<br>Virtual Machine Networking<br>sections.|



© 2026 Nutanix, Inc. All rights reserved  | **67** 

Nutanix Files Storage 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **68** 

Nutanix Files Storage 

## **List of Figures** 

Figure 1: Files Storage Architecture.............................................................................................................. 7 Figure 2: Data Path Architecture of Nutanix Files Storage............................................................................8 Figure 3: FSVM Internal Communication on One Node................................................................................9 Figure 4: Multiple Client Networks with Common and Unique Shares........................................................ 11 Figure 5: Initial Volume Group File System Pool.........................................................................................13 Figure 6: Distributed Directory Shares.........................................................................................................15 Figure 7: Three Standard Shares on the Same File Server........................................................................16 Figure 8: Submounting a Distributed Share into a Standard Share............................................................ 20 Figure 9: High Availability for File Server Volume Groups.......................................................................... 24 Figure 10: FSVM Ownership with Single Share and Volume Group Example............................................ 25 Figure 11: FSVM-1 Failure........................................................................................................................... 26 Figure 12: DNS Request for SMB............................................................................................................... 29 Figure 13: Unified Namespace Redirection................................................................................................. 34 Figure 14: DNS Request for NFSv3............................................................................................................ 37 Figure 15: Multiprotocol Shares................................................................................................................... 38 Figure 16: Smart Tier Architecture Example with Data Lens.......................................................................44 Figure 17: Files Storage Cloning Use Cases.............................................................................................. 49 Figure 18: Files Storage in a Metro Configuration.......................................................................................51 Figure 19: Files Storage Smart Disaster Recovery..................................................................................... 52 Figure 20: Data Consolidation Overview..................................................................................................... 54 Figure 21: VDI Sync Overview.....................................................................................................................55 Figure 22: Files Storage in AWS................................................................................................................. 57 Figure 23: ICAP Workflow............................................................................................................................60 

Figure 24: CFT Backup Process..................................................................................................................63 


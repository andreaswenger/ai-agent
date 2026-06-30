# **Files Storage Performance on Nutanix** 

## Legal 

© 2025 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Code samples and snippets that appear in this content are unofficial, are unsupported, and will require extensive modification before use in a production environment. As such, the code samples and snippets are provided AS IS and are not guaranteed to be complete, accurate, or up-to-date. Nutanix makes no representations or warranties of any kind, express or implied, as to the operation or content of the code samples or snippet. Nutanix expressly disclaims all other guarantees, warranties, conditions and representations of any kind, either express or implied, and whether arising under any statute, law, commercial use or otherwise, including implied warranties of merchantability, fitness for a particular purpose, title and non-infringement therein. 

This content reflects an experiment in a test environment. Results, benefits, savings, or other outcomes described depend on a variety of factors including use case, individual requirements, and operating environments, and this publication should not be construed as a promise or obligation to deliver specific outcomes. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Files Storage Performance on Nutanix 

## **Contents** 

**1. Executive Summary.................................................................................5 2. Files Storage.............................................................................................9 3. Four Corners Workload.........................................................................11** NFS Mixed Workload........................................................................................................................ 12 Media and Entertainment..................................................................................................................14 Advantages of nconnect................................................................................................................... 15 **4. Cloud Service Provider......................................................................... 17** Cloud Service Provider Tests........................................................................................................... 17 Cloud Service Provider Throughput Test Results.............................................................................18 Cloud Service Provider UI Performance Results............................................................................. 19 Cloud Service Provider Key Findings...............................................................................................20 **5. NFS Performance: Application Workloads..........................................21** Key Nutanix Performance Features..................................................................................................21 Enabling the Performance Profile.....................................................................................................23 Intensive Operations......................................................................................................................... 23 Software Compilations...................................................................................................................... 24 Electronic Chip Design Simulation....................................................................................................25 **6. File Share Type...................................................................................... 27** Random Share Type.........................................................................................................................28 Sequential Share Type......................................................................................................................29 **7. SMB Performance.................................................................................. 31** Windows Home Directory Scalability................................................................................................32 Containerized VDI Profiles on Files Storage....................................................................................35 Legacy File Server Migration to Files Storage................................................................................. 35 Performance Considerations for Files Storage Data Protection Features........................................37 

**8. Performance Scaling with Files Storage............................................. 39** Performance Scaling with Faster Networks......................................................................................39 **9. Files Storage Performance Recommendations Checklist................. 41** Additional Test Environment Configuration Notes............................................................................ 43 fio Four Corners Settings..................................................................................................................46 **About Nutanix.............................................................................................47 List of Figures.............................................................................................................................................48** 

Files Storage Performance on Nutanix 

## 1. Executive Summary 

The Files Storage solution provides a software-defined, scale-out file storage repository for unstructured data. This data can include home directories, user profiles, and departmental shares. It can also include application data, application logs, backups, and archives. Flexible and responsive to workload requirements, Files Storage is a fully integrated core component of Nutanix Cloud Platform that supports clients and servers connecting over SMB and NFS protocols. Files Storage offers native high availability and uses Nutanix storage for intracluster data resilience and intercluster asynchronous disaster recovery. Nutanix storage also extends data efficiency techniques to Files Storage, including erasure coding (EC-X) and compression. Nutanix supports Files with both ESXi and AHV. 

You can run Files Storage on-premises in a dedicated cluster or in a cluster running user VMs. Unlike standalone network-attached storage (NAS) appliances, Files Storage consolidates VM and file storage, eliminating an infrastructure silo. Administrators can manage Files with Nutanix Prism, just like they manage VM services, which unifies and simplifies management. Integration with Active Directory enables support for authentication, access-based enumeration, quotas, and the Self-Service Restore feature. Files Storage also supports file server cloning, which lets you back up Files Storage offsite and run antivirus scans and machine learning without affecting production. 

In the public cloud, Files Storage has the same features as it does on-premises. You can deploy Files Storage like an app running on Nutanix Cloud Clusters (NC2), or you can deploy it natively on AWS. Files Storage on AWS can be a cost-effective disaster recovery solution. You can replicate between on-premises deployments and the cloud or between cloud environments. 

We can measure performance with workloads—sets of processes that a computer (the file server, in this case) must complete in a certain amount of time. In this document, we describe the performance capabilities of Files Storage based on extensive testing with several well-known workload generators and copy tools. With this performance data, you can feel confident that Files Storage provides the performance required for many common workloads. 

© 2025 Nutanix, Inc. All rights reserved  | **5** 

Files Storage Performance on Nutanix 

File servers are appropriate for a growing number of workloads and use cases. This document details the performance capabilities of Files Storage on various common workloads, with results based on detailed testing and analysis conducted in the Performance Engineering labs at Nutanix. For our tests, we assessed several common workloads for both Linux (NFS) and Windows (SMB) environments. This document addresses the following applications, verticals, and performance recommendations to obtain the best performance from your Files Storage deployment: 

- NFS and SMB: 

   - › Four Corners Microbenchmark performance test (25 GbE, 100 GbE, and 200 GbE) 

   - › Media and entertainment 

- Cloud service providers 

- NFS performance for application workloads: 

   - › AI and machine learning training performance 

   - › AI image processing 

   - › Tar and GNU Compiler Collection (GCC) to extract and compile source code data set 

   - › Migration performance with Nutanix Move 5.3 

   - › Genetic analysis 

   - › Software compilations 

   - › Electronic chip design simulation 

- Workload-specific shares and single-client performance: 

   - › Random 

   - › Sequential 

© 2025 Nutanix, Inc. All rights reserved  | **6** 

Files Storage Performance on Nutanix 

- SMB: 

   - › Windows copy and paste to and from a Files Storage share (reads and writes) 

   - › Windows Robocopy 

   - › Microsoft File Server Capacity Tool (FSCT) 

   - › Containerized VDI profiles (such as FSLogix) on Files Storage 

- Migration performance with Nutanix Move 5.3 

- Performance considerations for Files Storage data protection features 

- Scaling performance 

New software releases can change file server system performance. Because Files Storage is a software-defined file server, performance results are likely to change and improve as new software versions and hardware platforms become available. The following list provides the performance-related changes in Files Storage version 5.0 and 5.1: 

- AI and machine learning training performance 

- Nutanix Move migration 

- Media and entertainment 

- AMD platform for AI and machine learning training deployments 

- Software compilation and electronic chip design workloads on an all-NVMe 100 GbE platform 

- NFS advantages for nconnect 

- Updated the settings recommended for performance 

You can often improve performance by upgrading file servers or deploying them with the latest release. However, you can only achieve some performance gains with new hardware, such as 100 GbE and all NVMe platforms. For recent improvements, see the latest Files Storage software release. 

For more information, see Files Storage documentation on the Nutanix Support Portal. _Table: Document Version History_ 

© 2025 Nutanix, Inc. All rights reserved  | **7** 

Files Storage Performance on Nutanix 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|March 2019|Original publication.|
|2.0|October 2019|Updated for Files Storage|
|||version 3.6.|
|3.0|September 2020|Updated for Files Storage|
|||version 3.7.|
|4.0|December 2021|Updated for Files Storage|
|||version 4.0.|
|5.0|April 2022|Updated for Files Storage|
|||version 4.1.|
|6.0|May 2023|Updated for Files Storage|
|||version 4.3.|
|7.0|December 2023|Updated for Files Storage|
|||version 4.4.|
|8.0|January 2025|Updated for Files Storage|
|||version 5.1. Added the|
|||Artificial Intelligence and|
|||Machine Learning Training|
|||section. Removed the Genetic|
|||Analysis section.|
|8.1|November 2025|Updated document structure|
|||and the Four Corners|
|||Workload with Files Storage,|
|||NFS Mixed Workloads, and|
|||Additional Test Environment|
|||Configuration Notes sections.|
|||Removed the Files Storage|
|||Hardware Performance|
|||Comparison, Linear Scalability,|
|||and Single Client Workloads|
|||sections.|
|8.2|December 2025|Updated the Four Corners|
|||Workload, NFS Performance:|
|||Application Workloads, and|
|||Performance Scaling with Files|
|||Storage sections.|



© 2025 Nutanix, Inc. All rights reserved  | **8** 

Files Storage Performance on Nutanix 

## 2. Files Storage 

Files Storage is a scale-out approach that provides Server Message Block (SMB) and Network File System (NFS) file services to clients. Files Storage server instances are composed of a set of VMs called file server VMs (FSVMs). Files Storage requires at least three FSVMs running on three nodes to satisfy a quorum for high availability. 

Figure 1: Files Storage Architecture 

For more information on Files Storage architecture, see Nutanix Files. For a complete list of Files Storage prerequisites, see the Prerequisites section of the Files Storage User Guide. 

Reviewing how we allocate CPU and memory resources between Files Storage and the Nutanix AOS Controller VM (CVM) can help optimize performance sizing. The simple model we provide in this document can help you think through the layers of the storage stack, but it doesn't comprehensively list the services in each OS. FSVMs primarily use CPUs for protocol work such as user connections, inodes and file handles, accesscontrol lists (ACLs) and permissions, metadata and caching, and network traffic. CVM CPUs interact with the underlying hardware and process the I/O that the file server sends 

© 2025 Nutanix, Inc. All rights reserved  | **9** 

Files Storage Performance on Nutanix 

to its volume groups. They also provide data path access, so workloads with significant I/ O and bandwidth requirements use them more heavily. 

To achieve the performance required for file server workloads, you must size your solution properly. Because Files Storage is software-defined, you can add hardware resources (including CPU, memory, nodes, shares, and so on) as your performance and capacity needs grow. 

Files Storage can run on a dedicated cluster or share compute and storage running alongside user VMs. In shared environments, the FSVMs share host CPU and memory with user VMs and CVMs. Use Files Storage nodes in shared environments for light to medium workloads where extra CPU, memory, and storage are available. 

Ensure that Files Storage workloads with high-performance requirements have sufficient resources to run optimally. If the combined workload is high, your performance can suffer in shared environments due to resource contention. For these workloads, we recommend using dedicated Files Storage clusters or providing enough hardware resources to avoid overcommitting CPU and memory. On a dedicated Files Storage system, resource contention isn't a concern because user VMs aren't competing for resources, and you can allocate the maximum amount of CPU and memory to the FSVMs. 

© 2025 Nutanix, Inc. All rights reserved  | **10** 

Files Storage Performance on Nutanix 

## 3. Four Corners Workload 

The first tests that we ran were the standard Four Corners Microbenchmark using the fio tool. 

The four workloads in order of completion: 

**1.** Random reads 

**2.** Sequential reads 

**3.** Random writes 

**4.** Sequential writes 

All random operations reported have a block size of 8 KB, and all sequential numbers are 1 MB sequential writes reported in throughput (megabytes per second). We chose this varied-workload approach because it provides insight into the diverse capabilities of the storage system. Sequential workload tests indicate the maximum throughput achievable on a system, and the small random workloads tend to be more processor intensive; a smaller I/O size means that the storage subsystems must process a larger number of input/output operations per second (IOPS). 

To take advantage of the latest Nutanix Unified Storage performance features, we conducted these tests using Network File System (NFS) traffic over Remote Direct Memory Access (RDMA) version 4.0 on a 200 GbE network. To fully use the capability of RDMA, we used Single Root I/O Virtualization (SR-IOV) to allow passthrough of host NICs to both the file server VMs (FSVMs) and to the Ubuntu 24.04 client VMs. We ran two types of random read tests. In one, the data resided completely in the FSVM cache. In the other, the data resided almost completely in permanent storage. These tests showcase the high performance of both the internal protocol processing stack as well as of the full storage I/O path. 

_Table: Four Corners Results with One and Eight Clients_ 

|**Workload**|**One Client (1 FSVM)**|**Eight Clients (4 FSVMs)**|
|---|---|---|
|Cached random reads (IOPS)|312,000|1,581,000|
|Uncached random reads|109,000|650,000|
|(IOPS)|||



© 2025 Nutanix, Inc. All rights reserved  | **11** 

Files Storage Performance on Nutanix 

|**Workload**|**One Client (1 FSVM)**<br>**Eight Clients (4 FSVMs)**|
|---|---|
|Sequential reads (MBps)<br>Random writes (IOPS)<br>Sequential writes (MBps)|20,100<br>83,600<br>39,400<br>178,000<br>3,460<br>12,000|



We obtained these results in a closed lab environment with a clean network and clients deployed on a cluster separate from Files Storage. We deployed Files Storage on a dedicated four-node NX-8170-G9-NVMe cluster. The FSVMs had 32 vCPUs and 256 GB of memory. We didn't deploy any other workloads or user VMs. 

We used multiple standard shares for the eight-client multinode tests. We used the NFS mount option `nconnect=16` to provide additional connections from client to server for the random read workload. 

For the random workloads, the clients saw very low maximum latency. For the singleclient tests, the cached random read latency averaged 0.1 ms, and the uncached random read latency averaged 0.29 ms. For the eight-client tests, the average client latency for cached random reads was 0.16. The average latency for uncached random reads with eight clients was 0.4 ms. The average random write latency for the eight-client tests was 1.4. All of the random write tests were run synchronously (fio option direct=1). For additional test notes, see the Files Storage Performance Recommendations Checklist section. 

## **NFS Mixed Workload** 

In this section, we show how Files Storage handles a high-throughput mixed read/ write workload on a 200 GbE network. We used Network File System (NFS) traffic over Remote Direct Memory Access (RDMA) with Single Root I/O Virtualization (SR-IOV) NICs on the file server VMs (FSVMs) and clients. We used eight Ubuntu 24.04 VMs running fio to drive the workload. This test measured maximum throughput for a mixed workload with high concurrency. The sequential tests used a 1 MB operation size. 

© 2025 Nutanix, Inc. All rights reserved  | **12** 

Files Storage Performance on Nutanix 

Figure 2: NFS High Throughput, Mixed Sequential Workload, Four Nodes 

_Table: NFS over RDMA High Throughput, Mixed Sequential Workload, Four Nodes_ 

|**Read/Write Percentage**|**Read Throughput (MBps)**|**Write Throughput (MBps)**|
|---|---|---|
|100% read, 0% write|83,600|0|
|75% read, 25% write|47,070|8,200|
|50% read, 50% write|12,198|10,800|
|25% read, 75% write|5,250|12,300|
|0% read, 100% write|0|14,000|



We also ran a mixed random workload test. The operation size for these tests was 8 KB. 

© 2025 Nutanix, Inc. All rights reserved  | **13** 

Files Storage Performance on Nutanix 

Figure 3: NFS High Throughput, Mixed Random Workload, Four Nodes 

_Table: NFS over RDMA High Throughput, Mixed Random Workload, Four Nodes_ 

|**Read/Write Percentage**|**Read Throughput (IOPS)**|**Write Throughput (IOPS)**|
|---|---|---|
|100% read, 0% write|650,000|0|
|75% read, 25% write|428,000|39,200|
|50% read, 50% write|278,000|77,600|
|25% read, 75% write|134,000|123,000|
|0% read, 100% write|0|178,000|



## **Media and Entertainment** 

We used Frametest to test how Files Storage handles a raw video streaming workload. This benchmark simulates reading and writing video data to storage. The data files created by the benchmark are similar to raw video frames, which are compressible, so 

© 2025 Nutanix, Inc. All rights reserved  | **14** 

Files Storage Performance on Nutanix 

we kept default compression enabled on the file shares. For more information, see the How to use frametest article. 

This benchmark measures both the total throughput of the operations and the frames per second (FPS) achieved by the client. You can specify the size of video frames with Frametest, so we chose to run our tests with 4K video frames. We tested with CentOS EL8 Linux clients and used one standard share per client; however, you can achieve the same results using a distributed share with a top-level directory for each client in the share. 

The following tables provide the throughput and FPS measured on the clients. Each test read or wrote 100,000 video frames, and each frame was approximately 48 MB. The total amount of data read or written during each test was 4 TB. We ran as many frames per second as possible on each client, resulting in a high-throughput workload. 

## _Table: Frametest Reads_ 

|**Number of Clients**|**Frames per Second**|**Throughput (MBps)**|
|---|---|---|
|1|107|5,251|
|4|102 (Average)|20,111|



_Table: Frametest Writes_ 

|**Number of Clients**|**Frames per Second**|**Throughput (MBps)**|
|---|---|---|
|1|52|2,541|
|4|52 (Average)|10,141|



We ran the tests on an NX-8170-G8 NVMe cluster with 2 × 100 GbE NICs configured in an LACP bond. Each client reports its FPS value, and the throughput value is the total throughput as measured on all clients. The four-client FPS values are the average of all four clients. The number of dropped frames was negligible in each test, amounting to less than 0.01 percent. 

## **Advantages of nconnect** 

Starting with kernel version 5.3 and later, Linux distributions implement the `nconnect` NFS mount option. This option allows multiple connections from a single client to the server, which can significantly improve the parallelism of single-client workloads. Using this 

© 2025 Nutanix, Inc. All rights reserved  | **15** 

Files Storage Performance on Nutanix 

option can provide many advantages for workloads that require high throughput with few clients. 

Our tests found that `nconnect=8` is the optimal setting in many cases. Your optimal setting might depend on the network bandwidth available and your client's number of CPUs. As a general rule, use as many nconnect connections as you have CPUs on your client. 

The nconnect option can improve your throughput for low client count environments. We compared two fio sequential read test runs from the same client to the same file server. In the first test run, we used a typical NFSv4.2 mount without nconnect: 

```
Run status group 0 (all jobs):
   READ: bw=2474MiB/s (2594MB/s)
```

In the second run, we remounted the NFS share using the mount option `nconnect=8` : 

```
Run status group 0 (all jobs):
   READ: bw=6970MiB/s (7309MB/s)
```

The throughput for this single client test increased by roughly 280 percent. 

We used the following fio command-line options and mount options for the Files Storage standard share: 

```
fio --runtime=600 --filename=/home/nutanix/std2/newtest --readwrite=read --
size=1000g --name=quicktest --ioengine=libaio --blocksize=1m --direct=1 --
iodepth=32 --time_based --numjobs=8
```

```
std2 on /home/nutanix/std2 type nfs4
 (rw,relatime,vers=4.2,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,
  nconnect=8,timeo=600,retrans=2,sec=sys,local_lock=none
```

© 2025 Nutanix, Inc. All rights reserved  | **16** 

Files Storage Performance on Nutanix 

## 4. Cloud Service Provider 

We constantly test the performance of Files Storage using various workloads and conditions. To this end, we set up an environment that mimics a cloud service provider. In this type of environment, users from various domains and namespaces require access to different types of data with varying workloads. To keep data and namespaces separate, users can create dozens of file servers on a single large AOS cluster. 

To determine whether Files Storage works well for this scenario—a multitenant service provider solution—we tested the performance, measured in throughput, IOPS, and latency. We also tested the administrator UI experience. We found that Files Storage works well for a multitenant service provider solution. 

We used the following configuration for our testing: 

- 6 × NX-8170-G8 node AOS cluster 

- 8 NVMe per node 

- 755 GB of available memory per node 

- 2 × 32 cores, Intel Xeon Platinum 8352Y CPU @ 2.20 GHz 

- AOS 6.5.2 

- Files Storage 4.3 

- NFS 4.2 

- CentOS 8 (kernel 5.14) clients 

## **Cloud Service Provider Tests** 

We tested a heavy workload against various numbers of file servers. Each file server had three file server VMs (FSVMs) with four vCPUs and 16 GB of memory each, one standard-purpose share, and one distributed share. Our CentOS clients mounted distributed shares on each file server and used multiple directories to shard the data and load evenly across all FSVMs. 

© 2025 Nutanix, Inc. All rights reserved  | **17** 

Files Storage Performance on Nutanix 

For each standard share, Files Storage creates a volume group composed of four data disks, one metadata disk, and one SLOG write log disk. Each distributed share creates five volume groups. Each of the test configurations had thousands of virtual disks hosted on AOS and accessed by Files Storage at any one time. 

We changed the total number of file servers to determine the recommended maximum number of FSVMs per AOS node. With tests at or under this maximum, we found that the UI experience remained good, and the total throughput of each workload was consistent. 

We used fio 3.7 running on CentOS 8 VMs to generate the workload. To understand how the configuration performed under an extremely heavy load, we used outstanding I/O and clients to attempt to saturate the underlying storage. During this load, we also measured the time it took the system to complete some common UI administration tasks. 

We used the following workloads: 

- 8 KB random reads 

- 1 MB sequential reads 

- 1 MB sequential writes 

- 8 KB random writes, asynchronous (direct=0) 

- 8 KB random writes, synchronous (direct=1) 

## **Cloud Service Provider Throughput Test Results** 

After testing various quantities of file server VMs (FSVMs), we recommend that you limit the FSVMs per AOS node in a cluster to 25. We obtained optimal performance on our six-node AOS cluster with a total of 150 FSVMs and 50 file server clusters (three nodes each). 

## _Table: Throughput Results from Five Workloads_ 

|**Workload**|**Per File Server**|**Total**|
|---|---|---|
|8 KB random read (IOPS)|9,400|470,000|
|1 MB sequential read (MBps)|360|18,000|
|8 KB random write (async)|2,120|106,000|
|(IOPS)|||



© 2025 Nutanix, Inc. All rights reserved  | **18** 

Files Storage Performance on Nutanix 

|**Workload**|**Per File Server**<br>**Total**|
|---|---|
|8 KB random write (sync)<br>(IOPS)<br>1 MB sequential write (MBps)|1,500<br>75,000<br>142<br>7,100|



The workload was extremely heavy during this testing and CPU usage on the FSVMs, Controller VM (CVM), or AHV host was at or near 100 percent during these test runs. The per–file server performance results in the previous table aren't results you might see under normal operation. In cases where more resources are available on the AOS cluster, the user's throughput is higher. We conducted this testing to study the worst-case scenario for a system at or near 100 percent utilization. 

## **Cloud Service Provider UI Performance Results** 

The following table shows how long particular administrative tasks took from the user's perspective. We collected these values during the heaviest portion of the workload, where CPU usage was at or near 100 percent and UI response was slowest. For comparison, we also provide the amount of time the UI tasks took on an idle system with no workload. In both of these cases, we created and ran the file server VMs and shares but didn't access them. 

_Table: UI Task Times_ 

|**UI Task**|**Idle System in Seconds**|**Maximum Workload in**|
|---|---|---|
|||**Seconds**|
|List Prism VMs|2|9|
|List Prism file server page|10|33|
|Create standard purpose|39|90|
|shares|||
|Create distributed shares|15|16|
|Create file servers|730|1,723|



© 2025 Nutanix, Inc. All rights reserved  | **19** 

Files Storage Performance on Nutanix 

## **Cloud Service Provider Key Findings** 

Our testing results show that the configuration performed best with 25 file server VMs (FSVMs) per AOS node, which we recommend as the maximum for any highFSVM-count scenario. Additionally, the final number of file servers per node in your environment depends on how much memory and CPU you allocate to each file server. We recommend against oversubscribing resources if possible. 

Comparing the UI response time of Files Storage 4.3 with Files 4.2.1, we found that many UI tasks took 50 percent less time than they took in 4.2.1. We also noted a 10 percent increase in sequential read and write performance in this service provider scenario. For these reasons, we recommend running Files Storage 4.3 or later in a service provider environment. 

We also found that increasing the amount of memory allocated to the Controller VMs (CVMs) to 64 GB improved the write performance of Files Storage by 10 percent, and increasing the CVM vCPU count to 20 (from the default of 16) improved write performance by the same amount. Therefore, we recommend increasing the amount of memory and vCPU allocated to the CVMs in a service provider environment. The amount to increase depends on the hardware. In our case, we increased memory from 48 GB to 64 GB and vCPU count from 16 to 20. 

To provide enough resources, we recommend running with at least 2 × 20 physical CPU cores. For the absolute best performance, we recommend not overcommitting CPU unless you know that the typical workloads are light and can ensure that the cluster has enough memory so that you don't overcommit. If you have any questions, contact your system engineer or account team. 

© 2025 Nutanix, Inc. All rights reserved  | **20** 

Files Storage Performance on Nutanix 

## 5. NFS Performance: Application Workloads 

NAS workloads typically perform many operation types—not just read and write. Microbenchmarks like fio are great at stressing the I/O or block subsystems, but they aren't as good at heavily using the protocol and file system components, which can involve activities like reading directories, enumerating and setting user permissions and access, and deleting files and directories. 

The following sections demonstrate the capabilities of Files Storage to meet the performance requirements of real-world applications. Whether quickly tarring a set of data, compiling code, storing machine learning training data sets, or completing a myriad of other use cases, Files Storage can provide the performance required for enterprise applications. 

To test use cases that include modern applications and workloads, we worked with the well known benchmark MLperf Storage produced by MLcommons. This benchmark software simulates various types of AI training workloads. For the officially published results of Nutanix with MLperf, see Nutanix Unified Storage Pushes AI Performance Further. 

For information on requirements and configuration for running high throughput AI training workloads on NUS, see Managing the NUS Performance Profile. 

## **Key Nutanix Performance Features** 

Nutanix offers the following performance features for highly demanding AI and machine learning training workloads: 

© 2025 Nutanix, Inc. All rights reserved  | **21** 

Files Storage Performance on Nutanix 

- Files Storage Performance Profile: 

   - › Files Storage 5.0 includes a new gflag option to enable the performance profile. We recommend enabling this feature when you first create the file server cluster. 

   - › When you enable the performance profile, the local node of the file server VM (FSVM) hosts all volume group disks. For some workloads, this feature provides lower latency and increases throughput. 

   - › AI training workloads benefit greatly from the performance profile. 

   - › The performance profile causes several changes. Most notably, new shares and volume groups use a different path rather than the traditional iSCSI, hypervisor attached disks. 

   - › Additional vmnic queues for the FSVMs offer enhanced network CPU distribution. 

   - › The performance profile adds data vDisks in the volume group. 

- iSER: 

   - › iSER offloads some of the network traffic using a local hardware NIC to handle the virtual network traffic between the Controller VM (CVM) and FSVM, rather than passing it through the AHV vSwitch. 

   - › This feature creates a fast path for communication between the FSVMs and CVMs and lowers overall latency, which is beneficial on very high throughput AI training workloads. 

   - › You must deploy iSER on a host with at least 3 x 100 GbE NVIDIA Mellanox CX-6 NICs in the following configuration: 

      - 2 NICs in an LACP bond 

      - 1 NIC for iSER 

   - › iSER uses the sequential share type and distributed shares. 

© 2025 Nutanix, Inc. All rights reserved  | **22** 

Files Storage Performance on Nutanix 

- Modern Linux: 

   - › Files Storage supports modern Linux clients (for example, Ubuntu). 

   - › Files Storage can run on clients with the following configurations: 

      - Deployment host with 25 GbE, 100 GbE, or 200 GbE with multiple NICs in an LACP bond 

      - Eight additional vmnic queues 

      - nconnect=16 

## **Enabling the Performance Profile** 

To enable the performance profile, follow these steps: 

**1.** Upload the Files Storage software to Prism. 

**2.** Connect to the Controller VM (CVM) over SSH. 

**3.** Run the following command before deploying the file server: 

```
nutanix@cvm$ afs infra.set_performance_profile true
```

**4.** Run the following command on the CVM after creating the file server: 

```
nutanix@cvm$ afs.infra.set_max_read_performance_profile <fs-name>
```

**5.** Restart Stargate: 

```
nutanix@cvm$ allssh genesis stop stargate && cluster start
```

## **Intensive Operations** 

We used tar and GNU Compiler Collection (GCC) to simulate the intensive operations used by chip semiconductors and software compilation workflows. We set up a distributed share and ran a heavy, file-based workload with 25 and 40 users to illustrate how the system scales under load. Each user performed operations against approximately 100,000 files with the following steps: 

**1.** Use the tar command to extract a large file into the share that contains the source code. 

**2.** Run GCC to compile the source code. 

© 2025 Nutanix, Inc. All rights reserved  | **23** 

Files Storage Performance on Nutanix 

This complex set of file operations stressed the Files Storage protocol and metadata stacks and the storage subsystem provided by AOS. 

For the NFS workload with 25 users, the tar extraction took 6 minutes and 48 seconds, and the GCC compilation took 37 minutes and 40 seconds. For the workload with 40 users, tar extraction took 8 minutes and 3 seconds, and the GCC compilation took 45 minutes and 33 seconds. These results measure the time required to complete the simultaneous jobs running on multiple users, and they indicate that the distributed share is highly optimized for multiple clients running multiple streams of work. 

To achieve optimal performance for workloads with high file counts, configure the file server VMs (FSVMs) with sufficient CPU and memory resources. Check the VM performance graphs in Prism to monitor CPU and memory usage, and use the FSVM configuration menu to hot-upgrade when needed. 

## **Software Compilations** 

We tested a simulated developer environment where multiple clients perform software compilations simultaneously. This test simulates the workload performed in a production environment by utilities such as Linux `make` , which automatically builds executable programs and libraries from source code. The software compilation workload primarily performs statistics and other metadata operations, so it's metadata heavy. The following table shows the breakdown of specific operation types in this workload. 

_Table: Software Compilation Operation Types_ 

|**Operation**|**Percentage**|
|---|---|
|read file|6|
|write file|7|
|access|6|
|chmod|5|
|create|1|
|stat|70|
|mkdir|1|
|readdir|2|
|unlink|2|



© 2025 Nutanix, Inc. All rights reserved  | **24** 

Files Storage Performance on Nutanix 

To be considered successful, the average storage latency of the compilation workload had to be less than 10 ms, which it was (1 ms). We obtained the following results on a four-node cluster: 

- Client type: NFS 

- Compilations (per FSVM): 107 

- Total compilations with the maximum FSVM count (estimated): 3,424 

**Note:** We ran the tests on an NX-8170-G8 all-NVMe cluster. 

## **Electronic Chip Design Simulation** 

We simulated a chip design environment where multiple clients perform sets of operations, modeling the software suites that work on everything from specifications to fabrications. This simulation runs many compute operations against millions of small files and a subset of large files. The following tables show the breakdown of specific operation types in the two workloads that make up the simulation. 

_Table: Chip Design Simulation Operations Workload 1_ 

|**Operation**|**Percentage**|
|---|---|
|read file|7|
|write file|10|
|access|15|
|chmod|1|
|create|2|
|stat|39|
|mkdir|1|
|unlink 1|1|
|unlink 2|1|
|rand read|8|
|rand write|15|



_Table: Chip Design Simulation Operations Workload 2_ 

© 2025 Nutanix, Inc. All rights reserved  | **25** 

Files Storage Performance on Nutanix 

|**Operation**|**Percentage**|
|---|---|
|read|50|
|write|50|



The electronic design workload is challenging and composed of many metadata operations and small block reads and writes. We obtained the following results on a seven-node cluster: 

- Client type: NFS 

- Chip design simulations per FSVM: 128 

- Total chip design simulations with the maximum FSVM count (estimated): 4,096 

**Note:** We ran the tests on an NX-8170-G8 all-NVMe cluster. 

© 2025 Nutanix, Inc. All rights reserved  | **26** 

Files Storage Performance on Nutanix 

## 6. File Share Type 

Files Storage offers multiple share types to accommodate different workloads. The default share type provides a good balance between small and large operations and should be optimal for mixed workloads. Setting the share type to random has a positive impact on random writes, and setting the share type to sequential can increase the amount of sequential throughput possible on the share. Each share type has its benefits, but the random setting decreases sequential performance, and the sequential setting decreases random performance. 

If you know that the workload isn't mixed, you can set the share type to either random or sequential to increase the maximum performance of the share, but you must know the workload on the share before changing the share type. To assess your workload, use the following command to obtain information about I/O size on the share: 

```
afs share.io_size_distribution
```

The following code block is an example of the output of the `afs share.io_size_distribution` command for a share named `dist1` . Each row of the output shows the count of I/O operations of a particular size in bytes, ranging from 512 to 1,048,576 bytes: 

```
nutanix@NTNX-10-56-66-109-A-FSVM:~$ afs share.io_size_distribution dist1
Waiting for task completion: 98239b80-6d66-4934-b6bd-ce542e01b3d4
  Task status: kNotStarted, Time elapsed: 0 sec
Read IO Size Distribution:
IO Size             Count
512                 117
1024                0
2048                0
4096                0
8192                20191439
16384               14
32768               4
65536               1
131072              6
262144              3
524288              0
1048576             123
Write IO Size Distribution:
IO Size             Count
512                 52070674
1024                102
2048                122
```

© 2025 Nutanix, Inc. All rights reserved  | **27** 

Files Storage Performance on Nutanix 

```
4096                103
8192                20210429
16384               103
32768               104
65536               25
131072              2
262144              2
524288              7
1048576             182
```

In the example output, the I/O sizes that show the majority of operations on share `dist1` are 512 bytes and 8,192 bytes (8 KB). In this case, setting the share type to random can optimize the share for small I/O operations and increase its throughput capability. 

If the majority of your workload reports 1 MB operation sizes (for example, writing out large files, log ingestion throughput, backups, and so on), setting the share type to sequential increases your performance. The random share type improves performance when overwriting files with small I/O operations (16 KB or smaller). 

## **Random Share Type** 

Setting a share to the random type decreases the file system block size, which is beneficial for operations like small random overwrites into larger files. OS operations on a virtual disk in a virtual desktop infrastructure (VDI) environment are a good example of this workload type. 

The following chart contains performance test results that show the potential benefits of using the random share type when most operations in your workload are 16 KB or smaller. 

© 2025 Nutanix, Inc. All rights reserved  | **28** 

Files Storage Performance on Nutanix 

Figure 4: Random Share Type Comparison, 8 KB Random Write IOPS 

_Table: Random Share Type Comparison, 8 KB Random Write IOPS_ 

|**Share Type**|**Throughput (IOPS)**|
|---|---|
|Default|48,000|
|Random|68,000|



## **Sequential Share Type** 

Setting the share to sequential increases the file system block size, which can improve performance of large sequential workloads including large file copies, log files, and backups. The following chart shows a single Windows client writing a large file sequentially, which simulates a backup or large file copy from a Windows server to a file server share. Note that even with a single client, throughput scales with network speeds. We recommend deploying nodes with 100 GbE for the most demanding workloads. 

© 2025 Nutanix, Inc. All rights reserved  | **29** 

Files Storage Performance on Nutanix 

Figure 5: Sequential Share Type Comparison, Sequential Write Workload on a Single Client 

_Table: Sequential Share Type Comparison, Sequential Write Workload on a Single Client_ 

|**Share Type**|**Throughput (IOPS)**|
|---|---|
|Default|2,700|
|Sequential|3,300|



© 2025 Nutanix, Inc. All rights reserved  | **30** 

Files Storage Performance on Nutanix 

## 7. SMB Performance 

The first test that many users try with a file server is to copy and paste or move some data from a Windows client to a file share. This process isn’t an ideal test for a file server because it tends to be limited by the client. More sophisticated copy tools on Windows, such as Robocopy, provide you with some control over the application, including the ability to use multiple threads to increase copy speed. Nutanix tests both copy methods regularly to measure our performance. 

The following figures show the results we achieved when copying a large file from the client to a server. The 100 GbE results are for a client and Files Storage instance deployed on 100 GbE networks. The 25 GbE results are for a client and Files Storage instance deployed on 25 GbE. Note that the performance scales with network speed to some degree. However, the performance doesn't scale the entire network bandwidth because the Windows copy application limits the outstanding I/O. Applications that use multiple threads, connections, and outstanding I/O might see a higher total performance. 

Figure 6: Windows Client Copy and Paste to and from Files Storage 

© 2025 Nutanix, Inc. All rights reserved  | **31** 

Files Storage Performance on Nutanix 

## _Windows Client Copy and Paste to and from Files Storage_ 

|**I/O Type**|**Network Speed**|**Throughput (GBps)**|
|---|---|---|
|Large file read|100 GbE|1.8|
|Large file read|25 GbE|1.0|
|Large file write|100 GbE|1.5|
|Large file write|25 GbE|0.8|



**Note:** We ran the tests on an NX-8170-G8 all-NVMe cluster. 

We also tested Robocopy with large files, achieving speeds similar to those of copy and paste. However, sets of small files tend to be slower; larger data sets can require thousands to millions of metadata operations. These operations include find, create or open, close, and so on. Combined with reads and writes of the actual data, a large number of metadata operations can require significant time. 

Robocopy offers several specific options that can negatively affect performance. Even using the default Robocopy settings that output the console log of all files and folders copied to the destination can reduce throughput. The best Robocopy options for your environment depend on your business requirements, but for our testing, we used some Robocopy flags to optimize performance. We achieved around 80 MBps for a data set with 50 GB of text and images (181,250 files, 57,493 folders). The following command provides an example of the Robocopy flags we used: 

```
robocopy $sourceDir $targetDir /COPY:DATSO /S /E /DCOPY:T /mt:64 /NFL /NDL /
NP /LOG:"C:\log\small-robocopy-$shortdate.log"
```

Robocopy settings are independent of file server options, and decisions regarding these settings or flags aren't related to the file server solution you use. It's important to research and understand the Robocopy flags that you use to ensure data validity for data migrations or backups. 

## **Windows Home Directory Scalability** 

Files Storage works well as a storage solution for Windows home directories and user profiles. As each user connects to a folder or share served by a file server VM (FSVM), they create an SMB connection. We regularly stress test Files Storage with a Microsoft File Server Capacity Tool (FSCT) workload to ensure sustainable, consistent home 

© 2025 Nutanix, Inc. All rights reserved  | **32** 

Files Storage Performance on Nutanix 

directory performance. The chart in the following figure breaks down the workload mix of FSCT; it's heavy on small metadata operations, with thousands of connected users. 

**Note:** This chart only displays operations that make up at least 2 percent of the operation mix. 

Figure 7: FSCT SMB Operation Mix 

## _Table: FSCT SMB Operation Mix_ 

|**Operation Type**|**Percentage of Workload**|
|---|---|
|Session Setup|3|
|Tree Connect|2|
|Create|22|
|Close|19|
|Read|14|
|Write|7|
|Ioctl|4|
|Query Directory|10|
|Query Info|13|



A single four-node Files Storage cluster can support 13,800 simultaneous FSCT users. 

© 2025 Nutanix, Inc. All rights reserved  | **33** 

Files Storage Performance on Nutanix 

Files Storage also supports access-based enumeration (ABE). With the ABE feature enabled, clients don't attempt to enumerate directories that the user can't access. Because of the overhead involved in processing extra metadata, enabling ABE might impact performance. How much ABE can affect performance depends on the number of groups and ACLs in the environment. If you have concerns about using ABE, contact Nutanix Support. 

Based on memory analysis and capacity testing, we defined the following configuration limits in software tied to FSVM hardware. We stress-tested these limits with Windows VDI clients to ensure that they work well for general single user–to–single share connections. The following table presents user count numbers per node; we expect these numbers to scale with the Files Storage cluster node count. 

_Table: Connection Count System Limits_ 

|**vCPU Count**|**Memory (GB) per**|**User Count**|
|---|---|---|
||**Files Storage Node**||
|12|96|4,000|
|8|64|3,250|
|8|40|2,750|
|6|32|2,000|
|6|24|1,500|
|4|16|1,000|
|4|12|500|



The connection count system limits listed in the preceding table don't apply for terminal servers (a server that enables multiple client systems to connect to a LAN network without using a modem or a network interface). Two common terminal server solutions are Windows Server Remote Desktop Services (formerly known as Windows Terminal Services) and Omnissa Horizon. Each of these solutions allows multiple users to access Files Storage through one connection through one server. With the density of users on one connection, this use case has higher CPU processing and memory requirements. 

Windows Remote Desktop Services Host can connect to Files Storage for profile storage. If users experience sign-in delays, we recommend reducing the number of users per Windows Remote Desktop Services Host to 50. Files Storage does not have system 

© 2025 Nutanix, Inc. All rights reserved  | **34** 

Files Storage Performance on Nutanix 

hardware or software limits that enforce this recommendation; it's up to you to design with this guidance in mind to achieve balanced density and consistent performance. 

Terminal servers work well with Files Storage when properly designed. For more information, see the End-User Computing (EUC) Solutions page on the Nutanix website. 

## **Containerized VDI Profiles on Files Storage** 

The performance requirements for traditional user profile storage solutions (like folder redirection) are well understood. These requirements are automatically factored into the solution when using the Nutanix Sizer tool and selecting Files Storage as the workload and VDI home directories as the use case. 

Containerized user profile solutions like FSLogix have different I/O patterns and performance requirements on the underlying storage. Containerized profiles usually have higher IOPS requirements for each user because the data appears as a local disk on the client. These solutions also require continuously available shares on the SMB server, which can increase latency for user writes. 

For a typical VDI user, the guidelines for storage performance recommend, on average, 50 IOPS per user while logging on and off and 10 IOPS per user in the steady state. Actual IOPS can vary based on the applications used. The Files Sizing Guide documents the industry guidance on IOPS per user, and it recommends that these solutions be sized with the VDI Users use case and the Container based user data storage type under the File Services (Files Storage) workload type in the Nutanix Sizer tool. 

To simplify sizing, we created a new use case in the Nutanix Sizer tool for containerized user profiles. You can streamline your sizing process by entering the number of active users and, if known, the IOPS per user. For more information, see the Files Sizing Guide. 

## **Legacy File Server Migration to Files Storage** 

In addition to enabling VM migration, Nutanix Move can help you migrate file shares from legacy NFS or SMB servers to Nutanix Unified Storage. For more information on migrating data to Files Storage, see the Files Migration Guide. 

To aid in fast and efficient file copies, Nutanix Move isn't directly in the data path for the copy. Its role is to enumerate the legacy servers’ file shares and then to command and control Files Storage for the migration process. Nutanix Move uses APIs to tell the 

© 2025 Nutanix, Inc. All rights reserved  | **35** 

Files Storage Performance on Nutanix 

Files Storage server to directly mount the shares from the legacy file server and then read and pull the data. This process makes migration very efficient because you don't require a server in the middle to perform the copy process. Additionally, Files Storage can perform asynchronous reads with high concurrency so that a lot of data is in flight. With this method, the copy is performed as quickly as possible. 

We ran comparison tests between Nutanix Move and other traditional methods of migrating file shares, specifically rsync for NFS and Robocopy for SMB. We used three publicly available data sets, each with different characteristics and data types: 

- Data Set 1: Composed mostly of text files 

- Data Set 2: Larger than Data Set 1, with more files and many non-compressible image files 

- Data Set 3: An old public data set containing several million files, including Personal Storage Table (PST) email files, with many directories and different file sizes 

## **Nutanix Move and rsync Over NFS Comparison** 

The following tables provide the results of our migration tests comparing rsync and Nutanix Move. 

## _Table: Data Set 1: 800,000 Files, 392 GB_ 

|**Method**|**Copy Time**|**Copy Time Improvement**|
|---|---|---|
|rsync|124 minutes|-|
|Nutanix Move|23 minutes|539%|



_Table: Data Set 2: 88,000 Files, 90 GB_ 

|**Method**|**Copy Time**|**Copy Time Improvement**|
|---|---|---|
|rsync|9 minutes|-|
|Nutanix Move|4 minutes|225%|



## _Table: Data Set 3: 2,200,000 Files, 382 GB_ 

|**Method**|**Copy Time**|**Copy Time Improvement**|
|---|---|---|
|rsync|217 minutes|-|



© 2025 Nutanix, Inc. All rights reserved  | **36** 

Files Storage Performance on Nutanix 

|**Method**|**Copy Time**<br>**Copy Time Improvement**|
|---|---|
|Nutanix Move|38 minutes<br>571%|



## **Nutanix Move and Robocopy Over SMB (16 Threads) Comparison** 

The following tables provide the results of our migration tests comparing Robocopy and Nutanix Move. 

_Table: Data Set 1: 800,000 Files, 392 GB_ 

|**Method**|**Copy Time**|**Copy Time Improvement**|
|---|---|---|
|Robocopy|81 minutes|-|
|Nutanix Move|29 minutes|279%|



_Table: Data Set 2: 88,000 Files, 90 GB_ 

|**Method**|**Copy Time**|**Copy Time Improvement**|
|---|---|---|
|Robocopy|16 minutes|-|
|Nutanix Move|9 minutes|178%|



_Table: Data Set 3: 2,200,000 Files, 382 GB_ 

|**Method**|**Copy Time**|**Copy Time Improvement**|
|---|---|---|
|Robocopy|192 minutes|-|
|Nutanix Move|93 minutes|206%|



## **Performance Considerations for Files Storage Data Protection Features** 

We performed extensive testing to ensure an ideal experience when using the Smart Disaster Recovery (Smart DR), Smart Tiering, and Smart Sync features provided by Files Storage. Our goal was to enable the best possible performance of these features while still preserving the throughput capability of user workloads. 

For Smart DR, we recorded our test results during a heavily loaded worst-case condition —users were performing an extremely heavy workload during a large disaster recovery transfer. Real-world workloads are rarely as heavy as these test conditions; however, sizing for the worst-case scenario helps prevent problems when the system is deployed. 

© 2025 Nutanix, Inc. All rights reserved  | **37** 

Files Storage Performance on Nutanix 

When sizing your system to work with these data protection features, we recommend providing additional overhead to your existing workloads. For example, if your workload produces 10,000 IOPS, size for an additional 10 percent overhead (11,000 IOPS). The Nutanix Sizer tool can account for these data protection features when you size a Files Storage workload. 

When using Files Storage data protection features, we recommend sizing with the following additional percentages of overhead in mind: 

- Smart DR: 15 percent 

- Smart Tiering: 10 percent 

- Smart Sync: 10 percent 

In general during our testing, these features didn't affect the client or user throughput by any more than 7 to 8 percent, even though the system was running at or near its I/O capacity. 

However, we noticed a rare case where user performance was affected more when running Smart Sync. This additional performance overhead only occurs during the initialization of several large data sets because Smart Sync scans the file system to be synced. This scan's metadata and data are read into the system's cache, so the file-system buffer cache usage increases by that amount. If the file server VM (FSVM) memory is not large enough to contain it, cache evictions can occur. 

If your current user random read workload is very highly cached, IOPS can drop because more data has to be read from disk during this time. We saw the main difference with a 100 percent cached read test. When several large syncs occurred, we noticed a 20 to 30 percent decrease in 8 KB random read IOPS during the initialization phase. Again, this is a worst-case scenario, and it doesn't reflect the impact on a busy system that reads data from both the cache and disks. 

© 2025 Nutanix, Inc. All rights reserved  | **38** 

Files Storage Performance on Nutanix 

## 8. Performance Scaling with Files Storage 

From Files Storage 5.0 onward, you can configure each file server VM (FSVM) with up to 32 vCPUs and 512 GB of memory. This configuration allows dedicated Files Storage instances to use hardware efficiently. If the workload requires it, each FSVM can use all the cores on one socket while the Controller VM (CVM) uses all the cores on the other. The additional memory also provides a larger FSVM cache for larger working sets, potentially speeding data access. 

The increased memory and CPU let you cache much larger sets of metadata and data than in previous releases, which is helpful for enterprise applications with a known hot data set because almost all the data can fit into the cache. For example, for a fournode cluster where each FSVM has 512 GB of data and the data set is 50 percent compressible, you can cache over 2 TB of data. We tested a 100 percent cached random read workload, comparing 12 vCPUs to 16 vCPUs for the FSVM. We saw a 20 percent gain in IOPS, achieving over 600,000 IOPS for a four-node cluster. 

With an NX-8170-G9 running the Files Performance Profile along with iSER and Network File System (NFS) over Remote Direct Memory Access (RDMA), we achieved over 1.5 million cached read IOPs on a four-node cluster. The FSVMs had 32 vCPUs and 512 GB of RAM. This test demonstrates the value of increasing FSVM CPU and memory when the resources are available. 

## **Performance Scaling with Faster Networks** 

Modern applications like AI and big data need high-speed networks. We tested clients and Files Storage clusters deployed on 100 GbE networks and found that performance improved for both Network File System (NFS) and SMB workloads. 

The following chart provides the performance results for a single Windows client using fio to write a large file to Files Storage, simulating log ingestion throughput. This scenario doesn't show the maximum throughput capabilities of the Files Storage instance because we only tested a single client. Additional clients might demonstrate higher performance by using the additional network and resource capabilities of the file server. 

© 2025 Nutanix, Inc. All rights reserved  | **39** 

Files Storage Performance on Nutanix 

Figure 8: Performance Comparison 100 GbE vs. 25 GbE 

_Table: Performance Comparison 100 GbE vs. 25 GbE_ 

|**Network Speed**|**Log Ingestion Throughput (GBps)**|
|---|---|
|25 GbE|1.0|
|100 GbE|2.7|



© 2025 Nutanix, Inc. All rights reserved  | **40** 

Files Storage Performance on Nutanix 

## 9. Files Storage Performance Recommendations Checklist 

This checklist for using Files Storage provides the settings and configuration information for our tests. 

For maximum sustained performance, deploy Files Storage as a dedicated cluster using the performance profile. For more information, see Managing the Performance Profile in the Files User Guide. 

When testing performance, use one dedicated cluster for clients and one for Files Storage and the Controller VM (CVM) only. 

CPU and memory: 

- For dedicated Files Storage deployments, we recommend assigning all CPU and memory resources to the CVM and Files. Devoting one NUMA node's CPU and memory to the file server VM (FSVM) and the other NUMA node to the CVM ensures that you fully use all hardware resources. 

- For heavy I/O (random or sequential) environments, increase the CPU count for CVMs. 

- Increase FSVM memory to increase the cache size for larger data sets. 

- For environments with higher file counts, dedicate more CPU and memory to the FSVMs. 

- For environments that use access-based enumeration (ABE), continuous availability shares, and SMB encryption, we recommend allocating more CPU to the FSVMs to exceed the default configuration. 

Networking: 

- When possible, use balance-slb for active-active load balancing of traffic with AHV. 

- If you configured your switch for Link Aggregation Control Protocol (LACP), use balance-tcp. 

© 2025 Nutanix, Inc. All rights reserved  | **41** 

Files Storage Performance on Nutanix 

- We recommend using at least 25 GbE network adapters for Files Storage deployments. For the most demanding workloads, like AI and machine learning training, we suggest using 100 GbE network adapters between clients and servers. Files Storage performance can decrease when running over clusters with a single 10 GbE link or 1 GbE links. 

- For the best performance, use the following network port configurations: 

   - › 2 × 100 GbE ports for the CVM and FSVM networks 

   - › 1 × 100 GbE port for Remote Direct Memory Access (RDMA) for CVM-to-CVM traffic 

   - › 1 × 100 GbE port for iSER (iSCSI over RDMA) 

Client tuning for NFS clients: 

- For clients deployed on AHV, configure multiple VM NIC queues: 

```
acli vm.nic_update nfsclient-4 50:6b:8d:d7:6e:07 queues=8
```

- Mount FSVMs with asynchronous options from the NFS client, unless the application specifically requires you to do otherwise. If you use a synchronous mount, all write operations are synchronous, which can decrease write performance. 

- To improve performance for NFS clients with demanding workload requirements, we recommend using nconnect because it allows for multiple connections between clients and FSVMs. We see performance improve for some workloads from 50 to 100 percent. 

- Specify the nconnect value on the client at mount time using the `nconnect=<value>` mount option. We recommend using an nconnect value of 4, 8, 12 or 16. For more information, see the Advantages of nconnect section. 

Data protection features: 

- We recommend configuring your FSVMs with additional resources when using Smart Sync, Smart Tier, or Smart Disaster Recovery (Smart DR), especially if you use multiple features. For example, if your FSVMs use six vCPUs, increase vCPU count to eight. 

© 2025 Nutanix, Inc. All rights reserved  | **42** 

Files Storage Performance on Nutanix 

- When running Smart Sync, we recommend configuring your FSVMs with more memory to increase the file system cache. Adding memory can improve random read performance during the initialization of large data sets. 

## General: 

- Don't configure more than 50 users per Windows Remote Desktop Services Host connecting to a Files Storage share. 

- Use Nutanix Sizer and the Files Storage Sizing Guide (links require portal credentials) when designing a Files Storage solution for performance. 

- When sizing an EUC or VDI environment, it's important to understand how the user profiles are hosted and configured. If you use a containerized profile solution such as FSLogix, use the containerized VDI sizing profile under the Nutanix Sizer tool for Files Storage. 

- Use the following FSVM setting to optimally distribute top-level directories in a distributed share for AI and machine learning workloads: 

```
afs fs.tld_distribution_type distribution_type=2
```

- For large-memory file server deployments, you can allow the file system buffer cache to grow to a maximum of 90 percent of the system memory. Dedicated deployments that have FSVMs configured with 128 GB of memory or more can use this tool. This change improves performance for read-heavy workloads like AI and machine learning. You can enable this feature using the following command on the FSVM: 

```
afs misc.update_zfs_params zfs_arc_max_pct=90
```

## **Additional Test Environment Configuration Notes** 

We used the following Nutanix hardware and software to host the Nutanix Unified Storage file servers and data for on-premises testing: 

© 2025 Nutanix, Inc. All rights reserved  | **43** 

Files Storage Performance on Nutanix 

- AMD NX-8155-G9 file server cluster (per node): 

   - › 12 × NVMe (all-NVMe platform) 

   - › 2 × AMD EPYC 9274F 24-core processors 

   - › 2 × 100 GbE NVIDIA Mellanox (CX-6) cards (LACP bond) 

   - › 1 × 100 GbE iSER 

   - › CVM: 24 vCPU, 64 GB of memory per node 

   - › FSVM: 24 vCPU, 512 GB of memory 

- Nutanix NX-8170-G9-NVMe cluster (per node): 

   - › 12 × NVMe (Kioxia 13.84 TB PCI-SSD) 

   - › Dual 32-core Intel Xeon Gold 6548Y CPU @ 2.5 GHz per node 

   - › 1 × 200 GbE Mellanox (CX-7) cards (OVS network) 

   - › 1 × 200 GbE RDMA for CVM to CVM 

   - › 1 × 200 GbE iSER 

   - › 1 × 200 GbE SR-IOV FSVM external network for NFS over RDMA 

   - › CVM: 32 vCPUs, 64 GB of memory per node 

   - › FSVM: 256 GB of memory 

We used the following hardware and software to host Files Storage natively on the public cloud (AWS): 

© 2025 Nutanix, Inc. All rights reserved  | **44** 

Files Storage Performance on Nutanix 

- EC2 x2idn.32xlarge: 

   - › 1 × 100 GbE 

   - › 4 × 900G EBS IO2 volumes, 5,000 IOPS per volume 

   - › 1 × 20G EBS IO2 volume, 5,000 IOPS per volume 

   - › 1 × 16G EBS GP2 volume, 100 IOPS per volume 

   - › 2 × 4G EBS GP3 volume, 3,000 IOPS per volume 

   - › 1 × 50G EBS GP3 volume, 3,000 IOPS per volume 

   - › Sequential share type, distributed shares 

- EC2 C6in.32xlarge: 

   - › 1 × 200 GbE 

   - › 4 × 500G EBS IO2 volumes, 14,000 IOPS per volume 

   - › 1 × 20G EBS IO2 volume, 14,000 IOPS per volume 

   - › 1 × 16G EBS GP2 volume, 100 IOPS per volume 

   - › 2 × 4G EBS GP3 volume, 3,000 IOPS per volume 

   - › 1 × 50G EBS GP3 volume, 3,000 IOPS per volume 

   - › Sequential share type, distributed shares 

The following list provides the client hosting details: 

- NFS clients had a range of CPU and memory configurations 

- For SR-IOV NFS over RDMA testing, each client host had 2 × client VMs with 32 vCPUs, 64 GB RAM, and 1 × 200 GbE Mellanox CX-7 NIC per VM (SR-IOV passthrough) 

- Windows Server 2016 Build 14393 and Windows Server 2019 Build 

- Nutanix AHV versions 10.3 and AHV 20230302.1004376 

- Nutanix AOS versions 6.9 and 7.3 

- Multiple Files Storage versions 5.1.0, 5.2.0, and 5.3.0 

© 2025 Nutanix, Inc. All rights reserved  | **45** 

Files Storage Performance on Nutanix 

- Nutanix X-Ray version 4.3.1 

Unless otherwise stated, we ran all tests against file shares that were evenly balanced across cluster nodes. For single-client tests, we ran the workload against a single node and file share. To maximize performance, we set up the cluster with an active-active networking configuration. To ensure that we used both physical NICs, we used balancetcp with LACP for load balancing in AHV. For more information on AHV network load balancing, see the AHV Networking Best Practices. We also recommend LACP for VMware vSphere environments. For more information about LACP in vSphere, see the VMware vSphere Networking Best Practices. By providing solid hardware and networking and separate client clusters, we ensured that the clients and network weren’t bottlenecks. This approach allowed for consistent performance testing of the Files Storage cluster, engaging all nodes and shares. 

We simulated customer workloads to the best of our ability, but real-world workloads vary across applications and customer environments. Additionally, we obtained these results in a closed lab setting on an isolated system running no other competing workloads. Because performance varies based on platform CPU count, CPU speed, physical storage type (NVMe, SSD, or HDD), data size, and other factors, results obtained in other environments might vary. However, you can improve your results for Files Storage in any situation by following some of the practices discussed in this document. 

We performed all tests with standard Nutanix space-saving and data protection features enabled, including Files Storage share-level compression and a fault tolerance level of 1. 

## **fio Four Corners Settings** 

We used a standard set of configuration values for fio, whether running it on Linux or Windows. Each test run was 10 minutes. Each client addressed a 12 GB or 16 GB file for Linux and Windows clients, respectively. We ran random write tests with the fio direct option set to 1. We ran all random read tests with **direct=1** to bypass the client read cache. We initially ran these workloads with varying amounts of concurrency (outstanding I/O or iodepth) to push the system as close to its maximum capabilities as possible. For X-Ray testing, we used an iodepth of 64 per client for all tests. 

© 2025 Nutanix, Inc. All rights reserved  | **46** 

Files Storage Performance on Nutanix 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2025 Nutanix, Inc. All rights reserved  | **47** 

## **List of Figures** 

Figure 1: Files Storage Architecture.............................................................................................................. 9 Figure 2: NFS High Throughput, Mixed Sequential Workload, Four Nodes................................................ 13 Figure 3: NFS High Throughput, Mixed Random Workload, Four Nodes................................................... 14 Figure 4: Random Share Type Comparison, 8 KB Random Write IOPS.....................................................29 Figure 5: Sequential Share Type Comparison, Sequential Write Workload on a Single Client....................30 Figure 6: Windows Client Copy and Paste to and from Files Storage........................................................ 31 Figure 7: FSCT SMB Operation Mix............................................................................................................33 Figure 8: Performance Comparison 100 GbE vs. 25 GbE.......................................................................... 40 


```
v2.3 | January 2026 | NVD-
```
##### NUTANIX VALIDATED DESIGN

# Nutanix GPT-in-a-Box with

# Nutanix Kubernetes® Platform

# Design


## Legal

```
© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix
product and service names mentioned are registered trademarks or trademarks of
Nutanix, Inc. in the United States and other countries. Kubernetes is a registered
trademark of The Linux Foundation in the United States and other countries. All other
brand names mentioned are for identification purposes only and may be the trademarks
of their respective holder(s).
Certain information contained in this content may link or refer to, or be based on,
studies, publications, surveys, and other data obtained from third-party sources and
our own internal estimates and research. While we believe these third-party studies,
publications, surveys, and other data are reliable as of the date of publication, they have
not independently verified unless specifically stated, and we make no representation as
to the adequacy, fairness, accuracy, or completeness of any information obtained from
a third-party. Our decision to publish, link to or reference third-party data should not be
considered an endorsement of any such content.
Nutanix, Inc.
1740 Technology Drive
San Jose, CA 95110
```

## Contents



## 1. Executive Summary.................................................................................

```
Many enterprises need to run and manage large language models (LLM) and AI
workloads on their private infrastructure for greater security and control. This Nutanix
Validated Design (NVD) supports the deployment of an innovative, flexible, and secure
generative pretrained transformer (GPT) solution on Nutanix Cloud Platform that can
run across private and public clouds and edge locations. Nutanix can deliver this
NVD as the GPT-in-a-Box solution, which provides a full-stack enterprise platform for
scoping, designing, installing, and testing generative AI (GenAI) and machine learning
applications.
```

- 1. Executive Summary.................................................................................
      - Nutanix GPT-in-a-Box Software Versions...........................................................................................
- 2. Nutanix Terminology..............................................................................
- 3. Core Infrastructure Design for Nutanix GPT-in-a-Box.......................
      - Scalability Design for Nutanix GPT-in-a-Box....................................................................................
      - Resilience Design for Nutanix GPT-in-a-Box...................................................................................
      - Cluster Design for Nutanix GPT-in-a-Box.........................................................................................
      - Storage Design for Nutanix GPT-in-a-Box.......................................................................................
      - Network Design for Nutanix GPT-in-a-Box.......................................................................................
      - Nutanix Unified Storage Design for Nutanix GPT-in-a-Box..............................................................
      - Nutanix Database Service Design for Nutanix GPT-in-a-Box..........................................................
      - Management Components for Nutanix GPT-in-a-Box......................................................................
      - Monitoring Design for Nutanix GPT-in-a-Box...................................................................................
      - Security and Compliance for Nutanix GPT-in-a-Box........................................................................
      - Infrastructure for Nutanix GPT-in-a-Box...........................................................................................
- 4. Kubernetes Cluster Design for Nutanix GPT-in-a-Box.......................
      - Kubernetes Cluster Resilience and Networking for Nutanix GPT-in-a-Box......................................
      - Kubernetes Cluster Monitoring and Backup for Nutanix GPT-in-a-Box............................................
   - Box........................................................................................................... 5. Large Language Model Application Design for Nutanix GPT-in-a-
      - Large Language Model Conceptual Design for Nutanix GPT-in-a-Box............................................
      - Large Language Model Logical Design for Nutanix GPT-in-a-Box..................................................
      - Research and Document Ingestion Workflows for Nutanix GPT-in-a-Box.......................................
- 6. Backup and Disaster Recovery for Nutanix GPT-in-a-Box................
- 7. Ordering Nutanix GPT-in-a-Box Deployments....................................
      - Sizing Considerations........................................................................................................................
      - Nutanix GPT-in-a-Box Cluster Bill of Materials................................................................................
   - Nutanix Professional Services..........................................................................................................
- 8. References and Resources for Nutanix GPT-in-a-Box.......................
- About Nutanix.............................................................................................
- List of Figures.............................................................................................................................................
- Figure 1: Architectural Layers of the Nutanix Validated Design for GPT-in-a-Box 2.


Key features:

- A centralized deployment in one availability zone in one region in an on-premises
    datacenter
- A single Nutanix cluster that hosts the Nutanix management components, including
    the Nutanix Prism Central solution, and Nutanix Unified Storage software to provide
    Network File System (NFS) storage and S3-compatible storage capabilities
- An enterprise-grade Kubernetes solution with the Nutanix Kubernetes® Platform
    (NKP) solution, which simplifies the processes for deploying, scaling, and managing
    containerized applications across hybrid and multicloud environments
- Independent PostgreSQL database workflows, provisioned by the Nutanix Database
    Service (NDB) software, for the Nutanix Enterprise AI back end and for user
    applications that require pgvector for semantic search and retrieval-augmented
    generation (RAG) pipelines
- A single-cloud operating model using Nutanix Cloud Platform hyperconverged
    infrastructure with graphic processing units (GPUs)

Advantages:

- Total data management, security, privacy, and resilience with Nutanix Unified Storage
- An enterprise-grade inference endpoint from Nutanix Enterprise AI to deploy your
    choice of LLMs from leading providers and create and manage secure APIs to connect
    your GenAI applications
- A Nutanix Professional Services engagement that includes planning and design
    workshops
- A full-stack focus with integrated data privacy installed using opinionated software

Benefits:

- Faster time to value: A validated design with a comprehensive bill of materials
    accelerates deployment and reduces trial and error.
- Improved IT agility: You can rapidly provision and scale workloads in response to
    business needs.


- Reduced risk: Built-in backup and disaster recovery protect applications and the entire
    infrastructure to provide business continuity.
- Lower total cost of ownership: Efficient resource utilization and automation reduce
    operational overhead.

This NVD is just one example of a supported GPT configuration. You can design and
build a GPT solution on Nutanix in many ways, and you can deviate from this specific
configuration while still following Nutanix best practices.

Nutanix rigorously tests and documents solutions in the NVD program to align with
best practices and support faster, more resilient deployments, giving IT teams a trusted
foundation for building enterprise-grade private cloud environments.

_Table: Document Version History_

```
Version Number Published Notes
1.0 May 2024 Original publication.
1.1 June 2024 Updated the Rack Layout
image, the Scalability Design
Decisions, GPT-in-a-Box
Cluster: Hardware, and GPT-
in-a-Box Cluster: Per-Node
Hardware Configuration
tables, and the Core
Infrastructure Design,
Scalability, and Rack Design
sections.
1.2 September 2024 Updated the Ordering section.
2.0 January 2025 Significant updates throughout
for GPT-in-a-Box 2.0, NKP,
AMX support for embedding,
and NX-9151-G9.
2.1 March 2025 Added the Nutanix Enterprise
AI version, Lenovo GPU
sizing, and GPT-in-a-Box
Cluster: Professional Services
SKUs table.
```

```
Version Number Published Notes
2.2 June 2025 Added NDB to the design.
Updated NKP to Version 2.14.
2.3 January 2026 Updated document structure.
```
#### Nutanix GPT-in-a-Box Software Versions...........................................................................................

```
The following table summarizes the complete package of software that Nutanix validated
for functionality and interoperability with this solution.
Table: Software Versions Used in Validation Testing
Component Software Version
Prism Central 2024.3.0.
Nutanix AOS 7.
Nutanix AHV 10.
Nutanix Foundation 5.
Nutanix Life Cycle Manager (LCM) 3.
Nutanix Cluster Check (NCC) 5.
NVIDIA nvidia_aie_10.0.0.1-11_535.216.
Nutanix Kubernetes Platform (NKP) 2.
Nutanix Enterprise AI 2.
Files Storage 5.
Objects Storage 5.
Nutanix Database Services 2.8.
Nutanix Database Service Operator for
Kubernetes
```
```
0.5.
```

## 2. Nutanix Terminology..............................................................................

```
This document uses the following terms to refer to different elements of the Nutanix
hybrid cloud solution.
Cluster
A cluster is the management boundary of the storage provided to a group of
workloads.
Region
Regions are the geographic areas where you deploy datacenters. Different regions
are far enough away from each other that they are unlikely to be impacted by the
same natural disasters, power grid outages, and other events as other regions.
Nutanix defines a region as a datacenter location where round-trip latency is
greater than 5 ms but less than 100 ms.
Availability zone
Availability zones (AZs) are physically and logically separated datacenters
or datacenter rooms with independent power sources, networks, and cooling
connected with an extremely low-latency network. With Nutanix, Prism Central
manages these components.
Management domain
A management domain is a logical construct that refers to components such as
Nutanix Cloud Manager, application domain Prism Central instances, Foundation
or Foundation Central, Nutanix Central, and vCenter Server that are deployed on
dedicated Nutanix clusters located in a separate security zone.
Application domain
An application domain is a logical construct that refers to the Prism Element
clusters in a single AZ and the Prism Central instance that they're registered to.
Pod
A pod is a group of resources managed by the Prism Central instances in the two
management clusters, and isn't bound by physical location.
```

**Block**

```
A block is a Nutanix cluster or a pair of clusters that are located in different AZs.
```
**Fault domain**

```
Fault domains are groups of VMs that share a common power source, network
infrastructure, server rack, Nutanix cluster, or datacenter location.
```

## 3. Core Infrastructure Design for Nutanix GPT-in-a-Box.......................

```
The following lists provide core infrastructure design requirements, assumptions, risks,
and constraints.
Core infrastructure design requirements by component:
```
- Management:
    › Provide a unified management and control plane to VMs and Kubernetes runtimes.
    › Configure the management plane to integrate with Active Directory for
       authentication.
    › Use Active Directory–based groups for access control.
    › Provide automated infrastructure and application deployment.
    › Provision PostgreSQL databases for application back ends and vector search.
    › Provide Kubernetes-based automated database provisioning and life cycle
       management of PostgreSQL instances.
- Business continuity and disaster recovery (BCDR): Ensure persistent inference
    availability.
- Performance:
    › Support GPU passthrough for VMs and Kubernetes workloads.
    › Provide high-bandwidth, low-latency shared storage for the AI application's data.
- Environment: Optimize hardware and software for maximum performance.
- Scale: Support a single Nutanix GPT-in-a-Box cluster that can scale up to 16 nodes
    with a minimum of eight GPUs in the cluster.
- Resilience: Provide enough GPU resources to run GPU-enabled workloads in a failure
    scenario.


- Hardware:

```
› Provide hardware with GPU support using NVIDIA L40S.
› Use hardware that supports up to four GPU cards per node.
› Use servers with 100 Gbps network uplinks.
› Use NVMe disks for optimal performance.
```
- Monitoring:

```
› Monitor performance metrics and store historical data for the past 12 months.
› Monitor resources that are critical to Nutanix AOS operations (for example, CPU,
memory, storage, and network resources); resource usage that exceeds configured
limits generates an alert.
› For resources that have high availability reservations, measure the resource
utilization threshold against the usable capacity after subtracting the capacity
reserved for high availability.
› Use email as the primary channel for event monitoring alerts.
› Ensure that event monitoring is resilient.
For example, when the management plane is the primary source of alerts,
use a secondary method to monitor the management plane itself. If the
management plane fails, an alert from the secondary source can trigger the
action to recover the management plane.
› Use consolidated monitoring for all deployed Nutanix Kubernetes Platform (NKP)
clusters.
```
- Security and compliance:

```
› Use role-based access control (RBAC) to follow the least-privilege rule.
› Support network microsegmentation.
```
- Networking: Use Link Aggregation Control Protocol (LACP) to optimize networking
    traffic.

Core infrastructure design assumptions by component:


- Networking: Top-of-rack switches provide a 100 Gbps connection.
- Monitoring:

```
› IT operations teams can continuously staff the mailbox that receives monitoring
alerts to address critical issues in a timely manner.
› IT operations teams can provide email infrastructure with sufficient resilience to
send, receive, and access emails even during critical outages.
```
- Infrastructure: You have existing infrastructure components like Dynamic Host
    Configuration Protocol (DHCP), Domain Name System (DNS), and Active Directory
    available.
- BCDR:

```
› A single Nutanix cluster hosts the infrastructure.
› Replication isn't configured.
› Velero explicitly backs up stateful workloads in the Kubernetes cluster occasionally
with the BackupStorageLocation pointing to a bucket in the Nutanix Objects Storage
solution to save the backups external to the NKP cluster. In the event of a failure,
you can restore the workload from the backup by pointing to the same backup
location.
› Objects Storage buckets replicate to external destinations.
```
Core infrastructure design risks by component:

- Availability: A clusterwide outage (power outage, PDU failure, availability zone (AZ)
    failure, software stack failure) causes services to be unavailable.
- Monitoring:

```
› If Prism Central becomes unavailable for any reason, the platform can no longer
send alerts. To mitigate this risk, configure each Prism Element instance to send
alerts as well. This approach results in duplicate alerts during normal operations, so
send Prism Element alerts to a different mailbox that you can monitor when Prism
Central is unavailable.
› If the Kubernetes management cluster becomes unavailable, the platform can't
send Kubernetes-related alerts or record Kubernetes performance metrics.
```

```
Core infrastructure design constraints by component:
```
- Scalability:
    › The number of inference endpoints and models is limited to the number of GPUs
       available in a single node.
    › The maximum number of nodes in the GPT clusters is 16, and the maximum
       number of GPUs depends on the hardware you choose.
- Budget: Budget is limited.
- Hardware: The solution uses NVIDIA GPUs.
- Application: AI applications can't use vGPU with this validated design.
The conceptual pod design has the following features:
- Single AZ in a single region in an on-premises datacenter
- A single Nutanix cluster that hosts the following services, among others:
    › Nutanix management components, including Prism Central
    › Nutanix Unified Storage to provide Network File System (NFS) storage and S3-
       compatible storage capabilities
    › NKP workload clusters used for Nutanix Enterprise AI and GenAI workload-stack
    › NKP management cluster used for deployments and observability

#### Scalability Design for Nutanix GPT-in-a-Box....................................................................................

```
Scalability is one of the core concepts of the Nutanix platform and refers to the ability
to increase storage and compute capacity to meet both current and future workload
demands. A well-designed cluster meets current requirements while providing a path to
support future growth.
This NVD permits horizontal and vertical scaling within the boundaries of maximum
cluster size. For more information on Nutanix scalability concepts, see the Hybrid
Cloud: AOS 6.5 with AHV On-Premises Design and the Enterprise Edge with Artificial
Intelligence: AOS 6.5 with AHV Design.
```

```
This NVD supports a single GPT-in-a-Box cluster with a defined GPU capacity of eight
GPU cards using a standardized node's hardware configuration. This approach provides
maximum flexibility and ease of management. If the GPU, memory, or CPU pressure
increases, add nodes to the cluster. If storage demand grows, add drives to the existing
nodes or add nodes to the cluster. This design uses all-flash storage to accommodate
peak workload demands.
When using the Nutanix NX-Platform, you can scale compute and storage by adding
NX-8155-G9 nodes. You can scale GPU resources independently by adding NX-9151-G
nodes.
Table: Scalability Design Decisions
Design Option Validated Selection
Node memory population Ensure that node memory is evenly distributed.
Node drive type Use all-flash NVMe drives.
Drive bays Partially populate the drive bays.
Rack availability Don't use rack availability.
Available cluster sizes Use a single GPT-in-a-Box cluster with a
starting size of four nodes on Lenovo or HPE
or six nodes on NX (four HCI nodes and two
GPU nodes).
```
#### Resilience Design for Nutanix GPT-in-a-Box...................................................................................

```
Nutanix provides many resilience features, including storage replication, snapshots,
block awareness, degraded node detection, and self-healing. These capabilities
increase the resilience of all workloads, even if the application itself has limited resilience
options. Nutanix layers these software features on hardware designed with resilience
in mind (for example, with redundant physical components and power supplies, many
of which are hot-swappable or otherwise easily serviceable). Running workloads in a
virtualized environment adds another kind of resilience, because you can perform many
maintenance operations without application downtime. A resilient network fabric that can
sustain individual link, node, or block failures without significant impact completes the
architecture.
```

```
Many components are physically redundant. The physical components include the top-
of-rack switches, the nodes and their internal parts, and the datacenter itself in case of a
disaster.
For more information on how to protect workloads and meet or exceed service-level
agreements (SLAs), see the Hybrid Cloud: AOS 6.5 with AHV On-Premises Design and
Enterprise Edge with Artificial Intelligence: AOS 6.5 with AHV Design.
Table: Resilience Design Decisions
Design Option Validated Selection
Full redundancy of all components Ensure the full redundancy of all components
in the datacenter.
Storage replication factor Use storage replication factor 2.
Compute fault tolerance Use fault tolerance of 1 (n + 1).
Availability zone Use a single availability zone with no
replication.
```
#### Cluster Design for Nutanix GPT-in-a-Box.........................................................................................

```
The design incorporates a single GPU-enabled Nutanix cluster dedicated to GPT-in-a-
Box workloads that offers access to LLMs. Supporting management applications like
Prism Central and file and object storage are hosted on this cluster.
Size the Nutanix GPT-in-a-Box cluster Controller VM (CVM) with 16 vCPUs and 64 GB of
memory.
Use the following cluster naming convention:
<cluster_type><cluster_instance_number><hypervisor><cluster_suffix_number>.
For example, gpt01ahv01 is the name of the first GPT cluster on AHV.
This NVD uses one region with a single AZ that hosts the GPT-in-a-Box cluster. This
NVD doesn't use any replication targets to protect the workloads because the focus is to
host the GPT workloads in a single cluster.
```

```
Figure 2: GPT-in-a-Box Nutanix Cluster Conceptual Architecture
```
_Table: Cluster Design Decisions_

```
Design Option Validated Selection
Cluster size Ensure the full redundancy of all components
in the datacenter.
CPU Use at least 24 cores and a high clock rate.
Minimum cluster size Use at least four nodes.
Cluster expansion Expand in increments of 1.
Maximum cluster size Use at most 16 nodes.
```

```
Design Option Validated Selection
Networking Use 100 GbE networking.
Cluster replication factor Use storage replication factor 2.
Cluster high availability configuration Guarantee high availability.
VM high availability Enable high availability reservation on the
clusters.
```
```
For platform details, see the Nutanix GPT-in-a-Box Cluster Bill of Materials section of this
document.
```
**Nutanix GPT-in-a-Box Cluster Resilience**

```
VM high availability ensures that VMs restart on another AHV host in the AHV cluster
when a host becomes unavailable, either because the original AHV host has a complete
failure or becomes network partitioned or because of an AHV host management process
failure. When the AHV host where a VM is running becomes unavailable, the VM turns
off; therefore, from the perspective of the VM operating system, VM high availability
involves a full VM start cycle.
Table: High Availability Configuration
Feature Description Setting
High availability reservation Guarantee compute failover
capacity within cluster for
application
```
```
Enabled
```
```
Rebuild capacity reservation Guarantee storage rebuild
capacity within cluster
```
```
Enabled
```
#### Storage Design for Nutanix GPT-in-a-Box.......................................................................................

```
Nutanix uses a distributed, shared-nothing architecture for storage. For information on
node types, counts, and physical configurations, see the Cluster Design for Nutanix GPT-
in-a-Box section in this document.
Creating a cluster automatically creates the following storage containers:
```

- NutanixManagementShare: Used for Nutanix features like the Nutanix Files Storage
    software and Objects Storage and other internal storage needs; doesn't store
    workload vDisks
- SelfServiceContainer: Used by the Nutanix Cloud Manager (NCM) Self-Service Portal
    to provision VMs
- Default-Container-XXXX: Used by VMs to store vDisks for user VMs and applications
In addition to the automatically created storage containers, the following additional
storage containers are created during the Files Storage and Objects Storage
deployment:
- NTNX_<fileserver_name>_ctr: Provides storage for the file server instances, which
    provide Network File System (NFS) storage for the application tier
- objectsd<uniqueidentifier>: Data container for Objects Storage
- objectsm<uniqueidentifier>: Metadata container for Objects Storage
The Default-Container stores VMs and their vDisks. The additional containers for Files
Storage and Objects Storage are created throughout the deployment process of the
respective components.

**Nutanix GPT-in-a-Box Data Reduction and Resilience Options**

```
To increase the effective capacity of the cluster, the design enables inline compression
on all storage containers. It doesn't use additional functionalities such as deduplication
or erasure coding. Replication factor 2 protects against the loss of a single component in
case of failure or maintenance.
Table: Data Reduction Settings
Container Compression Deduplication Erasure Coding Replication
Factor
Default-
Container-XXXX
```
```
On Off Off 2
```
```
Nutanix
ManagementShare
```
```
On Off Off 2
```
```
SelfService
Container
```
```
On Off Off 2
```

```
Container Compression Deduplication Erasure Coding Replication
Factor
NTNX_files_ctr On (60 min.) Off Off 2
Objects
containers
```
```
On Off Off 2
```
The following table provides information about the storage decisions made for this
design.

_Table: Storage Design Decisions_

```
Design Option Validated Selection
Sizing a cluster Use an all-flash cluster to provide low-latency,
high-throughput storage to support the
application's active data set.
Node type vendors Don't mix node types from different vendors in
the same cluster.
Node and disk types Use similar node types that have similar disks.
Don't mix nodes that contain NVMe SSDs
in the same cluster with hybrid SSD or HDD
nodes.
Sizing for node redundancy for storage and
compute
```
```
Size all clusters for n + 1 failover capacity.
```
```
Fault tolerance and replication factor settings Configure the cluster for fault tolerance 1 and
configure the container for replication factor 2.
Inline compression Enable inline compression.
Deduplication Don't enable deduplication.
Erasure coding Don't enable erasure coding.
Availability domain for cluster Use node awareness.
Storage containers in cluster The cluster has the following storage
containers: NutanixManagementShare,
SelfServiceContainer, Default-Container,
NTNX_files_ctr, and the two objects storage
containers.
Reserve rebuild capacity Enable reserve rebuild capacity.
```

#### Network Design for Nutanix GPT-in-a-Box.......................................................................................

```
A Nutanix cluster can tolerate multiple simultaneous failures because it maintains a set
redundancy factor and offers features such as block awareness and rack awareness.
However, this level of resilience requires a highly available network connecting a cluster's
nodes.
Nutanix clusters send each write to another node in the cluster. As a result, a fully
populated cluster sends storage replication traffic in a full mesh, using network bandwidth
between all Nutanix nodes. Because storage write latency directly correlates to the
network latency between Nutanix nodes, any increase in network latency adds to storage
write latency. Protecting the cluster's read and write storage capabilities requires highly
available connectivity between nodes. Even with intelligent data placement, if network
connectivity between multiple nodes is interrupted or becomes unstable, VMs on the
cluster can experience write failures and enter read-only mode.
Use datacenter-grade switches designed to handle high-bandwidth server and storage
traffic at low latency. This NVD uses a leaf-spine network topology, which requires at
least two spine switches and two leaf switches, and LACP at the switch end. Every leaf
connects to every spine using 100 GbE uplink ports and connects to every node using a
100 GbE virtual switch. For more information, see Physical Networking Best Practices.
```

```
Figure 3: GPT-in-a-Box Physical Network Architecture
```
For more information on deploying a network to achieve the necessary resilience levels,
see the Nutanix On-Premises Hybrid Cloud with AHV Design and the Nutanix Enterprise
Edge with Artificial Intelligence Design.

_Table: Network Design Decisions_

```
Design Option Validated Selection
Datacenter switch Use large-buffer nonblocking switches.
Network topology Use the highly available leaf-spine network
topology.
Top-of-rack switches Populate each rack with 2 × 100 GbE top-of-
rack switches.
```

```
Design Option Validated Selection
Controller VM (CVM) and hypervisor VLAN Configure the CVM and hypervisor VLAN as
native, or untagged, on server-facing switch
ports.
Switch ports for guest workloads Use tagged VLANs on the switch ports for all
guest workloads.
Virtual switches Use one vs0 virtual switch with two of the
fastest uplinks of the same speed.
Logical network separation Use VLANs to separate logical networks.
Number of required VLANs and subnets The design requires four VLANs and subnets:
one for CVM and hypervisor management,
one for Integrated Lights-Out out-of-band
management, one for the management
network, and one for the client front-end
network.
```
#### Nutanix Unified Storage Design for Nutanix GPT-in-a-Box..............................................................

```
For this NVD, Files Storage provides high-performance, shared storage to Nutanix
Kubernetes Platform (NKP) using the Nutanix Container Storage Interface (CSI). Nutanix
Enterprise AI stores the LLMs in ReadWriteMany (RWX) PersistentVolumeClaims
(PVCs) to make them available to multiple endpoints. Use an additional Network
File System (NFS) share to provide custom models that you can import into Nutanix
Enterprise AI.
Objects Storage provides S3-compatible storage capabilities to the application, and you
can upload new data models to Objects Storage for import into Nutanix Enterprise AI.
Files Storage and Objects Storage run on the same Nutanix cluster as the GPT-in-a-Box
workloads. If you plan to expand the environment, you can also run Files Storage and
Objects Storage in a dedicated cluster.
Use Prism Central to manage all the components.
```
**Files Storage, Objects Storage, and Nutanix Kubernetes Platform Network Design**

```
Files Storage and Objects Storage have the following network requirements:
```

- For maximum performance, the storage network for Files Storage uses the same
    subnet as the CVMs.
- To maximize security, the client network connects to a separate subnet.
- The GPT-in-a-Box cluster that provides a single file server instance requires four
    storage network IP addresses and three client network IP addresses for its three file
    server VMs (FSVMs).

Objects Storage runs as a containerized service on a Kubernetes microservices platform,
which provides benefits such as increased velocity of new features. Several of the
required storage IP addresses are for functions related to the underlying microservices
platform. You must also manage these networks. Objects Storage with three worker
nodes requires seven storage network IP addresses and two client network IP
addresses.

Each cluster provisioned by NKP has minimum IP address requirements. A Kubernetes
cluster in a production-level layout with three worker nodes requires one static IP
address for the control plane, six or more IPAM addresses for VMs, and a range of four
or more IP addresses for the Kubernetes LoadBalancer Services.

The client network provides all IP addresses. You might need additional IPAM addresses
for additional worker nodes.

The following table provides information about the Files Storage and Objects Storage
decisions made for this design.

_Table: Files Storage Design Decisions_

```
Design Option Validated Selection
FSVM cluster size Use three FSVMs.
vCPU and memory for FSVM Use 12 vCPUs and 64 GB of memory for each
FSVM.
Storage networking Keep the storage network in the same subnet
as the CVMs and AHV.
Client networking Use a separate subnet to provide client access
to storage.
Fault tolerance and replication factor settings Configure the cluster for fault tolerance 1 and
configure the container for replication factor 2.
```

```
Design Option Validated Selection
Storage containers Use a separate storage container to host
Nutanix Unified Storage files.
Erasure coding Don't enable erasure coding.
Compression Enable compression.
Deduplication Don't enable deduplication.
Protocols for shares Use Network File System (NFS) for shares.
Nutanix Container Storage Interface Use dynamic provisioning.
```
_Table: Objects Storage Design Decisions_

```
Design Option Validated Selection
Objects Storage cluster size Use three worker nodes and two load balancer
nodes.
Worker node size Use 10 vCPUs and 32 GB of memory for each
worker node.
Load balancer node size Use 2 vCPUs and 4 GB of memory for each
load balancer node.
Storage networking Keep the storage network in the same subnet
as the Controller VMs and AHV.
Client networking Use a separate subnet to provide client access
to storage.
Fault tolerance and replication factor settings Configure the cluster for fault tolerance 1, and
configure the container for replication factor 2.
Storage containers Use a separate storage container to host
Nutanix Unified Storage objects.
Erasure coding Don't enable erasure coding.
Compression Enable compression.
Deduplication Don't enable deduplication.
Buckets to create Create two buckets: user_upload (for end-user
access) and backup (backup target).
```

#### Nutanix Database Service Design for Nutanix GPT-in-a-Box..........................................................

```
Nutanix Database Service (NDB) provides automated provisioning, scaling, patching,
and management of databases on Nutanix infrastructure. This validated design includes
NDB to provision and manage PostgreSQL databases used by GenAI workloads.
NDB provisions two independent workflows:
Nutanix Enterprise AI back-end database
A transactional PostgreSQL database backing the Nutanix Enterprise AI services
Vector store with pgvector
A PostgreSQL instance with pgvector extension enabled, acting as a vector store
for user applications using semantic search or retrieval-augmented generation
(RAG)
The NDB Operator for Kubernetes integrated with Nutanix Kubernetes Platform (NKP)
workload clusters provisions and manages these databases. Database life cycle
management (provisioning, patching, backup) is declarative, consistent with GitOps
practices used for other NKP-managed workloads.
Metrics for the provisioned PostgreSQL instances are available through Prometheus as
part of the logging stack observability in NKP. Metrics include CPU and memory usage,
connection count, query duration, and IOPS.
Table: Nutanix Database Service Design Decisions
Design Option Validated Selection
NDB version 2.8.0
Number of cores for NDB instance 16
NDB instance size 32 GB
Database deployment method NDB Operator for Kubernetes
Database engine PostgreSQL
Vector store extension pgvector
Number of PostgreSQL instances 2 (Nutanix Enterprise AI back end, pgvector
store)
```

```
Design Option Validated Selection
Resource separation Each database provisioned into dedicated
database VMs
Backup Managed through NDB backup policy
Monitoring Integrated with NKP observability stack
```
#### Management Components for Nutanix GPT-in-a-Box......................................................................

```
Management components such as Prism Central, Active Directory, DNS, and NTP are
critical services that must be highly available. Prism Central is the global control plane for
Nutanix, responsible for VM management, application orchestration, microsegmentation,
and other monitoring and analytics functions. You can deploy Prism Central in a single-
VM or scale-out (three-VM) configuration. This NVD doesn't use all of these management
features.
In this NVD, the management components run on the same physical cluster as the
applications.
The infrastructure management plane for this validated design is Prism Central, which
manages and controls the following components:
```
- Files Storage and Objects Storage
- VMs
- RBAC
- Monitoring, observability, and auditing for the core infrastructure
Nutanix Kubernetes Platform provides its own independent management plane for
Kubernetes workloads that includes the following:
- Workload cluster deployment and management using workspace profiles
- Application catalog
- Application deployment using workspace profiles and projects
You can perform the following actions from the administrator interface in the Nutanix
Enterprise AI workload cluster:


- Manage large language models (LLMs)
- Manage inference endpoints
- Manage role-based access control (RBAC)

Name resolution and SSL certificate management are critical to deploying and managing
Kubernetes clusters. In this NVD, Microsoft DNS manages the DNS root domain.
Subdomains map the DNS and route the traffic to the Kubernetes clusters. The core
infrastructure components use DNS forwards to redirect the traffic for a particular
subdomain to the local domain controller.

You can substitute your own DNS solution but you might need to make changes to
certificate management.

```
Figure 4: DNS Management
```
In this NVD, the cluster runs AOS with AHV hosting Prism Central. Prism Central is
deployed in a scaled-out architecture (three-node cluster) to provide the desired uptime
and performance for the management plane.

The following table lists the design decisions for the Nutanix management components.

_Table: Management Component Design Decisions_

```
Design Option Validated Selection
Cluster placement Deploy the management components on the
same cluster as the applications.
```

```
Design Option Validated Selection
Prism Central deployment Deploy scaled-out Prism Central (three VMs).
Prism Central deployment size Use a small deployment size of 3 VMs, each
with 6 vCPUs, 26 GB of memory, and 800 GiB
of storage.
Prism Central container name Use the default container name.
Active Directory authentication Use Active Directory authentication.
Connection to Active Directory Use SSL or TLS for Active Directory.
Certificates Replace self-signed certificates with certificate
authority–signed certificates.
```
#### Monitoring Design for Nutanix GPT-in-a-Box...................................................................................

```
Monitoring in the NVD falls into two categories: event monitoring and performance
monitoring. Each category addresses different needs and issues.
In a highly available environment, you must monitor events to maintain high service
levels. When faults occur, the system must raise alerts on time so that administrators
can take remediation actions as soon as possible. This NVD configures the Nutanix
platform's built-in ability to generate alerts in case of failure.
In addition to keeping the platform healthy, maintaining a healthy level of resource
usage is also essential to the delivery of a high-performing environment. Performance
monitoring continuously captures and stores metrics that are essential when you need to
troubleshoot application performance. A comprehensive monitoring approach tracks the
following areas:
```
- Application and database metrics
- Operating system metrics
- Hyperconverged platform metrics
- Network environment metrics
- Physical environment metrics
By tracking a variety of metrics in these areas, the Nutanix platform can also provide
capacity monitoring across the stack. Most enterprise environments inevitably grow, so


you need to understand resource usage and the rate of expansion to anticipate changing
capacity demands and avoid any business impact caused by a lack of resources.

In this NVD, Prism Central performs event monitoring for the Nutanix core infrastructure.
This NVD uses syslog for log collection; for more information, see the Security and
Compliance for Nutanix GPT-in-a-Box section. SMTP-based email alerts serve as the
channel for notifications in this design. To cover situations where Prism Central might be
unavailable, each Nutanix cluster in this NVD sends out notifications using SMTP as well.
The individual Nutanix clusters send alerts to a different receiving mailbox that's only
monitored when Prism Central isn't available.

Prism Element transmits the data to Prism Central using an API. Prism Central then
transmits the log to Nutanix Pulse (using the Secure Sockets Layer) and the syslog
server using the User Datagram Protocol (UDP). All components (Prism Central,
Nutanix Unified Storage, and Nutanix Kubernetes Platform (NKP)) send the alerts to the
monitoring system using SMTP.

For more information on monitoring in NKP, see the Kubernetes Cluster Design for
Nutanix GPT-in-a-Box section in this document.


```
Figure 5: GPT-in-a-Box Monitoring Conceptual Design
```
Prism Central monitors cluster performance in key areas such as CPU, memory,
network, and storage usage and captures these metrics by default. When a Prism
Central instance manages a cluster, Prism Central transmits all Nutanix Pulse data, so
it doesn't originate from individual clusters. When you enable Nutanix Pulse, it detects
known issues affecting cluster stability and automatically opens support cases.


```
Figure 6: Hybrid Cloud Performance Metrics Systems
```
The network switches that connect the cluster also play an important role in cluster
performance. A separate monitoring tool that's compatible with the deployed switches
can capture switch performance metrics. For example, a Simple Network Management
Protocol (SNMP) tool can regularly poll counters from the switches.

The following table provides descriptions of the monitoring design decisions.

_Table: Monitoring Design Decisions_


```
Design Option Validated Selection
Platform performance monitoring Prism Central monitors Nutanix platform
performance.
Network switch performance monitoring A separate tool that performs SNMP polling
to the switches monitors network switch
performance.
SMTP alerting Use SMTP alerting; use an enterprise SMTP
service as the primary SMTP gateway for
Prism Element and Prism Central.
SMTP alerting source email address Configure the source email address to
be clustername@<yourdomain>.com
to uniquely identify the source of email
messages. For Prism Central, use the Prism
Central host name in place of cluster name.
SMTP alerting Prism Central recipient email
address
```
```
Configure the Prism Central
recipient email address to be
primaryalerts@<yourdomain>.com.
SMTP alerting Prism Element recipient email
address
```
```
Configure the Prism Element
recipient email address to be
secondaryalerts@<yourdomain>.com.
NCC reports Configure daily NCC reports to run at 6:00
AM local time and send them by email to the
primary alerting mailbox.
Nutanix Pulse Configure Nutanix Pulse to monitor the Nutanix
cluster and send telemetry data to Nutanix.
```
```
Application-specific monitoring is described in the Kubernetes Cluster Monitoring for
Nutanix GPT-in-a-Box section.
```
#### Security and Compliance for Nutanix GPT-in-a-Box........................................................................

```
Nutanix recommends a defense-in-depth strategy for layering security throughout any
enterprise datacenter solution. This design section focuses on validating the layers that
Nutanix can directly oversee at the control and data-plane levels. For more information,
see the Security and Compliance section of the Nutanix for Enterprise Edge Computing
reference architecture.
```

Isolate Nutanix cluster management and out-of-band interfaces from the rest of the
network using firewalls, and only allow direct access to them from the management
security domain. In addition, Nutanix recommends separating out-of-band management
and cluster management interfaces onto a dedicated VLAN away from the application
traffic.

All Nutanix control plane endpoints use Active Directory–hosted Lightweight Directory
Access Protocol over SSL (LDAPS). Active Directory itself is highly available and
redundant in the existing customer landscape. Only administrative accounts are mapped
to admin roles, which are controlled through a named Active Directory group.

This NVD rotates all default passwords for all accounts that aren't integrated with Active
Directory, such as emergency accounts or local accounts for out-of-band interfaces.
Because clusters don't have lockdown mode enabled, password SSH is enabled by
default.

This NVD enables additional non-default hardening options in each AOS cluster:

- Advanced Intrusion Detection Environment (AIDE)
- Hourly security configuration management automation (SCMA)

Both features are trivial to enable, introduce little to no discernible system overhead,
and help detect and prevent internal system configuration changes that could otherwise
compromise service availability. These features add to the intrinsic hardening built into
AOS.

For each control plane endpoint (Prism Central), system-level internal logging goes to a
centralized third-party syslog server that runs in the existing customer landscape. The
system is configured to send logs for all available modules when they reach the syslog
Error severity level.

This NVD assumes that the centralized syslog servers are highly available and
redundant, so you can inspect the log files in case the primary log system is unavailable.

SSL endpoints serve all Nutanix control plane web pages. This NVD replaces the default
self-signed certificates with certificates signed by an internal certificate authority in your
environment. Any client endpoints that interact with the control plane must have the
trusted certificate authority chain preloaded to prevent browser security errors.


```
Note: Certificate management is an ongoing activity, and you must rotate certificates periodically. This NVD
signs all certificates for one year of validity.
```
```
Table: Security Design Decisions
Design Option Validated Selection
Data-at-rest encryption (DaRE) Don't use DaRE.
SSL endpoints Sign control plane SSL endpoints with an
internal trusted certificate authority.
Certificates Provision certificates with a yearly expiration
date and rotate accordingly.
Authentication Use Active Directory LDAPS authentication.
Control plane endpoint administration Use a common administrative Active Directory
group for all control plane endpoints.
Cluster lockdown mode Don't enable cluster lockdown mode (allow
password-driven SSH).
Nondefault hardening options Enable AIDE and hourly SCMA.
System-level internal logging Enable error-level logging to an external syslog
server for all available modules.
Syslog delivery Use User Datagram Protocol (UDP) transport
for syslog delivery.
```
```
Table: Security Configuration References
Configuration Target Key:Value
Active Directory AD-admin-group:ntnx-ctrl-admins
Syslog Server infra-az[1..2]-syslog:6514 (udp)
```
#### Infrastructure for Nutanix GPT-in-a-Box...........................................................................................

```
This design assumes that the datacenter in the hosting region sustains a single
availability zone (AZ). To ensure high availability at the physical cluster level, this NVD
addresses the connection between the hardware running Nutanix software and the
datacenter's power and networking components.
```

This NVD recommends dedicating one rack to the GPT-in-a-Box workload. Rack 1 has
the following requirements:

- Two top-of-rack switches
- One management switch
- GPT-in-a-Box cluster (four 2 RU hyperconverged infrastructure nodes based on the
    bill of materials; for Nutanix NX, add two 2 RU GPU compute-only nodes)


```
Figure 7: Rack Layout
```
In this design's physical rack space, one generic 42 RU rack contains 8 RU of systems
with 3 RU reserved for two data switches and one out-of-band switch.

The four nodes consume four network ports on each of the two data switches. In
addition, each node has a dedicated out-of-band management network interface,
consuming four ports on the management switch.


When scaling the environment, consider physical rack space (including floor weight
capacity), network port availability, and the datacenter's power and cooling capacity.


## 4. Kubernetes Cluster Design for Nutanix GPT-in-a-Box.......................

```
This conceptual design outlines a robust, highly available Kubernetes environment on the
Nutanix platform, using Nutanix Kubernetes Platform (NKP) and integrating with essential
Kubernetes tools (many of which come with NKP) and practices for efficient and secure
operations.
The GPT-in-a-Box cluster requires the following features:
```
- Application catalogs and GitOps with built-in Flux CD to push workloads to multiple
    clusters
- Workspaces and projects to provide isolation (in terms of access)
- Cluster profile definition (in terms of applications deployed to the cluster)
- Segregation (in terms of clusterwide and namespace-specific workloads)
For more information on Nutanix licensing to meet these requirements, see the Nutanix
Cloud Platform Software Options page.
In this design, we use a single NKP Ultimate management cluster to manage workloads
and configurations and push them to multiple workload clusters. The NKP management
cluster is used for management only. Nutanix Enterprise AI and its supporting
components run on one NKP workload cluster with GPU node pools. GenAI apps and
supporting components run on another workload cluster. The data resides locally in these
clusters or in an S3 bucket (Objects Storage) or Network File System (NFS) share (Files
Storage).
All production Kubernetes clusters are set up as production-level clusters using multiple
control plane and worker nodes distributed across different physical hosts to ensure high
availability.
When you build an NKP cluster, the process automatically deploys the following
preconfigured cloud-native tools for NKP's internal use:
- Certificate management: Cert-Manager


- Ingress controller: Traefik
- Authentication: Dex
- Continuous delivery: Flux CD
- Visibility: Kubernetes dashboard
- Policy admission webhook: Gatekeeper
- Cost management: Kubecost

```
Note: Although these tools are for internal use, workloads running on the cluster can also use them.
```
This NVD uses the following design decisions for NKP workspace and projects:

- Workspaces: Because the solution's two cluster categories (Nutanix Enterprise
    AI clusters and GenAI clusters) require different resources (Nutanix Enterprise AI
    requires GPU and GenAI doesn't), run different workloads, and are likely managed by
    different teams, create two workspaces (Nutanix Enterprise AI and GenAI).
- Projects: To allow multiple application teams to share clusters, create a project
    for each GenAI application in the GenAI workspace. This process also creates a
    namespace in the associated clusters.
       The continuous delivery mechanism makes it easy to deploy apps across all
       clusters associated with the project.

The following NKP workspace applications are often explicitly enabled from the NKP
application catalog in both workspaces:

- Logging: Logging Operator, Grafana Logging, Grafana Loki, Fluent Bit
- Monitoring: Prometheus Monitoring, Prometheus Adpater
- Backup and recovery: Velero with Objects Storage as the S3-compatible object store
    used for storage
- Object storage: Rook Ceph, Rook Ceph Cluster

The following NKP workspace applications are explicitly enabled only on the Nutanix
Enterperise AI workspace:

- Istio Service Mesh


- NVIDIA GPU Operator
- Knative
- Nutanix Enterprise AI

```
Note: To enable Nutanix Enterprise AI, add the https://github.com/nutanix-cloud-
native/nkp-nutanix-product-catalog repo to the Nutanix Enterprise AI workspace as a Flux
CD GitRepository resource in the management cluster.
```
The NDB Operator for Kubernetes is currently not part of the NKP catalog and must be
installed separately in both workspaces. Define manifests for each PostgreSQL instance
(Nutanix Enterprise AI back end, vector store); enable the pgvector extension through an
additional Kubernetes job after you deploy the underlying database.

Create the GenAI app project in the GenAI workspace and use its continuous delivery
mechanism to deploy the GenAI app. The clusters in the GenAI workspace are explicitly
attached to this project. You can add more projects to this workspace to allow more than
one app team to share clusters in the GenAI workspace.


```
Figure 8: Nutanix GPT-in-a-Box Kubernetes Conceptual Design
```
_Table: GPT-in-a-Box Cluster with NKP Scalability Design Decisions_

```
Design Option Validated Selection
NKP cluster type Use the NKP Ultimate license.
Control plane size Size the control plane with 4 CPUs, 16 GB of
memory, and 80 GB of storage.
Initial workload size Start with four worker nodes with 8 CPUs, 32
GB of memory, and 100 GB of storage.
GPU pool size (Nutanix Enterprise AI cluster
only)
```
```
Use two worker nodes with 12 CPUs, 40 GB of
memory, and 100 GB of storage.
```

The workloads are divided into NKP management, GPT-in-a-Box Nutanix Enterprise AI,
and production or development environments. The single NKP management cluster is
named <custom-prefix>-nkp-mgmt, and Nutanix Enterprise AI runs on a dedicated cluster
in the Nutanix Enterprise AI workspace named <custom-prefix>-nai. The workload
clusters use the following naming convention:

```
<custom-prefix>-<cluster_type>-<environment_id>-wl-<app_id>-
<optional_app_index_number>
```
NKP cluster names use a maximum of 63 characters and have the same restriction as
any Kubernetes resource (the cluster name must be a valid DNS-1035 label), which
means that cluster names can only contain lowercase alphanumeric characters and
hyphens and must start and end with an alphanumeric character.

You can scale the NKP management and workload cluster worker nodes to
accommodate different workload sizes. Nutanix recommends scaling out. The total
number of workers in the GPU node pools is constrained by the number of physically
installed GPUs.

All GPUs are licensed to run Nutanix Enterprise AI, but this design keeps some spare
GPU dev and experimental workloads:

- NKP management cluster: acme-nkp-mgmt
- GPT-in-a-Box Nutanix Enterprise AI environment: acme-nai (two GPU nodes with two
    L40S each)
- Prod environment: acme-gpt-p-wl-genai-01
- Dev environment:

```
› acme-gpt-d-wl-nai (two GPU nodes with one L40S each)
› acme-gpt-d-wl-genai-01
› acme-gpt-d-wl-genai-02
› acme-gpt-d-wl-genai-03
› acme-gpt-d-wl-genai-04
```
You can use other layouts based on your development requirements.


#### Kubernetes Cluster Resilience and Networking for Nutanix GPT-in-a-Box......................................

```
Nutanix Kubernetes Platform (NKP) clusters are production-level clusters that provide a
resilient control plane by running multiple nodes for the control plane and etcd. To ensure
high availability, Kubernetes deployments use multiple replica pods and implement pod
antiaffinity rules. This approach helps maintain service availability, even in the event of a
worker node update or failure.
Kubernetes services and ingress controllers perform the essential service of load
balancing by evenly distributing network traffic across all available pods, enhancing
service reliability and system performance.
NKP uses the following for network and communication:
```
- Cilium as the Container Network Interface
- Kube-VIP for network connectivity to load balance the Kubernetes API server
- MetalLB for network connectivity to load balance the Kubernetes LoadBalancer
    Services
- Traefik as the default ingress controller for NKP's dashboards and other internal tools
- Istio Mesh as the ingress for the Nutanix Enterprise AI
- Cert-Manager to automate the management and issuance of TLS certificates
Each deployed cluster uses a base configuration for networking.

#### Kubernetes Cluster Monitoring and Backup for Nutanix GPT-in-a-Box............................................

```
Nutanix Kubernetes Platform (NKP) comes with a fully integrated observability stack out
of the box. The logging stack is turned off by default; you must enable it explicitly.
Monitoring stack:
```
- Prometheus to collect metrics and generate alerts
    It uses Prometheus Operator, which allows you to modify the configuration
    using custom resource definitions such as PrometheusRules and
    ServiceMonitors.


- Prometheus Adapter to serve Kubernetes metric API
- AlertManager for alerting
- Grafana to visualize metrics with a rich set of precreated custom dashboards

For centralized monitoring, the NKP management cluster also runs Thanos for metric
aggregation from all attached clusters. A centralized Grafana instance hosts dashboards
to convert these metrics to meaningful information. Similarly, Karma provides a
multicluster dashboard for alerts.

Logging stack:

- Fluentbit to collect host, host_kernel, and audit logs
- Logging-Operator to collect and forward container logs
- Loki to index and store logs

```
By default, logs are stored in a locally deployed rook-ceph object store.
```
- Grafana Logging to visualize logs and the audit dashboard

All clusters run their local logging and monitoring stack. A Thanos instance running
on the management cluster aggregates metrics from all Prometheus instances across
NKP clusters. The management cluster runs an instance of Karma that pulls alerts from
workload clusters and shows them in its dashboard.


```
Figure 9: Kubernetes Monitoring Conceptual Design
```
The default storage class installed by NKP during deployment provides persistent
storage for the management and workload clusters. The persistent volumes and
application data in the all clusters can be protected by a Velero backup schedule and
stored in a S3 bucket provided by Objects Storage (or optionally a local object store
provided by a rook-ceph that runs locally on the NKP cluster). A bucket in the local
Objects Storage store can be configured as the backup storage location and replicated to
an external location.

```
Note: Because NKP application catalogs or GitOps Flux CD manages application deployment, only back up
stateful workloads.
```
Nutanix Database Service (NDB) time machine snapshots protect the content of the
vector store provided by NDB.


## 5. Large Language Model Application Design

## for Nutanix GPT-in-a-Box

```
Fine-tuning a large language model (LLM) on specific documents and using retrieval-
augmented generation (RAG) pipelines are two distinct approaches to enhancing
language models. Fine-tuning involves directly adjusting the model's parameters based
on a specific set of documents. This process customizes the model to better reflect
the style, terminology, and content of the input material, making it more effective at
generating or interpreting text consistent with the training data.
In contrast, a RAG pipeline doesn't alter the underlying model; instead, it augments the
LLM’s capabilities by integrating a retrieval component. This component dynamically
fetches relevant information from external sources (such as a database or a set of
documents) in real time during the generation process. The LLM uses this retrieved
information to inform its responses, making it more accurate and contextually rich without
changing the model's fundamental structure.
Fine-tuning can sometimes lead to overfitting, where the model becomes overly
specialized to the training data and might not perform well on general or varied inputs.
In contrast, RAG preserves the generalization capabilities of the original LLM while
enhancing its outputs with information retrieved dynamically from external sources. This
approach is particularly important for applications where the reliability and predictiveness
of the base model are crucial.
Fine-tuning an LLM can be resource-intensive, requiring substantial computational
resources and data for retraining. In contrast, RAG uses the existing model by integrating
external data dynamically during the inference stage, which is generally less demanding
in terms of computational resources.
For these reasons, this NVD uses a RAG pipeline instead of fine-tuning. A RAG pipeline
is particularly useful for applications that require up-to-date information or handle vast,
ever-changing data sets.
```

#### Large Language Model Conceptual Design for Nutanix GPT-in-a-Box............................................

```
This design introduces a robust architecture to support GenAI applications using a wide
range of open-source large language model (LLM) models. The Nutanix Enterprise
AI platform can host these models as OpenAI-compatible LLM inferencing endpoints,
providing flexibility and compatibility for diverse use cases.
While the Nutanix Enterprise AI platform operates on any Kubernetes infrastructure,
this specific design uses NKP as the container orchestration platform to streamline
operations and enhance efficiency. The design offers a comprehensive, scalable, and
efficient framework tailored specifically for building retrieval-augmented generation
(RAG) pipeline applications.
By incorporating the latest LLM technologies and supporting tools, this framework
ensures ease of development, seamless deployment, and effective monitoring. It
provides a reliable and adaptable solution for modern AI workflows. Using Intel AMX–
enabled CPUs and optimized OpenVINO libraries speeds up document embedding by a
factor of 2.
```
#### Large Language Model Logical Design for Nutanix GPT-in-a-Box..................................................

```
Large language model (LLM) inference endpoints, provided through the Nutanix
Enterprise AI platform, form the foundation of this architecture. These essential
components play a critical role in deploying and managing machine learning models,
particularly for addressing real-time inference requirements. The integration with the
Nutanix GPT-in-a-Box 2.0 design guarantees scalability, reliable accessibility, and
consistent performance.
With this modular architecture, you can independently scale and update each
component, providing flexibility for system optimization. Its compatibility with the retrieval-
augmented generation (RAG) framework allows the LLM to query and retrieve data from
the vector database, significantly improving the accuracy and relevance of generated
outputs.
The data ingestion process supports both batch processing and event-driven workflows
through Kafka. With this dual approach, the system can seamlessly handle large-scale
periodic batch uploads and continuous real-time data streams. Serverless functions built
on Knative and triggered by Objects Storage event notifications relayed through Kafka
```

```
process events. The embedding function automatically discovers Intel AMX features
to optimize data processing. This architecture ensures efficient, scalable, and highly
responsive handling of incoming data streams.
After the embedding function vectorizes the data using the LangChain toolset, it stores
the vectors in a pgVector-enabled PostgreSQL database, managed by Nutanix Database
Service (NDB), to provide the robust back-end storage necessary to meet its large-scale
data demands.
During the inference process, integration with the RAG pipeline architecture allows
the LLMs running on Nutanix Enterprise AI to dynamically query and retrieve relevant
information from the vector database, significantly improving the accuracy and relevance
of generated outputs. This approach enhances the system's ability to provide accurate,
domain-specific answers by reducing reliance on the LLM's internal knowledge, which
might be insufficient. Instead, the RAG workflow ensures that responses are informed by
up-to-date and highly relevant data, improving both accuracy and contextual relevance
for user queries.
In this architecture, Nutanix Enterprise AI uses the Nutanix Kubernetes Platform (NKP)
observability stack to enable seamless monitoring and logging for LLM workloads and
GPU performance. After you deploy the NVIDIA GPU Operator from the NKP catalog,
it automatically configures NVIDIA Data Center GPU Manager metrics for Prometheus
and default Grafana dashboards that detail GPU usage, memory bandwidth, and thermal
performance. Thanos aggregates metrics and Grafana visualizes them. Fluentbit and
Loki handle log collection and indexing. The result is an advanced monitoring solution
that goes beyond conventional GPU observability.
```
#### Research and Document Ingestion Workflows for Nutanix GPT-in-a-Box.......................................

```
At a high level, the large language model (LLM) uses the following research workflow:
```
**1.** Ask a question: The interaction begins when the user poses a question through the UI
    or chatbot interface.
**2.** Create a query embedding: The embedding model transforms the user's query into a
    vector representation.
       This process is known as vectorization.


**3.** Search and retrieve similar context: The vector database, which is specifically
    designed for similarity searches, stores the document embeddings generated by the
    model. It can efficiently search for and retrieve items based on these embeddings,
    which encapsulate the semantic meaning of the texts.
**4.** Send the prompt: The workflow augments the user's query with relevant contextual
    information retrieved from the database, then sends this enriched query as a prompt
    to the LLM endpoint running on Nutanix Enterprise AI. The LLM processes the
    enriched query and generates a response.
**5.** Get an answer: The UI or chatbot interface presents the LLM's response as the
    answer to the user's query.

```
Figure 10: LLM Research Workflow
```
The design uses the following high-level workflow for document ingestion:

**1.** Uploading the document: Each time a new document is added to the bucket, an event
    notification is sent through Kafka.
**2.** Processing the Kafka event: The notification triggers the document ingestion service
    to generate a new embedding for that specific document.
**3.** Ingesting the batch: The document ingest function downloads the document.


**4.** Embedding the document: Embedding involves splitting the document into smaller
    segments and using an embedding model to create a vector representation of each
    segment.
**5.** Storing the embeddings: The generated vectors are stored in a vector database for
    future retrieval and searches.

```
Figure 11: Document Ingestion Workflow
```

## 6. Backup and Disaster Recovery for Nutanix GPT-in-a-Box................

```
Because the scope for this NVD is a single standalone GPT-in-a-Box cluster, you must
back up the application data on the S3 buckets in the Objects Storage store to an
external environment. You don't need to back up Kubernetes application resources
deployed by the Nutanix Kubernetes Platform (NKP) application catalog or GitOps
because Flux CD handles the deployment and stores the configuration data in an
external Git repository. However, if these applications use any stateful data, you must
back that up. NKP provides Velero as a tool to back up Kubernetes resources and
stateful data.
You can use the streaming replication mechanism built into Objects Storage to replicate
the data at the bucket level to a different S3 object store outside the GPT-in-a-Box
cluster. You can also use the existing backup solution to back up the persistent
application data and store it outside the GPT-in-a-Box cluster.
For more information on protecting workloads and meeting or exceeding service-level
agreements (SLAs), see the Nutanix On-Premises Hybrid Cloud with AHV Design and
Nutanix Enterprise Edge with Artificial Intelligence Design.
```

## 7. Ordering Nutanix GPT-in-a-Box Deployments....................................

```
This bill of materials reflects the validated and tested hardware and services that Nutanix
recommends to achieve the outcomes described in this document. Consider the following
points when you build your orders:
```
- All software is based on core licensing whenever possible.
- Nutanix Professional Services or an affiliated partner selected by Nutanix provides all
    services.

```
Note: Because available hardware, software, and services can change without notice, contact Nutanix
Sales when ordering to ensure that you have the correct product codes.
```
```
Nutanix recommends purchasing the exact hardware configuration reflected in the bill of
materials whenever possible. If a specific hardware configuration is unavailable, choose
a similar option that meets or exceeds the recommended specification. To allow the
greatest possible flexibility, you can choose between Intel and AMD.
```
- You can make hardware substitutions to suit your preferences; however, such
    changes might result in a solution that doesn't follow the recommended Nutanix
    configuration.
- Avoid software product code substitutions except when:
    › You need different quantities to maintain software licensing compliance.
    › You prefer a higher license tier or support level for the same software product code.
- Adding any software or workloads not specified in this design (including additional
    Nutanix products) might affect the validated density calculations and result in a
    solution that doesn't follow the recommended Nutanix configuration.
- Nutanix Professional Services substitutions to accommodate customer preferences
    aren't possible.


#### Sizing Considerations........................................................................................................................

```
This NVD describes the design for a single GPT-in-a-Box cluster with four or more
nodes. If one of the scaling factors (such as the total number of VMs, Kubernetes
applications, or GPU workloads) exceeds the maximum specified for the solution, extend
the cluster with the required capacity (CPU, memory, storage, GPU). After reaching the
maximum cluster size, consider building a new cluster to support the demand for further
growth.
```
#### Nutanix GPT-in-a-Box Cluster Bill of Materials................................................................................

```
The following sections show the bill of materials for the Nutanix GPT-in-a-Box cluster.
Table: GPT-in-a-Box Cluster: Hardware
Item Specification
Platform (HCI, Intel-based) Lenovo HX650V3, HPE DX380a, or Nutanix
NX-8155-G9
Platform (Compute-only, Intel-based) Nutanix NX-9151-G9
Platform (HCI, AMD-based) Lenovo HX665V3 or HPE DX385
Configuration 1-node
Type All-flash NVMe
Support level Production
NRDK support No
NR node support No
```
```
Note: We used the Lenovo HX665V3, HPE DX380a, and NX-8155-G9 and NX-9151-G9 models for
validation.
```
```
Table: GPT-in-a-Box Cluster: Per-Node Hardware Configuration HPE or Lenovo
Component Description Quantity
Processor Intel Xeon-Gold 6442Y (2.6
GHz, 24 cores) or AMD EPYC
9254 (2.9 GHz, 24 cores)
```
```
2
```

```
Component Description Quantity
Memory 64 GB, 4,800 MHz DDR5
RDIMM
```
```
24
```
```
SSD 3.2 TB NVMe 6
Network adapter 100 GbE, 2-port Mellanox
ConnectX-6 (QSFP56)
```
```
1
```
```
Network adapter 10 or 25 GbE, 2-port Mellanox
ConnectX-6 (SFP28)
```
```
1
```
```
GPU (Lenovo) NVIDIA L40S 48 GB PCIe 3
GPU (HPE) NVIDIA L40S 48 GB PCIe 2
```
```
Note: We validated HPE with two GPUs, but you can order up to four GPUs. We validated Lenovo with
three GPUs; you must use a minimum of two GPUs.
```
_Table: GPT-in-a-Box Cluster: Per-Node Hardware Configuration Nutanix NX-8155-G9_

```
Component Description Quantity
Processor Intel Xeon-Gold 6442Y (2.6
GHz, 24 cores)
```
```
2
```
```
Memory 32 GB, 5,600 MHz DDR5
RDIMM
```
```
32
```
```
SSD 3.84 TB NVMe 6
Network adapter Mellanox 100, 40, or 25 GbE,
2-port NIC (CX6 100 GbE)
```
```
2
```
```
Network adapter SMC 10 GbE, 2-port Base-T,
2-port SFP+ NIC (X710)
```
```
1
```
_Table: GPT-in-a-Box Cluster: Per-Node Hardware Configuration Nutanix NX-9151-G9_

```
Component Description Quantity
Processor Intel Xeon-Platinum 8462Y+
(2.8 GHz, 32 cores)
```
```
2
```
```
Memory 64 GB, 4,800 MHz DDR5
RDIMM
```
```
16
```

```
Component Description Quantity
Network adapter Mellanox 100, 40, or 25 GbE,
2-port NIC (CX6 100 GbE)
```
```
3
```
```
Network adapter SMC 10 GbE, 2-port Base-T,
2-port SFP+ NIC (X710)
```
```
1
```
```
GPU NVIDIA L40S 48 GB PCIe 4
```
```
Use the following software for the GPT-in-a-Box cluster.
```
- GPT-in-a-Box Full Stack Ultimate:
    › HPE or Lenovo: 192
    › Nutanix NX: 320
- Nutanix Enterprise AI GPU memory: 8 × 48 GB (384 GB)
For information on licensing for Nutanix products, see the Nutanix Cloud Platform
Software Options page.

### Nutanix Professional Services..........................................................................................................

```
To ensure a seamless experience across design, migration planning, implementation,
and optimization of the Nutanix platform for both virtualized and containerized
applications, we strongly recommend engaging Nutanix Professional Services.
Reach out to your Nutanix Account Team to discuss the recommended services tailored
to your environment. Leveraging these expert services helps reduce complexity and risk
at every stage of your journey.
```

## 8. References and Resources for Nutanix GPT-in-a-Box.......................

```
For more information on the components that make up Nutanix GPT-in-a-Box
deployments, see the following references:
```
**1.** Nutanix On-Premises Hybrid Cloud with AHV Design
**2.** Nutanix Enterprise Edge with Artificial Intelligence Design
**3.** Nutanix for Enterprise Edge Computing Reference Architecture
**4.** Files Storage User Guide
**5.** Objects Storage User Guide
**6.** Physical Networking Best Practices


## About Nutanix.............................................................................................

```
Nutanix offers a single platform to run all your apps and data across multiple clouds
while simplifying operations and reducing complexity. Trusted by companies worldwide,
Nutanix powers hybrid multicloud environments efficiently and cost effectively. This
enables companies to focus on successful business outcomes and new innovations.
Learn more at Nutanix.com.
```

## List of Figures.............................................................................................................................................

```
Figure 1: Architectural Layers of the Nutanix Validated Design for GPT-in-a-Box 2.0...................................6
Figure 2: GPT-in-a-Box Nutanix Cluster Conceptual Architecture...............................................................18
Figure 3: GPT-in-a-Box Physical Network Architecture...............................................................................23
Figure 4: DNS Management........................................................................................................................ 29
Figure 5: GPT-in-a-Box Monitoring Conceptual Design...............................................................................32
Figure 6: Hybrid Cloud Performance Metrics Systems................................................................................33
Figure 7: Rack Layout..................................................................................................................................38
Figure 8: Nutanix GPT-in-a-Box Kubernetes Conceptual Design................................................................43
Figure 9: Kubernetes Monitoring Conceptual Design..................................................................................47
Figure 10: LLM Research Workflow.............................................................................................................51
Figure 11: Document Ingestion Workflow.................................................................................................... 52
```


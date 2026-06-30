# **Nutanix Platform Design Blueprint for AI Solutions** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Nutanix Platform Design Blueprint for AI Solutions 

## **Contents** 

**1. Executive Summary.................................................................................4** 1.1. Nutanix AI Platform Architecture Design Assumptions................................................................7 1.2. Nutanix AI Platform Architecture Components............................................................................ 8 1.3. Nutanix AI Platform Architecture Software Versions................................................................. 10 **2. Nutanix AI Platform Architecture Overview........................................ 11** 2.1. Nutanix AI Cluster Architecture..................................................................................................12 2.2. Nutanix Unified Storage for AI Architecture...............................................................................21 2.3. Nutanix AI Network Architecture................................................................................................24 2.4. AI Platform Layer Overview.......................................................................................................31 2.5. Generative AI Applications Layer Overview.............................................................................. 41 2.6. Nutanix Agentic AI Security Architecture...................................................................................44 **3. Nutanix AI Design Blueprint................................................................. 45** 3.1. Converged Design Blueprint...................................................................................................... 45 3.2. Foundation Design Blueprint......................................................................................................47 **4. Nutanix AI Platform Architecture References.....................................50** 4.1. Nutanix and NVIDIA-Certified Reference Configuration Partner Platforms............................... 51 **List of Figures.............................................................................................................................................52 List of Tables...............................................................................................................................................53** 

Nutanix Platform Design Blueprint for AI Solutions 

## 1. Executive Summary 

The Nutanix AI platform architecture integrates with NVIDIA building blocks to deliver AI capabilities as centralized inference services. 

Generative AI workloads are evolving beyond single-turn interactions with large language models (LLMs). Modern applications increasingly use retrieval-augmented generation (RAG) techniques and agentic AI workflows to deliver contextually relevant, accurate, and dynamic responses. These applications combine reasoning, retrieval, embedding services, guardrails, and policy enforcement into coordinated, multistep workflows. As a result, enterprises require more than raw GPU capacity. They need a controlled, operationally governed inference platform that can support these workloads at scale. 

Figure 1: Nutanix Agentic AI Solution 

With the Nutanix AI platform architecture, you can deploy agentic AI workflows that coordinate reasoning, tool invocation, and dynamic data access. Foundational platform capabilities: 

© 2026 Nutanix, Inc. All rights reserved  | **4** 

Nutanix Platform Design Blueprint for AI Solutions 

- Nutanix Enterprise AI (NAI) delivers centralized, API-driven inference services with OpenAI-compatible interfaces. NAI abstracts the underlying complexity of GPU infrastructure and model placement, allowing developers to focus on applications and agents rather than runtime management. 

- Enterprise-grade orchestration with Nutanix Kubernetes Platform (NKP) provides a Kubernetes-native framework to manage container life cycles and bare-metal GPU scheduling. By using standard, declarative Kubernetes constructs, NKP ensures consistent workload distribution across the entire ecosystem. 

- Nutanix Unified Storage (NUS) offers high-performance data services and decoupled, scalable storage optimized for AI. The Nutanix Files Storage software manages model artifacts and inference assets, and the Nutanix Objects Storage software provides the high-throughput foundation for RAG corpora, embedding services, and agent state. 

We provide a comprehensive blueprint for designing and operating an enterprise AI platform using the Nutanix agentic AI solution, informed by NVIDIA reference design concepts. We outline best practices, validated configurations, and architectural principles to help you build a secure, scalable, and high-performance AI infrastructure that can support modern generative AI workloads. 

Key topics: 

- Infrastructure design: Detailed specifications for compute, storage, and networking components that are optimized for AI workloads 

- Platform services: Configuration and design patterns for NAI, NKP, and NUS 

- Security and compliance: Strategies for building a solution that addresses data sovereignty, identity management, network security, and model governance 

- Operational best practices: Recommendations for monitoring, backup, disaster recovery, and life cycle management of AI infrastructure 

- Use case patterns: Reference applications for common AI workloads, including RAG pipelines and agentic workflows 

We intentionally exclude the following topics from this platform design: 

- Validated lab configurations and benchmark performance: Because this document serves as an architectural blueprint, it does not include exact lab-tested hardware build 

© 2026 Nutanix, Inc. All rights reserved  | **5** 

Nutanix Platform Design Blueprint for AI Solutions 

guides, firmware versions, or standardized inference server benchmarking. Nutanix Validated Designs contain this type of information. 

- Custom application development: This solution does not cover the development of specific AI applications or custom model training workflows. 

- Third-party software integration: Although this architecture supports extensibility, we don't provide detailed guidance on integrating third-party AI tools or platforms beyond those explicitly mentioned. 

- Future roadmap features: We don't include information on emerging features and capabilities that might be introduced in future releases of Nutanix or NVIDIA products. 

- Industry-specific compliance: We provide general security and compliance strategies, but we don't address specific industry regulations (such as HIPAA or GDPR). 

- Public cloud management: We focus on on-premises and enterprise-controlled datacenter deployments. We don't cover public cloud management or hybrid cloud orchestration. 

Support and integration responsibilities: 

Operational support resides with the provider of each respective technology: 

- Nutanix Support: 

   - › Nutanix Cloud Platform 

   - › NKP 

   - › NUS 

   - › NAI 

- NVIDIA Support: 

   - › NVIDIA AI software stack, such as NIM microservices and GPU operators 

   - › NVIDIA-specific networking hardware and software 

- NVIDIA-Certified Systems Partner Support: 

   - › Hardware troubleshooting 

   - › Firmware updates 

   - › Physical networking 

© 2026 Nutanix, Inc. All rights reserved  | **6** 

Nutanix Platform Design Blueprint for AI Solutions 

- Customer or Designated Systems Integrator Support: 

   - › End-to-end integration 

   - › AI application development 

   - › Specific model fine-tuning 

   - › Workload testing 

Table 1: Document Version History 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|May 2026|Original publication.|



## **1.1. Nutanix AI Platform Architecture Design Assumptions** 

The following AI platform design assumptions align with Nutanix guidelines. 

Hardware qualification and configuration assumptions: 

- NVIDIA-Certified Systems Reference Configurations: All GPU compute nodes used in the solution is qualified under the NVIDIA-Certified Systems program. 

- Compute node configuration: The solution uses eight-GPU compute nodes with the PCIe form factor running on bare metal to provide deterministic performance. 

- Exclusions: 

   - › Four-GPU configurations (such as 2-4-3-200) are out of scope for this solution because they primarily target smaller deployments. 

   - › For smaller deployments than this solution design, see the validated Nutanix GPTin-a-Box with Nutanix Kubernetes Platform Design. 

Scalability and deployment scope assumptions: 

- Minimum Scalable Unit (SU) definition: The fundamental building block is a 4-node SU. 

© 2026 Nutanix, Inc. All rights reserved  | **7** 

Nutanix Platform Design Blueprint for AI Solutions 

- Cluster management: 

   - › In the Nutanix AI platform architecture, one or multiple Nutanix Kubernetes Platform (NKP) clusters run AI workloads. 

   - › You can deploy additional NKP clusters for larger-scale environments, but that configuration is outside the scope of this solution architecture. 

## **1.2. Nutanix AI Platform Architecture Components** 

The Nutanix AI platform architecture integrates components and technologies from Nutanix and NVIDIA to deliver a unified platform for AI workloads. 

## **Nutanix Components** 

## **Nutanix Cloud Platform (NCP)** 

NCP provides the management and orchestration layer for AI solutions, consolidating compute, storage, and networking operations. 

## **Nutanix Unified Storage (NUS)** 

NUS delivers high-performance, scalable storage services through the Nutanix Files Storage and Objects Storage solutions optimized for AI workloads. 

## **Nutanix Kubernetes Platform (NKP)** 

NKP manages containerized AI workloads on bare-metal GPU nodes, providing Kubernetes orchestration and GPU scheduling. 

## **Nutanix Enterprise AI (NAI)** 

NAI offers centralized model serving, governance, and life cycle management for AI workloads. 

## **Prism Central** 

Prism Central provides a unified management interface for monitoring and managing the AI solution stack. 

## **Nutanix Container Storage Interface (CSI) driver** 

The CSI driver enables dynamic provisioning and management of Nutanix storage resources in Kubernetes environments. 

## **NVIDIA Components** 

## **NVIDIA Blackwell GPUs** 

Blackwell is the latest-generation GPU architecture optimized for AI workloads, providing enhanced performance for inference and training tasks. 

© 2026 Nutanix, Inc. All rights reserved  | **8** 

Nutanix Platform Design Blueprint for AI Solutions 

## **NVIDIA MGX Platform** 

The MGX platform for modular server design consists of entry-level PCIe-based GPU servers designed for smaller AI workloads, providing a cost-effective solution for initial deployments and pilot projects. 

## **NVIDIA GPU Operator** 

The GPU Operator manages the life cycle of NVIDIA GPU resources in Kubernetes clusters for optimal configuration and usage. 

## **NVIDIA Network Operator** 

The Network Operator automates the deployment and management of NVIDIA networking components, including single root I/O virtualization (SR-IOV) and remote direct memory access (RDMA) configurations. 

## **NVIDIA AI Data Platform (AIDP)** 

NVIDIA AIDP offers predefined architectural patterns and best practices for building AI solutions, including retrieval-augmented generation (RAG) pipelines and agentic applications; it includes vector databases, embedding services, and retrieval mechanisms. 

## **Key Technologies** 

## **SR-IOV** 

SR-IOV allows you to isolate PCIe resources for better performance and lower latency in virtualized environments. 

## **RDMA** 

RDMA allows direct memory access from one computer's memory to another's without involving the OS, enabling high-throughput and low-latency networking. 

## **NFS over RDMA (NFSoRDMA)** 

This protocol enables high-performance data transfers between storage and compute nodes by using RDMA capabilities, which reduces CPU overhead and improves throughput. 

## **Parallel NFS (pNFS)** 

GPU nodes use pNFS to communicate directly with NUS nodes using NFSoRDMA for high-speed, concurrent read/write operations during petabyte-scale batch inference or training. 

## **iSCSI Extensions for RDMA (iSER)** 

With iSER, iSCSI can use RDMA for improved storage networking performance. 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

Nutanix Platform Design Blueprint for AI Solutions 

## **1.3. Nutanix AI Platform Architecture Software Versions** 

We recommend the following minimum software versions for the Nutanix AI platform architecture. 

Nutanix minimum software versions: 

- Prism Central 7.5: Unified management and governance control plane 

- AHV 11.0: Required to support the underlying virtualization and single root I/O virtualization (SR-IOV) hardware passthrough capabilities managed by AOS Storage 

- AOS Storage 7.5: Although AOS Storage 6.10 is the general baseline, AOS Storage 7.5 is explicitly required for high-performance iSCSI Extensions for RDMA (iSER) or remote direct memory access (RDMA) support for the storage fabric through the UI 

- Files Storage 5.2.2.1: Required to activate the AI Performance Profile, which configures SR-IOV and RDMA paths for file server VMs (FSVMs) 

- Nutanix Kubernetes Platform (NKP) 2.17: Required for managing bare-metal GPU nodes and orchestrating the CSI drivers that use NFSoRDMA 

- Nutanix Container Storage Interface (CSI) driver 3.0: Required to support the dynamic provisioning of persistent volumes that use NFS over RDMA (NFSoRDMA) and Data Path (DP) Offload 

- Nutanix Enterprise AI (NAI) 2.6.0: Required for the latest inference gateway capabilities and integration with high-performance model loading paths 

NVIDIA minimum software versions: 

- NVIDIA Network Operator v25.1.0: Automates the configuration of SR-IOV, RDMA, and OpenFabrics Enterprise Distribution (OFED) drivers on worker nodes, allowing the compute layer to communicate with storage using the optimized path 

- NVIDIA GPU Operator v25.3.0: Automates GPU driver and container runtime configuration, integrating with GPUDirect Storage (GDS) paths 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

Nutanix Platform Design Blueprint for AI Solutions 

## 2. Nutanix AI Platform Architecture Overview 

The Nutanix AI platform architecture meets the requirements of modern AI workloads. 

The Nutanix AI platform architecture can help you transition AI workloads from an experimental or cloud-hosted proof of concept to a secure, cost-efficient, and scalable on-premises AI platform. This design addresses requirements for data sovereignty, security, performance, and integration with existing IT operations while preserving the agility and scale-out characteristics typically associated with public cloud AI services. 

This architecture abstracts the complexities of GPU management, model life cycle, and data services, providing a unified interface for AI applications. The logical architecture consists of four functional layers that transform raw infrastructure into consumable AI services: 

## **Application layer** 

The application layer contains the business-facing AI applications, such as retrievalaugmented generation (RAG) pipelines, autonomous agentic workflows, and developer productivity tools. These applications are deployed as containers and consume platform services through standard APIs. 

## **Platform layer** 

The platform layer orchestrates the AI life cycle and container operations. Nutanix Kubernetes Platform (NKP) manages the life cycle of bare-metal GPU clusters, and Nutanix Enterprise AI (NAI) serves as the inference gateway, providing OpenAI-compatible APIs and model governance. 

## **Infrastructure layer** 

The infrastructure layer provides the foundational compute, storage, and networking resources. It uses Nutanix Cloud Infrastructure (NCI) for virtualized management services and Nutanix Unified Storage (NUS) for high-performance data services. 

## **Management layer** 

The management layer delivers unified visibility and governance through Nutanix Prism Central. This layer handles life cycle management, monitoring, alerting, and security policy enforcement across all clusters. 

The logical architecture isolates critical functions so that management operations, data services, and AI workloads can scale and operate independently without interference. 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

Nutanix Platform Design Blueprint for AI Solutions 

This separation aligns with best practices for enterprise AI deployments and supports the evolving needs of AI applications. 

## **2.1. Nutanix AI Cluster Architecture** 

The Nutanix AI platform architecture integrates deterministic performance, fault isolation, and predictable scaling into enterprise datacenter operations. 

The hardware topology, cluster separation, and deployment boundaries in this design establish multiple independent clusters, each with a distinct role. This separation aligns with NVIDIA guidance for isolating control-plane, data-plane, and GPU-intensive workloads while enabling independent scaling and life cycle management. 

Figure 2: Nutanix Agentic AI Cluster Architecture 

The Nutanix AI platform architecture requires the following distinct cluster types. 

## **Nutanix Cloud Platform management cluster** 

The Nutanix Cloud Platform (NCP) cluster nodes provide the management control plane. These Nutanix hyperconverged infrastructure (HCI) general compute nodes run Nutanix Prism Central, Nutanix Kubernetes Platform (NKP) control plane components, and Nutanix Enterprise AI (NAI) system services. In pure inference use cases, Files Storage and Objects Storage might be consolidated on this cluster to avoid the cost of a dedicated Nutanix Unified Storage (NUS) cluster. 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

Nutanix Platform Design Blueprint for AI Solutions 

## **Nutanix Unified Storage cluster** 

The dedicated Nutanix Unified Storage (NUS) cluster provides persistent data services for model artifacts, retrieval-augmented generation (RAG) corpora, embeddings, and data sets. This cluster is built on storage-dense Nutanix HCI nodes that are optimized for highthroughput storage workloads. This cluster type is optional for pure inference use cases. 

## **Nutanix Enterprise AI workload clusters** 

The workload clusters are one or more bare-metal accelerated compute nodes, managed by NKP as GPU node pools, that you can use for scheduling inference workloads, which are provisioned and hosted by Nutanix Enterprise AI (NAI). The bare-metal GPU hardware is an MGX-based platform and qualified as an NVIDIA-certified server. 

These clusters can be deployed in either of the following deployment models: 

- HCI consolidated mode, which is ideal for pure inference use cases 

- NUS dedicated mode, which is ideal for larger fine-tuning, training, and hybrid or distributed inference use cases 

HCI consolidated mode brings NCP and NUS cluster components together in the same HCI compute cluster. This use model is ideal for workloads that don't require advanced GPU-to-GPU interconnectivity with high-performance storage, such as a proof-of-concept pure inference workload, development or testing, or small- to medium-size production environments, and basic RAG use cases. 

In NUS dedicated mode, NCP and NUS cluster components are deployed as disaggregated, dedicated clusters. This model can provide better isolation, resilience, and performance for medium to large hybrid (mixed) or distributed inference workloads, fine-tuning and training that requires advanced interconnectivity capabilities between GPUs (within and across nodes), and remote storage access (using remote direct memory access or GPUDirect Storage). 

## **Nutanix AI Management Cluster Architecture** 

The management cluster provides the foundational control plane for the Nutanix AI solution design. 

This design isolates the management plane from the compute data plane to maintain responsive management operations regardless of AI workload intensity. 

The Nutanix AI platform architecture covers the following key aspects of the Nutanix Cloud Platform (NCP) management cluster design: 

- VM and node compute and storage requirements 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

Nutanix Platform Design Blueprint for AI Solutions 

- High availability and resilience considerations 

- Sizing and scalability considerations 

These considerations help the management cluster support the deployment and operation of clusters managed by Nutanix Kubernetes Platform (NKP), which host the Nutanix Enterprise AI (NAI) workloads and Prism Central for infrastructure management. The management cluster can scale to align with the NVIDIA AI Factory Reference Architecture's requirements. 

The NCP management cluster is required to deploy NKP-managed clusters, while NAI deploys as a workload on an NKP-managed cluster. 

The NCP management cluster has the following minimum topology: 

- One Prism Central cluster (three-VM scale-out) 

- One NKP management cluster (control plane and worker nodes) 

- Two NKP-managed workload clusters (control plane and worker nodes): 

   - › NAI workload cluster: Runs NAI core system resources for managing the inference endpoint 

   - › Generative AI applications workload cluster: Runs generative AI applications, agentic services, chain servers, and Model Context Protocol (MCP) servers that interact with NAI large language model (LLM) endpoints 

- One Nutanix Unified Storage (NUS) cluster (consolidated mode only): 

   - › Files Storage and Objects Storage (optional): In pure inference–only scenarios, having a dedicated, high-performance NUS cluster might be excessive, so using the existing management cluster to consolidate both NCP management and NUS resources might be acceptable and aligns with the topology in the Nutanix GPT-ina-Box with Nutanix Kubernetes Platform Design. 

   - › Volumes Block Storage (required): The Nutanix Container Storage Interface (CSI) driver uses Volumes Block Storage to dynamically provision block storage for stateful application workloads running on both NKP management and NKPmanaged workload clusters. 

**Note:** The NCP management cluster doesn't include secondary NKP GPU node pools running on NVIDIAcertified servers (which host NAI inference endpoints or generative AI application endpoints that require accelerated compute resources). 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

Nutanix Platform Design Blueprint for AI Solutions 

For more information on NKP components that span both management and managed clusters, see the Monitoring Design for Nutanix GPT-in-a-Box section of the Nutanix GPT-in-a-Box with Nutanix Kubernetes Platform Design. 

## **Nutanix Cloud Platform Management Cluster Requirements** 

In a minimum configuration, Nutanix Cloud Platform (NCP) management clusters require the following compute and storage resources. 

The NCP management cluster includes Prism Central, the Nutanix Kubernetes Platform (NKP) management cluster, and an NKP-managed workload cluster with Nutanix Enterprise AI (NAI) components for a deployment that aligns with the scale requirements of 16 compute or GPU nodes (128 GPUs). 

Table 2: Example NCP Management Cluster VM Resources Summary 

|**Component**|**VMs**|**vCPUs**|**Memory (GB)**|**SSD (GB)**|**Description**|
|---|---|---|---|---|---|
|Prism Central|3|18|82|154|Nutanix|
||||||centralized|
||||||management|
||||||platform|
|NKP|7|44|175|410|Kubernetes|
|management|||||management|
|cluster|||||cluster with|
||||||three control|
||||||plane nodes|
||||||and four|
||||||worker nodes|
|NKP-managed|7|104|246|1,905|Production|
|workload|||||workload|
|cluster (NAI)|||||cluster with|
||||||three control|
||||||plane nodes|
||||||and four|
||||||worker nodes|



© 2026 Nutanix, Inc. All rights reserved  | **15** 

Nutanix Platform Design Blueprint for AI Solutions 

|**Component**|**VMs**<br>**vCPUs**<br>**Memory (GB)**<br>**SSD (GB)**<br>**Description**|
|---|---|
|Total|17<br>166<br>512<br>2,458<br>Total<br>resources<br>for NCP<br>management<br>cluster|



Scaling beyond this minimum configuration requires additional compute and storage resources but aligns with the same architecture and design principles. 

Nutanix hyperconverged infrastructure (HCI) and NKP provide the flexibility to scale out as needed to meet growing workload demands. This configuration can scale linearly, but plan carefully to validate that the underlying infrastructure can support the increased resource requirements. 

**Note:** Consult with Nutanix representatives to validate the proper sizing and configuration to accommodate larger deployments. 

## **Nutanix AI Compute Nodes** 

In the Nutanix AI solution, Nutanix compute nodes support the management and control plane operations. 

Nutanix hyperconverged infrastructure (HCI) compute nodes are a critical component of the Nutanix AI platform architecture. These nodes are responsible for running Nutanix Cloud Infrastructure (NCI) services, which include virtualization, storage management, and cluster orchestration. 

The NX-8170-G10 and NX-8150-G10 models can serve as the underlying HCI compute nodes. They offer different CPU architectures to meet varying performance and workload requirements. 

The following table provides the specifications for the NX-8170-G10 and NX-8150-G10 models. 

Table 3: NX-8170-G10 and NX-8150-G10 Specifications 

|**Specification**|**NX-8170-G10**|**NX-8150-G10**|
|---|---|---|
|Form factor|1U1N (one rack unit with one|2U1N (two rack units with one|
||node)|node)|



© 2026 Nutanix, Inc. All rights reserved  | **16** 

Nutanix Platform Design Blueprint for AI Solutions 

|**Specification**|**NX-8170-G10**<br>**NX-8150-G10**|
|---|---|
|Server compute<br>Boot drive<br>Maximum Memory<br>Maximum processor cores<br>Storage type<br>NVMe+NST storage capacity<br>All NVMe storage capacity<br>All SSD storage capacity|Dual Intel Xeon 6th Gen.<br>(Granite Rapids):<br>•<br>Xeon 6736P (36 cores,<br>2.00 GHz)<br>Dual Intel Xeon 6th Gen.<br>(Granite Rapids):<br>•<br>Xeon 6730P (32 cores,<br>2.50 GHz)<br>•<br>Intel Xeon 6737P (32<br>cores, 2.90 GHz)<br>•<br>Intel Xeon 6748P (48<br>cores, 2.50 GHz)<br>•<br>Intel Xeon 6760P (64<br>cores, 2.20 GHz)<br>2 × 480 GB M.2 boot device<br>2 × 480 GB M.2 boot device<br>4,096 GB<br>4,096 GB<br>40 cores (per CPU socket)<br>64 cores (per CPU socket)<br>NVMe + NVMe Storage Tier<br>(NST); all NVMe<br>NVMe + NST; all NVMe; all<br>SSD<br>•<br>4 NVMe: 15.36 TB<br>•<br>8 NVMe Storage Tier: [60<br>TB]<br>•<br>6 NVMe: 15.36 TB<br>•<br>14 NVMe Storage Tier:<br>60 TB<br>2, 4, 6, 8, 10, or 12 NVMe 3.84<br>TB, 7.68 TB, or 15.36 TB<br>4, 6, 8, 10, 12, 14, 16, 18, 20<br>NVMe: 3.84 TB, 7.68 TB, or<br>15.36 TB<br>N/A<br>[2, 4, 6, 8, 10, 12] x SSD: [3.84<br>TB, 7.68 TB]|



NX-8170-G10 and NX-8150-G10 models have the following networking specifications: 

- Add-on NICs in PCIe slots (RDMA-enabled NICs only): 0, 1, 2, or 3 × 2-port Mellanox CX-7 (200 GbE) or CX6 (100 GbE) NICs 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

Nutanix Platform Design Blueprint for AI Solutions 

- Advance I/O Module (AIOM): 

   - › Dual-port AOS Storage management network interfaces 

   - › Dual-port 10GBASE-T and dual-port SFP+ (Port 1 shared IPMI) 

- On-board network connection: 1 GbE dedicated IPMI (RJ45) 

## **Nutanix AI Storage Cluster Architecture** 

The dedicated NUS cluster provides high-performance, scalable storage for AI workloads in the Nutanix AI platform architecture. 

The following table provides the clusterwide configurations for deploying a NUS dedicated storage cluster for AI workloads: 

Table 4: NUS Dedicated Storage Cluster Configuration 

|**Configuration Type**|**Configuration**|
|---|---|
|Environment type|Production|
|Use case|AI or HPC|
|License|Files Storage (Dedicated)|
|NUS dedicated node cluster profile|Files Storage AI Performance Profile|
|Minimum cluster size|Four-node dedicated NUS hyperconverged|
||infrastructure (HCI) nodes|
|Hypervisor|AHV|
|Growth factor|5% growth over 3 years|
|Controller VM (CVM) vCPU-to-pCore ratio per|1:1 (High Performance Mode)|
|node||
|File server VM (FSVM) vCPU-to-pCore ratio|1:1 (High Performance Mode)|
|per node||
|Failover plan|Standard (n + 1)|
|Replication factor|2|
|Storage configuration|All-NVMe storage and performance tier|
|Throughput target|20–40 GBps per scalable unit (SU)|
|Erasure Coding (see note)|No|



© 2026 Nutanix, Inc. All rights reserved  | **18** 

Nutanix Platform Design Blueprint for AI Solutions 

|**Configuration Type**|**Configuration**|
|---|---|
|Erasure Coding: Dedicated files and objects<br>(see note)<br>Deduplication (see note)<br>Compression (see note)<br>SR-IOV and network offload<br>iSCSI Extensions for RDMA (iSER)<br>NFSoRDMA available<br>Block awareness<br>Rack awareness<br>Data protection|No<br>No<br>No<br>Yes<br>Yes<br>Yes<br>No<br>No<br>No|



We recommend deploying the NUS dedicated storage cluster to align with the Files Storage AI Performance Profile. This profile can help you meet the performance requirements for AI workloads as outlined in the NVIDIA Enterprise AI Factory Reference Architecture. 

The following table provides the minimum required and preferred FSVM and CVM configuration for deploying the Files Storage AI Performance Profile on the NUS dedicated storage cluster. 

Table 5: FSVM and CVM Configuration Requirements 

|**Files Storage Requirement**|**Minimum Required**|**Preferred**|
|---|---|---|
|CVM (vCPUs or cores) per|24|32|
|NUS node|||
|CVM memory per NUS node|128 GiB|256 GiB|
|FSVM vCPUs or cores|24|32|
|FSVM memory|256 GiB|512 GiB|



© 2026 Nutanix, Inc. All rights reserved  | **19** 

Nutanix Platform Design Blueprint for AI Solutions 

## **Files Storage AI Performance Profile Requirements** 

The following list provides the minimum hardware requirements for deploying Files Storage using the AI Performance Profile: 

- Model: NX-8150-G9 or NX-8150-G10 

- CPU generation: Intel Emerald Rapids or Intel Granite Rapids 

- CPU type: 2 × Intel Xeon Silver 4516Y+ or 2 × Intel Xeon 6730P 

- Number of CPUs: 2 × 24 cores (48 cores total) or 2 × 32 cores (64 cores total) 

- Total memory: 1 TB per node (minimum) 

- NVMe storage tier devices: 16 (minimum) 

- NVMe performance tier devices: 4 (minimum) 

- RDMA-enabled network interfaces (dual port): 2 × Mellanox ConnectX-7 (200 GbE) 

- NIC LAN on Motherboard (LOM) or Advanced I/O Module (AIOM) for AOS Storage management network interfaces (dual port): 1 × Mellanox ConnectX-7 (200 GbE) 

- NIC LOM (Onboard): 1 × 1 GbE dedicated IPMI 

For a list of configuration and optimization steps, see Nutanix Files Storage Performance Profile for AI Workloads. 

## **Nutanix AI Workload Cluster Architecture** 

Workload clusters optimized for AI model inference have the following minimum requirements and configurations. 

The Nutanix Enterprise AI (NAI) workload clusters run AI workloads on GPU-accelerated nodes and use Nutanix Kubernetes Platform (NKP) for orchestration and management. To provide a balanced approach to compute, acceleration, and networking resources, the Nutanix AI platform architecture blueprint incorporates elements from the NVIDIACertified Systems program. NAI workload clusters use NVIDIA-certified scale-out baremetal GPU nodes for compute. Using these certified platforms provides a catalog of hardware that ensures predictable performance and compatibility for AI workloads without strictly dictating a specific node-level hardware topology. 

For more information, see NVIDIA PCIe Optimized Reference Configurations (2-8-5-200) with RTX PRO 6000 and H200 NVL and the NVIDIA-Certified Systems section of the NVIDIA Certification Programs Documentation. 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

Nutanix Platform Design Blueprint for AI Solutions 

## **NVIDIA Bare-Metal AI Compute Nodes** 

The Nutanix AI platform design blueprint uses NVIDIA's GPU portfolio to address enterprise AI workloads. 

All AI compute nodes are NVIDIA-Certified Systems that have been tested for performance, reliability, and compatibility with the complete NVIDIA software stack and Spectrum-X Ethernet networking. This certification helps AI compute nodes form reliable building blocks for scaling AI factories, offering system-level testing to support streamlined deployment at scale. 

## **2.2. Nutanix Unified Storage for AI Architecture** 

Nutanix Unified Storage (NUS) provides distinct storage tiers optimized for the Nutanix AI platform architecture. 

NUS serves as the native (and preferred) data layer for the Nutanix AI solution because it offers a unified operational model through Prism Central. However, the architecture uses a modular design, and you can integrate alternative high-performance storage solutions. 

In scenarios where other architectures, existing infrastructure standards, or requirements necessitate alternative storage platforms, you can substitute the NUS dedicated storage cluster. For this alternative solution, incorporate hardware from the NVIDIA-Certified Systems program and consider NVIDIA Enterprise Reference Architecture guidelines. 

## **NVIDIA-Certified Nutanix Unified Storage Configurations** 

The Nutanix AI platform architecture uses Nutanix Unified Storage (NUS) in alignment with NVIDIA-Certified Storage (NVCS) program standards. 

The NVCS program evaluates NUS solutions for the I/O demands of enterprise-scale AI deployments. This program provides a structured evaluation of storage performance and interoperability, specifically as it relates to feeding high-quality data across all stages of the AI pipeline, including training, fine-tuning, and inference. 

The NVCS program's Foundation level certifies NUS solutions that follow the 2-8-5-200 Reference Configuration. These configurations typically use PCIe-optimized systems, such as NVIDIA RTX PRO 6000 platforms, with up to eight GPUs and five network adapters per node. This NVCS level is ideal for entry-level production deployments, including retrieval-augmented generation (RAG) pipelines, small-scale model training, and departmental inference services. 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

Nutanix Platform Design Blueprint for AI Solutions 

Figure 3: NVCS Foundation-Level NUS Architecture 

## **North-South Network Implications** 

To support enterprise AI workload targets, the Nutanix Unified Storage (NUS) nodes must connect to the north-south network fabric. 

You must configure these nodes with a minimum of three ConnectX-7 NICs providing 200 Gbps ports. 

Depending on the target scalable unit (SU) size, which is defined by the total number of GPUs and compute nodes, the north-south network fabric is either converged with the east-west traffic (for smaller clusters) or deployed as a fully dedicated fabric for management and NUS connectivity. 

The following table outlines the physical switch requirements and available ports for NUS connectivity as the cluster scales. 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

Nutanix Platform Design Blueprint for AI Solutions 

Table 6: Available Network Ports and Adapter Requirements for NUS Nodes 

|**SUs**|**Nodes**<br>**GPUs**<br>**Number**<br>**of Leaf**<br>**Switches**<br>**Available**<br>**200 Gbps**<br>**Ports for**<br>**NUS**<br>**Number of**<br>**NUS Nodes**<br>**(with 3**<br>**CX-7s)**<br>**Total CX-7s**<br>**Needed**<br>**per SU**|
|---|---|
|4<br>8<br>16<br>32|16<br>128<br>Converged<br>with east-<br>west<br>network<br>16<br>4<br>12<br>32<br>256<br>2 (dedicated<br>north-south)<br>32<br>4<br>12<br>64<br>512<br>2 (dedicated<br>north-south)<br>64<br>5<br>15<br>128<br>1,024<br>6 (dedicated<br>north-south)<br>128<br>10<br>30|



**Note:** NVIDIA Spectrum-X SN5600 and SN5610 leaf switches can also accommodate 400 Gbps port speeds to support advanced NVIDIA B300 reference configurations. If you use 400 GbE ports, only half of the maximum port counts listed in the table are available. 

## **Network Topology Scaling and Isolation Thresholds** 

You must change the network architecture as you scale the Nutanix AI storage cluster to help meet performance demands without bottlenecks. 

In smaller designs with 1–4 scalable units (SUs) or 4–16 GPU nodes, you can merge north-south data traffic and east-west compute traffic on the same physical switches using VLAN isolation. This approach can lower costs and streamline the architecture. 

However, you must use an isolated east-west network fabric in designs with more than 16 GPU nodes (4 SUs). High-performance AI workloads rely on highly synchronized collective communications (such as All-Reduce operations), so if you scale beyond 16 nodes, you must have a dedicated north-south network for storage or management that is physically isolated from the east-west GPU compute fabric. This prevents computeintensive GPU communication from affecting storage data flows. 

The PCIe-optimized 2-8-5-200 configuration requires five network adapters per node (four east-west and one north-south). Because of this moderate port density, a pair of high-capacity leaf switches can support a converged fabric up to 16 nodes (4 SUs). 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

Nutanix Platform Design Blueprint for AI Solutions 

Scaling past 16 nodes exhausts the top-of-rack port capacity, which requires the split into a dedicated north-south storage fabric and an isolated east-west spine-leaf fabric. 

## **2.3. Nutanix AI Network Architecture** 

The Nutanix agentic AI network provides high-throughput, low-latency communication for AI workloads. 

The NVIDIA Enterprise Reference Architecture guidelines outline three physical networking fabrics. These network configurations are designed to isolate different types of traffic and optimize performance for specific use cases. 

**Note:** For a comprehensive list of supported network configurations and their recommended use cases, see the official NVIDIA Enterprise Reference Architecture documentation. The Nutanix AI platform architecture doesn't include detailed network fabric requirements for common use cases, such as storage, management, customer-facing, admin, or support nodes. 

The following primary networking fabrics are outlined in the NVIDIA Enterprise Reference Architecture guidelines: 

## **Compute node (east-west) Ethernet networking** 

This dedicated fabric handles high-bandwidth, low-latency communication between GPUs. It uses remote direct memory access (RDMA) over Converged Ethernet (RoCE) to facilitate GPUDirect RDMA, which enables direct memory transfers between GPUs across nodes without involving the CPU. 

## **Converged node (north-south) Ethernet networking** 

This fabric handles converged traffic, including end-user application traffic, highperformance storage (HPS) I/O, Kubernetes control plane communication, and external ingress and egress. By using NVIDIA ConnectX or BlueField-3 DPU NICs, the architecture offloads packet parsing, storage management, and security tasks from the host CPU. 

## **Out-of-band (OOB) management networking** 

This fabric is dedicated to infrastructure management tasks, providing a separate network for out-of-band access to server baseboard management controllers (BMCs). 

For the north-south network that connects the Nutanix Cloud Platform (NCP) nodes running AHV to the leaf switches, you must configure Link Aggregation Control Protocol (LACP) instead of an active-passive failover setup. Implementing LACP aligns with AHV deployment best practices of using an active-active link, maximizing aggregate bandwidth for converged storage and management traffic, and providing fault tolerance if a single physical link or top-of-rack switch fails. 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

Nutanix Platform Design Blueprint for AI Solutions 

The following figure shows a typical network topology that includes east-west, northsouth, and OOB management fabrics for a Nutanix agentic AI four-node scalable unit (SU) cluster deployment. 

Figure 4: Nutanix Agentic AI Physical Network Architecture 

**Note:** Nutanix does not provide physical networking hardware, such as switches, transceivers, or cables; instead, the architecture relies on an NVIDIA-Certified networking stack and physical infrastructure provided by ecosystem partners. 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

Nutanix Platform Design Blueprint for AI Solutions 

Specific network hardware choices vary based on organizational preferences and existing infrastructure, and the Nutanix AI platform architecture does not provide recommendations on explicitly using specific NVIDIA switches. We recommend working with certified partners to select hardware that meets the expected performance and scalability requirements 

## **Converged vs. Isolated Network Fabric** 

Select a converged or isolated networking fabric based on your workload, scale, and performance needs, and plan for growth and flexibility. 

If you plan to use the Nutanix AI platform architecture exclusively for pure inference deployments, you can consolidate the east-west (GPU compute network) and northsouth (converged network) fabric traffic into a single converged leaf-spine fabric. You can configure this setup on the same high-performance, NVIDIA-Certified networking (Spectrum-4) switches. 

When designing this converged leaf-spine topology across multiple switches, you must account for the expected crosslink oversubscription ratio. Dedicated east-west compute fabrics for heavy training must use a nonblocking (1:1) architecture, but a converged fabric that supports pure inference and north-south traffic can tolerate a carefully planned oversubscription ratio on the spine uplinks. This approach can reduce costs and simplify the network architecture while providing sufficient bandwidth and low latency capabilities required for inference workloads compared to an isolated east-west fabric. 

You must test RoCEv2 behavior, PFC and ECN configurations, storage traffic, and GPU collective communication under load to validate that the chosen oversubscription ratio still provides predictable performance and meets the minimum required bandwidth per GPU to prevent cross-rack bottlenecks. 

## **Nutanix SmartNIC in the North-South Fabric** 

Nutanix SmartNIC uses NVIDIA networking hardware to accelerate the north-south (converged) network fabric. 

In the context of the Nutanix AI platform architecture, this fabric connects the AI compute layer running Nutanix Kubernetes Platform (NKP) and Nutanix Enterprise AI (NAI) to the high-performance storage layer running Nutanix Unified Storage. Additionally, it handles critical data flows, such as model weight loading, checkpointing, and data retrieval. 

Nutanix SmartNIC integrates two distinct configuration modes to optimize this fabric, ensuring that storage traffic doesn't bottleneck GPU usage: 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

Nutanix Platform Design Blueprint for AI Solutions 

- Single root I/O virtualization (SR-IOV): This mode allows direct assignment of physical NIC resources to a VM, bypassing the hypervisor's virtual switch, which is implemented with Open vSwitch (OVS). This feature is particularly useful for highthroughput workloads like Files Storage, where minimizing latency and maximizing bandwidth are critical. By using SR-IOV, the VM can achieve near-native performance by directly accessing the NIC hardware. 

- Data Path (DP) Offload: This mode enables SmartNIC to offload certain networking functions from the host CPU while allowing the VM to use the OVS. This feature is beneficial for workloads that require high network performance and need to maintain integration with the hypervisor's networking stack. DP Offload can help reduce CPU overhead and improve overall network efficiency for AI workloads running on AHV. 

Both modes can be useful for simplifying the configuration and enablement of the northsouth fabric used throughout this platform architecture, while ensuring that AI workloads can efficiently access and process large data sets stored in Files Storage. 

**Note:** When designing a cluster that spans both SR-IOV and DP Offload use cases, plan the virtual switch configuration carefully to avoid conflicting requirements for the physical NIC ports. 

Regarding VM mobility and high availability, SR-IOV relies on hardware passthrough and ties the VM directly to a physical PCIe device on the host. This VM can't migrate, similar to VMs configured with GPU passthrough or strict host affinity rules. When a host fails, you can cold-start the affected VM on a surviving node only if the target host has an identical SR-IOV NIC profile with available hardware resources. For more information, see Single Root I/O Virtualization Limitations and Live Migration Restrictions in the AHV Administration Guide. 

## **Files Storage AI Performance Profile with SR-IOV** 

The Files Storage AI Performance Profile maps physical NIC functions to maximize throughput and minimize latency. 

Storage in the north-south fabric is provided by Files Storage, and we recommend aligning your configuration with the guidance in Files Storage Performance Profile for AI Workloads (Nutanix Portal credentials required). The Files Storage performance profile is a predefined compute, storage, and networking configuration optimized for highthroughput, low-latency AI workloads typical of large language model (LLM) inference and model serving. 

© 2026 Nutanix, Inc. All rights reserved  | **27** 

Nutanix Platform Design Blueprint for AI Solutions 

The Files Storage performance profile for AI includes several optimizations specifically designed to enhance performance: 

- Single root I/O virtualization (SR-IOV): Bypasses the hypervisor networking stack, allowing file server VMs (FSVMs) to directly access NIC hardware for maximum throughput and minimum latency 

- iSCSI Extensions for RDMA (iSER): Offloads the data path between the Controller VM (CVM) and FSVM to the NIC hardware, creating a fast path for internal storage I/O 

- NFS over RDMA (NFSoRDMA): Enables clients to access file shares using RDMA, eliminating TCP/IP stack overhead 

Figure 5: Nutanix Node Logical Networking Using RDMA, iSER, and SR-IOV 

The following figure shows an example of the physical node connections that correspond with the logical network architecture provided in the previous figure and are required for a single NUS HCI node when connecting to a highly available Spectrum-X network fabric. 

**Note:** This example uses the NX-8170 model, but the number of ConnectX-7 ports and the purpose for each connection are the same for NX-8150 models. 

© 2026 Nutanix, Inc. All rights reserved  | **28** 

Nutanix Platform Design Blueprint for AI Solutions 

Figure 6: Physical Node Connection for Highly Available NUS AI Performance Profile 

In the previous figure, the solid lines represent the active interfaces that provide primary throughput, and the dashed lines represent the passive or standby interfaces that remains idle until a failure is detected to provide service continuity. Every node is crossconnected to the two independent switches, further reducing the likelihood of a single point of failure. 

For more information on configuration and optimization features, see Nutanix Files Performance Profile for AI Workloads. 

## **NFS Over RDMA and DP Offload** 

For the worker nodes on AHV, we recommend the Data Path (DP) Offload mechanism to accelerate NFS over remote direct memory access (NFSoRDMA) traffic. 

The compute side of the north-south fabric consists of Nutanix Kubernetes Platform (NKP) worker nodes hosting AI applications. These nodes use DP Offload (also known as Network Offload) to accelerate storage traffic without using single root I/ O virtualization (SR-IOV) passthrough on the nodes while retaining software-defined networking features like IPAM and microsegmentation. 

NKP worker nodes use the Nutanix Container Storage Interface (CSI) Driver to provision and manage Persistent Volumes (PVs) backed by Files Storage. When these PVs are mounted using NFS over RDMA (NFSoRDMA), the DP Offload capability on the SmartNIC accelerates the NFS traffic between the NKP worker nodes and the Files Storage cluster. 

Using DP Offload on the NKP worker nodes provides several benefits: 

© 2026 Nutanix, Inc. All rights reserved  | **29** 

Nutanix Platform Design Blueprint for AI Solutions 

- Reduced CPU overhead: By offloading NFS processing to the SmartNIC hardware, host CPU cycles are freed up for AI application logic, improving overall performance. 

- Near line-rate throughput: The combination of NFSoRDMA and DP Offload enables near line-rate data transfers between the NKP worker nodes and Files Storage, minimizing latency and maximizing bandwidth for AI workloads. 

- Simplified management: DP Offload allows the NKP worker nodes to remain integrated with the AHV virtual networking stack, simplifying network management and maintaining compatibility with Nutanix networking features. 

When the NKP worker nodes mount these NFS shares, the DP Offload path accelerates the traffic. This enables the worker nodes to use NFSoRDMA to communicate with the Files Storage cluster that has SR-IOV enabled, enabling a high-performance data path from storage to compute. 

## **Nutanix Enterprise AI Model Loading Acceleration** 

In the Nutanix AI platform architecture, an optimized north-south fabric can minimize latency and maximize throughput to load model weights efficiently. 

Nutanix Enterprise AI (NAI) deployments typically store large language models (LLMs) in Files Storage using ReadWriteMany (RWX) volumes. This storage configuration facilitates shared access across multiple Nutanix Kubernetes Platform (NKP) worker nodes. Model weights can be substantial in size, with many LLMs exceeding tens of gigabytes. When NAI deploys an inference service (such as using NVIDIA Inference Microservices (NIM) or vLLM), it loads these model weights from the shared RWX volume. 

You can optimize the north-south fabric by combining Files Storage with single root I/ O virtualization (SR-IOV) and NKP worker nodes with Data Path (DP) Offload. NAI can use the optimized north-south fabric to reduce cold-start latency effects when initializing LLM endpoints. This method improves the availability and resilience of the underlying inference infrastructure. 

Between NKP worker nodes and Files Storage, NFS over Remote Direct Memory Access (RDMA) with DP Offload creates a high-performance data path for loading model weights. This process is especially important for LLMs, where large model files must be transferred quickly to minimize startup times. 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

Nutanix Platform Design Blueprint for AI Solutions 

To further optimize this data path, NAI can use GPUDirect Storage (GDS) capabilities natively. GDS allows model weights to be transferred from Files Storage by creating a direct data path from the physical storage (using Nutanix SmartNIC) to the GPU memory, bypassing the host CPU and system memory entirely. 

**Note:** The performance benefits and behavior of SR-IOV and DP Offload using the SmartNIC are proposed architectural capabilities. You must test and validate actual throughput and latency improvements under specific application loads during the deployment phase. 

## **2.4. AI Platform Layer Overview** 

The AI platform services layer handles operations, monitoring, and life cycle management across all tiers of the Nutanix AI solution. 

The AI platform services layer is a critical component of the Nutanix AI solution. Using Nutanix Cloud Platform (NCP) capabilities, you can efficiently manage the distributed environment through a unified interface. The AI platform layer is composed of the application and infrastructure software layers. 

The application software layer of the Nutanix AI platform architecture represents the set of AI-enabled services, runtimes, and workflows that deliver business outcomes on the platform. This layer includes both Nutanix-managed AI services and customer-deployed AI frameworks that run in the Nutanix AI platform architecture but are not managed directly by the platform. 

The infrastructure software layer transforms raw GPU and storage capacity into a cohesive, cloud-like environment for modern AI workloads. This layer manages the life cycle of bare-metal compute nodes, orchestrates Kubernetes clusters, and automates the deployment of high-performance data services. By integrating Nutanixnative management with NVIDIA-Certified operators, the platform provides a unified governance model for the entire solution. 

**Note:** The platform capabilities, inference engines, and application workflows described in the following sections represent proposed architectural design patterns. Because AI application requirements are highly variable, these sections are conceptual. Specific performance outcomes, maximum concurrent user scaling, and token throughput depend entirely on your organization's defined test workloads and model selections. 

© 2026 Nutanix, Inc. All rights reserved  | **31** 

Nutanix Platform Design Blueprint for AI Solutions 

## **AI Application Software Layer Architecture** 

Nutanix Enterprise AI serves as the primary managed AI service in the application software layer. 

Nutanix Enterprise AI (NAI) is a comprehensive inference endpoint management solution that streamlines model orchestration on Kubernetes clusters. From an application perspective, NAI functions as a platform as a service by exposing centralized large language model (LLM) inference capabilities through OpenAI-compatible APIs. Key NAI features and capabilities: 

- Centralized inference management: NAI provides a single dashboard to deploy and manage all AI infrastructure, including retrieval-augmented generation (RAG) pipelines, conversational agents, and standard inference endpoints. 

- OpenAI-compatible gateway: The platform provides a centralized gateway with an OpenAI-compatible REST API for integration with existing AI applications and developer tools. 

- Optimized model serving: NAI supports multiple inference engines, including vLLM and TensorRT-LLM, to provide high-throughput and low-latency performance on GPU or Intel AMX-enabled CPU accelerators. 

- AI Gateway mode and unified endpoints: NAI features AI Gateway mode, which provides a unified API to manage and route requests across all models, whether you host them on-premises or in the cloud. You can create unified endpoints that integrate with external providers (such as Anthropic, Amazon Bedrock, OpenAI, or Google Cloud Vertex AI) alongside local models. This ability offers you vendor choice, cost control through granular rate limits per API key, and enhanced reliability through configurable fallback models. 

- Intelligent key-value (KV) cache aware routing: To optimize performance, NAI includes KV cache aware routing. For multi-turn chat applications and complex RAG workflows, the system intelligently routes requests to a GPU that already has the conversation history saved. This feature helps reduce the time to first token (TTFT) and boost overall throughput. 

- Speculative decoding: Powered by vLLM's speculative decoding technique, NAI predicts multiple future tokens quickly and verifies them with the main model. This feature saves main model computation time and reduces inter-token latency, resulting in faster overall response times for tasks like code generation and summarization. 

© 2026 Nutanix, Inc. All rights reserved  | **32** 

Nutanix Platform Design Blueprint for AI Solutions 

- Model fine-tuning (LoRA): You can turn general-purpose models into domain specialists using the Low-Rank Adaptation (LoRA) technique. This approach allows you to customize base models with proprietary data without requiring massive compute resources. Once trained, the customized model is automatically registered and managed by NAI. 

- Remote Model Context Protocol (MCP) server integration: NAI provides centralized and secure control over the tools used by AI agents through remote MCP servers. This feature simplifies integration by aggregating multiple tool servers behind a single interface and implements robust governance with role-based access control (RBAC). 

- Advanced vLLM inference sandbox: Developers can test and deploy customized vLLM versions with custom command-line interface (CLI) arguments in an isolated environment without waiting for major platform releases to use new configurations. 

Regardless of the deployment mode, the application interaction model perfectly mirrors public cloud AI APIs, and you can retain absolute architectural control. For sensitive workloads, inference runs entirely within the enterprise-controlled Nutanix AI platform architecture. In this scenario, model weights, prompts, responses, and intermediate data can remain strictly on your infrastructure to help you achieve compliance with security, data residency, and sovereignty requirements. 

Simultaneously, when in AI Gateway mode, you can use the platform to route lesssensitive requests to external cloud providers for a flexible hybrid AI operating model managed through a single control plane. 

NAI functions as an abstraction layer that decouples application behavior from infrastructure complexity by managing the following responsibilities on behalf of applications: 

- GPU placement and scheduling: NAI determines where inference workloads run across available GPU resources managed by Nutanix Kubernetes Platform (NKP). It pools and allocates capacity dynamically so applications don't have to manage GPU affinity. 

- Model loading and life cycle management: NAI controls the onboarding, activation, versioning, and retirement of models. Applications reference models by logical identifiers rather than physical locations, allowing you to update or replace models without requiring application changes. 

© 2026 Nutanix, Inc. All rights reserved  | **33** 

Nutanix Platform Design Blueprint for AI Solutions 

- Runtime configuration and scaling behavior: NAI manages runtime parameters like concurrency limits and continuous batching without exposing scaling logic to developers. 

By providing this abstraction, NAI enables multiple AI applications (such as chat assistants, enterprise search tools, coding assistants, and agentic workflows) to safely share both GPU and CPU resources. Applications remain focused on business logic and user experience, while the platform enforces resource isolation, governance, and performance controls. 

Figure 7: NAI Inference Endpoint Overview 

This design aligns with NVIDIA Enterprise Reference Architecture principles by centralizing inference control, improving GPU utilization efficiency, and preventing application-level infrastructure coupling. It allows enterprises to deliver cloud-like AI consumption models on-premises while maintaining operational consistency and control at scale. 

## **Model Life Cycle and Hub Integration** 

Nutanix Enterprise AI (NAI) integrates directly with public and private model registries to simplify model acquisition and deployment. 

NAI integrations: 

- Hugging Face integration: You can import prevalidated text-generation LLMs directly from the Hugging Face Model Hub using access tokens. 

© 2026 Nutanix, Inc. All rights reserved  | **34** 

Nutanix Platform Design Blueprint for AI Solutions 

- NVIDIA GPU Cloud (NGC) catalog: The platform supports importing NVIDIA Inference Microservices (NIM) from the NGC catalog. 

- Manual and custom imports: For dark sites and air-gapped or network-restricted deployments (requiring HTTP/S Proxy integration), NAI can import custom or prevalidated models from local Network File System (NFS) exports or S3-compatible object buckets. 

- Prevalidated models: Nutanix maintains a catalog of tested models, including Llama-3.1, Mistral, and Granite. 

## **Nutanix Enterprise AI Labs** 

The Nutanix Enterprise AI (NAI) Labs feature provides the following preview-mode applications for verifying inference endpoints: 

- Chat Application: Enables immediate testing of text-generation endpoints through an interactive interface 

- Talk To My Data: Facilitates RAG pipeline validation by allowing you to upload documents and query them using an integrated set of LLM, embedding, and reranker endpoints 

NAI Labs can integrate with external Milvus vector database instances for improved performance and scalability. Additionally, when configuring the Talk To My Data application, you can add specialized Object Detection endpoints (like nemoretrieverparse) and Safeguard endpoints (like LlamaGuard or NeMo Guard) for content safety and advanced document processing. 

In an enterprise-scale Nutanix AI platform architecture, you can integrate your enterprise retrieval-augmented generation (RAG) pipelines with an external Milvus vector database, deployed using NVIDIA GPU devices. This configuration can benefit from accelerator optimizations by using libraries such as NVIDIA cuVS. 

## **Nutanix Enterprise AI Performance and Observability** 

For responsive applications and efficient resource use, the Nutanix AI platform architecture uses an advanced observability stack and inference optimizations. 

These features provide deep, real-time visibility into both application-level inference performance and underlying hardware health. NAI's inference engine can maximize the performance of generative AI workloads. To maximize throughput and minimize latency, NAI uses the following advanced serving optimizations: 

© 2026 Nutanix, Inc. All rights reserved  | **35** 

Nutanix Platform Design Blueprint for AI Solutions 

- Continuous batching and flash attention: These techniques optimize how the GPU queues and processes requests, significantly increasing token throughput and reducing memory bottlenecks during concurrent inference requests. 

- Model quantization: With this technique, you can run models in reduced-precision formats to lower GPU memory footprints and accelerate response times. 

- Speculative decoding: Powered by vLLM, this technique predicts multiple future tokens quickly and verifies them with the main model, saving main model computation time and reducing inter-token latency. 

The NAI Dashboard provides specialized widgets that track real-time application metrics. Administrators and developers can filter these metrics over specific intervals (15 minutes, 1 hour, or 24 hours) to identify bottlenecks: 

- Usage metrics: Tracks the total API requests, successful and failed requests, token usage (input and output), and Cached Token Usage (specifically for vLLM endpoints to track KV-cache efficiency) 

- Latency and queue metrics: Monitors end-to-end request latency, the number of requests currently running, and the number of requests waiting in the inference engine queue 

- Token generation speeds: Provides granular visibility into TTFT (the delay before the first token is streamed back) and time per output token (TPOT) (the average time taken to generate each subsequent token) 

- Throughput: Measures the overall tokens generated per second across endpoints 

NAI's observability extends to the underlying infrastructure and monitors the physical resources for optimal performance and reliability: 

- Cluster and node usage: The dashboard displays real-time CPU capacity, system memory usage, and storage disk consumption across the Kubernetes cluster. 

- GPU telemetry: Using the NVIDIA Data Center GPU Manager (DCGM) Exporter, the platform tracks critical compute hardware metrics, including GPU utilization percentages and GPU memory usage across individual bare-metal nodes. 

NAI's observability capabilities extend beyond the built-in dashboard, allowing you to integrate it with your existing monitoring ecosystem for a unified view of application and infrastructure health: 

© 2026 Nutanix, Inc. All rights reserved  | **36** 

Nutanix Platform Design Blueprint for AI Solutions 

- OpenTelemetry integration: You can configure NAI with the OpenTelemetry Collector to export metrics from the API server, vLLM and NIM inference engines, Node Exporter, and the NVIDIA DCGM Exporter. Then you can export these metrics to any OpenTelemetry-compliant observability tool for correlated, full-stack visibility. 

- Centralized logging stack: In Nutanix Kubernetes Platform (NKP), an integrated stack of Prometheus, Grafana, and Loki captures cluster health and operational logs. 

- Remote syslog and audit events: Track application events, model import logs, endpoint failures, and user actions (like API key creation or model deletion) using the NAI Audit Events dashboard. You can forward these events to an external Syslog server for compliance and security auditing. 

- Automated support bundles: For deep troubleshooting, you can generate diagnostic support bundles using the troubleshoot.sh plug-in, which collects cluster logs, metrics, and Helm release states to accelerate issue resolution with Nutanix Support. 

## **Endpoint Security and Access Control** 

Govern access to Nutanix Enterprise AI (NAI) endpoints, foundation models, and platform administration with a multilayered security architecture. 

The NAI security architecture can help you protect sensitive data, enforce compliance, and prevent unauthorized resource consumption using the following methods: 

- Identity and access management (IAM): NAI integrates directly with enterprise directory services to support local user accounts alongside Active Directory, OpenLDAP, and SAML-based single sign-on (SSO). This integration can provide an authentication experience that aligns with your existing identity policies. 

- Role-based access control (RBAC): You control strict access rights on the platform through Authorization Policies, which bind user identities or groups to specific roles: 

   - › AI/ML Admin: This role has full control over the infrastructure, user management, and all model deployments. 

   - › AI/ML User: This role operates with least-privilege access. Users can deploy models but are restricted to viewing, testing, updating, and deleting only the specific endpoints and API keys they've provisioned. 

- Workspace governance: You can create isolated workspaces with dedicated RBAC and resource quotas to maintain secure multitenancy across different business units or project teams. 

© 2026 Nutanix, Inc. All rights reserved  | **37** 

Nutanix Platform Design Blueprint for AI Solutions 

- Granular model access control: To maintain strict governance over the AI supply chain, enforce model access control policies. With this capability, you decide which models users are allowed to download and deploy. You can create specific allowlists for the Hugging Face and NGC catalogs, and they can globally disable direct model URL downloads or manual offline uploads to prevent unapproved or potentially malicious models from entering the environment. 

- API key management and rate limiting: Application access to shared inference endpoints is strictly authenticated with API keys, which developers and administrators can dynamically create, rotate, or revoke. When operating in AI Gateway mode with unified endpoints, you can enforce strict cost controls and prevent token exhaustion by applying granular rate limits. These thresholds are measured in total tokens per minute and can be configured as global rate limits (capping the total token usage across all API keys accessing a specific unified endpoint) or rate limits per API key (restricting the token throughput for individual applications or users accessing the endpoint). 

- Audit logging and syslog integration: For comprehensive compliance monitoring, all platform activities are captured in detailed audit logs. NAI tracks a wide range of events, including user sign-ins, model imports, endpoint modifications, and API key generation. You can filter these events by user, action, entity type, or support telemetry and export them for external analysis. Additionally, NAI can automatically forward these logs to an external syslog server, allowing security teams to monitor AI infrastructure activities within their centralized security information and event management (SIEM) platforms. 

## **AI Infrastructure Software Layer Architecture** 

At the infrastructure software layer of the Nutanix AI platform architecture, Nutanix Kubernetes Platform (NKP) orchestrates AI applications. 

NKP is engineered to manage the complexities of large-scale containerized environments without manual configuration overhead. The following figure offers a highlevel overview of the NKP management and managed clusters. 

© 2026 Nutanix, Inc. All rights reserved  | **38** 

Nutanix Platform Design Blueprint for AI Solutions 

Figure 8: Nutanix Kubernetes Platform Architecture Overview 

For more information, see the Architecture section of the Nutanix Kubernetes Platform Guide. 

Production-level NKP clusters offer resilience by running multiple nodes for the control plane and etcd. For high availability, Kubernetes deployments use multiple replica pods and implement pod antiaffinity rules. This approach helps maintain service availability, even in the event of a worker node update or failure. 

Kubernetes services and ingress controllers perform the essential service of load balancing by evenly distributing network traffic across all available pods, enhancing service reliability and system performance using components that include (but are not limited to) Cert-Manager, Cilium, Kube-VIP, MetalLB, and Traefik. 

© 2026 Nutanix, Inc. All rights reserved  | **39** 

Nutanix Platform Design Blueprint for AI Solutions 

For more information on the applications that NKP supports and where to deploy them based on licensing and cluster deployment type, see the Supported Platform Applications section of the Nutanix Kubernetes Platform Guide. 

For more information on NKP services and their respective dependencies, see the Platform Applications Dependencies For All Clusters section of the Nutanix Kubernetes Platform Guide. 

NKP also provides Kubernetes-native orchestration patterns for bare-metal GPU node provisioning, OS image life cycle management, GPU driver automation, and declarative workload deployment using GitOps fundamentals. 

- Bare-metal orchestration: NKP manages GPU nodes on bare metal to eliminate the 5–10 percent performance overhead associated with virtualization. This approach maximizes the return on hardware investments for intensive LLM workloads. 

- Multicluster fleet management: Through a centralized management cluster, NKP provides observability and unified policy control over a fleet of managed clusters across different failure domains. 

- GitOps-driven delivery: Built-in integration with FluxCD enables declarative, versioncontrolled deployment of AI applications and infrastructure configurations, promoting consistency and simplifying rollbacks. 

## **Nutanix and NVIDIA Kubernetes Operators** 

Use Nutanix and NVIDIA Kubernetes operators for data services and life cycle management in the Nutanix AI platform architecture. 

Nutanix-native operators provide Kubernetes with access to high-performance storage and automated data services. 

- Nutanix Container Storage Interface (CSI) driver: Facilitates dynamic provisioning of persistent volumes from Nutanix Unified Storage (NUS) and supports both iSCSI and RDMA-enabled iSER protocols for low-latency block storage access 

- Nutanix COSI driver: Enables Kubernetes workloads to provision and manage S3compatible object storage buckets on demand (ideal for RAG corpora and retrieval data sets) 

- Nutanix Data Services for Kubernetes (NDK): Provides application-consistent snapshots and cross-cluster replication to protect the state of AI applications and data sets 

© 2026 Nutanix, Inc. All rights reserved  | **40** 

Nutanix Platform Design Blueprint for AI Solutions 

The architecture uses specialized NVIDIA operators to automate the life cycles of hardware-specific software components in the Kubernetes environment. 

- NVIDIA GPU Operator: Automates driver installation, container runtime configuration, and device plug-in deployment and integrates the Data Center GPU Manager (DCGM) for real-time telemetry on GPU health, usage, and power consumption 

- NVIDIA Network Operator: Orchestrates the high-speed networking stack required for multinode AI training, automatically configures RDMA and SR-IOV virtual functions, and automates DOCA-OFED driver installation to enable low-latency GPUDirect RDMA paths 

## **2.5. Generative AI Applications Layer Overview** 

The following common AI application scenarios serve as design blueprints for the Nutanix AI platform architecture. 

We selected these scenarios to demonstrate how the platform supports common enterprise AI adoption patterns using NVIDIA Enterprise Reference Architecture–aligned infrastructure and Nutanix software services. 

We do not prescribe specific applications or tools for these scenarios; we provide repeatable architectural patterns that can be consistently implemented, scaled, and operated in an enterprise AI factory design. 

These scenarios intentionally avoid application-specific optimizations and focus on platform-level integration and operational behavior. 

Proposed use cases: 

- Talk to My Data (retrieval-augmented generation (RAG) pipeline): Talk to My Data builds secure RAG pipelines using the NVIDIA AI Data Platform (AIDP) Blueprint and Nutanix Enterprise AI (NAI), integrating large langauge models (LLMs) with enterprise data stored in Objects Storage for fast, governed, and scalable retrieval. 

- Browsing agent: This use case offers real-world examples of how Nutanix uses AI to help knowledge workers quickly surface internal documentation, technical references, and best-practice guidance—reducing search friction and improving productivity. 

- Coding agent: Coding agents running high-reasoning LLMs on NAI support developers with code generation, code review, troubleshooting, and integration guidance 

© 2026 Nutanix, Inc. All rights reserved  | **41** 

Nutanix Platform Design Blueprint for AI Solutions 

—securely referencing internally hosted repositories, wikis, and engineering documentation 

- Shared inferencing as a service (using NAI): Multiple AI applications can securely share centralized inference resources through NAI, providing a unified control point for model hosting, routing, and life cycle management. This architecture enables dynamic scaling, optimized GPU usage, and consistent model governance across teams and applications. 

## **Nutanix and NVIDIA AI Data Platform Overview** 

As a certified storage partner in the NVIDIA AI Data Platform ecosystem, Nutanix provides enterprise-grade object and file storage solutions. 

The following Nutanix software services align with and implement the NVIDIA AI Data Platform (AIDP) blueprint: 

- Objects Storage serves as the retrieval-augmented generation (RAG) data tier, providing S3-compatible object storage for document corpora, embeddings, and unstructured data sets. It integrates with NVIDIA NeMo Retriever and third-party vector databases to support high-concurrency retrieval workloads. 

- Files Storage provides high-performance NFS storage for large language model (LLM) weights, checkpoints, and intermediate data sets, using the AI Performance Profile (including NFSoRDMA and GPUDirect Storage) to maximize data throughput. 

- Nutanix Enterprise AI (NAI) functions as the inference gateway, exposing OpenAIcompatible APIs for embedding generation, LLM inference, and reranking services. NAI integrates NVIDIA NIM microservices and provides centralized model life cycle management, governance, and observability. 

- Nutanix Kubernetes Platform (NKP) orchestrates NVIDIA AIDP component deployment (such as NeMo Retriever and vector databases) on bare-metal GPU nodes, ensuring optimal resource utilization and Kubernetes-native scaling. 

By aligning with the NVIDIA AIDP blueprint, the Nutanix AI platform design blueprint helps you build secure, scalable, and governed RAG pipelines and agentic AI workflows on enterprise-controlled infrastructure while you maintain compliance with data sovereignty, security, and operational consistency requirements. 

© 2026 Nutanix, Inc. All rights reserved  | **42** 

Nutanix Platform Design Blueprint for AI Solutions 

## **NVIDIA AI Data Platform on Nutanix Workload Sizing** 

Carefully weigh the size of workloads against your requirements, and target Nutanix AI infrastructure capabilities with a Reference Configuration. 

Sizing guidelines for NVIDIA AI Data Platform (AIDP) workloads in the Nutanix AI platform architecture follow the NVIDIA AI Data Platform Design Guide for Storage Partners. 

**Note:** Work closely with Nutanix and NVIDIA solution architects to understand and achieve the right AIDP workload sizing for your requirements. 

Consider the following workload characteristics: 

- Volume of data to ingest (in GB or TB) 

- Ingestion throughput (documents per second) 

- Average query complexity (input size and context length) 

- Average response size (output size, max tokens) 

- Average and peak number of concurrent users and queries 

- Rate of change (every 24 hours versus real-time) 

- Type of data being ingested (text versus multimodal) 

These characteristics—and others not included in this list—directly affect the compute, GPU, and storage sizing requirements for the AIDP workload. 

Currently, the only supported NVIDIA-certified Reference Configurations for AIDP workloads are based on the OVX-certified server and storage configurations, including the PCIe-optimized 2-8-5-200 Reference Configuration with RTX PRO 6000 Blackwell Server Edition GPUs. 

Beyond the workload characteristics and Reference Configurations, other decisions that can affect sizing include, but are not limited to, the following factors: 

- Model selection and parameter size: Affects GPU memory requirements 

- Model weights size on disk (in GB): Affects storage requirements 

- Model precision (FP16 versus INT8): Affects GPU Memory and inference performance 

- Inference engine (vLLM versus TensorRT-LLM, and so on): Affects compute requirements 

© 2026 Nutanix, Inc. All rights reserved  | **43** 

Nutanix Platform Design Blueprint for AI Solutions 

- Number of concurrent requests or users and token request patterns: Affects GPU usage and throughput requirements 

In all cases, see the NVIDIA AI Data Platform Design Guide for Storage Partners for detailed guidance on sizing AIDP workloads and contact your Nutanix or NVIDIA representative for assistance. 

## **2.6. Nutanix Agentic AI Security Architecture** 

The Nutanix AI platform architecture offers a comprehensive defense-in-depth model that spans hardware, network, platform, and application layers. 

By adopting robust security and compliance principles, the architecture meets the needs of regulated industries, government agencies, and organizations with strict data governance requirements. Network segmentation isolates control-plane, storage, and GPU fabrics into distinct security zones, minimizing lateral movement and enforcing hardware-backed trust boundaries. 

Building on these foundations, Nutanix Kubernetes Platform (NKP) provides hardened Kubernetes orchestration that aligns with strict enterprise and public sector security guidelines. Access governance follows least-privilege role-based access control (RBAC), and NAI provides fine-grained model endpoint permissions and policy controls to separate duties across operators, developers, and consuming applications. 

For auditing and compliance, Files Storage, Objects Storage, and Nutanix Unified Storage (NUS) snapshots provide immutable data protection against cyberattacks such as ransomware and help you meet stringent regulatory expectations. Comprehensive logging, event tracing, and platform-level attestation support alignment with government compliance frameworks. 

With these capabilities, the Nutanix AI solution operationally reinforces the stringent security and compliance standards required by highly regulated enterprise and public sector environments. 

© 2026 Nutanix, Inc. All rights reserved  | **44** 

Nutanix Platform Design Blueprint for AI Solutions 

## 3. Nutanix AI Design Blueprint 

The Nutanix AI solution offers a blueprint help you accelerate time-to-value for AI initiatives. 

The following proposed configurations combine NVIDIA-certified components into design blueprints tailored for different enterprise requirements. 

**Note:** These configurations are architectural proposals, not lab-validated end-to-end deployments. 

## **3.1. Converged Design Blueprint** 

The Converged design blueprint offers the lowest barrier to entry into the Nutanix AI portfolio. 

It is the fastest path to deploying AI and proving business value with a minimal rack footprint. Optimized for initial AI enablement, proof-of-concept deployments, and costeffective small to medium model inference, this configuration delivers 32 NVIDIA RTX PRO 6000 Blackwell Server Edition GPUs across four bare-metal compute nodes (1 scale unit). The resulting 384 GB of GPU memory per node yields a total cluster capacity of 1,536 GB (1.5 TB). 

The network architecture employs a highly cost-efficient, collapsed leaf-spine design where east-west compute traffic shares the same infrastructure as the north-south data and management traffic (Consolidated topology). Each GPU node connects through four single-port B3140H (or CX-7) SuperNICs, providing 200 Gbps each. One dual-port B3220 DPU provides 400 Gbps of north-south connectivity. The entire configuration requires only four switches (2 × SN5610 converged leaf switches and 2 × SN2201 out-ofband management switches), making it exceptionally straightforward to deploy. 

From a storage and management perspective, this entry-level architecture uses a fully converged approach to minimize infrastructure costs. Instead of separate dedicated clusters, four Nutanix HCI nodes act as a shared platform. These nodes co-host both the NCP management cluster (including Prism Central and the Nutanix Kubernetes Platform control plane) and the NUS cluster (providing Files Storage and Objects Storage services). This converged HCI storage tier is capable of supporting initial AI pipelines before scaling to a dedicated NVCS infrastructure. 

© 2026 Nutanix, Inc. All rights reserved  | **45** 

Nutanix Platform Design Blueprint for AI Solutions 

Figure 9: Converged NCP Management and NUS Cluster and RTX PRO Rack Layout 

This architecture can start small and scale out to 4 SU (16 nodes) to accommodate growing inference demands. 

© 2026 Nutanix, Inc. All rights reserved  | **46** 

Nutanix Platform Design Blueprint for AI Solutions 

**Note:** The Nutanix AI platform architecture solution is OEM-agnostic. You can implement the configurations using any NVIDIA-Certified System solution that meets the specified hardware requirements. The Cisco UCS platforms included in diagrams are only used as examples. 

## **3.2. Foundation Design Blueprint** 

The Foundation design blueprint represents a common entry point into the Nutanix AI portfolio. 

It is optimized for inference and analytics workloads. This configuration delivers up to 128 RTX PRO 6000 GPUs across 16 nodes, organized into 4 SU. It provides 384 GB of GPU memory per node with a total cluster capacity of 6,144 GB. 

Four NUS storage nodes (NX-8150-G9 or NX-8150-G10) are deployed to provide high-throughput storage capabilities aligned with NVIDIA-Certified Storage (NVCS performance guidance. Four HCI management nodes (NX-8170-G10 or NX-8150-G10) host Prism Central and Nutanix Kubernetes Platform (NKP) control plane components. 

The network architecture employs a consolidated spine-leaf design where east-west compute traffic shares infrastructure with north-south data traffic, simplifying deployment and reducing switch count. Each node connects through 4 × B3140H SuperNICs delivering 1.6 Tbps of east-west bandwidth, while 1 × B3220 DPU provides 400 Gbps of north-south connectivity. The entire configuration requires only four switches (2 × SN5610 leaf and 2 × SN2201 management) and approximately 200 cables. 

© 2026 Nutanix, Inc. All rights reserved  | **47** 

Nutanix Platform Design Blueprint for AI Solutions 

Figure 10: RTX PRO AI Factory 4 SU Rack Layout 

For more information on network and switch configuration, see the NVIDIA RTX Pro AI Factory Reference Architecture documentation. 

Although the proposed blueprint outlines 4 SU (16 nodes), this architecture supports scaling to 8 SU (32 nodes and 256 GPUs). Scaling out to 8 SU typically requires transitioning from a consolidated network to an isolated east-west fabric (separate leaf switches for GPU traffic) to prevent port oversubscription and promote predictable performance for larger inference clusters. 

© 2026 Nutanix, Inc. All rights reserved  | **48** 

Nutanix Platform Design Blueprint for AI Solutions 

Figure 11: RTX Pro AI Factory 8 SU Rack Layout 

**Note:** The Nutanix AI platform architecture is OEM-agnostic. You can implement the configurations using any NVIDIA-Certified System solution that meets the specified hardware requirements. The Cisco UCS platforms included in diagrams are only used as examples. 

© 2026 Nutanix, Inc. All rights reserved  | **49** 

Nutanix Platform Design Blueprint for AI Solutions 

## 4. Nutanix AI Platform Architecture References 

For more information on the Nutanix AI platform architecture components, see the following references. 

Nutanix documentation: 

- AOS Advanced Administration Guide: Nutanix operating system administration and architecture 

- Nutanix Kubernetes Platform (NKP) Guide: Kubernetes life cycle management and bare metal orchestration 

- Files Storage User Guide: High-performance file services, AI performance profile 

- Objects Storage User Guide: S3-compatible object storage administration 

- Nutanix Enterprise AI (NAI) Guide: Model serving platform and inference gateway 

- Prism Central Admin Center Guide: Multicluster governance and automation 

NVIDIA documentation: 

- NVIDIA Enterprise Reference Architecture: Scale unit (SU) specifications and design guidelines 

- NVIDIA GPU Operator Documentation: Kubernetes GPU resource management 

- NVIDIA Network Operator Documentation: RDMA networking and RoCE v2 configuration 

- NVIDIA DCGM User Guide: GPU monitoring and telemetry 

- Spectrum-X Switch Configuration: Network fabric setup and best practices 

© 2026 Nutanix, Inc. All rights reserved  | **50** 

Nutanix Platform Design Blueprint for AI Solutions 

## **4.1. Nutanix and NVIDIA-Certified Reference Configuration Partner Platforms** 

Cisco, Dell Technologies, HPE, and Lenovo are strategic Nutanix hardware partners that are also NVIDIA-Certified Systems partners. 

Because the list of partners and validated servers changes frequently, for the latest information on NVIDIA-Certified Systems and reference configurations, see NVIDIACertified Systems Documentation. 

This conceptual design document highlights RTX Pro 6000 Blackwell GPUs. Consider all GPU options that meet your requirements, especially if you plan to run larger models. 

The alignment between Nutanix and NVIDIA-validated partners is significant for organizations planning to deploy GPU-accelerated AI workloads on Nutanix Cloud Platform (NCP). With these vendors, you can use validated NVIDIA GPU configurations while maintaining compatibility with Nutanix hyperconverged infrastructure (HCI) software, simplifying procurement decisions and working with a supported infrastructure stack, including compute, storage, and virtualization. This curated approach reduces certification complexity and provides a clear path for enterprises seeking to combine the Nutanix hybrid multicloud capabilities with NVIDIA's AI compute platforms. 

© 2026 Nutanix, Inc. All rights reserved  | **51** 

## **List of Figures** 

Figure 1: Nutanix Agentic AI Solution............................................................................................................4 Figure 2: Nutanix Agentic AI Cluster Architecture....................................................................................... 12 Figure 3: NVCS Foundation-Level NUS Architecture.................................................................................. 22 Figure 4: Nutanix Agentic AI Physical Network Architecture....................................................................... 25 Figure 5: Nutanix Node Logical Networking Using RDMA, iSER, and SR-IOV........................................... 28 Figure 6: Physical Node Connection for Highly Available NUS AI Performance Profile.............................. 29 Figure 7: NAI Inference Endpoint Overview................................................................................................ 34 Figure 8: Nutanix Kubernetes Platform Architecture Overview....................................................................39 Figure 9: Converged NCP Management and NUS Cluster and RTX PRO Rack Layout.............................46 Figure 10: RTX PRO AI Factory 4 SU Rack Layout................................................................................... 48 Figure 11: RTX Pro AI Factory 8 SU Rack Layout......................................................................................49 

Nutanix Platform Design Blueprint for AI Solutions 

## **List of Tables** 

Table 1: Document Version History................................................................................................................7 Table 2: Example NCP Management Cluster VM Resources Summary..................................................... 15 Table 3: NX-8170-G10 and NX-8150-G10 Specifications............................................................................16 Table 4: NUS Dedicated Storage Cluster Configuration..............................................................................18 Table 5: FSVM and CVM Configuration Requirements............................................................................... 19 Table 6: Available Network Ports and Adapter Requirements for NUS Nodes............................................23 


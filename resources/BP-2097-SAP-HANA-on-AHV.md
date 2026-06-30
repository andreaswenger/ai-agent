```
v2.7 | April 2026 | BP-
```
#### SOLUTIONS DOCUMENT

# SAP HANA on Nutanix AHV

# Best Practices


## Legal

```
© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix
product and service names mentioned are registered trademarks or trademarks of
Nutanix, Inc. in the United States and other countries. All other brand names mentioned
are for identification purposes only and may be the trademarks of their respective
holder(s).
Certain information contained in this content may link or refer to, or be based on,
studies, publications, surveys, and other data obtained from third-party sources and
our own internal estimates and research. While we believe these third-party studies,
publications, surveys, and other data are reliable as of the date of publication, they have
not independently verified unless specifically stated, and we make no representation as
to the adequacy, fairness, accuracy, or completeness of any information obtained from
a third-party. Our decision to publish, link to or reference third-party data should not be
considered an endorsement of any such content.
Code samples and snippets that appear in this content are unofficial, are unsupported,
and will require extensive modification before use in a production environment. As
such, the code samples and snippets are provided AS IS and are not guaranteed to
be complete, accurate, or up-to-date. Nutanix makes no representations or warranties
of any kind, express or implied, as to the operation or content of the code samples or
snippet. Nutanix expressly disclaims all other guarantees, warranties, conditions and
representations of any kind, either express or implied, and whether arising under any
statute, law, commercial use or otherwise, including implied warranties of merchantability,
fitness for a particular purpose, title and non-infringement therein.
Nutanix, Inc.
1740 Technology Drive
San Jose, CA 95110
```

## Contents

- 1. Executive Summary.................................................................................
- 2. SAP HANA Deployment Requirements.................................................
   - SAP HANA Deployment Prerequisites...............................................................................................
   - SAP HANA Design Considerations and Caveats...............................................................................
   - Sub-NUMA Clustering Support.........................................................................................................
   - Live Migration for SAP HANA VMs..................................................................................................
   - Hardware and Hypervisor.................................................................................................................
   - SAP HANA VMs and Applications....................................................................................................
- 3. Conclusion..............................................................................................
- About Nutanix.............................................................................................
- List of Figures.............................................................................................................................................


## 1. Executive Summary.................................................................................

```
SAP helps customers migrate from traditional relational databases to their in-memory
SAP HANA database to gain more agility in their business processes. Many SAP
customers search for simple, efficient ways to deploy SAP HANA that minimize risk and
preserve the benefits of an agile platform. Nutanix provides such an option.
The following steps and best practices for deploying SAP HANA on the Nutanix Cloud
Platform can help you achieve the best possible performance and obtain production
support from SAP for your SAP HANA database VMs. We cover all necessary guidelines
and prerequisites for successfully deploying SAP HANA in production on Nutanix AOS
using Nutanix AHV as the hypervisor. Use these technical settings and considerations
to get the most out of your SAP HANA scale-up and scale-out environments running on
Nutanix AOS with AHV.
Nutanix, in collaboration with SAP Engineering, developed these requirements and
recommendations by extensively testing SAP HANA on the Nutanix Cloud Platform.
These settings ensure that you run a fully supported production SAP HANA system.
Compared to traditional three-tier virtualization implementations, deploying SAP HANA
on Nutanix lets you realize several key benefits, including dramatic reductions in
complexity, significant risk avoidance, gains in flexibility, and a shorter time to market.
We wrote this best practice guide for customers, partners, and internal employees
responsible for working on any SAP HANA project. Note that you need credentials for the
SAP Knowledge Base to access some of the SAP notes mentioned in this guide.
If you deploy SAP HANA on Nutanix AOS using VMware vSphere as the hypervisor, see
the SAP HANA on vSphere Best Practices.
Table: Document Version History
Version Number Published Notes
1.0 August 2018 Original publication.
1.1 January 2019 Updated the SAP HANA Best
Practices and References
sections.
```

**Version Number Published Notes**

```
1.2 June 2019 Added Fujitsu certification
information.
1.3 July 2019 Updated the SAP HANA Best
Practices section.
1.4 January 2020 Added Nutanix Support Portal
link.
1.5 October 2020 Added and updated content
throughout.
1.6 January 2021 Updated for consistency with
companion SAP best practice
guides.
1.7 April 2022 Updated the Prerequisites,
Design Considerations and
Caveats, Hardware and
Hypervisor, and VM and
Application sections and
added the Live-Migrating SAP
HANA VMs section.
1.8 August 2022 Updated the Hardware and
Hypervisor section.
1.9 September 2022 Updated the Hardware and
Hypervisor section.
2.0 May 2023 Updated supported AOS
versions.
2.1 September 2023 Updated links to SAP notes
and updated AHV and AOS
content for AOS 6.5.
2.2 November 2023 Updated the Hardware and
Hypervisor section.
2.3 January 2024 Added Intel Sapphire Rapids
microarchitecture support
and updated the Executive
Summary and SAP HANA
Deployment Prerequisites
sections.
```

**Version Number Published Notes**

```
2.4 October 2024 Added supported software
versions and sub-NUMA
clustering and removed
versions no longer supported.
2.5 November 2024 Updated AOS release cycle
descriptions.
2.6 October 2025 Updated AOS release and
added Intel Emerald Rapids
support.
2.7 April 2026 Updated for quad socket Intel
Sapphire Rapids support.
```

## 2. SAP HANA Deployment Requirements.................................................

```
To ensure that you run your SAP HANA VMs in a way that allows for maximum
performance and full production support, work through the following lists to verify your
settings and complete any modifications. These requirements and recommendations
are the direct results of intensive testing and validation exercises run by Nutanix with
guidance from SAP HANA Engineering.
Note: Unless otherwise noted, these requirements are mandatory to achieve full support from SAP and
Nutanix for the implementation.
```
```
We organize our guidance into five main categories:
```
- Prerequisites
- Design considerations and caveats
- Live migration best practices for SAP HANA VMs
- Hardware and hypervisor best practices
- VM and application best practices

### SAP HANA Deployment Prerequisites...............................................................................................

```
Before you install a system, verify that it meets the following prerequisites. For production
landscapes, these prerequisites are mandatory; for nonproduction landscapes, we
strongly recommend adhering to the same prerequisites.
```
- Ensure that you use a supported OS for your VM, according to the SAP Product
    Availability Matrix (PAM) (SAP account required).
    › Running a Windows VM with an SAP application on a production system with AHV
       isn't supported.
    › Running a Windows VM with an application other than SAP with AHV is fully
       supported.


- Use one of the major enterprise Linux distributions supported by Nutanix AHV for SAP
    HANA workloads: SUSE or Red Hat.
       **Note:** Currently, AHV-based VMs can't be part of a Red Hat High Availability cluster with Pacemaker.
       Nutanix and Red Hat are working together to allow support for this solution. AOS 7.3 has a Nutanix AHV
       fence agent for Pacemaker installations that we plan to support in upcoming releases. For a preview with
       implementation steps, see AHV Internals: Red Hat HA Clusters with Nutanix AHV.
- For AOS 6.5.4 and later versions in the 6.5.nn family, ensure that your version of
    Nutanix AHV is AHV20220304.439 or later.
- For AOS 6.8 and later versions in the 6.8.nn family, ensure that your version of
    Nutanix AHV is 9.1 (AHV20230302.100173) or later.
       For more detailed information about Nutanix product releases and support
       cycles, see our Support Policies and FAQs.
- For AOS 6.10 and later versions in the 6.10.nn family, ensure that your version of
    Nutanix AHV is 9.1.2 (AHV20230302.102001) or later.
- For AOS 7.3 and later versions in the 7.3.nn family, ensure that your version of
    Nutanix AHV is AHV 10 or later.
       Nutanix strongly recommends updating to the newest supported AOS release
       validated for SAP HANA as soon as possible.
- Ensure that you use a supported version of SAP HANA.

```
› Both SAP HANA 1 and 2 are supported on the Intel Cascade Lake
microarchitecture.
› Only SAP HANA 2 is supported on the Intel Sapphire Rapids and Intel Emerald
Rapids microarchitectures.
```
- For supported and certified solutions from Nutanix original equipment manufacturers
    (OEMs), see the SAP HANA HCI Hardware directory.
       Only Intel Xeon Scalable Processors with the Intel Cascade Lake, Intel
       Sapphire Rapids, or Intel Emerald Rapids microarchitecture support SAP
       HANA on HCI.


```
Note: To view the Nutanix version running in the cluster, click the user icon in the main menu, then select
About Nutanix from the dropdown list. The About Nutanix dialog that appears displays the AOS and Nutanix
cluster check (NCC) version numbers and includes a link to Nutanix patent information.
```
### SAP HANA Design Considerations and Caveats...............................................................................

```
When you design your environment, keep the following caveats in mind for production
SAP HANA database systems:
```
- The maximum memory size of a single production SAP HANA VM is 6 TB on quad
    socket Intel Sapphire Rapids Processors.
- The maximum memory size of a single production SAP HANA VM is 4.5 TB on quad
    socket Intel Cascade Lake Processors.
- The maximum number of virtual CPUs for a single production SAP HANA VM is 360
    vCPUs on quad socket Intel Sapphire Rapids Processors.
- The maximum number of virtual CPUs for a single production SAP HANA VM is 168
    vCPUs on quad socket Intel Cascade Lake Processors.
       This maximum assumes that the VM has hyperthreading enabled. Be sure
       to consider the CPU generation when establishing the maximum number of
       virtual CPUs.
- Don't place the SAP HANA production database on the same socket as the Controller
    VM (CVM).
- Don't share resources between production SAP HANA VMs and any other VMs.
- You can live-migrate SAP HANA VMs on Nutanix AHV with the following Nutanix AOS
    versions or later: 6.5.6, 6.8.1, 6.10, and 7.3.
       For more details and conditions, see the Live Migration for SAP HANA VMs
       section.
- For production SAP HANA database VMs, adhere to the resource combinations
    outlined in the following table.
_Table: Resource Combinations for Production SAP HANA Database VMs_


**Platform VMs CPUs or Memory
Sockets per VM**

```
Notes
```
Dual Socket 1 1 Example: 120 vCPUs
and 2 TB of RAM on
an Intel Xeon Platinum
8490H Processor

Quad Socket 3 1 3 VMs with 1 socket's
worth of CPU and
memory

Quad Socket 2 1 / 2 1 VM with 1 socket's
worth of CPU and
memory; 1 VM with 2
sockets' worth of CPU
and memory

Quad Socket 1 3 Example: 168 vCPUs
and 4.5 TB of RAM on
Cascade Lake Intel
Xeon Platinum 8280
Processor

Quad Socket 1 3 Example: 360 vCPUs
and 6 TB of RAM on
Sapphire Rapids Intel
Xeon Platinum 8490H
Processor


- Memory configuration for SAP HANA production VMs must follow these rules:

```
› You must assign each SAP HANA production VM the full memory amount of its
assigned sockets.
› Reserve some memory for hypervisor overhead.
The amount of memory depends on several configuration specifics, such
as the number of vCPUs, the number of disk devices, and so on, but plan
for approximately 3–6 percent of the available memory.
› When installing SAP HANA and using the scale-out architecture for your tenant
databases, configure your network environment for jumbo frames.
Follow the steps in Nutanix KB-3529.
› You can use Nutanix encryption methods for security.
› When using software-based encryption, incorporate additional CPU resources for
the CVM into the design.
Note: Nutanix SAP Engineering tested encryption with SAP HANA, and it passed all necessary
key performance indicators (KPIs). For more detailed information, see the Data-at-Rest
Encryption section of the AOS Security Guide.
```
For more details about memory configuration and NUMA, see Nutanix AHV Best
Practices.

We support nonproduction SAP HANA database systems as follows:

- For nonproduction databases, we support the listed production VM configurations and
    database VMs that consume half a socket's worth of CPU and memory resources.
- For nonproduction databases, we support running the database VM in parallel with
    other nonproduction VMs, including the CVM.
- For nonproduction databases, you don't need to follow the strict memory and NUMA
    configuration rules described previously.

For your Nutanix cluster design, consider the following points:

- Always plan for failover capacity in the form of n + 1, so your Nutanix cluster can
    sustain a complete node loss without any manual intervention.


- If you plan to use live migration, ensure that you have enough free resources in your
    cluster to migrate large SAP HANA database VMs.
- If you plan to use live migration for SAP HANA VMs with more than 1 TB of assigned
    memory, use 25 Gbps network connectivity for your Nutanix cluster.
- If possible, start with a four-node cluster. A four-node cluster allows you to complete
    maintenance operations without worrying about free space or timing.
- When you size usable storage on the cluster, ensure the following:
    › For production VMs, assume three times the SAP HANA database memory
       footprint per VM available locally on the node where the VM is running.
    › For nonproduction VMs, assume twice the SAP HANA database memory footprint
       available locally on the node where the VM is running.
- Don't configure storage-saving functionalities such as compression, deduplication, or
    erasure coding on a storage container containing production database files.
       These features deliver no benefits because of how the SAP HANA
       Persistence Engine stores data. Nutanix SAP Engineering has tested
       compression with SAP HANA workloads; the tests indicate no noticeable
       performance impact but also show no reduction in the space the SAP HANA
       workload consumes.

### Sub-NUMA Clustering Support.........................................................................................................

```
With the Intel Sapphire Rapids or Intel Emerald Rapids CPU generations and Nutanix
AOS versions 6.8.nn or 6.10.n, you can use sub-NUMA clustering (SNC-2), a memory
subsystem enhancement created by Intel that you can enable in the server's BIOS.
When you enable SNC-2, the system splits the underlying CPU into two logical disjointed
domains, also referred to as clusters. With SNC-2 enabled on Intel Sapphire Rapids or
Intel Emerald Rapids, each cluster holds exactly half of the cores, last layer cache slices,
memory controllers, and double data rate memory channels.
Note: You can only enable SNC-2 when the physical memory is fully populated.
```
```
SNC-2 exposes the SNC-2 clusters to the hypervisor, so you have four NUMA nodes
available to the hypervisor instead of two, each with exactly half of the physical CPU's
```

resources. You can support up to three productive HANA VMs on a dual-socket server.
Because each SNC-2 NUMA node now only has half of the physical resources, the
maximum values for the number of CPUs and memory are halved for each production
VM.

To use SNC-2, enable the option in the BIOS of your server before imaging the nodes
with Nutanix Foundation. After you enable SNC-2 in the BIOS, Nutanix Foundation
automatically configures the Controller VM (CVM) with the correct settings. If you must
enable SNC-2 on an existing cluster, create a support ticket.

To create an SNC-2 VM, follow the instructions in the Creating SAP HANA VMs section
using one of the following options:

- Host with two single-socket, single-CPU clusters, one with two VMS (one VM per SNC
    node) and one with a VM on one SNC node and a CVM on the other
- Host with two single-socket, single-CPU clusters, one with a VM spanning two
    adjacent SNC nodes and one with a VM on one SNC node and a CVM on the other

```
Figure 1: SAP HANA on a Sub-NUMA Clustered Node
```
_Table: Options for Configuring SAP HANA with SNC_

```
VM NUMA Size Total number of VMs CPUs per VM Notes
Single NUMA node 3 64 Example: 64 vCPUs
and 1 TB of RAM
per VM on an Intel
Xeon Platinum 8592+
Processor
```

```
VM NUMA Size Total number of VMs CPUs per VM Notes
Dual NUMA node and
single NUMA node
```
```
2 128 and 64 Example: One VM with
128 vCPUs and 2 TB
of RAM and one VM
with 64 vCPUs and 1
TB of RAM
```
```
Note: The VMs can't span nonadjacent nodes or more than two nodes.
```
```
Figure 2: Unsupported Configurations for SAP HANA on a Sub-NUMA Clustered Node
```
Consider the following requirements and limitations when designing for and working with
SNC-2 SAP HANA production VMs:

- SNC-2 requires symmetrically populated host memory.
- Only dual-socket Intel Sapphire Rapids or Intel Emerald Rapids hardware supports
    SNC-2; quad-socket hardware does not.
- You can't live-migrate VMs between SNC-2 enabled and SNC-2 disabled hosts.

```
Use affinity rules to avoid migrating VMs from an SNC-2 enabled host to a
SNC-2 disabled host.
```
- You must enable SNC-2 in the AHV host BIOS.
- The CVM is pinned to an SNC-2 NUMA node, which means that it only has half of the
    physical CPU's resources. With SNC-2, for example, a 20-core CPU only offers the
    CVM the resources of 10 cores, which can affect performance. Size accordingly to
    mitigate this risk.


### Live Migration for SAP HANA VMs..................................................................................................

```
AOS 6.8 supports live migration starting with AOS 6.8.1 or later 6.8.nn versions.
Note: Intel Emerald Rapids CPUs support live migration starting with AOS 7.3 or later 7.3.n versions.
```
```
Although Nutanix has conducted extensive tests to ensure full functionality while
live-migrating SAP HANA VMs, we can't guarantee a successful migration under all
circumstances due to the high active memory nature of SAP HANA as an in-memory
database. We therefore strongly recommend and only support using live migrations
for SAP HANA production instances in low-utilization scenarios or during maintenance
windows.
Nutanix conducted live migration tests using SAP HANA instances with 2 TB of memory
under 35 percent CPU load with a mixed load (ML4) test. This test mimics real-world
user activity with thousands of simulated users. In these tests, we successfully moved a
VM from one AHV host to another without impacting the stability or creating any errors in
the ML4 testbed.
Many factors contribute to a successful live migration, including available network
bandwidth. Nutanix SAP Engineering strongly recommends using 25 Gbps networking if
you plan to use live migration with any SAP HANA database VM. For more information,
see the Nutanix SAP solution page.
Note: To enable support for live migration, enable the VM metrics host daemon as described in the Creating
SAP HANA VMs section.
```
### Hardware and Hypervisor.................................................................................................................

```
SAP maintains the list of supported systems available from different OEMs on its
Certified HCI Solutions page. We recommend that you check this page regularly for
newly certified systems and OEMs. For your SAP HANA on Nutanix deployment to be
supported, you must select a system listed in the SAP HANA HCI Hardware directory.
When you choose and set up your hardware, follow the SAP HANA networking
recommendations described in SAP HANA Network Requirements to ensure the
availability of enough physical and virtual network interfaces.
```

```
Note: Separate HANA network traffic (for example, database access and HANA replication) from all other
types of traffic.
```
```
Nutanix recommends different configurations for your storage subsystem depending on
the use case. For production systems with low latency requirements and workloads that
are generally transactional, we recommend using SSDs with NVMe. Data warehouse
workloads and systems with less strict latency requirements can use all-flash (SSD only)
configurations. Hybrid disk (SSD and HDD) configurations aren't supported for SAP
HANA.
Note: Provide at least four SSD devices for all-flash configurations.
```
```
For systems with strict latency requirements, we recommend using network cards that
support Remote Direct Memory Access (RDMA) technology. When you select these
cards, ensure that your connecting Ethernet switches support the RDMA over Converged
Ethernet (RoCE) standard. You don't need RDMA networking to meet SAP HANA
storage performance requirements, but we recommend using RDMA whenever you have
strict latency requirements for your SAP HANA workloads.
Verify the best way to configure the hardware-specific BIOS settings to the equivalent of
"maximum performance" with your specific hardware vendor. These hardware settings
can significantly affect latency performance for the overall solution.
```
**AOS and AHV Hardware and Hypervisor Configuration Commands**

```
The optdedicatedapp toggle command changes several performance parameters for
AHV, including hypervisor hugepages, VM scheduling behavior, and performance- and
power-saving settings.
Run the following command on all AHV hosts and then reboot them for the changes to
take effect:
$ /srv/salt/statechange optdedicatedapp on
The intel_pstate performance scaling driver lets you configure different clock frequency
and voltage settings, which are referred to as p-states. For SAP HANA, we recommend
loading the Intel p-state driver and setting the operation mode to active. If you don't
load the Intel p-state driver, configure your BIOS settings according to the OEM
recommendations or contact support. You can confirm that you loaded the Intel p-state
driver by running the following command:
cpupower frequency-info
```

```
For SAP HANA, we recommend setting the performance bias value to 0. If you don't set
the performance bias value to zero, contact Nutanix Support. To check the performance
bias value, run the following command:
cpupower info
The previous settings are configured automatically on all supported AOS versions after
running the statechange command and rebooting the host.
Modern CPUs have various power-saving states. For SAP HANA workloads, we
recommend disabling the C1E and C6 c-states, which you can do at the BIOS or
hypervisor level. By default, Nutanix disables these states at the hypervisor level. If you
don't disable the C1E and C6 c-states, configure your BIOS settings according to the OEM
recommendations or contact Nutanix Support. You can confirm that you turned off these
c-states by running the following command on the AHV host:
cpupower idle-info
When you plan for n + 1, follow the steps in the Enabling High Availability for the Cluster
section of the Prism Element Web Console Guide to configure the cluster for high
availability. For more information, see Virtual Machine High Availability.
```
### SAP HANA VMs and Applications....................................................................................................

```
Note the following sizing limitations when creating SAP HANA production VMs:
```
- On dual-socket hardware with Intel Emerald Rapids CPUs:
    › An SAP HANA production VM can't have more than 128 vCPUs.
       This maximum assumes that the VM has hyperthreading enabled.
    › A VM can't have more than 2.0 TB of RAM.
- On dual-socket hardware with Intel Sapphire Rapids CPUs:
    › An SAP HANA production VM can't have more than 120 vCPUs.
       This maximum assumes that the VM has hyperthreading enabled.
    › A VM can't have more than 2.0 TB of RAM.


- On quad-socket hardware with Intel Sapphire Rapids CPUs:

```
› A VM can't have more than 360 vCPUs.
This maximum assumes that the VM has hyperthreading enabled.
› A VM can't have more than 6 TB of RAM.
```
- On dual-socket hardware with Intel Cascade Lake CPUs:

```
› An SAP HANA production VM can't have more than 56 vCPUs.
This maximum assumes that the VM has hyperthreading enabled.
› A VM can't have more than 1.5 TB of RAM.
```
- On quad-socket hardware with Intel Cascade Lake CPUs:

```
› A VM can't have more than 168 vCPU.
This maximum assumes that the VM has hyperthreading enabled.
› A VM can't have more than 4.5 TB of RAM.
Overhead varies depending on the hardware platform configuration. To
avoid VM startup issues, we recommend staying below 4,500 GB of RAM.
The overhead is specific to the system and VM configuration; it varies
across setups.
```
- Using UEFI boot: For the limitations on guest OS versions using UEFI boot, see
    Nutanix Compatibility and Interoperability Matrix: AHV Guest OS.

In addition to these sizing limitations, follow these guidelines when creating your VMs:

- Stay within NUMA boundaries for each VM's vCPU and memory configurations.

```
For more information about NUMA, see Nutanix AHV Best Practices.
```
- Supported operating systems for SAP HANA:

```
To view supported operating systems for SAP HANA, see SAP note 2235581:
SAP HANA: Supported Operating Systems (SAP account required).
```

- OS settings:

```
› When using SUSE SLES 12: Apply OS settings for SAP HANA inside the VM as
recommended in SAP note 2205917: SAP HANA DB: Recommended OS settings
for SLES 12 (SAP account required).
› When using SUSE SLES 15: Apply OS settings for SAP HANA inside the VM as
recommended in SAP note 2684254: SAP DB: Recommended OS settings for
SLES15 (SAP account required).
› When using Red Hat RHEL 7: Apply OS settings for SAP HANA inside the VM as
recommended in SAP note 2292690: SAP HANA DB: Recommended OS settings
for RHEL 7 (SAP account required).
› When using Red Hat RHEL 8: Apply OS settings for SAP HANA inside the VM as
recommended in SAP note 2777782: SAP HANA DB: Recommended OS Settings
for RHEL 8 (SAP account required).
› When using Red Hat RHEL 9: Apply OS settings for SAP HANA inside the VM as
recommended in SAP note 3108302: SAP HANA DB: Recommended OS Settings
for RHEL 9 (SAP account required).
```
- Use a minimum of four virtual hard disks for the database log and four virtual hard
    disks for the database data volume.
- Depending on your usage pattern, it might be beneficial to use a similar construct of
    multiple virtual hard disks for your other SAP HANA volume requirements.
- Use a supported file system as described in SAP note 405827: Journaled file system
    and raw devices on Linux (SAP account required).
- SAP fully supports the use of the Linux Logical Volume Manager (LVM), as described
    in SAP note 597415: Logical volume manager (LVM) on Linux (SAP account required).
    › Keep the disks for the data volume and the disk for the log volume in separate LVM
       volume groups.
    › When you create the logical volume, create a striped logical volume using all the
       physical volumes in the volume group.
- For disk space requirements for SAP HANA log, data, and shared volumes, see the
    SAP HANA Guide attached to SAP note 1900823.


```
The following command provides an efficient way to generate multiple virtual hard disks
from the command-line interface:
$ acli vm.disk_create <VM name> container=<storage container name>
create_size=<size of disk in GB>G bus=scsi
```
**Creating File Systems**

```
To create file systems, adapt the following steps to the requirements for your
configuration:
```
**1.** Create log and data volume groups for SAP HANA:
    $ vgcreate hanalog /dev/sd{b,c,d,e}
    $ vgcreate hanadata /dev/sd{f,g,h,i}
    $ vgcreate hanashared /dev/sdj
**Note:** The preceding code block is an example; replace the sample letters with those from your setup.
**2.** Create logical volumes for log and data striped across four virtual hard disks with 1 M
    stripe size and readahead=none:
       $ lvcreate -i <# of virtual disks for log> -I 1M -l 100%VG -r none -n vol
       hanalog
       $ lvcreate -i <# of virtual disks for data> -I 1M -l 100%VG -r none -n vol
       hanadata
       $ lvcreate -l 100%VG -r none –n vol hanashared
Use all logical extents of a volume group for the logical volumes.
**3.** Create XFS file systems on the log and data volumes:
    $ mkfs.xfs /dev/mapper/hanalog-vol
    $ mkfs.xfs /dev/mapper/hanadata-vol
    $ mkfs.xfs /dev/mapper/hanashared-vol
**4.** Create mount points /hana/log, /hana/data and /hana/shared:
    $ mkdir -p /hana/{log,data,shared}
**5.** When using XFS, add the following mount parameters to the relevant entries in /etc/
    fstab:
       $ inode64,largeio,swalloc 1 2
**6.** Mount the volumes.
    **Note:** The previous steps show one example of a possible configuration that you need to adjust to the actual
    requirements for your configuration.

**SAP HANA VM Creation**

```
When you create SAP HANA VMs, verify the following configuration changes:
```

- Define the VM CPU topology:

```
$ acli vm.update <vm_name> num_vcpus=<number of virtual sockets>
num_vnuma_nodes=<number of virtual numa nodes> num_cores_per_vcpu=<amount
of virtual cores> num_threads_per_core=<1 for no hyperthreading, 2 for
hyperthreading> vcpu_hard_pin=<True or False> machine_type=<VM machine
type>
Note: The following examples are based on a physical CPU with 28 cores (56 threads).
```
```
› Example 1: This command updates the settings for an SAP HANA VM with three
CPU sockets. Each vCPU has 28 cores and hyperthreading enabled. This update
results in a VM with three virtual sockets, three NUMA nodes, and a total of 168
vCPUs (including hyperthreads).
$ acli vm.update SAP-HANA num_vcpus=3 num_vnuma_nodes=3
num_cores_per_vcpu=28 num_threads_per_core=2 vcpu_hard_pin=True
machine_type=q35
› Example 2: This command updates the settings for an SAP HANA VM with two
CPU sockets. Each vCPU has 28 cores and hyperthreading enabled. This update
results in a VM with two virtual sockets, two NUMA nodes, and a total of 112 vCPUs
(including hyperthreads).
$ acli vm.update SAP-HANA num_vcpus=2 num_vnuma_nodes=2
num_cores_per_vcpu=28 num_threads_per_core=2 vcpu_hard_pin=True
machine_type=q35
› Example 3: This command updates the settings for an SAP HANA VM with one
CPU socket. Each vCPU has 28 cores and hyperthreading enabled. This update
results in a VM with one virtual socket, one NUMA node, and a total of 56 vCPUs
(including hyperthreads).
$ acli vm.update SAP-HANA num_vcpus=1 num_vnuma_nodes=1
num_cores_per_vcpu=28 num_threads_per_core=2 vcpu_hard_pin=True
machine_type=q35
```
- Enable the VM metrics host daemon as described in SAP note 2656072 (SAP account
    required):
       $ acli vm.update <vm_name> enable_metrics=True


- Set the machine type for the VM to **Q35** :
    $ acli vm.update <vm_name> machine_type=q35
**Note:** This machine type emulates the ICH9 host chipset.

```
Because some issues can arise from trying to use an IDE CD-ROM drive
attached to the VM, use a SATA CD-ROM drive:
$ vm.disk_create <vm_name> cdrom=true bus=sata
```
- Set extra flags as needed:
    › On AOS versions 6.8.nn and later, set the appropriate extra flags:
       acli vm.update <vm_name> extra_flags="disable_hyperv=true"
    › On AOS versions 7.3 and later, set the appropriate extra flags:
       acli vm.update <vm_name> extra_flags="disable_hyperv=true
       cpu_hotplug=false"
These flags turn off CPU hotplugging and enable the VM to better handle
virtual interrupt calls.

**SAP HANA VM Operating System Settings**

```
Add the following to the operating system kernel parameters configuration section, then
restart the VM for the changes to take effect:
skew_tick=1
skew_tick=1
Add this parameter as a kernel parameter to the SAP HANA VM. On modern Linux
systems, lock contention can occur when multiple CPUs request a kernel timer tick
handler simultaneously. Using the skew_tick=1 kernel parameter starts the ticks per
CPU at different times to avoid kernel lock contention. Reboot the VM for changes
to take effect.
Use halt polling to optimize the HANA VM's scheduling. Run the following commands in
the HANA VM after each restart:
modprobe cpuidle-haltpoll
POLL_NS=2400000
GROW_START=2400000
echo $POLL_NS > /sys/module/haltpoll/parameters/guest_halt_poll_ns
echo $GROW_START > /sys/module/haltpoll/parameters/guest_halt_poll_grow_start
```

```
Alternatively, write a systemd service script to automatically configure these settings on
start.
Note: The POLL_NS and GROW_START values are specific to CPU generations. The value of 2400000
applies to all currently supported CPUs.
```
```
Verify the status of the irqbalance service. When configured according to best practice
guidance from SUSE and Red Hat, the irqbalance service is disabled and has the status
inactive.
systemctl disable irqbalance.service
systemctl status irqbalance.service
For further guidance, see the relevant SUSE and Red Hat best practice guides.
```
**SAP HANA Database Recommendations**

- Review the SAP HANA Master Guide and the SAP HANA Server Installation and
    Update Guide.
- Always check the relevant SAP notes for updates before you install any SAP HANA–
    specific software.
- Change the current clock source to TSC (Time Stamp Counter) to ensure proper SAP
    HANA timer operation.
       You can set the clock source in different ways depending on your chosen
       guest OS (SUSE or Red Hat). Consult the OEM documentation for the
       necessary procedures to change this setting.
- Use SAP HANA System Replication to ensure application availability.

**Setting max_parallel_io_requests**

```
Nutanix SAP Engineering conducted extensive tests to find the optimal settings
for the SAP HANA persistent layer. We strongly recommend setting the
max_parallel_io_requests value to 256. To set this parameter, follow these steps:
```
**1.** Navigate to **Configuration and Monitoring** in SAP HANA Studio.
**2.** Click **Open Administration** and select **global.ini** , then **fileio**.
**3.** For max_parallel_io_requests, change the setting to **256**.
Alternatively, you can set this parameter with the following hdbsql command:


$ <sidadm>@<hostname>:/usr/sap/<SID>/HDB00> hdbsql -n <hostname> -i 00 -user
<system user> -password <password>

$ hdbsql QO1=> ALTER SYSTEM ALTER CONFIGURATION ('global.ini', 'SYSTEM') SET
('fileio','max_parallel_io_requests') = '256' WITH RECONFIGURE;


## 3. Conclusion..............................................................................................

```
When you choose Nutanix for your SAP HANA implementation, you benefit from reduced
complexity and improved agility. Following the recommendations provided in this
document can help ensure the successful implementation and operation of SAP HANA
on Nutanix.
Nutanix AOS software is certified for production SAP HANA deployments, and you can
choose between Nutanix AHV and VMware ESXi as the hypervisor. Customers can
use either Linux enterprise distribution (SUSE or Red Hat) as the guest OS to run SAP
HANA.
If you have questions regarding this document, visit the Nutanix SAP solution page.
For SAP support information and verification, see SAP note 2686722: SAP HANA
virtualized on Nutanix AOS (SAP account required).
```

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
Figure 1: SAP HANA on a Sub-NUMA Clustered Node..............................................................................13
Figure 2: Unsupported Configurations for SAP HANA on a Sub-NUMA Clustered Node............................14
```


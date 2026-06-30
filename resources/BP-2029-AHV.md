# **Nutanix AHV Best Practices** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Code samples and snippets that appear in this content are unofficial, are unsupported, and will require extensive modification before use in a production environment. As such, the code samples and snippets are provided AS IS and are not guaranteed to be complete, accurate, or up-to-date. Nutanix makes no representations or warranties of any kind, express or implied, as to the operation or content of the code samples or snippet. Nutanix expressly disclaims all other guarantees, warranties, conditions and representations of any kind, either express or implied, and whether arising under any statute, law, commercial use or otherwise, including implied warranties of merchantability, fitness for a particular purpose, title and non-infringement therein. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Nutanix AHV Best Practices 

## **Contents** 

**1. Executive Summary.................................................................................5 2. Nutanix AHV Networking........................................................................ 8** Nutanix AHV Ports and Bonded Ports............................................................................................... 9 Nutanix AHV Virtual Local Area Networks....................................................................................... 10 Nutanix AHV IP Address Management............................................................................................ 12 **3. Nutanix AHV Virtual Machine High Availability...................................14 4. Nutanix Acropolis Dynamic Scheduler................................................16 5. Nutanix AHV VM Deployment...............................................................18** New VM with New OS Installation....................................................................................................19 New VM Cloned from Existing AHV VM or VM Snapshot............................................................... 20 New VM Imported from Disk Image or Non-AHV Cluster................................................................ 21 Guest Customization.........................................................................................................................21 **6. Nutanix Guest Tools.............................................................................. 23 7. Nutanix AHV VM Data Protection.........................................................25 8. Nutanix Hypervisor Mobility and Conversion.....................................27 9. Nutanix AHV Live Migration................................................................. 30 10. Nutanix AHV CPU Configuration........................................................32 11. Nutanix AHV Memory Configuration..................................................34** Nonuniform Memory Access and Guest VMs.................................................................................. 35 

Virtual Nonuniform Memory Access and Guest VMs....................................................................... 37 Memory Overcommit.........................................................................................................................39 **12. AHV Turbo Technology....................................................................... 43 13. Nutanix AHV VM Disk Configuration................................................. 46** SCSI UNMAP for Windows.............................................................................................................. 47 SCSI UNMAP for Linux.................................................................................................................... 48 

**14. Nutanix AHV Resource Oversubscription.........................................49** CPU and Memory Oversubscription................................................................................................. 50 **15. Nutanix AHV Additional References..................................................51** Command Examples.........................................................................................................................54 **About Nutanix.............................................................................................55 List of Figures.............................................................................................................................................56** 

Nutanix AHV Best Practices 

## 1. Executive Summary 

The Nutanix Cloud Platform solution is compute agnostic, providing the flexibility to choose the hypervisor and cloud service combination that suits your needs today and the freedom to move workloads on demand. Nutanix AHV, the native hypervisor for the Nutanix solution, lets you unbox a Nutanix system and immediately start loading VMs with enterprise-class virtualization capabilities at no extra cost. 

This document presents an overview of AHV features and offers best practices for integrating these features into a production datacenter. We address virtualization topics including VM deployment, CPU configuration, oversubscription, high availability, data protection, and live migration. We also make recommendations for networking, including VLANs and segmentation, load balancing, and IP address management (IPAM). This document prepares you to design and deploy a VM environment using AHV. It also prepares you to customize AHV's features for optimal performance and to make full use of its extensive capabilities. 

With AHV, Nutanix adapted proven open-source technology and wrapped it in the easy-to-use and efficient Nutanix Prism interface. Underneath Nutanix Prism, Nutanix configured the hypervisor to take advantage of the seamless scale and reliability of AOS Storage. The remaining user decisions include points like host networking and VM-level features that sometimes require you to make a choice. This document is the guide to those choices. 

## _Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|November 2015|Original publication.|
|2.0|August 2016|Updated for AOS 4.6.|
|3.0|January 2017|Updated for AOS 5.0.|
|3.1|August 2017|Updated for AOS 5.1.|
|4.0|December 2017|Updated for AOS 5.5.|
|4.1|May 2018|Updated the NUMA and|
|||vNUMA sections.|



© 2026 Nutanix, Inc. All rights reserved  | **5** 

Nutanix AHV Best Practices 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|4.2<br>4.3<br>4.4<br>4.5<br>4.6<br>4.7<br>4.8<br>4.9<br>5.0<br>5.1|October 2018<br>Updated product naming and<br>the Disks section.<br>December 2018<br>Updated the VM Data<br>Protection section.<br>September 2019<br>Updated the AHV Turbo<br>Technology section.<br>October 2019<br>Updated the Virtual Machine<br>High Availability and VM Data<br>Protection sections.<br>February 2020<br>Updated CPU<br>recommendations.<br>June 2020<br>Updated the Nutanix overview,<br>jumbo frames guidance, and<br>Virtual Nonuniform Memory<br>Access and Guest VMs<br>section.<br>April 2022<br>Updated the Memory<br>Oversubscription section<br>and added the Memory<br>Overcommit section.<br>July 2022<br>Updated the Virtual Machine<br>High Availability, Affinity<br>Policies, and AHV Best<br>Practices Checklist sections.<br>September 2023<br>Updated the Networking,<br>Acropolis Dynamic Scheduler,<br>VM Deployment, Hypervisor<br>Mobility and Conversion, Live<br>Migration, CPU Configuration,<br>Memory, and Appendix<br>sections.<br>March 2024<br>Updated the Executive<br>Summary, Nutanix AHV<br>Networking, and Acropolis<br>Dynamic Scheduler sections.|



© 2026 Nutanix, Inc. All rights reserved  | **6** 

Nutanix AHV Best Practices 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|5.2<br>5.3<br>5.4<br>5.5<br>5.6<br>5.7<br>5.8<br>5.9<br>6.0<br>6.1|June 2024<br>Updated the Memory<br>Configuration section.<br>July 2024<br>Updated the Memory<br>Overcommit section.<br>August 2024<br>Updated the CPU<br>Oversubscription section.<br>January 2025<br>Updated the`vm.update`<br>command in the Memory<br>Overcommit section.<br>January 2025<br>Updated the Affinity Policies<br>section.<br>February 2025<br>Updated the limitations in the<br>Memory Overcommit section.<br>May 2025<br>Updated the Hugepages and<br>Nutanix AHV Best Practices<br>Checklist sections.<br>July 2025<br>Updated the Guest<br>Customization and Resource<br>Oversubscription sections.<br>September 2025<br>Updated document structure.<br>May 2026<br>Updated the Command<br>Examples section.|



© 2026 Nutanix, Inc. All rights reserved  | **7** 

Nutanix AHV Best Practices 

## 2. Nutanix AHV Networking 

Nutanix AHV uses Open vSwitch (OVS) to connect the Controller virtual machine (CVM), the hypervisor, and guest VMs to each other and to the physical network. The OVS service, which starts automatically, runs on each AHV node. 

OVS is an open source software switch implemented in the Linux kernel and designed to work in a multiserver virtualization environment. By default, OVS behaves like a layer 2 learning switch that maintains a MAC address table. The hypervisor host and VMs connect to virtual ports on the switch. 

OVS supports many popular switch features, including virtual local area network (VLAN) tagging, Link Aggregation Control Protocol (LACP), port mirroring, and quality of service. Each AHV server maintains an OVS instance, and all OVS instances combine to form a single logical switch. Constructs called bridges manage the switch instances residing on the AHV hosts. 

Bridges act as virtual switches to manage network traffic between physical and virtual network interfaces. The default AHV configuration includes an OVS bridge called br0 and a native Linux bridge called virbr0. The virbr0 Linux bridge carries management and storage communication between the CVM and AHV host. All other storage, host, and VM network traffic flows through the br0 OVS bridge. The AHV host, VMs, and physical interfaces use ports for connectivity to the bridge. 

The Nutanix CVM uses the standard Ethernet maximum transmission unit (MTU) of 1,500 bytes for all network interfaces by default. The standard 1,500-byte MTU delivers excellent performance and stability. Nutanix doesn't support configuring the MTU on a CVM's network interfaces to higher values. You can enable jumbo frames (MTU of 9,000 bytes) on the physical network interfaces of AHV, ESXi, or Hyper-V hosts and guest VMs if the applications on your guest VMs require them. If you choose to use jumbo frames on hypervisor hosts, enable them end-to-end in the desired network and consider both the physical and virtual network infrastructure affected by the change. 

© 2026 Nutanix, Inc. All rights reserved  | **8** 

Nutanix AHV Best Practices 

## **Nutanix AHV Ports and Bonded Ports** 

Ports are logical constructs created in a bridge that represent connectivity to the virtual switch. Nutanix uses several port types, including the following: 

- An internal port—with the same name as the default bridge (br0)—provides access for the AHV host. 

- Tap ports act as bridge connections for virtual network interface controllers (NICs) presented to VMs. 

- AOS uses VXLAN ports for IP address management (IPAM). 

- Bonded ports provide NIC teaming for the physical interfaces of the AHV host. 

Bonded ports aggregate the physical interfaces on the AHV host. By default, a bond named br0-up is created in bridge br0. The Foundation node imaging process places all interfaces in a single bond as required. Changes to the default bond, br0-up, often rename it to bond0. We recommend using the name br0-up to quickly identify the interface as the bridge br0 uplink. 

OVS bonds allow several load-balancing modes, including active-backup, balanceslb, and balance-tcp. You can also activate LACP for a bond. The bond_mode setting isn't specified during installation and therefore defaults to active-backup, which is the recommended configuration. For more information on bonds, see the Nutanix AHV Networking best practice guide. 

The following diagram illustrates the networking configuration described previously for a single host immediately after imaging. We recommend this configuration if you're using at least two NICs of the same speed (for example, 10 Gbps) and the remaining NICs are disconnected. 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

Nutanix AHV Best Practices 

Figure 1: Post-Foundation Network State 

**Note:** Only use NICs of the same speed in the same bond. 

We recommend using only the 10 Gbps NICs and leaving the 1 Gbps NICs unplugged if you don't need them. 

## **Nutanix AHV Virtual Local Area Networks** 

AHV supports VLANs for the CVM, AHV host, and guest VMs. For information on assigning VLANs to the AHV host and CVM, see Nutanix AHV Networking best practice 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

Nutanix AHV Best Practices 

guide. For guest VMs, you can create and manage the networks that a virtual NIC uses in Nutanix Prism or by using the Acropolis command-line interface (aCLI) or REST. 

Each virtual network in AHV maps to a single VLAN and bridge. You must create each VLAN and virtual network created in AHV on the physical top-of-rack switches as well, but integration between AHV and the physical switch can automate this provisioning. When you create a network in Nutanix Prism, assign a memorable network name and VLAN ID. 

Although a virtual network is configured for a specific VLAN, you can configure a virtual NIC in either access or trunked mode. By default, all virtual NICs are created in access mode, which allows a single configured VLAN based on the virtual network. For information on configuring virtual NICs in trunked mode, see the Nutanix AHV Networking best practice guide. 

We recommended placing the CVM and AHV in the default untagged (or native) VLAN, as shown in the following figure. 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

Nutanix AHV Best Practices 

Figure 2: Default Untagged VLAN for CVM and AHV Host 

To avoid sending untagged traffic to the CVM and AHV, or if the security policy doesn't allow you to do so, see the Nutanix AHV Networking best practice guide. 

## **Nutanix AHV IP Address Management** 

In addition to network creation and VLAN management, AHV also supports IPAM. With IPAM, AHV can assign IP addresses automatically to VMs using the Dynamic Host 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

Nutanix AHV Best Practices 

Configuration Protocol (DHCP). You can configure each virtual network and associated VLAN with a specific IP subnet, associated domain settings, and group of IP address pools available for assignment. AOS uses VXLAN and OpenFlow rules in OVS to intercept DHCP requests from guest VMs so that the configured IP address pools and settings are used. 

You can use AOS with IPAM to deliver a complete virtualization deployment, including network management, from the unified Nutanix Prism interface. This capability radically simplifies the traditionally complex network management associated with provisioning VMs and assigning network addresses. To avoid address overlap, work with your network team to reserve a range of addresses for VMs before enabling the IPAM feature. 

**Note:** The AOS leader assigns an IP address from the address pool when creating a managed VM NIC; the address returns to the pool when you delete the VM NIC or VM. 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

Nutanix AHV Best Practices 

## 3. Nutanix AHV Virtual Machine High Availability 

Virtual machine high availability (VMHA) ensures that VMs restart on another AHV host in the cluster if a host fails. VMHA considers RAM when calculating available resources throughout the cluster for starting VMs. 

VMHA respects affinity and antiaffinity rules. For example, with VM-host affinity rules, VMHA doesn't start a VM pinned to AHV host 1 and host 2 on another host when those two hosts are down unless the affinity rule specifies an alternate host. Choose from one of two VMHA modes: 

## **Default** 

The Default mode requires no configuration and is included by default when you install an AHV-based Nutanix cluster. When an AHV host becomes unavailable, the VMs that were running on the failed AHV host restart on the remaining hosts, depending on the available resources. If the remaining hosts don't have sufficient resources, some of the failed VMs might not restart. 

## **Guarantee** 

The nondefault Guarantee mode configuration reserves space throughout the AHV hosts in the cluster to guarantee that all VMs can restart on other hosts in the AHV cluster during a host failure. To enable Guarantee mode, open the Manage VM High Availability pane in Nutanix Prism and select the **Enable HA Reservation** checkbox. A message displays the amount of memory reserved and how many AHV host failures the system can tolerate. 

The VMHA configuration reserves resources to protect against the following scenarios: 

- One AHV host failure, if all Nutanix containers are configured with a replication factor of 2 

- Two AHV host failures, if any Nutanix container is configured with a replication factor of 3 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

Nutanix AHV Best Practices 

You can use the aCLI to manage protection against two AHV host failures when using replication factor 2. Use the following command to designate the maximum number of tolerable AHV host failures: 

```
nutanix@CVM$ acli ha.update num_host_failures_to_tolerate=X
```

When an unavailable AHV host comes back online after a VMHA event, VMs that were running there migrate back to their original host to maintain data locality. 

Follow these best practices for VM high availability: 

- Use the nondefault VMHA Guarantee mode to ensure that all VMs can restart in case of an AHV host failure. 

- When using Guarantee mode, keep the default reservation type of kAcropolisHAReserveSegments; don't alter this setting. 

**Note:** The VMHA reservation type kAcropolisHAReserveHosts is deprecated. Never change the VMHA reservation type to kAcropolisHAReserveHosts. 

- Consider storage availability requirements when using VMHA Guarantee mode. 

- Ensure that the parameter `num_host_failures_to_tolerate` isn’t higher than the configured storage availability. 

With only two copies of the VM data, the VM data might become unavailable if two hosts are down at the same time, even with enough CPU and RAM resources to run the VMs. 

- Disable VMHA for VMs that are pinned to one AHV host using VM-host affinity. 

You must complete this step before you can pin a VM to one AHV host using Acropolis Dynamic Scheduler (ADS). We don't recommend this configuration, as we discuss in the following section. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

Nutanix AHV Best Practices 

## 4. Nutanix Acropolis Dynamic Scheduler 

ADS ensures that compute and storage resources are available for VMs and volume groups (VGs) in the Nutanix cluster. ADS, enabled by default, uses real-time statistics to determine the following configurations: 

- Initial placement of VMs and VGs, specifically which AHV host runs a particular VM when it starts or a particular VG after creation 

- Required runtime optimizations, including moving particular VMs and VGs to other AHV hosts to give all workloads the best possible access to resources 

ADS doesn't migrate VMs with CPU passthrough enabled or VMs classified as agent VMs, which are typically appliances providing a specific feature such as network or backup and restore capabilities. All these VM types bind to a specific AHV host. They turn off during a host upgrade and automatically start with the AHV host. 

Affinity policies are rules that you define one of two ways: manually, by a Nutanix administrator, or with a VM provisioning workflow. The following affinity policies are available: 

## **VM-host affinity** 

This Prism Central configuration keeps a VM on a specific set of AHV hosts based on categories. Use it when you must limit VMs to a subset of available AHV hosts due to application licensing, AHV host resources (such as available CPU cores or CPU gigahertz speed), available RAM or RAM speed, or local SSD capacity. Host affinity is a _must_ rule; AHV always honors the specified rule. 

## **VM-VM antiaffinity** 

This Prism Central policy ensures that two or more VMs don't run on the same AHV host. Use it when an application provides high availability and an AHV host can't be that application's single point of failure. Antiaffinity is a _should_ rule that only takes effect when enough resources are available to run VMs on separate hosts. 

Although you can pin a VM to a specific AHV host using the VM-host affinity rule, we don't recommend doing so; this configuration introduces additional operational procedures and might cause the following issues: 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

Nutanix AHV Best Practices 

- The AHV upgrade process might fail and require manual intervention. 

- High availability might enter a critical state, preventing other VM operations. 

To resolve any adverse effects that might result from pinning a VM to a single host using a VM-host affinity rule, perform one of the following actions: 

- Remove the VM-host affinity rule. 

- Add an AHV host to the VM-host affinity rule. 

Follow these recommendations for using ADS: 

- To ensure VMHA functionality on a VM, use VM-host affinity to pin the VM to at least two AHV hosts if VMHA tolerates one AHV host failure or to at least three AHV hosts if VMHA tolerates two AHV host failures. 

- Don't use VM-host affinity rules to pin a VM to only one AHV host. 

- To guarantee that all VMs in an ADS VM-VM antiaffinity group run on different AHV hosts, limit the maximum number of VMs in the group to the number of AHV hosts in the cluster. 

If VMs must not run on the same hosts during a VMHA event, the number of VMs per VM-VM antiaffinity group must equal the number of AHV hosts minus one or two, depending on the VMHA failure configuration value for num_host_failures_to_tolerate. This configuration ensures that all VMs run on different AHV hosts at all times, even during failure scenarios. 

- Disable ADS only if you require strict control without automatic ADS rebalancing. Keep the following considerations in mind: 

   - › You can disable ADS rebalancing across the cluster using the aCLI, but not on a per-VM basis. 

   - › When you disable ADS, automatic hotspot remediation and antiaffinity violation correction operations don't occur. 

   - › Running the command `nutanix@CVM$ acli ads.update enable=false` turns off ADS. 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

Nutanix AHV Best Practices 

## 5. Nutanix AHV VM Deployment 

Choose from the following options to create VMs in AHV: 

- Create an empty VM and install the operating system and needed applications. 

- Create a VM from an existing VM using the clone feature. 

- Create a VM from an existing VM snapshot using the clone feature. 

- Import a VM from a VM disk image using the image service or network share. 

- Import a VM using cross-hypervisor disaster recovery or hypervisor conversion. 

The following table summarizes the driver requirements for the different VM deployment options. 

_Table: VM Driver Requirements_ 

|**AHV VM Operation**|**VirtIO Required**|**NGT Required**|**Additional**|
|---|---|---|---|
||||**Information**|
|Create New VM:|Y|N|VirtIO driver|
|Windows|||installation is required.|
||||The Nutanix Guest|
||||Tools (NGT) software|
||||is optional.|
|Create New VM: Linux|Y|N|VirtIO support in|
||||the Linux kernel is|
||||required and enabled|
||||for all AHV-supported|
||||Linux distributions|
||||by default. The NGT|
||||software is optional.|



© 2026 Nutanix, Inc. All rights reserved  | **18** 

Nutanix AHV Best Practices 

|**AHV VM Operation**|**VirtIO Required**|**NGT Required**|**Additional**|
|---|---|---|---|
||||**Information**|
|External Import via|Y|N|VirtIO is required|
|Image Service or|||in the image (for|
|Copied to Active|||example, VMDK,|
|Directory Federation|||qcow) uploaded to|
|Service (ADFS):|||the image service|
|Windows|||or copied to ADFS.|
||||The NGT software is|
||||optional.|
|External Import via|Y|N|VirtIO support in|
|Image Service or|||the Linux kernel is|
|Copied to ADFS: Linux|||required for the image|
||||uploaded to image|
||||service or copied|
||||to ADFS. All AHV-|
||||supported Linux|
||||distributions have|
||||VirtIO enabled by|
||||default. The NGT|
||||software is optional.|
|Cross-Hypervisor|Y|Y|VirtIO and NGT are|
|DR or Conversion:|||required.|
|Windows||||
|Cross-Hypervisor DR|Y|Y|VirtIO support in the|
|or Conversion: Linux|||VM Linux kernel and|
||||NGT are required.|



## **New VM with New OS Installation** 

When creating a new Windows VM on AHV, use the Nutanix VirtIO installation ISO to install the correct network and disk drivers in the guest VM. Windows VMs require these drivers. The Nutanix Support Portal maintains complete installation instructions. The NGT software is optional for newly installed Windows VMs; install NGT only to use the features discussed in the NGT section. 

Ensure that new Linux guest VMs contain the virtio_scsi kernel driver when installed on AHV. Each of the supported Linux guest VMs listed on the Compatibility and 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

Nutanix AHV Best Practices 

Interoperability Matrix: AHV Guest OS page contains these drivers. Without the virtio_scsi disk drivers, Linux guests can only use the IDE or PCI disk formats. As with Windows VMs, only install NGT if you need it. 

## **New VM Cloned from Existing AHV VM or VM Snapshot** 

When cloning VMs, Nutanix recommends starting with an existing VM that is turned off. When cloning from turned-off VMs, the only limit to the number of clones you can create is the amount of available resources. Select a name for the VM that's easy to find in a search, such as base, original, or template. For maximum performance when making more than 10 clones of a turned-on VM, use the snapshot method described in the next section instead of cloning from the VM directly. 

Optimize the template VM to include only the features and applications that are required in the resulting cloned VMs before performing the clone operation. 

For clones of Windows guest VMs, install the Nutanix VirtIO drivers in the original VM before performing the clone operation. 

To clone a VM from an existing VM in the Nutanix Prism web console, select the desired VM and click **Clone** . To clone a VM using the aCLI, run the command `vm.clone <new_vm>` 

`clone_from_vm=<vm_name>` . 

Cloning from a snapshot of an existing VM is similar to cloning from the VM itself—the base is simply from an earlier point. You can create snapshots of VMs from the Nutanix Prism web console or through the aCLI. Use a recognizable name for the snapshot and follow the previous recommendations for optimizing your base image. 

To clone a VM from an existing snapshot in the Nutanix Prism web console, navigate to the VM Snapshots tab of the desired VM and select a snapshot. To clone from a snapshot using the aCLI, run the command `vm.clone <new_vm>` 

## `clone_from_snapshot=<snapshot_name>` . 

We recommend cloning from a VM snapshot instead of from a VM when cloning more than 10 VMs at a time because cloning from a VM snapshot is more efficient. The following example creates 20 clones from a single snapshot, numbered 01 through 20. 

```
vm.clone clone-vm-[01..20] clone_from_snapshot=base-vm
```

© 2026 Nutanix, Inc. All rights reserved  | **20** 

Nutanix AHV Best Practices 

## **New VM Imported from Disk Image or Non-AHV Cluster** 

To create a VM from a disk image that was imported from the image service or network share transfer, follow these rules before exporting the disk image from the source system: 

- Install the Nutanix VirtIO package in Windows VMs. You can install the optional NGT package, which contains VirtIO drivers, if you need NGT features. 

- Install the NGT package in Linux VMs. The NGT package ensures that the appropriate kernel drivers are enabled for the first boot into AHV using dracut.conf. 

**Note:** From AOS 5.5 onward, if you deploy Prism Central, you must use it for image management. 

Before importing Windows or Linux VMs from a non-AHV cluster using cross-hypervisor disaster recovery or hypervisor conversion, install the NGT package using the instructions in the Nutanix Cross Hypervisor Disaster Recovery section of the Data Protection and Recovery with Prism Element guide. The NGT package provides support for cross-hypervisor disaster recovery and contains the VirtIO drivers required for the VM to function properly in AHV. 

## **Guest Customization** 

You can also customize the operating system deployment in conjunction with Sysprep for Windows environments or cloud-init for Linux environments. With Nutanix Prism or aCLI custom scripts, you can add a user, rename a host, join a domain, or inject a driver. Specify the scripts by using an existing file stored on the cluster, uploading a new file, or pasting the script directly into Nutanix Prism. You can insert additional customization files into the VM during deployment using Nutanix Prism or the REST API. 

In Windows environments using Sysprep, you can add customizations when cloning a VM, creating a new VM from an existing file, or creating a new VM from installation media. Sysprep uses an XML-based answer file to manage customization. When cloning a VM from an existing file, specify an `unattend.xml` file and format. When deploying a VM during a new installation, use the `autounattend.xml` file and format. 

In Linux environments using cloud-init, you can add customizations when cloning a VM or when creating a new VM from an existing file. Cloud-init doesn't support customization 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

Nutanix AHV Best Practices 

when creating VMs from installation media. AOS with cloud-init can support several script formats, as detailed in cloud-init documentation. 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

Nutanix AHV Best Practices 

## 6. Nutanix Guest Tools 

NGT is a set of software features installed in a VM and a Nutanix CVM. A Nutanix Guest Agent publishes information about the VM to the Nutanix cluster, such as guest operating system type, VM mobility status, and the Volume Shadow Copy Service (VSS). The Nutanix Guest Agent is installed in both Windows and Linux guests. 

## **Nutanix VSS hardware provider** 

The Nutanix VSS hardware provider enables integration with native Nutanix data protection. The provider allows for application-consistent, VSS-based snapshots when using Nutanix protection domains and is supported with Windows guests. 

## **Self-Service Restore feature** 

The Self-Service Restore feature enables Nutanix administrators or VM owners to mount snapshots directly to the VM they were taken from. This capability allows users to select specific files from a snapshot. 

The Self-Service Restore GUI and CLI are installed in Windows guests. The NGT CLI (ngtcli) runs the Self-Service Restore CLI. To access the Self-Service Restore GUI, open a browser, navigate to `http://localhost:5000` , and sign in with machine administrator credentials. 

## **Nutanix VirtIO drivers** 

NGT installs Nutanix VirtIO drivers and provides the registry configuration necessary to support the migration or conversion of VMs bidirectionally between AHV and ESXi. AOS uses these drivers when performing Nutanix VM Mobility or Nutanix cluster conversion. The required drivers are installed in Windows guests. 

NGT requires network connectivity between the Nutanix cluster and the guest VMs. NGT also requires an empty IDE CD-ROM slot in the guest for attaching the ISO image. 

For NGT communication with the Nutanix cluster, configure the Nutanix cluster with a virtual IP address and open port 2074 between the required VMs and the CVMs. 

You can enable NGT either from Nutanix Prism (VM page, table view) or with the Nutanix CLI (nCLI) on a VM-by-VM basis. When first enabled, NGT mounts an ISO to the 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

Nutanix AHV Best Practices 

specified VM. You can then run the NGT installer from inside the VM. After installation, the installer package ejects the CD-ROM and enables NGT for the VM. 

VSS snapshot functionality is enabled by default, while Self-Service Restore is disabled by default. Use Nutanix Prism to view or modify services or control ISO mount state by selecting the target VM and choosing **Manage Guest Tools** . You can also use the `ngt` command in the nCLI. The following example shows how to query the NGT state for a specific VM: 

```
nutanix@CVM$ ncli
ncli> ngt get vm-id=89bca6ab-e995-4de7-b094-a7c9d6f62b59
VM Id                     : 000524ac-81af-5cb8-0000-000000005240::89bca6ab-
e995-4de7-b094-a7c9d6f62b59
VM Name                   : VMCC
NGT Enabled               : true
Tools ISO Mounted         : false
Vss Snapshot              : true
File Level Restore        : false
Communication Link Active : true
```

Only install NGT in guests as required for the following use cases: 

- For VMs that require Self-Service Restore 

- For VMs that require VSS 

- Before performing a cluster conversion 

- When using Nutanix VM Mobility 

If none of these conditions apply, use the Nutanix VirtIO driver standalone installation package instead of NGT. 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

Nutanix AHV Best Practices 

## 7. Nutanix AHV VM Data Protection 

Quickly recover VM data through one of the built-in methods available in AHV: 

- On-demand VM instant recovery 

- Protection domain VM recovery 

Both options use the Nutanix cluster built-in snapshot functionality. On-demand VM instant recovery offers a way to create local snapshots or checkpoints local to the Nutanix cluster as needed. The on-demand option for VM snapshots is available in the VM section of Nutanix Prism or through the aCLI. With this option, you can take and restore VM snapshots and create and clone new VMs. This process is helpful when taking an on-demand snapshot before starting potentially sensitive administrative tasks. 

A protection domain is an administrator-defined set of VMs or VGs scheduled for local Nutanix cluster instant recovery. The protection domain option allows you to create VM or VG snapshots on a schedule and store them locally on the Nutanix cluster where the VM runs (using the protection domain asynchronous disaster recovery option) or on a remote site, such as a separate Nutanix cluster or a public cloud target, including Amazon Web Services (AWS) and Azure. When network-naming conventions at the local and remote sites differ, AOS allows you to change the VM network name and VLAN ID so that your local VM can restart on the remote site in a valid network. For example, you can configure a VM running on AHV network VLAN-X at the local site to start on network VLAN-Y at the remote site. For IPAM-enabled networks, remote sites don't share DHCP reservations, so plan for VMs to receive new addresses. The mapping section in a protection domain includes VStore or container names and network names to ensure that the system replicates VMs to the correct containers on the remote Nutanix cluster. 

To create a schedule for the snapshots, choose the add schedule option in the protection domain settings. You can use the protection domain option to provide a true backup and restore solution, which requires you to send the local Nutanix snapshots to a remote site. A VM can be part of only one protection domain and this protection domain can be replicated to one or more remote sites. Because a protection domain can replicate to remote Nutanix clusters, its name must be unique across the system. 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

Nutanix AHV Best Practices 

Both options provide a way to return rapidly to an exact time and state for both VMs and Nutanix VGs. 

Snapshots are currently available for restore only through the same method used for snapshot creation. In addition to the built-in Nutanix features, you can use third-party backup and restore solutions that include options to back up VMs through AHV or by using a VM-installed client to export VM data from the Nutanix cluster. 

Follow these recommendations for VM data protection: 

- Use snapshots or on-demand data protection for day-to-day checkpoints and snapshots (such as before making VM changes). This snapshot restore operation is simpler and faster than the scheduled snapshot option. 

- Use protection domains or scheduled data protection for local snapshots that need to run on a schedule or for offsite protection. 

- When replicating data to a remote site, use the following naming convention for easier protection domain identification: `<Local-Site>_<Remote-Site>_PD#` (or function)—for example, `SiteB_SiteA_PD1` . 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

Nutanix AHV Best Practices 

## 8. Nutanix Hypervisor Mobility and Conversion 

AOS includes core capabilities that enhance VM mobility and cluster conversion between different hypervisors—specifically, the bidirectional movement of VMs between Nutanix clusters running AHV and ESXi, called Nutanix VM Mobility, or the conversion of a Nutanix cluster from ESXi to AHV or from AHV to ESXi. 

Nutanix VM Mobility simplifies the process of replicating Nutanix-based VMs between different hypervisors. VM Mobility supports replicating Windows and Linux-based VMs bidirectionally between a Nutanix ESXi cluster and a Nutanix AHV cluster. 

Nutanix VM Mobility uses native Nutanix snapshots to replicate data between clusters, and NGT installs the appropriate drivers and communicates with the cluster to confirm mobility support. The mobility process is straightforward and includes the following highlevel steps: 

**1.** Enable and install NGT for the VMs to be replicated. 

**2.** Create a remote site in each Nutanix cluster. 

This step includes network mapping between the AHV and ESXi networks. 

**3.** Create an asynchronous protection domain with schedules to replicate the required VMs. 

**4.** Move or create VMs using one of the following workflow options: 

   - For planned mobility, all VMs in a protection domain move using the migrate option, which unregisters the VMs from the source cluster, replicates any incremental changes since the last snapshot, and registers the VMs in the target cluster. 

   - For unplanned mobility, where a source cluster is offline, the activate option moves all VMs in a protection domain. Activation registers the VMs in a protection domain in the target cluster using the most recently available local snapshot. 

   - For individual VMs, a snapshot restore to a new location operates as a clone and registers the VM in the target cluster, which enables test and development scenarios and allows you to target individual VMs in a protection domain. 

Requirements and limitations: 

© 2026 Nutanix, Inc. All rights reserved  | **27** 

Nutanix AHV Best Practices 

- You must install NGT so that the appropriate drivers are available in the VM. This one-time operation enables communication between the CVM and Nutanix cluster to validate driver installation. 

- ESXi delta disks aren't supported. 

- VMs with SATA or PCI devices aren't supported. 

Nutanix Prism sends an alert if specific VMs can't be converted because of delta disks or virtual hardware. These recovery details are in the VM Recovery column of a local or remote snapshot. Also, because you must install Nutanix VirtIO drivers in a VM targeted for conversion, Nutanix Prism gives a warning if the installation status is unknown. 

You can convert a Nutanix cluster from ESXi to AHV. You can also convert a cluster from AHV to ESXi if it was previously converted from ESXi. Existing data, hypervisor networking settings, and VM settings are preserved during the cluster conversion process. 

For more information on in-place hypervisor conversion, see the Prism Element Web Console Guide. 

Follow these recommendations for Nutanix hypervisor mobility and conversion: 

- Use Nutanix VM Mobility for simplified migration between Nutanix clusters that use different hypervisors. 

- Use snapshot restores to new locations when using VM Mobility to create VMs without affecting protection domain replication schedules. 

- For additional automation controls during migration, use the Disaster Recovery Runbook feature to add advanced functionality such as IP address control, automated scripts, and failover orchestration to base VM Mobility–based replication. 

- Use Nutanix cluster conversion to use the same hardware while converting to a different hypervisor. 

- Use DHCP where possible to simplify network connectivity for converted VMs. 

- Nutanix Move is available for migrating VMs between clusters and to and from other public or private clouds. 

© 2026 Nutanix, Inc. All rights reserved  | **28** 

Nutanix AHV Best Practices 

- For more information, see the Migrating VMs to Nutanix AHV tech note and the Prism Web Console Guide. 

© 2026 Nutanix, Inc. All rights reserved  | **29** 

Nutanix AHV Best Practices 

## 9. Nutanix AHV Live Migration 

Live migration lets you move a guest VM from one AHV host to another AHV host or cluster while the VM is turned on, provided the target has available resources. If you're live-migrating to an AHV cluster in a different Prism Central instance, you must first configure it as a remote availability zone in the local Prism Central. 

Start live migration with any of the following methods: 

- Put the AHV host in maintenance mode (VM evacuation). 

- Use the Prism UI. 

- Use the aCLI (automatic, targeted, or maintenance mode). 

- Use the REST API (automatic, targeted, or maintenance mode). 

CPU types are abstracted from the VMs because AHV uses virtual CPUs (vCPUs) that are compatible between all nodes in the AHV cluster. As long as the destination AHV host has sufficient CPU cycles, you can migrate a VM. 

By default, live migration uses as much available bandwidth as required over the AHV host management interface, br0 and br0-up. You can restrict the bandwidth allocated for migration through the aCLI and the REST API using `bandwidth_mbps=X` during each migration. 

The following aCLI example enforces a 100 Mbps limit when migrating a VM, slow-laneVM1, to AHV host 10.10.10.11: 

```
nutanix@CVM$ acli vm.migrate slow-lane-VM1 bandwidth_mbps=100
host=10.10.10.11
```

To prevent resource contention, AHV limits the number of simultaneous automatic livemigration events to two. Nutanix doesn't recommend any specific live-migration method; use the live-migration method suited to the current administrative task. 

The number of simultaneous user-initiated live migrations isn’t limited, so consider the following points to avoid compute and network resource contention: 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

Nutanix AHV Best Practices 

- AHV limits the number of active live migrations in which a host can participate to two. If you schedule more than two live migrations, AHV queues them before completing them. 

- Starting in AOS 6.7 and its accompanying AHV version, you can use additional CPU threads for live migrations to speed transfers. Using additional CPU threads is most beneficial for faster uplinks such as 25 Gbps or for uplinks bonded with LACP, because AHV can schedule threads on both uplinks. 

© 2026 Nutanix, Inc. All rights reserved  | **31** 

Nutanix AHV Best Practices 

## 10. Nutanix AHV CPU Configuration 

AHV lets you configure vCPUs (equivalent to CPU sockets) and cores per vCPU (equivalent to CPU cores) for each VM. We recommend first increasing the number of vCPUs when a VM needs more than 1 CPU, rather than increasing the cores per vCPU. For example, if a guest VM requires 4 CPU, use 4 vCPU with one core per vCPU. 

The CPU scheduler is aware of physical cores and hyperthreaded cores. When a guest VM starts, it has an approximately even distribution of vCPU between physical and hyperthreaded cores. The CPU scheduler maintains the balance and can spread the workload across the physical cores for demanding workloads if needed. 

For an AHV host, guest VM performance is the same whether you have multiple vCPUs with single cores or fewer vCPUs with multiple cores. However, applications in a guest VM might perform slightly better with one configuration over another depending on how they use CPUs. 

The following situations might require increasing cores per vCPU instead of the number of vCPUs: 

- When the VM guest operating system can't use more than a limited number of vCPUs or sockets 

- When the VM application licensing is based on the number of vCPUs or sockets 

The maximum possible number of vCPUs and cores for a single VM is equal to the total number of hyperthreaded cores available in the AHV host. 

The CPU scheduler can schedule individual vCPUs that a guest VM is using. For example, in a guest VM that has 8 vCPU assigned but is using 2 vCPU, only the active 2 vCPU are scheduled, simplifying the scheduling process. 

Another example of this capability is the CVM that runs on every node and is pinned to a specific nonuniform memory access (NUMA) node. The CVM doesn't monopolize the CPUs assigned to it; the scheduler only schedules the CVM vCPUs that have active threads. Therefore, the NUMA node the CVM is using still has plenty of time to service guest VMs. 

© 2026 Nutanix, Inc. All rights reserved  | **32** 

Nutanix AHV Best Practices 

The hotplug feature allows you to add vCPUs or sockets to increase CPU capacity. Size VM compute resources when you create them or when they're turned off. If you must increase compute resources while the VM is running, consider the following limitations: 

- CPU: The multiqueue virtio-scsi PCI storage controller presented to the VM uses one request queue per vCPU. Because adding a vCPU to a running VM doesn't create new queues, the addition might limit performance. 

- Memory: The number of memory hotplug operations is limited to two or three, depending on the number of memory regions available for the VM when you turn it on. This counter resets on VM power cycles (VM off and VM on). 

Follow these recommendations for CPU configuration: 

- Use vCPUs instead of cores to increase the number of vCPUs available for a VM. Using vCPUs to add CPU power also ensures that hot-adding CPU works. You can't hot-add cores. 

- Use only as many vCPUs as the VM requires to limit resource waste. 

If the application performance isn't affected, it's better for AHV resource scheduling and usage to have 2 vCPU running at 50 percent utilization each than 4 vCPU running at 25 percent utilization each, because it's easier to schedule 2 vCPU. 

- Use the physical core count instead of the hyperthreaded count for maximum single VM sizing. 

- Don’t configure a single VM with more vCPU cores than physical CPU cores available on the AHV host, as this configuration can cause significant performance problems for the VM. 

For example, an application that consumes large amounts of CPU eventually exhausts the physical cores in the VM and must rely on the hyperthreaded cores, decreasing performance. 

- › If you require a VM configuration with large amounts of CPU, use hosts with larger core counts. 

- › If the application supports vNUMA, see the application documentation for guidance on using this topology. 

© 2026 Nutanix, Inc. All rights reserved  | **33** 

Nutanix AHV Best Practices 

## 11. Nutanix AHV Memory Configuration 

You can use all physical AHV host memory for VMs, aside from the memory used by the CVM and the AHV host itself. CVM memory requirements depend on the features that the cluster uses, specifically deduplication. If you enable performance-tier deduplication, set the CVM memory to at least 24 GB. For capacity-tier deduplication, configure at least 32 GB of memory on each CVM. Use the steps detailed in Increasing the Controller VM Memory Size to modify CVM memory on all AHV hosts. 

x86-based CPUs address memory in 4 KB pages by default, but they can use larger 2 MB pages, which are called large pages or hugepages. AHV uses these hugepages to provide the best possible performance. Certain applications running specific types of workloads see performance gains when using hugepages at the VM level. However, if the VM is configured to use hugepages, but the application and VM guest operating system don't fully utilize them, memory fragmentation and wasted memory might occur. Some database applications, such as MongoDB, recommend against using Transparent Huge Pages (THP; a method to abstract the use of hugepages) in the guest VM. Enable hugepages in the guest VMs only when the application and VM operating system require them. 

Follow these recommendations for memory configuration: 

- If possible, keep regular VMs within the size of one AHV host NUMA node. 

- Avoid running vNUMA, vUMA, and regular VMs on the same AHV host because migration of other VMs can cause vNUMA or vUMA VMs to fail to start. 

- Recommendations specific to vNUMA: 

   - › Use a vNUMA configuration for large VMs that require more CPU or memory than is available in one physical NUMA node. 

   - › Follow application-specific num_vnuma_nodes configuration recommendations when provided by Nutanix. 

   - › If no application-specific num_vnuma_nodes configuration recommendations are available, ignore the CVM compute resources and allow CPU overcommit. Set 

© 2026 Nutanix, Inc. All rights reserved  | **34** 

Nutanix AHV Best Practices 

num_vnuma_nodes equal to the number of AHV host NUMA nodes and set the number of VM CPUs equal to the number of available AHV host pCPUs. 

   - › vNUMA VMs run alone on the AHV host, separate from the CVM. 

- Recommendations specific to vUMA: 

   - › Size compute and memory for vUMA VMs proportionally. For example, if vRAM is half of physical NUMA RAM, set the number of vCPUs to be half the pCPUs in the physical socket. 

   - › Don't use vUMA for VMs that require more vCPU than the cores that are available in a NUMA node. 

   - › Use vUMA only for specific use cases determined through testing or official recommendations. Such use cases include applications that need predictable performance and low memory latency and are vNUMA-aware. 

For example, Microsoft SQL Server is vNUMA-aware; when the use case demands additional performance, vUMA might be a good fit. Oracle has similar performance requirements but isn't vNUMA-aware, so vUMA isn't a good fit. 

- When taking advantage of vNUMA and vUMA features, use hosts with the same sized memory and CPU or determine the NUMA boundaries of all Nutanix nodes where you are placing vNUMA- and vUMA-enabled VMs. 

- Use VM-host affinity rules, with a minimum of two AHV hosts to cover for VM high availability and maintenance activities, together with vNUMA to ensure that the VM doesn't move between nodes. 

This configuration is extremely important if the cluster has AHV hosts with differing amounts of memory and CPU. 

## **Nonuniform Memory Access and Guest VMs** 

CPUs have integrated memory controllers on the processor socket. NUMA is a computer memory design in which memory access times depend on the memory's location relative to a processor. In other words, because accessing nonlocal memory requires going through an interconnect, a system can access the memory local to a processor much 

© 2026 Nutanix, Inc. All rights reserved  | **35** 

Nutanix AHV Best Practices 

faster than it can access nonlocal memory. As a result, the best practice is to create VMs that fit within the memory available to the CPU socket where the VM is running whenever possible. 

An AHV host containing 2 CPU sockets with 16 cores each and 512 GB of memory has two NUMA nodes (one per physical socket), each of the following size: 

- 1 socket 

- 16 CPU cores (32 with hyperthreading) 

- 256 GB memory 

Some hosts have a different NUMA architecture than this per-socket setup; for example, a 12-core processor can have two NUMA nodes with 6 cores each. Conversely, it's also possible (but unusual) to have an architecture in which a NUMA node can span 2 physical CPU sockets. In most cases, an AHV host has one NUMA node per socket. 

For optimal resource usage, use both CPU and memory from the same NUMA node. Running CPU on NUMA node 0 and accessing memory from NUMA node 1 introduces memory latency. To take advantage of the lowest memory latency, create a virtual hardware topology for VMs that matches the physical hardware topology. 

Use the following CVM command to determine the NUMA topology of all AHV hosts. 

```
nutanix@cvm$ allssh "virsh capabilities | egrep 'cell id|cpus num|memory
 unit'"
Executing virsh capabilities | egrep 'cell id|cpus num|memory unit' on the
 cluster
================== 10.4.56.82 =================
        <cell id='0'>
          <memory unit='KiB'>134181480</memory>
          <cpus num='20'>
        <cell id='1'>
          <memory unit='KiB'>134217728</memory>
          <cpus num='20'>
```

Note that the value for `cpus num` includes hyperthreaded CPU cores. 

We use the following VM assumptions throughout this document: 

## **vNUMA or wide VMs** 

These VMs require more CPU or memory capacity than is available on a single NUMA node. 

© 2026 Nutanix, Inc. All rights reserved  | **36** 

Nutanix AHV Best Practices 

## **Virtual uniform memory access (vUMA) or narrow VMs** 

The capacity of a single NUMA node can fulfill the CPU and memory requirements of these VMs. Thus, you only need one NUMA node to provide these resources. 

## **Regular VMs** 

The total capacity of the AHV host can fulfill the CPU and memory requirements of these VMs; thus, one or more NUMA nodes might have to provide the resources. All NUMA nodes can provide the compute resources, even if the VM's requirements fit within a single NUMA node. 

## **Virtual Nonuniform Memory Access and Guest VMs** 

The primary purpose of virtual nonuniform memory access (vNUMA) is to give large or wide VMs the best possible performance. vNUMA helps wide VMs create multiple vNUMA nodes. Each vNUMA node has vCPUs and virtual RAM. Pinning a vNUMA node to a physical NUMA node ensures that vCPUs accessing virtual memory can access the expected NUMA behavior. Low-latency memory access expectations in virtual hardware (within vNUMA) can now match low latency in physical hardware (within physical NUMA), and high latency expectations in virtual hardware (cross vNUMA) can match high latency on physical hardware (cross physical NUMA). 

In many situations that require a vNUMA configuration, each AHV host is running only one wide VM. Some circumstances require multiple VM types (including vNUMA, vUMA, and regular VMs) to run on the same AHV host, but AHV isn’t optimized for such cases. In a few scenarios described later in this section, the vNUMA or vUMA configuration might be temporarily violated when the configured guest VMs move. You can configure vNUMA for a VM through the aCLI or REST API; this configuration is VM-specific and defines the number of vNUMA nodes. Memory and compute are divided in equal parts across each vNUMA node. 

```
nutanix@CVM$ acli vm.create <VMname> num_vcpus=<X> num_cores_per_vcpu=<X>
 memory=<X>G num_vnuma_nodes=<X>
```

vNUMA and vUMA VM configurations require strict fit. With strict fit, for each VM virtual node (configured using num_vnuma_nodes), memory must fit in a physical NUMA node. Each physical NUMA node can provide memory for any number of vNUMA nodes. Without enough memory in a NUMA node, the VM doesn't turn on. 

© 2026 Nutanix, Inc. All rights reserved  | **37** 

Nutanix AHV Best Practices 

The VM-to-AHV host configuration only implements strict fit for memory—not for CPU —as shown in the following examples, which use four physical NUMA nodes, each with one CPU socket, 10 CPU cores, and 128 GB of memory: 

- vUMA configuration: 

   - › A VM configured with 8 cores and 96 GB of memory fits within the NUMA node from a memory perspective, so you can turn the VM on. 

   - › A VM configured with 8 cores and 256 GB of memory doesn't fit within the NUMA node from a memory perspective, so you can't turn the VM on. 

- vNUMA configuration: 

   - › A VM configured with 16 cores, 192 GB of memory, and two vNUMA nodes fits in two NUMA nodes, so you can turn the VM on. 

   - › A VM configured with 20 cores, 384 GB of memory, and two vNUMA nodes doesn't fit in two NUMA nodes, so you can't turn the VM on. 

If a VM's vUMA or all vNUMA nodes fit in an AHV host, the virtual resources are pinned to physical resources. If a VM doesn't fit any AHV host, the VM start action either succeeds or fails, depending on who initiated the start action. Administrator-initiated VM start and VM live-migration operations are always strictly pinned, which means that you can't perform them unless the target AHV host's node can fit the vNUMA and vUMA configuration. You can move vNUMA and vUMA VMs to an AHV host without sufficient memory in a single NUMA node under the following circumstances: 

- AHV maintenance mode 

- Host high availability failover or restore 

**Note:** Any VM configured for vNUMA or vUMA is excluded from the ADS. 

## **Wide and vUMA VMs** 

VMs with memory requirements that can't be fulfilled by one NUMA node are wide VMs. With wide VMs, you define the number (and, implicitly, the size) of vNUMA nodes. The compute and memory for the CVM are pinned to a physical NUMA node, so when you determine the number of vNUMA nodes for a wide VM, you can choose from one of the following approaches: 

© 2026 Nutanix, Inc. All rights reserved  | **38** 

Nutanix AHV Best Practices 

- Ignore all compute resources used by the CVM and allow CPU overcommit on the CVM's physical NUMA node. 

- Set aside an AHV host core for each CVM vCPU; this reservation prevents CPU overcommit. 

- Ignore some fraction of the CVM compute resources out of the available AHV host compute resources. Allow CPU overcommit to the extent of the ignored fraction. 

Depending on the workload and sizing, if the vNUMA VM doesn't drive both compute and storage heavily at the same time, overcommit might not be a problem. If CVM resource usage doesn't affect an application, you can allow CPU resource overcommit; however, some applications see decreased performance with any CPU overcommit. 

If a VM's compute and memory requirements can fit on one NUMA node, you can enable virtual uniform memory access (vUMA), guaranteeing that the VM uses CPU and memory from the same NUMA node. 

vUMA or narrow VMs deliver the lowest memory latency possible for VMs that fit within one NUMA node. Enabling vUMA uses the same command syntax as enabling vNUMA; the only difference is that the num_vnuma_nodes parameter is set to 1. 

```
nutanix@CVM$ acli vm.create <VMname> num_vcpus=<X> num_cores_per_vcpu=<X>
 memory=<X>G num_vnuma_nodes=1
```

As a common example, the CVM is configured as a vUMA VM and pinned to a specific NUMA node, while all the guest VMs on the cluster are typically configured as regular VMs. 

You can’t use vUMA when the VM requires more memory than is available in a NUMA node based on strict-fit requirements. Although technically you can use vUMA if the VM requires more vCPU than the cores available in the NUMA node, we strongly recommend that you don’t take this approach. 

## **Memory Overcommit** 

At any point, the VMs on the host might not use all their allocated memory. With memory overcommit, you can take memory from VMs that aren't using it and give it to other VMs that currently need it. For example, a host with a total memory of 100 GB contains five VMs with varying memory allocations. At any point, each VM might have idle memory that it doesn't use. Memory overcommit allows the host to run more VMs than its natural 

© 2026 Nutanix, Inc. All rights reserved  | **39** 

Nutanix AHV Best Practices 

memory allocation permits, which means that the sum of VM memory sizes on a host can be larger than the physically available memory on that host, increasing density. 

On VMware ESXi, memory overcommit is enabled by default on a new VM. You can decide to set limits differently or to guarantee all the memory of the VM. Historically, AHV VMs had a fixed memory size, and VMs continue to be created as fixed-size by default, so you must enable memory overcommit on a VM to use this feature. 

To mirror this behavior, VMs are always created as fixed-size during a Nutanix Move VM migration operation. You can manually change them to overcommitted mode after the migration is complete. 

For this memory management technique to work most efficiently, install a balloon driver on each VM enabled with memory overcommit. While you technically can run overcommitted VMs without a balloon driver, performance can be very poor. Most modern versions of standard Linux distributions generally provide a VirtIO balloon driver. For an overview of fully and partially supported Linux distributions, see the following table. 

_Table: Fully and Partially Supported Linux Distributions_ 

|**Distribution**|**No Balloon Driver**|**Partially Supported**|**Fully Supported**|
|---|---|---|---|
|CentOS|6.1, 6.2|6.3–6.9, 7.1, 7.2|7.3–7.7, 8.0–8.2|
|Oracle|7.3|7.4, 7.5|7.6, 7.7|
|Ubuntu*|See note.|12.04|14.04 and newer|



**Note:** * Some Ubuntu versions have a working balloon driver, but it might be disabled by default. 

To ensure that the balloon driver is loaded and active, run the following command on the VM: 

```
# lsmod | grep virtio_balloon
```

If any output returns, the driver is active. 

**Note:** Windows VMs don't have a balloon driver installed by default, so you must install NGT version 2.1.1 or later to ensure that one is available. 

Because overcommitted VMs give up part of their memory during idle periods, that memory might not be available immediately when a VM suddenly requires a large amount of memory again. In such situations, the VM might run out of memory before 

© 2026 Nutanix, Inc. All rights reserved  | **40** 

Nutanix AHV Best Practices 

AHV can react to the increased requirement, causing it to end applications. To prevent unexpected application shutdown, ensure that sufficient swap space is available inside the VM to provide a temporary memory area the VM can overflow to while memory is reassigned to it. To be safe, we recommend provisioning swap space that is at least equal to the RAM size of the VM. 

Before implementing memory overcommit on any VMs in a cluster, evaluate the following important limitations and considerations for each VM and workload: 

Limitations: 

- Memory overcommit isn't supported when the VM uses GPU passthrough or vNUMA. 

- You can't hot-add memory to a VM when memory overcommit is enabled. 

- Memory overcommit isn't supported when high availability is configured to use reserved hosts. 

- The memory overcommit enabled status of VMs isn't retained after failover or migration. 

- You can only enable or disable memory overcommit when the VM is turned off. 

- Considerations: 

- Security concerns about adding memory content to storage 

- Potential performance impact on guest VMs that share memory 

For more information, see the Limitations of Memory Overcommit section of the AHV Administration Guide. 

Nonproduction deployments, such as test and development environments, are often good candidates for safely using memory overcommit. However, environments with dynamic memory usage aren't good candidates. Sudden changes in demand can create periods of decreased performance in a VM that rapidly tries to increase memory usage beyond what's available in the reserved buffer maintained by the hypervisor. This scenario is likely to occur when multiple VMs request memory increases simultaneously. 

You can use Nutanix Prism to enable and disable memory overcommit on a per-VM basis. If you need to enable memory overcommit for a larger group of VMs, run the following command from the aCLI: 

```
vm.update <vm_list> memory_overcommit=True
```

© 2026 Nutanix, Inc. All rights reserved  | **41** 

Nutanix AHV Best Practices 

For the `<vm_list>` parameter, you can provide a comma-separated list of VM names or a VM name wildcard expression to update multiple VMs. 

© 2026 Nutanix, Inc. All rights reserved  | **42** 

Nutanix AHV Best Practices 

## 12. AHV Turbo Technology 

To maximize storage performance, Nutanix developed AHV Turbo technology, which benefits the following entities: 

- VMs, by presenting a multiqueue virtio-scsi PCI controller 

- The hypervisor, by processing different queues in parallel using multiple threads 

- The interface between the hypervisor and the storage layer, by increasing efficiency when receiving the requests from the hypervisor and passing them to the CVM 

The following figure explains the logical implementation of AHV Turbo. 

© 2026 Nutanix, Inc. All rights reserved  | **43** 

Nutanix AHV Best Practices 

Figure 3: AHV Turbo Technology Logical Implementation 

AHV Turbo technology is transparent to VMs, but you can make the following configuration changes to achieve greater performance: 

© 2026 Nutanix, Inc. All rights reserved  | **44** 

Nutanix AHV Best Practices 

- Enable multiqueue on the VM. Consult your Linux distribution documentation to ensure that the guest operating system fully supports multiqueue before you enable it and for instructions on how to enable it. One common method for enabling multiqueue is to add one of the following lines to the kernel command line: 

```
scsi_mod.use_blk_mq=y
scsi_mod.use_blk_mq=1
```

- Install virtIO for Windows-based VMs. No additional configuration is required. 

- Provide more than 1 vCPU to the VM. 

- Ensure that the workloads are multithreaded. 

© 2026 Nutanix, Inc. All rights reserved  | **45** 

Nutanix AHV Best Practices 

## 13. Nutanix AHV VM Disk Configuration 

AHV-based VMs can use SCSI, PCI, SATA, or IDE disks; SCSI is the default disk type when creating AHV-based VMs. You can determine the appropriate number of disks for a VM by referring to Nutanix reference architectures, best practices guides, and technical notes, as well as by consulting vendor recommendations. The maximum number of SCSI disks attached to a VM is 256, and the maximum number of PCI disks attached to a VM is 7. 

Thin provisioning provides efficient storage space utilization by allowing the system to present storage devices without allocating space until data is written. Nutanix storage pools and containers enable thin provisioning by default as a core feature of AOS Storage. vDisks created in AHV VMs or VGs are thin-provisioned. 

As the system writes data, it allocates storage in the vDisks. Data deleted from a vDisk can remain allocated in the Nutanix storage pool. To maintain storage efficiency, when you delete a vDisk from an AHV VM or VG, AOS Storage reclaims the space consumed by the deleted file as a background process. 

Follow these recommendations for disk configuration: 

- Use SCSI-based disks wherever possible for maximum performance. 

- Use the default Windows settings for UNMAP. 

- Configure Linux guest VMs with a weekly scheduled fstrim if needed. 

- Use the nodiscard option when formatting Linux disks for maximum format speed. 

- Use SATA or PCI-based disks for older Linux VMs (such as RHEL5) that don't include the SCSI paravirtual drivers used by AHV. 

- Use the IDE disk type for CD-ROM tasks such as booting a VM and installing the paravirtual drivers from an ISO. Because the IDE bus has lower performance, use the other bus types for applications and drives that require high storage performance. 

© 2026 Nutanix, Inc. All rights reserved  | **46** 

Nutanix AHV Best Practices 

## **SCSI UNMAP for Windows** 

AHV VM vDisks and VGs support the SCSI UNMAP command as defined in the SCSI T10 specification. The SCSI UNMAP command allows a host application or operating system to specify that a range of storage is no longer in use and can be reclaimed. This capability is useful when data is deleted from a vDisk that AHV presents. Both the Windows and Linux operating systems provide native support for issuing UNMAP commands to storage. 

When the guests send UNMAP commands to the Nutanix storage layer, Nutanix Prism accurately reflects the amount of available storage as AOS Storage background scans complete. If the guest doesn't send UNMAP commands, freed space isn't made available for other guests and doesn't display as available in Nutanix Prism. 

Windows Server 2012 or later can issue industry-standard UNMAP commands to tell the Nutanix cluster to free storage associated with unused space. Windows supports reclaim operations against both NTFS and ReFS formatted volumes; these operations are enabled by default. 

With Windows Server 2012 or later, UNMAP commands are issued under the following conditions: 

- When you delete files from a file system, Windows automatically issues reclaim commands for the area of the file system that you freed. 

- When you format a volume residing on a thin-provisioned drive with the quick option, Windows reclaims the entire size of the volume. 

- When a regularly scheduled operation selects the Optimize option for a volume or when you manually select this option either from the Optimize Drives console or when using the optimize-volume PowerShell command with the Retrim option, Windows reclaims the space. 

To check the current Windows configuration, which is a global setting for the host, use the fsutil command: 

```
fsutil behavior query DisableDeleteNotify
DisableDeleteNotify=0    <---- enabled (default)
DisableDeleteNotify=1    <---- disabled
```

To disable the feature, use the following command: 

© 2026 Nutanix, Inc. All rights reserved  | **47** 

Nutanix AHV Best Practices 

```
fsutil behavior set DisableDeleteNotify 1
```

To enable the feature, use the following command: 

```
fsutil behavior set DisableDeleteNotify 0
```

We recommend using the default Windows configuration, which allows the guest file system to report freed space to the storage layer as files are deleted. 

Windows guests send UNMAP commands by default during disk format operations, which can increase the amount of time required to complete the format. Set DisableDeleteNotify to **1** , temporarily disabling the feature during format to finish the format operation faster. Enable the feature after the format operation is complete. 

## **SCSI UNMAP for Linux** 

Most Linux guests don't send UNMAP operations by default for mounted disks. You can enable UNMAP operations in the Linux guest, either by mounting the disk with the discard option or by performing scheduled fstrim operations. We recommend following RHEL documentation and using a periodic fstrim operation instead of the discard mount option if you need guest free space reporting in Linux. 

To check the current Linux configuration, which is a setting for each disk, you can view the value of discard_zeroes_data: 

```
cat /sys/block/<device>/queue/discard_zeroes_data
0 => TRIM (UNMAP) disabled
1 => TRIM (UNMAP) enabled
```

Some Linux distributions, such as Ubuntu and SUSE, perform periodic fstrim operations for you in a weekly cron task. We recommend using the following configuration in Linux guests: 

```
cat /etc/cron.weekly/fstrim
/sbin/fstrim --all || true
```

If possible, randomize the start time among guests. 

Linux file systems such as Ext4 send UNMAP commands during the format operation, which can increase the amount of time required to finish formatting. To finish the format operation faster, use the nodiscard option: 

```
mkfs.ext4 -E nodiscard /dev/sdXY
```

© 2026 Nutanix, Inc. All rights reserved  | **48** 

Nutanix AHV Best Practices 

## 14. Nutanix AHV Resource Oversubscription 

You must account for a small amount of memory overhead per host and per VM in the AHV host when calculating available memory for VMs. This overhead is used for tasks such as management, the emulation layer, and I/O buffers. Although actual overhead amounts might change across upgrades and depend on the host and VM configurations, you can use the following values to estimate overhead when calculating memory requirements: 

- 3.5 GB per host, plus 2.2 percent of total system memory, for AHV 

- 2 GB per host if Flow Virtual Networking is enabled in Prism Central 

- 4 GB per host if Flow microsegmentation is enabled in Prism Central 

- 1.5 percent of total system memory if GPUs are installed on the host 

Using these estimates, the memory overhead on a 256 GB host with Flow Virtual Networking and microsegmentation enabled is approximately 15 GB (3.5 + 256 × 0.022 + 2 + 4), which leaves about 241 GB to run the CVM and user VMs. 

Overhead for VMs depends on the VM configuration. A standard VM with one VirtIO NIC and no GPUs has an overhead of approximately 50 MB. VM overhead increases by an additional 1 MB per allocated vCPU and 7 MB per allocated GB of memory. The following table provides VM overhead values for various combinations of memory size and vCPU count. 

_Table: VM Overhead Based on Memory and vCPU Sizing_ 

|**VM Memory**|**VM Overhead**|**VM Overhead**|**VM Overhead**|**VM Overhead**|
|---|---|---|---|---|
||**with 1 vCPU**|**with 2 vCPU**|**with 4 vCPU**|**with 8 vCPU**|
|1 GB|58 MB|59 MB|61 MB|65 MB|
|2 GB|65 MB|66 MB|68 MB|72 MB|
|4 GB|79 MB|80 MB|82 MB|86 MB|
|8 GB|107 MB|108 MB|110 MB|114 MB|



© 2026 Nutanix, Inc. All rights reserved  | **49** 

Nutanix AHV Best Practices 

## **CPU and Memory Oversubscription** 

Virtualization allows the number of vCPU cores in the VM to be decoupled from the number of physical CPU cores in the hypervisor. We recommend following industrystandard virtual-to-physical oversubscription ratios for CPU to provide the best possible performance. 

The following examples of virtual-to-physical CPU oversubscription ratios are general guidance; adjust them as appropriate for individual deployments. Latency-sensitive server virtualization applications have lower ratio requirements than applications like enduser computing that can tolerate higher virtual-to-physical CPU oversubscription. 

_Table: CPU Oversubscription Ratios_ 

|**Virtual-to-Physical CPU Ratio**|**Application Class**|
|---|---|
|1:1 or 2:1|Tier 1: Business-critical or latency-sensitive|
||applications|
|2:1 or 4:1|Tier 2: Noncritical applications, not highly|
||latency sensitive|



Use the figures for CPU oversubscription ratios provided in the previous table as a starting point for determining CPU oversubscription. The environment's business and technical requirements are the most critical inputs for determining oversubscription. 

For the Acropolis Dynamic Scheduler (ADS) to turn on a VM, the AHV host must have at least 5 percent of its CPU free. 

You can oversubscribe memory in AHV with active VMs. You can create and configure VMs to oversubscribe memory. 

© 2026 Nutanix, Inc. All rights reserved  | **50** 

Nutanix AHV Best Practices 

## 15. Nutanix AHV Additional References 

For more information, see these documents: 

- AHV Administration Guide 

- AHV Administration Guide: Windows VM Provisioning 

- Compatibility and Interoperability Matrix: AHV Guest OS page 

- Prism Web Console Guide: Increasing the Controller VM Memory Size 

- Prism Web Console Guide: In-Place Hypervisor Conversion 

For AHV networking best practices, see the checklist in the Nutanix AHV Networking best practice guide. 

The following list summarizes the best practice recommendations in this document: 

- Acropolis Dynamic Scheduler: 

   - › Leave ADS enabled unless you require strict control—specifically, if no live migrations are allowed. 

   - › Using VM-host affinity, pin each VM to a minimum of two AHV hosts to ensure VMHA functionality for the VM during a VMHA event. 

   - › Limit the maximum number of VMs in an ADS VM-VM antiaffinity group to the number of AHV hosts in the cluster to ensure that all VMs in the group run on different AHV hosts. 

- VM deployment: 

   - › Create VM clones from an optimized base image, cleared of all unnecessary applications, data, and instance-specific information. 

   - › Create clones from a turned-off VM or from a VM snapshot for best performance when creating more than 10 clones. 

   - › Create new VMs without clones to maximize storage performance for highly latency-sensitive applications. 

© 2026 Nutanix, Inc. All rights reserved  | **51** 

Nutanix AHV Best Practices 

- VM data protection: 

   - › Use on-demand snapshots for day-to-day VM management tasks. 

   - › Use scheduled snapshots with protection domains and remote sites for disaster recovery. 

   - › Ensure that you configure layer 2 VLAN mappings when enabling a remote site protection domain. 

- Live migration: 

   - › Restrict migration bandwidth using the aCLI if required. 

   - › Exercise caution when manually migrating a large number of VMs, as the number of simultaneous manually initiated migrations is unlimited. 

- CPU configuration: 

   - › Increase the vCPU count in VMs rather than cores per vCPU, unless specific VM licensing requirements call for a minimum number of CPU sockets. 

   - › Use the physical core count instead of the hyperthreaded core count for maximum single VM sizing. 

   - › Don't configure a single VM with more vCPU cores than physical CPU cores available on the AHV host. 

- Memory configuration: 

   - › Increase the CVM memory in the cluster if deduplication and compression are enabled. 32 GB of RAM for the CVM is a good starting point if using both deduplication and compression. 

   - › Size VMs to fit within the smallest potential NUMA boundary of the cluster for maximum VM performance on all cluster nodes. 

   - › Use vUMA or vNUMA for guest VMs only when an application is NUMA-aware, supports using NUMA, and requires the additional performance. 

© 2026 Nutanix, Inc. All rights reserved  | **52** 

Nutanix AHV Best Practices 

- VM disk configuration: 

   - › Use SCSI disks wherever possible for maximum performance. In order of preference, use SATA, PCI, and IDE only where required. 

   - › Use the default Windows guest configuration to send UNMAP commands to the Nutanix cluster. You can temporarily disable UNMAP temporarily to improve disk format speed. 

   - › Configure Linux guests with a weekly scheduled fstrim task to return free space to the Nutanix cluster. Format disks with the nodiscard option to improve disk format speed. 

   - › Install the required VirtIO drivers in the Windows guest VMs using the packaged Nutanix installer. 

   - › Use NGT when you need VM mobility, cross-hypervisor disaster recovery, VSS, or Self-Service Restore. Otherwise, use the standalone Nutanix VirtIO driver package. 

- Resource oversubscription: 

   - › Follow industry-standard vCPU oversubscription practices and don't oversubscribe more than 4:1 when starting VMs. 

   - › Consider memory overhead per vCPU when calculating available memory. 

   - › Oversubscribe memory only in nonproduction use cases. 

- Hugepages: Allow the VM OS or VM administrator to enable hugepages as required in the guest. 

- General management: 

   - › Manage the AHV environment through Nutanix Prism or use SSH to access the CVM when required. 

   - › Avoid connecting directly to the AHV host. 

   - › Use one of the following options to make sure the configuration is consistent across all CVMs or AHV hosts in the cluster: 

```
allssh
hostssh
for i in `svmips`
for i in `hostips`
```

© 2026 Nutanix, Inc. All rights reserved  | **53** 

Nutanix AHV Best Practices 

## **Command Examples** 

## • Network view commands: 

```
nutanix@CVM$ manage_ovs --bridge_name br0 show_uplinks
nutanix@CVM$ ssh root@192.168.5.1 "ovs-appctl bond/show br0-up"
nutanix@CVM$ ssh root@192.168.5.1 "ovs-vsctl show"
nutanix@CVM$ acli
<acropolis> net.list
<acropolis> net.list_vms vlan.0
nutanix@CVM$ allssh "manage_ovs show_interfaces"
nutanix@CVM$ allssh "manage_ovs --bridge_name <bridge> show_uplinks"
```

## • Load balance view: 

```
nutanix@CVM$ ssh root@192.168.5.1 "ovs-appctl bond/show"
```

## • High availability: 

```
nutanix@CVM$ acli ha.update num_reserved_hosts=X
```

## • Live-migration bandwidth limits: 

```
nutanix@CVM$ acli vm.migrate slow-lane-VM1 bandwidth_mbps=100
host=10.10.10.11 live=yes
```

## • View NUMA topology: 

```
nutanix@cvm$ allssh "virsh capabilities | egrep 'cell id|cpus num|memory
 unit'"
```

## • Verify THP: 

```
nutanix@CVM$ ssh root@192.168.5.1 "cat /sys/kernel/mm/transparent_hugepage/
enabled"
[always] madvise never
```

© 2026 Nutanix, Inc. All rights reserved  | **54** 

Nutanix AHV Best Practices 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **55** 

## **List of Figures** 

Figure 1: Post-Foundation Network State....................................................................................................10 Figure 2: Default Untagged VLAN for CVM and AHV Host.........................................................................12 Figure 3: AHV Turbo Technology Logical Implementation...........................................................................44 


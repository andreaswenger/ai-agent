# **Migrating VMs to Nutanix AHV** 

## Legal 

© 2025 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Migrating VMs to Nutanix AHV 

## **Contents** 

**1. Migration Overview..................................................................................4 2. Nutanix AHV Migration Considerations.................................................7 3. Nutanix Move............................................................................................8** Migration Planning with Nutanix Move............................................................................................. 10 Migrating with Nutanix Move............................................................................................................ 11 Nutanix Move for ESXi, Hyper-V, and Public Cloud Environments.................................................. 12 **4. Third-Party ESXi to Nutanix AHV Migration........................................17** Migrating with Nutanix Image Service.............................................................................................. 17 Migrating with Sureline......................................................................................................................18 Application-Centric Migration............................................................................................................ 19 **5. Nutanix ESXi Cluster to Nutanix AHV Migration................................ 21** Migrating with Nutanix VM Mobility.................................................................................................. 21 Migrating with Cluster Conversion....................................................................................................23 **6. AWS to Nutanix AHV Migration............................................................25 About Nutanix.............................................................................................27 List of Figures.............................................................................................................................................28** 

Migrating VMs to Nutanix AHV 

## 1. Migration Overview 

Migrations are necessary to ensure that you can replace aging systems while taking advantage of stronger technologies that add business value. A robust migration methodology lowers risk and ensures minimal disruption. 

Methods for performing cross-hypervisor migrations vary greatly. Administrators must choose methods based on their requirements because each migration scenario has unique technical and business needs. Consider the following aspects of planning a migration: 

- Downtime requirements for specific applications 

- Whether the source and target for a migration are physical operating systems or virtualized environments 

- Whether the source and target for a migration run on the same infrastructure vendor, where you can use native replication tools 

- Whether native application-level methods exist, including integrated replication technologies used for high availability and disaster recovery or backup and restore 

- Whether you need to make changes to an application as a part of the migration, such as moving to a newer version or modifying the physical layout of a database 

- Whether you need to change the physical location of the environment, which could affect existing networking configurations and data replication considerations 

- The trade-offs inherent to using third-party technologies that can simplify migrations and limit downtime but also add to the total cost of the project 

- Whether the team has the skills and experience to perform the migration with minimal impact on the business 

This list isn't exhaustive, but it outlines some of the complexities involved in running a successful migration project. We don't cover all of these considerations in depth here; instead, this guide provides high-level guidance on the recommended methods for migrating VMs to Nutanix AHV. We address native Nutanix migration methods, from 

© 2025 Nutanix, Inc. All rights reserved  | **4** 

Migrating VMs to Nutanix AHV 

third-party platforms or from Nutanix platforms, as well as third-party migration software that can help simplify the process. Nutanix also provides a workload migration offering through Nutanix Professional Services. 

_Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|February 2017|Original publication.|
|2.0|December 2017|Updated for AOS 5.5 and|
|||added product information.|
|2.1|May 2018|Updated Nutanix overview|
|||and the AHV Migration|
|||Considerations section.|
|2.2|October 2018|Updated product information.|
|3.0|April 2019|Updated product information|
|||and added support for Amazon|
|||Web Services (AWS).|
|3.1|April 2020|Content refresh.|
|3.2|November 2022|Updated the AHV Migration,|
|||Nutanix Move for AWS, and|
|||Migration Using Nutanix|
|||Move sections and added|
|||the Nutanix Move for ESXi,|
|||Nutanix Move for Hyper-V,|
|||and Nutanix Move for Azure|
|||sections.|
|3.3|September 2023|Updated the Nutanix Move|
|||section.|
|3.4|December 2023|Added information about|
|||Microsoft Windows SAN policy|
|||to the Cluster Conversion|
|||section.|
|3.5|December 2024|Refreshed content and|
|||diagrams.|
|3.6|September 2025|Updated document structure.|



© 2025 Nutanix, Inc. All rights reserved  | **5** 

Migrating VMs to Nutanix AHV 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|3.7|December 2025<br>Updated the Nutanix Move<br>for ESXi, Hyper-V, and Public<br>Cloud Environments section.|



© 2025 Nutanix, Inc. All rights reserved  | **6** 

Migrating VMs to Nutanix AHV 

## 2. Nutanix AHV Migration Considerations 

The most common migration method in virtualized environments involves moving an existing operating system and its applications without modification. Nutanix AHV supports a wide range of operating systems that you can migrate from physical or virtual environments. The specific recommended virtual hardware, such as SCSI or PCI bus types, depends on the operating system. For a current list of supported operating systems and recommended bus types, see the Compatibility and Interoperability Matrix: AHV Guest OS. 

Migrating an existing operating system to AHV involves changing the underlying virtual hardware. Among other differences, this transition includes a different virtual NIC (vNIC) and virtual SCSI (vSCSI) device than ESXi environments. Thus, you must install the drivers that support AHV virtual hardware (the VirtIO drivers) in the operating systems that you migrate. 

Nutanix offers a VM Mobility driver package, which includes Nutanix-qualified VirtIO drivers for Windows VMs. Nutanix-based environments can install Nutanix Guest Tools (NGT), which includes the VM Mobility driver package. Check Linux operating systems to ensure that the VirtIO modules are installed. 

It's important to choose the best method for moving data to a Nutanix cluster with AHV because the method can affect the availability of the applications and operating systems that you migrate. 

Regardless of source and target platform type, third-party software can perform storageand platform-agnostic migrations. Some application-specific methods don't involve migrating an operating system but instead move data between existing operating systems and applications running on the target system. 

© 2025 Nutanix, Inc. All rights reserved  | **7** 

Migrating VMs to Nutanix AHV 

## 3. Nutanix Move 

Nutanix Move quickly and easily transfers VMs from ESXi, Hyper-V, and public cloud environments (including AWS) to Nutanix AHV, keeping VM data unmodified and intact. After you deploy Move as a VM in your AHV environment, you can use its HTML5-based interface to manage it. 

Nutanix Move is a VM appliance, typically hosted on the target AHV cluster. Several software services come together to build Nutanix Move, but we can group them into the following major software components: 

- The management server 

- Agents for source and target 

- Disk readers and writers 

The architecture for each source environment that Move uses is slightly different, but Nutanix makes the difference in implementation invisible to users. 

© 2025 Nutanix, Inc. All rights reserved  | **8** 

Migrating VMs to Nutanix AHV 

Figure 1: Nutanix Move Architecture 

## **Management server** 

The management server maintains source and target cluster information, as well as migration plan details and current status. It also allows APIs and the UI to create and manage migration plans. 

## **Agents for source and target** 

The source agent is a platform-specific (ESXi, Hyper-V, AHV, or cloud) software component that schedules migration copy requests through disk readers. It collects source cluster and VM details and helps you select the VMs to migrate using the management server UI. 

The target agent collects and keeps inventory information for the target cluster, allowing you to create migration plans. It also mounts the container in the target to 

© 2025 Nutanix, Inc. All rights reserved  | **9** 

Migrating VMs to Nutanix AHV 

prepare the disk writer to copy the data. At cutover, the target agent converts disk formats to support AHV. 

## **Disk readers and writers** 

Disk reader processes use source-specific APIs to read data and coordinate with disk writer processes to complete outstanding copy operations. The disk reader checkpoints copy operations to handle failures and resume operations as needed. 

Nutanix Move is intuitive, simple, and fast while reducing the risk and cost of migrations. For migrations with ESXi, Hyper-V, Azure, or AWS as the source, we recommend using Move for migrating to Nutanix AHV. 

Note that Move doesn't currently support all hypervisor environments as migration sources or non-AHV environments as targets, so heterogeneous source and target environments require other migration methods. 

## **Migration Planning with Nutanix Move** 

After you define your environments, you can create migration plans. Plans enable migrations to target a subset of VMs in a source. 

Nutanix Move verifies that the target environment has enough compute and storage resources to support the VMs added to a migration plan. Move can sort VMs by whether you can migrate them and provides a summary to indicate why you can't migrate certain VMs (for example, if the VM doesn't have VMware tools installed or doesn't meet virtual hardware version level minimums). Move supports the same virtual guest operating systems that AHV supports. For a full list of supported operating systems, see the Move User Guide. 

For ESXi sources, Move uses VMware VADP to manage the replication process, so you don't need to install agents in the VMs or the ESXi hosts. For Hyper-V sources, Move uses the agent installed on the Hyper-V hosts to manage the replication process. For AWS and Azure sources, Move uses the storage APIs to manage the replication process. 

You can specify network mappings to match the source and destination networks for the VMs and set up a test network to test the migration beforehand. Because the test process leaves the source VM turned on, we recommend using a nonroutable test network so that it doesn't interfere with the source VM. 

© 2025 Nutanix, Inc. All rights reserved  | **10** 

Migrating VMs to Nutanix AHV 

Automatic preparation mode allows Move to connect to VMs directly to install the AHVcompatible VirtIO drivers and to capture network settings to carry over to the target environment. You can specify the credentials to connect to the VMs selected in a plan either for all VMs at once or individually as needed. If the target VM is migrating to ESXi, Move can uninstall the VMware guest tools from the target VM when the migration is complete. 

By defining a migration schedule, you can set data seeding to start in a predetermined window. 

After you configure the options described, the migration can seed the data to the AHV cluster. This process involves creating snapshots for each VM and then replicating the virtual disks to the specified AHV container. You can pause or stop migrations in progress at any time. Move stores the virtual files for the migrating VMs in a temporary folder and incrementally uses CBT APIs and continued snapshot operations to keep them current. 

When it's time to cut over and complete the migration, Move turns off the source VMs and disconnects the virtual NICs. Incremental data then synchronizes over to the AHV cluster. After all data replication completes, Move uses the AHV image service to convert the source virtual disk files to the native RAW format that AHV uses. Because the disk formats are the same, conversion from the source virtual disk to RAW is extremely fast —each disk converts in just a few seconds, limiting downtime. Move also provides an estimated cutover time, so you can determine any maintenance window in advance. 

You can cut over VMs in a plan together or separately. To complete the migration, Move turns the VMs on in the target environment and removes all temporary VMDK files and converted images in the AHV image service. Although the source VMs are off and disconnected from their networks, they persist in case you need them. 

Move keeps track of which VMs you finish migrating and which VMs remain to be transferred, so it's easy to know which source VMs to target if you create additional migration plans. 

## **Migrating with Nutanix Move** 

To use Nutanix Move for migration, follow these steps: 

**1.** Download the Nutanix Move image. 

**2.** Launch Move as a VM from the Prism console of a target cluster. 

© 2025 Nutanix, Inc. All rights reserved  | **11** 

Migrating VMs to Nutanix AHV 

**3.** Complete one or more of the following options to register your environments with Move as migration sources and targets: 

   - To register an AHV environment, connect to the AHV cluster (Prism Element) or to a Prism Central instance name or IP address with the appropriate credentials. 

   - To register an ESXi environment, connect to your vCenter instance and supply the name or IP address and the appropriate credentials. 

   - To register a Hyper-V environment, connect to your Hyper-V server or cluster instance and provide the name or IP address and the appropriate credentials. 

   - To register an AWS environment, provide your account ID, username, password, access key, and secret keys. 

   - To register an Azure environment, provide your environment name, subscription ID, tenant ID, client ID, and client secret. 

## **Nutanix Move for ESXi, Hyper-V, and Public Cloud Environments** 

The architecture of Nutanix Move for ESXi uses vCenter for inventory collection and vSphere Storage APIs for Data Protection (VADP), the Virtual Disk Development Kit (VDDK), and Changed Block Tracking (CBT) for data migration. 

© 2025 Nutanix, Inc. All rights reserved  | **12** 

Migrating VMs to Nutanix AHV 

Figure 2: Nutanix Move for ESXi Architecture 

The architecture of Nutanix Move for Hyper-V has an agent on each Hyper-V server that makes up part of the Hyper-V cluster. The agents move data, and Nutanix Move uses the Hyper-V Manager to collect inventory. 

© 2025 Nutanix, Inc. All rights reserved  | **13** 

Migrating VMs to Nutanix AHV 

Figure 3: Nutanix Move for Hyper-V Architecture 

For more information on Hyper-V, see Hyper-V to AHV in the Move User Guide. 

The architecture of Nutanix Move for Amazon Web Services (AWS) is somewhat different from Move in other environments. When you add AWS as an environment, the Move appliance connects to AWS for inventory and uses the Elastic Block Store (EBS) direct APIs for data migration. Nutanix Move doesn't create an agent VM for AWS. 

© 2025 Nutanix, Inc. All rights reserved  | **14** 

Migrating VMs to Nutanix AHV 

Figure 4: Nutanix Move for AWS Architecture 

For more information on AWS, see AWS to AHV in the Move User Guide. 

The architecture of Nutanix Move for Azure is similar to the architecture for AWS in that it uses the Azure Public Cloud REST APIs to collect inventory and access storage. 

© 2025 Nutanix, Inc. All rights reserved  | **15** 

Migrating VMs to Nutanix AHV 

Figure 5: Nutanix Move for Azure Architecture 

For more information on Azure, see Microsoft Azure Cloud to AHV in the Move User Guide. 

© 2025 Nutanix, Inc. All rights reserved  | **16** 

Migrating VMs to Nutanix AHV 

## 4. Third-Party ESXi to Nutanix AHV Migration 

For migrations involving non-Nutanix platforms as the source, Nutanix allows NFS-, SMB-, and SFTP-based access to the underlying Nutanix storage containers to permit migrating or copying virtual disk and ISO files. In ESXi environments, you can use NFS access to perform a storage vMotion that moves the required virtual disks to the AHV environment nondisruptively. The AHV cluster's file system allowlist controls access using NFS. 

After you migrate the virtual disks, the Nutanix Image Service can convert those disks to the RAW disk format that AHV uses. The system stores the converted virtual disks in the `.Acropolis` folder on the specified storage container as an image. You then use the image to create a VM. 

## **Migrating with Nutanix Image Service** 

The Nutanix Image Service feature imports images (ISO files, disk images, or any images supported in ESXi or Hyper-V) directly into AHV for virtualization management. Nutanix supports the RAW, VHD(X), VMDK, VDI, ISO, and QCOW2 disk formats. 

The Nutanix Image Service is an excellent tool for testing VM migrations and performing small-scale or staged migrations to Nutanix AHV. The ability to perform a storage vMotion from ESXi helps eliminate downtime while migrating data. The conversion process from *-flat.vmdk to RAW is fast (just a few seconds), because it doesn't require additional copies of the virtual disk. 

For migration, you can use this feature to convert virtual disks to the RAW format that AHV uses. As an example, the image service converts the flat virtual disks (*-flat.vmdk) that ESXi uses to RAW. 

To minimize downtime, a storage vMotion can use NFS connectivity to move the ESXi virtual disks to the Nutanix cluster. After the virtual disks are on AOS storage, you can import the disk by pointing the image service to the migrated files using an NFS source URL and a loopback address. 

© 2025 Nutanix, Inc. All rights reserved  | **17** 

Migrating VMs to Nutanix AHV 

You can also import virtual disks directly from an external HTTP or NFS source URL or upload them from your local machine. The import process creates a RAW virtual disk and stores it as an image. You can then create a new VM set to reference this stored disk image. 

The general migration process uses the following steps: 

**1.** Ensure that you meet the VM migration prerequisites. 

**2.** Install Nutanix VM Mobility drivers or validate that the VirtIO driver modules are installed. 

**3.** Migrate the VM disks to AOS storage. 

**4.** Use the image service to convert the VM disks. 

**5.** Create a new VM and attach the converted disks. 

The previous steps are manual, which makes using the image service for large-scale migrations potentially cumbersome. Virtual hardware changes during conversion, so you must manage network settings (such as static IP addresses) within VMs after the migration. 

## **Migrating with Sureline** 

Sureline Systems offers a migration solution for heterogeneous environments. SUREedge Migrator is a Nutanix Ready solution that supports migrating operating systems and data from any platform to Nutanix, with the option to use AHV. Nutanix Services has validated SUREedge for migrating to AHV. 

SUREedge is a scalable solution that helps simplify migrating large environments to AHV. Migration orchestration, which lets you modify system images and network addresses, eliminates additional steps that you would otherwise need to complete manually after migration. 

To use this solution, follow these steps: 

**1.** Install SUREedge Migrator on a node in the Nutanix cluster. 

**2.** Add systems, including physical hosts and VMs, that are targeted for migration. 

**3.** Create plans to target specific systems to migrate. 

These plans also run migrations. 

© 2025 Nutanix, Inc. All rights reserved  | **18** 

Migrating VMs to Nutanix AHV 

SUREedge captures system images that you can modify as a part of the migration by changing the memory, CPU, or network, including IP addresses. SUREedge injects the VirtIO drivers as required so the migrated operating system can discover the AHV-based virtual hardware. SUREedge replicates incremental changes that you can apply before migration, minimizing downtime on the final cutover. SUREedge also supports testing before migration, which helps minimize risk. 

Sureline Systems is a Nutanix Technology Alliance partner. SUREedge is a third-party product that requires separate licensing outside Nutanix. 

## **Application-Centric Migration** 

Many migration methods focus on moving an operating system or VM between environments. Some application-specific methods can move data without moving the operating system itself. The general process for this data migration involves installing a VM on Nutanix AHV along with a new installation of the application that you plan to migrate. You can then use native application-based migration, backup and restore, or disaster recovery methods to move data from the source system to the AHV target. 

Application-centric migration options enable movement between heterogeneous environments. Differences between environments can include the underlying hardware, virtualization layer, operating system versions, and application versions. You can generally stage, test, and run migrations for specific users, files, and databases as required. Many migration options are free or included as a part of the base licensing of the application. 

In some cases, you can migrate to a newer version of the application running on an updated operating system. Examples of such upgrades include the following solutions: 

- Microsoft Exchange Server: Install newer versions alongside existing production environments, then move user mailboxes from the old system to the new one. 

- Microsoft SQL Server: Back up databases and restore them between systems, including between older and newer SQL Server versions. Always On availability groups can also replicate databases between SQL Server instances, not only to migrate data but also to implement a high-availability solution at the same time. 

- File shares: Copy files between a source and a target with a tool like Robocopy to run a migration. In environments that use Microsoft Distributed File System (DFS), you 

© 2025 Nutanix, Inc. All rights reserved  | **19** 

Migrating VMs to Nutanix AHV 

can add new servers to a DFS namespace and use them to host new copies of DFS replicas. After DFS performs the replication, you can remove the replicas from the old systems. 

Application-centric migrations can add complexity to the overall migration process. Each application has different migration requirements and processes, including networking configuration and user or application connectivity. In some cases, existing application licensing levels might not include the feature you want to use for the migration. If you don't need a newer operating system or application version, migrating operating systems or VMs instead might ultimately be a simpler process. 

© 2025 Nutanix, Inc. All rights reserved  | **20** 

Migrating VMs to Nutanix AHV 

## 5. Nutanix ESXi Cluster to Nutanix AHV Migration 

Native replication tools can help simplify migrations that involve Nutanix platforms as both the source and target. In addition to following the Sureline and application-centric methods described in the Third-Party ESXi to Nutanix AHV Migration section, you can also choose to migrate through Nutanix VM Mobility or cluster conversion. 

## **Migrating with Nutanix VM Mobility** 

Nutanix VM Mobility provides a simplified method for replicating Windows- and Linux-based VMs bidirectionally between a Nutanix ESXi cluster and a Nutanix AHV cluster. Nutanix VM Mobility uses native Nutanix snapshots to replicate data between clusters. AOS provides Nutanix Guest Tools (NGT) to install the appropriate drivers and communicate with the Nutanix cluster to confirm mobility support. If your existing ESXi environment is Nutanix-based, VM Mobility is a good option for performing migrations to AHV at any deployment size. 

The ability to test the conversion process without impacting production also makes VM Mobility a strong option. You can discover any potential issues and address them during the final cutover process. Using integrated orchestration to perform testing or final migration cutover simplifies the process and minimizes risk. You can stage migrations by grouping a subset of the total environment into a protection domain that you can replicate and subsequently migrate as needed. 

VM Mobility maintains data replication between clusters incrementally, which helps limit downtime on final cutover. VM conversion, which includes converting virtual disks, is a very fast process, measured in a few seconds per VM. VM Mobility converts VMs in batches of 10 during the cutover process. 

The mobility process includes the following high-level steps: 

**1.** Enable and install NGT for the VMs you want to replicate. 

© 2025 Nutanix, Inc. All rights reserved  | **21** 

Migrating VMs to Nutanix AHV 

**2.** Create a remote site in each Nutanix cluster. 

This step includes network mapping between the AHV and ESXi networks. 

**3.** Create an asynchronous protection domain and schedule it to replicate the required VMs. 

**4.** Move or create VMs using one of several workflow options: 

   - For planned mobility, use the migrate option to move all VMs in a protection domain. This option unregisters the VMs from the source cluster, replicates any incremental changes since the last snapshot, and registers the VMs in the target cluster. 

   - For unplanned mobility, where a source cluster is offline, the activate option moves all VMs in a protection domain. This option registers the VMs in a protection domain in the target cluster using the most recently available local snapshot. 

   - The snapshot restore option clones individual VMs. A snapshot restore to a new location operates as a clone and registers the VM in the target cluster. This option enables test and development scenarios and allows you to target individual VMs in a protection domain. 

Requirements and limitations: 

- You must install NGT so that the appropriate drivers are available in the VM. This one-time operation allows communication between the Controller VM (CVM) and the Nutanix cluster to validate driver installation. 

- ESXi delta disks aren't supported. 

- VMs with SATA or PCI devices aren't supported. 

- Virtual hardware changes during conversion, so you must manage network settings, such as static IP addresses, in the VMs after the migration. 

Prism sends an alert if VM Mobility can't convert specific VMs because of delta disks or virtual hardware. In Prism, you can find these recovery details under the VM Recovery column of a local or remote snapshot. Also, because a VM targeted for conversion must have VM Mobility drivers installed, Prism generates a warning if the installation status is unknown. 

© 2025 Nutanix, Inc. All rights reserved  | **22** 

Migrating VMs to Nutanix AHV 

For additional details, including instructions on how to use Nutanix VM Mobility, see the Prism Web Console Guide. 

## **Migrating with Cluster Conversion** 

Nutanix offers a native method for performing an in-place conversion of an existing Nutanix cluster from ESXi to AHV. You can also convert a cluster from AHV to ESXi if you previously converted it from ESXi. This cluster conversion process preserves existing data, hypervisor networking settings, and VM settings. 

Cluster conversion is a good choice for repurposing existing Nutanix-qualified hardware currently running ESXi to run AHV. You don't need to replicate data or have another Nutanix cluster available. 

Requirements and limitations: 

- You must install and enable Nutanix Guest Tools (NGT) to ensure that the proper drivers are available. 

- You must enable ESXi High Availability (HA) and Distributed Resource Scheduler (DRS). 

- An ESXi cluster supports only one external virtual switch. 

- All uplinks must be homogenous (that is, they must have the same adapter speed—all 10 Gb or all 1 Gb). 

- LACP-based virtual switch load balancing isn't supported. 

- You can't convert VMs with delta disks from ESXi to AHV. 

- You must set the Microsoft Windows SAN policy to **OnlineAll** before migrating a Windows Server VM to AHV. For more information, see Nutanix KB-4479. 

The general conversion process is as follows: 

**1.** Select **Convert Cluster** from Prism Element. 

**2.** Select the target hypervisor and VM boot options. 

You can choose to keep the original power state of the VMs following conversion or turn off VMs before conversion. 

© 2025 Nutanix, Inc. All rights reserved  | **23** 

Migrating VMs to Nutanix AHV 

**3.** Cluster validation determines whether the cluster meets requirements or if limitations exist. 

**4.** Prism displays the following warnings when applicable: 

   - If an existing active-active network team is going to become active-passive on conversion 

   - If specific VMs don't have NGT enabled, preventing confirmation of VM Mobility driver installation 

**5.** If no blocking limitations exist, the conversion process proceeds. 

**6.** After the conversion begins, the following high-level steps occur: 

   - **a.** The conversion process collects and saves hypervisor information. 

The cluster remains manageable during the conversion process. 

- **b.** Guest VMs live migrate from the node targeted for conversion to other nodes in the cluster. 

- **c.** The process converts the node evacuated in the previous step to the targeted hypervisor. 

- **d.** The process restores guest VMs to the newly converted node one by one. 

Each VM is then also converted. Running VMs experience downtime similar to the duration of one power cycle (turn off and turn on). 

- **e.** After the targeted node and all original VMs are converted, the process moves to the next node. 

- **f.** When all nodes are using the targeted hypervisor, the conversion is complete. 

For additional details, including instructions for using cluster conversion, see the Prism Web Console Guide. 

Although safeguards are in place to ensure a successful conversion, you can't test to validate the process before proceeding. Conversion currently targets the entire cluster; working with the whole cluster affects the ability to apply a phased approach to the migration. 

© 2025 Nutanix, Inc. All rights reserved  | **24** 

Migrating VMs to Nutanix AHV 

## 6. AWS to Nutanix AHV Migration 

Migrating VMs from AWS is similar to migrating from other sources, but the technical implementation is somewhat different. 

Migrating VMs out of AWS involves additional AWS costs. AWS charges a per-gigabyte rate for transferring data out of their storage. For example, if the AWS transfer charge is $0.09 per GB, moving a 1 TB VM out of AWS costs approximately $92 for the data transfer alone. 

Nutanix Move launches the move-agent as one t2.micro VM instance per region. While migrating VMs, data transfer from AWS to AHV occurs through the move-agent. Therefore, if source VMs are hosted in a different availability zone than the move-agent, the AWS regional data transfer cost applies. For example, if AWS charges $0.01 per GB for crossing a region boundary, moving a 1 TB VM between regions incurs a regional data transfer cost of $10.24. 

Nutanix Move automatically launches an EC2 instance for the move-agent VM (t2.micro) when a migration plan involves AWS. Because this EC2 instance runs until the migration is complete, we must also factor in its cost. If Move has future AWS migrations scheduled, it leaves the EC2 instance running but terminates it when all migration plans involving AWS are complete or removed. 

The data transfer rate across AWS to AHV might vary greatly because of factors like WAN delay. The following table assumes that you transfer a 1 TB VM from AWS to AHV at the conservative WAN speed of 50 Mbps. 

_Table: Example Cost Estimate for Migrating from AWS_ 

|**Details**|**Cost (USD)**|
|---|---|
|AWS: internet data transfer out ($0.09 per GB)|$92.16|
|AWS: regional data transfer cost ($0.01 per|$10.24|
|GB). If the move-agent VM (t2.micro) and||
|source VM are in the same region (which||
|they usually are), this cost is zero. We have||
|included this figure to demonstrate what the||
|cost impact of different regions could be.||



© 2025 Nutanix, Inc. All rights reserved  | **25** 

Migrating VMs to Nutanix AHV 

|**Details**|**Cost (USD)**|
|---|---|
|AWS: EC2 cost for move-agent (t2.micro) VM<br>(approximately $2 per day). With an estimated<br>average throughput of 50 Mbps, migration<br>takes about 48 hours (2 days) to complete.<br>Total|$4.00<br>$106.40|



**Note:** The figures in the previous table are example costs. Verify current AWS charges before migrating. 

© 2025 Nutanix, Inc. All rights reserved  | **26** 

Migrating VMs to Nutanix AHV 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2025 Nutanix, Inc. All rights reserved  | **27** 

## **List of Figures** 

Figure 1: Nutanix Move Architecture..............................................................................................................9 Figure 2: Nutanix Move for ESXi Architecture.............................................................................................13 Figure 3: Nutanix Move for Hyper-V Architecture........................................................................................14 Figure 4: Nutanix Move for AWS Architecture.............................................................................................15 Figure 5: Nutanix Move for Azure Architecture............................................................................................16 


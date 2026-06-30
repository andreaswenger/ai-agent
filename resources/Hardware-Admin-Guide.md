# **NX Series Hardware Administration Guide** 

**Platform ANY June 11, 2026** 

## **Contents** 

**System Specifications and Hardware Information.................................. iv Firewall Port Requirements for IPMI........................................................v Product Mixing Restrictions................................................................... vi Mixing Nutanix Nodes in a Cluster............................................................................................ xii Visio Stencils........................................................................................ xv** 

**==> picture [498 x 423] intentionally omitted <==**

**----- Start of picture text -----**<br>
|||
|---|---|
|1. Manual Firmware Updates................................................................|16|
|Shutting Down a Single-node Cluster....................................................................................... 16|
|Node Shutdown Precheck...............................................................................................16|
|Preparing a Single Node for Shutdown...........................................................................17|
|Shutting Down a Single Node (vSphere Client)............................................................... 17|
|Shutting Down a Single Node (AHV)............................................................................... 18|
|Shutting Down a Node in a Multinode Cluster.......................................................................... 19|
|Node Shutdown Precheck...............................................................................................19|
|Preparing Nodes for Shutdown.......................................................................................20|
|Shutting Down a Node in a Cluster through the vSphere Client.......................................20|
|Shutting Down a Node in a Cluster through the Command Line...................................... 21|
|Shutting Down a Node in a Cluster (AHV).......................................................................22|
|Shutting Down a Node in a Cluster (Hyper-V)................................................................. 24|
|Manually Updating M.2 RAID Hypervisor Boot Drive Firmware.................................................|25|
|Manually Updating NVMe Drive Firmware.................................................................................32|
|Manually Updating Data Drive Firmware...................................................................................38|
|Manually Updating the BMC and BIOS......................................................................................44|
|Manually Updating HBA Controller Firmware (G6 and G7 Platforms).........................................44|
|Manually Updating HBA Controller Firmware (G8 and Newer Platforms)...................................49|
|Manually Updating NIC Firmware............................................................................................. 54|
|Starting a Single-node Cluster................................................................................................. 55|
|Starting a Single Node (vSphere Client).........................................................................|55|
|Starting a Single Node (AHV)......................................................................................... 56|
|Node Startup Post-Check............................................................................................... 57|
|Starting a Node in a Multinode Cluster.....................................................................................58|
|Starting a Node in a Cluster through the vSphere Client.................................................58|
|Starting a Node in a Cluster (AHV)................................................................................. 60|
|Starting a Node in a Cluster (Hyper-V)........................................................................... 61|
|Node Startup Post-Check............................................................................................... 63|
|2. Changing an IPMI IP Address............................................................64|
|Configuring the Remote Console IP Address (IPMI Web Interface)...........................................|65|
|Configuring the Remote Console IP Address (Command Line)..................................................66|
|Configuring the Remote Console IP Address (BIOS).................................................................68|

**----- End of picture text -----**<br>


**ii** 

**3. Changing the IPMI Password.............................................................70 Allowed Special Characters in IPMI Passwords........................................................................71 Changing the IPMI Password for ESXi...................................................................................... 71 Changing the IPMI Password for Hyper-V.................................................................................72 Changing the IPMI Password for AHV.......................................................................................73 Changing the IPMI Password without Operating System Access...............................................73 4. Using the Rescue Shell..................................................................... 74 Creating the Controller VM Recovery Image (Hyper-V)............................................................. 74 Launching the Recovery Shell.................................................................................................. 74 Rescue Shell Commands..........................................................................................................76 Exiting the Rescue Shell...........................................................................................................76 5. Adding a Drive..................................................................................78 6. Remote Direct Memory Access......................................................... 82 7. CMOS Battery Replacement..............................................................86 8. Memory Configurations..................................................................... 87 Supported Memory Configurations...........................................................................................87 9. Host Secure Boot (UEFI)...................................................................88 Host Secure Boot Overview..................................................................................................... 88 G8 Platforms: Enabling Host Secure Boot (Redfish API Method)...............................................89 G8 Platforms: Enabling Host Secure Boot (BIOS Method).........................................................91 G8 Platforms: Verifying Enabled Host Secure Boot Settings With the Redfish API or BIOS.........93 Verifying Enabled Host Secure Boot Settings in the Redfish API..................................... 93 G9 Platforms: Enabling Host Secure Boot Using IPMI raw Commands...................................... 95 G10 Platforms: Enabling Host Secure Boot.............................................................................. 96 All Platforms: Imaging with Foundation on a Host Secure Boot Enabled Node...........................98 All Platforms: Secure Boot Error Handling and Failure Conditions............................................98 All Platforms: Verifying Enabled Host Secure Boot Settings with the Hypervisor or CVM...........99 G8 Platforms: Disabling Host Secure Boot............................................................................. 100 Verifying the Disabled Host Secure Boot Settings in the Redfish API (Optional).............102 Confirming the Disabled Secure Boot Status from the Hypervisor and CVM...................104 G9 Platforms: Disabling Host Secure Boot Using IPMI raw Commands................................... 104 G10 Platforms: Disabling Host Secure Boot............................................................................105 10. Intel SST Performance Profile....................................................... 107 The Intel SST Performance Profile..........................................................................................107 Managing the Intel SST Performance Profile Through IPMI..................................................... 107 Managing the Intel SST Performance Profile Through the BIOS.............................................. 108 Copyright............................................................................................111** 

**iii** 

## **SYSTEM SPECIFICATIONS AND HARDWARE INFORMATION** 

Find system specifications. 

For system specifications and other platform-specific hardware information, consult the Nutanix portal at this link: System specifications. 

Select your NX generation to see the system specifications. 

## **FIREWALL PORT REQUIREMENTS FOR IPMI** 

For port information, see the _Ports and Protocols_ document at this link: Ports and Protocols. You can also visit the Nutanix Support Portal and enter "Ports and Protocols" in the search bar. 

Platform | Firewall Port Requirements for IPMI | **v** 

## **PRODUCT MIXING RESTRICTIONS** 

Check hardware and software restrictions for Nutanix platforms and clusters. 

**Caution:** Do not configure a cluster that violates any of the following rules. 

## **Compatibility** 

The Nutanix portal includes a compatibility matrix. From the drop-down menu, select **Compatibility and Interoperability Matrix** . Filter and display compatibility by Nutanix NX model, AOS release, hypervisor, or feature (platform and cluster intermixing). 

Nutanix recommends that you consult the compatibility matrix before installing or upgrading software on your cluster. 

Standard SuperMicro tools might not work correctly on Nutanix systems. Only use tools provided by Nutanix. If you need more information or assistance, contact Nutanix Support. 

## **Hardware Restrictions** 

- Platforms that use AMD CPUs cannot mix in a cluster with platforms that use Intel CPUs. 

- Nutanix clusters support N+3 and N-3 generations of Nutanix platforms: 

   - You can mix G6 nodes in a cluster with G7, G8/N-G8, or G9 platforms, but not with platforms later than G9. 

   - You can mix G7 nodes in a cluster with G6, G8/N-G8, G9, or G10 platforms. 

   - You can mix G8 or N-G8 nodes in a cluster with G6, G7, G9, or G10 platforms. 

   - You can mix G9 nodes in a cluster with G6, G7, G8/N-G8, or G10 platforms, but not with platforms earlier than G6. 

   - You can mix G10 nodes in a cluster with G7, G8/N-G8, or G9 platforms, but not with platforms earlier than G7. 

- You can mix nodes that use different CPU families in the same _cluster_ , but not in the same _block_ . For example, a cluster can contain nodes from any NX generation, but a G8 block must contain only G8 or N-G8 nodes, a G9 block must contain only G9 nodes, and so on. 

- You can mix G8 and N-G8 nodes in the same block. For example, you can mix NX-1065-G8 and NX-1065N-G8 nodes in the same block. This rule applies to all NX multi-node platforms. 

- NX nodes that use different types of storage can mix in the same cluster only according to certain guidelines. See Mixing Nutanix Nodes in a Cluster on page xii. 

- Nutanix does not support mixing nodes from different hardware vendors in the same cluster. However, you can manage separate clusters using the Prism web console regardless of the hardware type. 

Platform | Product Mixing Restrictions | **vi** 

- For hardware made by vendors other than Nutanix, you can mix nodes from the same hardware vendor product series in the same cluster. 

Consider the following mixing examples. 

   - You can mix Cisco UCS (B and C Series) nodes in the same cluster. 

   - You cannot mix Cisco UCS (B and C Series) nodes and Nutanix NX series nodes in the same cluster. 

   - You cannot mix Dell PowerEdge and Dell XC series nodes in the same cluster. 

   - You cannot mix Intel nodes and AMD nodes in the same cluster. 

   - You can mix HPE DX and non-DX HPE HFCL-certified platforms in the same cluster (generation 10 platforms only). 

- Nutanix does not support mixing nodes with RDMA enabled and nodes without RDMA enabled in the same cluster. 

- Nodes in the same block can join different clusters. For example, in a four-node block, three nodes can form part of cluster A while the fourth node is part of cluster B. When considering such a configuration, remember that any maintenance procedure (such as chassis replacement) that involves shutting down power to the entire block can result in downtime across multiple clusters. 

## **Operating System Restrictions** 

All Controller VMs in a cluster must use the same version of AOS. 

## **CPU Restrictions** 

All CPUs in a block must be identical. When adding a node to a multi-node block, make sure that the new node and the existing nodes use identical CPUs. 

## **GPU Restrictions** 

All graphics processing unit (GPU) cards in the same platform must be identical. Nutanix does not support mixing different types of GPU cards in the same platform. For example, the NX-3155G-G8 platform supports both A-series and T-series NVIDIA GPU cards, but you cannot put both types of GPU card in the platform at the same time. 

## **NIC Restrictions** 

- For NX G6, G7, G8, and G9 platforms, intermixing of add-on NICs is not supported. All add-on NICs in a node must be identical. N-G8 and G10 platforms support NIC intermixing according to the following rules. 

- For N-G8 platforms, the following rules apply: 

   - Multi-node platforms that do not have an AIOM card support intermixing between a Base-T and Optical NIC. The NICs do not have to be identical. 

   - Multi-node platforms that have an AIOM card do not support intermixing of add-on NICs. All add-on NICs in a node must be identical. 

   - Single-node platforms have a REQUIRED 2-port 10GBase-T NIC installed. If any add-on NICs are installed, they do not have to be 10GBase-T NICs. However, if more than one add-on NIC is installed, the add-on NICs must be identical. 

Platform | Product Mixing Restrictions | **vii** 

- For G10 platforms, the following rules apply. 

   - NX G10 platforms include an on-board AIOM LOM. What degree of add-on NIC mixing the platform allows depends on which of two supported on-board AIOM LOMs is present. 

   - **Table 1: NIC Intermixing in G10 Platforms** 

|**AIOM LOM card**|**NX-1175S-G10**|**NX-3035-G10,**<br>**NX-3060S-G10**|**NX-8150-G10,**<br>**NX-8150G-G10,**<br>**NX-8155A-G10,**<br>**NX-8170-G10,**<br>**NX-8170A-G10**|
|---|---|---|---|
|Broadcom 57416|NIC mixing is not<br>supported.|The following NICs<br>can be mixed:<br>•<br>SuperMicro dual-<br>port 10G Base-T<br>•<br>Mellanox CX6 25<br>GbE|The following NICs can<br>be mixed:<br>•<br>SuperMicro dual-<br>port 10G Base-T<br>•<br>Mellanox CX6 25<br>GbE|
|Intel X710|NIC mixing is not<br>supported.|The following NICs<br>can be mixed:<br>•<br>Intel X710 quad-<br>port 10G Base-T<br>•<br>Mellanox CX6 25<br>GbE|The following NICs can<br>be mixed:<br>•<br>Intel X710 quad-<br>port 10G Base-T<br>•<br>Mellanox CX6 25<br>GbE|



- Nutanix does not manufacture or provide support for long-range SFP+ transceivers or any active direct attach cables. For more details, see KB 13531. 

## **Fan Restrictions** 

- Nutanix does not support fan redundancy on any platform. Replace failed fans immediately. 

## **Mixing Drive Capacities** 

Nutanix allows mixing of drives with different capacities in the same node, for replacement cases where only higher-capacity drives are available. For details, see KB 10683. 

**Note:** The node treats the higher-capacity drives as if they had the same capacity as the lower-capacity drives. To increase the overall storage capacity of the node, replace all drives with higher-capacity drives. 

## **Storage Restrictions** 

- Do not move drives from one node to another. 

- NX nodes that use different types of storage can mix in the same cluster only according to certain guidelines. See Mixing Nutanix Nodes in a Cluster on page xii. 

- High-capacity NVMe storage tier (NST) drives cannot mix in a node with SSD or HDD drives. You cannot configure a node with only NST drives. The only supported configuration for NST drives is as part of an NVMe + NST node. 

Platform | Product Mixing Restrictions | **viii** 

- If you configure a cluster and later disassemble the chassis (for example, to move the hardware to a different site), then when you reassemble the chassis, you must replace all drives in their original nodes. If you move a drive from one node to another, cluster services may not come back up correctly. 

- If your cluster contains nodes that have significant differences in SSD capacity, there can be cases where you encounter storage issues when downsizing a cluster. An example is the case of a five-node cluster where one node has high-capacity SSDs and all the other nodes have relatively low-capacity SSDs. If you remove the high-capacity node from the cluster, the other nodes may not have enough space to migrate all the data, and the system gives you a `no space available` warning. 

- For the NX-8155A-G9 platform, NVMe drives are supported with AHV version 10.0 or later. NVMe support is not available with ESXi. 

## **Encryption Restrictions** 

- Encrypted drives (SED) can be mixed with unencrypted (non-SED) drives in the same node, if encryption has never been enabled. 

Encrypted nodes can be mixed with unencrypted nodes in the same cluster if encryption was never enabled and remains disabled. 

- Nutanix platforms earlier than G9, or platforms using a version of AOS earlier than 7.3, do not support self-encryption for NVMe drives. 

- G9 platforms support self-encryption (SED) for NVMe drives under the following conditions. 

   - Intel-based single-node platforms only 

   - AOS 7.3 or later 

   - NX-1150S-G9 and NX-1175S-G9 platforms support NVMe SED with either Sapphire Rapids or Emerald Rapids CPUs. All other platforms support NVMe SED only with Emerald Rapids CPUs. 

   - **Table 2: NX G9 platforms that support NVMe SED** 

|**Platform**|**CPU Family Required for NVMe SED**|
|---|---|
|NX-1150S-G9|Sapphire Rapids or Emerald Rapids|
|NX-1175S-G9|Sapphire Rapids or Emerald Rapids|
|NX-3155-G9|Emerald Rapids only|
|NX-8150-G9|Emerald Rapids only|
|NX-8155-G9|Emerald Rapids only|
|NX-8170-G9|Emerald Rapids only|



- G10 platforms support self-encryption (SED) for NVMe drives on Intel-based platforms only. 

## **DIMM Restrictions** 

## DIMM types 

For all platforms: within a node, all DIMMs must be of the same type. For example, you cannot mix RDIMMs and LRDIMMs in the same node. 

## DIMM capacity 

For all platforms: within a node, all DIMMs must have the same memory capacity. For example, you cannot mix 16 GB and 32 GB DIMMs in the same node. 

Platform | Product Mixing Restrictions | **ix** 

## DIMM manufacturers 

For G9 and G10 platforms, all DIMMs in a node must come from the same manufacturer. 

For G6, G7, G8, and N-G8 platforms, Nutanix supports mixing DIMMs from different manufacturers within the same _node_ , but not within the same _channel_ . 

- DIMM slots are arranged on the motherboard in groups called _channels_ . 

   - On G6 and G7 platforms, all channels contain two DIMM slots (one blue and one black.) 

   - On G8 and G9 platforms, channels contain either one DIMM slot (blue) or two DIMM slots (one blue and one black.) 

- Within a channel, all DIMMs must be from the same manufacturer. 

- When replacing a failed DIMM, ensure that you are replacing the original DIMM like-for-like. 

- When adding new DIMMs to a node, if the new DIMMs and the original DIMMs are from different manufacturers, arrange the DIMMs so that the original DIMMs and the new DIMMs are not mixed in the same channel. 

**Note:** You do not need to balance numbers of DIMMs from different manufacturers within a node, so long as you never mix them in the same channel. 

## DIMM speed 

- Nutanix platforms up to and including G8 and N-G8 use DDR4 DIMMs, whose speed is measured in _megaHertz_ (MHz). 

- Nutanix G9 and G10 platforms use DDR5 DIMMs, whose speed is measured in _megatransfers per second_ (MT/s). 

**Note:** DDR5 128 GB 5600 MT/s DIMMS require BIOS E _`x`_ 30.001 or later, and cannot mix in the same node with 4800 MT/s DIMMs. 

Nutanix supports higher-speed replacement DIMMs. You can mix DIMMs that use different speeds in the same node, and in the same memory channel, under the following conditions. 

- When you mix DIMMs of different speeds in the same node, your system operates at the lowest common DIMM speed or CPU supported frequency. 

- You can only use higher-speed replacement DIMMs from one NX generation later than your platform. 

Platform | Product Mixing Restrictions | **x** 

- You can mix DIMMs that use different speeds in the same node but not in the same channel. Within a channel, Nutanix supports only the following mixes of DIMM speeds. 

   - G6 platforms: 

      - Samsung 2666 MHz B-die and Samsung 2666 MHz C-die 

      - Samsung 2666 MHz C-die and Samsung 2933 MHz C-die 

      - Samsung 2666 MHz B-die and Samsung 2933 MHz C-die 

   - G7 platforms: 

      - Samsung 2933 MHz 32 GB and Samsung 3200 MHz 32 GB 

      - Samsung 2933 MHz 64 GB and Samsung 3200 MHz 64 GB 

      - Micron 2933 MHz 32 GB and Micron 3200 MHz 32 GB 

      - Micron 2933 MHz 64 GB and Micron 3200 MHz 64 GB 

   - G8 platforms use DDR4 DIMMs and G9 platforms use DDR5 DIMMs, so you cannot mix higher-speed G9 DIMMs in any G8 or NG8 platform. 

   - G9 platforms: 

      - G9 platforms that use Intel Sapphire Rapids processors ship with 4800 MT/s DIMMs. 

      - G9 platforms that use Intel Emerald Rapids processors ship with 5600 MT/s DIMMs. Supported DIMM speed depends on the CPU class and on whether you have installed one DIMM per memory channel (1DPC) or two DIMMs per memory channel (2DPC). 

         - Platinum-8xxx: max 5600 MT/s at 1DPC; max 4400 MT/s at 2DPC. 

         - Gold-6xxx: max 5200 MT/s at 1DPC; max 4400 MT/s at 2DPC. 

         - Gold-5xxx: max 4800 MT/s at 1DPC; max 4400 MT/s at 2DPC. 

         - Silver-4xxx: max 4400 MT/s at both 1DPC and 2DPC. 

         - Bronze-3xxx (single socket boards only): max 4400 MT/s at both 1DPC and 2DPC. 

      - If you install a 5600 MT/s DIMM in a G9 platform that uses Intel Sapphire Rapids processors, it runs at a max of 4800 MT/s. 128 GB 5600 MT/s DIMMs cannot mix in the same node with 4800 MT/s DIMMs. 

   - G10 platforms ship with 6400 MT/s DIMMs. This is the highest DIMM speed Nutanix currently supports. 

- Certain CPU types put limits on DIMM speed. 

|**NX Generation**|**CPU**|**Max DIMM Speed**|
|---|---|---|
|G6|Intel Xeon Silver_4116|2400 MHz|
|G7|Intel Xeon Silver_4208|2400 MHz|
|G8 / NG8|Intel Xeon Silver_4309Y|2666 MHz|



- On G7 platforms, full DIMM population puts limits on DIMM speed. When fully populated with 24 DIMMs, the maximum DIMM speed is 2666 MHz. 

Platform | Product Mixing Restrictions | **xi** 

## **Hypervisor Restrictions** 

All nodes in a cluster must use the same hypervisor type and version. This restriction does not apply if the cluster contains storage-only nodes. 

Storage-only nodes always run AHV, but you can add them to clusters that run on other hypervisors. With Nutanix Foundation version 4.0 and later, any node can act as a storage-only node. 

**Note:** Citrix Hypervisor nodes do not support storage-only nodes. 

- If you expand a cluster by adding a node with older generation hardware to a cluster that was initially created with later generation hardware, power cycle any guest VMs (a guest reboot is not sufficient) before migrating them to the added older generation node or before upgrading the cluster. 

Guest VMs are migrated during hypervisor and firmware upgrades (but not AOS upgrades). 

For example, if you are adding a node with G8 Icelake CPUs to a cluster that also has newer G9 nodes with Sapphire Rapids or Emerald Rapids CPUs, you must power cycle the guest VMs hosted on the G9 nodes before you can migrate the VMs to the node with G8 CPUs. Power cycling the guest VMs enables them to discover G8 processor changes. 

Power cycle guest VMs from the Prism web console VM dashboard. Do not perform a guest reboot; this case requires a VM power cycle. 

## **vSphere Restrictions** 

- Nutanix supports mixing nodes with different processor architectures in the same cluster. However, vSphere only supports live vMotion of VMs from one type of node to another when you have enhanced vMotion compatibility (EVC) enabled. For more information about EVC, see the vSphere 5 documentation and the following VMware knowledge base articles: 

   - _Enhanced vMotion Compatibility (EVC) Processor Support_ [1003212] 

   - _EVC and CPU Compatibility FAQ_ [1005764] 

Enabling EVC on an existing vCenter cluster requires shutting down all VMs. Because EVC is a vSphere cluster setting, it applies to all hosts at the same time. For these reasons, adding a node with a different processor type to a vCenter cluster requires shutting down the Nutanix cluster. 

- Clusters are block-aware only under certain conditions. For complete explanation of the requirements, see Block Fault Tolerance in the _Prism Web Console Guide_ . 

## **Mixing Nutanix Nodes in a Cluster** 

Rules for mixing different types of NX nodes in a cluster. 

**Note:** The rules in this chapter apply to storage drives. Boot drive configurations are set at the factory and are not configurable. 

Nutanix offers three storage media tiers: HDD, SSD, and NVMe. Nutanix does not support mixing all three tiers in the same cluster. Nutanix only allows clusters of two media tiers or fewer. 

**Note:** With versions of AOS 6.5.3 or later, Nutanix allows an exception to the two-media-tiers restriction: you can expand an existing cluster that contains hybrid SSD + HDD nodes by adding hybrid NVMe + HDD nodes (two nodes minimum). This is the only exception. 

Nutanix currently offers the following types of NX nodes. 

Platform | Product Mixing Restrictions | **xii** 

**Table 3: NX node types** 

- Hybrid SSD + HDD 

- All-SSD 

- Mixed SSD + NVMe 

- All-NVMe 

- Hybrid NVMe + HDD 

The safest way to leverage a new node type is to create a new cluster and migrate the workload from the existing cluster into the new cluster. If creating a new cluster and migrating the workload is not feasible, you can add a new node type to an existing cluster, following the guidelines in this chapter. 

To mix two types of media, you must start with a cluster that contains only one type of media, and then add a new node that contains a different media type to that cluster. You cannot create a cluster with more than one type of media right from cluster creation. 

Cluster expansion must follow the correct order. For example, adding hybrid NVMe + HDD nodes to an existing cluster of hybrid SSD + HDD nodes is supported, but adding hybrid SSD + HDD nodes to an existing cluster of hybrid NVMe + HDD nodes is not supported. 

When mixing different node types in a cluster, the minimum number of each node type must be equal to the cluster redundancy factor. For example, clusters with a redundancy factor of 2 must have a minimum of two nodes of each node type that is present in the cluster. 

A cluster that contains different node types operates at the capabilities of the lowest media tier. 

## **Clusters that Contain Hybrid Nodes** 

Clusters containing hybrid SSD + HDD nodes, or hybrid NVMe + HDD nodes, have the following restrictions. 

**Table 4: Mixing that involves hybrid nodes** 

|**If you have an existing**<br>**cluster containing this type**<br>**of node**|**Then you can add one of these node types to that cluster**|
|---|---|
|Hybrid SSD + HDD|•<br>Hybrid SSD + HDD, or<br>•<br>All-SSD, or<br>•<br>Hybrid NVMe + HDD (requires AOS 6.5.3 or later)|
|Both hybrid SSD + HDD and<br>all-SSD|•<br>All-SSD|
|Both hybrid SSD + HDD and<br>hybrid NVMe + HDD|•<br>Hybrid NVMe + HDD|
|Hybrid NVMe + HDD|•<br>Hybrid NVMe + HDD|



**Caution:** Once you have expanded a hybrid SSD + HDD cluster with an all-SSD node, all future nodes you add to that cluster must be all-SSD. 

Platform | Product Mixing Restrictions | **xiii** 

**Caution:** There is no migration path from a hybrid SSD + HDD cluster to an all-NVMe cluster other than full cluster-to-cluster migration. 

**Caution:** Hybrid SSD + HDD nodes that contain only one SSD cannot mix in clusters that contain all-SSD nodes. 

## **Clusters that Do Not Contain Hybrid Nodes** 

Clusters that do not contain hybrid nodes have the following restrictions. 

**Table 5: Mixing that does not involve hybrid nodes** 

|**If you have an existing**<br>**cluster containing any one**<br>**or more of these types of**<br>**nodes in any combination**|**Then you can add one of these node types to that cluster**|
|---|---|
|•<br>All-SSD<br>•<br>Mixed SSD + NVMe<br>•<br>All-NVMe|•<br>All-SSD, or<br>•<br>Mixed SSD + NVMe, or<br>•<br>All-NVMe|



**Caution:** In a mixed SSD + NVMe node, the NVMe drives must have the same capacity as the SSDs. 

## **VISIO STENCILS** 

To find Visio stencils for Nutanix products, visit VisioCafe at this link: VisioCafe. 

Platform | Visio Stencils | **xv** 

**1** 

## **MANUAL FIRMWARE UPDATES** 

Nutanix recommends that you use the Prism Life Cycle Manager to perform firmware updates. However, you must update firmware manually if the following conditions apply: 

- The component is located in a single-node cluster, OR 

- The component is located in a multi-node cluster, but the hypervisor the cluster is running does not support LCM firmware updates. 

To find the latest binary firmware files, consult the Nutanix knowledge base article at this link: KB 6937. 

## **Shutting Down a Single-node Cluster** 

Before updating firmware in a single-node cluster, shut down the node, following the method for your hypervisor. 

**Note:** Single-node clusters do not support Hyper-V. 

Topics that cover single-node cluster shutdown are at the following links: 

- Node shutdown prechecks: Node Shutdown Precheck on page 16 

- Preparation: Preparing a Single Node for Shutdown on page 17 

- vSphere Web Client: Shutting Down a Single Node (vSphere Client) on page 17 

- AHV: Shutting Down a Single Node (AHV) on page 18 

## **Node Shutdown Precheck** 

Check to make sure that there are no issues that might prevent the node from being shut down safely. 

## **Procedure** 

**1.** In Prism Element web console, go to the **Home** page and make sure **Data Resiliency Status** displays a green **OK** . 

**2.** In Prism Element web console, go to the **Health** page and select **Actions** > **Run NCC Checks** . 

**3.** In the dialog box that appears, select **All Checks** and click **Run** . 

Alternatively, issue the following command from the CVM: 

`nutanix@cvm$ ncc health_checks run_all` 

**4.** If any checks fail, see the related KB article provided in the output and the _Nutanix Cluster Check Guide: NCC Reference_ for information on resolving the issue. 

**5.** If you have any unresolvable failed checks, contact Nutanix Support before shutting down the node. 

Platform | Manual Firmware Updates | **16** 

**6.** Gather component details by running the NCC `show_hardware_info` command: 

   - `nutanix@cvm$ ncc hardware_info show_hardware_info` ~~ee~~ 

**7.** Save the output of the `show_hardware_info` command so that you can compare details when verifying the component replacement later. 

**8.** Turn on the chassis identifier lights on the front and back of the node to be removed using one of the following options: 

   - » Log on to Cisco UCS Manager and Cisco Intersight to turn on the UID LEDs. 

   - » For AHV - Log on to the hypervisor host ( _`hypervisor_ip_addr`_ Se ) with SSH and issue the following command. 

> `root@ahv# ipmitool chassis identify 240` ~~ee~~ This command returns the following: 

`Chassis identify interval: 240 seconds` 

- » For vSphere - Log on to the hypervisor host ( ~~a~~ _`hypervisor_ip_addr`_ ) with SSH and issue the following command: 

> `root@esx# /ipmitool chassis identify 240` ~~SCS‘:~~ 

This command returns the following: 

- `Chassis identify interval: 240 seconds` ~~SCS‘:~~ 

## **Preparing a Single Node for Shutdown** 

Prepare the node for shutting down. 

## **About this task** 

## **Procedure** 

**1.** Log on to the CVM and make a note of the BIOS, BMC, and SATA DOM versions. 

**2.** Verify the versions from the configuration file `/etc/nutanix/firmware_config.json` . 

`cat /etc/nutanix/firmware_config.json {"satadom": {"model": "SATADOM-SL 3IE3 V2", "firmware_version": "S560301N"}, "bmc": {"model": "X10_ATEN", "firmware_version": "03.56"}, "motherboard_model": "X10SRW-F", "bios": {"model": "0833", "firmware_version": "20170425"}}` 

**3.** Find the IPMI IP address of the node (necessary in order to access the IPMI web UI.) `nutanix@cvm$` ee `ipmiips` 

**4.** Shut down all running guest VMs on the cluster. 

## **Shutting Down a Single Node (vSphere Client)** 

## **Before you begin** 

Shut down any guest VMs that are running on the node. 

## **About this task** 

Platform | Manual Firmware Updates | **17** 

## **Procedure** 

**1.** Log on to the vSphere Client. 

**2.** Shut down any VMs other than the Controller VM. 

**3.** Right-click the host and select **Maintenance Mode** > **Enter Maintenance Mode** . 

**4.** In the **Enter Maintenance Mode** dialog box, click **OK** . 

The host gets ready to go into maintenance mode, which prevents VMs from running on this host. 

**5.** Log on to the Controller VM with SSH and shut down the Controller VM. 

> `nutanix@cvm$ cvm_shutdown -P now` ~~SSS~~ 

**Note:** Do not reset or shutdown the Controller VM in any way other than the `cvm_shutdown` command to ensure that the cluster is aware that the Controller VM is unavailable 

**6.** After the Controller VM shuts down, wait for the host to go into maintenance mode. 

**7.** Right-click the host and select **Shut Down** . 

Wait until vCenter Server displays that the host is not responding, which may take several minutes. If you are logged on to the ESXi host rather than to vCenter Server, the vSphere web client disconnects when the host shuts down. 

## **Shutting Down a Single Node (AHV)** 

## **Before you begin** 

Shut down guest VMs that are running on the node. 

## **About this task** 

## **Procedure** 

**1.** If the Controller VM is running, shut down the Controller VM. 

   - a. Log on to the Controller VM with SSH. 

   - b. Run the command `acli host.list` . 

   - c. Note the value of **Hypervisor address** for the node. 

   - d. Put the node into maintenance mode. 

`nutanix@cvm$ acli host.enter_maintenance_mode` _`Hypervisor-address`_ `[wait="{ true | false }" ]` 

> Replace ~~a~~ _`Hypervisor-address`_ with the value of **Hypervisor address** for the node. Value of **Hypervisor address** is either the IP address of the AHV host or the host name. 

- e. Shut down the Controller VM. 

~~ee~~ `nutanix@cvm$ cvm_shutdown -P now` 

**2.** Log on to the AHV host with SSH. 

**3.** Shut down the host. 

> `root@ahv# shutdown -h now` ~~ee~~ 

Platform | Manual Firmware Updates | **18** 

## **Shutting Down a Node in a Multinode Cluster** 

Before updating firmware in a cluster, shut down the node, following the method for your hypervisor. 

Topics that cover node shutdown in a multinode cluster are at the following links: 

- Node Shutdown Prechecks: Node Shutdown Precheck on page 16 

- Preparation: Preparing Nodes for Shutdown on page 20 

- vSphere Web Client: Shutting Down a Node in a Cluster through the vSphere Client on page 20 

- vSphere Command Line: Shutting Down a Node in a Cluster through the Command Line on page 21 

- AHV: Shutting Down a Node in a Cluster (AHV) on page 22 

- Hyper-V: Shutting Down a Node in a Cluster (Hyper-V) on page 24 

## **Node Shutdown Precheck** 

Check to make sure that there are no issues that might prevent the node from being shut down safely. 

## **Procedure** 

**1.** In Prism Element web console, go to the **Home** page and make sure **Data Resiliency Status** displays a green **OK** . 

**2.** In Prism Element web console, go to the **Health** page and select **Actions** > **Run NCC Checks** . 

**3.** In the dialog box that appears, select **All Checks** and click **Run** . 

Alternatively, issue the following command from the CVM: 

`nutanix@cvm$ ncc health_checks run_all` 

**4.** If any checks fail, see the related KB article provided in the output and the _Nutanix Cluster Check Guide: NCC Reference_ for information on resolving the issue. 

**5.** If you have any unresolvable failed checks, contact Nutanix Support before shutting down the node. 

**6.** Gather component details by running the NCC `show_hardware_info` command: 

`nutanix@cvm$ ncc hardware_info show_hardware_info` 

**7.** Save the output of the `show_hardware_info` command so that you can compare details when verifying the component replacement later. 

Platform | Manual Firmware Updates | **19** 

**8.** Turn on the chassis identifier lights on the front and back of the node to be removed using one of the following options: 

   - » Log on to Cisco UCS Manager and Cisco Intersight to turn on the UID LEDs. 

   - » For AHV - Log on to the hypervisor host ( _`hypervisor_ip_addr`_ Se ) with SSH and issue the following command. 

> `root@ahv# ipmitool chassis identify 240` ~~CSS~~ This command returns the following: 

`Chassis identify interval: 240 seconds` 

- » For vSphere - Log on to the hypervisor host ( ~~a~~ _`hypervisor_ip_addr`_ ) with SSH and issue the following command: 

> `root@esx# /ipmitool chassis identify 240` ~~SCS~~ This command returns the following: 

> `Chassis identify interval: 240 seconds` ~~SCS~~ 

## **Preparing Nodes for Shutdown** 

Prepare each node in the cluster for shutting down. 

## **About this task** 

## **Procedure** 

**1.** Log on to the CVM and make a note of the BIOS, BMC, and SATA DOM versions. 

**2.** Verify the versions from the configuration file `/etc/nutanix/firmware_config.json` . 

`cat /etc/nutanix/firmware_config.json {"satadom": {"model": "SATADOM-SL 3IE3 V2", "firmware_version": "S560301N"}, "bmc": {"model": "X10_ATEN", "firmware_version": "03.56"}, "motherboard_model": "X10SRW-F", "bios": {"model": "0833", "firmware_version": "20170425"}}` 

**3.** Find the IPMI IP address of each node (necessary in order to access the IPMI web UI.) `nutanix@cvm$` ee `ipmiips` 

**4.** Shut down all running guest VMs on the cluster. 

## **Shutting Down a Node in a Cluster through the vSphere Client** 

## **About this task** 

**Caution:** Verify the data resiliency status of your cluster. If the cluster only has replication factor 2 (RF2), you can only shut down one node for each cluster. If an RF2 cluster would have more than one node shut down, shut down the entire cluster. 

## **Procedure** 

## **1.** 

**2.** Log on to vCenter with the vSphere Client. 

If vCenter is not available, log on to the ESXi host IP address with vSphere. 

Platform | Manual Firmware Updates | **20** 

**3.** If DRS is not enabled, manually migrate all the VMs except the Controller VM to another host in the cluster or shut down any VMs other than the Controller VM that you do not want to migrate to another host. 

If DRS is enabled on the cluster, you can skip this step. 

**4.** Right-click the host and select **Maintenance Mode** > **Enter Maintenance Mode** . 

**5.** In the **Enter Maintenance Mode** dialog box, click **OK** . The host gets ready to go into maintenance mode, which prevents VMs from running on this host. DRS automatically attempts to migrate all the VMs to another host in the cluster. 

**Note:** If DRS is not enabled, manually migrate or shut down all the VMs excluding the Controller VM. If certain options are configured in VMs, but not present on the target host, those VMs might not be migrated automatically, even when DRS is enabled. 

**6.** Log on to the Controller VM with SSH and shut down the Controller VM. 

> `nutanix@cvm$ cvm_shutdown -P now` ~~SCS~~ 

**Note:** Do not reset or shutdown the Controller VM in any way other than the `cvm_shutdown` command to ensure that the cluster is aware that the Controller VM is unavailable 

**7.** After the Controller VM shuts down, wait for the host to go into maintenance mode. 

**8.** Right-click the host and select **Shut Down** . 

Wait until vCenter Server displays that the host is not responding, which may take several minutes. If you are logged on to the ESXi host rather than to vCenter Server, the vSphere client disconnects when the host shuts down. 

## **Shutting Down a Node in a Cluster through the Command Line** 

## **About this task** 

This command-line procedure applies only to clusters running AOS 6.7.x or earlier. 

Because AOS 6.8 migrated to Python 3, AOS 6.8 and later does not support the command-line commands for shutting down the node. If your cluster uses AOS 6.8 or later, follow the procedure in Shutting Down a Node in a Cluster through the vSphere Client on page 20. 

If DRS is not enabled, manually migrate all the VMs except the Controller VM to another host in the cluster or shut down any VMs other than the Controller VM that you do not want to migrate to another host. If DRS is enabled on the cluster, you can skip this pre-requisite. 

**Caution:** Verify the data resiliency status of your cluster. If the cluster only has replication factor 2 (RF2), you can only shut down one node for each cluster. If an RF2 cluster would have more than one node shut down, shut down the entire cluster. 

You can put the ESXi host into maintenance mode and shut it down from the command line or by using the vSphere web client. 

## **Procedure** 

**1.** Log on to the Controller VM with SSH and shut down the Controller VM. 

**2.** Log on to another Controller VM in the cluster with SSH. 

Platform | Manual Firmware Updates | **21** 

## **3.** Shut down the host. 

> `nutanix@cvm$ ~/serviceability/bin/esx-enter-maintenance-mode -s` ~~SSS~~ _`cvm_ip_addr`_ 

If successful, this command returns no output. If it fails with a message like the following, VMs are probably still running on the host. 

`CRITICAL esx-enter-maintenance-mode:42 Command vim-cmd hostsvc/maintenance_mode_enter failed with ret=-1` 

Ensure that all VMs are shut down or moved to another host and try again before proceeding. 

> `nutanix@cvm$ ~/serviceability/bin/esx-shutdown -s` ~~SSS~~ _`cvm_ip_addr`_ 

> Replace ~~a~~ _`cvm_ip_addr`_ with the IP address of the Controller VM on the ESXi host. Alternatively, you can put the ESXi host into maintenance mode and shut it down using the vSphere Web Client. 

If the host shuts down, a message like the following is displayed. 

`INFO esx-shutdown:67 Please verify if ESX was successfully shut down using ping` _`hypervisor_ip_addr`_ 

**4.** Confirm that the ESXi host has shut down. 

`nutanix@cvm$ ping` _`hypervisor_ip_addr`_ 

> Replace Se _`hypervisor_ip_addr`_ with the IP address of the ESXi host. 

If no ping packets are answered, the ESXi host shuts down. 

## **Shutting Down a Node in a Cluster (AHV)** 

## **Before you begin** 

Check if the cluster can tolerate a single-node failure. Do not proceed if the cluster cannot tolerate singlenode failure. 

Log on to the Prism Element web console, then go to the **Home** page and make sure **Data Resiliency Status** displays a green OK status. 

Alternatively, you can also log on to any Controller VM (CVM) as the Nutanix user to run the following command and confirm that each component type has a fault tolerance of 1: 

~~ee~~ `nutanix@cvm$ ncli cluster get-domain-fault-tolerance-status type=node` 

Fault tolerance of 1 implies that it is safe to shut down the node. 

## **About this task** 

**Caution:** Verify the data resiliency status of your cluster. If the cluster only has replication factor 2 (RF2), you can only shut down one node for each cluster. If an RF2 cluster would have more than one node shut down, shut down the entire cluster. 

You must shut down the CVM to shut down a node. Before you shut down the CVM, you must put the node in maintenance mode. 

When a host is in maintenance mode, AOS marks the host as unschedulable so that no new VM instances are created on it. Next, an attempt is made to evacuate VMs from the host. If the evacuation attempt fails, the host remains in the **Entering maintenance mode** state, where it is marked unschedulable, waiting for user remediation. You can shut down VMs on the host or move them to other nodes. Once the host has no more running VMs, it is in maintenance mode. 

Platform | Manual Firmware Updates | **22** 

VMs that can be migrated are moved from that host to other hosts in the cluster. After exiting maintenance mode, those VMs are returned to the original host, eliminating the need to manually move them. 

VMs with GPUs, CPU passthrough, PCI passthrough, and host affinity policies are not migrated to other hosts in the cluster. 

Agent VMs are always shut down if you put a node in maintenance mode and are powered on again after exiting maintenance mode. 

## **Procedure** 

**1.** Put the node into maintenance mode: 

**Warning:** The following steps might temporarily affect cluster operations or availability. Coordinate performing this procedure with the customer to help minimize these effects. 

- a. Log on to a CVM in the cluster using SSH. 

- b. Determine the IP address of the node to be put into maintenance mode. 

~~CSCC~~ `nutanix@cvm$ acli host.list` 

Note the value of **Hypervisor IP** for the node that you want to put in maintenance mode. 

- c. Put the node into maintenance mode: 

`nutanix@cvm$ acli host.enter_maintenance_mode` _`Hypervisor-IP-address`_ `[wait="{ true | false }" ] [non_migratable_vm_action="{ acpi_shutdown | block }" ]` 

> Replace ee _`Hypervisor-IP-address`_ with either the IP address or host name of the AHV host to be shut down. 

> The following parameters are optional for running the ~~EE~~ `acli host.enter_maintenance_mode` command: 

- **wait** : Set the **wait** parameter to **true** to wait for the host evacuation attempt to finish. 

- **non_migratable_vm_action** : By default the **non_migratable_vm_action** parameter is set to **block** , which means that VMs with GPU, CPU passthrough, PCI passthrough, and host affinity policies are not migrated or shut down when you put a node into maintenance mode. To automatically shut down such VMs for the duration of the maintenance mode, set the **non_migratable_vm_action** parameter to **acpi_shutdown** . 

If you set the **non_migratable_vm_action** parameter to **block** and the operation to put the host into the maintenance mode fails, exit the maintenance mode, and either manually migrate the VMs to another host or shut down the VMs by setting the **non_migratable_vm_action** parameter to **acpi_shutdown** . 

**2.** Verify that the host is in the maintenance mode: 

> `nutanix@cvm$ acli host.get` ~~SSS~~ _`host-ip`_ 

In the command output, ensure that **node_state** equals to **EnteredMaintenanceMode** and **schedulable** equals to **False** . 

**Note:** Do not continue if the host fails to enter maintenance mode. 

Platform | Manual Firmware Updates | **23** 

**3.** As the Nutanix user, log on to the node you want to shut down and run this command to shut down the CVM: 

> `nutanix@cvm$ cvm_shutdown -P now` ~~SSS~~ 

**Caution:** Ensure that you are running this command on the CVM to be shut down to prevent any accidental cluster outage. 

**4.** Log on to the AHV host with SSH. 

**5.** Make sure that the CVM completed shutting down: 

> `[root@AHV~]# virsh list` ~~SCS~~ 

Running `virsh list` should not list any VMs. 

If the CVM is still running, wait for a few minutes until it is powered off. 

**6.** Shut down the host: 

> `root@ahv# shutdown -h now` ~~SCS~~ 

**Caution:** To prevent accidental cluster outage, make sure that the host on which you are running this command is the host that you want to shut down. 

## **Shutting Down a Node in a Cluster (Hyper-V)** 

Shut down a node in a Hyper-V cluster. 

## **Before you begin** 

Shut down guest VMs that are running on the node, or move them to other nodes in the cluster. 

## **About this task** 

**Caution:** Verify the data resiliency status of your cluster. If the cluster only has replication factor 2 (RF2), you can only shut down one node for each cluster. If an RF2 cluster would have more than one node shut down, shut down the entire cluster. 

Perform the following procedure to shut down a node in a Hyper-V cluster. 

## **Procedure** 

**1.** Log on to the Hyper-V host with Remote Desktop Connection. 

**2.** Open the Failover Cluster Manager. 

**3.** Click on **Nodes** . 

**4.** Right-click on the node name. 

Platform | Manual Firmware Updates | **24** 

## **5.** Under **Pause** click **Drain Roles** . 

Under Status the node appears as **Paused** . 

**Figure 1: Pause Node Options in Failover Cluster Manager** 

**6.** (Optional) At the bottom of the center pane, click on the **Roles** tab. Once all roles have moved off this node, it is safe to shut down the node. 

**7.** Log on to the Controller VM with SSH and shut down the Controller VM. 

`nutanix@cvm$ cvm_shutdown -P now` 

## **Note:** 

Always use the `cvm_shutdown` command to reset, or shutdown the Controller VM. The `cvm_shutdown` command notifies the cluster that the Controller VM is unavailable. 

**8.** Log on to the Hyper-V host with Remote Desktop Connection and start PowerShell. 

**9.** Do one of the following to shut down the node. 

   - » `> shutdown /s /t 0` 

   - » > `Stop-Computer -ComputerName localhost` 

See the Microsoft documentation for more details about how to shut down a Hyper-V node. 

## **Manually Updating M.2 RAID Hypervisor Boot Drive Firmware** 

Manually update the firmware on HW RAID M.2 boot drives on NX-G7 or newer platforms in cases when you cannot use LCM. 

## **About this task** 

Starting with G7 platforms, Nutanix platforms use a host boot device with a hardware RAID. Whenever possible, use LCM to update the firmware on M.2 RAID drives. LCM automates the upgrade. In some cases you might not be able to use LCM. Use this topic for all manual firmware upgrades for RAID M.2 boot drives. 

During this procedure, you must restart your node into the Phoenix ISO and fetch a firmware update binary from Nutanix. You can the find the update binaries listed in Nutanix knowledge base article 6937, available at this link: KB 6937. 

Platform | Manual Firmware Updates | **25** 

To upgrade NX-G8/G9 nodes to the latest firmware version for the Marvell m.2 raid card, follow the procedure in Nutanix knowledge base article 17387, available at this link:KB 17387. 

**Note:** If you need to update M.2 firmware on any G6 or newer platforms that have only a single M.2 drive, see Manually Updating Data Drive Firmware on page 38 in this document. Do not attempt to follow this procedure on older platforms. 

**Note:** Always use the latest version of the Phoenix ISO. 

**Caution:** Only use this process on Nutanix NX platforms. 

## **Procedure** 

**1.** Go to the Nutanix portal at https://portal.nutanix.com and select **Downloads** > **Phoenix** to reach the Phoenix landing page. Download the Phoenix ISO to your system. 

**2.** From the CVM prompt, identify an active 10G interface on the host. Make a note of the interface for use in a later step. 

   - » AHV: ~~SS~~ `nutanix@cvm$ manage_ovs show_interfaces` 

   - » ESXi: ~~Se~~ `nutanix@cvm$ ssh root@` _`host-IP-address`_ `esxcli network nic list` 

**3.** Find an unused IP address that is in the same subnet as the CVM. Make a note of it so you can assign this IP address to the Phoenix ISO in a later step. 

**4.** Check to see if the CVM has a VLAN configured. If it does, make a note of the VLAN ID. 

**5.** Check the state of the cluster. 

   - a. Make sure that no services are down on the cluster. 

~~CSCS~~ `nutanix@cvm$ cluster status | grep -v UP` 

- b. Make sure that all hosts are part of the metadata ring. 

~~CSCS~~ `nutanix@cvm$ nodetool -h 0 ring` 

   - c. Check cluster data resiliency. 

      - ~~OO~~ `nutanix@cvm$ ncli cluster get-domain-fault-tolerance-status type=node` —C—SSCSCSCSSCCs 

**6.** Put the host into maintenance mode and shut down the CVM and the node. 

   - » If the drive is part of a single-node cluster, follow the procedures that start with Shutting Down a Single-node Cluster on page 16 . 

   - » If the drive is part of a multinode cluster, follow the procedures that start with Shutting Down a Node in a Multinode Cluster on page 19. 

**7.** From the system where you downloaded the Phoenix ISO, enter the IPMI IP address of the node in a web browser to reach the IPMI web UI for the node. 

Platform | Manual Firmware Updates | **26** 

**8.** In the IPMI web UI, launch the remote console by selecting **Remote Control** > **Console Redirection** . 

**Figure 2: Console redirection** 

**9.** Click the **Launch Console** button. 

**10.** From the console menu, select **Virtual Media** > **Virtual Storage** . 

**Figure 3: Virtual storage** 

Platform | Manual Firmware Updates | **27** 

**11.** In the **Virtual Storage** dialog box, specify the storage settings. 

   - a. In the **Logical Drive Type** field, select **ISO File** . 

   - b. In the **Image File Name and Full Path** field, select **Open Image** , browse to the location where you downloaded the Phoenix ISO file, and click **Open** . 

   - c. Click **Plug In** to mount the ISO on the node. 

**Figure 4: Specifying the image** 

**12.** Click **OK** . 

**13.** From the console menu, restart the node by selecting **Power Control** > **Set Power On** . 

**Figure 5: Restart the node with Set Power On** 

The node restarts, showing the Phoenix prompt. 

`phoenix ~#` 

**14.** Set an IP address, netmask, and default gateway. 

   - » If the CVM has a VLAN configured: 

`phoenix ~# ifconfig` _`interface`_ `up phoenix ~# ip link add link` _`interface`_ `name` _`interface`_ `.` _`VLAN_ID`_ `type vlan id` _`VLAN_ID`_ `phoenix ~# ip addr add` _`PHX_IP_address`_ `/24 dev` _`interface`_ `.` _`VLAN_ID`_ `phoenix ~# ip link set dev` _`interface`_ `.` _`VLAN_ID`_ `up` 

Platform | Manual Firmware Updates | **28** 

`phoenix ~# ip link show phoenix ~# ip route add default via` _`CVM_default_gateway`_ `dev` _`interface`_ `.` _`VLAN_ID`_ 

- For _`interface`_ ~~a~~ , use the active interface you identified in step 2. 

- For _`PHX_IP_address`_ ~~a~~ , use the IP address that you identified in step 3. 

> In the following example, the interface is — `eth2` and the VLAN ID is _ `691` . 

`phoenix ~# ifconfig eth2 up phoenix ~# ip link add link eth2 name eth2.691 type vlan id 691 phoenix ~# ip addr add 198.51.100.10/24 dev eth2.691 phoenix ~# ip link set dev eth2.691 up phoenix ~# ip link show phoenix ~# ip route add default via 198.51.100.16 dev eth2.691` 

- » If the CVM does not have a VLAN configured: 

`phoenix ~# ip link set dev` _`interface`_ `up phoenix ~# ifconfig` _`interface PHX_IP_address`_ `phoenix ~# ifconfig` _`interface`_ `netmask` _`CVM_subnet_mask`_ `phoenix ~# ip route add default via` _`CVM_default_gateway`_ `dev` _`interface`_ 

- For _`interface`_ ~~a~~ , use the active interface you identified in step 2. 

- For _`PHX_IP_address`_ ~~a~~ , use the IP address that you identified in step 3. 

> In the following example, the interface is — `eth2` . 

`phoenix ~# ip link set dev eth2 up phoenix ~# ifconfig eth2 198.51.100.10 phoenix ~# ifconfig eth2 netmask 255.255.255.0 phoenix ~# ip route add default via 198.51.100.16 dev eth2` 

**15.** Test connectivity by pinging another CVM or the gateway. 

**16.** Enable the rescue shell. 

   - a. Change your working directory to the directory that contains the `do_rescue_shell.sh` script. 

`phoenix ~# cd /root` 

- b. Set executable permissions on the rescue shell script. 

`phoenix/root~# chmod +x do_rescue_shell.sh` 

- c. Run the rescue shell script. 

`phoenix/root~# sh do_rescue_shell.sh` 

**17.** Open a new SSH session to the CVM where the binaries are staged, and log on as root with the password `nutanix/4u` Le . 

**18.** Run the `lsscsi` command. 

`[root@phoenix ~]# lsscsi [0:0:0:0]    disk    ATA      SAMSUNG MZ7LH3T8 404Q  /dev/sda [0:0:1:0]    disk    ATA      ST4000NM0035-1V4 TN05  /dev/sdb` 

Platform | Manual Firmware Updates | **29** 

`[0:0:2:0]    disk    ATA      ST4000NM0035-1V4 TN05  /dev/sdc [1:0:0:0]    cd/dvd  ATEN     Virtual CDROM    YS0J  /dev/sr0 [16:0:0:0]   disk    ATA      SMC VD           00-0  /dev/sdd [18:0:0:0]   process Marvell  Console          1.01  -` 

> The enumeration of the disks can vary. You can see a device labeled " a `SMC VD` " (in this eample it is 

> `/dev/sdd` a ). This disk represents the hardware RAID for the boot partition, rather than showing the individual disks in the RAID. This is one difference which prevents a direct update of the firmware. This also prevents you from directly checking the firmware of the disks with `smartctl` a . 

**19.** Check the version details for the disks (and HBA) with the `mvcli` utility in Phoenix. 

   - a. Check the version of disk in port 0: 

`[root@phoenix ~]# mvcli info -o pd -i 0 SG driver version 3.5.36.` 

`Physical Disk Information ---------------------------Adapter:             0 PD ID:               0 Type:                SATA PD Linked at:           HBA port 0 Size:                234431064 K Write cache:         not supported SMART:               supported (on) NCQ:                 supported (on) 48 bits LBA:         supported supported speed:     1.5 3 6 Gb/s Current speed:       6 Gb/s model:               Micron_5100_MTFDDAV240TCB Serial:              18381E845493 Firmware version:     D0MU071 Locate LED status:   Not Support Running OS:          no SSD Type:            SSD block ids:           0 associated VDs:      0 PD valid size:       0 K` 

- b. Check the version of disk in port 1: 

`[root@phoenix ~]# mvcli info -o pd -i 1 SG driver version 3.5.36.` 

`Physical Disk Information ---------------------------Adapter:             0 PD ID:               1 Type:                SATA PD Linked at:           HBA port 1 Size:                234431064 K Write cache:         not supported SMART:               supported (on) NCQ:                 supported (on) 48 bits LBA:         supported supported speed:     1.5 3 6 Gb/s Current speed:       6 Gb/s model:               Micron_5100_MTFDDAV240TCB Serial:              18381E8454D3 Firmware version:     D0MU071 Locate LED status:   Not Support Running OS:          no` 

Platform | Manual Firmware Updates | **30** 

`SSD Type:            SSD block ids:           4 associated VDs:      0 PD valid size:       0 K` 

- c. Check the version of the raid controller: 

`[root@phoenix ~]# mvcli info -o hba SG driver version 3.5.36.` 

`Adapter ID:                          0 Product:                             1b4b-9230 Sub Product:                         15d9-1b22 Chip revision:                       A1 slot number:                         0 Max PCIe speed:                      5Gb/s Current PCIe speed:                  5Gb/s Max PCIe link:                       2 Current PCIe link:                   2 BIOS version:                        1.0.0.1031 Firmware version:                    2.3.21.1003 Boot loader version:                 2.1.0.1009 # of ports:                          3 Buzzer:                              Not supported Supported port type:                 SATA Supported RAID mode:                 RAID1 JBOD Maximum disk in one VD:              2 PM:                                  Not supported Expander:                            Not supported Maximum supported disk:              2 Maximum supported VD:                1 Max total blocks:                    128 Features:                            rebuild,media patrol Advanced features:                   event sense code,multi VD,spc 4,image health,timer,ata pass through,oem data Advanced features 2:                 scsi pass through,flash Max buffer size:                     3 Stripe size supported:               32K 64K Image health:                        Healthy Autoload image health:               Healthy Boot loader image health:            Healthy Firmware image health:               Healthy Boot ROM image health:               Healthy HBA info image health:               Healthy` 

**20.** Download the firmware update binary for your drive from the list in Nutanix knowledge base article 6937, available at this link: KB 6937. 

**21.** Copy the firmware update binary to the Phoenix IP address that you assigned earlier: use a secure copy command such as `scp` . 

Platform | Manual Firmware Updates | **31** 

**22.** Use the following steps to update the firmware. 

   - a. Copy the firmware to the phoenix host over `ssh` (to the a `/root` directory): 

~~SCS~~ `scp firmware_binary.bin root@phoenix_ip_address:/root/` 

- b. Change to the — `/root` directory if you are not there already. ~~OO~~ `cd /root` 

- c. If the binary was zipped, unzip the firmware that you uploaded in step a to get access to the .bin file. 

~~ee~~ `unzip firmware_bundle.zip` 

- d. To update the drive in port 0, type: 

~~CSCS~~ `mvcli pdflash -i 0 -f firmware_binary.bin` 

- e. To update the drive in port 1, type: 

~~CSCS~~ `mvcli pdflash -i 1 -f firmware_binary.bin` 

f. Verify the new firmware version of each M.2 device: ~~OO~~ `[root@phoenix ~]# mvcli info -o pd -i 1` ~~OO~~ `[root@phoenix ~]# mvcli info -o pd -i 0` 

**23.** From the console menu, select **Virtual Media** > **Virtual Storage** to unmount the ISO. 

**24.** Disconnect from the IPMI web UI and restart the host. 

   - » If the drive is part of a single-node cluster, follow the procedures in Starting a Single-node Cluster on page 55. 

   - » If the drive is part of a multinode cluster, follow the procedures in Starting a Node in a Multinode Cluster on page 58. 

**25.** Check the state of the cluster. 

   - a. Make sure that all hosts are part of the metadata ring. 

~~ee~~ `nutanix@cvm$ nodetool -h 0 ring` 

- b. Check cluster data resiliency. 

~~CSCS~~ `ncli> cluster get-domain-fault-tolerance-status type=node` 

**26.** To show the updated firmware in the UI, open LCM from Prism and select **Inventory** > **Perform Inventory** in the LCM dashboard. 

## **Manually Updating NVMe Drive Firmware** 

Update firmware on NVMe drives. 

## **About this task** 

To update an NVMe drive manually, you must restart the node and download a firmware update binary provided by Nutanix. You can the find the update binaries listed in Nutanix knowledge base article 6937, available at this link: KB 6937. Contact Nutanix Support if you need further assistance. 

Platform | Manual Firmware Updates | **32** 

## **Procedure** 

**1.** Go to the Nutanix portal at https://portal.nutanix.com and select **Downloads** > **Phoenix** to reach the Phoenix landing page. Download the Phoenix ISO to your system. 

**2.** From the CVM prompt, identify an active 10G interface on the host. Make a note of the interface for use in a later step. 

   - » AHV: 

`nutanix@cvm$ manage_ovs show_interfaces` 

- » ESXi: 

`nutanix@cvm$ ssh root@` _`host-IP-address`_ `esxcli network nic list` 

**3.** Find an unused IP address that is in the same subnet as the CVM. Make a note of it so you can assign this IP address to the Phoenix ISO in a later step. 

**4.** Check to see if the CVM has a VLAN configured. If it does, make a note of the VLAN ID. 

**5.** Check the state of the cluster. 

   - a. Make sure that no services are down on the cluster. 

`nutanix@cvm$ cluster status | grep -v UP` 

- b. Make sure that all hosts are part of the metadata ring. 

`nutanix@cvm$ nodetool -h 0 ring` 

- c. Check cluster data resiliency. 

`ncli> cluster get-domain-fault-tolerance-status type=node` 

**6.** Put the host into maintenance mode and shut down the CVM and the node. 

   - » If the drive is part of a single-node cluster, follow the procedures in Shutting Down a Single-node Cluster on page 16. 

   - » If the drive is part of a multinode cluster, follow the procedures in Shutting Down a Node in a Multinode Cluster on page 19. 

**7.** From the system where you downloaded the Phoenix ISO, enter the IPMI IP address of the node in a web browser to reach the IPMI web interface for the node. 

Platform | Manual Firmware Updates | **33** 

**8.** In the IPMI web interface, launch the remote console by selecting **Remote Control** > **Console Redirection** . 

**Figure 6: Console redirection** 

**9.** Click the **Launch Console** button. 

**10.** From the console menu, select **Virtual Media** > **Virtual Storage** . 

**Figure 7: Virtual storage** 

Platform | Manual Firmware Updates | **34** 

**11.** In the **Virtual Storage** dialog box, specify the storage settings. 

   - a. In the **Logical Drive Type** field, select **ISO File** . 

   - b. In the **Image File Name and Full Path** field, select **Open Image** , browse to the location where you downloaded the Phoenix ISO file, and click **Open** . 

   - c. Click **Plug In** to mount the ISO on the node. 

**Figure 8: Specifying the image** 

**12.** Click **OK** . 

**13.** From the console menu, restart the node by selecting **Power Control** > **Set Power Reset** . 

**Figure 9: Restart the node** 

The node restarts, showing the Phoenix prompt. 

`phoenix ~#` 

**14.** Set an IP address, netmask, and default gateway. 

   - » If the CVM has a VLAN configured: 

`phoenix ~# ifconfig` _`interface`_ `up phoenix ~# ip link add link` _`interface`_ `name` _`interface`_ `.` _`VLAN_ID`_ `type vlan id` _`VLAN_ID`_ `phoenix ~# ip addr add` _`PHX_IP_address`_ `/24 dev` _`interface`_ `.` _`VLAN_ID`_ `phoenix ~# ip link set dev` _`interface`_ `.` _`VLAN_ID`_ `up` 

Platform | Manual Firmware Updates | **35** 

`phoenix ~# ip link show phoenix ~# ip route add default via` _`CVM_default_gateway`_ `dev` _`interface`_ `.` _`VLAN_ID`_ 

- For _`interface`_ ~~a~~ use the active interface that you identified in step 2. 

- For _`PHX_IP_address`_ ~~a~~ use the IP address that you identified in step 3. 

> In the following example, the interface is — `eth2` and the VLAN ID is _ `691` . 

`phoenix ~# ifconfig eth2 up phoenix ~# ip link add link eth2 name eth2.691 type vlan id 691 phoenix ~# ip addr add 198.51.100.10/24 dev eth2.691 phoenix ~# ip link set dev eth2.691 up phoenix ~# ip link show phoenix ~# ip route add default via 198.51.100.16 dev eth2.691` 

- » If the CVM does not have a VLAN configured: 

`phoenix ~# ip link set dev` _`interface`_ `up phoenix ~# ifconfig` _`interface PHX_IP_address`_ `phoenix ~# ifconfig` _`interface`_ `netmask` _`CVM_subnet_mask`_ `phoenix ~# ip route add default via` _`CVM_default_gateway`_ `dev` _`interface`_ 

- For _`interface`_ ~~a~~ use the active interface you identified in step 2. 

- For _`PHX_IP_address`_ ~~a~~ use the IP address that you identified in step 3. 

> In the following example, the interface is — `eth2` . 

`phoenix ~# ip link set dev eth2 up phoenix ~# ifconfig eth2 198.51.100.10 phoenix ~# ifconfig eth2 netmask 255.255.255.0 phoenix ~# ip route add default via 198.51.100.16 dev eth2` 

**15.** Test connectivity by pinging another CVM or the gateway. 

**16.** Enable the rescue shell. 

   - a. Change your working directory to the directory that contains the `do_rescue_shell.sh` script. 

`phoenix ~# cd /root` 

- b. Set executable permissions on the rescue shell script. 

`phoenix/root~# chmod +x do_rescue_shell.sh` 

- c. Run the rescue shell script. 

`phoenix/root~# sh do_rescue_shell.sh` 

**17.** Return to the Phoenix prompt. 

**18.** List the NVMe drives installed on the system. 

`[root@phoenix ~]# nvme list Node           SN  Model        Namespace Usage    Format    FW Rev -------------------------------------------------------------------------` 

Platform | Manual Firmware Updates | **36** 

`/dev/nvme0n1 S3HDNX0KC00402 SAMSUNG MZWLL1T6HEHP-00003 1 1.60 TB / 1.60  TB 512 B + 0 B GPNA9B3Q /dev/nvme1n1 S3HDNX0KC00400 SAMSUNG MZWLL1T6HEHP-00003 1 1.60 TB / 1.60  TB 512 B + 0 B GPNA9B3Q` 

**19.** Check the NVMe firmware version and firmware slots. 

`[root@phoenix ~]# nvme id-ctrl /dev/nvme0 -H |egrep -w 'sn|mn|fr|frmw|Firmware' sn        : S3HDNX0KC00402 mn        : SAMSUNG MZWLL1T6HEHP-00003 fr        : GPNA9B3Q [9:9] : 0        Firmware Activation Notices Not Supported frmw      : 0x17 [4:4] : 0x1    Firmware Activate Without Reset Supported [3:1] : 0x3    Number of Firmware Slots [0:0] : 0x1    Firmware Slot 1 Read-Only` 

The output varies depending on the drive firmware version. 

In the output shown in this example, slot 1 is read-only and the drive supports three firmware slots. Therefore you need to activate the firmware image from slot 2, which is read/write. If slot 1 is read/ write, activate slot 1. For more information, see the NVMe specification at the NVMe express website, available at this link: NVMe express website. 

**20.** Download the firmware update binary for your drive from the list available at this link: KB 6937. 

**21.** Copy the firmware update binary to the Phoenix IP address you assigned in step 14, using a secure copy command such as `scp` . 

**22.** Download the firmware. 

`[root@phoenix tmp]# nvme fw-download /dev/nvme0 --fw=/root/tmp/GPNABB3Q.bin Firmware download success` 

**23.** Activate the firmware in the appropriate slot. 

In this example, slot 1 is read-only, so the command activates slot 2. 

`[root@phoenix tmp]# nvme fw-commit /dev/nvme0 --slot=2 --action=3 Success committing firmware action:3 slot:2` 

**24.** Use the `id-ctrl` command to verify that the new firmware is active. 

`[root@phoenix tmp]# nvme id-ctrl /dev/nvme0 |egrep 'mn |fr' mn        : SAMSUNG MZWLL1T6HEHP-00003 fr        : GPNABB3Q frmw      : 0x17` 

**25.** Repeat steps 18 through step 24 for all the NVMe drives. 

**26.** From the console menu, select **Virtual menu** > **Virtual Storage** to unmount the ISO. 

**27.** Disconnect from the IPMI web interface and restart the host. 

   - » If the drive is part of a single-node cluster, follow the procedures in Starting a Single-node Cluster on page 55. 

   - » If the drive is part of a multinode cluster, follow the procedures in Starting a Node in a Multinode Cluster on page 58. 

**28.** Check the state of the cluster. 

Platform | Manual Firmware Updates | **37** 

**29.** Ensure that all hosts are part of the metadata ring. 

`nutanix@cvm$ nodetool -h 0 ring` 

**30.** Check the cluster data resiliency. 

`ncli> cluster get-domain-fault-tolerance-status type=node` 

**31.** To show the updated firmware in the UI, open LCM from Prism and select **Inventory** > **Perform Inventory** in the LCM dashboard. 

## **Manually Updating Data Drive Firmware** 

Manually update the firmware for a SAS or SATA data drive, or update the hypervisor boot drive firmware on a non-RAID M.2 drive. 

## **About this task** 

All G6 platforms have non-RAID M.2 drives. The same procedure applies to G7 platforms that have only one M.2 device, such as the NX-1175S-G7, the NX-1120S-G7, or newer platforms with a single M.2 drive. To update RAID M.2 devices, see Manually Updating M.2 RAID Hypervisor Boot Drive Firmware on page 25. 

To update a data drive manually, you must restart your node into the Phoenix ISO and fetch a firmware update binary from Nutanix. You can find a list of update binaries in Nutanix knowledge base article 6937, available at this link: KB 6937. 

## **Procedure** 

**1.** Go to the Nutanix portal at https://portal.nutanix.com and select **Downloads** > **Phoenix** to reach the Phoenix landing page. Download the Phoenix ISO to your system. 

**2.** From the CVM prompt, identify an active 10G interface on the host. Make a note of the interface for use in a later step. 

   - » AHV: 

`nutanix@cvm$ manage_ovs show_interfaces` 

- » ESXi: 

`nutanix@cvm$ ssh root@` _`host-IP-address`_ `esxcli network nic list` 

**3.** Find an unused IP address that is in the same subnet as the CVM. Make a note of it so you can assign this IP address to the Phoenix ISO in a later step. 

**4.** Check to see if the CVM has a VLAN configured. If it does, make a note of the VLAN ID. 

Platform | Manual Firmware Updates | **38** 

**5.** Check the state of the cluster. 

   - a. Make sure that no services are down on the cluster. 

`nutanix@cvm$ cluster status | grep -v UP` 

- b. Make sure that all hosts are part of the metadata ring. 

`nutanix@cvm$ nodetool -h 0 ring` 

- c. Check cluster data resiliency. 

`ncli> cluster get-domain-fault-tolerance-status type=node` 

**6.** Put the host into maintenance mode and shut down the CVM and the node. 

   - » If the drive is part of a single-node cluster, follow the procedures in Shutting Down a Single-node Cluster on page 16. 

   - » If the drive is part of a multinode cluster, follow the procedures in Shutting Down a Node in a Multinode Cluster on page 19. 

**7.** From the system where you downloaded the Phoenix ISO, enter the IPMI IP address of the node in a web browser to reach the IPMI WebUI for the node. 

**8.** In the IPMI WebUI, launch the remote console by selecting **Remote Control** > **Console Redirection** . 

**Figure 10: Console redirection** 

**9.** Click the **Launch Console** button. 

**10.** From the console menu, select **Virtual Media** > **Virtual Storage** . 

**Figure 11: Virtual storage** 

Platform | Manual Firmware Updates | **39** 

**11.** In the **Virtual Storage** dialog box, specify the storage settings. 

   - a. In the **Logical Drive Type** field, select **ISO File** . 

   - b. In the **Image File Name and Full Path** field, select **Open Image** , browse to the location where you downloaded the Phoenix ISO file, and click **Open** . 

   - c. Click **Plug In** to mount the ISO on the node. 

**Figure 12: Specifying the image** 

**12.** Click **OK** . 

**13.** From the console menu, restart the node by selecting **Power Control** > **Set Power On** . 

**Figure 13: Restart the node** 

The node restarts, showing the Phoenix prompt. 

`phoenix ~#` 

**14.** Set an IP address, netmask, and default gateway. 

   - » If the CVM has a VLAN configured: 

`phoenix ~# ifconfig` _`interface`_ `up phoenix ~# ip link add link` _`interface`_ `name` _`interface`_ `.` _`VLAN_ID`_ `type vlan id` _`VLAN_ID`_ `phoenix ~# ip addr add` _`PHX_IP_address`_ `/24 dev` _`interface`_ `.` _`VLAN_ID`_ `phoenix ~# ip link set dev` _`interface`_ `.` _`VLAN_ID`_ `up` 

Platform | Manual Firmware Updates | **40** 

`phoenix ~# ip link show phoenix ~# ip route add default via` _`CVM_default_gateway`_ `dev` _`interface`_ `.` _`VLAN_ID`_ 

- For _`interface`_ a , use the active interface you identified in step 2. 

- For _`PHX_IP_address`_ Se , use the IP address that you identified in step 3. 

> In the following example, the interface is — `eth2` and the VLAN ID is —_ `691` . 

`phoenix ~# ifconfig eth2 up phoenix ~# ip link add link eth2 name eth2.691 type vlan id 691 phoenix ~# ip addr add 198.51.100.10/24 dev eth2.691 phoenix ~# ip link set dev eth2.691 up phoenix ~# ip link show phoenix ~# ip route add default via 198.51.100.16 dev eth2.691` 

- » If the CVM does not have a VLAN configured: 

`phoenix ~# ip link set dev` _`interface`_ `up phoenix ~# ifconfig` _`interface PHX_IP_address`_ `phoenix ~# ifconfig` _`interface`_ `netmask` _`CVM_subnet_mask`_ `phoenix ~# ip route add default via` _`CVM_default_gateway`_ `dev` _`interface`_ 

- For _`interface`_ ~~a~~ , use the active interface you identified in step 2. 

- For _`PHX_IP_address`_ ~~a~~ , use the IP address that you identified in step 3. 

> In the following example, the interface is — `eth2` . 

`phoenix ~# ip link set dev eth2 up phoenix ~# ifconfig eth2 198.51.100.10 phoenix ~# ifconfig eth2 netmask 255.255.255.0 phoenix ~# ip route add default via 198.51.100.16 dev eth2` 

**15.** Test connectivity by pinging another CVM or the gateway. 

**16.** Enable the rescue shell. 

   - a. Change your working directory to the directory that contains the `do_rescue_shell.sh` script. 

`phoenix ~# cd /root` 

- b. Set executable permissions on the rescue shell script. 

`phoenix/root~# chmod +x do_rescue_shell.sh` 

- c. Run the rescue shell script. 

`phoenix/root~# sh do_rescue_shell.sh` 

**17.** Return to the Phoenix prompt. 

~~OO~~ `phoenix/root~# cd` 

**18.** Enter the `lsscsi` command to find the device name of the drive you want to update. 

`phoenix ~# lsscsi` 

Platform | Manual Firmware Updates | **41** 

|[0:0:0:0]    disk    ATA    SAMSUNG MZ7KM1T9 104Q    /dev/sda|[0:0:0:0]    disk    ATA    SAMSUNG MZ7KM1T9 104Q    /dev/sda|[0:0:0:0]    disk    ATA    SAMSUNG MZ7KM1T9 104Q    /dev/sda|[0:0:0:0]    disk    ATA    SAMSUNG MZ7KM1T9 104Q    /dev/sda|
|---|---|---|---|
|[0:0:1:0]    disk    ATA    SAMSUNG MZ7KM1T9 104Q    /dev/sdb|[0:0:1:0]    disk    ATA    SAMSUNG MZ7KM1T9 104Q    /dev/sdb|[0:0:1:0]    disk    ATA    SAMSUNG MZ7KM1T9 104Q    /dev/sdb|[0:0:1:0]    disk    ATA    SAMSUNG MZ7KM1T9 104Q    /dev/sdb|
|[0:0:2:0]    disk    ATA    ST6000NM0115-1YZ SN04    /dev/sdc|[0:0:2:0]    disk    ATA    ST6000NM0115-1YZ SN04    /dev/sdc|[0:0:2:0]    disk    ATA    ST6000NM0115-1YZ SN04    /dev/sdc|[0:0:2:0]    disk    ATA    ST6000NM0115-1YZ SN04    /dev/sdc|
|[0:0:3:0]    disk    ATA    ST6000NM0115-1YZ SN04    /dev/sdd|[0:0:3:0]    disk    ATA    ST6000NM0115-1YZ SN04    /dev/sdd|[0:0:3:0]    disk    ATA    ST6000NM0115-1YZ SN04    /dev/sdd|[0:0:3:0]    disk    ATA    ST6000NM0115-1YZ SN04    /dev/sdd|
|[1:0:0:0]    cd/dvd  ATEN   Virtual CDROM    YS0J    /dev/sr0|[1:0:0:0]    cd/dvd  ATEN   Virtual CDROM    YS0J    /dev/sr0|[1:0:0:0]    cd/dvd  ATEN   Virtual CDROM    YS0J    /dev/sr0|[1:0:0:0]    cd/dvd  ATEN   Virtual CDROM    YS0J    /dev/sr0|
|[10:0:0:0]   disk    ATA    SATADOM-SL 3IE3  301N    /dev/sde|[10:0:0:0]   disk    ATA    SATADOM-SL 3IE3  301N    /dev/sde|[10:0:0:0]   disk    ATA    SATADOM-SL 3IE3  301N    /dev/sde|[10:0:0:0]   disk    ATA    SATADOM-SL 3IE3  301N    /dev/sde|



**19.** Run the `smartctl` command (where `/dev/sd` / _`x`_ is the device name of the drive you want to update.) Look in the **Information** section of the output to find out whether your drive uses a SAS or SATA interface. At the same time, make a note of the current firmware version (so you can verify the version after the update.) 

`phoenix ~# smartctl -i /dev/sd` _`x`_ `smartctl 6.4 2015-06-04 r4109 [x86_64-linux-4.10.1] (local build) Copyright (C) 2002-15, Bruce Allen, Christian Franke, www.smartmontools.org === START OF INFORMATION SECTION === Device Model:     SAMSUNG MZ7KM1T9HMJP-00005 Serial Number:    S3F6NY0J400685 LU WWN Device Id: 5 002538 c000dae18 Firmware Version: GXM5104Q User Capacity:    1,920,383,410,176 bytes [1.92 TB] Sector Size:      512 bytes logical/physical Rotation Rate:    Solid State Device Form Factor:      2.5 inches Device is:        Not in smartctl database [for details use: -P showall] ATA Version is:   ACS-2, ATA8-ACS T13/1699-D revision 4c SATA Version is:  SATA 3.1, 6.0 Gb/s (current: 6.0 Gb/s) Local Time is:    Tue Jul  3 00:36:40 2018 UTC SMART support is: Available - device has SMART capability. SMART support is: Enabled` 

**20.** Download the firmware update binary for your drive from the list available at this link: KB 6937. 

**21.** Change to the — `/root` directory if you are not there already. `cd /root` ~~CS~~ CS‘; 

**22.** Copy the firmware update binary to the Phoenix IP address you assigned in step 16, using a secure copy command such as `scp` . 

For example, to copy the firmware to the `/root` directory, use the following command. 

> `scp` ~~ee~~ _`firmware_binary`_ `root@` _`phoenix_ip_address`_ `:/root/` 

## **23.** Update the firmware. 

- » For a SATA drive or a G6 platform M.2 drive: At the Phoenix prompt, enter the following command, where _`binary-file-name`_ ~~ee~~ is the firmware update binary and `/dev/sd` _`x`_ / is the device name of the drive: 

Platform | Manual Firmware Updates | **42** 

`phoenix ~# hdparm --fwdownload` _`binary-file-name`_ `--yes-i-know-what-i-am-doing -- please-destroy-my-drive /dev/sd` _`x`_ 

**Note:** The command does not affect any data on your drive. It only updates the firmware. The `-please-destroy-my-drive` flag does not actually destroy the drive. It is safe to proceed with the firmware update. 

- » For a SAS drive: At the Phoenix prompt, enter the following command, where ee _`binary-file-name`_ is the firmware update binary and `/dev/sd` / _`x`_ is the device name of the drive: 

`phoenix ~# sg_write_buffer -v -I` _`binary-file-name`_ `-m 7 -S 0 -b 0x10000 /dev/sd` _`x`_ 

**24.** Run the `smartctl` command (where `/dev/sd` / _`x`_ is the device name of the updated drive) and look in the **Information** section of the output to verify the firmware update. 

`phoenix ~# smartctl -i /dev/sd` _`x`_ `smartctl 6.4 2015-06-04 r4109 [x86_64-linux-4.10.1] (local build) Copyright (C) 2002-15, Bruce Allen, Christian Franke, www.smartmontools.org === START OF INFORMATION SECTION === Device Model:     SAMSUNG MZ7KM1T9HMJP-00005 Serial Number:    S3F6NY0HB00176 LU WWN Device Id: 5 002538 c000af0b6 Firmware Version: GXM5304Q User Capacity:    1,920,383,410,176 bytes [1.92 TB] Sector Size:      512 bytes logical/physical Rotation Rate:    Solid State Device Form Factor:      2.5 inches Device is:        Not in smartctl database [for details use: -P showall] ATA Version is:   ACS-2, ATA8-ACS T13/1699-D revision 4c SATA Version is:  SATA 3.1, 6.0 Gb/s (current: 6.0 Gb/s) Local Time is:    Tue Jul  3 01:40:43 2018 UTC SMART support is: Available - device has SMART capability. SMART support is: Enabled` 

**25.** From the console menu, select **Virtual Media** > **Virtual Storage** to unmount the ISO. 

**26.** Disconnect from the IPMI WebUI and restart the host. 

   - » If the drive is part of a single-node cluster, follow the procedures in Starting a Single-node Cluster on page 55. 

   - » If the drive is part of a multinode cluster, follow the procedures in Starting a Node in a Multinode Cluster on page 58. 

**27.** Check the state of the cluster. 

   - a. Make sure that all hosts are part of the metadata ring. 

~~CSCS~~ `nutanix@cvm$ nodetool -h 0 ring` 

- b. Check cluster data resiliency. 

~~SCS~~ `ncli> cluster get-domain-fault-tolerance-status type=node` 

**28.** To show the updated firmware in the UI, open LCM from Prism and select **Inventory** > **Perform Inventory** in the LCM dashboard. 

Platform | Manual Firmware Updates | **43** 

## **Manually Updating the BMC and BIOS** 

Manually update BMC and BIOS versions. 

## **Before you begin** 

Before updating, follow the node preparation and shutdown procedures described above. 

## **About this task** 

**Note:** If you are upgrading both the BMC and the BIOS, upgrade the BMC first. 

## **Procedure** 

**1.** Update the BMC by following the procedures described in the Nutanix BMC Manual Upgrade Guide. 

**2.** Update the BIOS by following the procedures described in the Nutanix BIOS Manual Upgrade Guide. 

**3.** To show the updated BMC and BIOS versions in the UI, open LCM from Prism and select **Inventory** > **Perform Inventory** in the LCM dashboard. 

## **Manually Updating HBA Controller Firmware (G6 and G7 Platforms)** 

Update the firmware for an HBA card on G6 or G7 platforrms. 

## **About this task** 

To update HBA firmware, you need an ISO provided by Nutanix that includes the binary and the `sas3flash` utility. Download the firmware update ISO for your HBA from the list available at this link: KB 6937. Contact Nutanix support if you need further assistance. 

To update the HBA firmware on G8, NG8, or G9 platforms, see Manually Updating HBA Controller Firmware (G8 and Newer Platforms) on page 49. 

## **Procedure** 

**1.** Download the HBA controller firmware update ISO. 

**2.** Shut down the CVM and the node. 

   - » If the HBA card is in a single-node cluster, follow the procedures in Shutting Down a Single-node Cluster on page 16. 

   - » If the HBA card is in a multinode cluster, follow the procedures in Shutting Down a Node in a Multinode Cluster on page 19. 

**3.** From the system where you downloaded the update ISO, enter the IPMI IP address of the node in a web browser to reach the IPMI web UI for the node. 

Platform | Manual Firmware Updates | **44** 

**4.** In the IPMI web UI, select **Remote Console** in the sidebar. 

## **Figure 14: Remote console** 

**5.** Click on the word **here** in the line **To set the Remote Console default interface, please click here** . 

**Figure 15: Setting the interface** 

**6.** In the **Remote Console Settings** page that appears, select the **HTML5** radio button and click **Save** . You are returned to the **Remote Control** page. 

**7.** Click **Launch Console** . 

Platform | Manual Firmware Updates | **45** 

**8.** From the console menu, select **Virtual Media** . 

**Figure 16: Virtual media** 

Platform | Manual Firmware Updates | **46** 

**9.** In the **Virtual Media** dialog that appears, specify the settings. 

**Figure 17: Specifying the image** 

- a. Make sure that the **Device Type** field is set to **ISO Image** . 

- b. Click **Browse** , navigate to the location where you downloaded the ISO file, and click **Open** . 

- c. Click **Mount** to mount the ISO on the node. 

The mounted device now appears in the **Virtual Media** dialog. 

**Figure 18: Mounted image** 

Platform | Manual Firmware Updates | **47** 

**10.** In the remote console, select **Power Control** > **Set Power On** to turn on the node and restart from the ISO. 

**Figure 19: Turn on the node** 

When the host restarts into the ISO, the `sas3flash` utility performs the update automatically. 

`Avago Technologies SAS3 Flash Utility Version 09.00.00.00 (2025.02.03) Copyright 2008-2025 Avago Technologies. All rights reserved. Adapter Selected is a Avago SAS: SAS3008(C0) Num   Ctlr            FW Ver        NVDATA        x86-BIOS         PCI Addr ---------------------------------------------------------------------------0  SAS3008(C0)  14.00.00.00    0e.00.30.28    08.31.03.00     00:00:05:00 Finished Processing Commands Successfully. Exiting SAS3Flash. Reached End >_` 

**11.** In the **Virtual Storage** dialog box, click **Unmount** to unmount the ISO. 

**12.** Disconnect from the IPMI web UI and restart the host. 

   - » If the HBA card is in a single-node cluster, follow the procedures in Starting a Single-node Cluster on page 55. 

   - » If the HBA card is in a multinode cluster, follow the procedures in Starting a Node in a Multinode Cluster on page 58. 

Platform | Manual Firmware Updates | **48** 

## **13.** Check the state of the cluster. 

a. Make sure that all hosts are part of the metadata ring. 

~~ee~~ `nutanix@cvm$ nodetool -h 0 ring` 

- b. Check cluster data resiliency. 

~~CSCS~~ `ncli> cluster get-domain-fault-tolerance-status type=node` 

**14.** From the CVM, verify the firmware version. 

`nutanix@cvm$ sudo /usr/local/nutanix/bootstrap/lib/lsi-sas/sas3flash -list` 

`Avago Technologies SAS3 Flash Utility Version 09.00.00.00 (2025.02.03) Copyright 2008-2025 Avago Technologies. All rights reserved. Adapter Selected is a Avago SAS: SAS3008(C0) Controller Number              : 0 Controller                     : SAS3008(C0) PCI Address                    : 00:00:05:00 SAS Address                    : 5003048-0-1977-6201 NVDATA Version (Default)       : 0e.00.30.28 NVDATA Version (Persistent)    : 0e.00.30.28 Firmware Product ID            : 0x2221 (IT) Firmware Version               : 14.00.00.00 NVDATA Vendor                  : LSI NVDATA Product ID              : LSI3008-IT BIOS Version                   : 08.31.03.00 UEFI BSD Version               : 12.00.00.00 FCODE Version                  : N/A Board Name                     : LSI3008-IT Board Assembly                 : N/A Board Tracer Number            : N/A Finished Processing Commands Successfully. Exiting SAS3Flash.` 

**15.** To show the updated firmware in the UI, open LCM from Prism and select **Inventory** > **Perform Inventory** in the LCM dashboard. 

## **Manually Updating HBA Controller Firmware (G8 and Newer Platforms)** 

Update the firmware for a SAS3816 or SAS3808 HBA card on NX G8 and newer platforms. 

## **About this task** 

NX-8170-G8 and NX-8170N-G8 platforms do not have an HBA card. All other G8, NG8, G9, and G10 platforms with SAS3808 and SAS3816 HBA cards can be updated with this procedure. 

To update HBA firmware, you need an ISO provided by Nutanix that includes the binary and the StorCLI utility. Download the firmware update ISO for your HBA from the list available at this link: KB 6937. For further assistance, contact Nutanix Support. 

To update HBA firmware on G6 and G7 platforms, see Manually Updating HBA Controller Firmware (G6 and G7 Platforms) on page 44. 

Platform | Manual Firmware Updates | **49** 

## **Procedure** 

**1.** Download the firmware update ISO for your HBA from the list available at this link: KB 6937. 

**2.** Shut down the CVM and the node. 

   - » If the HBA card is in a single-node cluster, follow the procedures in Shutting Down a Single-node Cluster on page 16. 

   - » If the HBA card is in a multinode cluster, follow the procedures in Shutting Down a Node in a Multinode Cluster on page 19. 

**3.** Access the IPMI web user interface for the node. From the system where you downloaded the update ISO, enter the IPMI IP address of the node. 

**4.** In the IPMI web user interface, launch the remote console by selecting **Remote Control** . 

**Figure 20: Launch Console Window** 

**5.** Click the **Launch Console** button. 

**6.** From the console menu, select **Virtual Media** . 

**Figure 21: Virtual Media Dialog** 

Platform | Manual Firmware Updates | **50** 

**7.** In the **Virtual Media** dialog box, specify the storage settings. 

   - a. In the **Device Type** field, select **ISO Image** . 

   - b. In the **Image File Name and Full Path** field, select **Open Image** , browse to the location where you downloaded the ISO file, and click **Open** . 

   - c. Select **Mount** to mount the ISO on the node. 

## **Figure 22: Specifying the Image** 

**8.** Click **Mount** . 

Platform | Manual Firmware Updates | **51** 

**9.** Start the node in order to restart from the ISO. 

**Figure 23: Start the Node** 

When the host restarts into the ISO, the `StorCLI` utility performs the update automatically. When the root prompt is displayed, the upgrade is complete. 

**Figure 24: StorCLI Performs Update Automatically** 

**10.** Run the following command to confirm the firmware update from Phoenix. 

`[root@phoenix ~]# /opt/MegaRAID/storcli/storcli64 /c0 show` 

Platform | Manual Firmware Updates | **52** 

**11.** Verify that the controller firmware is updated. 

**Figure 25: StorCLI Utility Reports Firmware Version** 

**12.** In the **Virtual Media** dialog box, click **Unmount** to unmount the ISO. 

**13.** Disconnect from the IPMI web user interface and restart the host. 

   - » If the HBA card is in a single-node cluster, follow the procedures in Starting a Single-node Cluster on page 55. 

   - » If the HBA card is in a multinode cluster, follow the procedures in Starting a Node in a Multinode Cluster on page 58. 

**14.** Check the state of the cluster. 

   - a. Make sure that all hosts are part of the metadata ring. 

`nutanix@cvm$ nodetool -h 0 ring` 

- b. Check cluster data resiliency. 

`ncli> cluster get-domain-fault-tolerance-status type=node` 

**15.** From the Controller VM (CVM), verify the firmware version. 

**Caution:** Do not run the `sas3flash` command on G8 or NG8 platforms. That command can cause the CVM to stop. 

`nutanix@NTNX-S411801X0A04013-A-CVM:10.x.x.x:~/cluster/lib/storcli$ sudo ./ storcli64 /call show CLI Version = 007.2002.0000.0000 Oct 11, 2021 Operating system = Linux 3.10.0-1160.66.1.el7.nutanix.20220626.cvm.x86_64` 

Platform | Manual Firmware Updates | **53** 

`Controller = 0 Status = Success Description = None Product Name = SAS3816 Serial Number = UA2x3S009x50R1x1 SAS Address =  50030480243305f0 PCI Address = 00:00:06:00 System Time = 09/13/2022 08:31:24 FW Package Build = 23.00.00.01 FW Version = 23.00.00.00 BIOS Version = 09.45.00.00_23.00.00.00 NVDATA Version = 23.00.00.12 Driver Name = mpt3sas Driver Version = 33.00.00.00 Bus Number = 0 Device Number = 6 Function Number = 0 Domain ID = 0 Vendor Id = 0x1000 Device Id = 0xE6 SubVendor Id = 0x15D9 SubDevice Id = 0x1B65 Board Name = SAS3816 Board Assembly = 032621235419B001 Board Tracer Number = UA2x3S009x50R1x1 Security Protocol = None Package Stamp Mismatch = No Physical Drives = 8 PD LIST : =======` 

`--------------------------------------------------------------------------------EID:Slt DID State DG     Size Intf Med SED PI SeSz Model                      Sp --------------------------------------------------------------------------------0:0       1 JBOD  -  1.746 TB SATA SSD -   -  512B SAMSUNG MZ7LH1T9HMLT-00005 - 0:1       2 JBOD  -  1.746 TB SATA SSD -   -  512B SAMSUNG MZ7LH1T9HMLT-00005 - 0:2       3 JBOD  -  1.746 TB SATA SSD -   -  512B SAMSUNG MZ7LH1T9HMLT-00005 - 0:3       7 JBOD  -  1.746 TB SATA SSD -   -  512B SAMSUNG MZ7LH1T9HMLT-00005 - 0:4       4 JBOD  -  1.746 TB SATA SSD -   -  512B SAMSUNG MZ7LH1T9HMLT-00005 - 0:5       8 JBOD  -  1.746 TB SATA SSD -   -  512B SAMSUNG MZ7LH1T9HMLT-00005 - 0:6       5 JBOD  -  1.746 TB SATA SSD -   -  512B SAMSUNG MZ7LH1T9HMLT-00005 - 0:7       6 JBOD  -  1.746 TB SATA SSD -   -  512B SAMSUNG MZ7LH1T9HMLT-00005 - ---------------------------------------------------------------------------------` 

`EID-Enclosure Device ID|Slt-Slot No|DID-Device ID|DG-DriveGroup UGood-Unconfigured Good|UBad-Unconfigured Bad|Intf-Interface Med-Media Type|SED-Self Encryptive Drive|PI-Protection Info SeSz-Sector Size|Sp-Spun|U-Up|D-Down|T-Transition` 

`Requested Boot Drive = Not Set` 

**16.** To show the updated firmware in the UI, open LCM from Prism and select **Inventory** > **Perform Inventory** in the LCM dashboard. 

## **Manually Updating NIC Firmware** 

LCM supports most NIC firmware updates. Make sure that your cluster is running the latest version of LCM. Run an LCM inventory to check whether NIC firmware updates are available and then use LCM to update the NIC. If the NIC update does not appear in LCM but you need to manually update the firmware 

Platform | Manual Firmware Updates | **54** 

or drivers, see KB 10634. If you are unsure that you need to manually update firmware or drivers, contact Nutanix Support for guidance. 

## **Starting a Single-node Cluster** 

After updating firmware in a single-node cluster, restart the node using the method for your hypervisor. 

## **Starting a Single Node (vSphere Client)** 

## **About this task** 

## **Procedure** 

**1.** Turn on the node with one of the following methods: 

   - » Press the power button. 

   - » Use the IPMI Web Interface. 

If the node is already powered on proceed to the next step. 

**2.** Log on to the vSphere Client. 

**3.** Right-click the ESXi host and select **Exit Maintenance Mode** . 

**4.** Right-click the Controller VM and select **Power** > **Power on** . 

Wait approximately 5 minutes for all services to start on the Controller VM. 

**5.** Right-click the ESXi host in the vSphere client and select **Rescan for Datastores** . Confirm that all Nutanix datastores are available. 

**6.** Verify that all services are up on all Controller VMs. 

> `nutanix@cvm$ cluster status` ~~SSS~~ 

If the cluster is running properly, output similar to the following is displayed for each node in the cluster: 

|CVM: 10.1.64.60 Up|CVM: 10.1.64.60 Up|CVM: 10.1.64.60 Up|
|---|---|---|
|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|
|Scavenger   UP       [6174, 6215, 6216, 6217]|Scavenger   UP       [6174, 6215, 6216, 6217]|Scavenger   UP       [6174, 6215, 6216, 6217]|
|SSLTerminator   UP       [7705, 7742, 7743, 7744]|SSLTerminator   UP       [7705, 7742, 7743, 7744]|SSLTerminator   UP       [7705, 7742, 7743, 7744]|
|SecureFileSync   UP       [7710, 7761, 7762, 7763]|SecureFileSync   UP       [7710, 7761, 7762, 7763]|SecureFileSync   UP       [7710, 7761, 7762, 7763]|
|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|
|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|
|Pithos   UP       [8328, 8399, 8400, 8418]|Pithos   UP       [8328, 8399, 8400, 8418]|Pithos   UP       [8328, 8399, 8400, 8418]|
|Hera   UP       [8347, 8408, 8409, 8410]|Hera   UP       [8347, 8408, 8409, 8410]|Hera   UP       [8347, 8408, 8409, 8410]|
|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|
|InsightsDB   UP       [8774, 8805, 8806, 8939]|InsightsDB   UP       [8774, 8805, 8806, 8939]|InsightsDB   UP       [8774, 8805, 8806, 8939]|
|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|
|8890]|||
|Ergon   UP       [8814, 8862, 8863, 8864]|Ergon   UP       [8814, 8862, 8863, 8864]|Ergon   UP       [8814, 8862, 8863, 8864]|
|Cerebro   UP       [8850, 8914, 8915, 9288]|Cerebro   UP       [8850, 8914, 8915, 9288]|Cerebro   UP       [8850, 8914, 8915, 9288]|
|Chronos   UP       [8870, 8975, 8976, 9031]|Chronos   UP       [8870, 8975, 8976, 9031]|Chronos   UP       [8870, 8975, 8976, 9031]|
|Curator   UP       [8885, 8931, 8932, 9243]|Curator   UP       [8885, 8931, 8932, 9243]|Curator   UP       [8885, 8931, 8932, 9243]|
|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|
|CIM   UP       [8990, 9042, 9043, 9084]|CIM   UP       [8990, 9042, 9043, 9084]|CIM   UP       [8990, 9042, 9043, 9084]|
|AlertManager   UP       [9017, 9081, 9082, 9324]|AlertManager   UP       [9017, 9081, 9082, 9324]|AlertManager   UP       [9017, 9081, 9082, 9324]|
|Arithmos   UP       [9055, 9217, 9218, 9353]|Arithmos   UP       [9055, 9217, 9218, 9353]|Arithmos   UP       [9055, 9217, 9218, 9353]|
|Catalog   UP       [9110, 9178, 9179, 9180]|Catalog   UP       [9110, 9178, 9179, 9180]|Catalog   UP       [9110, 9178, 9179, 9180]|
|Acropolis   UP       [9201, 9321, 9322, 9323]|Acropolis   UP       [9201, 9321, 9322, 9323]|Acropolis   UP       [9201, 9321, 9322, 9323]|
|Atlas   UP       [9221, 9316, 9317, 9318]|Atlas   UP       [9221, 9316, 9317, 9318]|Atlas   UP       [9221, 9316, 9317, 9318]|



Platform | Manual Firmware Updates | **55** 

|Uhura   UP       [9390, 9447, 9448, 9449]|Uhura   UP       [9390, 9447, 9448, 9449]|Uhura   UP       [9390, 9447, 9448, 9449]|Uhura   UP       [9390, 9447, 9448, 9449]|
|---|---|---|---|
|Snmp   UP       [9418, 9513, 9514, 9516]|Snmp   UP       [9418, 9513, 9514, 9516]|Snmp   UP       [9418, 9513, 9514, 9516]|Snmp   UP       [9418, 9513, 9514, 9516]|
|SysStatCollector   UP       [9451, 9510, 9511, 9518]|SysStatCollector   UP       [9451, 9510, 9511, 9518]|SysStatCollector   UP       [9451, 9510, 9511, 9518]|SysStatCollector   UP       [9451, 9510, 9511, 9518]|
|Tunnel   UP       [9480, 9543, 9544]|Tunnel   UP       [9480, 9543, 9544]|Tunnel   UP       [9480, 9543, 9544]|Tunnel   UP       [9480, 9543, 9544]|
|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|
|10301]||||
|Janus   UP       [9532, 9624, 9625]|Janus   UP       [9532, 9624, 9625]|Janus   UP       [9532, 9624, 9625]|Janus   UP       [9532, 9624, 9625]|
|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|
|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|
|ClusterConfig   UP       [10205, 10233, 10234, 10236]|ClusterConfig   UP       [10205, 10233, 10234, 10236]|ClusterConfig   UP       [10205, 10233, 10234, 10236]|ClusterConfig   UP       [10205, 10233, 10234, 10236]|
|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|
|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|
|10503]||||
|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|
|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|
|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|



## **Starting a Single Node (AHV)** 

## **About this task** 

## **Procedure** 

**1.** Turn on the node with one of the following methods: 

   - » Press the power button. 

   - » Use the IPMI Web Interface. 

If the node is already powered on proceed to the next step. 

**2.** Log on to the AHV host with SSH. 

**3.** Find the name of the Controller VM. 

> `root@ahv# virsh list --all | grep CVM` ~~SSS~~ Make a note of the Controller VM name in the second column. 

**4.** Determine if the Controller VM is running. 

   - If the Controller VM is off, a line similar to the following should be returned: 

      - `NTNX-12AM2K470031-D-CVM        shut off` ~~CSCC~~ Make a note of the Controller VM name in the second column. 

   - If the Controller VM is on, a line similar to the following should be returned: 

      - `NTNX-12AM2K470031-D-CVM        running` ~~SCS~~ 

**5.** If the Controller VM is shut off, start it. 

- `root@ahv# virsh start` ~~e~~ _`cvm_name`_ ~~e~~ 

> Replace a _`cvm_name`_ with the name of the Controller VM that you found from the preceding command. Wait approximately 5 minutes for all services to start on the Controller VM. 

Platform | Manual Firmware Updates | **56** 

**6.** Verify that all services are up on all Controller VMs. 

> `nutanix@cvm$ cluster status` ~~SSS~~ If the cluster is running properly, output similar to the following is displayed for each node in the cluster: 

|CVM: 10.1.64.60 Up|CVM: 10.1.64.60 Up|CVM: 10.1.64.60 Up|
|---|---|---|
|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|
|Scavenger   UP       [6174, 6215, 6216, 6217]|Scavenger   UP       [6174, 6215, 6216, 6217]|Scavenger   UP       [6174, 6215, 6216, 6217]|
|SSLTerminator   UP       [7705, 7742, 7743, 7744]|SSLTerminator   UP       [7705, 7742, 7743, 7744]|SSLTerminator   UP       [7705, 7742, 7743, 7744]|
|SecureFileSync   UP       [7710, 7761, 7762, 7763]|SecureFileSync   UP       [7710, 7761, 7762, 7763]|SecureFileSync   UP       [7710, 7761, 7762, 7763]|
|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|
|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|
|Pithos   UP       [8328, 8399, 8400, 8418]|Pithos   UP       [8328, 8399, 8400, 8418]|Pithos   UP       [8328, 8399, 8400, 8418]|
|Hera   UP       [8347, 8408, 8409, 8410]|Hera   UP       [8347, 8408, 8409, 8410]|Hera   UP       [8347, 8408, 8409, 8410]|
|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|
|InsightsDB   UP       [8774, 8805, 8806, 8939]|InsightsDB   UP       [8774, 8805, 8806, 8939]|InsightsDB   UP       [8774, 8805, 8806, 8939]|
|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|
|8890]|||
|Ergon   UP       [8814, 8862, 8863, 8864]|Ergon   UP       [8814, 8862, 8863, 8864]|Ergon   UP       [8814, 8862, 8863, 8864]|
|Cerebro   UP       [8850, 8914, 8915, 9288]|Cerebro   UP       [8850, 8914, 8915, 9288]|Cerebro   UP       [8850, 8914, 8915, 9288]|
|Chronos   UP       [8870, 8975, 8976, 9031]|Chronos   UP       [8870, 8975, 8976, 9031]|Chronos   UP       [8870, 8975, 8976, 9031]|
|Curator   UP       [8885, 8931, 8932, 9243]|Curator   UP       [8885, 8931, 8932, 9243]|Curator   UP       [8885, 8931, 8932, 9243]|
|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|
|CIM   UP       [8990, 9042, 9043, 9084]|CIM   UP       [8990, 9042, 9043, 9084]|CIM   UP       [8990, 9042, 9043, 9084]|
|AlertManager   UP       [9017, 9081, 9082, 9324]|AlertManager   UP       [9017, 9081, 9082, 9324]|AlertManager   UP       [9017, 9081, 9082, 9324]|
|Arithmos   UP       [9055, 9217, 9218, 9353]|Arithmos   UP       [9055, 9217, 9218, 9353]|Arithmos   UP       [9055, 9217, 9218, 9353]|
|Catalog   UP       [9110, 9178, 9179, 9180]|Catalog   UP       [9110, 9178, 9179, 9180]|Catalog   UP       [9110, 9178, 9179, 9180]|
|Acropolis   UP       [9201, 9321, 9322, 9323]|Acropolis   UP       [9201, 9321, 9322, 9323]|Acropolis   UP       [9201, 9321, 9322, 9323]|
|Atlas   UP       [9221, 9316, 9317, 9318]|Atlas   UP       [9221, 9316, 9317, 9318]|Atlas   UP       [9221, 9316, 9317, 9318]|
|Uhura   UP       [9390, 9447, 9448, 9449]|Uhura   UP       [9390, 9447, 9448, 9449]|Uhura   UP       [9390, 9447, 9448, 9449]|
|Snmp   UP       [9418, 9513, 9514, 9516]|Snmp   UP       [9418, 9513, 9514, 9516]|Snmp   UP       [9418, 9513, 9514, 9516]|
|SysStatCollector   UP       [9451, 9510, 9511, 9518]|SysStatCollector   UP       [9451, 9510, 9511, 9518]|SysStatCollector   UP       [9451, 9510, 9511, 9518]|
|Tunnel   UP       [9480, 9543, 9544]|Tunnel   UP       [9480, 9543, 9544]|Tunnel   UP       [9480, 9543, 9544]|
|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|
|10301]|||
|Janus   UP       [9532, 9624, 9625]|Janus   UP       [9532, 9624, 9625]|Janus   UP       [9532, 9624, 9625]|
|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|
|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|
|ClusterConfig   UP       [10205, 10233, 10234, 10236]|ClusterConfig   UP       [10205, 10233, 10234, 10236]|ClusterConfig   UP       [10205, 10233, 10234, 10236]|
|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|
|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|
|10503]|||
|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|
|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|
|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|



**7.** If the node is in maintenance mode, log on to the Controller VM and take the node out of maintenance mode. `nutanix@cvm$ acli` ~~ee~~ `<acropolis> host.exit_maintenance_mode` ~~e~~ _`AHV-hypervisor-IP-address`_ ~~e~~ Replace ~~ES~~ _`AHV-hypervisor-IP-address`_ with the AHV IP address. `<acropolis> exit` ~~ee~~ 

## **Node Startup Post-Check** 

Check the health of the cluster after starting a node. 

Platform | Manual Firmware Updates | **57** 

## **Procedure** 

**1.** Make sure that the system is running the latest supported firmware, BIOS, and BMC versions. 

Nutanix recommends performing an LCM inventory and then running the relevant updates by following the Life Cycle Manager Guide. 

In scenarios where you cannot run LCM, manually update the firmware. For more information, see Manual Firmware Updates in the _NX Series Hardware Administration Guide_ . 

Optionally, after the manual firmware update, you can run the following command from a CVM and check the output to confirm the firmware versions: 

`nutanix@cvm$ ncc hardware_info show_hardware_info --cvm=` _`cvm_ipaddress`_ 

**2.** In Prism Element web console, go to the **Health** page and select **Actions** > **Run NCC Checks** . 

**3.** In the dialog box that appears, select **All Checks** and click **Run** . 

Alternatively, issue the following command from the CVM: 

`nutanix@cvm$ ncc health_checks run_all` 

If any checks fail, see the related KB article provided in the output and the _Nutanix Cluster Check Guide: NCC Reference_ for information on resolving the issue. 

**4.** If you have any unresolvable failed checks, contact Nutanix Support. 

## **Starting a Node in a Multinode Cluster** 

After updating firmware in a cluster, restart the node, using the method for your hypervisor. 

## **Starting a Node in a Cluster through the vSphere Client** 

## **Before you begin** 

Get the encryption recovery key if you have a Trusted Platform Module (TPM) installed. You should have listed and stored the key by following Back Up the Encryption Recovery Key for the Trusted Platform Module. 

## **Procedure** 

**1.** If the node is off, turn it on by pressing the power button on the front. Otherwise, proceed to the next step. 

If the node fails to restart, contact Nutanix Support to check the raid status and M2 drive health. 

**2.** Log on to vCenter (or to the node if vCenter is not running) with the vSphere client. 

**3.** If you have a TPM, follow the procedure in VMware’s Recover the Secure ESXi Configuration documentation to input the encryption recovery key. 

VMware KB-81446, "Boot time failures due to ESXi configuration encryption," lists other boot time failures and resolutions surrounding the encryption key. Verify that the TPM module and Secure Boot are enabled in BIOS. 

Follow the best practices for managing the recovery key outlined by VMware’s Managing a Secure ESXi Configuration documentation for managing the recovery key, including rotating the recovery key. Contact Nutanix Support if the node fails to boot as expected. 

**4.** Right-click the ESXi host and select **Exit Maintenance Mode** . 

Platform | Manual Firmware Updates | **58** 

**5.** Right-click the Controller VM and select **Power** > **Power on** . 

Wait approximately 5 minutes for all services to start on the Controller VM. 

**6.** Log on to another Controller VM in the cluster with SSH. 

**7.** Confirm that cluster services are running on the Controller VM. 

   - `nutanix@cvm$ ncli cluster status | grep -A 15` ~~ee~~ _`cvm_ip_addr`_ 

Output similar to the following is displayed. 

`Name                      : 10.1.56.197 Status                    : Up ... ... StatsAggregator           : up SysStatCollector          : up` 

Every service listed should be `up` . 

**8.** Right-click the ESXi host in the vSphere client and select **Rescan for Datastores** . Confirm that all Nutanix datastores are available. 

**9.** Verify that all services are up on all Controller VMs. 

   - `nutanix@cvm$ cluster status` ~~SSS~~ 

If the cluster is running properly, output similar to the following is displayed for each node in the cluster: 

|CVM: 10.1.64.60 Up|CVM: 10.1.64.60 Up|CVM: 10.1.64.60 Up|
|---|---|---|
|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|Zeus   UP       [5362, 5391, 5392, 10848, 10977, 10992]|
|Scavenger   UP       [6174, 6215, 6216, 6217]|Scavenger   UP       [6174, 6215, 6216, 6217]|Scavenger   UP       [6174, 6215, 6216, 6217]|
|SSLTerminator   UP       [7705, 7742, 7743, 7744]|SSLTerminator   UP       [7705, 7742, 7743, 7744]|SSLTerminator   UP       [7705, 7742, 7743, 7744]|
|SecureFileSync   UP       [7710, 7761, 7762, 7763]|SecureFileSync   UP       [7710, 7761, 7762, 7763]|SecureFileSync   UP       [7710, 7761, 7762, 7763]|
|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|
|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|
|Pithos   UP       [8328, 8399, 8400, 8418]|Pithos   UP       [8328, 8399, 8400, 8418]|Pithos   UP       [8328, 8399, 8400, 8418]|
|Hera   UP       [8347, 8408, 8409, 8410]|Hera   UP       [8347, 8408, 8409, 8410]|Hera   UP       [8347, 8408, 8409, 8410]|
|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|
|InsightsDB   UP       [8774, 8805, 8806, 8939]|InsightsDB   UP       [8774, 8805, 8806, 8939]|InsightsDB   UP       [8774, 8805, 8806, 8939]|
|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889,|
|8890]|||
|Ergon   UP       [8814, 8862, 8863, 8864]|Ergon   UP       [8814, 8862, 8863, 8864]|Ergon   UP       [8814, 8862, 8863, 8864]|
|Cerebro   UP       [8850, 8914, 8915, 9288]|Cerebro   UP       [8850, 8914, 8915, 9288]|Cerebro   UP       [8850, 8914, 8915, 9288]|
|Chronos   UP       [8870, 8975, 8976, 9031]|Chronos   UP       [8870, 8975, 8976, 9031]|Chronos   UP       [8870, 8975, 8976, 9031]|
|Curator   UP       [8885, 8931, 8932, 9243]|Curator   UP       [8885, 8931, 8932, 9243]|Curator   UP       [8885, 8931, 8932, 9243]|
|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076]|
|CIM   UP       [8990, 9042, 9043, 9084]|CIM   UP       [8990, 9042, 9043, 9084]|CIM   UP       [8990, 9042, 9043, 9084]|
|AlertManager   UP       [9017, 9081, 9082, 9324]|AlertManager   UP       [9017, 9081, 9082, 9324]|AlertManager   UP       [9017, 9081, 9082, 9324]|
|Arithmos   UP       [9055, 9217, 9218, 9353]|Arithmos   UP       [9055, 9217, 9218, 9353]|Arithmos   UP       [9055, 9217, 9218, 9353]|
|Catalog   UP       [9110, 9178, 9179, 9180]|Catalog   UP       [9110, 9178, 9179, 9180]|Catalog   UP       [9110, 9178, 9179, 9180]|
|Acropolis   UP       [9201, 9321, 9322, 9323]|Acropolis   UP       [9201, 9321, 9322, 9323]|Acropolis   UP       [9201, 9321, 9322, 9323]|
|Atlas   UP       [9221, 9316, 9317, 9318]|Atlas   UP       [9221, 9316, 9317, 9318]|Atlas   UP       [9221, 9316, 9317, 9318]|
|Uhura   UP       [9390, 9447, 9448, 9449]|Uhura   UP       [9390, 9447, 9448, 9449]|Uhura   UP       [9390, 9447, 9448, 9449]|
|Snmp   UP       [9418, 9513, 9514, 9516]|Snmp   UP       [9418, 9513, 9514, 9516]|Snmp   UP       [9418, 9513, 9514, 9516]|
|SysStatCollector   UP       [9451, 9510, 9511, 9518]|SysStatCollector   UP       [9451, 9510, 9511, 9518]|SysStatCollector   UP       [9451, 9510, 9511, 9518]|
|Tunnel   UP       [9480, 9543, 9544]|Tunnel   UP       [9480, 9543, 9544]|Tunnel   UP       [9480, 9543, 9544]|
|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977,|
|10301]|||
|Janus   UP       [9532, 9624, 9625]|Janus   UP       [9532, 9624, 9625]|Janus   UP       [9532, 9624, 9625]|
|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|
|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|
|ClusterConfig   UP       [10205, 10233, 10234, 10236]|ClusterConfig   UP       [10205, 10233, 10234, 10236]|ClusterConfig   UP       [10205, 10233, 10234, 10236]|
|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|



Platform | Manual Firmware Updates | **59** 

|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|
|---|---|---|---|---|---|---|---|
|10503]||||||||
|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]||
|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]||
|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]||



## **Starting a Node in a Cluster (AHV)** 

## **About this task** 

## **Procedure** 

**1.** If the node is off, turn on the node by pressing the power button on the front control panel. The controller VM (CVM) starts automatically when you restart the node. 

   - If the node fails to restart, contact Nutanix Support to check the raid status and M2 drive health. 

**2.** As the Nutanix user, log on to the CVM and verify that all cluster services on all the CVMs are in the UP state: 

   - `nutanix@cvm$ cluster status` ~~ee~~ 

If the cluster is running properly, output similar to the following is displayed for each node in the cluster: 

`CVM: 10.1.64.60 Up Zeus     UP       [5362, 5391, 5392, 10848, 10977, 10992] Scavenger   UP       [6174, 6215, 6216, 6217] SSLTerminator   UP       [7705, 7742, 7743, 7744] SecureFileSync   UP       [7710, 7761, 7762, 7763] Medusa   UP       [8029, 8073, 8074, 8176, 8221] DynamicRingChanger   UP       [8324, 8366, 8367, 8426] Pithos   UP       [8328, 8399, 8400, 8418] Hera   UP       [8347, 8408, 8409, 8410] Stargate   UP       [8742, 8771, 8772, 9037, 9045] InsightsDB   UP       [8774, 8805, 8806, 8939] InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888, 8889, 8890] Ergon   UP       [8814, 8862, 8863, 8864] Cerebro   UP       [8850, 8914, 8915, 9288] Chronos   UP       [8870, 8975, 8976, 9031] Curator   UP       [8885, 8931, 8932, 9243] Prism   UP       [3545, 3572, 3573, 3627, 4004, 4076] CIM   UP       [8990, 9042, 9043, 9084] AlertManager   UP       [9017, 9081, 9082, 9324] Arithmos   UP       [9055, 9217, 9218, 9353] Catalog   UP       [9110, 9178, 9179, 9180] Acropolis   UP       [9201, 9321, 9322, 9323] Atlas   UP       [9221, 9316, 9317, 9318] Uhura   UP       [9390, 9447, 9448, 9449] Snmp   UP       [9418, 9513, 9514, 9516] SysStatCollector   UP       [9451, 9510, 9511, 9518] Tunnel   UP       [9480, 9543, 9544] ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976, 9977, 10301] Janus   UP       [9532, 9624, 9625] NutanixGuestTools   UP       [9572, 9650, 9651, 9674] MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371] ClusterConfig   UP       [10205, 10233, 10234, 10236] APLOSEngine   UP       [10231, 10261, 10262, 10263] APLOS   UP       [10343, 10368, 10369, 10370, 10502, 10503] Lazan   UP       [10377, 10402, 10403, 10404] Orion   UP       [10409, 10449, 10450, 10474]` 

Platform | Manual Firmware Updates | **60** 

`Delphi   UP       [10418, 10466, 10467, 10468]` 

It might take a few minutes to get all services up and running. 

Do not continue if the CVM fails to exit the maintenance mode. 

**3.** Remove the AHV host from the maintenance mode: 

   - a. From any CVM in the cluster, run the following command to exit the AHV host from the maintenance mode: 

`nutanix@cvm$ acli host.exit_maintenance_mode` _`host-ip`_ 

Replace _`host-ip`_ with the new IP address of the host. 

This command live migrates all the VMs that were previously running on the host back to the host. 

- b. As the nutanix user, log on to the CVM and verify that the host exited the maintenance mode: 

`nutanix@cvm$ acli host.get` _`host-ip`_ 

In the output that is displayed, ensure that **node_state** is equal to **kAcropolisNormal** or **AcropolisNormal** and **schedulable** is equal to **True** . 

Contact Nutanix Support if any of the steps described in this document produce unexpected results. 

## **Starting a Node in a Cluster (Hyper-V)** 

## **About this task** 

## **Procedure** 

**1.** If the node is off, turn it on by pressing the power button on the front. Otherwise, proceed to the next step. 

**2.** Log on to the Hyper-V host with Remote Desktop Connection and start PowerShell. 

**3.** Open the Failover Cluster Manager. 

**4.** Click on **Nodes.** . 

**5.** Right-click on the node name and select **Resume** . 

**6.** Click the **Fail Roles Back** menu item. 

This resumes the node. It also restores the roles that were running on the node previously. 

**Figure 26: Resume Node Options in Failover Cluster Manager** 

Platform | Manual Firmware Updates | **61** 

**7.** From the Powershell, determine if the Controller VM is running. 

   - `Get-VM | Where {$_.Name -match 'NTNX.*CVM'}` ~~SCS:~~ 

   - If the Controller VM is off, a line similar to the following should be returned: 

      - `NTNX-13SM35230026-C-CVM Stopped -           -             - Opera...` ~~ee~~ Make a note of the Controller VM name in the second column. 

   - If the Controller VM is on, a line similar to the following should be returned: 

      - `NTNX-13SM35230026-C-CVM Running 2           16384             05:10:51 Opera...` ~~CSCS~~ 

**8.** Start the Controller VM from the GUI or use the powershell. 

   - ~~i~~ `> Start-VM -Name NTNX-*CVM` Wait approximately 5 minutes for all services to start on the Controller VM. 

**9.** Log on to any Controller VM in the cluster with SSH. 

**10.** Verify that all services are up on all Controller VMs. 

~~i~~ `nutanix@cvm$ cluster status` 

If the cluster is running properly, output similar to the following is displayed for each node in the cluster: 

|CVM: 10.1.64.60 Up|CVM: 10.1.64.60 Up|
|---|---|
|Zeus   UP       [5362, 5391, 5392, 10848, 10977,|Zeus   UP       [5362, 5391, 5392, 10848, 10977,|
|10992]||
|Scavenger   UP       [6174, 6215, 6216, 6217]|Scavenger   UP       [6174, 6215, 6216, 6217]|
|SSLTerminator   UP       [7705, 7742, 7743, 7744]|SSLTerminator   UP       [7705, 7742, 7743, 7744]|
|SecureFileSync   UP       [7710, 7761, 7762, 7763]|SecureFileSync   UP       [7710, 7761, 7762, 7763]|
|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|Medusa   UP       [8029, 8073, 8074, 8176, 8221]|
|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|DynamicRingChanger   UP       [8324, 8366, 8367, 8426]|
|Pithos   UP       [8328, 8399, 8400, 8418]|Pithos   UP       [8328, 8399, 8400, 8418]|
|Hera   UP       [8347, 8408, 8409, 8410]|Hera   UP       [8347, 8408, 8409, 8410]|
|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|Stargate   UP       [8742, 8771, 8772, 9037, 9045]|
|InsightsDB   UP       [8774, 8805, 8806, 8939]|InsightsDB   UP       [8774, 8805, 8806, 8939]|
|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888,|InsightsDataTransfer   UP       [8785, 8840, 8841, 8886, 8888,|
|8889, 8890]||
|Ergon   UP       [8814, 8862, 8863, 8864]|Ergon   UP       [8814, 8862, 8863, 8864]|
|Cerebro   UP       [8850, 8914, 8915, 9288]|Cerebro   UP       [8850, 8914, 8915, 9288]|
|Chronos   UP       [8870, 8975, 8976, 9031]|Chronos   UP       [8870, 8975, 8976, 9031]|
|Curator   UP       [8885, 8931, 8932, 9243]|Curator   UP       [8885, 8931, 8932, 9243]|
|Prism   UP       [3545, 3572, 3573, 3627, 4004,|Prism   UP       [3545, 3572, 3573, 3627, 4004,|
|4076]||
|CIM   UP       [8990, 9042, 9043, 9084]|CIM   UP       [8990, 9042, 9043, 9084]|
|AlertManager   UP       [9017, 9081, 9082, 9324]|AlertManager   UP       [9017, 9081, 9082, 9324]|
|Arithmos   UP       [9055, 9217, 9218, 9353]|Arithmos   UP       [9055, 9217, 9218, 9353]|
|Catalog   UP       [9110, 9178, 9179, 9180]|Catalog   UP       [9110, 9178, 9179, 9180]|
|Acropolis   UP       [9201, 9321, 9322, 9323]|Acropolis   UP       [9201, 9321, 9322, 9323]|
|Atlas   UP       [9221, 9316, 9317, 9318]|Atlas   UP       [9221, 9316, 9317, 9318]|
|Uhura   UP       [9390, 9447, 9448, 9449]|Uhura   UP       [9390, 9447, 9448, 9449]|
|Snmp   UP       [9418, 9513, 9514, 9516]|Snmp   UP       [9418, 9513, 9514, 9516]|
|SysStatCollector   UP       [9451, 9510, 9511, 9518]|SysStatCollector   UP       [9451, 9510, 9511, 9518]|
|Tunnel   UP       [9480, 9543, 9544]|Tunnel   UP       [9480, 9543, 9544]|
|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976,|ClusterHealth   UP       [9521, 9619, 9620, 9947, 9976,|
|9977, 10301]||
|Janus   UP       [9532, 9624, 9625]|Janus   UP       [9532, 9624, 9625]|
|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|NutanixGuestTools   UP       [9572, 9650, 9651, 9674]|
|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|MinervaCVM   UP       [10174, 10200, 10201, 10202, 10371]|
|ClusterConfig   UP       [10205, 10233, 10234, 10236]|ClusterConfig   UP       [10205, 10233, 10234, 10236]|



Platform | Manual Firmware Updates | **62** 

|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]|APLOSEngine   UP       [10231, 10261, 10262, 10263]||
|---|---|---|---|---|---|---|---|
|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|APLOS   UP       [10343, 10368, 10369, 10370, 10502,|
|10503]||||||||
|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]|Lazan   UP       [10377, 10402, 10403, 10404]||
|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]|Orion   UP       [10409, 10449, 10450, 10474]||
|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]|Delphi   UP       [10418, 10466, 10467, 10468]||



**11.** If necessary, add the host back to the metadata store. 

If the node is unavailable for more than 30 minutes, it might have been removed from metadata store. To add a host into the metadata store again, complete one of these steps. 

- a. Select the target host in the diagram (Diagram view) or click the **Host tab** and select that host in the table (Table view). 

- b. Click the **Enable Metadata Store** link on the right of the Summary line. 

## **Node Startup Post-Check** 

Check the health of the cluster after starting a node. 

## **Procedure** 

**1.** Make sure that the system is running the latest supported firmware, BIOS, and BMC versions. 

Nutanix recommends performing an LCM inventory and then running the relevant updates by following the Life Cycle Manager Guide. 

In scenarios where you cannot run LCM, manually update the firmware. For more information, see Manual Firmware Updates in the _NX Series Hardware Administration Guide_ . 

Optionally, after the manual firmware update, you can run the following command from a CVM and check the output to confirm the firmware versions: 

~~OO~~ `nutanix@cvm$ ncc hardware_info show_hardware_info --cvm=` S—“C:SSCSC‘(C‘(CNCéiés _`cvm_ipaddress`_ 

**2.** In Prism Element web console, go to the **Health** page and select **Actions** > **Run NCC Checks** . 

**3.** In the dialog box that appears, select **All Checks** and click **Run** . 

Alternatively, issue the following command from the CVM: 

> `nutanix@cvm$ ncc health_checks run_all` ~~ES~~ 

If any checks fail, see the related KB article provided in the output and the _Nutanix Cluster Check Guide: NCC Reference_ for information on resolving the issue. 

**4.** If you have any unresolvable failed checks, contact Nutanix Support. 

Platform | Manual Firmware Updates | **63** 

**2** 

## **CHANGING AN IPMI IP ADDRESS** 

Reset the IPMI address on a Nutanix node. 

## **Before you begin** 

Make sure that the cluster is healthy. Resolve any critical alerts and run Nutanix Cluster Check (NCC) if necessary. If you are unfamiliar with restarting cluster services, contact Nutanix Support. 

## **About this task** 

For initial setup, perform these steps once for every IPMI interface in the cluster. Complete the entire procedure on an interface before proceeding to the next interface. If you are reconfiguring the IPMI IP address because you have replaced a node, reconfigure the IPMI address only for that node. 

**Note:** Restart Genesis after changing the IPMI configuration. Otherwise, the cluster does not have access to the IPMI interface. 

## **Procedure** 

**1.** Configure the IPMI IP addresses by using either the IPMI web interface or the hypervisor host command-line interface. 

   - » Configuring the Remote Console IP Address (IPMI Web Interface) on page 65 

   - » Configuring the Remote Console IP Address (Command Line) on page 66 

Alternatively, you can configure the IP address in the BIOS by following Configuring the Remote Console IP Address (BIOS) on page 68. 

**Caution:** If you are unfamiliar with restarting Genesis, contact Nutanix support. 

**2.** Restart Genesis with one of the following commands: 

   - » If you replace a node with a new node, you need to update the cluster with the IPMI address of the new node. You only need to restart Genesis on the Controller VM for the new node, using the `genesis restart` eT command: `nutanix@cvm$ genesis restart` ~~ee~~ 

   - » If you plan to restart every Controller VM in the cluster, log on to any Controller VM in the cluster and restart Genesis. 

      - `nutanix@cvm$ allssh genesis restart` ~~CSS~~ 

If the restart is successful, output similar to the following appears: 

`Stopping Genesis pids [1933, 30217, 30218, 30219, 30241] Genesis started on pids [30378, 30379, 30380, 30381, 30403]` 

Platform | Changing an IPMI IP Address | **64** 

## **Configuring the Remote Console IP Address (IPMI Web Interface)** 

## **About this task** 

## **Procedure** 

**1.** Sign in to the IPMI web console. 

**2.** Go to **Configuration** > **Network** . 

**3.** Select DHCP or enter the new static IP address in the **IPv4 Setting** section. 

## **Figure 27: IPMI Network Configuration** 

## **4.** Click **Save** . 

The new IPv4 configuration takes effect. This change terminates your connection to the web interface. To start a new connection, go to the new IP address of the IPMI interface. 

Platform | Changing an IPMI IP Address | **65** 

**5.** Restart Genesis with one of the following commands: 

   - » If you replace a node with a new node, you need to update the cluster with the IPMI address of the new node. You only need to restart Genesis on the Controller VM for the new node, using the `genesis restart` Se command: 

      - `nutanix@cvm$ genesis restart` ~~SCS~~ 

   - » If you plan to restart every Controller VM in the cluster, log on to any Controller VM in the cluster and restart Genesis. 

> `nutanix@cvm$ allssh genesis restart` ~~CSS~~ 

**Note:** Consider the following points before you restart Genesis: 

- If you are reconfiguring the IPMI address on a node, perform a Genesis restart to allow the change in the IP address to be pushed to the configuration file managed by Zeus. 

- If you have reconfigured IPMIs for multiple nodes, you must perform a Genesis restart for each reconfigured node. 

- If you are configuring the IPMI for the first time, a Genesis restart is not needed. 

- If you have installed a new node but you have not changed the IPMI LAN settings, you do not need to perform a Genesis restart. 

**Note:** If you are unfamiliar with restarting Genesis, contact Nutanix support. 

If the restart is successful, output similar to the following is displayed: 

`Stopping Genesis pids [1933, 30217, 30218, 30219, 30241] Genesis started on pids [30378, 30379, 30380, 30381, 30403]` 

## **Configuring the Remote Console IP Address (Command Line)** 

## **About this task** 

You can configure the management interface from the hypervisor host on the same node. 

Perform these steps once from each hypervisor host in the cluster where you want to change the management network configuration. 

## **Procedure** 

**1.** Log on to the hypervisor host with SSH (vSphere or AHV) or remote desktop connection (Hyper-V). 

**2.** Set the networking parameters. 

   - » vSphere 

`root@esx# /ipmitool lan set 1 ipsrc static root@esx# /ipmitool lan set 1 ipaddr` _`mgmt_interface_ip_addr`_ `root@esx# /ipmitool lan set 1 netmask` _`mgmt_interface_subnet_addr`_ `root@esx# /ipmitool lan set 1 defgw ipaddr` _`mgmt_interface_gateway`_ 

- » Hyper-V 

   - `ipmiutil lan -e -I` _`mgmt_interface_ip_addr`_ `-G` _`mgmt_interface_gateway`_ `-S` _`mgmt_interface_subnet_addr`_ 

Platform | Changing an IPMI IP Address | **66** 

- » AHV 

`root@ahv# ipmitool lan set 1 ipsrc static root@ahv# ipmitool lan set 1 ipaddr` _`mgmt_interface_ip_addr`_ `root@ahv# ipmitool lan set 1 netmask` _`mgmt_interface_subnet_addr`_ `root@ahv# ipmitool lan set 1 defgw ipaddr` _`mgmt_interface_gateway`_ 

   - Replace ~~ee~~ _`mgmt_interface_ip_addr`_ with the new IP address for the remote console. 

   - Replace ~~es~~ _`mgmt_interface_gateway`_ with the gateway IP address. 

   - Replace ~~EE~~ _`mgmt_interface_subnet_addr`_ with the subnet mask for the new IP address. 

**3.** Show current settings. 

   - » vSphere `root@esx# /ipmitool -v lan print 1` ~~SCS~~ 

   - » Hyper-V `> ipmiutil lan -r` ~~CSS~~ 

   - » AHV 

      - `root@ahv# ipmitool -v lan print 1` ~~ee~~ 

Confirm that the parameters are set to the correct values. 

**4.** Restart Genesis with one of the following commands: 

   - » If you replace a node with a new node, you need to update the cluster with the IPMI address of the new node. You only need to restart Genesis on the Controller VM for the new node, using the `genesis restart` Se command: `nutanix@cvm$ genesis restart` ~~SCS~~ 

   - » If you plan to restart every Controller VM in the cluster, log on to any Controller VM in the cluster and restart Genesis. 

> `nutanix@cvm$ allssh genesis restart` ~~CSCC~~ 

**Note:** Consider the following points before you restart Genesis: 

- If you are reconfiguring the IPMI address on a node, perform a Genesis restart to allow the change in the IP address to be pushed to the configuration file managed by Zeus. 

- If you have reconfigured IPMIs for multiple nodes, you must perform a Genesis restart for each reconfigured node. 

- If you are configuring the IPMI for the first time, a Genesis restart is not needed. 

- If you have installed a new node but you have not changed the IPMI LAN settings, you do not need to perform a Genesis restart. 

**Note:** If you are unfamiliar with restarting Genesis, contact Nutanix support. 

If the restart is successful, output similar to the following is displayed: 

> `Stopping Genesis pids [1933, 30217, 30218, 30219, 30241]` ~~ee~~ 

Platform | Changing an IPMI IP Address | **67** 

`Genesis started on pids [30378, 30379, 30380, 30381, 30403]` 

## **Configuring the Remote Console IP Address (BIOS)** 

## **About this task** 

## **Procedure** 

**1.** Physically connect a keyboard and monitor to a node. 

**2.** Restart the node and press **Delete** to enter the BIOS setup utility. There is a limited amount of time to enter BIOS before the host completes the restart process. 

**3.** Press the right arrow key to select the **IPMI** tab. 

**4.** Press the down arrow key until **BMC network configuration** is highlighted and then press **Enter** . 

**5.** Press down the arrow key until **Update IPMI LAN Configuration** is highlighted and press **Enter** to select **Yes** . 

**6.** Select **Configuration Address source** and press **Enter** . 

**7.** Select **Static** and press **Enter** . 

**8.** Assign the **Station IP address** , **Subnet mask** , and **Router IP address** . 

**9.** Review the BIOS settings and press **F4** to save the configuration changes and exit the BIOS setup utility. The node restarts. 

Platform | Changing an IPMI IP Address | **68** 

## **10.** Restart Genesis with one of the following commands: 

- » If you replace a node with a new node, you need to update the cluster with the IPMI address of the new node. You only need to restart Genesis on the Controller VM for the new node, using the `genesis restart` ae command: `nutanix@cvm$ genesis restart` ~~ee~~ 

- » If you plan to restart every Controller VM in the cluster, log on to any Controller VM in the cluster and restart Genesis. 

> `nutanix@cvm$ allssh genesis restart` ~~CSCS~~ 

**Note:** Consider the following points before you restart Genesis: 

- If you are reconfiguring the IPMI address on a node, perform a Genesis restart to allow the change in the IP address to be pushed to the configuration file managed by Zeus. 

- If you have reconfigured IPMIs for multiple nodes, you must perform a Genesis restart for each reconfigured node. 

- If you are configuring the IPMI for the first time, a Genesis restart is not needed. 

- If you have installed a new node but you have not changed the IPMI LAN settings, you do not need to perform a Genesis restart. 

**Note:** If you are unfamiliar with restarting Genesis, contact Nutanix support. 

If the restart is successful, output similar to the following is displayed: 

`Stopping Genesis pids [1933, 30217, 30218, 30219, 30241] Genesis started on pids [30378, 30379, 30380, 30381, 30403]` 

Platform | Changing an IPMI IP Address | **69** 

**3** 

## **CHANGING THE IPMI PASSWORD** 

IPMI password information. 

## **About this task** 

- This procedure helps prevent the BMC password from being retrievable on port 49152. 

- The IPMI Password Complexity Enforcement enable/disable option is available for Nutanix G6 and later platforms. You can typically find this option in the IPMI web console **Users** > **Advanced User Account Security** page. 

**Table 6: IPMI Password Requirements** 

|**Item**|**Requirement**|
|---|---|
|Required password length|8–20 characters|
|Password name|Password cannot be the reverse of the user name.<br>Password must include characters from at least<br>three of the allowed character classes.|
|Allowed character classes|a - z, A - Z, 0–9, and special characters. For a list<br>of allowed special characters, seeAllowed Special<br>Characters in IPMI Passwordson page 71.|



**Note:** Starting with BMC 7.08, every node ships from the factory with a unique password. The default IPMI credentials are `username = ADMIN` and `password = node-serial-number` . To find the serial number, do one of the following. 

- From the host, issue the command `ipmitool fru print` . In the output, search for Board serial. The Board serial value is the IPMI password. 

- Look for the sticker on the node that reads IPMI PW/SN. That string is the default IPMI password. 

Perform these steps on every IPMI host in the cluster. 

## **Procedure** 

**1.** Sign in to the IPMI web interface as the administrative user. 

Platform | Changing the IPMI Password | **70** 

**2.** Go to the **Users** page. 

   - » For G8 and later platforms: Click **Configuration** > **Account Services** > **Users** , and then click the modify user icon (small pen icon) next to the user with administrator access. 

   - » For G7 and earlier platforms: Click **Configuration** > **Users** and then select the user with administrator access and click **Modify User** . 

The **Modify User** window appears. 

**3.** Type the new password in both text fields and then click **Modify** . 

**4.** Click **OK** to close the confirmation window. 

## **Allowed Special Characters in IPMI Passwords** 

List of allowed special characters in IPMI passwords for Nutanix platforms. 

## **G6 and G7 Platforms on Earlier BMC Versions** 

G6 and G7 platforms that use BMC 7.10 and earlier support the following 32 special characters in IPMI passwords. 

`` ~ ! @ $ % ^ & * ( ) = [ ] \ / _ + ; , . { } < > ? | # - ' : "` 

Spaces and tabs are not allowed. 

## **G6 and G7 Platforms on Later BMC Versions** 

G6 and G7 platforms that use BMC 7.11 or later support the following 29 special characters in IPMI passwords. 

`` ~ ! @ $ % ^ & * ( ) = [ ] \ / _ + ; , . { } < > ? | # -` 

Five special characters are disallowed: spaces, tabs, and the apostrophe, colon, and quotation mark characters ( — `' : "` ). 

## **G8, G9, and G10 Platforms** 

G8 (including N-G8), G9, and G10 platforms support the following 29 special characters in IPMI passwords. 

`` ~ ! @ $ % ^ & * ( ) = [ ] \ / _ + ; , . { } < > ? | # -` 

Five special characters are disallowed: spaces, tabs, and the apostrophe, colon, and quotation mark characters ( `' : "` a ). 

## **Changing the IPMI Password for ESXi** 

Change the IPMI password on an ESXi host. 

Platform | Changing the IPMI Password | **71** 

## **About this task** 

If you do not know the password of IPMI, but you have root access to ESXi host and want to change the password of the IPMI, perform the following procedure. You can use the IPMI tool from SSH as a root to do an in-band reset. 

## **Procedure** 

**1.** Log into the ESXi host with SSH. 

**2.** Determine the user ID of the administrator for which you want to change the password. 

   - `root@esx# /ipmitool user list` ~~ee~~ A sample output is as follows. 

`ID Name Callin Link Auth IPMI Msg Channel Priv Limit 2 ADMIN true false false Unknown (0x00)` 

In the sample output, the ID of the administrator for which you want to change the password is 2. 

**3.** Set the new password. 

   - `root@esx# /ipmitool user set password` ~~SSS~~ _`user_id new_password`_ 

**Note:** If you want to use more than 16 characters, you must append the parameter "20". The maximum allowed number of characters is 20. 

`root@esx# /ipmitool user set password` _`user_id new_password`_ `20` 

## **Changing the IPMI Password for Hyper-V** 

Change the IPMI password on a Hyper-V host. 

## **About this task** 

If you have administrator access to a Hyper-V host, you can change the IPMI password using an in-band reset, even if you do not know the current password. 

## **Procedure** 

**1.** Log on to the Hyper-V host with Remote Desktop Connection and start PowerShell. 

**2.** Determine the user ID of the administrator for which you want to change the password. 

   - `PS C:\Program Files\Nutanix\ipmicfg> .\IPMICFG-Win.exe -user list` ~~SSS~~ A sample output is as follows. 

`Maximum number of Users          : 10 Count of currently enabled Users : 1 User ID | User Name     |Privilege Level | Enable --------  ----------    |--------------- | ------2 | ADMIN         | Administrator  | Yes` 

In the sample output, the user ID of the administrator for which you want to change the password is 2. 

Platform | Changing the IPMI Password | **72** 

**3.** Set the new password. 

`PS C:\Program Files\Nutanix\ipmicfg> .\IPMICFG-Win.exe -user setpwd` _`user_id new_password`_ 

**Note:** If you want to use more than 16 characters, you must append the parameter "20". The maximum allowed number of characters is 20. 

`PS C:\Program Files\Nutanix\ipmicfg> .\IPMICFG-Win.exe -user setpwd` _`user_id new_password`_ `20` 

## **Changing the IPMI Password for AHV** 

Change the IPMI password on an AHV host. 

## **About this task** 

If you do not know the password of IPMI, but have root access to the AHV host and want to change the password of the IPMI, perform the following procedure. You can use the IPMI tool from SSH as a root to do an in-band reset. 

## **Procedure** 

**1.** Log into the AHV host with SSH. 

**2.** Determine the user ID of the administrator for which you want to change the password. 

> `root@ahv# ipmitool user list` ~~SSS~~ A sample output is as follows. 

`ID Name Callin Link Auth IPMI Msg Channel Priv Limit 2 ADMIN true false false Unknown (0x00)` 

In the sample output, the ID of the administrator for which you want to change the password is 2. 

**3.** Set the new password. 

> `root@ahv# ipmitool user set password` ~~ee~~ _`user_id new_password`_ 

**Note:** If you want to use more than 16 characters, you must append the parameter "20". The maximum allowed number of characters is 20. 

`root@ahv# ipmitool user set password` _`user_id new_password`_ `20` 

## **Changing the IPMI Password without Operating System Access** 

Change the IPMI password. 

## **About this task** 

If you do not know the IPMI password, and you do not have operating system access, you can reset the IPMI password using a bootable environment on a USB drive. 

**Note:** This procedure requires physical access to the node. 

## **Procedure** 

**1.** Obtain an empty USB drive. 

**2.** Follow the instructions in Nutanix KB article 7152, available at this link: KB 7152. 

Platform | Changing the IPMI Password | **73** 

**4** 

## **USING THE RESCUE SHELL** 

Log on to a rescue shell to diagnose issues with the boot device. 

To log on to a rescue shell, you must first create the `svmrescue.iso` image on another node. 

## **Creating the Controller VM Recovery Image (Hyper-V)** 

## **About this task** 

## **Procedure** 

**1.** Log on to another Controller VM in the cluster with SSH. 

**2.** Create the ISO image. 

`nutanix@cvm$ cd ~/data/installer/*` _`version`_ `* nutanix@cvm$ ./make_iso.sh svmrescue Rescue 10` 

Replace _`version`_ with the AOS version of the cluster. 

The `make_iso` command generates the `svmrescue.iso` file in the `/home/nutanix/data/installer` directory. 

## **Launching the Recovery Shell** 

Launch the recovery shell. 

## **About this task** 

**Caution:** This procedure is for the boot drive replacement in slot 1 of the node. Do not install the Controller VM on a metadata drive in slot 2 of the node. 

## **Procedure** 

**1.** Log on to the ESXi host as root with the vSphere client. 

**2.** If the Controller VM is running, right-click the Controller VM and select **Power** > **Power Off** . 

Platform | Using the Rescue Shell | **74** 

**3.** Set up the BIOS. 

   - a. Right-click the Controller VM and select **Edit Settings** / **Hardware** > **CD/DVD Drive** > **Device Type** > **Client Device** . 

   - b. Select **Options** > **Advanced** > **Boot Options** > **Force BIOS Setup** > **OK** . 

   - c. Click the **Console** tab of the Controller VM. 

   - d. Right-click the Controller VM and select **Power** > **Power On** . 

   - e. When the Controller VM restarts into BIOS, select the **Connect/Disconnect the CD/DVD devices of the virtual machine** icon at the top of the console and select **CD/DVD Drive** > **Connect to ISO image on local disk** . 

   - f. Select the `svmrescue.iso` file on your local system and click **OK** . 

   - g. In the console, press **Esc** and select **Exit Discarding Changes** to exit the BIOS. 

**4.** Choose **Rescue Shell** from the boot menu and press **Enter** . 

**5.** Select from the following choices. 

   - » **Start Nutanix Controller VM** 

   - » **Rescue Nutanix Controller VM** 

**Note:** This option reimages the Controller VM, but keeps the metadata (oplog/extent store) and cold data. 

- » **Factory Deploy Nutanix Controller VM** . 

**Warning:** This option formats and reimages all disks. 

- » **Rescue Shell** . Starts the rescue shell utility. 

Platform | Using the Rescue Shell | **75** 

## **Rescue Shell Commands** 

List of rescue shell commands. 

Inside the rescue shell, you can use the following commands to see if the disk is readable. 

## **`parted`** 

View the partitions on a drive. 

**Caution:** Only use the `print` command within `parted` . Using other commands can destroy the drive. 

As an example, here is what running the `parted` command looks like on an NX-3451: 

~~ee~~ `# parted /dev/sda` At the resulting prompt, type `print` . ~~OO~~ `(parted) print` e—CS—SSCSCCSCiés `Model: ATA INTEL SSDSC2BA80 (scsi) Disk /dev/sda: 800GB Sector size (logical/physical): 512B/512B Partition Table: msdos Number  Start   End     Size    Type     File system  Flags 1      1049kB  10.7GB  10.7GB  primary  ext4 2      10.7GB  21.5GB  10.7GB  primary  ext4 3      21.5GB  64.4GB  42.9GB  primary  ext4 4      64.4GB  800GB   736GB   primary  ext4` 

## **`lsscsi`** 

This command lists the SCSI devices presented to the Controller VM. The boot drive is listed as `/dev/sda` . 

As an example, here is what running the `lsscsi` command looks like on an NX-3450: 

~~SCS~~ `# lsscsi [2:0:0:0]    disk    ATA      INTEL SSDSC2BA80 0250  /dev/sda [2:0:1:0]    disk    ATA      INTEL SSDSC2BA80 0250  /dev/sdb [2:0:2:0]    disk    ATA      ST91000640NS     SN03  /dev/sdc [2:0:3:0]    disk    ATA      ST91000640NS     SN03  /dev/sdd [2:0:4:0]    disk    ATA      ST91000640NS     SN03  /dev/sde [2:0:5:0]    disk    ATA      ST91000640NS     SN03  /dev/sdf` 

On the NX-1000/NX-3050/NX-6000 series, the boot disk is divided into four partitions as follows: 

- `sda1` : root partition 

- `sda2` : alternate root partition (for upgrades) 

- `sda3` : `/home/nutanix` 

- `sda4` : Stargate extent data and metadata 

## **Exiting the Rescue Shell** 

Exit the rescue shell. 

## **Procedure** 

**1.** Shut down the Controller VM. 

Platform | Using the Rescue Shell | **76** 

**2.** Right-click the Controller VM and click **Edit Settings** . 

**3.** Select **Hardware** > **CD/DVD Drive** . 

**4.** Choose **Device Type** > **Datastore ISO File** . 

**5.** Click **Browse** and locate the ServiceVM ISO file on the local datastore (for example: `[NTNX-local-dsnfs-1-4] ServiceVM-1.25_Centos/ServiceVM-1.25_Centos.iso` ). Do not select a file or folder that starts with period and pound symbols ( **.#** ). 

**6.** Select the ServiceVM ISO file and click **OK** . 

**7.** Select **Device Status** > **Connect at power on** and click **OK** . 

**8.** Right-click the Controller VM and select **Power** > **Power On** . It might take a few minutes for the log-on prompt to appear. 

Platform | Using the Rescue Shell | **77** 

**5** 

## **ADDING A DRIVE** 

Add a drive to a platform. 

## **About this task** 

- The process of adding a drive is the same for all platforms (Nutanix, third-party, or OEM platforms), assuming the platform is running Nutanix AOS. 

- The types of drives you can add depends on your platform configuration. For supported drive configurations, see the system specifications for your platform, available at this link: System Specifications. 

- Starting with AOS 6.0, Nutanix allows mixing of drives with different capacities in the same node. However, the node treats higher-capacity drives as if they had the same capacity as the lower-capacity drives. 

Nutanix provides the ability to mix drive capacities for cases where you need to replace a drive, but only higher-capacity drives are available. To increase the overall storage capacity of the node, replace all drives with higher-capacity drives. 

- When adding more than one drive to a node, add one drive at a time. Complete all steps described in this procedure for each drive before proceeding to the next drive. 

- Supported node capacities are available at this link: Nutanix Configuration Maximums. 

## **Procedure** 

**1.** Insert the drive in an empty slot. 

Platform | Adding a Drive | **78** 

**2.** Log on to the web console, go to **Hardware** > **Diagram** , and select the added drive to view the details. 

**Figure 28: Added Drive (Multi-Node Block)** 

## **Caution:** 

Always wait one minute between every drive that you add in Prism (with **Repartition and Add** ). This ensures that the newly added drive is fully configured before adding another drive. 

If the drive is red and shows a label of `Unmounted Disk` , select the drive and click **Repartition and Add** under the diagram. 

This message and the button appear only if the replacement drive contains data. Their purpose is to protect you from unintentionally using a drive with data on it. 

**Caution:** This action removes all data on the drive. Do not repartition the drive until you have confirmed that the drive contains no essential data. ee 

Platform | Adding a Drive | **79** 

**3.** From the web console **Summary** > **Disk Details** field, verify that the disk has been added to the original storage pool. 

## **Figure 29: Disk Details** 

If the cluster has only one storage pool, Prism automatically adds the disk to the storage pool. 

Platform | Adding a Drive | **80** 

**4.** If the drive is not automatically added to the storage pool (because the cluster has more than one), add it to the desired storage pool. 

   - a. In the web console, select **Storage** from the pull-down main menu (upper left of screen) and then select the **Table** and **Storage Pool** tabs. 

**Figure 30: Storage Pool Table** 

- b. Select the target storage pool and then click **Update** . The **Update Storage Pool** window appears. 

- c. In the **Capacity** field, check the **Use unallocated capacity** box to add the available unallocated capacity to this storage pool then click **Save** . 

- d. Go back to **Hardware** > **Diagram** , select the drive, and confirm that it is in the correct storage pool. 

Platform | Adding a Drive | **81** 

**6** 

## **REMOTE DIRECT MEMORY ACCESS** 

Details of RDMA support. 

Remote direct memory access (RDMA) gives a node direct access to the memory subsystems of other nodes in the cluster, without involving the CPU-bounded network stack of the operating system. RDMA allows low-latency data transfer between memory subsystems, and so improves network latency and lowers CPU use. 

For more information about RDMA, RDMA port pass-through mechanism, and how to configure the RDMA network segmentation using Zero-Touch RoCE (ZTR) or Priority-Based Flow Control (PFC) mechanism, see the chapter on RDMA over Converged Ethernet in the AOS Security Guide, available at this link: RDMA over Converged Ethernet (RoCE). 

Nutanix currently supports the use of RDMA-enabled network cards for the following platforms. 

|G6 platforms|G6 platforms|•|NX-3060-G6|
|---|---|---|---|
|||•|NX-3155G-G6|
|||•|NX-3170-G6|
|||•|NX-8035-G6|
|||•|NX-8155-G6|
|G7 platforms|G7 platforms|•|NX-3060-G7|
|||•|NX-3155G-G7|
|||•|NX-3170-G7|
|||•|NX-8035-G7|
|||•|NX-8150-G7|
|||•|NX-8155-G7|
|||•|NX-8170-G7|



Platform | Remote Direct Memory Access | **82** 

|G8 platforms|•|NX-1065-G8 and N-G8|
|---|---|---|
||•|NX-1175S-G8|
||•|NX-3060-G8 and N-G8|
||•|NX-3155G-G8 and N-G8|
||•|NX-3170-G8 and N-G8|
||•|NX-8035-G8 and N-G8|
||•|NX-8150-G8 and N-G8|
||•|NX-8155-G8 and N-G8|
||•|NX-8170-G8 and N-G8|
|G9 platforms|•|NX-1065-G9|
||•|NX-1175S-G9|
||•|NX-3035-G9|
||•|NX-3060-G9|
||•|NX-3155-G9|
||•|NX-8150-G9|
||•|NX-8155-G9|
||•|NX-8155A-G9|
||•|NX-8170-G9|
|G10 platforms|•|NX-1175S-G10|
||•|NX-3035-G10|
||•|NX-3060S-G10|
||•|NX-8150-G10|
||•|NX-8150G-G10|
||•|NX-8155AS-G10|
||•|NX-8170-G10|
||•|NX-8170A-G10|



Use of RDMA in a Nutanix platform must meet the following conditions. 

- Each node in the cluster must contain two RDMA-enabled network cards. 

|G6 platforms|G6 platforms|Mellanox CX4|Mellanox CX4|
|---|---|---|---|
|G7 platforms|G7 platforms|•|Mellanox CX4|
|||•|Mellanox CX5|



Platform | Remote Direct Memory Access | **83** 

|G8 and N-G8 platforms|G8 and N-G8 platforms|Mellanox CX5 25 GB (part number MCX512A-ACUT)|Mellanox CX5 25 GB (part number MCX512A-ACUT)|
|---|---|---|---|
|G9 and G10 platforms|G9 and G10 platforms|•|Mellanox CX6 25 GB (part number MCX631102AS-|
||||ADAT)|
|||•|Mellanox CX6 100 GB (part number|
||||MCX623106AS-CDAT)|
|||•|Mellanox CX7 200 GB (part number|
||||MCX755106AS-HEAT)|



**Note:** Mellanox CX-type NICs are dual-port cards. One card uses a single port dedicated to RDMA traffic. The other card uses its ports for CVM and guest VM traffic. 

- NX platforms up through G8 do not support mixing network cards from different manufacturers, or of different capacities. 

- NX N-G8, G9, and G10 platforms support some NIC mixing. For details, see Product Mixing Restrictions on page vi. 

- RDMA-enabled cards must be installed at the factory. You cannot add them to a node in the field. 

- RDMA software support: 

|G6 and G7 platforms|G6 and G7 platforms|•|AHV|
|---|---|---|---|
|||•|ESXi (starting with AOS 5.11.2)|
||||•<br>ESXi 6.5: 6.5U1 or later|
||||•<br>ESXi 6.7: 6.7U1 or later|
||||•<br>ESXi 7.0|
|G8 and N-G8 platforms|G8 and N-G8 platforms|•|Foundation 5.0.4|
|||•|AOS 5.20.1.1|
|||•|AHV 7.2 and later|
|||•|ESXi|
||||•<br>6.7U3b|
||||•<br>7.0 U2a and later|



Platform | Remote Direct Memory Access | **84** 

|G9 platforms|•|Foundation 5.6|
|---|---|---|
||•|AOS 6.5.3|
||•|AHV 6.8.0.1 and later|
||•|ESXi|
|||•<br>7.0 U3n|
|||•<br>7.0 U3o|
|||•<br>8.0 U1|
|||•<br>8.0 U2 and later|
|G10 platforms|•|Foundation 5.10|
||•|AOS 7.5|
||•|AHV 11.0 and later|
||•|ESXi|
|||•<br>7.0 U3n|
|||•<br>8.0 U3e|



- RDMA nodes cannot mix with non-RDMA nodes in the same cluster. 

- AOS does not enable datacenter bridging (DCB) automatically, in order to avoid overwriting any existing switch configuration. Enable DCB on the customer switch manually. 

Platform | Remote Direct Memory Access | **85** 

**7** 

## **CMOS BATTERY REPLACEMENT** 

Details of CMOS battery replacement. 

Nutanix cannot ship batteries. Air freight regulations and international government agencies restrict the shipment of uninstalled lithium batteries. Customers can purchase and replace the CMOS battery if it fails. Nutanix can help install batteries after a customer has purchased the replacement battery. 

**Warning:** DANGER OF EXPLOSION IF THE BATTERY IS INCORRECTLY REPLACED. 

Replace only with the same or equivalent type recommended by the manufacturer. Dispose of used batteries according to manufacturer instructions. 

Only use the following batteries for the platforms shown: 

- For G5 platforms: KTS brand CR2032 3V or a reliable equivalent. 

- For G6 and later platforms: Niutanix recommends KTS brand BR2032 3V or a reliable equivalent. If BR2032 is unavailable, use KTS brand CR2032 3V or a reliable equivalent. 

For all currently supported Nutanix platforms, the only function of the CMOS battery is to preserve BIOS settings. A CMOS battery failure does not affect power control. 

Find the battery replacement documentation for each platform on the support portal by selecting **Hardware Replacement Documentation** > **Battery** > **(Platform Generation)** > **(Platform Name)** (for example: **G7** > **NX-3060-G7** ). 

**8** 

## **MEMORY CONFIGURATIONS** 

## **Supported Memory Configurations** 

Pointer to supported memory configurations. 

To see supported memory configurations and DIMM installation order, consult the memory replacement documentation for your platform. On the Nutanix portal, select **Documentation** > **Hardware Documentation** > **Memory** and choose your platform from the list. 

Platform | Memory Configurations | **87** 

**9** 

## **HOST SECURE BOOT (UEFI)** 

## **Host Secure Boot Overview** 

Minimum requirements and host secure boot workflows for Nutanix platforms. 

Secure boot support begins with Nutanix G8 platforms. 

Secure boot is a verification mechanism for ensuring that code launched by a computer's firmware is trusted. It is designed to protect a system against malicious code being loaded and executed early in the boot process, before the operating system is loaded. 

- For G8 platforms, use either the BIOS method or the Redfish API method. The Redfish API method can be simpler if you need to enable host secure boot on multiple nodes. 

- For G9 platforms, use IPMI `raw` commands. 

- For G10 platforms, use the Redfish API. 

The following table shows the minimum settings for secure boot. 

**Table 7: Secure boot minimum settings** 

|**Components**|**G8 Platform Minimum version**|**G9 Platform Minimum version**|
|---|---|---|
|Foundation<br>AOS<br>BIOS<br>BMC<br>AHV<br>ESXi<br>NCC|5.0.4<br>5.20.1.1<br>W(B/U/W)10.104<br>8.0.1<br>Recommended AHV<br>•<br>To find the recommended<br>version of AHV, see the<br>Compatibility matrix at this<br>link:Compatibility Matrix.<br>ESXI6.7U3b, ESXi7.0U2a<br>4.2.0|5.4.2<br>6.5.3.1<br>E(B/U/W) 10.201<br>01.00.06<br>Recommended AHV<br>•<br>To find the recommended<br>version of AHV, see the<br>Compatibility matrix at this<br>link:Compatibility Matrix.<br>ESXi7OU3n<br>Bundled|



Summary of the secure boot enablement sequence for NX platforms: 

**1.** Confirm the current secure boot status. 

**2.** Load all platform keys (G8 platforms only). 

**3.** Enable secure boot. 

**4.** Restart the host. 

**5.** Verify secure boot status. 

Platform | Host Secure Boot (UEFI) | **88** 

**6.** Complete the Foundation installation process. 

**7.** Verify secure boot status on the CVM/Hypervisor. 

The secure boot procedures follow this topic in this _Hardware Administration Guide_ . 

## **G8 Platforms: Enabling Host Secure Boot (Redfish API Method)** 

Enable host secure boot on Nutanix platforms using the Redfish API. 

## **About this task** 

The Redfish API method can be simpler if you need to enable host secure boot on multiple nodes. 

## **Procedure** 

**1.** Verify the secure boot status. 

URL: /redfish/v1/Systems/1/SecureBoot Method: GET Payload: None Response: 200 Example: `nutanix@cvm$ curl --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/SecureBoot" { "@odata.type": "#SecureBoot.v1_0_5.SecureBoot", "@odata.id": "/redfish/v1/Systems/1/SecureBoot", "Id": "SecureBoot", "Name": "Security Boot", "SecureBootCurrentBoot": "Disabled", "SecureBootEnable": false, "SecureBootMode": "SetupMode", "Actions": { "Oem": {}, "#SecureBoot.ResetKeys": { "target": "/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys", "@Redfish.ActionInfo": "/redfish/v1/Systems/1/SecureBoot/ResetKeysActionInfo" } } }` 

**2.** Load all platform keys. 

URL: /redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys 

Method: POST 

Payload: {"ResetKeysType": "ResetAllKeysToDefault"} 

Response: 202 

Example: 

`nutanix@cvm$ curl -X POST --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys" \ -d "{\"ResetKeysType\":\"ResetAllKeysToDefault\"}"` 

Platform | Host Secure Boot (UEFI) | **89** 

`{ "Success": { "code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

## **3.** Enable secure boot. 

URL: /redfish/v1/Systems/1/SecureBoot 

Method: PATCH Payload: {"SecureBootEnable": true} Response: 202 

Example: 

`nutanix@cvm$ curl -X PATCH --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/SecureBoot" \ -d "{\"SecureBootEnable\": true}" { "Success":{ "code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

## **4.** Restart the host. 

**Caution:** Place the host into maintenance mode and move all VMs to another node before continuing. The node restarts at the end of this step. 

URL: /redfish/v1/Systems/1/Actions/ComputerSystem.Reset 

Method: POST Payload: {"ResetType": "ForceRestart"} Response: 202 

Example: 

`nutanix@cvm$ curl -X POST --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/Actions/ComputerSystem.Reset" \ -d "{\"ResetType\": \"ForceRestart\"}" { "Success": { "code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

**5.** If necessary, refer to the other topics in this _Hardware Administration Guide_ for: 

   - » Secure Boot Error Handling and Failure Conditions 

   - » Verifying Host Secure Boot Settings 

   - » Imaging with Foundation 

   - » Disabling Host Secure Boot 

Platform | Host Secure Boot (UEFI) | **90** 

## **G8 Platforms: Enabling Host Secure Boot (BIOS Method)** 

Use the BIOS to set up host secure boot and verify the host secure boot status. 

## **About this task** 

If you need to enable host secure boot on multiple nodes, the Redfish API method can be simpler. 

## **Procedure** 

**1.** Start the host: when the boot logo appears, press DELETE to access the BIOS setup utility. 

**2.** Load all platform keys. 

**3.** Select the **Security Tab** > **Security Boot** > **Key Management** > **Restore Factory Keys** . 

**Figure 31: Restore Factory Defaults in BIOS Security** 

Platform | Host Secure Boot (UEFI) | **91** 

## **4.** Select **Yes** . 

**Figure 32: Install Factory Defaults BIOS Menu** 

The **Reset Without Saving** prompt appears. 

**5.** Select `No` . 

**Figure 33: Reset Without Saving Menu** 

**6.** From the **Security** tab, select **Security Boot** > **Secure Boot** . 

Platform | Host Secure Boot (UEFI) | **92** 

## **7.** Select **Enabled** . 

**Figure 34: Secure Boot Enabled BIOS Menu** 

**8.** Press F4 key to save and exit. The host restarts. 

**9.** If necessary, refer to the other topics in this _Hardware Administration Guide_ for: 

   - » Secure Boot Error Handling and Failure Conditions 

   - » Verifying Host Secure Boot Settings 

   - » Imaging with Foundation 

   - » Disabling Host Secure Boot 

## **G8 Platforms: Verifying Enabled Host Secure Boot Settings With the Redfish API or BIOS** 

Verify the host secure boot settings in the Redfish API or the UEFI console. 

## **About this task** 

The Redfish API method can be simpler if you need to enable host secure boot on multiple nodes. 

## **Verifying Enabled Host Secure Boot Settings in the Redfish API** 

## **Procedure** 

Verify secure boot status using the Redfish API. 

## **Redfish API method:** 

Platform | Host Secure Boot (UEFI) | **93** 

URL: /redfish/v1/Systems/1/SecureBoot Method: GET Payload: None Response: 200 Example: `nutanix@cvm$ curl --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/SecureBoot" { "@odata.type": "#SecureBoot.v1_0_5.SecureBoot", "@odata.id": "/redfish/v1/Systems/1/SecureBoot", "Id": "SecureBoot", "Name": "Security Boot", "SecureBootCurrentBoot": "` **`Enabled`** `", "SecureBootEnable":` **`true`** `, "SecureBootMode": "` **`UserMode`** `", "Actions": { "Oem": {}, "#SecureBoot.ResetKeys": { "target": "/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys", "@Redfish.ActionInfo": "/redfish/v1/Systems/1/SecureBoot/ResetKeysActionInfo" } } }` 

## **Verifying Enabled Host Secure Boot Settings through the BIOS** 

## **Procedure** 

**1.** Power-on the host: when the boot logo appears, press DELETE to access the BIOS setup utility. 

**2.** Enter the **BIOS Setup** screen. 

Platform | Host Secure Boot (UEFI) | **94** 

**3.** Browse to the **Security** > **Secure Boot** page. 

**Figure 35: BIOS Security UEFI Security Enabled** 

System mode shows the following details: 

   - **System Mode: User** 

   - **Vendor Keys: Active** 

   - **Secure Boot: Active** 

   - **Secure Boot: Enabled** 

**4.** Enter the **Key Management** page and verify that all the factory default keys have been loaded. 

**Figure 36: BIOS Security: UEFI Key Management Secure Boot Variables** 

## **G9 Platforms: Enabling Host Secure Boot Using IPMI raw Commands** 

Enable host secure boot on Nutanix platforms using IPMI `raw` commands. 

Platform | Host Secure Boot (UEFI) | **95** 

## **About this task** 

## **Procedure** 

**1.** On your AHV host, log in as root. 

**2.** From the AHV host prompt, verify the current secure boot status. 

`[root@User1-Clust-0 ~]# ipmitool raw 0x30 0x70 0x91 0x00 02` 

00 = secure boot is disabled 

01 = secure boot is enabled 

02 = the default setting is applied 

**3.** Enable secure boot using the IPMI `raw` command. 

`[root@User1-Clust-0 ~]# ipmitool raw 0x30 0x70 0x91 0x01 01 01` 

**4.** Restart the host. 

`[root@User1-Clust-0 ~]# reboot` 

## **5.** Exit root. 

## **G10 Platforms: Enabling Host Secure Boot** 

Enable host secure boot on Nutanix G10 platforms using the Redfish API. 

## **Procedure** 

**1.** Verify the secure boot status. 

URL: /redfish/v1/Systems/1/SecureBoot 

Method: GET 

Payload: None Response: 200 Example: 

`nutanix@cvm$ curl -k -u` _`username`_ `:` _`password`_ `\ -X GET https://` _`BMC-IP-address`_ `/redfish/v1/Systems/1/SecureBoot` 

`{ "@odata.type": "#SecureBoot.v1_1_2.SecureBoot", "@odata.id": "/redfish/v1/Systems/1/SecureBoot", "Id": "SecureBoot", "Name": "Secure Boot", "SecureBootCurrentBoot": "Enabled", "SecureBootEnable": true, "SecureBootMode": "DeployedMode", "SecureBootDatabases": { "@odata.id": "/redfish/v1/Systems/1/SecureBoot/SecureBootDatabases"` 

Platform | Host Secure Boot (UEFI) | **96** 

`}, "Actions": { "Oem": {}, "#SecureBoot.ResetKeys": { "target": "/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys", "@Redfish.ActionInfo": "/redfish/v1/Systems/1/SecureBoot/ ResetKeysActionInfo" } }, "@odata.etag": "\"c7eaec36003bc55b66e636bce1fe81b8\"" }` 

## **2.** Enable secure boot. 

URL: /redfish/v1/Systems/1/SecureBoot 

Method: PATCH Payload: {"SecureBootEnable": true} Response: 202 Example: 

`nutanix@cvm$ curl -k -u` _`username`_ `:` _`password`_ `\ -X PATCH https://` _`BMC-IP-address`_ `/redfish/v1/Systems/1/SecureBoot \ -H 'Content-Type: application/json' \ -d '{"SecureBootEnable": true}' { "Success":{ "code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

## **3.** Restart the host. 

**Caution:** Place the host into maintenance mode and move all VMs to another node before continuing. The node restarts at the end of this step. 

URL: /redfish/v1/Systems/1/Actions/ComputerSystem.Reset Method: POST Payload: {"ResetType": "ForceRestart"} Response: 202 Example: `nutanix@cvm$ curl -k u` _`username`_ `:` _`password`_ `\ -X POST "https://<IPMI_IP>/redfish/v1/Systems/1/Actions/ComputerSystem.Reset" \ -d "{\"ResetType\": \"ForceRestart\"}" { "Success": { " code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

Platform | Host Secure Boot (UEFI) | **97** 

**4.** Verify the secure boot status. 

URL: /redfish/v1/Systems/1/SecureBoot Method: GET Payload: None Response: 200 Example: `nutanix@cvm$ curl -k -u` _`username`_ `:` _`password`_ `\ -X GET https://` _`IPMI-IP-address`_ `/redfish/v1/Systems/1/SecureBoot { "@odata.type": "#SecureBoot.v1_0_6.SecureBoot", "@odata.id": "/redfish/v1/Systems/1/SecureBoot", "Id": "SecureBoot", "Name": "Secure Boot", "SecureBootCurrentBoot": "Enabled", "SecureBootEnable": true, "SecureBootMode": "Mode", "Actions": { "Oem": {}, "#SecureBoot.ResetKeys": { "target": "/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys", "@Redfish.ActionInfo": "/redfish/v1/Systems/1/SecureBoot/ResetKeysActionInfo" } }, "@odata.etag": "5b0f2f217147c086510ae101e8a7d6a1" }` 

**5.** Check that the `SecureBootCurrentBoot` ~~ee~~ parameter is set to ~~a~~ `Enabled` . 

## **All Platforms: Imaging with Foundation on a Host Secure Boot Enabled Node** 

Foundation instructions after enabling host secure boot. 

## **About this task** 

After you have enabled host secure boot, you can image the node. Make sure that you use the newest _Field Installation Guide_ from the Nutanix Support Portal. The following link directs you to version 5.0, but check to see if there is a newer version. 

## **Procedure** 

Follow the regular Foundation imaging workflow for imaging the host in the Field Installation Guide. 

## **All Platforms: Secure Boot Error Handling and Failure Conditions** 

Methods for checking on error messages or failures. 

## **About this task** 

If secure boot is not properly enabled, the node does not boot. The IPMI Remote Control console shows an error message. 

Platform | Host Secure Boot (UEFI) | **98** 

## **Procedure** 

**1.** Use any of the following alerts to check UEFI secure boot errors or failure conditions. 

   - » Check the iKVM console for the message `Secure Boot violation: Invalid signature detected. Check secure boot policy in setup.` 

**Figure 37: iKVM Secure Boot Violation Message** 

This message confirms that the node is down because of a secure boot violation. The cause is usually a mismatch of the secure boot keys between the BIOS and the hypervisor. 

**2.** If you discover errors, check that the compatible hypervisor image has been installed correctly. 

**3.** If you want to continue to boot the node with non-secure booting, you can disable the secure boot feature by following the topic _Disabling Host Secure Boot on Nutanix NX Platforms_ in this Hardware Administration Guide. 

**4.** Contact Nutanix support if you need further assistance. 

## **All Platforms: Verifying Enabled Host Secure Boot Settings with the Hypervisor or CVM** 

After Foundation imaging, verify the host secure boot settings from the hypervisor or the CVM. 

## **About this task** 

Wait until all cluster services are up before running these commands. 

## **Procedure** 

**1.** For AHV, use `mokutil` from the command line. 

`root@ahv# mokutil --sb-state SecureBoot enabled` 

The output shows `disabled` if secure boot is disabled. 

**2.** For vSphere, use the Python utility. 

`root@esx# cd usr/lib/vmware/secureboot/bin/` 

Platform | Host Secure Boot (UEFI) | **99** 

`root@esx#/usr/lib/vmware/secureboot/bin/] python secureBoot.py -s enabled` 

The output shows `disabled` if secure boot is disabled. 

**3.** From the Controller VM, use `grep` to determine if secure boot is true. 

`nutanix@cvm$ 10.x.x.x.:~$ zeus_config_printer | grep "is_secure" is_secure_booted: true` 

The output shows `false` if secure boot is disabled. 

## **G8 Platforms: Disabling Host Secure Boot** 

Disable host secure boot on G8 platforms using the RedFish API. 

## **About this task** 

Secure boot support begins with Nutanix G8 platforms. 

Summary of the secure boot disablement host reboot sequence for NX platforms: 

**1.** Verify secure boot status. 

**2.** Delete all platform keys. 

**3.** Disable the secure boot. 

**4.** Restart the host. 

**5.** Optionally, verify secure boot status from Redfish API or UEFI. 

**6.** Optionally, confirm the secure boot status from the hypervisor and CVM. 

**7.** Optionally, verify the secure boot status from the Redfish API or UEFI. 

Some tools can send API calls that include `curl` commands that you can run on the CVM or on other systems or tools such as Cygwin or Mobaxterm. 

## **Procedure** 

**1.** Verify the secure boot status. 

URL: /redfish/v1/Systems/1/SecureBoot 

Method: GET Payload: None Response: 200 Example: 

`nutanix@cvm$ curl --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/SecureBoot" { "@odata.type": "#SecureBoot.v1_0_5.SecureBoot", "@odata.id": "/redfish/v1/Systems/1/SecureBoot", "Id": "SecureBoot", "Name": "Security Boot", "SecureBootCurrentBoot": "Enabled", "SecureBootEnable": true, "SecureBootMode": "UserMode", "Actions": { "Oem": {}, "#SecureBoot.ResetKeys": { "target": "/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys", "@Redfish.ActionInfo": "/redfish/v1/Systems/1/SecureBoot/ResetKeysActionInfo"` 

Platform | Host Secure Boot (UEFI) | **100** 

`} } }` 

## **2.** Delete all the platform keys. 

URL: /redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys 

Method: POST 

Payload: {"ResetKeysType": "DeleteAllKeys"} 

Response: 202 

Example: 

`nutanix@cvm$ curl -X POST --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot. \ ResetKeys" -d "{\"ResetKeysType\":\"DeleteAllKeys\"}" { "Success": { "code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

## **3.** Disable secure boot. 

URL: /redfish/v1/Systems/1/SecureBoot 

Method: PATCH 

Payload: {"SecureBootEnable": false} Response: 202 

Example: 

`nutanix@cvm$ curl -X PATCH --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/SecureBoot" -d \ "{\"SecureBootEnable\": false}" { "Success":{ "code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

## **4.** Restart the host. 

**Caution:** Place the host into maintenance mode and move all VMs to another node before continuing. The node restarts at the end of this step. 

URL: /redfish/v1/Systems/1/Actions/ComputerSystem.Reset 

Method: POST 

Payload: {"ResetType": "ForceRestart"} Response: 202 

Example: 

`nutanix@cvm$ curl -X POST --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/Actions/ComputerSystem.Reset" \ -d "{\"ResetType\": \"ForceRestart\"}" {` 

Platform | Host Secure Boot (UEFI) | **101** 

`"Success": { "code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

## **Verifying the Disabled Host Secure Boot Settings in the Redfish API (Optional)** 

## **About this task** 

Verify the disabled host secure boot settings in the Redfish API or the UEFI console. 

## **Procedure** 

Verify the secure boot status using the Redfish API or the UEFI console. 

## **Redfish API method:** 

URL: /redfish/v1/Systems/1/SecureBoot 

Method: GET Payload: None Response: 200 

Example: 

`nutanix@cvm$ curl --insecure --user <Username>:<Password> \ "https://<IPMI_IP>/redfish/v1/Systems/1/SecureBoot" { "@odata.type": "#SecureBoot.v1_0_5.SecureBoot", "@odata.id": "/redfish/v1/Systems/1/SecureBoot", "Id": "SecureBoot", "Name": "Security Boot", "SecureBootCurrentBoot": "` **`Disabled`** `", "SecureBootEnable":` **`false`** `, "SecureBootMode": "` **`SetupMode`** `", "Actions": { "Oem": {}, "#SecureBoot.ResetKeys": { "target": "/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys", "@Redfish.ActionInfo": "/redfish/v1/Systems/1/SecureBoot/ResetKeysActionInfo" } } }` 

## **Verifying the Disabled Host Secure Boot Settings with UEFI (Optional)** 

## **About this task** 

Verify the disabled host secure boot settings in the UEFI console or the Redfish API. 

## **Procedure** 

**1.** Restart the host. 

**2.** Enter the BIOS/UEFI setup screen. 

Platform | Host Secure Boot (UEFI) | **102** 

**3.** Browse to the **Security** > **Secure Boot** page. 

**Figure 38: Disabled: BIOS/UEFI Security** 

System mode shows the following details: 

   - **System Mode: User** 

   - **Vendor Keys: Active** 

   - **Secure Boot: Active** 

   - **Secure Boot: Enabled** 

**4.** Enter the **Key Management** window and verify that all the factory default keys are loaded. 

**Figure 39: Disabled BIOS Security: UEFI Key Management Secure Boot Variables** 

Platform | Host Secure Boot (UEFI) | **103** 

## **Confirming the Disabled Secure Boot Status from the Hypervisor and CVM** 

## **About this task** 

Wait until all the cluster services are up before running the following commands. 

## **Procedure** 

**1.** For AHV, use `mokutil` from the command line. 

`root@ahv# mokutil --sb-state SecureBoot disabled root@ahv` 

**2.** For vSphere, use the Python utility. 

`root@esxi# cd usr/lib/vmware/secureboot/bin/ root@esxi#/usr/lib/vmware/secureboot/bin] python secureBoot.py -s disabled root@esxi#/usr/lib/vmware/secureboot/bin` 

**3.** From the Controller VM, use `grep` to determine if secure boot is true. 

`nutanix@cvm$ 10.x.x.x.:~$ zeus_config_printer | grep "is_secure" is_secure_booted: false nutanix@cvm:10.x.x.x.$` 

## **G9 Platforms: Disabling Host Secure Boot Using IPMI raw Commands** 

Disable host secure boot on Nutanix platforms using IPMI `raw` commands. 

## **About this task** 

## **Procedure** 

**1.** On your AHV host, log in as root. 

**2.** From the AHV host prompt, verify the current secure boot status. 

`[root@User1-Clust-0 ~]# ipmitool raw 0x30 0x70 0x91 0x00 01` 

00 = secure boot is disabled 

01 = secure boot is enabled 

02 = the default setting is applied. 

**3.** Disable secure boot using the IPMI `raw` command. 

`[root@User1-Clust-0 ~]# ipmitool raw 0x30 0x70 0x91 0x01 00 00` 

**4.** Restart the host. 

`[root@User1-Clust-0 ~]# reboot` 

Platform | Host Secure Boot (UEFI) | **104** 

## **5.** Exit root. 

## **G10 Platforms: Disabling Host Secure Boot** 

Disable host secure boot on G10 platforms using the RedFish API. 

## **Procedure** 

## **1.** Disable secure boot. 

URL: /redfish/v1/Systems/1/SecureBoot 

Method: PATCH Payload: {"SecureBootEnable": false} Response: 202 Example: `nutanix@cvm$ curl -k -u` _`username`_ `:` _`password`_ `\ -X PATCH https://` _`BMC-IP-address`_ `/redfish/v1/Systems/1/SecureBoot \ -H 'Content-Type: application/json' \ -d '{"SecureBootEnable": false}' { "Success":{ "code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

## **2.** Restart the host. 

**Caution:** Place the host into maintenance mode and move all VMs to another node before continuing. The node restarts at the end of this step. 

URL: /redfish/v1/Systems/1/Actions/ComputerSystem.Reset 

Method: POST Payload: {"ResetType": "ForceRestart"} Response: 202 Example: `nutanix@cvm$ curl -k u` _`username`_ `:` _`password`_ `\ -X POST "https://<IPMI_IP>/redfish/v1/Systems/1/Actions/ComputerSystem.Reset" \ -d "{\"ResetType\": \"ForceRestart\"}" { "Success": { " code": "Base.v1_4_0.Success", "Message": "Successfully Completed Request." } }` 

Platform | Host Secure Boot (UEFI) | **105** 

**3.** Verify the secure boot status. 

## **Redfish API method:** 

URL: /redfish/v1/Systems/1/SecureBoot Method: GET Payload: None Response: 200 Example: `nutanix@cvm$ curl -k -u` _`username`_ `:` _`password`_ `\ -X GET https://` _`IPMI-IP-address`_ `/redfish/v1/Systems/1/SecureBoot { "@odata.type": "#SecureBoot.v1_0_6.SecureBoot", "@odata.id": "/redfish/v1/Systems/1/SecureBoot", "Id": "SecureBoot", "Name": "Secure Boot", "SecureBootCurrentBoot": "Disabled", "SecureBootEnable": false, "SecureBootMode": "SetUpMode", "Actions": { "Oem": {}, "#SecureBoot.ResetKeys": { "target": "/redfish/v1/Systems/1/SecureBoot/Actions/SecureBoot.ResetKeys", "@Redfish.ActionInfo": "/redfish/v1/Systems/1/SecureBoot/ResetKeysActionInfo" } }, "@odata.etag": "5b0f2f217147c086510ae101e8a7d6a1" }` 

**4.** Check that the `SecureBootCurrentBoot` parameter is set to `Disabled` . 

**10** 

## **INTEL SST PERFORMANCE PROFILE** 

## **The Intel SST Performance Profile** 

Details of how Nutanix uses Intel SST-PP. 

Nutanix supports the Intel Speed Select Technology Performance Profile (SST-PP) on platforms that use Intel Sapphire Rapids and Intel Emerald Rapids processors. SST-PP allows you to choose from a number of profiles on an Intel processor to specify the number of cores and their associated core frequencies on that processor. 

Nutanix supports the following SST-PP settings. 

- **Auto** : this setting is the default. It means that the core count and CPU frequency are the same as the CPU specification. 

- **Level 0** : All cores are enabled with the maximum thermal design power (TDP). 

- **Level 3** : Intel Level 1 core profile (balanced performance and power consumption). 

- **Level 4** : Intel Level 2 core profile (lowest core count with highest base frequency; lowest TDP). 

New systems always ship with the default **Auto** profile selected. To make any changes to the SST-PP profile, you must edit the SST-PP settings in the BIOS before imaging the system, using the procedure described in this document. 

**Caution:** Once you image the system, you cannot change the SST-PP profile. Attempting to change the SST-PP profile after imaging can cause unpredictable system behavior and possible loss of data. 

The minimum required BIOS version for Intel SST-PP support is E : _`x`_ 30.101. 

To make SST-PP changes in the BIOS persistent, you must have LCM 3.1 or later. 

## **Managing the Intel SST Performance Profile Through IPMI** 

Edit Intel SST-PP settings in IPMI through API. 

## **Procedure** 

**1.** Configure the SST settings with the `curl` command. 

`[nutanix@localhost ~]$ curl -X PATCH --insecure --user` _`user_id`_ `:` _`password`_ `"https://` _`IPMI_IP`_ `/redfish/v1/Systems/1/Bios" -d "{\"Attributes\": {\"IntelSSTPP#3CD8\": \"` _`SST_setting`_ `\"}}"` 

- Replace ~~a~~ _`user_id`_ with your IPMI user ID. 

- Replace ~~a~~ _`password`_ with your IPMI password. 

- Replace ~~a~~ _`IPMI_IP`_ with your IPMI IP address. 

- Replace ~~a~~ _`SST_setting`_ with one of **Auto** , **Level0** , **Level3** , or **Level4** . 

Platform | Intel SST Performance Profile | **107** 

**2.** Restart the node. 

`[nutanix@localhost ~]$ curl -X POST --insecure --user` _`user_id`_ `:` _`password`_ `\ "https://` _`IPMI_IP`_ `/redfish/v1/Systems/1/Actions/ComputerSystem.Reset" \ -d "{\"ResetType\": \"ForceRestart\"}"` 

   - Replace ~~a~~ _`user_id`_ with your IPMI user ID. 

   - Replace ~~a~~ _`password`_ with your IPMI password. 

   - Replace ~~a~~ _`IPMI_IP`_ with your IPMI IP address. 

**3.** Check the core count against the CPU specification. 

`[nutanix@localhost ~]$ curl -X GET --insecure --user` _`user_id`_ `:` _`password`_ `"https://` _`IPMI_IP`_ `/redfish/v1/Systems/1/Processors/` _`processor_id`_ `"` 

   - Replace ~~a~~ _`user_id`_ with your IPMI user ID. 

   - Replace ~~a~~ _`password`_ with your IPMI password. 

   - Replace ~~a~~ _`IPMI_IP`_ with your IPMI IP address. 

   - Replace ~~a~~ _`processor_id`_ with the CPU ID. 

**4.** Image the node with Foundation. 

**5.** Use the Host CPU Utility to verify the core count. 

`[nutanix@localhost ~]$ lscpu Architecture:            x86_64 CPU op-mode(s):          32-bit, 64-bit Byte Order:              Little Endian CPU(s):                  96 On-line CPU(s) list:     0-95 Thread(s) per core:      2 Core(s) per socket:      24 Socket(s):               2` 

**6.** (Optional) To revert to the default Intel SST-PP setting, use the `curl` command. 

`[nutanix@localhost ~]$ curl -X PATCH --insecure --user` _`user_id`_ `:` _`password`_ `"https://` _`IPMI_IP`_ `/redfish/v1/Systems/1/Bios" -d "{\"Attributes\": {\"IntelSSTPP#3CD8\": \"Auto\"}}"` 

- Replace Pe _`user_id`_ with your IPMI user ID. 

- Replace a _`password`_ with your IPMI password. 

- Replace Le _`IPMI_IP`_ with your IPMI IP address. 

## **Managing the Intel SST Performance Profile Through the BIOS** 

Edit Intel SST-PP settings through the BIOS. 

Platform | Intel SST Performance Profile | **108** 

## **Procedure** 

**1.** Restart the host and interrupt the startup to enter the BIOS. 

**2.** Navigate to **Advanced** > **Advanced power management configuration** > **CPU P state control** . 

**Figure 40: BIOS Advanced page** 

By default the **Intel SST-PP** value is set to **Auto** . 

**3.** Use the arrow keys to select **Level 0** , **Level 3** , or **Level 4** . 

**Figure 41: BIOS Advanced page** 

**4.** Save your changes and exit the BIOS. 

**5.** Image the node with Foundation. 

**6.** Use the Host CPU Utility to verify the core count. 

`[nutanix@localhost ~]$ lscpu Architecture:            x86_64 CPU op-mode(s):          32-bit, 64-bit Byte Order:              Little Endian CPU(s):                  96 On-line CPU(s) list:     0-95` 

Platform | Intel SST Performance Profile | **109** 

```
Thread(s) per core:      2
Core(s) per socket:      24
Socket(s):               2
```

## **COPYRIGHT** 

Copyright 2026 Nutanix, Inc. 

Nutanix, Inc. 1740 Technology Drive, Suite 150 San Jose, CA 95110 

All rights reserved. This product is protected by U.S. and international copyright and intellectual property laws. Nutanix and the Nutanix logo are registered trademarks of Nutanix, Inc. in the United States and/or other jurisdictions. All other brand and product names mentioned herein are for identification purposes only and may be trademarks of their respective holders. 

Platform | Copyright | **111** 


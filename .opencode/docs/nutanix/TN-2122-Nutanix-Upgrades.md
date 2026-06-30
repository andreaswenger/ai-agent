# **Nutanix Upgrades with Life Cycle Manager** 

## Legal 

© 2025 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive, Suite 150 San Jose, CA 95110 

Nutanix Upgrades with Life Cycle Manager 

## **Contents** 

**1. Executive Summary.................................................................................4 2. Life Cycle Manager Design.....................................................................5 3. Life Cycle Manager Inventory.................................................................7 4. Software Upgrades Through Life Cycle Manager.................................8** Upgrading AOS with Life Cycle Manager...........................................................................................8 Upgrading AHV with Life Cycle Manager.........................................................................................10 Upgrading Third-Party Hypervisors with Life Cycle Manager...........................................................14 Upgrading Dark Site with Life Cycle Manager................................................................................. 16 **5. Firmware Upgrades Through Life Cycle Manager..............................18** Available Upgrades and Dependency Checking.............................................................................. 18 Orchestration Engine.........................................................................................................................20 Redfish Protocol................................................................................................................................23 **About Nutanix.............................................................................................24 List of Figures.............................................................................................................................................25** 

Nutanix Upgrades with Life Cycle Manager 

## 1. Executive Summary 

Nutanix designed the upgrade process from the ground up to provide the best customer experience and reduce risk during upgrades, which historically are risky and timeconsuming for datacenters. One-click upgrades apply to all the software products in the Nutanix platform. 

To maintain simplicity, we developed the second generation of one-click upgrade and released it as Life Cycle Manager (LCM), which allows simple upgrades of software layers that have multiple dependencies. 

The first goal of LCM was to expand server firmware upgrades beyond NX-branded appliances. LCM can provide one-click firmware upgrades for all Nutanix appliances, including OEM appliances. LCM is the only tool that can upgrade firmware on multiple server platforms. 

_Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|May 2019|Original publication.|
|2.0|May 2022|Updated screenshots|
|||throughout and the Software|
|||Upgrades and Firmware|
|||Upgrades sections. Added|
|||LCM Design, Taking Inventory,|
|||Redfish Protocol, and In-VM|
|||Updates (IVU) sections.|
|3.0|June 2023|Updated for LCM 2.6 and|
|||branding changes.|
|3.1|August 2024|Updated for LCM 3.0.|
|3.2|November 2024|Updated AOS release cycle|
|||descriptions.|
|3.3|August 2025|Updated document structure.|



For more information, see the Life Cycle Manager Guide. 

© 2025 Nutanix, Inc. All rights reserved  | **4** 

Nutanix Upgrades with Life Cycle Manager 

## 2. Life Cycle Manager Design 

Old software and firmware can lead to lost productivity, higher maintenance costs, and security vulnerabilities. Intruders can break through the security in older hardware, and older software versions often lack feature improvements, such as asset management, of newer software versions. Even if you apply the latest firmware and software updates, you still must ensure compatibility and security compliance with other versions and tools. All of these factors make the process of applying upgrades cumbersome and time-intensive. 

Life Cycle Manager (LCM) greatly simplifies infrastructure upgrades and version compliance. Data collected on the Dell XC Series 630-10 for a four-node cluster shows that LCM 2.0 is 70 percent faster than a traditional update cycle. 

Figure 1: Update Time: LCM vs. Traditional 

We designed LCM with a pluggable framework so that you can keep it updated in all your environments (automatically on portal-connected clusters) and update software and firmware independently and instantly when Nutanix releases the software and firmware modules. 

© 2025 Nutanix, Inc. All rights reserved  | **5** 

Nutanix Upgrades with Life Cycle Manager 

You can automatically update Nutanix Cluster Check (NCC) with LCM. We plan to expand this option to other software modules to mirror a cloud-like experience where all software is updated automatically. 

Figure 2: LCM Design 

Starting with LCM 3.0, update failures trigger an alert in the user interface. For ESXi updates, LCM also displays an alert if the pinned user VMs aren't on. 

With its platform filtering feature, LCM blocks platform updates to versions that are past their end-of-support dates. This feature ensures that you only update to versions that are supported and validated on your platform. 

© 2025 Nutanix, Inc. All rights reserved  | **6** 

Nutanix Upgrades with Life Cycle Manager 

## 3. Life Cycle Manager Inventory 

Before you perform any upgrade operation, run an inventory to understand what software version runs on the system and what firmware each node contains. The inventory also updates the Life Cycle Manager (LCM) framework if required. The LCM page in Prism displays this inventory for each cluster. After you start the initial inventory, you can set it up to run automatically at a specified interval, typically every 24 hours. 

LCM initially builds the inventories by downloading all the upgrade modules, either from the Nutanix portal or a dark site bundle, and storing them in the Catalog and the Insights Data Fabric (IDF). The Catalog is a service running on the Controller VM (CVM) that stores the images, and the IDF is the distributed database that the Nutanix platform uses to store configuration, alert, and performance data. LCM then determines which modules it must send to which endpoints; for example, every node gets the CVM and host modules. LCM sends the modules to each endpoint using SSH. 

After the modules are in place, the inventory process asks each one to run its detect function, which queries its related entities and discovers the current versions on all endpoints. LCM uses this data to build the inventory summary and stores it in the IDF for future lookups. LCM can then calculate the available upgrade versions by comparing the discovered inventory data to options on the portal or dark site bundle. 

The inventory updates when you use LCM to perform future upgrades. When Nutanix releases new content, LCM prompts you to run the inventory process if you didn't run it yet. You can also request a manual inventory through Prism to confirm the current state of the endpoints at any time. 

LCM 3.0 introduces inventory and update support for the Vulnerabilities widget on the Security Dashboard in Prism Central. This feature allows you to manually update the Security Dashboard to view a list of security vulnerabilities (or CVEs) associated with your clusters. 

© 2025 Nutanix, Inc. All rights reserved  | **7** 

Nutanix Upgrades with Life Cycle Manager 

## 4. Software Upgrades Through Life Cycle Manager 

As datacenters and the entire world become increasingly software-driven, updates shouldn't be a concern. Establishing a process and reputation that build confidence in regular updates is central to our mission. 

In legacy architectures, you must upgrade each piece of a traditional solution separately and in a specific order to maintain functionality and availability. Complex traditional upgrade processes generate massive upgrade manuals that are time-consuming to create and maintain. You also must review them with every version upgrade because the architectures change over time, which affects the process and number of pieces involved in an upgrade. 

For these reasons, organizations don't upgrade as often as they should. Many elect to only upgrade to major releases or even skip major releases, applying only critical security fixes in between. Many organizations elect to pay for professional services or a partner to perform the upgrades. 

Through Life Cycle Manager (LCM), Nutanix offers simple upgrades for platform software components. After you run an inventory, you can view and select available software updates from the LCM menu and create an update plan. 

After you select the software to upgrade, you can review the update plan and start the upgrade, which runs in the background. 

Starting with LCM 3.0, Prism Central supports the Direct Upload functionality. You must run pc.2024.1 or later to use this feature effectively. 

## **Upgrading AOS with Life Cycle Manager** 

The control plane (Prism) and data plane (AOS storage) are updated as part of an AOS upgrade. AOS upgrades typically offer several benefits, such as bug fixes, security updates, new features, and performance updates. AOS is typically the most upgraded 

© 2025 Nutanix, Inc. All rights reserved  | **8** 

Nutanix Upgrades with Life Cycle Manager 

layer in the Nutanix platform. You can perform these upgrades without moving VMs or upgrading the underlying hypervisor. 

Life Cycle Manager (LCM) shows the latest supported version by default, but you can select a different version by clicking the version number. To check whether you can upgrade from one version of AOS to another, see Upgrade Paths. 

To upgrade AOS with LCM, follow these steps: 

**1.** Select the AOS upgrade. 

**2.** Click **View Update Plan** . 

**3.** Review the plan and click **Apply 1 Update** . 

LCM pulls the package directly from the Nutanix Support Portal. 

For clusters without internet connectivity, download the AOS LCM bundle from the Nutanix Support Portal and directly upload the bundle to LCM (with LCM 2.4.2 and later versions) or set up a local web server. The software binaries are copied to two nodes in the cluster to ensure that a copy is always available during the process. 

The first phase of any upgrade is running preupgrade checks. These checks ensure that an upgrade can be successful. The first check is for version compatibility, ensuring that you can upgrade AOS from your existing version and still support the installed hypervisor version and any other features running in the cluster. Next, the process ensures that all Controller VMs (CVMs) and hypervisor nodes have network connectivity between them. Other checks include a check for free space and cluster status and Cassandra, Zookeeper, and Stargate health checks. After passing all the prechecks, the upgrade process proceeds. If an error or blocker occurs, Prism displays a message and logs the error in the alerts section for review. 

The upgrade process installs the new AOS version to all nodes in the cluster in parallel, but the new version isn't active until the CVM restarts. After it installs the new AOS version on all the CVMs, the process issues the upgrade token to the first node identified. Only one CVM in a cluster can hold the upgrade token, which means that the CVM must be on the only node that can restart. This first node restarts just the CVM to activate the new AOS version. 

During AOS upgrades, you don't need to migrate the VMs running on each node. While the node holding the upgrade token performs the CVM restart, the hypervisor temporarily redirects I/O from the local VMs to other CVMs in the cluster. This I/O redirection 

© 2025 Nutanix, Inc. All rights reserved  | **9** 

Nutanix Upgrades with Life Cycle Manager 

eliminates the need to move VMs around and reduces the time required to perform an upgrade. 

Different hypervisors redirect I/O differently. ESXi and Hyper-V use a process called CVM Autopathing, which uses HA.py to forward traffic from the local internal address (192.168.5.2) to the external IP addresses of other CVMs throughout the cluster. This process keeps the datastore intact; the CVM responsible for serving the I/O is just remote. After the local CVM returns and stabilizes, the redirection ends and the local CVM takes over all new I/O again. 

Nutanix AHV uses iSCSI multipathing, where the primary path is the local CVM and the two other paths are remote. If the primary path fails, one of the other paths becomes active. As with Autopathing, when the local CVM comes back online, it takes over again as the primary path. 

After the node holding the token restarts, it ensures that all local services on the CVM run and safely rejoin the cluster after passing an integrity check. The node then releases the upgrade token, and the upgrade process issues it to the next node in the cluster. The process repeats for each node in the cluster until the upgrade finishes. All these steps and checks together produce a rolling, nondisruptive upgrade process where you don't need to migrate any VMs. 

## **Upgrading AHV with Life Cycle Manager** 

You can perform AHV upgrades through Life Cycle Manager (LCM). 

To upgrade AHV on clusters with internet connectivity, follow these steps: 

**1.** Select the AHV upgrade. 

**2.** Click **View Update Plan** . 

**3.** Review the plan and click **Apply Update(s)** . 

LCM pulls the package directly from the Nutanix Support Portal. 

For clusters without internet connectivity, download the AHV LCM bundle from the Nutanix Support Portal and directly upload the bundle to LCM or set up a local web server. 

The upgrade process first evaluates the cluster to ensure that it's healthy before allowing an upgrade to begin. It uses the JSON metadata file included with each version to 

© 2025 Nutanix, Inc. All rights reserved  | **10** 

Nutanix Upgrades with Life Cycle Manager 

validate that the cluster can update from the currently running version of AHV to the uploaded version and that it's compatible with the AOS version running on the cluster. After passing these checks, the process copies the AHV tar package to the hypervisor host on each node in the cluster so it's accessible during its phase of the rolling upgrade. 

The upgrade process selects the first node to upgrade, issues it the upgrade token, and puts that node in maintenance mode. VMs running on that node live-migrate to other nodes in the cluster, but the following VM types can't live-migrate: 

- Agent VMs 

- VMs using GPU passthrough 

- Nested VMs with CPU passthrough 

These VM types receive an ACPI shutdown request for a graceful shutdown. If the VM doesn't shut down in the allotted two minutes, the process issues a shutdown command for any VMs that are still running. 

**Note:** Agent VMs are always the last to shut down and the first to turn back on following the Controller VM (CVM). 

The VMs that you couldn't migrate because of their physical constraints turn on automatically after you upgrade and restart the host to complete the process. 

Figure 3: VM Evacuation 

After you move or turn off all the VMs, the process runs the upgrade commands. AHV uses a standard yum upgrade process and references the repository on the local CVM (Nutanix repo) where the process placed the tar file. Yum compares the versions of all the RPM packages in the local upgrade repo against the installed versions, and it only upgrades the RPM packages that have an updated version available. The infrastructure process on the local CVM controls this workflow using SSH commands issued to the host. 

© 2025 Nutanix, Inc. All rights reserved  | **11** 

Nutanix Upgrades with Life Cycle Manager 

Some of the upgraded RPM packages have new configuration settings. In this case, the upgrade workflow uses the configuration management tools Puppet and Salt to apply the configuration changes immediately, keeping the node in compliance with the cluster's configuration. 

When the node finishes upgrading, the automated process issues a time-delayed shutdown command to the AHV host that gracefully shuts down the local CVM and restarts the node. 

Figure 4: CVM Shutdown 

After restarting the host, the CVM automatically starts and must pass upgrade checks that ensure that the host upgrade was successful. The Genesis service verifies that it's running the new kernel version, and then the CVM runs an abbreviated storage integrity check before rejoining the storage cluster. The cluster understands that you shut down the CVM safely as part of the upgrade process, so it doesn't run the full integrity check that's required when an unexpected failure occurs. 

Figure 5: VM Locality Restoration 

After the integrity checks finish, the host exits maintenance mode. Its unmigrated VMs turn back on, and the VMs that live-migrated to other hosts move back to the newly upgraded node to restore data locality. 

© 2025 Nutanix, Inc. All rights reserved  | **12** 

Nutanix Upgrades with Life Cycle Manager 

Figure 6: Host Upgrade Complete 

During the rolling upgrade process for the cluster, when you live-migrate VMs between hosts to allow a node to upgrade, the infrastructure service prevents compatibility issues. A cluster going through the upgrade process has two versions of AHV deployed: some hosts have the existing version (which we call AHV.old), and others that you already upgraded have the upgraded version (AHV.new). When you live-migrate VMs between nodes, they can only move between hosts that have AHV.old and AHV.new or from hosts with AHV.old to hosts with AHV.new. The process prevents VMs from a host with AHV.new from moving to one with AHV.old to avoid any loss of new capabilities. 

The upgrade process selects the next host to upgrade in the cluster, passes it the upgrade token, and repeats the process until it upgrades the entire AHV cluster. You can monitor the upgrade progress from LCM or the Tasks view in Prism to see which host the process is currently upgrading and the overall progress. 

Although it isn't a typical approach, you can also use a command-line interface (CLI) to run AOS and AHV upgrades if desired or instructed by Nutanix Support. The CLI method provides additional flexibility not available in Prism, such as the ability to change pinned VM migration behaviors or specify a different repository for upgrade packages. 

If any upgrade commands issue a nonzero response code, the upgrade fails and Genesis stops the upgrade. In these situations, contact Nutanix Support to resolve the issue. To retry the upgrade on the host that experienced the failure, restart the Genesis service on the local CVM to force it to retry the same upgrade. If it fails again, see the log files to identify which command is failing to determine how to proceed. 

You can check several logs if you need more details than the Prism event and task lists provide. During the upgrade process, data is written to the following logs: 

© 2025 Nutanix, Inc. All rights reserved  | **13** 

Nutanix Upgrades with Life Cycle Manager 

## • Genesis logs on CVMs: 

   - › `/home/nutanix/data/logs/genesis.out` 

   - › `/home/nutanix/data/logs/host_preupgrade.out` (on Genesis leader, node that triggers upgrade) 

   - › `/home/nutanix/data/logs/host_upgrade.out` 

   - › `/home/nutanix/data/logs/lcm_ops.out` 

- AHV logs on hosts: 

   - › `/var/log/yum.log` (explains what the upgrade ran) 

   - › `/var/log/upgrade_config.log` (primary log for upgrade details) 

   - › `/var/log/upgrade_config-puppet.log` (Puppet-related details; examine if mentioned in `upgrade_config.log` ) 

   - › `/var/log/upgrade_config-salt.log` (Salt-related details; examine if mentioned in `upgrade_config.log` ) 

LCM also supports NVIDIA GRID driver updates exclusively for platforms that use AHV. However, LCM only updates the NVIDIA GRID driver during AHV updates. Before updating your NVIDIA GRID driver version, ensure that the update doesn't cause a version mismatch between the AHV host and the guest VM. LCM blocks AHV updates if it detects an incompatible NVIDIA GRID driver, which means you can't update AHV if you delete the NVIDIA GRID driver bundle at any point. To re-enable AHV updates, upload the NVIDIA GRID driver again and perform an LCM inventory. For more information, see the Updating the NVIDIA GRID Driver with LCM section of the Life Cycle Manager Dark Site Guide. 

## **Upgrading Third-Party Hypervisors with Life Cycle Manager** 

You can use Life Cycle Manager (LCM) to upgrade ESXi on NX hardware. For more information, see the LCM User Guide. 

Upgrading a third-party hypervisor such as ESXi (on non-NX hardware) or HyperV is a one-click upgrade in Prism Element under **Settings** > **Upgrade Software** > **Hypervisor** . You must first download the hypervisor upgrade bits from the vendor's 

© 2025 Nutanix, Inc. All rights reserved  | **14** 

Nutanix Upgrades with Life Cycle Manager 

portal, upload them to Prism as part of the one-click process, and then download a JSON metadata file from the Nutanix Support Portal for your target hypervisor version. After you download these files, upload them to Prism Element, click the **Upgrade** button, and let the automated process take care of everything. 

First, the upgrade process evaluates the cluster to ensure that it's healthy before allowing an upgrade to begin. It uses the JSON metadata file to validate that the hypervisor version uploaded can update from the running version and is compatible with the AOS version currently running on the cluster. In addition to the JSON checks, the upgrade process performs several prechecks before allowing the upgrade to proceed. It performs a precheck for cluster status and free space and Cassandra, Zookeeper, and Stargate health checks. After passing the prechecks, the process copies the hypervisor package to the Controller VM (CVM) on each node in the cluster so it's accessible during its phase of the rolling upgrade. 

The upgrade process selects the first node to upgrade, issues it the upgrade token, and asks to put that node in maintenance mode. VMs running on that node live-migrate to other nodes in the cluster, but some VM types can't live-migrate: 

- Agent VMs 

- VMs using hardware or GPU passthrough 

- Nested VMs with CPU passthrough 

You must remediate these VMs, either before you begin the upgrade process or manually during the upgrade. Identifying all VMs in the cluster that can't live-migrate and shutting them down before starting the upgrade is the simplest method but results in longer downtime for these VMs, as they must turn back on after the upgrade finishes. The other option is to monitor the upgrade process using the hypervisor management console and, when a node enters maintenance mode, shut down these VMs on just that node and turn them back on after the node restarts. Repeat this process as you upgrade each node in the cluster. 

**Note:** If a node fails to enter maintenance mode, this process and the one-click upgrade process eventually time out, causing the upgrade to fail. You can remediate the problem VMs and restart the upgrade process to complete the remaining nodes in the cluster. 

© 2025 Nutanix, Inc. All rights reserved  | **15** 

Nutanix Upgrades with Life Cycle Manager 

After all the VMs move or turn off, the process runs the upgrade commands. With thirdparty hypervisors, you perform the upgrades using the CLI from a remote CVM in the cluster and source the hypervisor upgrade bits from another remote CVM in the cluster. 

When the node finishes upgrading, the automated process issues a time-delayed shutdown command to the hypervisor host that restarts the node. 

After restarting the host, the CVM automatically starts and must pass upgrade checks that ensure the host upgrade was successful. Then the CVM runs an abbreviated storage integrity check before rejoining the storage cluster. Because the cluster understands that you safely shut down the CVM as part of the upgrade process, it doesn't run the full integrity check that's required when an unexpected failure occurs. 

After the integrity checks finish, the host exits maintenance mode. The VMs that failed to live-migrate and turned off either restart now or wait until the upgrade completes. Depending on the hypervisor and the settings, some of the VMs that live-migrated to other hosts move back to the newly upgraded node. 

The upgrade process selects the next host to upgrade in the cluster, passes it the upgrade token, and repeats the process until it upgrades the entire hypervisor cluster. You can monitor the upgrade progress from the Tasks view in Prism to see which host the process is currently upgrading and the overall progress. 

## **Upgrading Dark Site with Life Cycle Manager** 

Life Cycle Manager (LCM) supports software and firmware upgrades for dark sites. To upgrade a dark site, follow these steps: 

**1.** In the LCM toolbar, go to **Settings** > **Update Source** . 

**2.** Set the **Source** field to one of the following categories: 

   - Direct Upload: Select **Dark Site (Direct Upload)** and click **Upload Bundle** . 

The system redirects you to the Directs Uploads page. 

- Web Server: Select **Dark Site (Local Web Server)** , specify the URL, and click **Save** . 

If you use LCM version 2.4.5 or earlier, you must upgrade the LCM framework bundle to the **-H** version using direct upload to support dark site upgrades. For more information, 

© 2025 Nutanix, Inc. All rights reserved  | **16** 

Nutanix Upgrades with Life Cycle Manager 

see the LCM Framework Update using Dark Site Method section of the Life Cycle Manager Guide. 

© 2025 Nutanix, Inc. All rights reserved  | **17** 

Nutanix Upgrades with Life Cycle Manager 

## 5. Firmware Upgrades Through Life Cycle Manager 

Upgrading firmware typically involves time-consuming research to check whether an upgrade is available, needed, and beneficial before moving forward. The process of running the actual upgrades also varies greatly between vendors. Depending on your configuration, a vendor might provide multiple options. 

Life Cycle Manager (LCM) simplifies firmware upgrades for Nutanix environments. LCM provides a single process in Prism that identifies and qualifies upgrade paths and then uses a nondisruptive, one-click process to complete the upgrade. LCM can perform firmware upgrades on the following server platforms: 

- Nutanix NX 

- Cisco UCS M6 and M7 

- Dell XC and XC Core 

- Lenovo HX and HX Core 

- HPE DX 

- HPE DL (G10) 

- Fujitsu XF 

- Intel DCB 

**Note:** We recommend using the latest versions of Nutanix AOS, Foundation, and LCM when you upgrade firmware. 

## **Available Upgrades and Dependency Checking** 

To determine if a software or firmware upgrade is available and needed, Prism compares the available inventory to the metadata available on the Nutanix Support Portal or dark 

© 2025 Nutanix, Inc. All rights reserved  | **18** 

Nutanix Upgrades with Life Cycle Manager 

site bundle. This comparison provides a convenient view of available upgrades for the nodes and cluster. 

Figure 7: LCM Dependencies 

As we expand our supported hardware base and release additional software products, the amount of invisible logic and automation required to manage the growing list of dependencies also increases. The logic understands dependencies and recommends an upgrade version. The version recommendation considers the firmware's dependencies on other items in the node and the versions for software like Nutanix AOS running on the cluster. Sometimes, before an upgrade can finish, the system must remediate the dependencies, which it typically does during the upgrade process because the orchestration engine understands package order. 

Often, more than one upgrade version is available for a particular entity. The recommended version is generally the best choice because the recommendation accounts for dependencies and the maturity of the firmware version. For example, a firmware version that was available for several months and widely deployed is preferable to one that's only weeks old. If you choose a version other than the recommended one, Prism allows you to easily change the version to apply. 

Finally, the logic knows if a server vendor requires all firmware updates to be bundled and upgraded together. 

© 2025 Nutanix, Inc. All rights reserved  | **19** 

Nutanix Upgrades with Life Cycle Manager 

## **Orchestration Engine** 

The orchestration engine behind Life Cycle Manager (LCM) upgrades takes the inputs from the inventory, available versions, and dependencies and runs the nondisruptive upgrades. At this stage, users can review the inventory, select the entities and versions to upgrade (downloaded from the Nutanix Support Portal or a dark site bundle), and start the upgrade. 

When the process starts, it evaluates the cluster to ensure that it's healthy before it allows an upgrade to begin. The engine polls all hosts in the cluster; the order of the hosts that the poll returns becomes their upgrade order. The process discovers any VMs that can't migrate and gives you a list of pinned VMs to remediate before the upgrade starts. Clusters running AHV turn off these VMs on a host as it enters maintenance mode. Other hypervisors, such as ESXi, require you to turn off the pinned VMs until the upgrade finishes. You often can't live-migrate these VMs to another host because of a passthrough hardware device or an affinity rule that only allows the VM to run on a declared host. 

Figure 8: LCM Upgrade Flow Part 1 

After you remediate the pinned VMs, AOS issues the shutdown token to the first host on the list. Only one host in a cluster can possess the shutdown token at a time; the token allows it to enter maintenance mode and restart. The host with the shutdown token issues a request to the hypervisor to enter maintenance mode, which evacuates all remaining VMs from the host with the Controller VM (CVM). After all VMs evacuate, the CVM receives a maintenance mode command. 

© 2025 Nutanix, Inc. All rights reserved  | **20** 

Nutanix Upgrades with Life Cycle Manager 

Figure 9: LCM Upgrade Flow Part 2 

With the CVM in maintenance mode, the upgrade process takes one of two paths to upgrade the firmware based on the capabilities of the underlying server infrastructure. A few server vendors offer server tools installed in the hypervisor layer and allow you to upgrade firmware using these tools. In this case, the upgrade process batches the firmware upgrades together and runs them in a single serial upgrade process. This batched process applies the upgrades in the order required by the understood dependencies. Rare cases might require an additional restart before applying a specific firmware upgrade; in these cases, the node restarts, and the server tools apply any remaining upgrades after the restart. 

For server platforms without server tools, the upgrade process restarts the node into Nutanix Phoenix. Phoenix is a Linux-based ISO that allows a node to boot into an environment and provides the tools necessary to upgrade firmware. Another CVM in the cluster presents the Phoenix ISO and it mounts using the IPMI interface on the node that you're upgrading. In Phoenix, the upgrade process batches the firmware upgrades together and runs them in a single serial upgrade process, just as with server tools. After the upgrade process finishes on the node, the node receives a restart command. 

© 2025 Nutanix, Inc. All rights reserved  | **21** 

Nutanix Upgrades with Life Cycle Manager 

Figure 10: LCM Upgrade Flow Part 3 

The node starts back into the installed hypervisor and starts the CVM. The process asks the node to exit hypervisor maintenance mode. The local CVM processes health checks before exiting maintenance mode and rejoining the cluster from a storage perspective, and the LCM inventory updates to reflect the upgrade date and the new firmware versions for the node. The process selects the second node from the list and repeats the same steps, working through each node in the cluster until it's upgraded every node. Then the upgrade task shows as completed in the Tasks view in Prism. 

Figure 11: LCM Upgrade Flow Part 4 

© 2025 Nutanix, Inc. All rights reserved  | **22** 

Nutanix Upgrades with Life Cycle Manager 

On NX platforms, LCM can perform disk firmware updates through the CVM without requiring a host restart. Because the data disks and host bus adapter (HBA) controller pass through to the CVM, LCM can start the CVM into a live CD that performs the updates on the disks and HBA controller instead of starting the node into Phoenix. Using this method, you don't need to migrate the VMs on the host, which results in much faster updates. 

## **Redfish Protocol** 

Life Cycle Manager (LCM) uses the Redfish protocol to update firmware through a virtual USB NIC on modern hardware generations on select platforms, including Nutanix NX, Dell XC, HP DX, and Fujitsu XF. For the latest platforms, see the Release Notes. 

The Redfish protocol provides a RESTful interface for infrastructure management, using HTTP methods for retrieving information on a server's BIOS, BMC, and other components without having to start into Phoenix (bootable ISO environment) and run vendor tools. The most visible effect of Redfish is that LCM performs BMC and BIOS updates about twice as fast as the procedure that uses the Phoenix ISO. With Redfish, LCM performs fewer system restarts. Using the older procedure, LCM restarted the system twice for BMC updates and three times for BIOS updates. With Redfish, LCM doesn’t need to restart the system for BMC updates and only needs one restart for BIOS updates. 

LCM detects and shows Redfish availability in the UI. For the full system requirements, see the Life Cycle Manager Guide. 

© 2025 Nutanix, Inc. All rights reserved  | **23** 

Nutanix Upgrades with Life Cycle Manager 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2025 Nutanix, Inc. All rights reserved  | **24** 

Nutanix Upgrades with Life Cycle Manager 

## **List of Figures** 

Figure 1: Update Time: LCM vs. Traditional..................................................................................................5 Figure 2: LCM Design.................................................................................................................................... 6 Figure 3: VM Evacuation..............................................................................................................................11 Figure 4: CVM Shutdown.............................................................................................................................12 Figure 5: VM Locality Restoration................................................................................................................12 Figure 6: Host Upgrade Complete............................................................................................................... 13 Figure 7: LCM Dependencies...................................................................................................................... 19 Figure 8: LCM Upgrade Flow Part 1............................................................................................................20 Figure 9: LCM Upgrade Flow Part 2............................................................................................................21 Figure 10: LCM Upgrade Flow Part 3..........................................................................................................22 Figure 11: LCM Upgrade Flow Part 4..........................................................................................................22 


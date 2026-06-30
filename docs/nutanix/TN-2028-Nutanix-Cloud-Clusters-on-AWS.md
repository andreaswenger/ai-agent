# **Nutanix Cloud Clusters on AWS** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Nutanix Cloud Clusters on AWS 

## **Contents** 

**1. Executive Summary.................................................................................5 2. Nutanix Cloud Clusters Console............................................................9 3. Nutanix Cloud Networking....................................................................11** Subnet Creation................................................................................................................................ 15 Flow Virtual Networking.................................................................................................................... 17 Security Groups.................................................................................................................................21 Multicast Network Support................................................................................................................ 26 

**4. Migration to Nutanix Cloud Clusters on Amazon Web Services....... 30 5. Storage for Nutanix Cloud Clusters on Amazon Web Services........ 32** Storage Availability............................................................................................................................34 Respond to Failures..........................................................................................................................34 **6. Hibernate and Resume with Nutanix Cloud Clusters on Amazon Web Services.......................................................................................... 38** Native Backup with Nutanix Cluster Protection................................................................................39 Nutanix Disaster Recovery to S3..................................................................................................... 41 **7. Nutanix Cloud Clusters on Amazon Web Services Deployment Models......................................................................................................44** Single-Cluster Deployment with Native Networking......................................................................... 44 Single-Cluster Deployment with Nutanix Flow Virtual Networking....................................................46 Disaster Recovery Across Multiple Availability Zones......................................................................48 **8. Nutanix Cloud Infrastructure Capacity Optimization......................... 51** 

**9. Encryption for Nutanix Cloud Clusters on Amazon Web Services................................................................................................... 52 10. Virtual Machine High Availability....................................................... 53** VM High Availability Recommendations and Requirements.............................................................54 **11. Acropolis Dynamic Scheduler............................................................ 55 About Nutanix.............................................................................................56 List of Figures.............................................................................................................................................57** 

Nutanix Cloud Clusters on AWS 

## 1. Executive Summary 

Nutanix designed its software to give customers running workloads in a hybrid cloud environment the same experience that they expect from on-premises Nutanix clusters. Because Nutanix in a hybrid multicloud environment runs Nutanix AOS and Nutanix AHV with the same CLI, UI, and APIs, existing IT processes and third-party integrations continue to work regardless of where they run. 

Figure 1: Overview of the Nutanix Hybrid Multicloud Software 

© 2026 Nutanix, Inc. All rights reserved  | **5** 

Nutanix Cloud Clusters on AWS 

Nutanix Cloud Clusters (NC2) on Amazon Web Services (AWS) situates the complete Nutanix hyperconverged infrastructure stack directly on an Amazon Elastic Compute Cloud (EC2) bare-metal instance. This bare-metal instance runs a Controller VM (CVM) and Nutanix AHV as the hypervisor, using the AWS elastic network interface (ENI) to connect to the network. AHV guest VMs don't require any additional configuration to access AWS services or other EC2 instances. 

AHV runs an efficient embedded distributed network controller that integrates guest VM networking with AWS networking. AHV assigns all guest VM IP addresses to the bare-metal host where the VMs run. Instead of creating an overlay network, the AHV embedded network controller simply provides the networking information of the VMs running on NC2 on AWS, even as a VM moves around the AHV hosts. Because NC2 on AWS integrates IP address management with AWS Virtual Private Cloud (VPC), AWS allocates all guest VM IP addresses from the AWS subnets in the existing VPCs. 

AOS can withstand hardware failures and software glitches and preserves application availability and performance. By combining features like native rack awareness with public cloud partition placement groups, Nutanix operates freely in a dynamic hybrid multicloud environment. You can save money by using hibernation to shut down unused clusters. You can also back up both Prism Central and guest VM data to S3 using AOS snapshots with Nutanix Multicloud Snapshot Technology (NMST). Using NMST, you can replicate AOS snapshots from your private datacenter directly to S3 for recovery. 

Nutanix software running in the cloud provides an easy extension for your on-premises datacenter. If you're already consuming cloud resources, the native Nutanix integration with AWS means that you don't need any additional skills to get your workloads running in the cloud. Management overhead shrinks when you no longer need an additional overlay network to secure and lock down networking between your on-premises environment and the cloud. Once you have Nutanix Cloud Clusters running, you can enjoy native networking speeds between migrated workloads and the new cloud services you want to consume. 

_Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|August 2020|Original publication.|
|1.1|September 2020|Updated the Cluster Outbound|
|||to the Cluster Portal table.|



© 2026 Nutanix, Inc. All rights reserved  | **6** 

Nutanix Cloud Clusters on AWS 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|1.2<br>1.3<br>1.4<br>1.5<br>1.6<br>1.6.1<br>1.7<br>1.8<br>1.9<br>2.0<br>2.1<br>2.2|September 2021<br>Added information on the<br>hibernation feature and<br>updated Storage Availability<br>in AWS and Hibernate and<br>Resume sections.<br>January 2022<br>Updated the Cluster Outbound<br>to the Cluster Portal table.<br>March 2022<br>Updated product names to<br>align with Nutanix Cloud<br>Platform packaging.<br>October 2022<br>Added heterogeneous cluster<br>support.<br>March 2023<br>Updated console.nutanix.com<br>to cloud.nutanix.com.<br>March 2023<br>Added outbound management<br>firewall requirements.<br>August 2023<br>Updated content for AOS 6.7.<br>May 2024<br>Updated content for AOS 6.8.<br>September 2024<br>Updated content for AOS<br>6.8.1. Added the Nutanix<br>Disaster Recovery to S3<br>section.<br>December 2024<br>Updated content for AOS 7.0.<br>January 2025<br>Added the Single-Cluster<br>Deployment with Native<br>Networking, Single-Cluster<br>Deployment with Nutanix<br>Flow Virtual Networking, and<br>Disaster Recovery Across<br>Multiple Availability Zones<br>sections.<br>February 2025<br>Updated the Virtual Machine<br>High Availability section.|



© 2026 Nutanix, Inc. All rights reserved  | **7** 

Nutanix Cloud Clusters on AWS 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|2.3<br>2.4<br>2.5<br>2.6|March 2025<br>Updated the Single-Cluster<br>Deployment with Native<br>Networking section.<br>June 2025<br>Updated AWS networking<br>information for AOS 7.3.<br>July 2025<br>Updated terminology for NC2<br>console.<br>May 2026<br>Updated document structure<br>and the Virtual Machine High<br>Availability section.|



© 2026 Nutanix, Inc. All rights reserved  | **8** 

Nutanix Cloud Clusters on AWS 

## 2. Nutanix Cloud Clusters Console 

You can access the NC2 console with your existing account at my.nutanix.com. You can use the console to deploy AWS clusters and manage tasks like health remediation and expanding and condensing your clusters. On-premises Prism Central instances can manage your deployed NC2 instance alongside your on-premises clusters. For day-two operations, Prism Central can also manage AOS upgrades for on-premises, remote or branch office, and cloud-based Nutanix clusters. 

Figure 2: Nutanix Cloud Clusters Management 

The NC2 console provides the following services: 

- Obtaining and managing bare-metal resources 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

Nutanix Cloud Clusters on AWS 

- Ensuring that you create and use the correct IAM roles for deployment 

- Creating AWS security group rules to help lock down your AWS resources 

- Performing hibernate and resume operations, including S3 bucket creation 

- Managing node placement strategy and removing or adding nodes based on cluster health 

Figure 3: Nutanix Cloud Clusters Console 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

Nutanix Cloud Clusters on AWS 

## 3. Nutanix Cloud Networking 

Nutanix delivers a truly hybrid multicloud experience because you can use the standalone native cloud networking or Flow Virtual Networking, which gives you the option for the low overhead of native networking with the same operational model as the rest of your hybrid multicloud. 

Nutanix integration with the AWS networking stack means that every VM deployed on NC2 on AWS receives a native AWS IP address when using native networking, so that applications have full access to all AWS resources as soon as you migrate or create them on NC2 on AWS. Because the Nutanix network capabilities are directly on top of the AWS overlay, network performance remains high and resource consumption is low because you don't need additional network components. 

AHV uses Open vSwitch (OVS) for all VM networking. You can configure VM networking through Prism or the aCLI, and each vNIC connects to a tap interface. Native networking uses the same networking stack as on-premises. Flow Virtual Networking uses separate ENIs to allow traffic to exit the cluster. The following figure shows a conceptual diagram of the OVS architecture. 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

Nutanix Cloud Clusters on AWS 

Figure 4: OVS Conceptual Architecture 

The AHV host, VMs, and physical interfaces use ports to connect to the bridges, and both bridges communicate with the AWS overlay network. Each host has the required drivers to use the AWS overlay network. 

With native network integration, you can deploy NC2 in existing AWS VPCs. Because existing AWS environments apply change control and security processes, you only need to allow NC2 on AWS to talk to an NC2 console. With this integration, you can increase security in your cloud environments. 

Nutanix uses native AWS API calls to deploy AOS on bare-metal EC2 instances and consume network resources. Each bare-metal EC2 instance has full access to its bandwidth through an ENI, so if you deploy Nutanix to an i3.metal instance, each node has access up to 25 Gbps. An ENI is a logical networking component in a VPC that represents a virtual network card, and each ENI can have one primary IP address and up to 49 secondary IP addresses. AWS hosts can support up to 15 ENIs. 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

Nutanix Cloud Clusters on AWS 

When deployed with NC2 on AWS, AHV runs the Cloud Network Controller (CNC) service on each node as a leaderless service and runs an OpenFlow controller. CNC uses an internal service called Cloud Port Manager to create and delete ENIs and assign ENI IP addresses to guest VMs. 

Cloud Port Manager can map large CIDR ranges from AWS and allows AHV to consume all or a subset of the range. If you use an AWS subnet of 10.0.0.0/24 and then create an AHV subnet of 10.0.0.0/24, Cloud Port Manager uses 1 ENI (cloud port) until all secondary IP addresses are consumed by active VMs. When the 49 secondary IP addresses are used, Cloud Port Manager attaches an additional ENI to the host and repeats the process. Because each new subnet uses a different ENI on the host, this process can lead to ENI exhaustion if you use many AWS subnets for your deployment. 

Figure 5: One-to-One Nutanix AHV and AWS Subnet Mapping 

To prevent ENI exhaustion, use an AWS subnet of 10.0.2.0/23 as your AWS target. In AHV, create two subnets of 10.0.2.0/24 and 10.0.3.0/24. With this configuration, Cloud Port Manager maps the AHV subnets to one ENI, and you can use many subnets without exhausting the bare-metal nodes’ ENIs. Because an NC2 cluster can have multiple AWS subnets if enough ENIs are available, you can dedicate ENIs to any subnet for more throughput. 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

Nutanix Cloud Clusters on AWS 

Figure 6: One-to-One and One-to-Many Nutanix AHV and AWS Subnet Mapping 

When you consume multiple AHV subnets in a large AWS CIDR range, the network controller generates an Address Resolution Protocol (ARP) request for the AWS default gateway with the ENI address as the source. Once the AWS default gateway responds, the network controller installs ARP proxy flows for all AHV subnets with active VMs on the ENI (cloud port). The ARP requests to a network’s default gateway reach the proxy flow and receive the MAC address of the AWS gateway in response. This configuration allows traffic to enter and exit the cluster, and it configures OVS flow rules so that traffic enters and exits on the correct ENI. 

For more information on the Nutanix implementation of OVS, see the AHV Administration Guide. 

NC2 on AWS creates a single default security group for guest VMs running in the Nutanix cluster. Any ENIs created to support guest VMs are members of this default security group, which allows all guest VMs in a cluster to communicate with each other. In addition to security groups, you can use Nutanix Flow Network Security to provide greater security controls for east-west network traffic. 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

Nutanix Cloud Clusters on AWS 

## **Subnet Creation** 

An AWS Region is a distinct geographic area. Each Region has multiple, isolated locations known as Availability Zones (AZs), which are logical datacenters available for any AWS customer in that Region to use. Each AZ in a Region has redundant and separate power, networking, and connectivity to reduce the likelihood of two AZs failing simultaneously. 

Create a subnet in AWS in a VPC, then connect it to AOS in Prism Element. Cloud network, a new service in the CVM, works with AOS configuration and assigns a VLAN ID (or VLAN tag) to the AWS subnet and fetches relevant details about the subnet from AWS. The network service prevents you from using the AHV or CVM subnet for guest VMs by not allowing you to create a network with the same subnet. 

You can use each ENI to manage 49 secondary IP addresses. A new ENI is also created for each subnet that you use. 

Always keep the following best practices in mind: 

- Don't share AWS guest-VM subnets between clusters. 

- Have separate subnets for management (AHV and CVM) and guest VMs. 

- If you plan to use VPC peering, use nondefault subnets so that they are unique across AWS Regions. 

- Divide your VPC network range evenly across all usable AZs in a Region. 

- In each AZ, create one subnet for each group of hosts that has unique routing requirements (for example, public versus private routing). 

- Size your VPC CIDR and subnets to support significant growth. 

## **Guest AHV IP Address Management** 

AHV uses IP address management (IPAM) to integrate with native AWS networking. NC2 on AWS uses the native AHV IPAM to inform the AWS DHCP server of all IP address assignments using API calls. NC2 relies on AWS to send gratuitous ARP packets for any additions to an ENI's secondary IP addresses. We rely on these packets to notify each hypervisor host when an IP address moves or new IP addresses become reachable. For 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

Nutanix Cloud Clusters on AWS 

guest VMs, you can't share an AWS subnet between two NC2 on AWS deployments. You can, however, use the same management subnet (AHV and CVMs) for multiple clusters. 

CNC runs an OpenFlow controller, which manages the OVS in the AHV hosts and handles mapping, unmapping, and migrating guest-VM secondary IP addresses between ENIs or hosts. A subcomponent of the cloud network controller called cloud port manager provides the interface and manages AWS ENIs. 

IPAM avoids address overlap by sending AWS API calls to inform AWS which addresses are being used. 

The AOS leader assigns an IP address from the address pool when it creates a managed vNIC, and it releases the address back to the pool when the vNIC or VM is deleted. 

**Note:** You can't use or assign the first four IP addresses or the last IP address in each subnet; they're reserved by AWS. 

Figure 7: Overview of NC2 on AWS 

By using native AWS networking, you quickly establish connectivity so that cloud administrators can focus on their tasks instead of managing additional networking technologies. The NC2 instance has full access to all AWS services, such as S3 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

Nutanix Cloud Clusters on AWS 

and other EC2 instances running in the same Amazon Virtual Private Cloud. For a walkthrough of a typical deployment, see the Creating a Cluster section of the Nutanix Cloud Clusters on AWS Deployment and User Guide. 

## **Flow Virtual Networking** 

Flow Virtual Networking builds on the native networking stack in AWS to provide the same network virtualization and control that is offered in other NC2-supported clouds. 

Flow Virtual Networking is a software-defined networking solution that provides multitenant isolation, self-service provisioning, and IP address preservation using Nutanix VPCs, subnets, and other virtual components that are separate from the physical network (AWS overlay) for the AHV clusters. It integrates tools to deploy networking features like virtual LANs (VLANs), VPCs, virtual private networks (VPNs), layer 2 virtual network extensions using a VPN or virtual tunnel end-points (VTEPs), and Border Gateway Protocol (BGP) sessions to support flexible networking that focuses on VMs and applications. For more information, see the Flow Virtual Networking Guide. 

Running Flow Virtual Networking requires that you run Prism Central on one of your NC2 on AWS clusters. Multiple NC2 clusters can use Flow Virtual Networking. The NC2 deployment process deploys Prism Central with high availability, and Prism Central hosts the control plane for Flow Virtual Networking. 

You need two new subnets when deploying Flow Virtual Networking: one for Prism Central and one for Flow Virtual Networking. The Prism Central subnet is automatically added to the Prism Element instance where it's deployed using a native AWS subnet. The Flow Virtual Networking subnet is also added to every bare-metal node. 

The Flow Virtual Networking subnet works as the external network for traffic from a Nutanix VPC. A VPC is an independent and isolated IP address space that functions as a logically isolated virtual network made of one or more subnets that are connected through a logical or virtual router. The IP addresses in a VPC must be unique, but IP addresses can overlap across VPCs. 

In AWS, Nutanix uses a two-tier topology to provide external access to the Nutanix VPCs. Deploying the first cluster in AWS with Flow Virtual Networking automatically creates a transit VPC that contains an external subnet called overlay-external-subnetnat. The transit VPC requires this external subnet to act as the Nutanix Flow VPC gateway by using an AWS ENI for north and south traffic out of the Nutanix VPC. You 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

Nutanix Cloud Clusters on AWS 

can create an additional external network in the transit VPC for routed traffic, also known as no-NAT networking. Both the NAT and no-NAT networks use a private address space to route traffic from the Nutanix user VPC and the transit VPC to the AWS ENI that hosts the Nutanix VPC gateway. 

Figure 8: Flow Virtual Networking Traffic on AWS 

In the previous diagram, the AWS ENI used at the Nutanix Flow gateway for UserVPC is node1. Flow Virtual Networking uses the host of the VPC gateway as the redirect chassis for AHV. The redirect chassis is the exit point for all traffic out of the VPC to the underlay network. Traffic for VM1 exits the redirect chassis to the transit VPC and out to the ENI, which is eth1 on node1. 

VM2 and VM3 on node2’s traffic path use the redirect chassis on node1. Traffic uses a Generic Network Virtualization Encapsulation (GENEVE) tunnel between hosts on the AHV network. The traffic is decapsulated and sent to the ENI for routing to the rest of the AWS network. 

The Flow Virtual Networking native AWS subnet consumes the Source Network Address Translation (SNAT) IP addresses and any floating IP addresses that are given to user VMs that need inbound traffic to enter the user VPC. Each NC2 bare-metal 

© 2026 Nutanix, Inc. All rights reserved  | **18** 

Nutanix Cloud Clusters on AWS 

node consumes the primary ENI IP address, and 60 percent of the native Flow Virtual Networking subnet range is available for floating IP addresses. 

Figure 9: Path for Nutanix VPC Traffic to the AWS Network 

Traffic exits the transit VPC through the AWS ENI link that is hosted on one of the NC2 nodes. Even though all NC2 nodes have the Flow Virtual Networking subnet connected, each VPC has only one Nutanix Flow gateway. You can host multiple Nutanix Flow VPC gateways on the same ENI. The transit gateway automatically assigns an IP address from its private network to each Nutanix user VPC. In the previous figure, the VPC named ACME is assigned 100.64.1.3, and the VPC named BOLT is assigned 100.64.1.4. 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

Nutanix Cloud Clusters on AWS 

When you configure an externally routable prefix (ERP) for a user VPC subnet in Prism Central, a route is added to the default route table in AWS that points the ERP CIDR range to the ENI. In the previous figure, the Nutanix Flow VPC gateway for ACME is on node1 with an ERP of 10.200.0.0/22. AWS traffic sees the route for all traffic destined to 10.200.0.0/22 and sends it to the ENI. Once traffic reaches the ENI, the transit gateway routes the traffic to 100.64.1.3. With this design, each node in the cluster can host multiple Nutanix Flow VPC gateways. 

In the transit-VPC page in Prism Central, you can see which NC2 bare-metal node hosts the peer-to-peer link by clicking **Associated External Subnets** in the Summary tab. 

After you deploy the cluster, you can set up a VPN gateway in AWS and create a siteto-site VPN connection. The following figure shows a high-level overview of a VPN connection for a typical NC2 on AWS deployment. If you're not using Flow Virtual Networking with routed traffic (no-NAT), you can use a standard virtual network gateway in AWS. You need the transit gateway to set a static route for routed traffic to the Nutanix user VPC. If you're using Flow Virtual Networking, you need one subnet for the baremetal node, one for Prism Central, and one for Flow Virtual Networking. 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

Nutanix Cloud Clusters on AWS 

Figure 10: VPN Connection 

NC2 needs outbound access to the NC2 console, either through an internet gateway or an on-premises VPN with outbound access. Your Nutanix cluster can sit in a private subnet that can only be accessed from your VPN, limiting exposure to your environment. Ensure that redundant paths are available for outbound internet access, as you use the NC2 console to add and remove AWS nodes based on the health of the system. 

## **Security Groups** 

You can use AWS security groups and network access control lists to secure your cluster relative to other AWS or on-premises resources. When you deploy Flow Virtual Networking, a fourth security group is deployed for Prism Central that has all the necessary rules for the Flow Virtual Networking control plane. If you plan to use Nutanix 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

Nutanix Cloud Clusters on AWS 

Disaster Recovery to protect AWS, edit this security group to allow traffic from the onpremises Prism Central instance. You can also use existing groups in your environment. 

With AWS security groups, you can limit access to the AWS CVMs, AHV host, and guest VMs to only allow traffic from your on-premises management network and CVMs. You can control replication from on-premises to AWS down to the port level, and you can easily migrate workloads because the replication software is embedded in the CVMs at both ends. AOS 6.7 and later versions support custom AWS security groups, which provide additional flexibility so that AWS security groups can apply to the VPC domain and at the cluster and subnet levels. 

Attaching the ENI to the bare-metal host applies your custom AWS security groups. You can use and reuse existing security groups across different clusters without additional scripting to maintain and support the prior custom security groups. 

The cloud network service—a distributed service that runs in the CVM and provides cloud-specific back-end support for subnet management, IP address event handling, and security group management—uses tags to evaluate which security groups to attach to the network interfaces. You can use these tags with any AWS security group, including custom security groups. The following list is arranged in dependency order: 

- Scope: VPC 

   - › Key: `tag:nutanix:clusters:external` 

   - › Value: `<none>` (leave this tag blank) 

   - › This tag can protect multiple clusters in the same VPC. 

- Scope: VPC or cluster 

   - › Key: `tag:nutanix:clusters:external:cluster-uuid` 

   - › Value: `<cluster-uuid>` 

   - › This tag protects all the guest VMs and interfaces that the CVM and AHV use. 

- Scope: VPC, cluster, network, or subnet 

   - › Key: `tag:nutanix:clusters:external:networks` 

   - › Value: `<cidr1, cidr2, cidr3>` 

   - › This tag only protects the subnets you provide. 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

Nutanix Cloud Clusters on AWS 

To apply a tag based on the subnet or CIDR, set both `external` and `cluster-uuid` for the network or subnet tag to be applied. 

The following sections provide configuration examples. 

## **Default Security Groups** 

Nutanix automatically creates three security groups to limit traffic to the cluster: 

- Internal management: Allows all internal traffic between all CVMs and all AHV hosts (EC2 bare-metal hosts) 

**Note:** Don't edit this group without approval from Nutanix Support. 

- User management: Allows users to access Prism Element and some other services running on the CVM 

- User VM (UVM in the following images): Allows guest VMs to talk to each other; doesn't allow subnet granularity 

By default, all guest VMs on all subnets can talk to each other, but you can edit the policy to lock down more traffic. You can instead use Flow Network Security to prevent east-west traffic. 

Figure 11: Default Security Group Logical Overview 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

Nutanix Cloud Clusters on AWS 

## **VPC-Level Security Groups** 

You can create a security group to protect the entire VPC (cluster 1 and cluster 2; shown by the dashed line surrounding both clusters in the following image). Use 

`tag:nutanix:clusters:external` as the key, `<none>` as the value, and `VPC` as the scope. 

Figure 12: VPC-Level Protection Logic 

## **Cluster-Level Security Groups** 

You can create a security group to protect a specific cluster (shown by the dashed line surrounding cluster 1 and its security groups and subnets in the following image). Changes to cluster-level security groups affect the subnets and guest VMs in the cluster. Create two tags: one with `tag:nutanix:clusters:external` as the key and `<none>` as the value, and one with `tag:nutanix:clusters:external:cluster-uuid` as the key and `<Cluster 1 -cluster-uuid>` as the value. Set `VPC, Cluster` as the scope for these tags. 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

Nutanix Cloud Clusters on AWS 

Figure 13: Cluster-Level Protection Logic 

## **Network-Level Security Groups** 

You can create a network-level security group that protects a single subnet (shown by the dashed line around the database subnet in the following image). Create three tags: one with `tag:nutanix:clusters:external` as the key and `<none>` as the value, one with `tag:nutanix:clusters:external:cluster-uuid` as the key and `<Cluster 1 -cluster-uuid>` as the value, and one with `tag:nutanix:clusters:external:networks` as the key and `10.72.50.0/24` as the value. Set `VPC, Cluster, Network/Subnet` as the scope for these tags. 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

Nutanix Cloud Clusters on AWS 

Figure 14: Network-Level Protection Logic 

In our example, the network-level custom security group only protects the database subnet. To cover the Nutanix Files subnet as well, add `10.73.55.0/24` to the value for the networks tag. 

## **Multicast Network Support** 

Multicast is a communication protocol for delivering a single stream of data to multiple receiving computers at the same time. This protocol is useful for financial trading applications or platforms. Most public clouds don't support multicast because the protocol produces extra traffic that can lead to performance problems, but AWS and Nutanix support multicast traffic natively with AWS Transit Gateway deployments. Many Nutanix customers use multicast traffic on-premises to manage high availability and simplify deployment of multicast applications like video streaming. 

Nutanix hosts use the Internet Group Management Protocol (IGMP) for multicast traffic. The multicast traffic routes to the subscribed guest VMs for a given multicast group based on the multicast membership table. You can add your existing NC2 deployment to an AWS Transit Gateway and associate your guest-VM subnets with an AWS 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

Nutanix Cloud Clusters on AWS 

membership group. You can have some guest VMs on the cluster in the membership group as native EC2 instances. 

For multicast traffic to work, enable IGMP snooping on AHV to allow AHV to send multicast traffic to the subscribed guest VMs. If you disable IGMP snooping, AHV floods multicast traffic to all guest VMs in the subnet. You can only enable or disable IGMP snooping for guest VMs attached to the default virtual switch; you can't enable or disable it for specific AHV or AWS subnets. Multicast traffic is only supported for guest VM subnets and not for CVM (cluster) subnets. 

You can forward multicast traffic to a guest VM even after it migrates from one AHV host to another. When AHV doesn't have a local VM subscribed to a multicast group, it doesn't receive multicast traffic specific to that multicast group from the AWS Transit Gateway. In the following image, the AWS Transit Gateway is configured on Subnet X. 

Figure 15: Multicast Network Configuration 

The following table describes how multicast traffic is routed for a combination of senders and receivers when you enable or turn off IGMP snooping. 

_Table: Multicast Traffic Routing_ 

|**Multicast Sender**|**Multicast Receivers**|**AOS IGMP Snooping**|**Multicast Traffic**|
|---|---|---|---|
|||**Status**|**Status**|
|EC2|UVM1, UVM2, UVM4|Enabled|UVM1, UVM2, and|
||||UVM4 receive traffic|
||||from EC2.|



© 2026 Nutanix, Inc. All rights reserved  | **27** 

Nutanix Cloud Clusters on AWS 

|**Multicast Sender**|**Multicast Receivers**|**AOS IGMP Snooping**|**Multicast Traffic**|
|---|---|---|---|
|||**Status**|**Status**|
|EC2|UVM1, UVM2, UVM4|Disabled|All guest VMs on|
||||AHV1 that share the|
||||subnet with AHV1|
||||or AHV2 and UVM4|
||||(UVM1 to UVM6)|
||||receive traffic from|
||||EC2.|
|UVM8|UVM1, UVM2, UVM4|Enabled|UVM1, UVM2, and|
||||UVM4 receive traffic|
||||from UVM8.|
|UVM8|UVM1, UVM2, UVM4|Disabled|All guest VMs on|
||||AHV1 that share|
||||the subnet with|
||||AHV1 or AHV2 and|
||||UVM4 (UVM1 to|
||||UVM6) receive traffic|
||||from EC2. UVM9|
||||also receives traffic|
||||because it shares a|
||||subnet with UVM8 on|
||||AHV3.|
|UVM8/EC2|None|Enabled or Disabled|No guest VM or EC2|
||||instance receives|
||||traffic.|
|UVM7|UVM1, UVM2, UVM4|Enabled|UVM1, UVM2, and|
||||UVM4 receive traffic|
||||from UVM7.|



© 2026 Nutanix, Inc. All rights reserved  | **28** 

Nutanix Cloud Clusters on AWS 

|**Multicast Sender**|**Multicast Receivers**|**AOS IGMP Snooping**|**Multicast Traffic**|
|---|---|---|---|
|||**Status**|**Status**|
|UVM7|UVM1, UVM2, UVM4|Disabled|All guest VMs on|
||||AHV1 that share the|
||||subnet with AHV1|
||||or AHV2 and UVM4|
||||(UVM1 to UVM6) and|
||||guest VMs that share|
||||a subnet with UVM7|
||||on AHV3 receive|
||||traffic from UVM7. In|
||||this case, no guest VM|
||||is available on AHV3.|
|UVM8|EC2 instance on|Enabled|The EC2 instance on|
||Subnet X||Subnet X receives|
||||traffic.|
|UVM8|EC2 instance on|Disabled|The EC2 instance on|
||Subnet X||Subnet X receives|
||||traffic. UVM9 also|
||||receives traffic|
||||because it shares the|
||||subnet with UVM8 on|
||||AHV3.|



© 2026 Nutanix, Inc. All rights reserved  | **29** 

Nutanix Cloud Clusters on AWS 

## 4. Migration to Nutanix Cloud Clusters on Amazon Web Services 

Move your applications to AWS for consolidation, bursting, or to have them on a cloudbased service. Once you configure networking from AWS to on-premises, you can choose any proven method for moving applications to an AHV-based cluster, which saves time and money. The following methods are the most common ways to migrate data to NC2 on AWS: 

## **Native data protection** 

You can use this method for ESXi- and AHV-based clusters. Creating a remote site for your new NC2 on AWS deployment and setting up the native networking integration only takes a few minutes; just ensure that the ports are open on the management security group that you need for replication. All the existing data protection best practices apply because a bare-metal AWS deployment essentially acts as an additional supported original equipment manufacturer (OEM). 

## **Nutanix Disaster Recovery** 

To take advantage of protection policies and recovery plans to protect applications across multiple Nutanix clusters, set up Nutanix Disaster Recovery from Prism Central by selecting the checkbox. Whether you're doing disaster recovery or migrations, Nutanix Disaster Recovery stages your applications to be restored in the right order. You can also use the protection policies to quickly revert to onpremises if desired. 

## **Nutanix Move** 

Nutanix Move is a cross-hypervisor migration solution that migrates VMs with minimal downtime. Nutanix Move supports three migration types: VMs running on ESXi managed by vCenter, Amazon EC2 instances backed by Elastic Block Storage (EBS) running on AWS, and VMs running on Hyper-V. Nutanix Move also supports migrating AWS EC2 VMs to AHV on the Nutanix cluster, though this use case is minimal. 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

Nutanix Cloud Clusters on AWS 

## **AHV-based backups** 

You can use any third-party backup product to restore applications to NC2 on AWS, which is important when you need to migrate or do testing and development work. 

© 2026 Nutanix, Inc. All rights reserved  | **31** 

Nutanix Cloud Clusters on AWS 

## 5. Storage for Nutanix Cloud Clusters on Amazon Web Services 

The primary storage for NC2 comes from the locally attached NVMe disks. The locally attached disks are the first tier that the CVMs use to persist the data. Each AWS node also consumes two AWS Nitro EBS volumes that are attached to the bare-metal node. One of those EBS volumes is used for AHV and the other for the CVM. When you provision the NC2 instance in the NC2 console, you can add more Nitro EBS volumes to each bare-metal node in the cluster as remote storage, scaling your storage to meet business needs without adding more bare-metal nodes. If you initially create the cluster with Nitro EBS volumes, you can add more Nitro EBS volumes later. 

© 2026 Nutanix, Inc. All rights reserved  | **32** 

Nutanix Cloud Clusters on AWS 

Figure 16: Node Storage Configuration 

When you add EBS volumes to a cluster, they're added in a uniform manner throughout the cluster. The total amount of storage follows the same AOS limitations (Nutanix portal credentials required) in terms of the maximum amount storage that can be added, with the additional constraint that the maximum storage can't be more than 20 percent of the local storage for the bare-metal node. If you use the I4i.metal instance, which has 30 TB of local AWS Nitro NVMe-based storage, you can reach the current AOS node limit with the other 80 percent when using snapshots. 

Through the NC2 console, you can add a small amount of EBS storage during the initial deployment and add more later. If you have a disaster recovery use case with NC2, you can deploy a three-node cluster with a small amount of EBS storage to cover the storage usage for tier-1 workloads that need fast recovery. If your needs change, you can scale 

© 2026 Nutanix, Inc. All rights reserved  | **33** 

Nutanix Cloud Clusters on AWS 

up the storage later. At failover, you can use Prism Central playbooks or the NC2 console to add NC2 nodes to cover any RAM usage not supplied by the three bare-metal nodes. 

Nutanix recommends keeping the minimum number of additional nodes greater than or equal to your cluster's redundancy factor. Expand the cluster in multiples of your redundancy factor for the additional nodes. 

**Note:** If you use EBS volumes for storage, only use homogenous clusters (all the nodes must be the same type). 

The data tiering process for NC2 on AWS is the same as the process for a hybrid configuration on-premises. The EBS storage forms the Cloud-SSD Tier (shown in the Tier column in Prism Element) and uses the rules listed in the Storage Tiering and Prioritization section of the Nutanix Bible. 

## **Storage Availability** 

The following table highlights the minimum number of racks required in your cluster to withstand a given number of rack failures using rack awareness. Nutanix Erasure Coding (EC-X) is one of the storage reduction technologies available in AOS. EC-X takes one or two data copies and calculates a parity bit you can use to recreate the data if required. 

_Table: Desired Fault Tolerance and Required Nodes for Rack Awareness_ 

|**Fault Tolerance**|**EC-X Enabled**|**Minimum Units**|**Simultaneous**|
|---|---|---|---|
|**Level**||**in the Cluster**|**Failure Tolerance**|
|1|No|3 racks|1 rack|
|1|Yes|4 racks|1 rack|
|2|No|5 racks|2 racks|
|2|Yes|6 racks|2 racks|



For more information, see the Placement Policy section in the Nutanix Bible Book of Nutanix Cloud Clusters: Nutanix Cloud Clusters on AWS. 

## **Respond to Failures** 

AOS storage withstands a variety of hardware failures and builds strong redundancy into the software stack. Nutanix software processes that encounter serious errors fail 

© 2026 Nutanix, Inc. All rights reserved  | **34** 

Nutanix Cloud Clusters on AWS 

fast to quickly restart normal operations instead of waiting for a potentially faulty process to complete. Because Nutanix storage continuously monitors components, it can stop and restart them when an error occurs to recover as quickly as possible, rather than letting them remain unresponsive. Each host relies on its local CVM to service all storage requests. AOS storage continuously monitors the health of all CVMs in the cluster. If an unrecoverable error occurs on a particular CVM, Nutanix autopathing automatically reroutes requests from the host to a healthy CVM on another node, providing data path redundancy. This redirection continues until the local CVM failure issue is resolved. 

Because the cluster has a global namespace and access to replicas for all the data on that node, it can service requests immediately. This ability provides a high degree of fault tolerance and failover for all VMs in a Nutanix cluster. If the node's CVM continues to be unavailable for a prolonged period, data automatically replicates to maintain the necessary replication factor. 

Figure 17: Data Path Redundancy 

## **Prevent Network Partition Errors** 

Nutanix uses the Paxos algorithm to avoid split-brain scenarios. Paxos is a proven protocol for reaching a consensus or quorum among several participants in a distributed system. Before the operation writes any file system metadata to Cassandra, Paxos verifies that all nodes in the system agree on the value. If the nodes don't reach a quorum, the operation fails, preventing any potential corruption or data inconsistency. This design protects against events like network partitioning, where communication between nodes fails or packets become corrupt, leading to a 

© 2026 Nutanix, Inc. All rights reserved  | **35** 

Nutanix Cloud Clusters on AWS 

scenario where nodes disagree on values. AOS storage also uses timestamps so that updates are applied in the proper order. 

## **Proactively Resolve Bad Disk Resources** 

AOS storage incorporates a Curator process that performs background housekeeping tasks to keep the entire cluster running smoothly. Curator promotes file system metadata consistency and checks the extent store for corrupt and underreplicated data. Curator scans extents in successive passes, computes each extent's checksum, and compares it with the metadata checksum to validate consistency. If the checksums don't match, the corrupted extent is replaced with a valid extent from another node. This proactive data analysis protects against data loss and identifies bad sectors that you can use to detect disks that are about to fail. 

## **Maintain Availability: Disk Failure** 

The Nutanix unified component Stargate receives and processes data. All read and write requests for a node are sent to the Stargate process on that node. The Hades service simplifies the break-fix procedures for disks and automates several tasks that previously required manual user actions. Hades helps fix failing devices before they become unrecoverable. 

If Stargate sees delays in responses to I/O requests to a disk, it marks the disk offline. Hades then automatically removes the disk from the data path and runs smartctl checks against it. If the checks pass, Hades marks the disk online and returns it to service. If the checks fail or if Stargate marks a disk offline three times in one hour (regardless of the smartctl check results), Hades automatically starts the Amazon Elastic Compute Cloud (EC2) disk removal process. Removing the disk triggers an API call to the cluster portal, which notifies the NC2 console. The cluster software automatically replicates the data on the bad disk to other drives throughout the cluster. The user must then start the node removal process from the NC2 console to maximize the available storage. 

When a node uses EBS storage for additional capacity, node replacement isn't triggered if one of the EBS volumes goes bad. Instead, Stargate marks the EBS volume offline, the cluster software replicates the data, and the NC2 console adds a new EBS volume to the node. 

© 2026 Nutanix, Inc. All rights reserved  | **36** 

Nutanix Cloud Clusters on AWS 

## **Maintain Availability: Availability Zone Failure** 

AZs can go offline for a variety of reasons—issues with power, cooling, or networking as well as scheduled system maintenance. To avoid downtime in AWS, protect your workloads with Nutanix Disaster Recovery. The destination for Nutanix Disaster Recovery can be another on-premises cluster or another NC2 on AWS instance in a different AZ. 

© 2026 Nutanix, Inc. All rights reserved  | **37** 

Nutanix Cloud Clusters on AWS 

## 6. Hibernate and Resume with Nutanix Cloud Clusters on Amazon Web Services 

NC2 on AWS can preserve customer data while bare-metal nodes are turned off. Hibernation can save you money when the cluster isn't in use—for example, in test or development environments that aren't used on the weekends. 

NC2 on AWS performs the following steps during the hibernation process: 

**1.** NC2 on AWS verifies that no VMs, upgrades, or other workflows (such as cluster expansion) are running on the cluster. 

**2.** NC2 on AWS creates and adds at least one S3 disk per node or CVM in the cluster. 

The S3 disks form a cloud storage tier used by the same internal process that tiers data between SSD and HDD for on-premises clusters. 

**3.** NC2 on AWS puts all hosts into maintenance mode to prevent guest VMs from starting and stopping all I/O operations. 

   - **a.** Curator and Stargate migrate extent store data to the cloud, maintaining replication factor 1 (a single copy). 

   - **b.** Curator and Stargate migrate the Cassandra metadata. 

**4.** NC2 on AWS snapshots the respective EBS volumes to protect the cluster configuration and state information maintained on the CVM boot disks during hibernation. 

**5.** NC2 on AWS releases the bare-metal nodes back to the AWS pool. 

If your cluster is hibernating, you can bring the clusters back online with the resume operation. Resume deploys the nodes you need into the original AWS Region and migrates your data back to them. 

NC2 on AWS performs the following steps during the resume process: 

**1.** NC2 on AWS deploys new EC2 instances and attaches cloud disks to them to form the AWS Nutanix cluster. 

**2.** NC2 on AWS restores the cluster boot disk's Zeus configuration from the EBS volume snapshot. 

© 2026 Nutanix, Inc. All rights reserved  | **38** 

Nutanix Cloud Clusters on AWS 

**3.** NC2 on AWS restores the cluster configuration from the EBS volume snapshot. 

**4.** Genesis starts the necessary cluster services. 

**5.** The Cassandra dynamic ring changer restores metadata. 

**6.** NC2 on AWS creates and adds at least one S3 disk per node or CVM in the cluster. 

The S3 disks form a cloud storage tier. 

**7.** Curator restarts and registers the kSelectiveClusterHibernate scan for the restore operation and schedules the selective hibernate scan to migrate extent store data from S3 to NVMe. 

**8.** Genesis transitions to kNormal mode after all the data is restored. 

**9.** NC2 on AWS marks the hibernated disks as to-remove and removes them from the cluster configuration 

## **Native Backup with Nutanix Cluster Protection** 

Applications in the cloud require the same day-two operations as on-premises applications. Nutanix cluster protection provides a native option for backing up data, including user data and Prism Central configuration data, from NC2 instances running on AWS to S3 buckets. Cluster protection backs up all user-created VMs and volume groups on the cluster. 

Nutanix already provides native protection for localized failures at the node and rack level, and cluster protection extends that protection to the cluster's AZ. Because this service is integrated, high-performance applications are barely affected, as the backup process uses native AOS snapshots to send the backup copy directly to S3. 

Two Nutanix services help protect the cluster: 

- Prism Central Disaster Recovery backs up the Prism Central data to a new S3 bucket that you supply, rather than to an AOS container. 

- Nutanix Multicloud Snapshot Technology (NMST) replicates native Nutanix AOS snapshots to object storage in a second new S3 bucket that you provide in AWS. 

The Cloud Snapshot Engine runs on a Prism Element instance and the Prism Central instance in AWS. 

The following high-level process describes how to protect your clusters and Prism Central in AWS: 

© 2026 Nutanix, Inc. All rights reserved  | **39** 

Nutanix Cloud Clusters on AWS 

**1.** Deploy a one- or three-node Prism Central instance in AWS. 

**2.** Create two S3 buckets: one for Prism Central and one for your cloud clusters. 

**3.** Enable Prism Central protection. 

**4.** Deploy NMST. 

**5.** Protect your AWS cloud clusters. 

The system takes Prism Central and AOS snapshots every hour and retains up to two snapshots in S3. A Nutanix Disaster Recovery category protects all the user-created VMs and volume groups on the clusters. A service named clustermgmt-nc2 watches for create and delete events and assigns them a cluster protection category. 

Figure 18: NC2 on AWS Backup to S3 Logical Overview 

The following high-level process describes how to recover your Prism Central instances and clusters on AWS: 

© 2026 Nutanix, Inc. All rights reserved  | **40** 

Nutanix Cloud Clusters on AWS 

**1.** The NC2 console automatically deploys a new NC2 instance during the recovery process. 

**2.** Add your Prism Central subnet and any guest VM networks to your recreated cloud cluster. 

**3.** Recover your Prism Central configuration from the S3 bucket. 

**4.** Register your Prism Central instance with the recovered cluster. 

**5.** Recover NMST. 

**6.** Create a recovery plan. 

**7.** Run the recovery plan from Prism Central. 

After you recover the NMST, you can restore your Prism Central instances and clusters on AWS using the recovery plan in Prism Central, which has all the VMs you need to restore. By using Nutanix Disaster Recovery with this service, you can easily recover when disaster strikes. 

For more information on protecting your data with Nutanix, see the Hybrid Cloud: AOS 6.5 with AHV Disaster Recovery to Nutanix Cloud Clusters on AWS Design and the Data Protection and Disaster Recovery best practice guide. 

## **Nutanix Disaster Recovery to S3** 

With NMST, you can send AOS snapshots from any Nutanix-based cluster to S3 and Objects Storage. This feature allows you to offload snapshots that you don’t regularly access or applications that have higher recovery time objectives (RTOs) to optimize performance and capacity in the primary storage infrastructure. You can then recover workloads using zero-compute (on-demand deployment of a Nutanix cluster on-premises or in AWS) or pilot-light (on-demand expansion of NC2 deployment) deployment models, based on your needs. 

Moving data between the cluster in AWS and S3 occurs in four main stages: metadata migration, data migration, AOS processing, and NC2 console processing. We tested throughput on a three-node i3.metal cluster with VMs consuming 18 virtual disks (vDisks) with a total usable capacity of 9.67 TB and 4.74 GB of metadata. The following list provides the test details and shows the timing results for all phases: 

© 2026 Nutanix, Inc. All rights reserved  | **41** 

Nutanix Cloud Clusters on AWS 

- Test details: 

   - › Three-node cluster i3.metal 

   - › 18 vDisks with 250 GB each 

   - › Total capacity consumed by the VMs = 9.74 TB 

   - › Total metadata size on cluster = 4.74 GB 

- Metadata migration phase: 34 minutes 

- Data migration phase: 59 minutes 

- Total AOS processing time: 1 hour, 38 minutes 

- Total NC2 console processing time: 1 hour, 50 minutes 

## **Zero-Compute Deployment** 

Zero compute is an on-demand Nutanix cluster deployment model where the NMST service runs alongside the protected workloads. With this model, you can replicate snapshots directly to an S3 bucket or to Nutanix Objects for storing and recovering less critical workloads, which can accommodate longer RTOs. In the event of a failover, you can quickly use the replicated snapshot to deploy an on-demand Nutanix cluster onpremises or in AWS for recovery. 

Figure 19: Zero-Compute Snapshot Storage with Nutanix Multicloud Snapshot Technology 

© 2026 Nutanix, Inc. All rights reserved  | **42** 

Nutanix Cloud Clusters on AWS 

After you deploy the new Nutanix cluster, redeploy Prism Central and the NMST service. The existing state of NMST restores from the S3 or Nutanix Objects bucket. You can then use your snapshots in Prism Central to restore your VMs. 

## **Pilot-Light Deployment** 

Pilot-light deployment occurs when the NMST service runs on an NC2 deployment with at least three nodes. NMST redirects the AOS snapshots to S3. You can recover snapshots to the NC2 deployment or back to the primary site if a healthy Nutanix cluster is available. If you need additional space to recover the S3-based snapshots to the NC2 deployment, you can add more nodes to the cluster using the NC2 console or the NC2 console API. 

Figure 20: Pilot Cluster Snapshot Storage with Nutanix Multicloud Snapshot Technology 

This model supports fast RTOs for tier-1 applications. It sends snapshots directly to the pilot cluster for quick restores and uses S3 to store snapshots for tier-2 applications. Combined with EBS as additional storage for tier-1 applications, this model can drastically reduce business costs while meeting requirements. 

© 2026 Nutanix, Inc. All rights reserved  | **43** 

Nutanix Cloud Clusters on AWS 

## 7. Nutanix Cloud Clusters on Amazon Web Services Deployment Models 

Because environments can vary greatly, we provide several example deployment models. Regardless of the model you use, deploying a Nutanix cluster in AWS has a few specific requirements in addition to the existing requirements that on-premises clusters use for support services. For more information on the ports and endpoints needed for a successful deployment, see the tables in Ports and Endpoints Requirements. 

**Note:** Many of the destinations use DNS failover and load balancing. For this reason, the IP address returned when resolving a specific domain might change rapidly. We can't provide specific IP addresses in place of domain names. 

The Ports and Protocols guide lists general firewall support requirements, with additional focus in the Disaster Recovery (formerly Leap) section. 

## **Single-Cluster Deployment with Native Networking** 

Most customers start with a single-cluster deployment with native networking. Native networking provides full access to all AWS features. 

© 2026 Nutanix, Inc. All rights reserved  | **44** 

Nutanix Cloud Clusters on AWS 

Figure 21: Single-Cluster Deployment with Native Networking 

The numbers in the following list correlate to numbered parts of the previous image: 

**1.** You can create connectivity between an on-premises datacenter and AWS with Direct Connect (private or transit virtual interfaces (VIF)) or a site-to-site VPN. 

   - NC2 doesn’t require any special configuration for connectivity apart from the firewall configurations provided in the documentation. 

**2.** AWS bare-metal nodes are automatically provisioned and imaged when you create a new cluster through the NC2 console. 

The minimum subnet mask for the bare-metal node subnet is /25. 

**3.** You can use an existing customer-created AWS VPC to deploy NC2 on AWS. You can also create a new VPC with the NC2 deployment through the NC2 console. 

**4.** Two or more NC2 deployments can share a user-created AWS VPC but they must be in separate subnets. 

Two NC2 deployments cannot share an AWS VPC created through the NC2 console. AWS subnets can’t overlap between NC2 deployments. 

**5.** You can deploy Nutanix Move with an interface in the Prism AWS subnet. You can also use other subnets. 

© 2026 Nutanix, Inc. All rights reserved  | **45** 

Nutanix Cloud Clusters on AWS 

**6.** You can deploy Prism Central in single or scaled-out configurations and link it with onpremises and NC2 deployments elsewhere in AWS. 

**7.** You can use many AWS-native features in conjunction with workloads running on NC2 on AWS. 

**8.** The cluster deployment process automatically creates AWS security groups. 

   - **a.** Deploying a cluster with AWS native networking adds the Internal management, User management, and UVM security groups. 

The UVM security group is applied to the peer-to-peer link network. 

   - **b.** Deploying Flow Virtual Networking creates additional security groups for Prism Central. 

**9.** Use AWS tags to apply a custom AWS security group to each VPC, individual cluster, or subnet level to provide more granular permissions. 

For even more control, including microsegmentation, use Flow Virtual Networking with overlay networks and Flow Network Security. 

## **Single-Cluster Deployment with Nutanix Flow Virtual Networking** 

Deploying a cluster with Flow Virtual Networking adds consistent network operations across a hybrid multicloud and more routing and networking features, including overlay networking, layer 2 extension, and VPN connectivity, for Nutanix customers. 

© 2026 Nutanix, Inc. All rights reserved  | **46** 

Nutanix Cloud Clusters on AWS 

Figure 22: Single-Cluster Deployment with Nutanix Flow Virtual Networking 

The numbers in the following list correlate to numbered parts of the previous image: 

**1.** You can create connectivity between an on-premises datacenter and AWS with Direct Connect (private or transit virtual interfaces) or a site-to-site VPN. 

      - NC2 doesn’t require any special configuration for connectivity apart from static routes for no-NAT networks and the firewall configurations provided in the documentation. 

**2.** You can use an existing customer-created AWS VPC to deploy NC2 on AWS. You can also create a new VPC with the NC2 deployment through the NC2 console. 

   - **a.** Two or more NC2 deployments can share a user-created AWS VPC but they must be in separate subnets. 

   - **b.** Two NC2 deployments cannot share an AWS VPC created through the multicloud management portal. 

   - **c.** AWS subnets can’t overlap between NC2 deployments. 

**3.** You can extend on-premises networks to the cloud to provide consistent network connectivity for user VMs during a migration. You can extend Nutanix clusters and use third-party physical or virtual extensible LAN appliances. 

© 2026 Nutanix, Inc. All rights reserved  | **47** 

Nutanix Cloud Clusters on AWS 

**4.** AWS bare-metal nodes are automatically provisioned and imaged when you create a new cluster through the NC2 console. 

   - The minimum subnet mask for the bare-metal node subnet is /25. 

**5.** You can deploy Prism Central in single or scaled-out configurations and link it with on-premises and NC2 deployments elsewhere in AWS. You can deploy Nutanix Move with an interface in the Prism AWS subnet. You can also use other subnets, including overlay subnets, as long as Nutanix Move has an IP address on an AWSnative subnet. 

**6.** Nutanix Flow Virtual Networking has a broad range of functions, including overlay networking, layer 2 extension, and VPN connectivity. 

**7.** When you create an NC2 deployment that includes Flow Virtual Networking, Flow Virtual Networking overlay NAT networks are enabled by default. 

   - North-south user VM traffic goes through the NAT network but the overlay network itself is only visible through Prism Central, not on the AWS console. 

**8.** You can also create no-NAT networks, which adds the CIDR ranges used by noNAT networks to native AWS route tables as an externally routable prefix (ERP) and provides access to services hosted on user VMs from outside the NC2 environment. 

   - You must have an AWS transit gateway to set up a no-NAT network. 

**9.** Entities with an ENI in the NC2 VPC can access user VMs on a no-NAT network directly. Anything outside the NC2 VPC must go through a transit gateway. 

**10.** You can use many AWS-native features in conjunction with workloads running on NC2 on AWS. 

## **Disaster Recovery Across Multiple Availability Zones** 

To protect your NC2 on AWS cluster in the event of an AZ failure, use another NC2 instance or your existing on-premises instance as a disaster recovery target. You have many options when it comes to Nutanix disaster recovery; for more information, see the Multicluster Deployment section under NC2 on AWS Deployment Models in the Nutanix Cloud Clusters on AWS Deployment and User Guide. 

© 2026 Nutanix, Inc. All rights reserved  | **48** 

Nutanix Cloud Clusters on AWS 

Figure 23: Disaster Recovery Across Multiple Availability Zones 

The numbers in the following list correlate to numbered parts of the previous image: 

**1.** You can create connectivity between an on-premises datacenter and AWS with Direct Connect (private or transit virtual interfaces) or a site-to-site VPN. 

   - NC2 doesn’t require any special configuration for connectivity apart from the firewall configurations provided in the documentation. 

**2.** Two or more NC2 deployments can share the same AWS VPC as long as the AWS VPC wasn’t created through the NC2 console. 

AWS subnets can’t overlap between NC2 deployments. 

© 2026 Nutanix, Inc. All rights reserved  | **49** 

Nutanix Cloud Clusters on AWS 

**3.** Nutanix clusters can perform intercluster disaster recovery. In each cluster, you can set Prism Central as a disaster recovery failover domain and link it with Prism Central instances in other clusters. 

   - **a.** NC2 supports asynchronous, near-synchronous (NearSync), and synchronous replication. 

   - **b.** You can set your recovery point objective (RPO) to be one hour with asynchronous replications. 

   - **c.** NearSync and asynchronous replication are both valid options if you run AHV onpremises. You can set your RPO to be as little as one minute with NearSync and one hour with asynchronous replication. 

   - **d.** To use NearSync, your on-premises cluster must meet the requirements listed in the Requirements of Data Protection with NearSync Replication section of the Data Protection and Recovery with Prism Element guide. 

**4.** NC2 on AWS offers multiple disaster recovery patterns. For point 3, the main cluster is on-premises and NC2 on AWS is used as the disaster recovery location. 

**5.** Disaster recovery with multisnapshot technology can help lower costs by replicating VMs with low RTO into a small pilot-light NC2 deployment and VMs with a higher RTO into S3. 

   - **a.** You can restore the VMs saved into S3 to NC2 in case of a disaster on-premises and scale the NC2 deployment up to handle the additional VMs. 

   - **b.** After the on-premises environment is back to normal, restore the VMs back to onpremises and shrink the NC2 deployment back to its original pilot-light size. 

**6.** You can only deploy an NC2 deployment into a single AWS AZ, but for disaster recovery purposes, you can deploy two clusters, each in a different AZ. Then, you can link these clusters and configure a disaster recovery plan to sync data between the AWS AZs. 

   - **a.** You can also replicate data to an NC2 deployment in another AWS region. 

   - **b.** Keep latency requirements in mind when planning a cross-region disaster recovery setup. For example, we recommend an RTT of less than 5 ms for synchronous replication (0 second RPO). By comparison, Nearsync replication has no minimum latency requirement (1–15 minute RPO). 

© 2026 Nutanix, Inc. All rights reserved  | **50** 

Nutanix Cloud Clusters on AWS 

## 8. Nutanix Cloud Infrastructure Capacity Optimization 

Nutanix Cloud Infrastructure (NCI) software offers capacity optimization features that improve storage utilization and performance. The two key features are compression and deduplication. 

Nutanix systems currently offer the following compression options: 

- Inline: The system compresses data synchronously as it is written to optimize capacity and to maintain high performance for sequential I/O operations. Inline compression only compresses sequential I/O to avoid degrading performance for random write I/O. 

- Post-process: For random workloads, data writes to the SSD tier uncompressed for high performance. Compression occurs after cold data migrates to lower-performance storage tiers. Post-process compression acts only when data and compute resources are available, so it doesn't affect normal I/O operations. 

The software-driven Elastic Deduplication Engine increases the effective capacity in the disk tier and the utilization of the performance tiers (RAM and flash) by eliminating duplicate data. By providing larger effective cache sizes in the performance tier, this feature can substantially increase performance for certain workloads. 

Deduplication savings vary greatly depending on workload and data types. In general, deduplication provides the largest benefit for common data sets, such as full-clone VDI workloads. Nutanix doesn't recommend deduplication for general-purpose server workloads, including business-critical applications. 

For containers hosting business-critical applications, VDI, general server workloads, and big data, we recommend disabling deduplication for all except full-clone VDI VMs. Increase CVM memory to at least 24 GB. 

Carefully consider the advantages and disadvantages of compression and deduplication for their specific applications. For more information on capacity optimization strategies, see the Nutanix Data Efficiency tech note. 

© 2026 Nutanix, Inc. All rights reserved  | **51** 

Nutanix Cloud Clusters on AWS 

## 9. Encryption for Nutanix Cloud Clusters on Amazon Web Services 

To help reduce cost and complexity, Nutanix supports a native local key manager (LKM) for all clusters with three or more nodes. The LKM runs as a service distributed among all the nodes. You can activate it from Prism Element to enable encryption without adding another silo to manage. 

Organizations often purchase external key managers (EKMs) separately for both software and hardware. However, because the Nutanix LKM runs natively in the CVM, it's highly available and has no variable add-on pricing based on the number of nodes. Every time you add a node you know the final cost. You also gain peace of mind because when you upgrade your cluster, the key management services are also upgraded. When upgrading the infrastructure and management services in lockstep, you're ensuring your security posture and availability by staying in line with the support matrix. 

Nutanix software encryption provides native AES-256 data-at-rest encryption, which can interact with any KMIP- or TCG-compliant external key management service (KMS) server (such as Vormetric or SafeNet) and the native Nutanix KMS. The system uses Intel AES-NI acceleration for encryption and decryption processes to minimize any potential performance impacts. 

We recommend using the native Nutanix KMS to provide additional security for your workloads in the cloud. 

**Note:** The first copy of the data (written locally) is encrypted. The copy sent over the wire is also encrypted and stored on a remote node. 

© 2026 Nutanix, Inc. All rights reserved  | **52** 

Nutanix Cloud Clusters on AWS 

## 10. Virtual Machine High Availability 

VM high availability restarts VMs on another AHV host in the cluster if a host fails. VM high availability considers RAM when calculating available resources throughout the cluster for starting VMs. 

VM high availability respects affinity and antiaffinity rules. For example, with VM-host affinity rules, VM high availability doesn't start a VM pinned to AHV host 1 and host 2 on another host when those two are down unless the affinity rule specifies an alternate host. 

AHV has two VM high availability modes: 

## **Default** 

This mode requires no configuration and is included by default when you deploy an AHV-based Nutanix cluster. When an AHV host becomes unavailable, the VMs that ran on the failed AHV host restart on the remaining hosts, depending on the available resources. If the remaining hosts don't have sufficient resources, some of the failed VMs might not restart. 

## **Guarantee** 

This nondefault configuration reserves space throughout the AHV hosts in the cluster to guarantee that all VMs can restart on other hosts in the AHV cluster during a host failure. To enable Guarantee mode, select the **Enable HA Reservation** checkbox in the Manage VM High Availability dialog. A message then displays the amount of RAM reserved and how many AHV host failures the system can tolerate. 

The VM high availability configuration reserves resources to protect against the following: 

- One AHV host failure, if you configure all Nutanix containers with replication factor 2 

- Two AHV host failures, if you configure any Nutanix container with replication factor 3 

You can use the aCLI to manage protection against two AHV host failures when using replication factor 3. Use the following command to designate the maximum number of tolerable AHV host failures: 

```
$ nutanix@CVM$ acli ha.update num_host_failures_to_tolerate=X
```

© 2026 Nutanix, Inc. All rights reserved  | **53** 

Nutanix Cloud Clusters on AWS 

When an unavailable AHV host comes back online after a VM high availability event, VMs previously running on that host migrate back to maintain data locality. 

The actual quantity of reserved resources depends on the current load of the cluster and is typically 1 to 1.25 times the resources used on the most-loaded host. For more information on reservations, see VM High Availability in Acropolis. 

## **VM High Availability Recommendations and Requirements** 

- Use the nondefault VM high availability Guarantee mode when all VMs must be able to restart if an AHV host fails. 

- When using Guarantee mode, keep the default reservation type of **kAcropolisHAReserveSegments** ; do not alter this setting. 

**Note:** The VM high availability reservation type kAcropolisHAReserveHosts is deprecated. Never change the VM high availability reservation type to kAcropolisHAReserveHosts. 

- Consider storage availability requirements when using VM high availability Guarantee mode. 

   - › Ensure that the value of the parameter `num_host_failures_to_tolerate` is less than the configured storage availability. 

   - › If you only have two copies of the VM data, the VM data might be unavailable if two hosts are down at the same time although the system has enough CPU and RAM resources to run the VMs. 

- You must disable VM high availability before you can use the Acropolis Dynamic Scheduler (ADS) VM-host affinity feature to pin a VM to one AHV host. 

**Note:** We don't recommend pinning VMs to a particular AHV host, as described in the following section. 

© 2026 Nutanix, Inc. All rights reserved  | **54** 

Nutanix Cloud Clusters on AWS 

## 11. Acropolis Dynamic Scheduler 

Acropolis Dynamic Scheduler (ADS) manages compute (CPU and memory) and storage resource availability for VMs and volume groups. You can also use ADS to define affinity policies. 

You can define VM-host affinity or VM-VM antiaffinity policies manually (if you're a Nutanix administrator) or with a VM-provisioning workflow. 

## **VM-host affinity** 

This configuration keeps a VM on a specific set of AHV hosts. Use it when you need to limit VMs to a subset of available AHV hosts because of application licensing, host resources (such as available CPU cores or CPU gigahertz speed), available RAM or RAM speed, or local SSD capacity. Host affinity is a **must** rule: AHV always honors the specified rule. 

**Note:** We don't recommend using VM-host affinity. 

## **VM-VM antiaffinity** 

This configuration prevents two or more VMs from running on the same AHV host. Use it when an application provides high availability and an AHV host must not be the application's single point of failure. Antiaffinity is a **should** rule: AHV honors it only when it has enough resources available to run VMs on separate hosts. 

For additional information about ADS and affinity policies, see the ADS section of the Nutanix AHV best practice guide. 

© 2026 Nutanix, Inc. All rights reserved  | **55** 

Nutanix Cloud Clusters on AWS 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **56** 

Nutanix Cloud Clusters on AWS 

## **List of Figures** 

Figure 1: Overview of the Nutanix Hybrid Multicloud Software..................................................................... 5 Figure 2: Nutanix Cloud Clusters Management.............................................................................................9 Figure 3: Nutanix Cloud Clusters Console...................................................................................................10 Figure 4: OVS Conceptual Architecture.......................................................................................................12 Figure 5: One-to-One Nutanix AHV and AWS Subnet Mapping..................................................................13 Figure 6: One-to-One and One-to-Many Nutanix AHV and AWS Subnet Mapping.....................................14 Figure 7: Overview of NC2 on AWS............................................................................................................16 Figure 8: Flow Virtual Networking Traffic on AWS.......................................................................................18 Figure 9: Path for Nutanix VPC Traffic to the AWS Network.......................................................................19 Figure 10: VPN Connection......................................................................................................................... 21 Figure 11: Default Security Group Logical Overview...................................................................................23 Figure 12: VPC-Level Protection Logic........................................................................................................24 Figure 13: Cluster-Level Protection Logic....................................................................................................25 Figure 14: Network-Level Protection Logic.................................................................................................. 26 Figure 15: Multicast Network Configuration................................................................................................. 27 Figure 16: Node Storage Configuration....................................................................................................... 33 Figure 17: Data Path Redundancy...............................................................................................................35 Figure 18: NC2 on AWS Backup to S3 Logical Overview...........................................................................40 Figure 19: Zero-Compute Snapshot Storage with Nutanix Multicloud Snapshot Technology...................... 42 Figure 20: Pilot Cluster Snapshot Storage with Nutanix Multicloud Snapshot Technology..........................43 Figure 21: Single-Cluster Deployment with Native Networking................................................................... 45 Figure 22: Single-Cluster Deployment with Nutanix Flow Virtual Networking..............................................47 Figure 23: Disaster Recovery Across Multiple Availability Zones................................................................49 


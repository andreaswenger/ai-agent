**Nutanix Cloud Clusters on AWS Networking Best Practices** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **Contents** 

**1. Overview of Nutanix Cloud Clusters on AWS Solution Networking................................................................................................ 4** 

**2. Nutanix Cloud Clusters on AWS Glossary............................................7 3. Flow Virtual Networking for Nutanix Cloud Clusters on AWS.............9** Adding Flow Virtual Networking Support to an AWS Cluster........................................................... 11 

## **4. Virtual Private Clouds............................................................................12** 

AWS Virtual Private Cloud with Flow Virtual Networking and Nutanix Cloud Clusters..................... 16 

**5. Configuring Nutanix Cloud Clusters on Amazon Web Services....... 20** Creating an Amazon Web Services Virtual Private Cloud................................................................22 Setting Up Connectivity With NAT....................................................................................................25 Setting Up Connectivity With No-NAT.............................................................................................. 29 Configuring Virtual Private Cloud Peering in AWS...........................................................................32 Creating AWS Site-to-Site Virtual Private Network Connections......................................................35 Configuring AWS Direct Connect..................................................................................................... 38 

**6. References and Resources for Nutanix Cloud Clusters on AWS Networking.............................................................................................. 39** Nutanix Cloud Clusters on AWS Networking Best Practices........................................................... 39 **About Nutanix.............................................................................................42 List of Figures.............................................................................................................................................43** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## 1. Overview of Nutanix Cloud Clusters on AWS Solution Networking 

The Nutanix Cloud Clusters (NC2) on AWS solution offers bare metal as a service to customers with on-demand hardware provisioning and consumption, with a userfriendly and consistent experience from on-premises to cloud to edge. AHV, the Nutanix hypervisor, runs an efficient embedded distributed network controller that integrates guest VM networking with Prism Element, the Nutanix cluster management UI. Prism Central provides centralized management for one or more clusters and works with AHV and NC2 to use the Flow Virtual Networking product to create an overlay that provides granular control. With Flow Virtual Networking, you can connect to all AWS services and workloads running on the clusters can send and receive north-south traffic. 

The NC2 console places the complete Nutanix platform directly on a bare-metal instance in Amazon Elastic Compute Cloud (EC2). Like any on-premises Nutanix deployment, this bare-metal instance runs a Controller VM (CVM) and AHV, but it uses the AWS elastic network interface (ENI) to connect to the network. AHV user VMs don't require any additional configuration to access AWS or other EC2 instances. 

AHV runs a native networking stack that integrates user VM networking with AWS networking. AWS allocates all user VM IP addresses from the AWS subnets in the existing virtual private clouds (VPCs). Integrate the native networking stack with AWS networking to seamlessly use AWS on AHV user VMs without encountering the complexities of a network deployment or performance loss. 

Starting with AOS 6.8, you can enable Flow Virtual Networking on AWS. NC2 on AWS uses the Flow Virtual Networking network controller to create overlay networks, providing granular control for Nutanix administrators while enabling seamless connectivity to AWS. 

© 2026 Nutanix, Inc. All rights reserved  | **4** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

Figure 1: Overview of NC2 on AWS 

Networking is arguably the most complex aspect of building a hybrid cloud environment for production use cases in enterprise environments, so we provide a user-friendly method of configuring AWS networking with NC2 for production use cases. Our audience is anyone who must inspect all traffic entering and leaving NC2. 

Key topics: 

- Overview of NC2 on AWS with Flow Virtual Networking 

- NC2 on AWS network management 

- NC2 on AWS best practices 

_Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|August 2024|Original publication.|
|1.1|October 2024|Updated the CIDR Subnet|
|||Requirements section.|



© 2026 Nutanix, Inc. All rights reserved  | **5** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|1.2<br>1.3<br>1.4|December 2024<br>Updated the Nutanix Clusters<br>in an Amazon Web Services<br>Virtual Private Cloud,<br>Limitations of Nutanix Cloud<br>Clusters on Amazon Web<br>Services, NAT Gateway, and<br>No-NAT Gateway sections.<br>December 2025<br>Updated the Nutanix Clusters<br>in an Amazon Web Services<br>Virtual Private Cloud section<br>and the Required CIDR<br>Subnets table.<br>March 2026<br>Updated document structure.|



© 2026 Nutanix, Inc. All rights reserved  | **6** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## 2. Nutanix Cloud Clusters on AWS Glossary 

We use the following Nutanix Cloud Clusters (NC2) on AWS terminology: 

## **Nutanix Cloud Clusters console** 

Manages cluster-related workflows (creation, expansion, termination, node replacement) 

## **Flow Virtual Networking** 

A software-defined networking solution that provides multitenant isolation, selfservice provisioning, and IP address preservation for the AHV clusters using virtual private clouds (VPCs), subnets, and other virtual components that are separate from the physical network 

## **Internet gateway** 

Connects the VPC to the public internet and connects devices that already have an AWS-assigned public address and are in a public subnet to the internet 

## **NAT gateway** 

A NAT service that you can use to connect instances in a private subnet to services outside your VPC while protecting them from external services attempting to initiate connections 

## **Subnet** 

A range of IP addresses in your VPC that you launch AWS resources, such as Amazon Elastic Compute Cloud (EC2) instances, into and route traffic to and from using route tables; can connect to the internet, other VPCs, and your datacenters 

## **Elastic IP address** 

A static, public IP address associated with your AWS account that you can associate with your instance to enable internet communication if you don't have a public IP address 

© 2026 Nutanix, Inc. All rights reserved  | **7** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **Security groups** 

Offer a firewall-like method of filtering traffic to elastic network interfaces (ENIs) and control the traffic to and from the resources in the VPC where the security groups are set up; typically used with EC2 instances 

## **Placement group** 

A logical grouping of instances in an availability zone that helps you influence the placement of individual EC2 instances to meet specific requirements for performance, latency, or compliance 

© 2026 Nutanix, Inc. All rights reserved  | **8** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## 3. Flow Virtual Networking for Nutanix Cloud Clusters on AWS 

Flow Virtual Networking implements seamless IP subnet stretching or overlay networks across multiple clusters (on-premises and cloud) for same-subnet connectivity across different physical networks using virtual private clouds (VPCs), virtual private networks (VPNs), NAT, floating IP addresses, and policy-based routing for AHV with Open Virtual Network (OVN) advanced virtual local area networks (VLANs). 

Flow Virtual Networking has two main components: the network controller and the network gateway. The network controller is the networking component of Prism Central that manages, configures, monitors, and optimizes the network resources for Flow Virtual Networking VPCs and VLAN subnets. It provides programmability, automation, and centralized control for configuring and managing network flows. With Flow Virtual Networking, you can use centralized VLAN management, Flow Virtual Networking, and Flow Network Security Next-Gen. The network gateway appliance is available from Prism Central, and with it, you can use network gateway VMs to create VPNs, virtual extensible LAN tunnel endpoints (VTEPs), or Border Gateway Protocol (BGP) gateways to connect subnets through VPN connections, layer 2 subnet extensions over VPN or VTEP, or BGP sessions. 

Flow Virtual Networking is a software-defined networking solution with the following three-plane architecture: 

## **Management plane (Prism Central)** 

From the management plane, you can configure, manage, and monitor virtual network resources such as IP addresses, subnets, routes, and protocols. Prism Central is the management plane for Flow Virtual Networking. Prism Central's Network & Security entity provides the Flow Virtual Networking components like subnets, VPCs, floating IP addresses, and connectivity (network gateways, connections like VPN or VTEP, and BGP sessions). Control access to these virtual networking components using role-based access control (RBAC) in Prism Central. 

© 2026 Nutanix, Inc. All rights reserved  | **9** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **Control plane** 

The control plane is defined by the network controller, and with it, you can create a virtual overlay network as an abstraction of the complex underlying network infrastructure. It manages the network services and directs packet traffic throughout the network and, with the network gateway appliance, helps you manage the networks, connections, and devices with ease. 

**Note:** In extra-large Prism Central deployments, the network controller is automatically enabled when you deploy Prism Central. In small and large Prism Central deployments, you must manually enable the network controller. 

## **Data plane (AHV)** 

Open vSwitch (OVS) in the AHV host is the data plane that processes VM network traffic. It processes all network packets between VMs on the host and from VMs and other processes on the host to the external network. 

Figure 2: Nutanix Flow Virtual Networking Component Overview 

OVS deploys a collection of bridges in the AHV hosts, and traffic flows through these bridges between the AHV hosts. To configure and manage these bridges, deploy virtual switches in AHV. During the installation, AHV deploys a default virtual switch (vs0) that manages the bridges (br0) on all the AHV hosts in the cluster. For more information, see About Virtual Switch. 

© 2026 Nutanix, Inc. All rights reserved  | **10** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

In the VPC where you deployed NC2, create the following subnets to manage inbound and outbound traffic: 

- One private management subnet for internal cluster management and communication between CVM, AHV, and so on 

- (Optional) One public subnet with an internet gateway and a NAT gateway to provide external connectivity to the NC2 console 

- One or more private subnets for user VM or Flow Virtual Networking traffic, depending on your needs 

**Note:** Don't add the management subnet as a user VM subnet in Prism Element because user VMs and management VMs must be on separate subnets. 

## **Adding Flow Virtual Networking Support to an AWS Cluster** 

The AWS cluster creates a public subnet for internet connectivity, along with the private subnet for management, Prism Central, and user VMs. To add Flow Virtual Networking support, follow these steps: 

**1.** Create private external overlay network subnets. 

**2.** Use the private external overlay network subnets in the transit virtual private cloud (VPC) as external overlay network NAT subnets. 

**3.** Create a separate elastic network interface (ENI) for each external overlay network NAT subnet. 

**4.** Associate the ENI with the bare-metal instance. 

**5.** Assign the ENI a primary IP address from the subnet. 

Perform these steps for each bare-metal node in the cluster. 

© 2026 Nutanix, Inc. All rights reserved  | **11** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## 4. Virtual Private Clouds 

Virtual private clouds (VPCs) are independent and isolated IP address spaces that function as logically isolated virtual networks that you can use to manage the virtual network with enhanced automation and scaling. They are one or more subnets that are connected through a logical or virtual router. The IP addresses within a VPC must be unique, but IP addresses can overlap across VPCs. Because you provision VPCs on other IP address–based infrastructure (connecting AHV nodes), they are often referred to as overlay networks. Tenants can create VMs and connect them to one or more subnets in a VPC. 

You can use IP address–based subnets to connect VMs in a VPC. A VPC can have multiple subnets. VPC subnets use private IP address ranges. The following figure shows two VPCs (Blue and Green), each with two subnets, 192.168.1.0/24 and 192.168.2.0/24, that are connected by a logical router. Each subnet has a VM with an IP address assigned. The subnets and VM IP addresses overlap between the two VPCs. 

Figure 3: Example VPC Configuration 

For external connectivity, connect a user VPC to a transit VPC or an overlay subnet with external connectivity. You can use a maximum of one NAT and one no-NAT external network for each VPC. 

Transit VPCs use a hub-and-spoke architecture and provide the following benefits: 

© 2026 Nutanix, Inc. All rights reserved  | **12** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

- Transit VPCs simplify and scale the routing configuration (for north-south traffic) for a large number of VPCs by introducing a hub VPC in the path. With the hub VPC, you don't advertise dynamic routing to infrastructure routers or statically configure infrastructure routers as often. 

- Transit VPCs route traffic between user VPCs using private IPv4 addresses (using externally routable prefix routes), giving user VPCs access to resources that you have in one of your regular VPCs. An added advantage is that you don't need to route traffic on the physical infrastructure. 

- Transit VPCs host services shared among VPCs (by hosting these services on overlay subnets under a transit VPC). 

- Transit VPCs provide a logical separation between provider (transit VPC) and tenant (user VPC) networks in a multitiered model. Multitiered models have layers of access control where each tenant controls their own routing and security policies, whereas with transit VPCs, the administrators control the routing and security policies in the layer above the tenant layer. 

- With transit VPCs, you can configure routing and policy control over cross-tenant communication without touching the physical infrastructure. 

Use NAT to map the IP addresses of an internal or private subnet to a public IP address that can communicate with the internet or other subnets. NAT is a process for modifying the source or destination addresses in the headers of an IP address packet when you put the packet in transit. In general, the sender and receiver applications don't register that the IP packets are being modified. 

## **NAT gateway** 

A NAT gateway service connects the entities inside an internal network to the internet without exposing the internal network and its entities. It performs NAT as a service. 

© 2026 Nutanix, Inc. All rights reserved  | **13** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

Figure 4: VPC Traffic Using NAT Subnet Overlay 

© 2026 Nutanix, Inc. All rights reserved  | **14** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **No-NAT Gateway** 

Like the NAT gateway service, the no-NAT gateway service also provides external connectivity. However, it doesn't perform NAT. The no-NAT gateway service selects an AHV host or node from the Prism Element cluster to act as the gateway and route the external traffic. When you create a VPC with a VLAN subnet providing external connectivity, you can deploy scale-out gateway services with up to four AHV hosts acting as gateways. The gateway distributes external (north-south) traffic for the VPC across the number of AHV hosts or nodes selected for the VPC. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

Figure 5: VPC Traffic Using No-NAT Subnet Overlay 

## **AWS Virtual Private Cloud with Flow Virtual Networking and Nutanix Cloud Clusters** 

The AWS virtual private cloud (VPC) contains one public and one private route table, the Elastic Compute Cloud (EC2) instance, and VPC endpoints. The public route table 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

has one public subnet and a router connected to the internet gateway and the NAT gateway (which connects to the Nutanix Cloud Clusters (NC2) console). This router is also connected to the router in the private route table, which in turn connects the management, Prism Central, and Flow Virtual Networking subnets, the VPC endpoints, and AWS Transit Gateway. The NAT and no-NAT gateways in Flow Virtual Networking also connect to the Flow Virtual Networking VPC. The AWS VPC endpoints connect to the S3 buckets, the Amazon Relational Database Service (RDS), and AWS Glue, an event-driven, serverless computing platform. AWS Transit Gateway connects to the onpremises Nutanix Cloud Platform and its network gateways through the VPN gateway or AWS Direct Connect. 

The AWS VPC has CIDR ranges that you can use for private and public subnets. The public subnet is primarily used for external communication with the internet gateway as the default route target in its route table, while the private subnet uses the NAT gateway as its default route. 

© 2026 Nutanix, Inc. All rights reserved  | **17** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

Figure 6: Flow Virtual Networking on AWS Model 

Create a Nutanix cluster in an existing or new VPC using the NC2 console. This process places AHV and the cluster Controller VMs (CVMs) on a dedicated subnet, separate from the VMs. 

Use the CIDR subnet IP address range /24 to /26 for the native networking deployment VPC or /23 for the Flow Virtual Networking VPC that you create the cluster in (with a primary CIDR range and secondary CIDR ranges). Follow the guidance in the AWS Subnet CIDR Blocks documentation. 

The subnets for the cluster are all in a single availability zone with no broadcast or multicast. The AHV and CVM subnet and the user VM subnets are private, and the subnet CIDR ranges are a subset of the VPC's CIDR range, so they can't overlap with each other. Reserve the first IP address in the subnet CIDR range for the AWS Gateway. AWS reserves the second and third IP addresses in the subnet CIDR range for itself. 

© 2026 Nutanix, Inc. All rights reserved  | **18** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

Reserve the primary elastic network interface (ENI) for AHV and the CVM, and create secondary ENIs for the user VMs. Each ENI supports one primary IP address and multiple secondary IP addresses based on instance type. Each node can support up to 686 IP addresses because each AWS bare-metal instance has a limit of 15 ENIs, which can each support 50 IP addresses. 

You can use AWS security groups and network access control lists to secure your cluster relative to other AWS or on-premises resources. When you deploy Flow Virtual Networking, a fourth security group is deployed for Prism Central that has all the necessary rules for the Flow Virtual Networking control plane. If you use Nutanix Disaster Recovery to protect your workloads, edit this security group to allow traffic from the onpremises Prism Central instance. You can also use existing groups in your environment. 

NC2 on AWS uses four main types of security groups: 

- Default (including internal management, user management, and user VM) 

- VPC-level 

- Cluster-level 

- Network-level 

For more information, see Security Groups. Create the following AWS security groups: 

- Internal management security groups 

- User management security groups 

- Prism Central security group 

- User VM security groups 

The NAT gateway has external connectivity using the internet gateway, which connects using a public subnet. The network load balancer provides reachability into the cluster; for example, for Prism, the web server is exposed using a public IP address. 

© 2026 Nutanix, Inc. All rights reserved  | **19** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## 5. Configuring Nutanix Cloud Clusters on Amazon Web Services 

You must have the following CIDR subnets for Nutanix Cloud Clusters (NC2) on AWS to work. 

## _Table: Required CIDR Subnets_ 

|**Purpose**|**CIDR**|
|---|---|
|Virtual private cloud (VPC)|/23|
|Private management subnet|/24|
|Public subnet|/28|
|User VM subnets|between /16 and /25|
|Prism Central subnet|/28|
|Flow Virtual Networking subnet|/24|



Assign appropriately sized CIDR subnets that don't conflict with existing corporate networks. Enable IP address forwarding for Flow Gateway VMs. For specific TCP port requirements, see Requirements for NC2 on AWS. 

The NC2 on AWS solution has the following limitations: 

- A cluster supports a maximum of 28 nodes. NC2 supports 28-node cluster deployments in AWS regions that have seven placement groups. 

- NC2 doesn't support two-node clusters, and Nutanix doesn't recommend using singlenode clusters in production environments. 

- NC2 doesn't support sharing AWS subnets among multiple clusters. 

- NC2 only supports IPv4. 

- You can only use private CIDR ranges for cloud subnets on NC2 on AWS clusters. Public CIDR ranges aren't supported. 

© 2026 Nutanix, Inc. All rights reserved  | **20** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

- You can't use the 192.168.5.0/24 CIDR range for the VPC that you use to deploy the NC2 on AWS cluster. All Nutanix nodes use that CIDR range for communication between the Controller VM (CVM) and the installed hypervisor. 

- NC2 on AWS drops broadcast and unknown unicast traffic. 

- NC2 on AWS doesn't support unmanaged networks. 

- If you create a new VPC in the NC2 console when you create a cluster, you can't deploy other clusters in that VPC. However, if you deploy a cluster in an existing VPC (a VPC that you created using AWS), you can deploy multiple clusters in that VPC. 

Create VPCs using the AWS console to deploy multiple clusters in the VPCs. 

- NC2 on AWS doesn't support reconfiguring Prism Central VM IP addresses for scaledout Prism Central deployments. 

- You can't preserve the IP address when you recover VMs from a Nutanix Cluster Protect backup in S3 buckets. 

- If your NC2 on AWS cluster uses Flow Virtual Networking, you can't use a proxy server. 

- You can't hibernate dedicated hosts. 

For hibernation prerequisites and workflow, see Hibernate and Resume in NC2. 

- You can't mix dedicated host tenancy and default tenancy instances in a single cluster or change the instance tenancy type after you deploy the NC2 cluster. 

- AWS Elastic Compute Cloud (EC2) dedicated hosts don't support partition placement groups, and Nutanix can't guarantee resilience against AWS rack partition failures with dedicated hosts. 

- NC2 on AWS supports a combination of i3.metal, i3en.metal, and i4i.metal instance types or z1d.metal, m5d.metal, and m6id.metal instance types, but the AWS region must also support the instance types used. 

For more information, see Supported Regions and Bare-metal Instances. 

© 2026 Nutanix, Inc. All rights reserved  | **21** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **Creating an Amazon Web Services Virtual Private Cloud** 

An AWS virtual private cloud (VPC) contains four subnets (one management subnet, two user VM subnets, and a public subnet) in a single availability zone. The private subnets have VMs in security groups and connect to the internet gateway through the NAT gateway and the Prism Element Network Load Balancer. 

Figure 7: Example AWS VPC 

To create a VPC, follow these steps: 

**1.** Sign in to AWS and go to **VPC service** . 

© 2026 Nutanix, Inc. All rights reserved  | **22** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**2.** Create a VPC with any CIDR range (10.0.0.0/16 in our example): 

   - **a.** Create an internet gateway and attach it to your VPC (vpc-0dbb5a4ff369d1adc in our example). 

   - **b.** Find the VPC's existing route table and add a route that directs route traffic 

      - 0.0.0.0/0 to the internet gateway that you created. 

   - **c.** Create another route table for NAT (Nutanix Cloud Clusters (NC2) NAT routing table in our example). 

You only need one NAT route table per VPC. 

**Note:** Remember the VPC cloud ID (vpc-0dbb5a4ff369d1adc in our example). 

**3.** Create the public subnet in the correct VPC and choose its CIDR IP range (10.0.1.0/24 in our example). 

You only need one public subnet per VPC, which is important if you plan to reuse this VPC. 

**Note:** Remember the subnet ID. 

**4.** Find the main route table and associate it with the public subnet. 

**5.** Create the management subnet and associate it with the route table that you created (NC2 NAT routing table in our example). 

We recommend creating one management subnet per cluster if you plan to reuse the VPC. 

**6.** (Optional) Create the user VM subnet and associate it with the route table that you created. 

**7.** Create a NAT gateway in the public subnet and an elastic IP address to associate with it. 

**8.** When the NAT gateway is available, locate the routing table you created and add a route that directs traffic from 0.0.0.0/0 to the NAT gateway. 

Only create one NAT gateway per VPC. 

## **Creating a Network Load Balancer for Accessing Prism** 

To create a network load balancer for accessing Prism, follow these steps: 

**1.** Open the Elastic Compute Cloud (EC2) service page and click **Load Balancers** , then click **Create Load Balancer** . 

© 2026 Nutanix, Inc. All rights reserved  | **23** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**2.** Select **Network Load Balancer** . 

**3.** Enter a name and select the **internet-facing** scheme. 

**4.** Select listeners (protocol and port) for the load balancer: 

   - **a.** For Protocol, select **TCP** . 

   - **b.** For Port, select **9440** . 

**5.** Select the virtual private cloud (VPC) and availability zone (AZ): 

   - **a.** Choose the VPC where you plan to deploy the cluster. 

   - **b.** For the AZ, choose the public subnet that you created. 

**6.** Skip the Configure Security Settings page. 

**7.** On the Configure Routing page, select **New target group** : 

   - **a.** Name the target group. 

   - **b.** For Target Type, select **IP** . 

   - **c.** For Protocol, select **TCP** . 

   - **d.** For Port, select **9440** . 

**8.** Configure the health checks settings: 

   - **a.** For Protocol, select **HTTPS** . 

   - **b.** For Path, select **/console/** . 

   - **c.** For Advance, use the default. 

   - **d.** For Healthy Threshold, select **5** . 

**9.** On the Register Targets page, under **Specify one or more IP address to register as target** , add a target entry using the CVM IP address from each node (in our example, 10.10.10.1 port: 9440) and click **Add to the list** . 

**10.** Click **Preview** and validate all values, then click **Create** . 

**11.** When this load balancer becomes active, use it with its DNS name. 

**12.** Ensure that the port you must access (:9440) is open in the inbound policy of the security group associated with the bare-metal instance. 

**13.** Access Prism: 

   - **a.** Copy the DNS name from the description of the network load balancer. 

   - **b.** Paste the DNS name into a browser and add **:9440** 

at the end of the URL (for example, `https://prod-lb-` 

`cluster-1152-1bb8b419e3e0b00a.elb.us.amazonaws:9440` ). 

© 2026 Nutanix, Inc. All rights reserved  | **24** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **Setting Up Connectivity With NAT** 

In a NAT routing setup, traffic flows from the Flow Virtual Networking user virtual private cloud (VPC) to external resources through a Flow Virtual Networking NAT gateway. This gateway translates private IP addresses to public IP addresses for outbound traffic and vice versa for inbound traffic. 

To create a Flow Virtual Networking user VPC and attach the NAT external overlay network subnet to it, follow these steps: 

**1.** Sign in to the Nutanix Cloud Clusters (NC2) Prism Central web console. 

**2.** Click the **Entities** menu in the main menu, click **Network & Security** , and click **Virtual Private Clouds** . 

**3.** In the Virtual Private Clouds List page, click **Create VPC** , then configure the VPC. 

   - **a.** Name the VPC (uservpc in our example). 

   - **b.** Select **External Connectivity** . 

   - **c.** Click **Associate External Subnet** : 

      - **i.** In the Associate External Subnet dialog, select **overlay-external-subnetnat** from the dropdown menu. 

      - **ii.** Select **Set this subnet as the default next hop for outbound traffic.** 

      - **iii.** Enter the destination prefix **0.0.0.0/0** . 

      - **iv.** Click **Save** . 

   - **d.** Leave the defaults for Externally Routable IP Addresses. 

**Note:** DHCP populates the Domain Name Server (DNS) field automatically, but you can override it when you configure the subnet. 

- **e.** Click **Create** . 

## **Creating an External Overlay Network NAT Subnet** 

To create an external overlay network NAT subnet in the user virtual private cloud (VPC), follow these steps: 

**1.** Sign in to the Nutanix Cloud Clusters (NC2) Prism Central web console. 

**2.** Click **Entities** in the main menu, click **Network & Security** , and click **Virtual Private Clouds** . 

© 2026 Nutanix, Inc. All rights reserved  | **25** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**3.** In the Virtual Private Clouds List page, click the user VPC (uservpc in our example) where you plan to create the subnet. 

**4.** Click the **Subnets** tab, then click **Create Subnet** . 

**5.** In the Create Subnet dialog, configure the subnet: 

   - **a.** Name the subnet (user-subnet in our example). 

   - **b.** Confirm that the type is **Overlay** . 

   - **c.** Enter the network IP address and prefix and the gateway IP address. 

   - **d.** Click **Add IP Pool** and enter the first and last IP address of the range, then click the checkmark under Actions. 

   - **e.** (Optional) Expand and configure **Domain Settings** . 

   - **f.** Click **Create** . 

## **Creating Flow Virtual Networking User VMs** 

To create Flow Virtual Networking user VMs, follow these steps: 

**1.** Sign in to the Nutanix Cloud Clusters (NC2) Prism Central web console. 

**2.** Click **Compute & Storage** , then click **VMs** . 

**3.** Click **Create VM** . 

**4.** On the Configuration page of the Create VM dialog, configure the VM: 

   - **a.** Name the VM (User-Vm1 in our example). 

   - **b.** (Optional) Describe the VM. 

   - **c.** In the Cluster dropdown menu, select the cluster where you plan to create the VM. 

   - **d.** Enter the number of VMs to create. 

The created VM names are suffixed sequentially. 

- **e.** Enter the number of virtual CPUs (vCPUs) to allocate to this VM. 

- **f.** Enter the number of cores assigned to each vCPU. 

- **g.** Enter the amount of memory (in gibibytes) to allocate to this VM. 

- **h.** Select the **Enable Memory Overcommit** checkbox. 

- **i.** Click **Next** . 

© 2026 Nutanix, Inc. All rights reserved  | **26** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**5.** On the Resources page, click **Attach Disk** and configure the disk in the Attach Disk dialog: 

   - **a.** For Type, select **CD-ROM** from the dropdown menu. 

   - **b.** For Operation, select **Empty CD-ROM** from the dropdown menu. 

   - **c.** For Bus Type, select **IDE** from the dropdown menu. 

   - **d.** Click **Save** . 

**6.** Click **Attach Disk** again and configure the disk in the Attach Disk dialog: 

   - **a.** For Type, select **Disk** from the dropdown menu. 

   - **b.** For Operation, select **Allocate on Storage Container** from the dropdown menu. 

   - **c.** For Storage Container, select the correct storage container from the dropdown menu. 

   - **d.** For Capacity, enter **20** . 

   - **e.** For Bus Type, select **SCSI** from the dropdown menu. 

   - **f.** Click **Save** . 

**7.** Click **Next** . 

**8.** Under Networks, click **Attach to Subnet** , and configure the subnet in the Attach to Subnet dialog: 

   - **a.** For Subnet, select **user-subnet: uservpc** from the dropdown menu. 

   - **b.** For Network Connection State, select **Connected** from the dropdown menu. 

   - **c.** Under Private IP Assignment, for Assignment Type, select **Assign with DHCP** from the dropdown menu. 

   - **d.** Under Floating IP Assignment, for Assignment Type, select **Assign Floating IP** from the dropdown menu. For Floating IP Address, select **10.0.0.135** from the dropdown menu. 

   - **e.** Click **Save** . 

**9.** Click **Next** . 

**10.** (Optional) On the Management page, switch the **Enable 'Default-Storage' policy** toggle on and select a time zone and guest customization or leave them as default. 

**11.** Click **Next** . 

**12.** On the Review page, click **Create VM** and close the Create VM dialog. 

**13.** In the VM Summary tab and List page, click the **List** tab and select the new VMs. 

**14.** Click **Actions** , then **Power On the VMs** . 

© 2026 Nutanix, Inc. All rights reserved  | **27** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **15.** Click **OK** . 

## **Requesting Floating IP Addresses** 

To request floating IP addresses, follow these steps: 

**1.** Sign in to the Prism Central web console. 

**2.** Click **Entities** in the main menu, click **Network & Security** , then click **Floating IPs** . 

**3.** In the Floating IPs List page, click **Request Floating IP** . 

**4.** In the Request Floating IP(s) dialog, configure the floating IP addresses: 

   - **a.** For External Subnet, select **overlay-external-subnet-nat** from the dropdown menu (default). 

   - **b.** Enter the number of floating IP addresses required. 

   - **c.** (Optional) Select the **Assign Floating IPs** checkbox to assign the floating IP addresses to specific VMs in the table. 

   - **d.** Click **Save** . 

## **Configuring AWS Transit Gateway** 

To configure the AWS transit gateway, follow these steps: 

**1.** Create a transit gateway. 

**2.** Attach virtual private clouds (VPCs) to the transit gateway. 

**3.** (Optional) Attach a virtual private network (VPN). 

**4.** Configure the associated route table of the transit gateway. 

AWS Transit Gateway supports route propagation to automatically update the route tables with learned routes from the attached VPCs. However, you might need to explicitly add a static route in the transit gateway–associated route table to ensure that it correctly routes traffic destined for the VPC through the transit gateway. 

**5.** In each VPC route table, add static routes to direct traffic to the transit gateway for externally routable prefix prefixes. 

If you don't add the static routes, traffic takes the default route to the internet gateway. 

**6.** Configure or update the AWS security groups. 

© 2026 Nutanix, Inc. All rights reserved  | **28** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **Setting Up Connectivity With No-NAT** 

As part of the no-NAT configuration, create a no-NAT overlay subnet in the Flow Virtual Networking transit virtual private cloud (VPC) with a prefix that doesn't overlap with the destination prefixes, including AWS VPC prefixes, Nutanix Cloud Clusters (NC2) Controller VM (CVM), AHV, and Prism Central subnets, and on-premises subnets. For example, the subnet in the user VPC for the virtual extensible LAN tunnel endpoints (VTEP) gateway uses the 100.64.1.0/24 prefix, so the no-NAT overlay subnet in the Flow Virtual Networking transit VPC must not use 100.64.1.0/24. 

To create an external overlay no-NAT subnet in the Flow Virtual Networking transit VPC, follow these steps: 

**1.** Sign in to the Prism Central web console. 

**2.** Click **Entities** in the main menu, click **Network & Security** , then click **Virtual Private Clouds** . 

**3.** In the Virtual Private Clouds List page, select the Flow Virtual Networking transit VPC you plan to create a subnet in and click **Create Subnet** . 

**4.** In the Create Subnet dialog, configure the subnet: 

   - **a.** Name the subnet. 

   - **b.** Confirm that the type is **Overlay** . 

   - **c.** Enter the network IP address and prefix and the gateway IP address. 

   - **d.** Click **Add IP Pool** and enter the first and last IP address of the range, then click the checkmark under Actions. 

   - **e.** Expand and configure **Domain Settings** . 

   - **f.** Click **Create** . 

## **Configuring the Externally Routable Prefix in Flow Virtual Networking Virtual Private Cloud** 

To configure the externally routable prefix in the Flow Virtual Networking transit virtual privatge cloud (VPC), follow these steps: 

**1.** Sign in to the Prism Central web console. 

**2.** Click **Entities** in the main menu, click **Network & Security** , then click **Virtual Private Clouds** . 

© 2026 Nutanix, Inc. All rights reserved  | **29** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**3.** In the Virtual Private Clouds List page, click the Flow Virtual Networking transit VPC you plan to configure an externally routable prefix for. 

**4.** In the Update VPC dialog, under Externally Routable IP Addresses, specify the prefixes of the user VPC subnets that must be accessed through the no-NAT option: 

   - **a.** (Optional) Configure the IP address in the Externally Routable IP Addresses field as the route in the AWS route table with AHV elastic network interface (ENI) as the next hop. 

   - **b.** Expand Virtual Private Cloud Entities in the AWS console and click **Route Tables** . 

   - **c.** Select or search for the VPC, select it, then click **Routes** . 

**5.** Click **Update** . 

## **Attaching External Overlay No-NAT Subnet in User Virtual Private Cloud** 

To attach the external overlay no-NAT subnet in the user virtual private cloud (VPC), follow these steps: 

**1.** Sign in to the Prism Central web console. 

**2.** Click **Entities** in the main menu, click **Network & Security** , then click **Virtual Private Clouds** . 

**3.** In the Virtual Private Clouds List page, select the user VPC you plan to attach the external overlay no-NAT subnet to and click **Update** . 

**4.** In the Update VPC dialog, attach the subnet: 

   - **a.** Select the **External Connectivity** checkbox. 

   - **b.** Click **Associate External Subnet** and attach the overlay no-NAT subnet that you created in the transit VPC. 

   - **c.** Click **Save** . 

   - **d.** Click **Update** . 

## **Configuring Externally Routable Prefix in User Virtual Private Cloud** 

To configure the externally routable prefix in the user virtual private cloud (VPC), follow these steps: 

**1.** Sign in to the Prism Central web console. 

**2.** Click **Entities** in the main menu, click **Network & Security** , then click **Virtual Private Clouds** . 

**3.** In the Virtual Private Clouds List page, click the user VPC hyperlink. 

© 2026 Nutanix, Inc. All rights reserved  | **30** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**4.** In the Update VPC dialog, configure external connectivity: 

   - **a.** Select the **External Connectivity** checkbox. 

   - **b.** In the Externally Routable IP Addresses field, specify the entire range of IP addresses taken from the Flow Virtual Networking transit VPC's externally routable prefix. 

   - **c.** Click **Update** . 

**5.** Create an overlay no-NAT subnet in the user VPC with the same prefix or sub-prefix of the externally routable prefix on the user VPC. 

**6.** Create Flow Virtual Networking user VMs. 

**7.** Attach the no-NAT subnet to the user VMs by adding a NIC to the VM in the subnet. 

The VMs attached to this overlay subnet have no-NAT connectivity to the AWS network. 

## **Configuring No-NAT Traffic Flow Through AWS Transit Gateway** 

To configure no-NAT traffic flow through the AWS transit gateway, follow these steps: 

**1.** Create a transit gateway. 

**2.** Attach virtual private clouds (VPCs) to the transit gateway. 

**3.** (Optional) Attach a virtual private network (VPN). 

**4.** Configure the associated route table of the transit gateway. 

AWS Transit Gateway supports route propagation to automatically update the route tables with learned routes from the attached VPCs. However, you might need to explicitly add a static route in the transit gateway–associated route table to ensure that it correctly routes traffic destined for the VPC through the transit gateway. 

**5.** In each VPC route table, add static routes to direct traffic to the transit gateway for externally routable prefix prefixes. 

If you don't add the static routes, traffic takes the default route to the internet gateway. 

**6.** Configure or update the AWS security groups. 

© 2026 Nutanix, Inc. All rights reserved  | **31** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **Configuring Virtual Private Cloud Peering in AWS** 

A virtual private cloud (VPC) peering connection is a networking connection between two VPCs that routes traffic between them using private IPv4 addresses or IPv6 addresses. Instances in either VPC can communicate with each other as if they are in the same network. You can create a VPC peering connection between your own VPCs or with a VPC in another AWS account. 

To create a VPC peering connection, follow these steps: 

**1.** Open the AWS console in the source region and select the source VPC service. 

**2.** Navigate to **Peering connections** and click **Create Peering Connection** . 

**3.** In the Create peering connection dialog, configure the peering connection: 

   - **a.** For the Name, enter **<team_name>-vpc-peering** . 

**Note:** Keep this naming convention, replacing **<team_name>** with the name of your team. 

   - **b.** For VPC ID (Requester), enter the VPC ID for the source VPC (vpc-09fc221324f190a4c in our example). 

   - **c.** Ensure that the VPC CIDR range is correct. 

   - **d.** Select **Another account** and enter the AWS account ID (661306471920 in our example). 

   - **e.** Select **Another region** and select the region of the destination VPC (Asia Pacific (Mumbai) (ap-south-1) in our example). 

   - **f.** For VPC ID (Accepter), enter the VPC ID for the destination VPC (vpc-0b2080fdd4c5dd1e9 in our example). 

   - **g.** Click **Create Peering Connection** . 

**4.** Open the AWS console in the destination region and select the destination VPC service. 

**5.** In the Accept VPC peering connection request dialog, verify the connection values and click **Accept request** . 

© 2026 Nutanix, Inc. All rights reserved  | **32** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**6.** Edit the VPC subnet route table to add the CIDR ranges for cluster 1 and 2. 

   - **a.** In the source cluster route table, add the destination CIDR range (172.16.0.0/16 in our example) and select **Peering Connection** from the dropdown menu and find the VPC peering connection ID. 

   - **b.** In the destination cluster route table, add the source CIDR range (10.0.0.0/16 in our example) and select **Peering Connection** from the dropdown menu and find the VPC peering connection ID. 

**7.** Add a rule for the Prism Central security group on the source cluster that allows all TCP traffic from the destination cluster network and the destination cluster's CIDR range. 

**8.** Add a rule for the user management security group on the destination cluster that allows all traffic from the source cluster network and the source cluster's CIDR range. 

## **Accessing Prism Using the Network Load Balancer** 

To configure the target group using the Prism virtual IP address, follow these steps: 

**1.** Sign in to AWS and open the Elastic Compute Cloud (EC2) service page. 

**2.** Click **Target Groups** , then click **Create target group** . 

© 2026 Nutanix, Inc. All rights reserved  | **33** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**3.** In the Create target group dialog, configure the target group: 

   - **a.** For Target Type, select **IP Addresses** . 

   - **b.** For Protocol, select **TCP** . 

   - **c.** For Port, select **9440** . 

   - **d.** Name the target group. 

   - **e.** For IP address type, select **IPv4** . 

   - **f.** Select the virtual private cloud (VPC) that hosts the cluster. 

   - **g.** Under Health Check, configure the following settings: 

      - **i.** For Protocol, select **HTTPS** . 

      - **ii.** For Path, select **/console/** . 

      - **iii.** For the Advanced Health Checks, use the defaults. 

   - **h.** Click **Next** . 

   - **i.** Assign the virtual IP address or CVM IP address to the cluster. 

   - **j.** Under Specify one or more IP address to register as target, add target entries using the CVM IP address from each node with port: 9440. 

   - **k.** Click **Create target group** . 

## **Creating a Network Load Balancer** 

To create a network load balancer, follow these steps: 

**1.** Sign in to AWS, open the Elastic Compute Cloud (EC2) service page and click **Load Balancers** , then click **Create Load Balancer** . 

**2.** Select **Network Load Balancer** . 

**3.** Enter a name and select the **internet-facing** scheme. 

**4.** For IP address type, select **IPv4** . 

**5.** Under Network Mapping, select the virtual private cloud (VPC) and availability zone (AZ): 

   - **a.** Choose the VPC where you plan to deploy the cluster. 

   - **b.** For the AZ, choose the public subnet that you created. 

AWS assigns the IPv4 address. 

**6.** Skip the Security Group page. 

© 2026 Nutanix, Inc. All rights reserved  | **34** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**7.** Select listeners: 

   - **a.** For Protocol, select **TCP** . 

   - **b.** For Port, select **9440** . 

   - **c.** For Target group, enter the target group name (for Prism access). 

**8.** Click **Create load balancer** . 

**9.** When the load balancer is active, use it with its DNS name. 

**10.** Confirm that port 9440 has access in the inbound policy of the user management and Prism Central security groups. 

**11.** Sign in to Prism: 

**12.** Copy the DNS name from the description of the network load balancer. 

**13.** Paste the DNS name into a browser and add **:9440** at the end of the URL (for 

example, `https://prod-lb-cluster-1152-1bb8b419e3e0b00a.elb.us.amazonaws:9440` ). 

## **Registering Prism Element With Prism Central** 

To register a Prism Element instance running in the destination cluster with a Prism Central instance running in the source cluster, follow these steps: 

**1.** Sign in to Prism Element. 

**2.** Click **Register or create new** and click **Connect** . 

**3.** Enter the Prism Central IP/FQDN, port, username, and password. 

**4.** Click **Connect** . 

## **Creating AWS Site-to-Site Virtual Private Network Connections** 

After you deploy the cluster, you can set up a virtual private network (VPN) gateway in AWS and create a site-to-site VPN connection. If you don't use Flow Virtual Networking with routed traffic (no-NAT), you can use a standard virtual network gateway in AWS. You need the transit gateway to set a static route for routed traffic to the Nutanix user virtual private cloud (VPC). If you use Flow Virtual Networking, you need one subnet for the bare-metal node, one for Prism Central, and one for Flow Virtual Networking. 

Nutanix Cloud Clusters (NC2) needs outbound access to the NC2 console, either through an internet gateway or an on-premises VPN with outbound access. Your Nutanix cluster can sit in a private subnet that can only be accessed from your VPN, limiting exposure to your environment. Ensure that redundant paths are available for outbound 

© 2026 Nutanix, Inc. All rights reserved  | **35** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

internet access because you use the NC2 console to add and remove AWS nodes based on the health of the system. 

## **Creating Customer Gateway in AWS** 

To create a customer gateway in AWS, follow these steps: 

**1.** Sign in to AWS. 

**2.** Navigate to the **VPC** dashboard, then to **Virtual Private Network (VPN)** . 

**3.** Click **Customer Gateways** , then click **Create Customer Gateway** . 

**4.** Name the customer gateway. 

**5.** For IP Address, enter the public IP address assigned to the firewall (on-premises gateway public IP address). 

**6.** For Border Gateway Protocol (BGP) Autonomous System Number (ASN), use the default ASN ( **65000** ). 

**7.** Don't select anything in Create Certificate ARN. 

**8.** Click **Create Customer Gateway** . 

**9.** Wait for the process to finish and the customer gateway to become available. 

## **Creating Virtual Private Gateway and Attaching to Virtual Private Cloud** 

To create a virtual private gateway and attach it to the virtual private cloud (VPC), follow these steps: 

**1.** Sign in to AWS. 

**2.** Navigate to the **VPC** dashboard, then to **Virtual Private Network (VPN)** . 

**3.** Click **Virtual Private Gateways** , then click **Create Virtual Private Gateway** . 

**4.** Name the virtual private gateway. 

**5.** For ASN, use the Amazon default ASN. 

**6.** Click **Create** . 

**7.** Wait for the process to finish and the virtual private gateway to change its status to available. 

**8.** Select the virtual private gateway that you just created and from the Action menu click **Attach** to attach it to your VPC. 

**9.** Wait for the virtual private gateway to change its status to attached. 

© 2026 Nutanix, Inc. All rights reserved  | **36** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **Creating Site-to-Site Virtual Private Network Connection** 

To create a site-to-site virtual private network (VPN) connection, follow these steps: 

**1.** Sign in to AWS. 

**2.** Navigate to the **VPC** dashboard, then to **Virtual Private Network (VPN)** . 

**3.** Click **Site-to-Site VPN Connections** , then click **Create VPN Connection** . 

**4.** Name the connection. 

**5.** Select the virtual private gateway you created. 

**6.** For Customer Gateway, select **Existing** and select the customer gateway you created. 

**7.** For Routing options, select **Static** . 

**8.** For the Static IP prefix, select the CIDR range assigned to your on-premises subnets. 

**9.** Use the default tunnel options. 

**10.** Click **Create** . 

**11.** Wait for the process to finish and the connection to change its status to available. 

## **Creating Static Route to On-Premises Subnets** 

To create a static route to the on-premises subnets, follow these steps: 

**1.** Sign in to AWS. 

**2.** Navigate to the **VPC** dashboard, then to **Route Tables** . 

**3.** Click the routing table associated with the bare-metal instance, CVM, and AHV network subnets. 

**4.** In the bare-metal instance, CVM, and AHV network subnets for the Nutanix cluster, find the routing table associated with those subnets. 

**5.** Add routes for the on-premises subnet and choose the virtual private gateway you created as the target. 

## **Creating On-Premises Virtual Private Network Connection** 

To create an on-premises virtual private network (VPN) connection, follow these steps: 

**1.** Sign in to AWS. 

**2.** Navigate to the **VPC** dashboard, then to **Virtual Private Network (VPN)** . 

© 2026 Nutanix, Inc. All rights reserved  | **37** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

**3.** Click **Site-to-Site VPN Connections** , then select the site-to-site VPN connection you created. 

**4.** Download the correct configuration: 

   - **a.** Select the firewall vendor name. 

   - **b.** For Platform, select **PA series** . 

   - **c.** Select the software version. 

**5.** Edit the downloaded config file to change the local IP address for ike-crypto-profile to the VPN IP address and change ike protocol to ike2. 

When the process finishes, you have a private tunnel between AWS and the on-premises datacenter. 

## **Configuring AWS Direct Connect** 

To configure AWS Direct Connect, follow these steps: 

**1.** Request an AWS Direct Connect dedicated connection. 

**2.** Create a virtual interface. 

**3.** Download the router configuration. 

**4.** Verify your virtual interface. 

© 2026 Nutanix, Inc. All rights reserved  | **38** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## 6. References and Resources for Nutanix Cloud Clusters on AWS Networking 

For more information on the individual components of the Nutanix Cloud Clusters (NC2) on AWS solution, see these references: 

- OVN Manual 

- Nutanix Cloud Clusters on AWS Tech Note 

- Nutanix Cloud Clusters on AWS Deployment and User Guide 

- Nutanix Cloud Clusters on AWS 

- AWS Direct Connect 

For a comprehensive and current list of supported regions and bare-metal types, see Supported Regions and Bare-Metal Instances in the Nutanix Cloud Clusters on AWS Deployment and User Guide. 

## **Nutanix Cloud Clusters on AWS Networking Best Practices** 

Best practice guidance for Nutanix Cloud Clusters (NC2) on AWS networking: 

© 2026 Nutanix, Inc. All rights reserved  | **39** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

- Network connectivity: 

   - › Use AWS Transit Gateway and AWS Direct Connect for connectivity to onpremises. 

You can use a virtual private network (VPN) connection until Direct Connect is available. 

- › To allow network traffic inspection to occur on-premises, configure a user-defined route as the default with the next hop of the on-premises router. 

- › Ensure that the required ports are allowed between devices by verifying the Requirements for NC2 on AWS. 

- › Use Flow Virtual Networking layer 2 stretch for VM migration purposes only. 

- › Configure Flow Virtual Networking with a highly available Flow Gateway and Border Gateway Protocol (BGP) equal-cost multipath (ECMP) routing with a minimum of two Flow Gateway VMs. 

This configuration allows the network routes in NC2 on AWS to automatically update and access the AWS services, corporate network, and the internet. It also removes single points of failure and decreases the number of user-defined routes you must configure. 

- › Create a virtual private cloud (VPC) and subnets, deploy a partner network virtual appliance, configure security groups, and set up user-defined routes to effectively route and inspect traffic using a network virtual appliance in AWS. 

- › Adjust the Flow Gateway network security group to allow communication between workloads on NC2 and the rest of AWS and on-premises. 

© 2026 Nutanix, Inc. All rights reserved  | **40** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

- IP address management: 

   - › Coordinate with the network administrator and architects to ensure that on-premises and NC2 on AWS don't have any overlapping or conflicting IP addresses. 

   - › Enable **Flow Gateway Requires IP Address Forwarding** in the AWS security policy. 

   - › Configure managed IP address pools to avoid address overlap and conflict with existing network DHCP pools. 

   - › Confirm IP address availability with the network administrator before you configure addresses for NC2 on AWS. 

   - › Assign enough CIDR subnets to AWS to cover Flow Gateways, BGP, AWS Route, and Prism Central virtual networks, as shown in the Required CIDR Subnets table. 

   - › Use no-NAT access to NC2 VMs to allow direct access for on-premises and native services. 

   - › Allocate sufficient floating IP addresses to accommodate NAT routing requirements for Flow Virtual Networking VPCs. 

- DNS resolution: 

   - › Use public DNS until you deploy NC2 and establish on-premises connectivity. 

   - › If you need hybrid AWS and on-premises DNS resolution, use AWS Route 53 Resolvers. 

   - › If the on-premises infrastructure provides all DNS resolution, don't use AWS Route 53 DNS Private Resolvers. 

When deciding whether to use AWS Route 53 Resolvers, consider how an on-premises communication issue might affect availability. 

For more detailed information, see DNS for On-Premises and AWS Resources. 

© 2026 Nutanix, Inc. All rights reserved  | **41** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **42** 

Nutanix Cloud Clusters on AWS Networking Best Practices 

## **List of Figures** 

Figure 1: Overview of NC2 on AWS..............................................................................................................5 Figure 2: Nutanix Flow Virtual Networking Component Overview............................................................... 10 Figure 3: Example VPC Configuration.........................................................................................................12 Figure 4: VPC Traffic Using NAT Subnet Overlay....................................................................................... 14 Figure 5: VPC Traffic Using No-NAT Subnet Overlay................................................................................. 16 Figure 6: Flow Virtual Networking on AWS Model.......................................................................................18 Figure 7: Example AWS VPC...................................................................................................................... 22 


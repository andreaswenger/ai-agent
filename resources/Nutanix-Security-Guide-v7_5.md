## **Security Guide** 

AOS Security 7.5 June 5, 2026 

## **Contents** 

**Audience & Purpose.........................................................................................7 Nutanix Security Infrastructure....................................................................... 8 Security Configuration Management Automation Implementation......................................................8 RHEL 8 STIG Implementation in Nutanix Controller VM.......................................................................8 Generating a STIG Compliance Report.................................................................................................. 9 Viewing Security Updates........................................................................................................................ 9 Security Management Using Prism Central................................................. 10 Identity and Access Management......................................................................................................... 10 Identity and Access Management Features.............................................................................. 10 Identity and Access Management Requirements..................................................................... 11 Identity and Access Management Considerations................................................................... 11 Identity Provider Configuration in Prism Central................................................................................ 12 Configuring Active Directory Authentication in Prism Central................................................14 Configuring OpenLDAP Authentication in Prism Central........................................................ 17 Configuring a SAML-Based Identity Provider in Prism Central...............................................19 Downloading the Prism Central Metadata File..........................................................................21 Common Access Card Authentication with Client Chain Certificate in Prism Central.......... 21 Configuring Common Access Card Authentication in Prism Central.....................................22 Enabling Certificate Revocation Checking using Online Certificate Status Protocol............23 Enabling Certificate Revocation Checking using Certificate Revocation List....................... 23 Updating ADFS When Using SAML Authentication..................................................................24 Deactivating Imported Users in Prism Central..........................................................................25 Reactivating Imported Users in Prism Central..........................................................................25 Local User Account in Prism Central...................................................................................................26 Creating a Local User Account in Prism Central......................................................................26 Editing a Local User Account in Prism Central........................................................................28 Changing a Local User Account Password in Prism Central..................................................28 Deactivating a Local User Account in Prism Central...............................................................29 Reactivating a Local User Account in Prism Central...............................................................29 Resetting a Local User Account Password Using nCLI...........................................................30 Service Accounts.................................................................................................................................... 31 Configuring a Service Account in Prism Central......................................................................31 Managing Service Account Keys................................................................................................32 Deactivating Service Accounts in Prism Central......................................................................33 Reactivating Service Accounts in Prism Central......................................................................34 Roles Management..................................................................................................................................34 Built-in Roles List.........................................................................................................................35 Creating a Custom Role.............................................................................................................. 38 Creating a Custom Role from an Existing Role........................................................................39 Adding an Authorization Policy to a Role.................................................................................41 Duplicating a Built-in Role.......................................................................................................... 42 Duplicating a Custom Role......................................................................................................... 43 Deleting a Role............................................................................................................................. 43 Updating a Custom Role............................................................................................................. 43 Viewing Role Permissions...........................................................................................................44** 

**ii** 

**Cluster or Category Independent Entities................................................................................. 44 Changes to Registered Operations and Custom Role Migration in Prism Central pc.7.3..... 45 Authorization Policies.............................................................................................................................57 Creating an Authorization Policy for Full Access.................................................................... 57 Creating an Authorization Policy for Configurable Access.....................................................58 Editing an Authorization Policy..................................................................................................59 Duplicating an Authorization Policy...........................................................................................60 Deleting an Authorization Policy................................................................................................60 Deleting a User or User Group..............................................................................................................61 Configuring Your Profile........................................................................................................................ 61 Updating Your Profile.................................................................................................................. 62 Changing Your Password............................................................................................................62 Centralized Password Management Using System Accounts............................................................62 Changing System Account Passwords......................................................................................63 SSL Certificate Management in Prism Central.................................................................................... 64 Generating a Self-Signed SSL Certificate with Subject Alternative Name..............................64 Generating a Certificate Signing Request with Subject Alternative Name for Submission to Certificate Authority......................................................................................67 Verifying the Certificate Generation Request............................................................................69 Troubleshooting the Certificate Generation Request...............................................................70 Importing a Self-Signed SSL Certificate in Prism Central....................................................... 70 Importing a CA-Signed SSL Certificate in Prism Central.........................................................73 Regenerating a Self-Signed Certificate in Prism Central......................................................... 75 Supported Key Configurations for SSL Certificates.................................................................75 Cluster Lockdown in Prism Central......................................................................................................76 Configuring Cluster Lockdown in Prism Central......................................................................76 Deleting a User SSH Key in Prism Central................................................................................77 Security Policies using Nutanix Flow...................................................................................................77 Data-in-Transit Encryption..................................................................................................................... 77 Enabling Data-in-Transit Encryption.......................................................................................... 78 Disabling Data-in-Transit Encryption......................................................................................... 78 Securing AHV VMs with a Virtual Trusted Platform Module.............................................................. 79 Virtual Trusted Platform Module Requirements........................................................................79 Virtual Trusted Platform Module Limitations............................................................................ 79 Configuring a VM with a Virtual Trusted Platform Module...................................................... 80 Enabling a Virtual Trusted Platform Module on an Existing VM............................................. 81 vTPM Integration with External KMS......................................................................................... 81 Disabling a Virtual Trusted Platform Module............................................................................ 82 Security Dashboard.................................................................................................................................83 Security Dashboard Requirements.............................................................................................83 Security Dashboard Widget in Prism Central Dashboard........................................................84 Security Dashboard Wizard.........................................................................................................84 Accessing the Security Dashboard............................................................................................85 Security Dashboard Details Page...............................................................................................85 Manually Refresh the Security Dashboard................................................................................88 Manually Upgrade the Security Dashboard Using LCM...........................................................89 External Key Management Server on Prism Central...........................................................................90 Configuring External Key Management Server.........................................................................90 Updating External Key Management Server Configuration..................................................... 92 Deleting External Key Management Server Configuration.......................................................92 Cloud Key Management Server on Prism Central...............................................................................93 Configuring Cloud Key Management Server.............................................................................93 Updating Cloud KMS Configuration...........................................................................................95 Deleting Cloud KMS Configuration............................................................................................ 96 Changing the KMS Type Using Prism Central..........................................................................96 Portal Proxy Connection in Prism Central...........................................................................................96** 

**iii** 

**Configuring the Portal Proxy Connection in Prism Central.....................................................97 Updating the Portal Proxy Connection in Prism Central......................................................... 97 Disabling the Portal Proxy Connection in Prism Central.........................................................98** 

**Security Management Using Prism Element................................................99 Authentication in Prism Element...........................................................................................................99 Configuring Active Directory Authentication in Prism Element..............................................99 Configuring OpenLDAP Authentication in Prism Element.....................................................102 Common Access Card Authentication with Client Chain Certificate in Prism Element.......104 Configuring Common Access Card Authentication in Prism Element................................. 105 Authentication Best Practices............................................................................................................. 106 Emergency Local Account Usage............................................................................................ 106 Modifying the Default CVM Password..................................................................................... 106 Setting Admin Session Timeout............................................................................................... 107 Role Mapping.........................................................................................................................................108 Configuring a Role Mapping..................................................................................................... 108 Editing a Role Mapping............................................................................................................. 109 Deleting a Role Mapping........................................................................................................... 110 Local User Account in Prism Element............................................................................................... 110 Creating a Local User Account in Prism Element.................................................................. 110 Backup Admin Role Capabilities.............................................................................................. 112 Editing a Local User Account in Prism Element.................................................................... 112 Deleting a Local User Account in Prism Element.................................................................. 112 Disabling Login Access to a Local User Account in Prism Element.................................... 113 Resetting a Local User Account Password Using nCLI.........................................................113 Configuring Your Profile...................................................................................................................... 114 Updating Your Profile................................................................................................................ 114 Changing Your Password..........................................................................................................115 Exporting an SSL Certificate for Third-Party Backup Applications.................................................115 SSL Certificate Management in Prism Element.................................................................................116 Generating a Self-signed SSL Certificate with Subject Alternative Name............................117 Generating a Certificate Signing Request with Subject Alternative Name for Submission to Certificate Authority....................................................................................119 Verifying the Certificate Generation Request..........................................................................122 Troubleshooting the Certificate Generation Request.............................................................122 Importing a Self-Signed SSL Certificate in Prism Element....................................................123 Importing CA-Signed SSL Certificate in Prism Element........................................................ 125 Regenerating Self-Signed Certificate in Prism Element.........................................................127 Supported Key Configurations for SSL Certificates...............................................................127 Cluster Lockdown in Prism Element.................................................................................................. 128 Configuring Cluster Lockdown in Prism Element.................................................................. 128 Deleting a User SSH Key in Prism Element............................................................................129 Data-at-Rest Encryption........................................................................................................................129 Data-at-Rest Encryption (SEDs)................................................................................................131 Data-at-Rest Encryption (Software Only).................................................................................140 Switching from SED-EKM to Software-LKM............................................................................151 Configuring Dual Encryption.....................................................................................................152 Backing up Keys........................................................................................................................ 152 Taking a Consolidated Backup of Keys (Prism Central)........................................................152 Importing Keys............................................................................................................................153 Data-at-Rest Encryption with Linux Unified Key Setup..........................................................154 Internationalization (i18n).....................................................................................................................154 Log Forwarding..................................................................................................................................... 155 Documenting the Log Fingerprint............................................................................................ 156 Admin Account Password Retry Lockout.......................................................................................... 156** 

**iv** 

**Portal Proxy Connection in Prism Element....................................................................................... 156 Configuring the Portal Proxy Connection in Prism Element................................................. 156 Updating the Portal Proxy Connection in Prism Element......................................................157 Disabling the Portal Proxy Connection in Prism Element..................................................... 157** 

**Hardening Instructions Using nCLI.............................................................159 AHV Security Hardening.......................................................................................................................159 CVM Security Hardening...................................................................................................................... 161 PCVM Security Hardening....................................................................................................................164 IP Set Based Firewall............................................................................................................................166 Eliminate the Default Passwords During a Cluster Creation.................... 167 Removing the Default SSH Password of a CVM............................................................................... 167 Enabling SSH Access to a CVM Using a Public Key........................................................................167 Enabling Cluster Lockdown Mode...................................................................................................... 168 Firewall Best Practices.................................................................................169 Securing Traffic Through Network Segmentation..................................... 170 Traffic Types In a Segmented Network.............................................................................................. 171 Segmented and Unsegmented Networks........................................................................................... 171 Supported Environment........................................................................................................................177 RDMA Requirements for Network Segmentation.............................................................................. 178 Prerequisites.......................................................................................................................................... 178 Limitations..............................................................................................................................................178 Cluster Services That Support Traffic Isolation................................................................................ 179 Unsupported Configurations for Network Segmentation................................................................. 179 Troubleshooting Tips............................................................................................................................179 Prepare the Network on an AHV Host................................................................................................179 Configuring the Network on Existing Nodes...........................................................................180 Configuring the Network on New Nodes................................................................................. 180 Network Segmentation for Traffic Types- Backplane and Management......................................... 181 Isolate the Backplane Traffic Logically Using VLAN-Based Segmentation..........................182 Isolating the Backplane Traffic Logically on an Existing Cluster......................................... 182 Physically Isolate the Backplane Traffic on an Existing Cluster...........................................183 Enabling Physical Backplane Segmentation on Hyper-V Using CLI.....................................189 Enabling Backplane Network Segmentation on a Mixed Hypervisor Cluster.......................190 Backplane IP Pool...................................................................................................................... 191 Configuring Backplane IP Pool.................................................................................................191 Reconfiguring the Backplane Network.................................................................................... 192 Update Backplane Port Groups................................................................................................193 Disabling Network Segmentation on an ESXi and Hyper-V Clusters....................................194 Disabling Network Segmentation on an AHV Cluster............................................................ 195 Service-Specific Traffic Isolation.........................................................................................................197 Isolating Service-Specific Traffic..............................................................................................197 Modifying Network Segmentation Configured for a Service................................................. 199 Disabling Network Segmentation Configured for a Service.................................................. 199 Deleting a vNIC Configured for a Service............................................................................... 199 Service-Specific Settings and Configurations.........................................................................200 Remote Direct Memory Access over Converged Ethernet............................................................... 204 Priority-Based Flow Control without Zero-Touch RoCE Specifications............................... 206 NIC Compatibility Matrix for RDMA Features..........................................................................206** 

**v** 

**Isolating the Backplane Traffic on an Existing RDMA Cluster.............................................. 209 Support for iSCSI Extensions for RDMA................................................................................. 211 Customizing IP Addresses for each CVM and Host..........................................................................218 Network Segmentation during Cluster Expansion............................................................................ 220 Network Segmentation-Related Changes During an AOS Upgrade.................................................220 Accessing a List of Open Source Software Running on a Cluster...........221 Copyright........................................................................................................222** 

## **AUDIENCE & PURPOSE** 

Understand the intended audience and scope of the Nutanix Security Guide. 

This document is written for security-minded people who are responsible for architecting, managing, and supporting infrastructures, especially those who want to address security without adding more human resources or additional processes to their datacenters. 

The Nutanix Security Guide offers an overview of the security development life cycle (SecDL) and host of security features that Nutanix supports. It also demonstrates how Nutanix complies with security regulations to streamline infrastructure security management. In addition, the guide covers the technical requirements that are site-specific or compliance standards that users must adhere to that are not enabled by default. 

From AOS 6.8 version onward, the underlying operating system upgrades to RHEL 8, which does not support TCP Wrapper. Therefore, AOS version 6.8 or later versions do not include TCP Wrapper. For any assistance in configuring an alternative to TCP Wrapper in AOS 6.8 or later version, contact Nutanix Support. 

**Note:** The hardening of the guest OS or any applications running on top of the Nutanix infrastructure is beyond the scope of this guide. Nutanix recommends that you refer to the documentation of the products that you deployed in your Nutanix environment. 

AOS Security | Audience & Purpose | **7** 

## **NUTANIX SECURITY INFRASTRUCTURE** 

Nutanix implements a holistic approach to security with a secure platform, extensive automation, and a robust partner ecosystem. 

Nutanix integrates security into every stage of product development through its security development life cycle (SecDL), rather than treating it as an afterthought. Nutanix makes the SecDL a foundational part of product design. The strong pervasive culture and processes built around security harden the enterprise cloud platform and eliminate zero-day vulnerabilities. The efficient one-click operations and self-healing security models enable automation to maintain security in an always-on hyperconverged solution. 

Nutanix conforms to RHEL 8 Security Technical Implementation Guides (STIGs) that use machine-readable code to automate compliance against rigorous common standards, because traditional manual configuration and checks cannot keep up with the ever-growing list of security requirements. Nutanix Security Configuration Management Automation (SCMA) allows you to quickly and continually assess and remediate your platform to ensure that it meets or exceeds all regulatory requirements. Nutanix standardized the security profile of the Controller VM to a security compliance baseline that meets or exceeds the standard high-governance requirements. 

In United States, vendors most commonly use the following references to build products according to the following set of technical requirements: 

- The National Institute of Standards and Technology Special Publications Security and Privacy Controls for Federal Information Systems and Organizations (NIST 800.53) 

- The US Department of Defense Information Systems Agency (DISA) Security Technical Implementation Guides (STIG) 

## **Security Configuration Management Automation Implementation** 

The Nutanix platform and all its products leverage the Security Configuration Management Automation (SCMA) framework to continuously inspect services for deviations from established security policies. 

For both Nutanix storage and AHV, Nutanix uses SCMA to check multiple security entities, including those defined in relevant STIGs (Security Technical Implementation Guides). The system automatically reports log inconsistencies and reverts them to the baseline. The SCMA has the lowest system priority within the virtual storage controller, ensuring that security checks do not interfere with platform performance. 

**Note:** You can modify only the SCMA schedule. The system runs the AIDE schedule on a fixed weekly interval. For more information on how to change the SCMA schedule for AHV or Controller VM (CVM), see the Security Hardening on page 86 or Hardening Instructions Using nCLI on page 159. 

## **RHEL 8 STIG Implementation in Nutanix Controller VM** 

Nutanix Controller VMs take advantage of SaltStack and Security Configuration Management Automation (SCMA) to self-heal any deviation from the security baseline configuration. 

If any component of the system deviates from the security baseline,the component is set back to the supported security settings without any intervention. Nutanix set up the Controller VM to support STIG compliance with the RHEL 8 STIG as published by DISA. STIG rules cover various security configurations, including the boot loader, packages, file system, boot process, services, file ownership, authentication, kernel, and logging. 

For example, STIG rules for authentication prohibit direct root login, lock system accounts other than root, enforce several password maintenance details, cautiously configure SSH, enable screen-locking, configure user shell defaults, and display warning banners. 

For more information on Nutanix STIGs, see Security Dashboard STIG Guidance Reference and Security Dashboard on page 83. 

AOS Security | Nutanix Security Infrastructure | **8** 

## **Generating a STIG Compliance Report** 

You can leverage the OpenSCAP report to ensure that the Controller VM is STIG compliant. 

## **About this task** 

To generate a STIG compliance report, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: `$ ssh nutanix@` ~~SCS~~ _`cvm_ip_address`_ 

**2.** Create an output directory for the STIG report: 

   - `nutanix@cvm$ mkdir /tmp/stig_report` ~~ee~~ 

**3.** Generate the STIG compliance report: 

`nutanix@cvm$ sudo python3 -B /home/nutanix/config/security/stig_scripts/ stig_scan_driver.py /home/nutanix/config/security/stig_scripts/ U_RHEL_8_V1R11_STIG_SCAP_1-2_Benchmark.xml /tmp/stig_report/` 

The STIG compliance report is generated in the /tmp/stig_report directory. 

**Note:** Depending on the size of your environment, the STIG report generation script runs for approximately 60 to 90 minutes to completion. 

## **Viewing Security Updates** 

Nutanix Security Advisories provide detailed information on the available security fixes and updates, including the vulnerability description and affected product or version. 

## **About this task** 

To view the list of security advisories, follow these steps: 

## **Procedure** 

**1.** Log in to Nutanix Support Portal . 

**2.** From the Entities Menu , select **Security** > **Advisories** . The system displays the list of security advisories. 

AOS Security | Nutanix Security Infrastructure | **9** 

## **SECURITY MANAGEMENT USING PRISM CENTRAL** 

Prism Central provides several mechanisms and features to enforce security of your multicluster environment. 

Security in Prism Central is managed through a comprehensive identity and access management (IAM) framework that spans multiple clusters. IAM supports local accounts, Active Directory, OpenLDAP, SAML identity providers, and common access card (CAC) authentication. You can configure roles, authorization policies, and service accounts with precise permissions. Additional capabilities include session timeouts, certificate-based authentication, and automatic role migration during upgrades, ensuring consistent and controlled access across the environment. 

## **Identity and Access Management** 

Manage fine-grained access control in Prism Central using identity and access management (IAM). 

IAM in Prism Central provides authentication and authorization to control access at a granular level. IAM includes the following components: 

- **Identity** : A user or user group that requires access. You can configure authentication for local users, directory services and a wide selection of identity providers including SAML-based identity providers. For more information, see Identity Provider Configuration in Prism Central on page 12. 

- **Entity** : A resource such as a virtual machine or disk that the identity interacts with. 

- **Operation** : An action performed on an entity, such as creating a VM. 

- **Role** : A set of permissions that define what operations an identity can perform. For more information, see Roles Management on page 34. 

- **Authorization Policy** : A rule that enforces granular role-based access control (RBAC) by binding identities, roles, and entities. For more information, see Authorization Policies on page 57. 

Prism Central enables IAM by default. When you enable Microservices Infrastructure either manually or by default, Prism Central automatically enables IAM. For more information, see Microservices Infrastructure . 

## **Identity and Access Management Features** 

Understand identity and access management (IAM) features in Prism Central: 

- **Highly Scalable Architecture** : Based on Kubernetes open source platform, IAM uses independent pods for authentication, authorization, and data storage or replication, and supports rolling upgrades to ensure zero downtime. 

- **Secure by Design** : IAM uses mutual TLS (mTLS) to secure communication between components. The Microservices Infrastructure in Prism Central provisions the certificates. 

AOS Security | Security Management Using Prism Central | **10** 

- **Support for Multiple SAML Identity Providers (IDPs)** : IAM supports SAML-based authentication with the following IDPs: 

   - Microsoft Active Directory Federation Services (ADFS) 

   - Microsoft Entra ID (formerly Azure Active Directory) 

**Note:** IAM supports only SAML-based authentication with Microsoft Entra ID. IAM does not support LDAPbased authentication with Microsoft Entra ID. 

- Okta 

- PingOne 

- Shibboleth 

- Keycloak 

Users can log in only from the Prism Central web console. IAM does not support IDP-initiated authentication workflows from an identity provider (IDP) web page or portal. 

Nutanix supports SAML 2.0 compliant IDPs. 

## **Identity and Access Management Requirements** 

Understand software and infrastructure requirements of identity and access management (IAM) feature: 

- For IAM specific minimum software support and requirements, see the Prism Central release notes . 

- For Prism Central, Prism Element Clusters, and Microservices infrastructure requirements, see the Microservices Infrastructure in _Prism Central Infrastructure Guide_ . 

## **Identity and Access Management Considerations** 

Understand the considerations of the identity and access management (IAM) feature: 

- After you upgrade to pc.2022.9 or later, log in to Prism Central for the first time using your Nutanix local account default admin credentials. You can use Active Directory (AD) account for subsequent logins. 

- IAM automatically migrates existing authentication and authorization settings, including Common Access Card (CAC) configurations. 

- IAM does not support signed single logout (SLO), leading to an error on the SAML Microsoft Active Directory Federation Services (ADFS) page when you logout of Prism Central. 

- If you previously enabled Microservices Infrastructure and IAM, both the services remain enabled after upgrading Prism Central, provided the cluster meets all requirements. Contact Nutanix Support for custom configurations. 

**Note:** After upgrade to pc.2022.9 if the Security Assertion Markup Language (SAML) IDP is configured, you need to download the Prism Central metadata and re-configure the SAML IDP to recognize Prism Central as the service provider. See Updating ADFS When Using SAML Authentication on page 24 to create the required rules for ADFS. 

- Maximum user session lifetime is 8 hours with a 15-minute idle timeout. After 15 minutes of idle timeout, IAM logs out the user and requires re-authentication. 

- IAM supports client authentication only when CAC authentication is enabled. Ensure port 9441 is open in the firewall for CAC client authentication. 

- IAM is available only on on-premise Prism Central deployments running on AOS clusters with AHV or ESXi. Other hypervisors are not supported. 

AOS Security | Security Management Using Prism Central | **11** 

- You cannot delete local or directory users from the Prism Central web console. However, you can disable and reenable local or directory users. 

**Note:** Use nCLI to delete Prism Central users or user groups. For more information, see Deleting a User or User Group on page 61. 

- In directory services such as Active Directory or OpenLDAP, IAM group-based authentication and authorization depend on the group’s full distinguished name (DN). Changing the DN by renaming or moving the group causes the system to treat it as a different group, which can result in role mapping and access issues. 

- When you change the scope in an authorization policy to include or exclude individual entities, it takes approximately five minutes for the filtered items to update and reflect on the entity page. During this time, the entities might display incorrectly. 

- After you upgrade to pc.2024.1 or later, IAM migrates built-in roles user admin and viewer to new authorization policies with similar permissions. If users were assigned these roles, IAM automatically migrates the role mapping configuration as new authorization policies. 

**Note:** IAM automatically migrates the existing role mapping data into equivalent authorization policies, that are renamed using the format `Role_Mapping_<Role_Name>_<timestamp>` . These migrated policies are available on the Authorization Policies page in Prism Central. 

- The **Role Details** page view does not display the associated authorization policies and users. 

- In an authorization policy configuration, if you select **All Entity Types** for the entity filter, scope of the authorization policy is limited to specific clusters or categories. The authorization policy does not grant access to Prism Central entities that do not have cluster-scoped attributes. To include such entities, explicitly allow access for those entities in the authorization policy configuration. 

For more information on list of entities that do not support cluster or category-based scoping, see Cluster or Category Independent Entities on page 44. 

- Starting from pc.7.3, IAM operations include several changes. For more information, see Changes to Registered Operations and Custom Role Migration in Prism Central pc.7.3 on page 45. 

## **Identity Provider Configuration in Prism Central** 

Prism Central supports the following user authentication methods: 

- Active Directory 

- OpenLDAP 

- SAML identity provider 

- Client Access Card (CAC) 

Your Prism Central login experience depends on the configured identity provider. For example, if you configure both local user account and AD authentication, the Prism Central login page defaults to AD authentication. 

AOS Security | Security Management Using Prism Central | **12** 

**Figure 1: Sample Prism Central Login Page with Active Directory And Local User Authentication** 

For example, if you configure multiple identity providers such as SAML authentication (Shibboleth), AD (AD2), and local user account, the Prism Central login page displays login fields for each. 

AOS Security | Security Management Using Prism Central | **13** 

**Figure 2: Sample Prism Central Login Page with SAML Identity Provider, Active Directory , and Local User Authentication** 

## **Configuring Active Directory Authentication in Prism Central** 

Configure Active Directory (AD) as the identity provider in Prism Central to enable AD credential-based login for users. 

## **Before you begin** 

Before you configure AD in Prism Central, consider the following points: 

- The service account for AD must have full read permissions on the directory service. 

- Users with the User must change password at next logon attribute enabled cannot authenticate to Prism Central. Ensure that users change their password on a domain workstation before logging into Prism Central. 

- If SSL is enabled on the AD server, ensure that Prism Central's firewall allows access to the appropriate port. 

- Use of the Protected Users group is not supported for Prism Central authentication. For more information, see _Guidance about how to configure protected accounts_ on Microsoft documentation website. 

- Prism Central supports AD with LDAP v2 on Windows Server 2012 R2, Windows Server 2016, and Windows Server 2019. 

**Important:** Prism Central does not support insecure SSLv2 and SSLv3 ciphers. To prevent the SSL fallback situation and being denied access to Prism Central, configure the following settings in your browser: 

- Disable SSLv2 and SSLv3 

- Enable TLS 

AOS Security | Security Management Using Prism Central | **14** 

## **About this task** 

To configure AD in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **IdP Configuration** tab. 

**4.** Click **Add Identity Provider** > **Active Directory/OpenLDAP** . 

The **Configure Directory** window appears. 

AOS Security | Security Management Using Prism Central | **15** 

## **5.** In the **Configure Directory** window, enter the following information: 

- **Directory Type** : From the dropdown list, select **Active Directory** . 

- **Name** : Enter a name to identify the directory service on Prism Central. 

- **Domain** : Enter the domain or sub-domain of the directory service. 

For example nutanix.com 

- **Directory URL** : Enter the URL address of the directory in ldap://host:ldap_port_num format. 

host: The host value is either the IP address or fully qualified domain name. 

ldap_port_num: The default LDAP port number is 389. Nutanix also supports ports 636 for LDAPS, 3268 and 3269 for LDAP/S global catalog servers. 

Examples: 

- Default LDAP (Port 389), when the configuration is single domain, single forest, and not using SSL: 

`ldap://ldap.example.com or ldap://10.1.4.111:389` 

- LDAPS (Port 636), when the configuration is single domain, single forest, and using SSL: 

`ldaps://ldap.example.com or ldaps://10.1.4.111:636` 

LDAPS requires that all AD Domain Controllers have properly installed SSL certificates. 

- LDAP/S Global Catalog non-SSL (Port 3268), when the configuration is multiple domain, single forest, and not using SSL: 

`ldap://globalcatalog.example.com:3268 or ldap://10.1.4.111:3268` 

- LDAP/S Global Catalog SSL/TLS encrypted (Port 3269), when the configuration is multiple domain, single forest, and using SSL: 

`ldaps://globalcatalog.example.com:3269 or ldaps://10.1.4.111:3269` 

When the directory URL is set to the domain FQDN, LDAP authentication can fail if any directory server becomes unavailable. For high availability (HA), configure an LDAP HA cluster and specify the cluster virtual IP (VIP) address in the directory URL instead of the domain FQDN. For more information, see KB-12568 . 

When constructing your LDAP/S URL to use a global catalog server, ensure that the domain control IP address or name belongs to a global catalog server within the domain that you are configuring. Otherwise, queries over port 3268 or 3269 might fail. 

Cross-forest trust between multiple AD forests is not supported. 

LDAPS support does not require custom certificates or certificate trust import. 

For the complete list of required ports, see Port Reference . 

- **Search Type** : From the dropdown list, select either **Non Recursive (Default)** or **Recursive** . 

When users are present in the first level group, select **Non Recursive (Default)** . 

When you have multi level group assignments, select **Recursive** . A recursive search performs a multi level or nested search during authentication, which might cause slowness. If you experience slowness, select **NonRecursive (Default)** . 

- **Service Account Username** : Enter the service account user name in _`user_name@domain.com`_ format. 

**Note:** A service account is created to run only a particular service or application with the credentials specified for the account. According to the requirement of the service or application, the administrator can limit access 

AOS Security | Security Management Using Prism Central | **16** 

to the service account. The service account is under the Managed Service Accounts in the AD server. An application or service uses the service account to interact with the operating system. 

- **Service Account Password** : Enter the service account password. 

**Note:** Be sure to update the service account credentials if the password changes or you use a different service account. User authentication and authorization requests fail if the service account password is updated in Active Directory but not in Prism Central. 

**6.** Click **Save** . 

## **What to do next** 

By default, permissions are not granted to the AD users. To grant permissions, you must create an authorization policy. For more information, see Authorization Policies on page 57. 

## **Configuring OpenLDAP Authentication in Prism Central** 

Configure OpenLDAP as the identity provider in Prism Central to enable OpenLDAP credential-based login for users. 

## **Before you begin** 

Before you configure OpenLDAP in Prism Central, consider the following points: 

- The service account for OpenLDAP must have full read permission on the directory service. 

- Prism Central uses a service account to query OpenLDAP directories for user information and does not currently support certificate-based authentication with the OpenLDAP directory. 

- The values for the **User Object Class** , **User Search Base** , **Username Attribute** , **Group Object Class** , **Group Search Base** , **Group Member Attribute** , and **Group Member Attribute** fields depend on your OpenLDAP configuration. 

- Prism Central supports OpenLDAP with LDAP v2. 

**Important:** Prism Central does not support insecure SSLv2 and SSLv3 ciphers. To prevent the SSL fallback situation and being denied access to Prism Central, configure the following settings in your browser: 

- Disable SSLv2 and SSLv3 

- Enable TLS 

## **About this task** 

To configure OpenLDAP in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **IdP Configuration** tab. 

**4.** Click **Add Identity Provider** > **Active Directory/OpenLDAP** . 

The **Configure Directory** window appears. 

AOS Security | Security Management Using Prism Central | **17** 

**5.** In the **Configure Directory** window, enter the following information: 

   - **Directory Type** : From the dropdown list, select **OpenLDAP** . 

   - **Name** : Enter a name to identify the directory service on Prism Central. 

   - **Domain** : Enter the domain or sub-domain of the directory service. 

For example nutanix.com 

- **Directory URL** : Enter the URL address of the directory in ldap://host:ldap_port_num format. 

host: The host value is either the IP address or fully qualified domain name. 

ldap_port_num: The default LDAP port number is 389. Nutanix also supports ports 636 for LDAPS. Examples: 

- Default LDAP Port (389), when the configuration is single domain, single forest, and not using SSL: 

`ldap://ldap.example.com or ldap://10.1.4.111:389` 

- LDAPS (Port 636), when the configuration is single domain, single forest, and using SSL: 

`ldaps://ldap.example.com or ldaps://10.1.4.111:636` 

When the directory URL is set to the domain FQDN, LDAP authentication can fail if any directory server becomes unavailable. For high availability (HA), configure an LDAP HA cluster and specify the cluster virtual IP (VIP) address in the directory URL instead of the domain FQDN. For more information, see KB-12568 . 

LDAPS support does not require custom certificates or certificate trust import. 

For the complete list of required ports, see Port Reference . 

- **Search Type** : From the dropdown list, select either **Non Recursive(Default)** or **Recursive** . 

When users are present in the first level group, you can select **Non Recursive(Default)** . 

When you have multi level group assignments, you can select **Recursive** . A recursive search performs a multi level or nested search during authentication, which might cause slowness. If you experience slowness, select **Non-Recursive (Default)** . 

- **User Object Class** : Enter the object class of users in the directory service. 

For example: user, person, inetOrgPerson, organizationalPerson, or posixAccount 

- **User Search Base** : Enter the base Distinguished Name (DN) to search for users. 

The base DN must include the domain components, each represented in the format of _`dc=domain_component`_ , separated by commas. 

For example, if your domain name is nutanix.com, then base DN includes: 

ou=users,dc=nutanix,dc=com 

cn=users,dc=nutanix,dc=com 

- **Username Attribute** : Enter the attribute of the user object class which uniquely identifies a user in the directory. 

For example: uid 

- **Group Object Class** : Enter the object class of groups in the directory service. 

For example: posixGroup or groupOfNames 

- **Group Search Base** : Enter the base DN to search for user groups. 

AOS Security | Security Management Using Prism Central | **18** 

The base DN must include the domain components, each represented in the format of _`dc=domain_component`_ ee , separated by commas. 

For example, if your domain name is nutanix.com, then base DN includes: 

cn=groups,dc=nutanix,dc=com 

ou=groups,dc=nutanix,dc=com 

- **Group Member Attribute** : Enter the attribute of the group object that holds the user association. 

For example: member or memberUid 

- **Group Member Attribute Value** : Enter the attribute of the user object that is used to configure the membership in the group object. 

For example: uid 

- **Service Account Username** : Enter the service account user name in cn=username, dc=example, dc=com format. 

For example cn=username, dc=nutanix, dc=com 

**Note:** A service account is created to run only a particular service or application with the credentials specified for the account. According to the requirement of the service or application, the administrator can limit access to the service account. The service account is under the Managed Service Accounts in the OpenLDAP server. An application or service uses the service account to interact with the operating system. 

- **Service Account Password** : Enter the service account password. 

**Note:** Be sure to update the service account credentials if the service account password changes or a different service account is used. User authentication and authorization requests fail if the service account password is updated in OpenLDAP but not in Prism Central. 

**6.** Click **Save** . 

## **What to do next** 

By default, permissions are not granted to the OpenLDAP users. To grant permissions, you must create an authorization policy. For more information, see Authorization Policies on page 57. 

## **Configuring a SAML-Based Identity Provider in Prism Central** 

Configure a SAML-based identity provider in Prism Central. 

## **Before you begin** 

Before you configure a SAML-based identity provider in Prism Central, consider the following points: 

- You must download the identity provider's XML metadata file from their website and upload it to Prism Central when configuring SAML authentication. 

- An identity provider (typically a server or other computer) is the system that provides authentication through a SAML request. There are various implementations that can provide authentication services in line with the SAML standard. 

- You can specify other tested standard-compliant IDPs in addition to Microsoft Entra ID (formerly Azure Active Directory). For more information, see the Prism Central Release Notes . 

- Prism Central supports SAML 2.0-compliant identity providers for authentication. To enforce additional security, you can configure multi-factor authentication (MFA) within your identity provider. When you configure MFA, the identity provider issues a SAML token after authentication, which provides single sign-on (SSO) access to Prism Central. 

AOS Security | Security Management Using Prism Central | **19** 

- You must configure your identity provider to return the a _`NameID`_ attribute in a SAML response. Prism Central uses the _`NameID`_ a attribute for role mapping. 

- Prism Central only supports redirect binding for identity provider sign on URLs. You must configure and include the redirect binding in identity provider's metadata. 

## **About this task** 

To configure a SAML-based identity provider in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **IdP Configuration** tab. 

**4.** Click **Add Identity Provider** > **SAML Identity Provider** . 

**5.** In the **Configure Identity Provider** window, enter the following information: 

   - **Configuration Name** : Enter a name for the identity provider. 

   - **Username Attribute** : Enter a username attribute of the identity provider configuration. 

   - **Email Attribute** : Enter an email attribute of the identity provider configuration. 

   - **Redirect URL (Optional)** : Enter the post-login redirection URL. This value is required for SAML integration in the following cases: 

      - The Prism Central instance is deployed behind a reverse proxy. 

      - The hostname of the Prism Central instance changes, such as during a transition from an IP address to a fully qualified domain name (FQDN). 

**Note:** Note: After configuring or updating the redirect URL, you must download the updated metadata file from the Prism Central instance and import it into your identity provider to re-establish trust. For more information on downloading metadata, see Downloading the Prism Central Metadata File on page 21. 

- **Group Attribute Name (Optional)** : Enter the group attribute name. 

**Note:** Ensure that the name matches the group attribute name provided in the IDP configuration. 

- **Group Attribute Delimiter (Optional)** : Enter a delimiter that must be used when multiple groups are selected for the group attribute. 

- **Import Metadata** : Upload the metadata file that contains the identity provider information. 

**Note:** For information about configuring Entra ID integration from both Prism Central and Entra ID Administration, see KB-16526 . 

**6.** Click **Save** . 

AOS Security | Security Management Using Prism Central | **20** 

## **What to do next** 

- You must download the Prism Central metadata file and configure your SAML-based identity provider to recognize Prism Central as the service provider. For more information, see Downloading the Prism Central Metadata File on page 21. 

- By default, permissions are not granted to the SAML IDP users. To grant permissions, you must create an authorization policy. For more information, see Authorization Policies on page 57. 

## **Downloading the Prism Central Metadata File** 

Download the Prism Central metadata file to configure the callback URL in the SAML-based identity provider's website. 

## **About this task** 

To download the Prism Central metadata file, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **IdP Configuration** tab. 

**4.** Select the identity provider checkbox from the list. 

**5.** Click **Actions** and select **Download Metadata** . 

A metadata file that describes the Prism Central attributes is downloaded in XML format. 

**6.** On the identity provider's website, upload the metadata file. 

**7.** (Optional) If your identity provider asks you to manually enter the Prism Central callback URL, you can find it in the `AssertionConsumerService` tag with attribute `Location` in the downloaded XML metadata file. 

The following sample XML metadata file shows `AssertionConsumerService` tag with attribute `Location` : 

`<md:AssertionConsumerService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://10.46.153.235:9440/api/iam/authn/callback" index="1" />` 

## **Common Access Card Authentication with Client Chain Certificate in Prism Central** 

Prism Central uses client chain certificate authentication to enable the Common Access Card (CAC) login. Client chain certificate authentication is a certificate-based authentication mechanism where the user is prompted to select a certificate for login instead of password. 

Client chain certificate authentication provides enhanced security by requiring users to present a digital certificate instead of a password, reducing the risk of password-based attacks. In a one-way authentication process, Prism Central presents a certificate, and the user's browser verifies it. When the client chain certificate authentication is enabled, the process becomes a two-way authentication. During authentication, Prism Central also verifies the user's identity with a valid digital certificate to ensure that only authorized users can access the cluster. A user must provide a valid certificate when accessing Prism Central. 

The CAC authentication has the following workflow: 

AOS Security | Security Management Using Prism Central | **21** 

- Certificate Installation: A user installs the certificate on their local machine. 

**Note:** The Certificate Authority (CA) must be the same for both the client chain certificate and the certificate on the local machine. 

- Certificate Presentation and Chain Verification: When the user accesses Prism Central, the system prompts them to select their CAC certificate. Prism Central verifies the presented certificate against its trusted CA chain. The chain establishes trust by linking the client certificate to a trusted root CA. This chain verification step confirms the certificate's validity. 

- EDIPI Extraction and Account Verification: Prism Central extracts the Electronically Data Interchange Personal Identifier (EDIPI) from the validated CAC certificate. It then queries Active Directory (AD) to verify that the user has a valid and active account associated with this EDIPI. 

- Login: Prism Central logs in the user based on their validated certificate and verified Active Directory account. 

Before you configure the CAC, consider the following points: 

- Prism Central supports AD as directory service for CAC. OpenLDAP is not supported. 

- When you authenticate Prism Central with client chain certificate, the `Subject name` field must be present. The subject name should match the UserPrincipalName (UPN) in the AD. The UPN is a username with domain address. For example user1@nutanix.com. 

- If you map a Prism role to a CAC user and not to an AD group or organizational unit to which the user belongs, specify the EDIPI UPN or UPN of that user in the role mapping. A user who presents a CAC with a valid certificate is mapped to a role and taken directly to the web console home page. The web console login page is not displayed. 

- By default, users with an admin role can log in to the Prism Central console using CAC authentication. For other LDAP user roles, ensure the correct role mappings or role assignments are configured to enable CAC authentication. 

- Client chain certificate must be PEM encoded. 

- After logging in to Prism Central using CAC authentication, log out by closing your browser. For added security, consider clearing your browser's cookies. 

- Nutanix recommends configuring a certificate revocation checking (OCSP or CRL), which IAMv2 uses to perform a secondary check, verifying certificates are not marked invalid during certificate validation. 

## **Configuring Common Access Card Authentication in Prism Central** 

Configure Common Access Card (CAC) authentication in Prism Central. 

## **Before you begin** 

Ensure that port 9441 is open in your firewall. After enabling the CAC authentication, the CAC login redirects the browser to use port 9441. 

## **About this task** 

To configure the CAC authentication in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

AOS Security | Security Management Using Prism Central | **22** 

**3.** Select the **IdP Configuration** tab. 

**4.** Click **Add Identity Provider** > **Common Access Card** . 

The **Configure Client Chain Certificate** window appears. 

**5.** Click **Upload** and select a client chain certificate from your local machine. 

**Note:** Client chain certificate must be PEM encoded. 

**6.** Select the **Enable CAC Authentication** checkbox. 

**7.** From the **Select Active Directory** dropdown list, select the Active Directory. 

**8.** Click **Save** . 

## **Enabling Certificate Revocation Checking using Online Certificate Status Protocol** 

Nutanix recommends Online Certificate Status Protocol (OCSP) as the standard method for checking certificate revocation in client chain certificate authentication. 

## **About this task** 

To enable OCSP for client chain certificate authentication, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: `$ ssh nutanix@` ~~CSS~~ _`cvm_ip_address`_ 

**2.** Set the OCSP responder URL: 

> `nutanix@cvm$ ncli authconfig set-certificate-revocation set-ocsp-responder=` ~~CSS~~ _`ocsp-url`_ 

> _`ocsp-url`_ a : Specify the location of the OCSP responder. 

> _`ocsp-url`_ a : Specify the location of the OCSP responder. Verify that OCSP checking is enabled: 

> `nutanix@cvm$ ncli authconfig get-client-authentication-config` ~~CSS~~ Output: 

**3.** Verify that OCSP checking is enabled: 

`Auth Config Status: true File Name: ca.cert.pem OCSP Responder URI: http://ocsp-responder-url` 

## **Enabling Certificate Revocation Checking using Certificate Revocation List** 

Enable the Certificate Revocation List (CRL) certificate revocation checking method. 

## **About this task** 

**Note:** Nutanix recommends Online Certificate Status Protocol (OSCP) as the standard method for checking certificate revocation in client authentication. 

To enable certificate revocation checking using CRL for client authentication, follow these steps: 

AOS Security | Security Management Using Prism Central | **23** 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` user: 

`$ ssh nutanix@` _`cvm_ip_address`_ 

**2.** Specify all the CRLs required for certificate validation: 

`nutanix@cvm$ ncli authconfig set-certificate-revocation set-crl-uri=` _`uri-1,uri-2`_ `/ set-crl-refresh-interval=` _`refresh-interval-in-seconds`_ `set-crl-expirationinterval=` _`expiration-interval-in-seconds`_ 

where: 

- _`set-crl-refresh-interval`_ : Specify the CRL refresh interval in seconds to periodically update the CRLs. The CRL refresh interval is common for the entire list of CRL distribution points. The default value is 86,400 seconds (1 day). 

- _`set-crl-expiration-interval`_ : Specify the duration in seconds for which the periodically updated CRLs are cached in-memory. The CRLs expire after the specified duration if a particular CRL distribution point is not reachable. The CRL expiration interval is common for the entire list of CRL distribution points. The default value is 604,800 seconds (7 days). 

**Note:** The URIs must be percent-encoded and comma-separated. 

## **Updating ADFS When Using SAML Authentication** 

With Nutanix IAM, to maintain compatibility with new and existing IDP/SAML authentication configurations, update your Active Directory Federated Services (ADFS) configuration - specifically the Prism Central Relying Party Trust settings. For these configurations, you are using SAML as the open standard for exchanging authentication and authorization data between ADFS as the identity provider (IDP) and Prism Central as the service provider. See the Microsoft Active Directory Federation Services documentation for details. 

## **About this task** 

In your ADFS Server configuration, update the Prism Central Relying Party Trust settings by creating claim rules to send the selected LDAP attribute as the SAML NameID in email address format. For example, map the User Principal Name to NameID in the SAML assertion claims. 

As an example, this topic uses UPN as the LDAP attribute to map. You could also map the email address attribute to NameID. See the _Microsoft Active Directory Federation Service_ s documentation for details about creating a claims aware Relying Party Trust and claims rules. 

## **Procedure** 

**1.** In the Relying Party Trust for Prism Central, configure a claims issuance policy with two rules. 

   - a. One rule based on the **Send LDAP Attributes as Claims** template. 

   - b. One rule based on the **Transform an Incoming Claim** template 

**2.** For the rule using the **Send LDAP Attributes as Claims** template, select the **LDAP Attribute** as **UserPrincipal-Name** and set **Outgoing Claim Type** to **UPN** . 

   - For User group configuration using the **Send LDAP Attributes as Claims** template, select the **LDAP Attribute** as **Token-Groups - Unqualified-Names** and set **Outgoing Claim Type** to **Group** . 

AOS Security | Security Management Using Prism Central | **24** 

**3.** For the rule using the **Transform an Incoming Claim** template: 

   - a. Set **Incoming claim type** to **UPN** . 

   - b. Set the **Outgoing claim type** to **Name ID** . 

   - c. Set the **Outgoing name ID format** to **Email** . 

   - d. Select **Pass through all claim values** . 

## **Deactivating Imported Users in Prism Central** 

Deactivate imported users in Prism Central. 

## **About this task** 

To deactivate an imported user in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select **Admin Center** , then select **IAM** from the navigation bar. 

**3.** Select the **Identities** tab. 

**4.** In the **Imported Users** tab, select the checkbox next to the account to deactivate. 

**5.** Select **Actions** > **Deactivate User** . 

**6.** Click **Confirm** . 

## **Note:** 

   - After deactivation, the account status changes to **Deactivated** . Inactive accounts are excluded from user search results and cannot be added to new authorization policies. 

   - By default, only active users appear in the users list. To view deactivated accounts, use the filter option and change the **Status** to **Deactivated** . 

**7.** Verify that the account status is displayed as **Deactivated** in the user list. 

## **Reactivating Imported Users in Prism Central** 

Reactivate imported users in Prism Central. 

## **About this task** 

To reactivate a deactivated imported user in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select **Admin Center** , then select **IAM** from the navigation bar. 

**3.** Select the **Identities** tab. 

**4.** In the **Imported Users** tab, use the **Filter** option to display only **Deactivated** accounts. 

**5.** Select the checkbox next to the account that you need to reactivate. 

AOS Security | Security Management Using Prism Central | **25** 

## **6.** Select **Actions** > **Reactivate** . 

**7.** Click **Confirm** . 

**Note:** After reactivation, the account status changes to **Active** . The account regains its previously assigned roles and permissions and becomes available in user search results and authorization policies. The login access for the account is also restored. 

**8.** Verify that the account status is displayed as **Active** in the user list. 

## **Local User Account in Prism Central** 

Prism Central allows you to create and manage local user accounts. 

During the cluster deployment process, an `admin` user account is created by default. However, you can create additional local user accounts by granting specific permissions for accessing Prism Central. 

## **Local User Account Considerations** 

Before creating a local user account, consider the following points: 

- A local user account cannot SSH into CVM and PCVM. SSH access to CVM and PCVM is limited only to the built-in `nutanix` and `admin` user accounts. 

- By default, no permissions are granted to a local user account. After creating a local user account, you must specify the custom or built-in roles and permissions. 

- A local user account does not have password expiration policy. 

- The delete option is not available for a local user account in the Prism Central web console. However, you can delete a local user account using nuclei CLI commands. For more information, see Deleting a User or User Group on page 61. 

- When you enable Prism Self Service feature, the system automatically uses Active Directory as the directory service. 

- Changing the Prism Central admin password does not affect the cluster registration. 

## **Creating a Local User Account in Prism Central** 

You can create a local user account in Prism Central. 

## **About this task** 

To create a local user account in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Identities** tab. 

AOS Security | Security Management Using Prism Central | **26** 

**4.** Click the **+ Add Local User** and enter the following information: 

   - **First Name** : Enter a first name. 

   - **Last Name** : Enter a last name. 

   - **Email** : Enter a valid user email address in the format _`user`_ `@` _`domain`_ `.` _`top-level-domain`_ . 

The _`user`_ and _`domain`_ can contain the following characters: 

- Letters (a-z, A-Z) 

- Digits (0-9) 

- Special characters: `.` , `_` , and `-` 

The _`top-level-domain`_ must be two to five characters long. 

- **Username** : Enter a user name. 

The username can contain only the following characters: 

   - Letters (a-z, A-Z) 

   - Digits (0-9) 

   - Special characters: `-` , `_` , `.` , and `@` 

   - Non-ASCII (international) characters 

- **Password** : Enter a password. 

Ensure that your password meets the following requirements: 

   - At least eight characters long 

   - At least one lowercase letter 

   - At least one uppercase letter 

   - At least one digit 

   - At least one special character (allowed special characters are: "#$%&'()*+,-./:;<=>@[]^_`{|}~!\ ) 

   - At least four characters different from the old password 

   - Must not be among the last five passwords 

   - Must not have more than two consecutive occurrences of a character 

   - Must contain at least one character from each of the following four character classes: uppercase letters, lowercase letters, digits, and special characters 

   - Must not contain the words "nutanix", "ntnx", "password", or any simple dictionary words. 

- **Language** : From the dropdown list, select the language setting for the user. 

For example, if you select **zh-CN** , the user interface displays in Simplified Chinese when the user logs in Prism Central. 

   - Turn on the **User Enabled** toggle to enable the login access to the local user account. 

**5.** Click **Save** . 

AOS Security | Security Management Using Prism Central | **27** 

## **Editing a Local User Account in Prism Central** 

You can edit the local user account settings in Prism Central. 

## **About this task** 

To edit the local user account, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Identities** tab. 

**4.** Select the checkbox associated with the local user account. 

**5.** Select **Actions** > **Edit** . 

**6.** In the **Edit Local User** window, edit the fields as needed and click **Update** . 

## **Changing a Local User Account Password in Prism Central** 

You can change the local user account password in Prism Central. 

## **About this task** 

To change the local user account password, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Identities** tab. 

**4.** Select the checkbox associated with the local user account. 

**5.** Select **Actions** > **Change Password** . 

AOS Security | Security Management Using Prism Central | **28** 

**6.** In the **Edit Password** window, enter the following information: 

   - **Password** : Enter the new password. 

   - **Re-Enter Password** : Re-enter the new password. 

Ensure that your password meets the following requirements: 

   - At least eight characters long 

   - At least one lowercase letter 

   - At least one uppercase letter 

   - At least one digit 

   - At least one special character (allowed special characters are: "#$%&'()*+,-./:;<=>@[]^_`{|}~!\ ) 

   - At least four characters different from the old password 

   - Must not be among the last five passwords 

   - Must not have more than two consecutive occurrences of a character 

   - Must contain at least one character from each of the following four character classes: uppercase letters, lowercase letters, digits, and special characters 

   - Must not contain the words "nutanix", "ntnx", "password", or any simple dictionary words. 

**7.** Click **Update** . 

## **Deactivating a Local User Account in Prism Central** 

You can disable login access to the local user account in Prism Central. 

## **About this task** 

To deactivate login access to the local user account, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Identities** tab. 

**4.** Select the checkbox associated with the local user account. 

**5.** Select **Actions** > **Deactivate** . 

**6.** Click **Save** . 

## **Reactivating a Local User Account in Prism Central** 

You can enable the login access for a disabled local user account. This action automatically restores all previously assigned permissions. 

## **About this task** 

To enable login access to the local user account, follow these steps: 

AOS Security | Security Management Using Prism Central | **29** 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Identities** tab. 

**4.** Select the checkbox associated with the local user account. 

**5.** Select **Actions** > **Reactivate** . 

**6.** Click **Save** . 

## **Resetting a Local User Account Password Using nCLI** 

Reset a local user account password using the Nutanix command-line interface (nCLI). 

## **About this task** 

To reset the local user account password using nCLI, follow these steps: 

## **Procedure** 

**1.** Do one of the following: 

   - » SSH into any of the Controller VMs (CVMs) as a `nutanix` user: 

`$ ssh nutanix@` _`cvm_ip_address`_ 

- » SSH into Prism Central VM (PCVM) as a `nutanix` user: 

   - `$ ssh nutanix@` _`pcvm_ip_address`_ 

AOS Security | Security Management Using Prism Central | **30** 

**2.** Reset the local user account password: 

   - » `nutanix@cvm$ ncli user reset-password user-name=` _`'user_name'`_ `password=` _`'new_password'`_ 

   - » `nutanix@pcvm$ ncli user reset-password user-name=` _`'user_name'`_ `password=` _`'new_password'`_ 

   - Replace _`'user_name'`_ with the local user account name. 

   - Replace _`'new_password'`_ with the new password. 

Ensure that your password meets the following requirements: 

- At least eight characters long 

- At least one lowercase letter 

- At least one uppercase letter 

- At least one digit 

- At least one special character (allowed special characters are: "#$%&'()*+,-./:;<=>@[]^_`{|}~!\ ) 

- At least four characters different from the old password 

- Must not be among the last five passwords 

- Must not have more than two consecutive occurrences of a character 

- Must contain at least one character from each of the following four character classes: uppercase letters, lowercase letters, digits, and special characters 

- Must not contain the words "nutanix", "ntnx", "password", or any simple dictionary words. 

## **Service Accounts** 

Service accounts provide controlled programmatic access to resources and operations in your Nutanix environment. 

A service account is a special type of account used by Nutanix applications or services to perform automated tasks within the system. It is identified by a unique name, and is used to authenticate the application or service that owns it. The authentication is done using a shared secret key, which is generated and managed using IAM. Once configured, the service account can perform actions that it has been explicitly authorized to do. Administrators can assign rolebased access control (RBAC) to a service account, allowing controlled access to specific resources or operations. 

Service accounts are of the following types. 

- User-managed service accounts: Created and maintained by administrators who have permission to manage them. These are typically used when custom automation or integration is needed. 

- Service-managed accounts: Created automatically by backend services during setup. 

## **Configuring a Service Account in Prism Central** 

You can create a local user account in Prism Central. 

## **About this task** 

To configure a service account in Prism Central, follow these steps: 

AOS Security | Security Management Using Prism Central | **31** 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Identities** tab. 

**4.** Select **Service Accounts** . 

**5.** Click **+ Add New Service Account** and enter the following information: 

   - **Service Account Name** : Enter a name for your service account. 

   - **Service Account Email (optional)** : Optionally, enter an email address for your service account. 

   - **Description** : Optionally, enter a description for your service account. 

**6.** Choose one of the following actions. 

   - » **Save** : To save the service account without creating keys. This completes the service account creation. To configure keys later, see Managing Service Account Keys on page 32. 

   - » **Save and Create Keys** : To save the service account and create keys. Proceed to the next step. 

**7.** Create keys for your service account. 

   - a. Click **+ Add New Key** . 

   - b. Select a **Key Type** from the following options. 

      - **Objects Key** : S3 compatible access key and secret key pair for Objects service. 

      - **API Key** : Access key and secret key pair for use with any Nutanix service. 

   - c. In the **Key Name** field, enter a name for the key. 

   - d. Optionally, in the **Assigned To** field, enter the email address of the user to whom you want to assign the key. 

   - e. Set key expiry. 

      - » In the **Key Expiry Date** and **Key Expiry Time** fields, click the calender icon and clock icon to select the key expiry date and time respectively. 

      - » Enable the **Key never expires** checkbox to set the key without an expiration date. 

   - f. Click **Generate Key** . 

The key is generated successfully. 

## **What to do next** 

Click the copy icon to copy the generated access key and secret access key. Alternatively, click **Download Key Details** to download the key details in the CSV file format. 

**Important:** You cannot retrieve the key details after closing the dialog box. Ensure that you copy or download the key details before exiting. 

## **Managing Service Account Keys** 

Add, revoke, or delete service account keys. 

AOS Security | Security Management Using Prism Central | **32** 

## **About this task** 

To manage keys associated with a service account, do the following: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Identities** tab. 

**4.** Select **Service Accounts** . 

**5.** From the list of configured service accounts, select the service account for which you want to add or update the key details. 

## **6.** Click **Manage Keys** . 

**7.** Choose any of the following key management options depending on your requirement. 

   - » To add a new key, click **+ Add New Key** and enter the key details. For more information, see step 5 on page 32 in Configuring a Service Account in Prism Central on page 31. 

**Important:** Ensure that you copy or download the key details before exiting. 

- » To revoke or delete the associated access key for a service account, click **Actions** and select **Revoke Key** or **Delete Key** . 

## **Deactivating Service Accounts in Prism Central** 

Deactivate service accounts in Prism Central. 

## **About this task** 

To deactivate a service account in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select **Admin Center** , and then select **IAM** from the navigation bar. 

**3.** Select the **Identities** tab. 

**4.** In the **Service Accounts** tab, click the service account to deactivate. 

**5.** Click **Deactivate User** . 

AOS Security | Security Management Using Prism Central | **33** 

## **6.** Click **Confirm** . 

**Note:** After you deactivate the service account, the following conditions take effect: 

   - You cannot use the existing API keys associated with the deactivated service account for authentication. 

   - You can delete or revoke the API keys as needed. However, you cannot create new API keys for a deactivated service account. 

   - You cannot use the account in authorization policy configurations. 

**7.** Verify that the account status is displayed as **Inactive** in the user list. 

## **Reactivating Service Accounts in Prism Central** 

Reactivate service accounts in Prism Central. 

## **About this task** 

To reactivate a service account in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select **Admin Center** , and then select **IAM** from the navigation bar. 

**3.** Select the **Identities** tab. 

**4.** In the **Service Accounts** tab, use the **Filter** option to display only **Deactivated** accounts. 

**5.** Click the service account that you need to reactivate. 

**6.** Select **Actions** > **Reactivate** . 

**7.** Click **Confirm** . 

**Note:** After you reactivate the service account, the following conditions take effect: 

   - You can use the API keys again for API authentication. 

   - Existing API keys, if not deleted, stay active or revoked. Additionally, you can create new API keys. 

   - You can use the account in authorization policy configurations. 

**8.** Verify that the account status is displayed as **Active** in the user list. 

## **Roles Management** 

Prism Central supports role-based access control (RBAC), which you can configure to provide customized access permissions for users based on their assigned roles. 

Prism Central provides the following role types: 

AOS Security | Security Management Using Prism Central | **34** 

- Built-in Role: Includes a set of predefined roles designed for common IAM use cases. Built-in roles simplify role assignment by eliminating the need for administrators to manually configure granular permissions. To view the complete list of built-in roles, see the Built-in Roles List on page 35. 

For example, the **VM Admin Role** provides comprehensive permissions for managing virtual machines, including creation, modification, and deletion. 

**Note:** You cannot update or delete the built-in roles. 

- Custom Role: In addition to the built-in roles, you can also create custom roles in Prism Central. The custom roles provide granular control necessary for fine-tuning permissions and to meet specific organizational needs, thus maintaining security and compliance. You can define precise access levels for the users based on specific job functions or tasks. 

Note the following hypervisor-specific limitations for granular RBAC: 

- AHV Environment: Granular RBAC is fully supported for all Prism Central entities. 

- ESXi Environment: Granular RBAC is not supported for VM entities and applies only to a limited set of other Prism Central entities. 

**Note:** Starting with the pc.2023.4 release, Prism Central provides additional built-in roles. If you created any custom roles with the same name as the new built-in roles, the system overrides the permissions of these custom roles with the default role permissions after you upgrade. Before upgrading to pc.2023.4 or later, Nutanix recommends renaming or deleting these custom roles to prevent any permission issues after the upgrade. 

## **Built-in Roles List** 

Prism Central offers several built-in roles, each designed for common administrative tasks. 

The following table lists the built-in roles available in Prism Central: 

**Table 1: Prism Central Built-in Roles** 

|**Role**|**Description**|
|---|---|
|Action Service User|Basic Playbook access for all users|
|Backup Admin|Full access for backup operations|
|Category Admin|Full access for category objects|
|Category Viewer|View access for category objects|
|Cluster Admin|Full access for cluster operations.|
|Cluster Viewer|View access for cluster operations.|
|Consumer|Launch blueprints and controls their life cycle and|
||actions|
|CSI System|Full access for Kubernetes cluster infrastructure|
||resources for CSI|
|Developer|Author blueprints, tests deployments, and publishes|
||applications for other project members.|
|Disaster Recovery Admin|Full access for disaster recovery operations.|
|Disaster Recovery Viewer|View access for disaster recovery operations.|



AOS Security | Security Management Using Prism Central | **35** 

**Role Description** Domain Manager Admin Full access for Domain Manager (Prism Central Management) v4 APIs Domain Manager Viewer View access for Domain Manager (Prism Central Management) v4 APIs File Server Security Admin Full access for Nutanix File server security related permissions. File Server Share Admin Full access for Nutanix File server share operations. Files Admin Full access for Files Storage operations. Files Viewer View access for Files Storage operations. Flow Admin Full access for Nutanix Flow operations including categories provisioning Flow Viewer View access for Nutanix Flow operations. Flow Policy Author Full access for flow operations except categories provisioning Foundation Central Admin Full access to perform all Nutanix Foundation Central operations Foundation Central Viewer View access to all API keys and nodes in Nutanix Foundation Central Kubernetes Data Services System Full access for Kubernetes cluster infrastructure resources for Kubernetes Data Services Kubernetes Infrastructure Provision Full access for Kubernetes cluster infrastructure VM resources License Admin Full access for Licensing operations License Viewer View access for Licensing operations Local Account Manager Admin Admin access for local account password related operations. Local Account Manager Viewer View access for local account password related operations. Monitoring Admin Full access to perform all monitoring operations Monitoring Viewer View access to all monitoring APIs NCM Connector Full access to perform operations on remote Prism Central instances through NCM Network Infra Admin Manage the infrastructure and underlay networking. Network Shared Resources Viewer View access for shared resources in underlay and overlay networking. Objects Admin Full access for Nutanix Objects store operations Objects Editor Edit access for Nutanix Objects store operations Objects Viewer View access for Nutanix Objects store operations Operations Management Admin Full access to perform all operations management Operations Management Viewer View access to all API in operations management 

AOS Security | Security Management Using Prism Central | **36** 

|**Role**|**Description**|
|---|---|
|Operator|Manage the existing application deployments and|
||exercise blueprint actions.|
|Prism Admin|Manage the infrastructure and platform, but cannot|
||entitle other users to be admins.|
|Prism Viewer|View access to all infrastructure and platform|
||features, but cannot make any changes|
|Project Admin|Manage end users within the project and has full|
||access to their entities|
|Project Manager|Manage virtual infrastructure, oversees self service,|
||and can delegate end user management.|
|Secure Policy Admin|Full access for secure policies operations|
|Secure Policy Disaster Recovery Admin|Manage associations for secure policies|
|Secure Policy Editor|Update access for secure policies|
|Secure Policy Viewer|View access for secure policies|
|Security Admin|Update access for security features like KMS and|
||encryption|
|Security Dashboard Admin|Update access for security dashboard|
|Security Dashboard Viewer|View access for security dashboard|
|Security Viewer|View access for security features like KMS and|
||encryption|
|Storage Admin|View and perform actions on storage entities.|
|Storage Viewer|View access for storage entities.|
|Super Admin|Manage Nutanix deployment, set-up, configure, and|
||make use of every feature in the platform|
||**Note:**Highest-level admin with full infrastructure|
||and tenant access.|
|Virtual Machine Admin|Full access for virtual machine operations.|
|Virtual Machine Operator|Access day-to-day activities on virtual machines.|
|Virtual Machine Viewer|View access for virtual machines.|
|VPC Admin|Manage VPCs and related entities. Agnostic of the|
||physical network infrastructure.|



You can perform the following actions on built-in roles: 

- Add an authorization policy. For more information, see Adding an Authorization Policy to a Role on page 41. 

- Duplicate an existing built-in role. For more information, see Duplicating a Built-in Role on page 42. 

- View the detailed list of permissions associated with a built-in role. For more information, see Viewing Role Permissions on page 44. 

**Note:** You cannot update or delete the built-in roles. 

AOS Security | Security Management Using Prism Central | **37** 

## **Creating a Custom Role** 

In addition to built-in roles, you can also create a custom role in Prism Central. 

## **About this task** 

To create the custom role in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

**4.** Click **Create Role** > **New Role** . 

**5.** In the **Role Name** field, enter a unique name for the role. 

**Note:** You must enter a unique name for the custom role. Built-in role names are not allowed. For more information, see Built-in Roles List on page 35. 

**6.** (Optional) In the **Description** field, enter a description for the role. 

**7.** Do one of the following: 

   - To filter all the operations available for a specific resource type, select the **Entity Type** from the dropdown list and in the search box enter the name of the resource. 

For example, when you select the **Entity Type** and enter _vm-vm anti-affinity policy_ in the search box, the search result displays all the operations related to vm-vm anti-affinity policy resource. 

   - To filter the individual operations available for a specific resource type, select the **Operation** from the drop down list and in the search box enter the name of the resource. 

      - For example, when you select the **Operation** and enter _anti affinity_ in the search box, the search result displays the individual operations that are performed on a vm-vm anti-affinity policy resource. 

**8.** Click the plus icon for adding an individual operation or a group of operations to the role. 

AOS Security | Security Management Using Prism Central | **38** 

**9.** (Optional) To view the related operations, click the 

related operations icon. 

**10.** (Optional) To add the recommended related operations, click **Add All and Save** . 

**11.** Select the recommended related operations and click **Add All and Save** . 

**Note:** Nutanix recommends that you select all the related operations to ensure that the role is granted with sufficient permissions within the authorization policy. 

**12.** Do one of the following: 

   - » To create the role, click **Save** . 

   - » To create the role and attach the authorization policy, click **Save & Create Authorization Policy** . 

The system redirects to the **Create New Authorization Policy** page. For more information, see Creating an Authorization Policy for Full Access on page 57. 

## **Creating a Custom Role from an Existing Role** 

You can use an existing built-in or custom role as a starting point when you create a custom role. 

## **Before you begin** 

Before you create the custom role from an existing role, consider the following points: 

AOS Security | Security Management Using Prism Central | **39** 

- Any system operations in the existing role are not added to the new role that you create. 

- Some operations are pre-selected if you are creating a role from an existing role. 

## **About this task** 

To create the custom role from an existing role, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

**4.** Click **Create Role** > **From Existing Role** . 

**Note:** The new role does not include system operations from the existing role. 

**5.** In the **Search roles** box, type the built-in or custom role name and select the role from the search result. The system displays the list of operations associated with the selected role. 

**6.** Click **Next** . 

**7.** On the **System Operations** window, click **Continue** . 

**8.** In the **Role Name** field, enter a unique name for the role. 

**Note:** You must enter a unique name for the custom role. Built-in role names are not allowed. For more information, see Built-in Roles List on page 35. 

**9.** (Optional) In the **Description** field, enter a description for the role. 

**10.** Do one of the following: 

   - To filter all the operations available for a specific resource type, select the **Entity Type** from the dropdown list and in the search box enter the name of the resource. 

For example, when you select the **Entity Type** and enter _vm-vm anti-affinity policy_ in the search box, the search result displays all the operations related to vm-vm anti-affinity policy resource. 

- To filter the individual operations available for a specific resource type, select the **Operation** from the drop down list and in the search box enter the name of the resource. 

For example, when you select the **Operation** and enter _anti affinity_ in the search box, the search result displays the individual operations that are performed on a vm-vm anti-affinity policy resource. 

**11.** Click the plus icon to add an individual operation or a group of operations to the role. 

AOS Security | Security Management Using Prism Central | **40** 

**12.** (Optional) To view the related operations, click the 

related operations icon. 

**13.** Select the recommended related operations and click **Add** . 

**Note:** Nutanix recommends that you select all the related operations to ensure that the role is granted with sufficient permissions within the authorization policy. 

**14.** Do one of the following: 

   - » To create the role, click **Save** . 

   - » To create the role and attach the authorization policy, click **Save & Create Authorization Policy** . 

The system redirects to the **Create New Authorization Policy** page. For more information, see Creating an Authorization Policy for Full Access on page 57. 

## **Adding an Authorization Policy to a Role** 

The **Roles** tab provides an option to associate the authorization policy to both built-in and custom roles. 

## **About this task** 

To add an authorization policy to a role, follow these steps: 

AOS Security | Security Management Using Prism Central | **41** 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

**4.** Select the checkbox associated with the role. 

**5.** Select **Actions** > **Add Authorization Policy** . 

**6.** Configure the authorization policy fields as needed. 

For more information on how to configure an authorization policy, see Creating an Authorization Policy for Full Access on page 57. 

## **Duplicating a Built-in Role** 

You can duplicate a built-in role. 

## **Before you begin** 

When you duplicate a built-in role, Prism Central displays a message indicating that it cannot duplicate internal operations. Prism Central excludes the internal operations from duplication to prevent misconfiguration or unintended behavior because these internal operations are tightly coupled with systemlevel logic and are subject to change. As a result, the duplicated role might include fewer operations and not provide access to specific features. Nutanix recommends that you verify the duplicated role's permissions before assigning it to users. 

## **About this task** 

To duplicate a built-in role, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

**4.** Select the checkbox associated with the built-in role. 

**5.** Select **Actions** > **Duplicate** . 

## **6.** Click **Continue** . 

**7.** Edit the fields in the **Duplicate Role** window. 

**8.** Do one of the following: 

   - » To save the role, click **Save** . 

   - » To save the role and attach the authorization policy, click **Save & Create Authorization Policy** . 

The system redirects to the **Create New Authorization Policy** page. For more information, see Creating an Authorization Policy for Full Access on page 57. 

AOS Security | Security Management Using Prism Central | **42** 

## **Duplicating a Custom Role** 

You can duplicate a custom role. 

## **About this task** 

To duplicate a custom role, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

**4.** Select the checkbox associated with the custom role. 

**5.** Select **Actions** > **Duplicate** . 

**6.** Edit the fields in the **Duplicate Role** window. 

**7.** Do one of the following: 

   - » To save the role, click **Save** . 

   - » To save the role and attach the authorization policy, click **Save & Create Authorization Policy** . 

The system redirects to the **Create New Authorization Policy** page. For more information, see Creating an Authorization Policy for Full Access on page 57. 

## **Deleting a Role** 

You can delete a custom role in Prism Central. 

## **About this task** 

To delete the custom role, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

**4.** Select the checkbox associated with the role. 

**5.** Select **Actions** > **Delete** . 

**6.** On the **Delete Role** window, click **Delete** . 

## **Updating a Custom Role** 

You can update a custom role in Prism Central. 

## **About this task** 

To update the custom role, follow these steps: 

AOS Security | Security Management Using Prism Central | **43** 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

**4.** Select the checkbox associated with the role. 

**5.** Select **Actions** > **Update** . 

**6.** Update the fields in the **Update Role** window as needed and click **Update** . 

## **Viewing Role Permissions** 

You can view the permissions associated with both built-in and custom roles. 

## **About this task** 

To view the permissions associated with a role, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

**4.** Select the checkbox associated with the role. 

The system displays all the permissions associated with the role. 

## **Cluster or Category Independent Entities** 

The following list identifies the entities within Prism Central that do not support cluster or category-based scoping: 

- License Configuration 

- Cost Governance Registration 

- Platform Dependent Field 

- Prism Central 

- XFit Policy 

- Cluster Metrics 

- Authorization Policy 

- Directory Server Config 

- Security Monitoring 

- Network Security Rule 

- Sync Policy 

- Category Mapping 

AOS Security | Security Management Using Prism Central | **44** 

- Network Function Chain 

- Identity Categorization Config 

- Flow Upgrade 

- OIDC Client 

- Stats 

- Simulation 

- Secure Snapshot Approval Policy 

- Stig Report 

- Summary 

- Stig Control 

- Vulnerability 

- Hardening Visibility Setting 

- NC2 AWS VPC 

- Network Controller Configuration 

- NC2 AWS Subnet List Capability 

- NC2 AWS Subnet 

- Certificate Authentication Provider 

- Role 

- Entity 

- Config Changeset 

- Operation 

- Client 

- Service Account 

- Alerts Policy 

- Audit 

- System Defined Alert Policy 

- Event 

- User Defined Alert Policy 

- Alert Email Config 

- Alert 

**Important:** To configure authorization policy for these entities, you must explicitly allow access in the authorization policy configuration. For more information, see Entities limitation in Authorization Policy . 

## **Changes to Registered Operations and Custom Role Migration in Prism Central pc.7.3** 

Prism Central version pc.7.3 includes changes to several IAM operations. 

AOS Security | Security Management Using Prism Central | **45** 

The IAM operations are renamed, split into multiple operations, or consolidated to improve RBAC precision. For more information, see Operations Changes in Prism Central pc.7.3 on page 46. 

These changes to operations might affect any custom roles that reference older versions of these operations. To ensure custom roles remain valid after the upgrade, IAM automatically updates roles where a direct mapping is possible. However, you must manually migrate any custom roles that cannot be updated automatically. For more information, see Migrating Custom Roles after Prism Central version pc.7.3 Upgrade on page 46. 

## **Migrating Custom Roles after Prism Central version pc.7.3 Upgrade** 

After upgrading to Prism Central version pc.7.3, some custom roles must be manually migrated to align with the changed operations in pc.7.3. 

## **Before you begin** 

Ensure that you have one of the following built-in roles assigned to perform custom role migration. 

- Super Admin 

- Prism Admin 

- Project Manager 

## **About this task** 

To migrate custom roles containing outdated operations, do the following: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Roles** tab. 

If there are roles that reference outdated operations, a banner containing the following message appears at the top of the page: **X custom roles in your system contain outdated operations and might not function as expected. To ensure full functionality, migrate your roles now.** 

**4.** In the banner, click the **migrate your roles now** link. 

## **What to do next** 

After migration, validate the role permissions to ensure they meet your access requirements. 

## **Operations Changes in Prism Central pc.7.3** 

Updated IAM operations in Prism Central version pc.7.3. 

The following table lists the updated IAM operations in Prism Central pc.7.3, along with their corresponding renamed, consolidated, or expanded forms. 

**Table 2: Updated IAM Operations in Prism Central pc.7.3** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|IAM|View User Key|View Service Account||
|||Api Key View User||
|||Buckets Access Key||



AOS Security | Security Management Using Prism Central | **46** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|IAM|Delete User Key|Delete User Buckets||
|||Access Key Delete||
|||Service Account Api Key||
|IAM|View Service Account|View Nutanix Access||
||Access Key|Key||
|IAM|Delete User Buckets|Delete Buckets Access||
||Access Key|Key||
|IAM|Create User Buckets|Add Buckets Access Key||
||Access Key|||
|IAM|Create User Group|Add User Group||
|IAM|Delete User Group|Remove User Group||
|IAM|Update Operation|Update Permission||
|IAM|Revoke Service Account|Revoke Nutanix Api Key||
||Api Key|||
|IAM|Update Authorization|Update ACP||
||Policy|||
|IAM|Delete Authorization|Delete ACP||
||Policy|||
|IAM|Revoke User Key|Revoke User Buckets||
|||Access Key Revoke||
|||Service Account Api Key||
|IAM|Change User State|Enable User||
|IAM|Delete Saml Identity|Delete SAML IdP Delete||
||Provider|Identity Provider||
|IAM|View Saml Identity|View Identity Provider||
||Provider|View SAML IdP||
|IAM|View Operation|View Permission||
|IAM|Create Saml Identity|Create Identity Provider||
||Provider|Create SAML IdP||
|IAM|Create Service Account|Add Service Account||
|IAM|Create Directory Service|Configure Directory||
|||Service||
|IAM|View Client|View Service Client||
|IAM|Create Service Account|Add Nutanix Api Key||
||Api Key|||
|IAM|Create User|Add User||
|IAM|Create Service Account|Add Nutanix Access Key||
||Access Key|||
|IAM|View Service Account|View Nutanix Api Key||
||Api Key|||



AOS Security | Security Management Using Prism Central | **47** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|IAM|Delete Directory Service|Delete Directory Service||
|||Configuration||
|IAM|Create Authorization|Create ACP||
||Policy|||
|IAM|Delete Service Account|Delete Nutanix Access||
||Access Key|Key||
|IAM|Delete Service Account|Delete Nutanix Api Key||
||Api Key|||
|IAM|Update Saml Identity|Update Identity Provider||
||Provider|Update SAML IdP||
|IAM|Delete Operation|Delete Permission||
|IAM|Reset User Password|Reset Password||
|IAM|Create Operation|Create Permission||
|IAM|View User Buckets|View Buckets Access||
||Access Key|Key||
|IAM|Delete User|Remove User||
|IAM|View Authorization Policy|View ACP||
|IAM|Create User Key|Create Service Account||
|||Api Key Create User||
|||Buckets Access Key||
|Files|View File Server|View File Server Data||
||Protected Pairs|Protected Pairs||
|Cluster Management|View Cluster Credentials|View Config Credentials||
|Cluster Management|View Storage Container|View Container Stats||
||Stats|View Container Stats||
|Cluster Management|Create Storage|Create Container Create||
||Container|Container||
|Cluster Management|Delete Storage|Delete Container Delete||
||Container|Container||
|Cluster Management|View Storage Container|View Container View||
|||Container||
|Cluster Management|Unmount Storage|Unmount Container||
||Container Datastore|Datastore Unmount||
|||Container Datastore||
|Cluster Management|Mount Storage Container|Mount Container||
||Datastore|Datastore Mount||
|||Container Datastore||
|Cluster Management|Update Storage|Update Container||
||Container|Update Container||
|Cluster Management|View Storage Container|View Container||
||Datastore|Datastore View||
|||Container Datastore||



AOS Security | Security Management Using Prism Central | **48** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|Volumes|View Volume Group||View Volume Group|
||iSCSI Attachments|||
||Internal|||
|Volumes|View Volume Group||View Volume Group|
||Category Associations|||
|Volumes|Update Volume Group||Update Volume Group|
||Disk||Virtual Disks Update|
||||Volume Group|
|Volumes|Update Volume Group||Update Volume Group|
||Disk Internal||Virtual Disks Update|
||||Volume Group|
|Volumes|Update Volume Group|Update Volume Group|Update Volume Group|
||Details Internal|Details||
|Volumes|Detach Volume Group||Update Connections with|
||From External iSCSI||External iSCSI Clients|
||Client Internal||Update Volume Group|
|Volumes|Attach Volume Group To||Update Connections with|
||External iSCSI Client||External iSCSI Clients|
||||Update Volume Group|
|Volumes|View Volume Group||View Volume Group|
||Details|||
|Volumes|Update External iSCSI|Update External iSCSI||
||Client Internal|Client||
|Volumes|Detach Volume Group||Update Connections with|
||From External iSCSI||External iSCSI Clients|
||Client||Update Volume Group|
|Volumes|Create Volume Group||Update Volume Group|
||Disk||Virtual Disks Update|
||||Volume Group|
|Volumes|View Volume Group||View Volume Group|
||Metadata|||
|Volumes|View Volume Group Disk||View Volume Group|
||Stats|||
|Volumes|View Volume Group||View Volume Group|
||Stats|||
|Volumes|Associate Volume Group||Update Volume Group|
||Categories||Categories Update|
||||Volume Group|
|Volumes|View Volume Group||View Volume Group|
||Disks|||
|Volumes|View Volume Group VM||View Volume Group|
||Attachments|||



AOS Security | Security Management Using Prism Central | **49** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|Volumes|Attach Volume Group To||Update Volume Group|
||AHV VM||Update Connections with|
||||Direct-attach AHV VMs|
|Volumes|Disassociate Volume||Update Volume Group|
||Group Categories||Categories Update|
||||Volume Group|
|Volumes|Detach Volume Group||Update Connections with|
||From AHV VM||Direct-attach AHV VMs|
||||Update Volume Group|
|Volumes|Delete Volume Group||Update Volume Group|
||Disk||Virtual Disks Update|
||||Volume Group|
|Volumes|View Volume Group||View Volume Group|
||Metadata Internal|||
|Volumes|View Volume Group||View Volume Group|
||iSCSI Attachments|||
|Volumes|Detach Volume Group||Update Connections with|
||From AHV VM Internal||Direct-attach AHV VMs|
||||Update Volume Group|
|Networking|Delete Subnet|Delete Overlay Subnet||
|||Delete Overlay External||
|||Subnet Delete External||
|||Subnet Delete Overlay||
|||Subnet Delete Overlay||
|||External Subnet Delete||
|||External Subnet||
|Networking|Unreserve Subnet Ip|Unreserve Vlan||
|||Subnet Ip Unreserve||
|||Overlay Subnet Ip||
|||Unreserve Vlan Subnet||
|||Ip Unreserve Overlay||
|||Subnet Ip||
|Networking|View VPC|View Virtual Network||
|||View Transit VPC View||
|||Transit VPC||
|Networking|View Subnet|View Overlay Subnet||
|||View Overlay External||
|||Subnet View External||
|||Subnet View Overlay||
|||Subnet View Overlay||
|||External Subnet View||
|||External Subnet||
|Networking|Migrate vNIC|Migrate Overlay vNIC||
|||Migrate Overlay vNIC||
|Networking|Update Network|Update Vpn Gateway||
||Gateway|||



AOS Security | Security Management Using Prism Central | **50** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|Networking|View Network Gateway|View Vpn Gateway||
|Networking|Update VPC|Update Transit VPC||
|||Update Virtual Network||
|||Update Transit VPC||
|Networking|Delete VPC|Delete Virtual Network||
|||Delete Transit VPC||
|||Delete Transit VPC||
|Networking|Update Subnet|Update Overlay Subnet||
|||Update Overlay External||
|||Subnet Update External||
|||Subnet Update Overlay||
|||Subnet Update Overlay||
|||External Subnet Update||
|||External Subnet||
|Networking|Create Network Gateway|Create Vpn Gateway||
|Networking|Create VPC|Create Virtual Network||
|||Create Transit VPC||
|||Create Transit VPC||
|Networking|Reserve Subnet Ip|Reserve Overlay Subnet||
|||Ip Reserve Vlan Subnet||
|||Ip Reserve Overlay||
|||Subnet Ip Reserve Vlan||
|||Subnet Ip||
|Networking|Delete Network Gateway|Delete Vpn Gateway||
|Networking|Create Subnet|Create Overlay External||
|||Subnet Create External||
|||Subnet Create Overlay||
|||Subnet Create Overlay||
|||External Subnet Create||
|||External Subnet Create||
|||Overlay Subnet||
|Microsegmentation|View Directory Server|View Directory Server||
||Config|||
|Microsegmentation|Update Directory Server|Update Directory Server||
||Config|||
|Microsegmentation|Policy Preview Flow|Preview Migration Flow||
||Upgrade|Migrator||
|Microsegmentation|Upgrade Config Flow|Migrate Config Flow||
||Upgrade|Migrator||
|Microsegmentation|Summarise Flow|Summarise Migration||
||Upgrade|Flow Migrator||
|Microsegmentation|Delete Directory Server|Delete Directory Server||
||Config|||
|Microsegmentation|Create Directory Server|Create Directory Server||
||Config|||



AOS Security | Security Management Using Prism Central | **51** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|Prism|View Category|View Name Category||
|||View Value Category||
|Prism|Delete Category|Delete Name Category||
|||Delete Value Category||
|Prism|View Domain Manager|View Prism Central||
|Virtual Machine|Power On Virtual||Update Virtual Machine|
|Management|Machine||Power State Update|
||||Virtual Machine Allow|
||||Virtual Machine Power|
||||On|
|Virtual Machine|Create Virtual Machine||Update Virtual Machine|
|Management|NIC||NIC List Update Virtual|
||||Machine|
|Virtual Machine|Delete Virtual Machine||Update Virtual Machine|
|Management|CD ROM||Update Virtual Machine|
||||Disk List|
|Virtual Machine|View Virtual Machine||View Virtual Machine|
|Management|Disk|||
|Virtual Machine|ACPI Shutdown Virtual||Allow Virtual Machine|
|Management|Machine||Power Off Update Virtual|
||||Machine Power State|
||||Update Virtual Machine|
|Virtual Machine|Power Off Virtual||Update Virtual Machine|
|Management|Machine||Allow Virtual Machine|
||||Power Off Update Virtual|
||||Machine Power State|
|Virtual Machine|Migrate Virtual Machine||Update Virtual Machine|
|Management|NIC||NIC List Update Virtual|
||||Machine|
|Virtual Machine|Create Virtual Machine||Update Virtual Machine|
|Management|Disk||Update Virtual Machine|
||||Disk List|
|Virtual Machine|Guest Reboot Virtual||Allow Virtual Machine|
|Management|Machine||Reboot Update Virtual|
||||Machine Power State|
||||Update Virtual Machine|
|Virtual Machine|Associate Virtual||Update Virtual Machine|
|Management|Machine Categories||Categories Update|
||||Virtual Machine|
|Virtual Machine|Update Virtual Machine||Update Virtual Machine|
|Management|NIC||NIC List Update Virtual|
||||Machine|
|Virtual Machine|Create Virtual Machine||Update Virtual Machine|
|Management|GPU||GPU List Update Virtual|
||||Machine|



AOS Security | Security Management Using Prism Central | **52** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|Virtual Machine|Create Virtual Machine||Update Virtual Machine|
|Management|Serial Port|||
|Virtual Machine|Power Off ESXi Virtual||Update ESX Virtual|
|Management|Machine||Machine|
|Virtual Machine|Insert Virtual Machine||Mount Virtual Machine|
|Management|CD ROM||CDROM Update Virtual|
||||Machine Disk List|
||||Update Virtual Machine|
|Virtual Machine|View Existing Virtual||View Virtual Machine|
|Management|Machine|||
|Virtual Machine|View Virtual Machine||View Virtual Machine|
|Management|Disk Stats|||
|Virtual Machine|Migrate Virtual Machine||Update Virtual Machine|
|Management|To Host|||
|Virtual Machine|Migrate Virtual Machine||Initiate Virtual Machine|
|Management|Disk||Disk Migration Update|
||||Virtual Machine Disk List|
||||Update Virtual Machine|
|Virtual Machine|Create Virtual Machine||Update Virtual Machine|
|Management|CD ROM||Disk List Update Virtual|
||||Machine|
|Virtual Machine|View Virtual Machine||View Virtual Machine|
|Management|NIC|||
|Virtual Machine|Power Cycle Virtual||Allow Virtual Machine|
|Management|Machine||Reboot Update Virtual|
||||Machine Power State|
||||Update Virtual Machine|
|Virtual Machine|Eject Virtual Machine CD||Update Virtual Machine|
|Management|ROM||Unmount Virtual Machine|
||||CDROM Update Virtual|
||||Machine Disk List|
|Virtual Machine|Update Virtual Machine||Update Virtual Machine|
|Management|Disk||Disk List Update Virtual|
||||Machine|
|Virtual Machine|Disassociate Virtual||Update Virtual Machine|
|Management|Machine Categories||Categories Update|
||||Virtual Machine|
|Virtual Machine|View VM Host Affinity||View VM Host Affinity|
|Management|Policy VM Compliances||Policy|
|Virtual Machine|Insert Virtual Machine||Update Virtual Machine|
|Management|NGT ISO||NGT Config Update|
||||Virtual Machine|
|Virtual Machine|View Virtual Machine CD||View Virtual Machine|
|Management|ROM|||



AOS Security | Security Management Using Prism Central | **53** 

**Operation Category Existing Operation Operation Renamed/ Operations Expanded Consolidated** Virtual Machine Reset ESXi Virtual Update ESX Virtual Management Machine Machine Virtual Machine Create New Virtual Create Virtual Machine Management Machine Virtual Machine Guest Shutdown Virtual Allow Virtual Machine Management Machine Power Off Update Virtual Machine Power State Update Virtual Machine Virtual Machine Delete Existing Virtual Delete Virtual Machine Management Machine Virtual Machine View Virtual Machine View Virtual Machine Management NIC Stats Virtual Machine Reset Virtual Machine Allow Virtual Machine Management Reset Allow Virtual Machine Reboot Update Virtual Machine Power State Update Virtual Machine Virtual Machine Guest Shutdown ESXi Update ESX Virtual Management Virtual Machine Machine Virtual Machine Clone Existing Virtual Clone Virtual Machine Management Machine Virtual Machine View Virtual Machine View Virtual Machine Management Stats Virtual Machine Delete Virtual Machine Update Virtual Machine Management NIC NIC List Update Virtual Machine Virtual Machine Guest Reboot ESXi Update ESX Virtual Management Virtual Machine Machine Virtual Machine Delete Virtual Machine Update Virtual Machine Management GPU Update Virtual Machine GPU List Virtual Machine Assign Virtual Machine Update Virtual Machine Management Owner Update Virtual Machine Owner Virtual Machine Update Virtual Machine Update Virtual Machine Management NGT NGT Config Update Virtual Machine Virtual Machine Delete Virtual Machine Update Virtual Machine Management Disk Update Virtual Machine Disk List Virtual Machine ACPI Reboot Virtual Allow Virtual Machine Management Machine Reboot Update Virtual 

Allow Virtual Machine Reboot Update Virtual Machine Power State Update Virtual Machine 

AOS Security | Security Management Using Prism Central | **54** 

**Operation Category Existing Operation Operation Renamed/ Operations Expanded Consolidated** Virtual Machine Suspend ESXi Virtual Update ESX Virtual Management Machine Machine Virtual Machine View Virtual Machine View Virtual Machine Management Serial Port Virtual Machine Delete Virtual Machine Update Virtual Machine Management Serial Port Virtual Machine Power On ESXi Virtual Update ESX Virtual Management Machine Machine Virtual Machine Update Virtual Machine Update Virtual Machine Management Serial Port Virtual Machine Assign Virtual Machine Update Virtual Machine Management NIC IP NIC List Update Virtual Machine Virtual Machine View Virtual Machine View Virtual Machine Management GPU Virtual Machine Install Virtual Machine Update Virtual Machine Management NGT NGT Config Update Virtual Machine Virtual Machine Upgrade Virtual Machine Update Virtual Machine Management NGT NGT Config Update Virtual Machine Virtual Machine Customise Virtual Update Virtual Machine Management Machine Guest Virtual Machine Reenforce VM Host Update VM Host Affinity Management Affinity Policy Policy Virtual Machine Update Virtual Machine Update Virtual Machine Management Basic Config Virtual Machine View Virtual Machine View Virtual Machine Management NGT Virtual Machine Uninstall Virtual Machine Update Virtual Machine Management NGT NGT Config Update Virtual Machine Virtual Machine View VM Host Affinity View VM Host Affinity Management Policy VM Compliance Policy View VM Host States Affinity Policy VM Compliances Virtual Machine Release Virtual Machine Update Virtual Machine Management NIC IP NIC List Update Virtual Machine Licensing View Licenses View License Metadata Licensing View License View License Entitlement Entitlements Operations Management Create Report Create Report Instance 

Update VM Host Affinity Policy Update Virtual Machine 

AOS Security | Security Management Using Prism Central | **55** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|Operations Management|Upload Report Artifact||Create Global Report|
||||Setting|
|Operations Management|Delete Report|Delete Report Instance||
|Operations Management|View Report Artifact File||View Global Report|
||||Setting|
|Operations Management|Update Global Report|Update Common Report||
||Setting|Config||
|Operations Management|View Report|View Report Instance||
|Operations Management|Delete Global Report|Delete Common Report||
||Setting|Config||
|Operations Management|Notify Recipients Report|Notify Report Instance||
|Operations Management|Download Report||View Report|
|Operations Management|Create Global Report|Create Common Report||
||Setting|Config||
|Operations Management|View Global Report|View Common Report||
||Setting|Config||
|Monitoring|View System Defined||View Alerts Policy|
||Alert Policy Cluster|||
||Configs|||
|Monitoring|Delete User Defined||Delete Alerts Policy|
||Alert Policy|||
|Monitoring|Create User Defined||Create Alerts Policy|
||Alert Policy|||
|Monitoring|View User Defined Alert||View Alerts Policy|
||Policy|||
|Monitoring|Update User Defined||Update Alerts Policy|
||Alert Policy|||
|Monitoring|Update System Defined||Update Alerts Policy|
||Alert Policy Cluster|||
||Configs|||
|Monitoring|View System Defined||View Alerts Policy|
||Alert Policy|||
|Objects|View Object Store|Download Certificate||
||Certificate|Object Store||
|Objects|Create Object Store|Replace Certificates||
||Certificate|Object Store||
|AIOps|View Scenario|View Whatif||
|AIOps|View Stats Sources||View Stats|
|AIOps|View Stats Entities||View Stats|
|AIOps|Share Scenario|Share Whatif||
|AIOps|Update Scenario|Update Whatif||



AOS Security | Security Management Using Prism Central | **56** 

|**Operation Category**|**Existing Operation**|**Operation Renamed/**|**Operations Expanded**|
|---|---|---|---|
|||**Consolidated**||
|AIOps|Update Ignore Window|Update Blackout||
|AIOps|Calculate Ignore Runway|Calculate Runway||
|AIOps|View Stats Entity Types||View Stats|
|AIOps|View Stats Entity||View Stats|
||Descriptors|||
|AIOps|Delete Scenario|Delete Whatif||
|AIOps|View Ignore Window|View Blackout||
|AIOps|Create Ignore Window|Create Blackout||
|AIOps|Delete Ignore Window|Delete Blackout||
|AIOps|Create Scenario|Create Whatif||



## **Authorization Policies** 

Authorization policy is an identity and access management (IAM) mechanism to define access to Nutanix resources using Prism Central. 

You create an authorization policy to assign a role (built-in or custom) to an identity (user or user group) for a global or customized scope of allowed actions. Authorization policy ensures that the identity accessing a resource for a specific operation has the appropriate permissions. 

After you configure user authentication, by default no permissions are granted to the users or user groups. You must explicitly assign the permissions to users by creating an authorization policy. 

**Note:** The System Admin, Prism Admin, and Prism Viewer roles include default authorization policies. You cannot modify or delete these policies. 

## **Creating an Authorization Policy for Full Access** 

Create an authorization policy with full access to all entity types and instances for the added users in the associated role. 

## **About this task** 

To create an authorization policy, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Authorization Policies** tab. 

**4.** Click **+ Create Authorization Policy** . 

**5.** (Optional) To edit the name, click the pencil icon next to the default name, edit the name, and click the checkmark icon. 

**6.** On the **Select Role** search box, enter the built-in or custom role's name and select the role from the list of suggestions. 

   - The system displays role details for the selected role. 

AOS Security | Security Management Using Prism Central | **57** 

**7.** Click **Next** . 

**8.** Select **Full Access: all entity type & instances** . 

## **Note:** 

After updating to pc.2024.1, when you create an Authorization Policy for the Prism Admin role, the LCM page sometimes stall with Waiting for LCM framework to start message. The workaround is to select the scope as **Full access: all entity types & instances#Automatically grant access to new entity types that are added to this role in the future** . 

**9.** (Optional): To automatically gain access to any new entity types that are added to your selected role in the future, select the **Automatically grant access to new entity types that are added to this role in the future** checkbox. 

**10.** Click **Next** . 

**11.** From the **Users** dropdown list, select the user type. 

**12.** On the **Search** box, enter the first few letters of the user or user group's name and select the correct user or user group from the list of suggestions. 

**13.** Click **Save** . 

## **Creating an Authorization Policy for Configurable Access** 

Create an authorization policy with configurable access to the selected entity types and instances for the added users in the associated role. 

## **About this task** 

To create an authorization policy, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Authorization Policies** tab. 

**4.** Click **+ Create Authorization Policy** . 

**5.** (Optional) To edit the name, click the pencil icon next to the default name, edit the name, and click the checkmark icon. 

**6.** On the **Select Role** search box, enter the built-in or custom role's name and select the role from the list of suggestions. 

The system displays role details for the selected role. 

**7.** Click **Next** . 

AOS Security | Security Management Using Prism Central | **58** 

**8.** Select **Configure access: select entity types & instances** and follow these steps: 

   - a. From the **Entity Type** dropdown list, select the entity type. 

The list of available entities depends on the role that you select. 

- b. From the **Filters** dropdown list, select the filter type. 

The list of available filter options depends on the **Entity Type** that you select. 

Starting with AOS 7.0 and pc.2024.3, the **Filters** dropdown list includes a new filter called **Advanced** for some of the entity types. The **Advanced Filter** creates a set of filters with the AND operator. 

You can repeat the **Entity Type** and **Filter** selections for any combination of entity and filter. Defining multiple entity-filter-search combinations by repeating the entity type and filter selection creates a set of filters with an OR operator. 

   - c. If you selected the **Advanced** filter, perform the following actions: 

      - From the **Filters** dropdown list, filter the entity type. 

      - On the **Search** box, select the entity. 

      - Click **+ Condition** . 

      - From the **Filters** dropdown list, filter the entity type. 

      - On the **Search** box, select the entity. 

      - Click **Add to Policy** . 

   - d. On the **Search** box, enter the first few letters of the specific entities and select the correct entities from the list of suggestions. 

**9.** (Optional) To allow the assigned users to access any new entity instances created by them, select the **Allow users access to entities created by them** checkbox. 

For example, if a user has permission to create a new VM, the user must also have access to the VM. 

**10.** Click **Next** . 

**11.** From the **Users** dropdown list, select the user type. 

**12.** On the **Search** box, enter the first few letters of the user or user group's name and select the correct user or user group from the list of suggestions. 

**13.** Click **Save** . 

## **Editing an Authorization Policy** 

You can edit an authorization policy. 

## **About this task** 

To edit an authorization policy, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Authorization Policies** tab. 

AOS Security | Security Management Using Prism Central | **59** 

**4.** Select the checkbox associated with an authorization policy. 

**5.** Select **Actions** > **Edit** . 

**6.** In the **Edit Authorization Policy** window, edit the fields as needed and click **Save** . 

## **Duplicating an Authorization Policy** 

You can create a copy of predefined or custom authorization policy. 

## **About this task** 

To duplicate an authorization policy, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Authorization Policies** tab. 

**4.** Select the checkbox associated with an authorization policy. 

**5.** Select **Actions** > **Duplicate** . 

**6.** In the **Duplicate Authorization Policy** window, edit the fields as needed and click **Save** . 

## **Deleting an Authorization Policy** 

You can delete an authorization policy. 

## **Before you begin** 

Before you delete an authorization policy, consider the following points: 

- You can not delete any predefined system authorization policies. 

- Any users associated with the authorization policy lose their assigned access after you delete the authorization policy. 

## **About this task** 

To delete an authorization policy, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **IAM** . 

**3.** Select the **Authorization Policies** tab. 

**4.** Select the checkbox associated with an authorization policy. 

**5.** Select **Actions** > **Delete** . 

**6.** Click **Delete** . 

AOS Security | Security Management Using Prism Central | **60** 

## **Deleting a User or User Group** 

Delete local user and identity provider user or users group using nuclei commands. 

## **Before you begin** 

Before you delete a user or users group, consider the following points: 

- Remove any authorization policies associated with a user or user group, whether directly assigned (Prism Central version pc.2024.1 or later) or through project or role mapping (Prism Central versions prior to pc.2024.1). 

- Verify that the user you want to delete does not own any VMs or other entities. You must transfer or delete the entity ownership before deleting an account, see Editing an Authorization Policy on page 59. 

## **About this task** 

To delete Prism Central user or users group, follow these steps: 

## **Procedure** 

**1.** SSH into the Prism Central VM (PCVM) as a `nutanix` a user: 

   - `$ ssh nutanix@` ~~ee~~ _`pcvm_ip_address`_ 

**2.** List all users or user groups UUIDs: 

   - » List all users: 

> `nutanix@PCVM$ nuclei user.list` ~~SCS~~ 

- » List all user groups: 

   - `nutanix@PCVM$ nuclei user_group.list` ~~CSS~~ 

If there are more than 20 users or user groups, use the count flag to specify the number to list. For example, to list 100 users or user groups: 

> `nutanix@PCVM$ nuclei user.list count=100` ~~SSCS~~ 

> `nutanix@PCVM$ nuclei user_group.list count=100` ~~SSCS~~ 

**3.** Delete the user or user group: 

   - » Delete a user by UUID. 

> `nutanix@PCVM$ nuclei user.delete` ~~CSS~~ _`user_UUID`_ 

- » Delete a user group by UUID. 

> `nutanix@PCVM$ nuclei user_group.delete` ~~SCS~~ _`user_group_UUID`_ 

> Replace SS _`user_UUID`_ or _`user_group_UUID`_ with the actual UUID of the user or users group. 

## **Configuring Your Profile** 

Configure your profile information in Prism Central. 

In the Prism Central web console, you can manage your profile settings by updating personal information and credentials. You can modify your first name, last name, email address, and preferred language. You can also change your password to strengthen security, keeping your account details accurate and ensuring reliable access within the environment. 

AOS Security | Security Management Using Prism Central | **61** 

## **Updating Your Profile** 

You can update your profile information in Prism Central. 

## **About this task** 

To update your profile information in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** Click **User Menu** > **Update Profile** 

**3.** On the **Update Profile** window, enter the following information: 

   - **First Name** : Enter a first name. 

   - **Last Name** : Enter a last name. 

   - **Email Address** : Enter a valid user email address. 

   - **Language** : From the dropdown list, select the language setting. 

**4.** Click **Save** . 

## **Changing Your Password** 

You can change your profile's password in Prism Central. 

## **About this task** 

To change your profile's password in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** Click **User Menu** > **Change Password** 

**3.** On the **Change Password** window, enter the following information: 

   - **Current Password** : Enter the current password. 

   - **New Password** : Enter the new password. 

   - **Confirm Password** : Re-enter the new password. 

**4.** Click **Save** . 

## **Centralized Password Management Using System Accounts** 

Prism Central System Accounts page provides a centralized way to view, update, and track system account passwords across Prism Central, Controller VMs, and AHV. 

## **System Accounts Overview** 

You can use the System Accounts page to centrally manage all the system account passwords using Prism Central. The centralized management of passwords ensures enhanced account security by providing a direct view of the status of passwords (default or secure) and the ability to change the passwords of system accounts that are grouped under Prism Central, Controller VM, or AHV scope. 

AOS Security | Security Management Using Prism Central | **62** 

You can also use different viewing options for the system account passwords using the **Filters** option. Using filters, you can refine the view based on the following categories: 

## Status of the account password 

- Default: The password is default. 

- Secure: The password is custom and secure. 

## Account Type 

- Root (AHV-only) 

- Admin 

- Nutanix 

**Tip:** You can view the password change history using the Audit Dashboard or Tasks for traceability. 

## **Password Management Requirements** 

Supported Software Versions: 

- Prism Central version pc.2023.4 or later 

- AOS version 6.7.1 or later 

Password Management for AHV system accounts is supported for the following software versions: 

- Prism Central version pc.2024.3 or later 

- AOS version 7.0 or later 

- AHV version 10.0 or later 

Node-level password management for AHV root and admin (only if password is set) accounts is supported for the following software versions: 

- Prism Central version pc.7.3 or later 

- AOS version 7.3 or later 

- AHV version 10.3 or later 

## **Password Management Limitations** 

- The account passwords for a maximum of 10 clusters can be changed simultaneously. 

- Bulk password change for multiple user accounts on different clusters is not supported. 

- Password change for AHV system accounts is supported on clusters containing at least one hyperconverged infrastructure (HCI) node. However, AHV password change is not available on clusters composed entirely of compute-only (CO) nodes. 

- In case of successful AHV system account password change, the status of the password on the **System Accounts** page is updated immediately. 

## **Changing System Account Passwords** 

Use the System Accounts page in Prism Central to view and update passwords for system accounts across Prism Central, Controller VMs, and AHV hosts. 

AOS Security | Security Management Using Prism Central | **63** 

## **About this task** 

To change the password of one or multiple system accounts, follow these steps: 

## **Procedure** 

**1.** Log on to Prism Central as an administrator. 

**2.** Select the **Infrastructure** application from Application Switcher Function , and go to **Network & Security** > **System Accounts** from the **Navigation Bar** . Alternatively, go to the Accessing the Security Dashboard on page 85 and click **View all System Account Passwords** . 

The **System Accounts** page opens. This page provides the list of accounts and the password status for all the accounts configured on the registered clusters. 

The system accounts are grouped by component type: Prism Central, Controller VM (CVM), and Hypervisor (AHV). AHV accounts are displayed at the host level, allowing you to view and manage passwords per host where supported. 

**3.** To change the password of accounts on Prism Central and Controller VMs, select the account from the **Account** column and click **Change Password** . 

For AHV accounts, expand the cluster row to view associated hosts, then select the specific hosts where you need to change the password. 

**4.** In the **Change Password** window, enter the following and click **Change Password** . 

   - Existing account password 

   - New password 

   - Confirm new password 

The passwords are updated for the selected accounts. 

## **SSL Certificate Management in Prism Central** 

Manage SSL certificates in Prism Central to ensure secure access. 

Prism Central supports SSL certificate-based authentication for console access. Prism Central includes a default selfsigned SSL certificate to enable secure communication with a cluster. You can replace the default self-signed SSL certificate with your own self-signed SSL certificate or a certificate authority (CA) signed SSL certificate from either an internal third-party trusted CA or a public trusted CA of your choice. 

For production environments, Nutanix recommends that you replace the default self-signed certificate with a CAsigned SSL certificate. 

**Note:** Prism Central supports only a cluster-wide SSL certificate. You cannot customize the SSL certificate for an individual Controller VM (CVM). Nutanix recommends that you check for the validity of the certificate periodically and replace the certificate if it is invalid. 

## **Generating a Self-Signed SSL Certificate with Subject Alternative Name** 

Generate a self-signed SSL certificate with Subject Alternative Name (SAN) using OpenSSL to secure Prism Central. 

## **Before you begin** 

- The certificate import process validates that the key and certificate pair uses the correct signature algorithm to comply with NIST SP800-131a and RFC 6460 (NSA Suite B) standards. Use the correct key types, sizes, curves, and signature algorithms. For more information, see Supported Key Configurations for SSL Certificates on page 75. 

AOS Security | Security Management Using Prism Central | **64** 

- Nutanix recommends including a DNS name for all Prism Central VMs (PCVMs) in the self-signed SSL certificate using the SAN extension to avoid SSL certificate errors when accessing a PCVM through its DNS name instead of the shared cluster IP address. 

## **About this task** 

To generate a self-signed SSL certificate with SAN, follow these steps: 

## **Procedure** 

**1.** SSH into the Prism Central VM (PCVM) as a `nutanix` a user: 

   - `$ ssh nutanix@` ~~SSS~~ _`pcvm_ip_address`_ 

**2.** Generate a private key: 

   - RSA private key with a bit length of 2048: 

      - `nutanix@pcvm$ openssl genrsa -out` ~~ee~~ _`my_key_name.key`_ `2048` 

   - RSA private key with a bit length of 4096: 

      - `nutanix@pcvm$ openssl genrsa -out` ~~CSCC~~ _`my_key_name.key`_ `4096` 

   - ECDSA private key using the prime256v1 curve: 

> `nutanix@pcvm$ openssl ecparam -name prime256v1 -genkey -out` ~~SCS~~ _`my_key_name.pem`_ 

**Important:** While you are generating the private key, ensure that the private key is not password protected. 

**3.** Generate the Certificate Signing Request (CSR): 

   - For RSA 2048 and RSA 4096 private keys: 

`nutanix@pcvm$ openssl req -new -nodes -key my_key_name.key` _`-signature_algorithm`_ `- out my_csr_name.csr` 

> _`signature_algorithm: Specify sha256 or sha384 or sha512.`_ ~~Se~~ 

For example, to generate a CSR for RSA 2048 private key using SHA-256 signature algorithm: 

`nutanix@pcvm$ openssl req -new -nodes -key my_key_name.key -sha256 -out my_csr_name.csr` 

- For ECDSA 256 private key: 

`nutanix@pcvm$ openssl req -new -nodes -key my_key_name.pem -sha256 -out my_csr_name.csr` 

For example, to generate a CSR for ECDSA 256 private key using SHA-256 signature algorithm: 

`nutanix@pcvm$ openssl req -new -nodes -key my_key_name.pem -sha256 -out my_csr_name.csr` 

**4.** Enter the information in the command output to incorporate into your certificate request: 

`You are about to be asked to enter information that will be incorporated into your certificate request. What you are about to enter is what is called a Distinguished Name or a DN. There are quite a few fields but you can leave some blank For some fields there will be a default value, If you enter '.', the field will be left blank.` 

`-----` 

AOS Security | Security Management Using Prism Central | **65** 

`Country Name (2 letter code) []:# Enter the 2 letter country code State or Province Name (full name) []:# Enter the full name of the state or province Locality Name (eg, city) []:# Enter the name of the city or locality Organization Name (eg, company) []: # Enter the legal name of your organization or company Organizational Unit Name (eg, section) []: # Enter the department or business unit Common Name (eg, fully qualified host name) []:# Enter the fully qualified domain name (FQDN) of the server Email Address []:# Enter a valid email address Please enter the following 'extra' attributes to be sent with your certificate request A challenge password []: # (Optional) Enter your password` 

**5.** Create a configuration file in your home directory with your preferred text editor named _`san.cnf`_ a that contains the following text: 

`[req] distinguished_name = req_distinguished_name req_extensions = v3_req [req_distinguished_name] [v3_req] basicConstraints = CA:FALSE keyUsage = nonRepudiation, digitalSignature, keyEncipherment subjectAltName = @alt_names` 

`[alt_names] DNS.0 = example1.domain.com # Primary domain or fully qualified domain name (FQDN) of the server DNS.1 = example2.domain.com # Secondary domain or alias for the server DNS.2 = example3.domain.com # Additional domain name that should be trusted DNS.3 = *.domain.com # Wildcard entry to match any subdomain under domain.com IP.0  = x.x.x.x # IP address of the Prism Central IP.1  = y.y.y.y # Additional IP address of the Prism Central` 

> _`[alt_names]`_ ~~a~~ Specify your DNS and IP addresses. If you have a range of hosts, use wildcards (*) to match any subdomain of the domain name. 

**6.** Generate a self-signed certificate: 

`nutanix@pcvm$ openssl x509 -req -days` _`number_of_days`_ `-in my_csr_name.csr -signkey my_key_name.key -out my_crt_name.crt` _`-signature_algorithm`_ `-extensions v3_req - extfile san.cnf` 

> _`number_of_days`_ es : Specify the number of days until a newly generated certificate expires. 

> _`signature_algorithm`_ ee : Specify sha256, sha384, or sha512. Ensure that you use the same signature algorithm that was used to generate the CSR. 

Example: 

`nutanix@pcvm$ openssl x509 -req -days 1460 -in my_csr_name.csr -signkey my_key_name.key -out my_crt_name.crt -sha256 -extensions v3_req -extfile san.cnf` 

**7.** Copy SS _`my_key_name.key`_ and _`my_crt_name.crt`_ from the PCVM to your local machine: 

`nutanix@pcvm$ scp my_key_name.key my_crt_name.crt` _`username@local-machine:/ local_file_path/`_ 

AOS Security | Security Management Using Prism Central | **66** 

## **What to do next** 

After generating the self-signed certificate with a private key, follow the procedure described in Importing a Self-Signed SSL Certificate in Prism Central on page 70 to replace the default certificate with your selfsigned SSL certificate. The following table lists certificate components and its corresponding file type to choose when SSL certificate window prompts: 

**Table 3: SSL Certificate Import Files** 

|**Certificate Components**|**File type**|
|---|---|
|Private Key|my_key_name.key|
|Public Certificate|my_crt_name.crt|
|CA Certificate/Chain|my_crt_name.crt|



## **Generating a Certificate Signing Request with Subject Alternative Name for Submission to Certificate Authority** 

Generate a Certificate Signing Request (CSR) with Subject Alternative Name (SAN) using OpenSSL for Certificate Authority (CA) submission. 

## **Before you begin** 

- The certificate import process validates that the key and certificate pair uses the correct signature algorithm to comply with NIST SP800-131a and RFC 6460 (NSA Suite B) standards. Use the correct key types, sizes, curves, and signature algorithms. For more information, see Supported Key Configurations for SSL Certificates on page 75. 

- Nutanix recommends including a DNS name for all Prism Central VMs (PCVMs) in the CSR using the SAN extension to avoid SSL certificate errors when accessing a PCVM through its DNS name instead of the shared cluster IP address. 

## **About this task** 

To generate a CSR with SAN, follow these steps: 

## **Procedure** 

**1.** SSH into the Prism Central VM (PCVM) as a `nutanix` a user: 

   - `$ ssh nutanix@` ~~ee~~ _`pcvm_ip_address`_ 

**2.** Create a configuration file in your home directory with your preferred text editor named _`ssl.cnf`_ a that contains the following text: 

`[req] distinguished_name = req_distinguished_name req_extensions = v3_req prompt = no [req_distinguished_name] countryName = # Country Name (2 letter code) stateOrProvinceName = # State or Province Name (full name) localityName = # Locality Name (eg, city) organizationName = # Organization Name (eg, company) organizationalUnitName = # Organizational Unit Name (eg, BU) commonName = # Common Name (e.g. server FQDN or YOUR name)` 

AOS Security | Security Management Using Prism Central | **67** 

`emailAddress = # Email Address [v3_req] subjectAltName = @alt_names [alt_names] DNS.0 = example1.domain.com # Primary domain or fully qualified domain name (FQDN) of the server DNS.1 = example2.domain.com # Secondary domain or alias for the server DNS.2 = example3.domain.com # Additional domain name that should be trusted DNS.3 = *.domain.com # Wildcard entry to match any subdomain under domain.com IP.0  = x.x.x.x # IP address of the Prism Central IP.1  = y.y.y.y # Additional IP address of the Prism Central` 

> _`[alt_names]`_ ~~a~~ - Specify your DNS and IP addresses. If you have a range of hosts, use wildcards (*) to match any subdomain of the domain name. 

**3.** Generate a private key: 

   - RSA private key with a bit length of 2048: 

      - `nutanix@pcvm$ openssl genrsa -out` ~~ee~~ _`my_key_name.key`_ `2048` 

   - RSA private key with a bit length of 4096: 

      - `nutanix@pcvm$ openssl genrsa -out` ~~CSCC~~ _`my_key_name.key`_ `4096` 

   - ECDSA private key using the prime256v1 curve: 

      - `nutanix@pcvm$ openssl ecparam -name prime256v1 -genkey -out` ~~SCS~~ _`my_key_name.pem`_ 

**Important:** While you are generating the private key, ensure that the private key is not password protected. 

## **4.** Generate a CSR: 

- For RSA 2048 and RSA 4096 private keys: 

`nutanix@pcvm$ openssl req -new -nodes -key my_key_name.key` _`-signature_algorithm`_ `- out my_csr_name.csr -config ssl.cnf` 

> _`signature_algorithm: Specify sha256 or sha384 or sha512.`_ ~~Se~~ 

For example, to generate a CSR for RSA 2048 private key using SHA-256 signature algorithm: 

`nutanix@pcvm$ openssl req -new -nodes -key my_key_name.key -sha256 -out my_csr_name.csr -config ssl.cnf` 

- For ECDSA 256 private key: 

`nutanix@pcvm$ openssl req -new -nodes -key my_key_name.pem -sha256 -out my_csr_name.csr -config ssl.cnf` 

For example, to generate a CSR for ECDSA 256 private key using SHA-256 signature algorithm: 

`nutanix@pcvm$ openssl req -new -nodes -key my_key_name.pem -sha256 -out my_csr_name.csr -config ssl.cnf` 

**5.** Copy SS _`my_key_name.key`_ and _`my_crt_name.csr`_ from the PCVM to your local machine: 

`nutanix@pcvm$ scp my_key_name.key my_csr_name.csr` _`username@local-machine:/ local_file_path/`_ 

**6.** Log out of the PCVM. 

AOS Security | Security Management Using Prism Central | **68** 

**7.** Send your CSR file to the CA of your choice. 

After receiving your CSR, the CA sends the following files: 

- CA signed public certificate 

- CA's public certificate 

- Root CA public certificate (if the CA is intermediate) 

The issuing CA validates the public certificate. If the issuing CA is intermediate, the root CA validates the issuing CA certificate. The system validates the certificate chain to establish trust. 

**8.** Download all the certificate files you receive from CA to the local file directory. 

**9.** (Optional) If the CA chain certificate provided by the certificate authority is not in a single file, run the following command to concatenate the list of CA certificates into a chain file: 

   - `$ cat intermediateCAcert.crt rootCAcert.crt > ca_chain_certs.crt` ~~ee~~ 

Start the chain with the signer’s certificate and end it with the root CA certificate. 

Ensure that the chain file only has the root and intermediate certificates. Prism Central fails to import the chain file if it contains public or private certificates. 

## **What to do next** 

Follow Importing a CA-Signed SSL Certificate in Prism Central on page 73 section to replace the default certificate with a CA-signed certificate. The following table lists certificate components and its corresponding file type to choose when SSL certificate window prompts: 

**Table 4: SSL Certificate Import Files** 

|**Certificate Components**|**File type**|
|---|---|
|Private Key|my_key_name.key|
|Public Certificate|ca_signed_public_cert.cer|
|CA Certificate/Chain|ca_public_cert.crt or ca_chain_certs.crt|



## **Verifying the Certificate Generation Request** 

Run the following commands to verify the certificate generation request. 

- Verify that the CA certificate chain is valid: 

   - ~~CSS~~ `nutanix@pcvm$ openssl verify -CAfile ca_chain_certs.crt myPublicCert.cer` 

Example output: 

~~CSS~~ `myPublicCert.cer: OK` 

- Verify the private key and signature algorithm details: 

`nutanix@pcvm$ openssl x509 -in my_cert_name.crt -text -noout | grep -i 'rsa\|ecdsa\| Public'` 

Example output: 

`Signature Algorithm: ecdsa-with-SHA256 Subject Public Key Info: Public Key Algorithm: id-ecPublicKey` 

AOS Security | Security Management Using Prism Central | **69** 

`Public-Key: (256 bit) Signature Algorithm: ecdsa-with-SHA256` 

- Verify that the CA certificate chain uses SHA 256 as the signature algorithm: 

`nutanix@pcvm$ openssl crl2pkcs7 -nocrl -certfile ca_chain_certs.crt | openssl pkcs7 - print_certs -noout -text | grep -Ew '(Subject|Issuer|Signature Algorithm):' | grep - C1 Issuer` 

## **Troubleshooting the Certificate Generation Request** 

The following troubleshooting tips can help you resolve common issues that can occur when generating certificates. 

## **Chain certificate format** 

If your chain certificate file has public or private certificates, it will fail to import in Prism Central. Ensure that the chain certificate file only has the root and intermediate certificates. 

For example, if a public certificate is present in a chain file, you can remove it by opening your chain file in your preferred text editor. Ensure that there are no extra white spaces at the bottom of the file. 

## **DER-encoded certificate issue** 

If the certificate is DER encoded, it fails to import in Prism Central. You can resolve the issue by converting it to PEM-encoded ASCII format. 

- Ensure that the certificate is DER encoded: 

   - ~~SSC~~ `nutanix@pcvm$ openssl x509 -in cert.crt -inform der -text -noout` 

- If the certificate is DER encoded, run the following command to convert the certificate from DER to PEMencoded ASCII format: 

~~ee~~ `nutanix@pcvm$ openssl x509 -in certDER.crt -inform der -outform pem -out cert.crt` 

## **Certificate format** 

> Ensure that all the certificates do not have any extra data (or custom attributes) before the beginning ~~a~~ `(-----BEGIN CERTIFICATE-----)` or after the end `(-----END CERTIFICATE-----)` of the block. ~~ee SE~~ 

## **Importing a Self-Signed SSL Certificate in Prism Central** 

Import a self-signed SSL certificate into Prism Central. 

## **Before you begin** 

Ensure that you generate a self-signed SSL certificate. For information, see Generating a Self-Signed SSL Certificate with Subject Alternative Name on page 64. 

## **About this task** 

To import a self-signed SSL certificate into Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **Settings** . 

**3.** Under Security, select **SSL Certificate** . 

AOS Security | Security Management Using Prism Central | **70** 

**4.** Click **Replace Certificate** . 

**5.** Select **Import Key and Certificate** and click **Next** . 

AOS Security | Security Management Using Prism Central | **71** 

**6.** On the SSL certificate window, provide the following information: 

   - **Private Key Type** : From the dropdown list, select the appropriate private key type for the self-signed certificate. 

**Note:** EC DSA 521 and EC DSA 384 bit private keys are not supported. 

- **Private Key** : Click **Choose file** and select the private key. 

**Note:** The private key that you import must be unencrypted. Password-protected private keys are not supported. 

- **Public Certificate** : Click **Choose file** and select the self-signed certificate corresponding to the private key. 

- **CA Certificate/Chain** : Click **Choose file** select the self-signed certificate corresponding to the private key. 

**Figure 3: Importing self-signed certificate** 

The following table lists certificate components and its corresponding file type to choose when SSL certificate window prompts: 

|**Certificate Components**|**File type**|
|---|---|
|Private Key|my_key_name.key|
|Public Certificate|my_crt_name.crt|
|CA Certificate/Chain|my_crt_name.crt|



AOS Security | Security Management Using Prism Central | **72** 

## **7.** Click **Import Files** . 

**Note:** Prism Central stores only one custom SSL certificate. When you upload a new certificate, Prism Central replaces the existing certificate. 

After you import the new certificate, the Prism Central web console restarts. If the certificate and credentials are valid, Prism Central uses the new certificate immediately. All open browser sessions becomes invalid until you reload the page and accept the new certificate. If the certificate is invalid due to corrupted file or wrong certificate type, Prism Central discards the new certificate and reverts to the default certificate provided by Nutanix. 

## **Importing a CA-Signed SSL Certificate in Prism Central** 

Import a certificate authority (CA) signed SSL certificate into Prism Central. 

## **Before you begin** 

Ensure that you generate a Certificate Signing Request (CSR) for submission to a CA. For more information, see Generating a Certificate Signing Request with Subject Alternative Name for Submission to Certificate Authority on page 67. 

## **About this task** 

To import a CA-signed SSL certificate into Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **Settings** . 

**3.** Under Security, select **SSL Certificate** . 

**4.** Click **Replace Certificate** . 

**5.** Select **Import Key and Certificate** and click **Next** . 

AOS Security | Security Management Using Prism Central | **73** 

**6.** On the SSL certificate window, provide the following information: 

   - **Private Key Type** : From the dropdown list, select the appropriate private key type for the CA signed certificate. 

**Note:** Prism Central does not support EC DSA 521 and EC DSA 384 bit private keys. 

- **Private Key** : Click **Choose file** and select the private key. 

**Note:** The private key that you import must be unencrypted. Password-protected private keys are not supported. 

- **Public Certificate** : Click **Choose file** and select the CA signed public portion of the certificate corresponding to the private key. 

- **CA Certificate/Chain** : Click **Choose file** and select the certificate or chain of the signing authority for the public certificate. 

**Note:** For more information on how to create a chain file from the list of CA certificates, see Generating a Certificate Signing Request with Subject Alternative Name for Submission to Certificate Authority on page 67. 

**Figure 4: Importing CA-signed certificate** 

The following table lists certificate components and its corresponding file type to choose when SSL certificate window prompts: 

AOS Security | Security Management Using Prism Central | **74** 

**Table 5: CA Signed Certificate File Type** 

|**Certificate Components**|**File type**|
|---|---|
|Private Key|my_key_name.key|
|Public Certificate|ca_signed_public_cert.cer|
|CA Certificate/Chain|ca_public_cert.crt or ca_chain_certs.crt|



**7.** Click **Import Files** . 

**Note:** Prism Central stores only one custom SSL certificate. When you upload a new certificate, Prism Central replaces the existing certificate. 

After you import the new certificate, the Prism Central web console restarts. If the certificate and credentials are valid, Prism Central uses the new certificate immediately. All open browser sessions becomes invalid until you reload the page and accept the new certificate. If the certificate is invalid due to corrupted file or wrong certificate type, Prism Central discards the new certificate and reverts to the default certificate provided by Nutanix. 

## **Regenerating a Self-Signed Certificate in Prism Central** 

Regenerate a self signed SSL certificate in Prism Central. 

## **About this task** 

To regenerate a self signed certificate in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **Settings** . 

**3.** Under Security, select **SSL Certificate** . 

**4.** Click **Replace Certificate** . 

**5.** Select **Regenerate Self Signed Certificate** and click **Apply** . 

**6.** Click **OK** . 

Prism Central generates and applies a new RSA 2048 bit self-signed certificate. 

## **Supported Key Configurations for SSL Certificates** 

Nutanix supports only the following key types, sizes or curves, and signature algorithms for SSL certificates: 

**Table 6: SSL Certificate Key Type Options** 

|**Key Type**|**Size or Curve**|**Signature Algorithm**|
|---|---|---|
|RSA|4096|SHA-256, SHA-384 or SHA512|
|RSA|2048|SHA-256, SHA-384 or SHA512|



AOS Security | Security Management Using Prism Central | **75** 

|**Key Type**|**Size or Curve**|**Signature Algorithm**|
|---|---|---|
|EC DSA 256|prime256v1|ecdsa-with-sha256|



In Prism Central versions prior to pc.2024.3, uploading an RSA 4096-bit certificate might cause some issues. For more information, see KB 12775 . 

Nutanix does not support SHA-1 certificates, including root CAs. 

Client and CAC authentication support only RSA 2048-bit certificates. 

## **Cluster Lockdown in Prism Central** 

Nutanix supports key-based SSH access to Prism Central. 

Cluster lockdown in Prism Central enhances security by disabling the password-based SSH login. Instead, authorized user SSH keys provide access to Prism Central, which improves the overall security posture. 

Nutanix supports the following key-based SSH encryption algorithms: 

- AES128-CTR 

- AES192-CTR 

- AES256-CTR 

Nutanix supports the following key types: 

- RSA 

- ECDSA 

## **Cluster Lockdown Considerations** 

Before you configure the cluster lockdown in Prism Central, consider the following points: 

- Generate the public key using the `ssh-keygen` command on Mac or Linux, or the PuTTY application on Windows. For more information, see KB-1895 . 

- When you enable the cluster lockdown, Prism Central does not store the passwords for both the Prism Central VM (PCVM) and host. You cannot change these passwords to access the cluster resources. 

- Adding a user SSH key enables SSH access for both the `nutanix` and `admin` accounts on the PCVM and host. 

- Disabling remote login and deleting all user SSH keys locks down cluster SSH access. 

- You can configure multiple user SSH keys. 

- For additional security, you can configure SSH security level for the PCVM. For more information, see PCVM Security Hardening on page 164. 

## **Configuring Cluster Lockdown in Prism Central** 

Configure cluster lockdown to enable SSH key-based access to your cluster. 

## **Before you begin** 

Ensure that you generate the public key using `ssh-keygen` command on Mac or Linux, or the PuTTY application on Windows. For more information, see KB-1895. 

## **About this task** 

To configure the cluster lockdown in Prism Central, follow these steps: 

AOS Security | Security Management Using Prism Central | **76** 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **Settings** . 

**3.** Under Security, select **Cluster Lockdown** . 

**4.** Deselect the **Enable Remote Login with Password** checkbox. 

**5.** Click **+ New user SSH Key** and enter the following information: 

   - **Name** : Enter a key name. 

   - **Key** : Paste the public key value. 

**6.** Click **Save** . 

## **Deleting a User SSH Key in Prism Central** 

You can disable the SSH key-based access to your cluster. 

## **About this task** 

**Caution:** Disabling remote login and deleting all user SSH keys locks down the cluster SSH access. Ensure to select the **Enable Remote Login with Password** checkbox before deleting all the user SSH keys. 

To remove the user SSH key, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **Settings** . 

**3.** Under Security, select **Cluster Lockdown** . 

**4.** Select the **Enable Remote Login with Password** checkbox. 

**5.** Click the **X** remove icon. 

**6.** Select **OK** . 

## **Security Policies using Nutanix Flow** 

Nutanix Flow secures traffic with microsegmentation policies. 

Nutanix Flow provides a policy-driven security framework to control and inspect application traffic within the data center. You can define microsegmentation policies that restrict east-west traffic between VMs, enforce application isolation, and apply service-specific rules. These policies improve visibility, reduce attack surfaces, and ensure compliance with organizational security requirements. For more information, see the Flow Microsegmentation Guide . 

## **Data-in-Transit Encryption** 

Data-in-transit encryption encrypts service-level traffic between cluster nodes to protect data from unauthorized access. 

AOS Security | Security Management Using Prism Central | **77** 

Data-in-Transit encryption, along with data-at-rest encryption, protects the entire life cycle of data and is an essential countermeasure against unauthorized access of critical data. 

For information on licensing Data-in-transit encryption and other Nutanix products, see Nutanix Cloud Platform Software Options . 

Before you configure data-in-transit encryption, consider the following points: 

- Data-in-transit encryption might have an impact on I/O latency and CPU performance. 

- Intra-cluster traffic encryption is supported only for the Stargate service. 

- RDMA traffic encryption is not supported. 

- When a CVM goes down, the traffic from guest VM to remote CVM is not encrypted. 

- Traffic between guest VMs connected to volume groups is not encrypted when the target disk is on a remote CVM. 

## **Enabling Data-in-Transit Encryption** 

Encrypt service-level traffic between the cluster nodes. 

## **About this task** 

To enable data-in-transit encryption, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Infrastructure** application and from the navigation bar, select **Hardware** > **Clusters** . 

**3.** Select the checkbox associated with the cluster. 

## **4.** Click **Actions** > **Enable Data-in-Transit Encryption** . 

**5.** Click **Enable** . 

## **Disabling Data-in-Transit Encryption** 

You can disable data-in-transit encryption. 

## **About this task** 

To disable data-in-transit encryption, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Infrastructure** application and from the navigation bar, select **Hardware** > **Clusters** . 

**3.** Select the checkbox associated with the cluster. 

**4.** Click **Actions** > **Disable Data-in-Transit Encryption** . 

**5.** Click **Disable** . 

AOS Security | Security Management Using Prism Central | **78** 

## **Securing AHV VMs with a Virtual Trusted Platform Module** 

You can configure VMs with a vTPM (virtual Trusted Platform Module) in Prism Central. 

A vTPM is a feature available on AHV that allows VMs to use the software-based TPM 2.0 specification that works as a virtual device without requiring physical hardware TPMs. The vTPM provides the same functionality as a physical TPM but within the virtualized environment that allows VMs to leverage TPM capabilities without needing dedicated hardware. 

You can also enable the vTPM using aCLI. For more information, see Securing AHV VMs with Virtual Trusted Platform Module (aCLI) in the _AHV Administration Guide_ . 

To secure vTPM-enabled VMs, you must back up the keys regularly and store the backup file securely in an external location. For more information, see vTPM Integration with External KMS on page 81. 

## **Virtual Trusted Platform Module Use Cases** 

A vTPM provides virtualization-based security support for the following primary use cases: 

- **Microsoft Credential Guard** : vTPM is utilized to protect Windows credentials. For more information on Microsoft Windows Defender Credential Guard, see Microsoft documentation. 

- **Windows 11 Installation** : Windows 11 requires TPM 2.0 and Secure Boot enabled for optimal security. For more information on Windows 11 specifications, features, and system requirements, see Microsoft documentation. 

- **Secure Boot for Windows** : vTPM is compatible with Secure Boot, enhancing the system security. For information on how to create and update a guest VM with Secure Boot enabled, see Creating a VM through Prism Central (AHV) and Managing a VM through Prism Central (AHV) sections in _Prism Central Infrastructure Guide_ . 

- **Windows BitLocker** : vTPM supports Windows BitLocker with certain restrictions. For more information, see Virtual Trusted Platform Module Limitations on page 79. 

## **Virtual Trusted Platform Module Requirements** 

Understand the requirements for a virtual Trusted Platform Module (vTPM) in Prism Central. 

Supported software versions: 

- pc.2022.9 or later 

- AHV version 20220304.242 or later 

- AOS version 6.5.1 or later 

VM requirements: 

- You must enable UEFI firmware. For more information, see UEFI Support for VM . 

- You must enable Secure Boot if you are using Windows BitLocker. For more information, see Creating/ Updating a VM with Secure Boot Enabled . 

## **Virtual Trusted Platform Module Limitations** 

Understand the limitations of a virtual Trusted Platform Module (vTPM) in Prism Central. 

- The Secure Boot limitations apply to vTPM-enabled VMs. For more information, see Secure Boot limitations . 

- The Nutanix Disaster Recovery limitations apply when protecting vTPM-enabled VMs. For more information, see Disaster Recovery limitations . 

AOS Security | Security Management Using Prism Central | **79** 

- If you take a third-party backup of the vTPM-enabled guest VMs, the vTPM data including crucial encryption keys is not protected because third-party backup software does not have access to the data. Therefore, third-party backups of vTPM-enabled guest VMs cannot protect the vTPM device data. 

- If the data stored in the vTPM device is critical, such as encryption keys for Windows BitLocker, restoring a VM from a third-party backup might render it unusable and result in unrecoverable data loss. 

- If the data in the vTPM device is non critical, such as for Windows Secure Boot, the backup software or user must attach a new vTPM device during the restore process. 

## **Configuring a VM with a Virtual Trusted Platform Module** 

Enable a virtual Trusted Platform Module (vTPM) feature to secure virtual machines running on AHV. 

## **Before you begin** 

For more information on VM creation settings, see Creating a VM in _Prism Central Infrastructure Guide_ . 

## **About this task** 

To enable the vTPM when creating a VM, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Infrastructure** application and from the navigation bar, select **Compute** > **VMs** . 

**3.** Select the **List** tab. 

**4.** Click **Create VM** . 

**5.** Navigate to the **Resources** tab and perform the following actions: 

   - **Boot Configuration** : Select **UEFI BIOS Mode** option. 

   - **Shield VM Security Settings** : Select the **Attach vTPM** checkbox. 

## **Figure 5: Attach vTPM** 

**6.** At the subsequent VM setting tabs, click **Next** . 

**7.** Click **Save** . 

## **What to do next** 

Power on the VM to verify if the vTPM configuration is applied on the VM. 

AOS Security | Security Management Using Prism Central | **80** 

## **Enabling a Virtual Trusted Platform Module on an Existing VM** 

Update the settings of an existing VM to enable a virtual Trusted Platform Module (vTPM). 

## **Before you begin** 

Before you enable the vTPM on an existing VM, consider the following points: 

- The UEFI BIOS mode is enabled for boot configuration in the existing VM. 

- Power off the VM before you enable the vTPM. 

## **About this task** 

To enable the vTPM on the existing VM, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Infrastructure** application and from the navigation bar, select **Compute** > **VMs** . 

**3.** Select the **List** tab. 

**4.** Select the checkbox associated with the VM. 

**5.** Click **Actions** > **Update** 

**6.** Navigate to the **Resources** tab and select **Attach vTPM** checkbox. 

## **Figure 6: Attach vTPM** 

**7.** At the subsequent VM setting tabs, click **Next** . 

**8.** Click **Save** . 

## **What to do next** 

Power on the VM to verify if the vTPM configuration is applied on the VM. 

## **vTPM Integration with External KMS** 

vTPM keys are protected using an external KMS when software data-at-rest encryption is enabled. 

When software data-at-rest encryption is enabled and an external KMS is configured through Prism Central, AHV vTPM keys are encrypted and stored in that KMS. The external KMS manages the key lifecycle, including validation and availability checks, using the same trust model as data-at-rest encryption. For more information on configuring an external KMS using Prism Central, see External Key Management Server on Prism Central on page 90. 

If no external KMS is configured, vTPM keys are stored and protected by the native key manager. In all cases, vTPM keys are included in key backups for recovery. 

AOS Security | Security Management Using Prism Central | **81** 

Ensure that the external KMS remains reachable and perform periodic key backups. During recovery, the key backup file is required for vTPM-enabled VMs to start successfully. For more information on performing a key backup manually, see Backing up encryption keys using Prism Element on page 82. 

**Caution:** If the backup file is not available, vTPM-enabled VMs might fail to start after recovery. 

## **Backing up encryption keys using Prism Element** 

Back up software data-at-rest encryption keys to ensure that vTPM-enabled VMs can start successfully after recovery. 

## **About this task** 

To backup software data-at-rest encryption keys, follow these steps. 

## **Procedure** 

**1.** Log on to the Prism Element cluster using SSH. 

**2.** Back up the software data-at-rest encryption keys. 

`nutanix@cvm$ ncli data-at-rest-encryption backup-software-encryption-keys filepath=` _`path`_ `password=` _`password`_ 

Replace _`path`_ and _`password`_ with values appropriate for your environment. 

## **Disabling a Virtual Trusted Platform Module** 

Disable a virtual Trusted Platform Module (vTPM) in a VM. 

## **Before you begin** 

Before you disable the vTPM, consider the following points: 

- Disabling a vTPM might severely affect VM functionality or result in data loss. For example, if your Windows BitLocker key is stored in a vTPM, ensure that you have the recovery key before disabling a vTPM. 

- Power off the VM before disabling a vTPM. 

## **About this task** 

To disable the vTPM, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Infrastructure** application and from the navigation bar, select **Compute** > **VMs** . 

**3.** Select the **List** tab. 

**4.** Select the checkbox associated with the VM. 

**5.** Click **Actions** > **Update** 

**6.** Navigate to the **Resources** tab and deselect **Attach vTPM** checkbox. 

**7.** At the subsequent VM setting tabs, click **Next** . 

**8.** Click **Save** . 

AOS Security | Security Management Using Prism Central | **82** 

## **Security Dashboard** 

Monitor and manage the security posture of your registered clusters with the customizable Security Dashboard, tracking key metrics and compliance. 

The Security Dashboard provides dynamic summary of the security posture across all the registered clusters. The Security Dashboard allows you to view the most critical security parameters like cluster-based issue summary, STIG policy compliance, security hardening, identified vulnerabilities, and the local account password status. The security dashboard is divided into multiple widgets to represent different security focus areas. 

**Note:** The Security Dashboard does not report security issues for user VMs. 

The following figure provides a sample view of the Security Dashboard with the default widgets: 

**Figure 7: Security Dashboard Overview** 

## **Security Dashboard Requirements** 

Understand the requirements of the Security Dashboard feature: 

- Your cluster must run AOS 6.6 or later and pc.2022.9 or later. 

**Note:** Ensure your cluster uses a compatible AHV or ESXi hypervisor. For software compatibility details, see the Compatibility and Interoperability Matrix on the Nutanix Support portal. 

- Starting from Prism Central version pc.2024.3.1, certain features of the Security Dashboard, such as the **Summary** widget, are available only on clusters running AOS 7.0 or later. 

- Only users with the Prism Admin role can access the Security Dashboard feature. Log in with an account that has Prism Admin role assignment to view and manage the Security Dashboard feature. 

- Starting with pc.7.5 release, the Service Manager feature by default supports deployment of Security Dashboard service in a dark site. 

AOS Security | Security Management Using Prism Central | **83** 

- Security Dashboard feature is not available on the Prism Central instances deployed on Nutanix Cloud Clusters (NC2). 

- Certain Security Dashboard widgets are available only on specific AOS versions: 

   - **Security Hardening and Vulnerabilities** widget requires AOS 6.6 or later. 

   - **System Account Passwords** widget requires AOS 6.7.1 or later. 

## **Security Dashboard Widget in Prism Central Dashboard** 

View security issue summaries across clusters from the Prism Central main dashboard. 

The Security Dashboard widget on the Prism Central main dashboard displays the total number of security issues across your clusters. The issue count is also available based on categories including security hardening, STIG issues, and vulnerabilities. 

Click **View All Issues** to access the Security Dashboard for detailed information on the security posture of all registered clusters. 

**Figure 8: Security Dashboard Widget** 

## **Security Dashboard Wizard** 

Take a guided tour of the Security Dashboard in Prism Central. 

The Security Dashboard wizard allows you to take a guided tour and navigate through the various tasks and workflows in the **Security Dashboard** in Prism Central. 

The Security Wizard automatically presents as a dialog-box when you access the dashboard feature for the first time. Click **Start Tour** to begin the dashboard walkthrough. 

Optionally, you can click **Skip for Now** to access the dashboard directly. To access this tour later, click the help menu icon (?), expand **New in Prism Central** , and select **Security Dashboard** . 

AOS Security | Security Management Using Prism Central | **84** 

## **Accessing the Security Dashboard** 

Access the Security Dashboard from Prism Central. 

## **About this task** 

To access the Security Dashboard from Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** Do one of the following: 

   - » From the Application Switcher Function , select the **Infrastructure** application and from the navigation bar, select **Network & Security** > **Security Dashboard** . 

   - » From the Prism Central main dashboard, under **Security** widget, click **View All Issues** . 

## **Security Dashboard Details Page** 

Monitor your cluster security posture in a single customizable view. 

Security Dashboard is a customizable and dynamic console that provides a unified view of your Nutanix infrastructure's security. 

By default, the Security Dashboard displays the following information widgets: 

- Summary on page 85 

- STIG Policy on page 86 

- Security Hardening on page 86 

- Vulnerabilities on page 87 

- System Account Passwords on page 88 

Use the **View** menu on the dashboard to switch between different cluster views. The widget data updates dynamically based on the following options: 

- All clusters 

- Individual cluster 

- Selection of clusters 

## **Summary** 

The **Summary** widget allows you to view your open security issues or focus on the clusters that have the most number of security issues. You can click the Summary pie graph to view the following information. 

- Total number of issues in the clusters 

- Number of issues in the clusters categorized based on the following issue categories: 

   - Security Hardening 

   - STIG Issues 

   - Vulnerabilities 

   - Local Account Passwords 

AOS Security | Security Management Using Prism Central | **85** 

**Tip:** The Security Dashboard refreshes once daily. Updating the STIG check based vulnerabilities information requires a manual refresh. You can initiate a manual refresh by clicking the refresh icon at the bottom of the widget. Manual refresh process takes some approximately 20 minutes to 2 hours to complete and depends on the number of clusters in your environment. 

## **STIG Policy** 

STIG helps detect deviation from the security baseline configuration of the operating system and hypervisor to remain in compliance. Nutanix has implemented the Controller VM to support STIG compliance with the RHEL 8 STIG as published by DISA. For more information, see _Security Dashboard STIG Guidance Reference_ . 

The **STIG Policy** widget provides an accurate snapshot of policy violations or deviations (resulting in failure) from the baseline STIG policy. This widget displays the STIG policy compliance status according to controls and resources. 

**Note:** The STIG checks are run only on the node running primary Prism Element. 

|**Status by Controls (Unique)**|Number of unique STIG controls that are not met.|
|---|---|
|**Status by Resources (Total)**|Total number of individual resources that have failed a<br>set of STIG controls.|



Click **View Failed Controls** to view the list of STIG controls that are not met. 

## **STIG Policy - Failed Controls** 

The STIG Policy -Failed Controls dialog box allows you to view STIG controls that do not meet the required settings. 

## **Security Hardening** 

Use the **Security Hardening** widget to view the status of security hardening controls that are applied to your clusters and to configure multiple security hardening controls directly. 

The **Security Hardening** widget includes the following security hardening configurations: 

## High-Strength Password 

Enables the Nutanix-recommended high-strength password policy. After enabling this setting, you must update the Controller VM and AHV host passwords. 

## Advanced Intrusion Detection Environment (AIDE) 

Enables AIDE to monitor operating system files on cluster nodes. AIDE runs weekly to detect unauthorized file modifications. 

**Important:** Nutanix recommends enabling AIDE for all clusters. 

## Security Configuration Management Automation (SCMA) 

Enables SCMA frequency for AHV hosts and Controller VM. SCMA checks multiple security entities for both Nutanix storage and AHV. For more information, see Security Configuration Management Automation Implementation on page 8. 

By default, SCMA scanning is set to **Daily** for both AHV and Controller VM. You can configure the scan frequency to **Hourly** , **Daily** , **Weekly** , or **Monthly** individually for Controller VM and AHV hosts. 

**Note:** Higher frequency SCMA scans can decrease performance on the respective cluster. 

## Cluster Lockdown 

Enables **Cluster Lockdown** mode to restrict administrative access and enhance security. For more information, see Configuring Cluster Lockdown using Security Dashboard on page 87. 

AOS Security | Security Management Using Prism Central | **86** 

## Defense Consent Banner 

Enables **Defense Consent Banner** . Once enabled, the system displays the consent banner of the US Department of Defense when connecting with SSH. You must enable the Defense Consent Banner individually for Controller VM and AHV hosts. This banner is customizable, but you must reconfigure it after every AOS upgrade. 

## Host Secure Boot 

Enables **Host Secure Boot** to prevent unauthorized firmware or software from running on AHV or ESXi hosts during the boot process. Host secure boot is a unified extensible firmware interface (UEFI) firmware feature that ensures only trusted system components are allowed to run at startup. For more information on enabling and verifying host secure boot, see Host Secure Boot (UEFI) . 

## Network Segmentation 

Shows the network segmentation status for each cluster. The network segmentation isolates CVM traffic types on separate VLANs or physical networks to improve security and performance. Configure network segmentation from the network configuration settings page in Prism Element. For more information, see Securing Traffic Through Network Segmentation on page 170. 

## Log Forwarding 

Shows the log forwarding status for each cluster. Log forwarding sends CVM system, audit, AIDE, and SCMA logs to a central log host to preserve log integrity and meet compliance requirements. Configure log forwarding from the network configuration settings page in Prism Element. For more information, see Log Forwarding on page 155. 

You can also configure security hardening setting using the nCLI interface. For more information, see Hardening Instructions Using nCLI on page 159. 

## **Configuring Cluster Lockdown using Security Dashboard** 

You can configure cluster lockdown directly using the Security Dashboard. 

## **About this task** 

To configure cluster lockdown using the **Security Dashboard** , follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application. 

**3.** From the navigation bar, go to **Network & Security** > **Security Dashboard** . 

**4.** In the **Security Hardening** widget, click **Cluster Lockdown** . 

**5.** From the **Your Clusters** list, click **Lock Down** for the required cluster. 

**6.** Disable remote log on using the **Enable Remote Login with Password** checkbox. 

**7.** Delete the SSH key. 

To delete the SSH key, click the cross button from the **Your Keys** list. 

Your preferences are automatically saved upon configuration. 

## **Vulnerabilities** 

The **Vulnerabilities** widget displays a list of vulnerabilities (or CVEs) associated with your clusters based on the AOS versions. 

The **Vulnerabilities** widget displays vulnerability data for the Prism Central instance and clusters running AOS and AHV only. 

AOS Security | Security Management Using Prism Central | **87** 

Click **View All Vulnerabilities** to view the list of all vulnerabilities and the recommended upgrade path for mitigating the identified vulnerabilities. 

**Note:** `Not all the listed CVEs might be resolved when upgraded to a new release.` 

Starting with Prism Central 2024.1 release, you can update the **Vulnerabilities** information available on the **Security Dashboard** . Upgrade the LCM framework to LCM 3.0 release, and then upgrade the **Security Dashboard CVE Data** using either the connected site or dark site method (web server-based method only). 

For more information, see Life Cycle Manager Guide . 

## **System Account Passwords** 

View system accounts and their password state. 

The **System Account Passwords** widget displays a list of local accounts and the associated password state (default or secure). 

You can view the local accounts passwords **By Component** or **By Cluster** . 

Click **View all System Account Passwords** to change the account passwords using the Centralized Password Management Using System Accounts on page 62 page. 

## **Manually Refresh the Security Dashboard** 

Manually refresh the Security Dashboard only when you are aware that updates or changes are made to the system or data since the last automatic refresh. 

## **About this task** 

To manually refresh the Security Dashboard, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Infrastructure** application and from the navigation bar, select **Network & Security** > **Security Dashboard** . 

**3.** Click **Manage Dashboard** . 

AOS Security | Security Management Using Prism Central | **88** 

## **4.** Click **Refresh Dashboard** . 

**Note:** The system might take some time to refresh the dashboard. 

Ensure that your Security Dashboard version is updated in Life Cycle Manager (LCM) to ensure security details are correct for all your clusters. 

**Figure 9: Manage Security Dashboard Window** 

## **Manually Upgrade the Security Dashboard Using LCM** 

The Life Cycle Manager (LCM) allows you to manually upgrade the security dashboard using PC core services module 

## **Before you begin** 

- Ensure that port 443 is open in your firewall and allow access to `https:// objects.githubusercontent.com/*` . For more information, see Prism Central Port and Protocols . 

- Manual upgrade of security dashboard is supported in the following software versions: 

   - pc.2022.9 and later 

   - Life Cycle Manager version 2.6 and later 

## **About this task** 

To upgrade the security dashboard manually using LCM, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **LCM** . 

**3.** Perform the inventory. 

For more information, see LCM Inventory . 

AOS Security | Security Management Using Prism Central | **89** 

## **4.** From the list of available upgrades, select **PC Core Services** . 

**Note:** Not every Prism Central core services module upgrade includes security dashboard updates. Some upgrades might only include internal service upgrades. 

Starting from pc.2024.1, you can update the Vulnerabilities on page 87 widget information available on the Security Dashboard by upgrading the LCM framework to LCM 3.0 release and the Security Dashboard CVE data using either the connected site or web server-based dark site. For more information, see Life Cycle Manager Guide . 

## **5.** Click **Upgrade** . 

## **External Key Management Server on Prism Central** 

Secure management of encryption keys is performed through an external Key Management Server (KMS) provider using Prism Central. 

For Software data-at-rest encryption , Nutanix provides the option to choose the KMS type as the Native KMS (local), Native KMS (remote), External KMS, and Cloud KMS (PC-only). Prism Central provides centralized management of key management interoperability protocol (KMIP)-based external key management servers . You can configure and manage the external KMS instances directly from Prism Central and use them for software data-at-rest encryption across multiple clusters. 

**Note:** KMIP-based key managers configured on Prism Central support only software data-at-rest encryption. Support for SED-based data-at-rest encryption is not available with these key managers. 

When you create an external KMS in Prism Central, the system establishes trust with the KMS by exchanging certificates. Prism Central stores the KMS configuration securely and shares the required certificate information with associated clusters. Each cluster uses this configuration to communicate with the external KMS to perform key management operations. 

External KMS configurations created in Prism Central can be applied to any cluster that supports data-at-rest encryption. The configuration is maintained centrally in Prism Central and does not require individual setup on each cluster. 

After you create an external KMS instance, you can associate it with a cluster. Once configured, the cluster communicates directly with the external KMS to retrieve and manage encryption keys. For more information, see Configuring External Key Management Server on page 90. 

## **Configuring External Key Management Server** 

Configure an external key management server (KMS) on Prism Central for software data-at-rest encryption. 

## **About this task** 

To configure external KMS for software data-at-rest-encryption, follow these steps: 

## **Before you begin** 

Ensure that your environment meets the following requirements: 

- Prism Central version pc.7.5 or later 

- AOS version 7.5 or later 

- Network connectivity and valid certificates between Prism Central and the external KMS 

AOS Security | Security Management Using Prism Central | **90** 

## **Procedure** 

**1.** Log on to the Prism Central VM using SSH. 

**2.** Create an external KMS instance: 

`nutanix@PCVM$ mantle_cli kms kmip create --name <KMS Name> --ca_name <CA_Name> -- ca_cert_path <Path to CA certificate> --client_cert_path <Path to client certificate> --private_key_path <Path to private key file> --ip_port_vec <Comma-separated list of KMS IP address:port pairs>` 

Replace <KMS Name>, <CA_Name>, <Path to CA certificate>, <Path to client certificate>, <Path to private key file>, <Comma-separated list of KMS IP address:port pairs> with the values for your environment. 

**Note:** You can create only up to two external KMS instances on a Prism Central instance. 

**3.** Verify that the external KMS is created successfully: 

   - `nutanix@PCVM$ mantle_cli kms kmip list` ~~SCS~~ 

The external KMS configuration appears in the output, indicating successful KMS creation. 

**4.** (Optional) Verify that the changes to KMS configuration propagate fully through the Nutanix environment: 

   - `nutanix@PCVM$ mantle_ops kms_info` ~~SSS~~ 

The KMS configuration can take up to 10 minutes to propagate. 

**5.** Note the **UUID** of the external KMS on Prism Central. 

**6.** (Optional) Enable software data-at-rest encryption on the target cluster using Prism Central if it is not already enabled. 

   - a. Log on to the Prism Central web console as an administrator. 

   - b. From the Application Switcher option, select the **Infrastructure** application. 

   - c. Go to **Hardware** > **Clusters** and from the **List** view, select the cluster to encrypt. 

   - d. Click **Actions** and select **Enable Data-at-Rest Encryption** . 

   - e. Select the Encryption Type as **Encrypt the entire cluster** or **Encrypt storage containers** . 

   - f. Click **Save Encryption Type** . 

   - g. Select the **Key Management Server** as **Native (remote)** and click **Enable Encryption** . 

**7.** Associate the external KMS with a cluster: 

   - a. Log on to the Prism Element cluster using SSH. 

   - b. Enable the external KMS configuration for the cluster: 

`nutanix@cvm$ ncli key-management-server change-key-manager-server-type keymanager-server-type=ekm_pc kms-uuids=<kms_uuid>` 

Replace the <kms_uuid> value with the UUID of the external KMS obtained in Step 5 on page 91. 

> The a `kms-uuids` parameter is optional. 

- If you specify one or more UUIDs, only those UUIDs are associated with the cluster. 

- If you do not specify any UUIDs, all external KMS UUIDs configured in Prism Central are associated with the cluster. 

The external KMS configuration is configured in the Prism Central instance and associated with the selected cluster. The cluster now uses the external KMS for software data-at-rest encryption. 

AOS Security | Security Management Using Prism Central | **91** 

## **Updating External Key Management Server Configuration** 

Update an existing external key management server (KMS) configuration in Prism Central. 

## **About this task** 

To update an external KMS configuration, follow these steps: 

## **Before you begin** 

Ensure that the Prism Central instance is running version pc.7.5 or later. 

## **Procedure** 

**1.** Log on to the Prism Central VM using SSH. 

**2.** List all configured KMS entries: 

> `mantle_cli kms kmip list` ~~SCS~~ Identify the KMS UUID that you need to update. 

**3.** Update the external KMS details: 

`mantle_cli kms kmip update --kms_uuid <kms_uuid> ca_cert_path=<Path to CA certificate> client_cert_path=<Path to client certificate> --private_key_path <Path to private key file> --ip_port_vec <Comma-separated list of KMS IP address:port pairs>` 

Replace <kms_uuid>, <Path to CA certificate>, <Path to client certificate>, and <Comma-separated list of KMS IP address:port pairs> with the updated values based on your environment. 

**4.** Verify that the changes are applied: 

> `mantle_cli kms kmip get --kms_uuid <kms_uuid>` ~~SCS~~ The external KMS configuration is updated successfully. Prism Central automatically propagates the new configuration to the registered clusters. 

## **Deleting External Key Management Server Configuration** 

Remove an external key management server (KMS) configuration from Prism Central. 

## **About this task** 

To delete an external KMS configuration for software data-at-rest encryption, follow these steps: 

## **Before you begin** 

Ensure that your environment meets the following requirements: 

- Prism Central is running version 7.5 or later. 

- The KMS entry is no longer in use by any cluster. 

## **Procedure** 

**1.** Log on to the Prism Central VM using SSH. 

**2.** List all registered external KMS configurations: 

> `mantle_cli kms kmip list` ~~SSS~~ Identify the KMS UUID that you need to delete. 

AOS Security | Security Management Using Prism Central | **92** 

**3.** Delete the external KMS: 

`mantle_cli kms kmip delete --kms_uuid <kms_uuid>` 

**4.** Verify that the KMS is no longer listed: 

`mantle_cli kms kmip list` 

The external KMS configuration is deleted from the Prism Central instance. 

## **Cloud Key Management Server on Prism Central** 

Secure management of encryption keys through the KMS service provided by a cloud platform. 

For Software data-at-rest encryption , Nutanix provides the option to choose the KMS type as the Native KMS (local), Native KMS (remote), External KMS, and Cloud KMS (PC-only). Use the Cloud KMS option to securely manage encryption keys using your cloud provider’s KMS using Prism Central. Enabling Cloud KMS provides the ability to manage all KMS-related operations, including changing or updating the KMS configuration. Cloud KMS offers a simplified and secure method to manage data-at-rest encryption for on-premises Nutanix clusters, NC2 on Azure and NC2 on AWS, provided the NC2 cluster can communicate with the Azure Key Vault endpoint. 

**Note:** Cloud KMS support is available for Microsoft Azure Key Vault only. 

## **Cloud KMS Requirements and Considerations** 

The following requirements and considerations apply to setting up and using Cloud KMS: 

- Prism Central integrates with Microsoft Azure Key Vault. To set up Cloud KMS, you must provide specific Azure configurations, including the Key Vault URI, client ID, tenant ID, and a client secret, which authorizes Prism Central to use the RSA keys stored in Azure. For more information, see Setting Up an Azure Key Vault for Cloud Key Management Server on page 95. 

- Prism Central supports adding only one Cloud KMS instance. 

- The Azure Key Vault URI must be accessible from the NC2 cluster. This often involves placing the Key Vault in the same Azure subscription or another subscription under the same Azure Active Directory (Entra ID) tenant, but this placement is not required as long as the NC2 cluster can reach the Key Vault URI. 

- Cloud KMS is supported for RSA keys only. 

- Cloud KMS supports RSA key sizes of 2048, 3072, and 4096 bits. 

- Cloud KMS supports both Standard and Premium SKUs of Azure Key Vault. For details on FIPS compliance levels and differences between Azure Key Vault SKUs, refer to Microsoft’s official documentation. 

- Cloud KMS is supported on cluster running AOS version 7.0 or later. 

- Cloud KMS is supported on Prism Central version pc.2024.3 or later. 

- After enabling Cloud KMS, all KMS management, any future changes to the KMS type (such as switching back to local KMS) must be done using Prism Central. You cannot manage KMS configurations directly through Prism Element. 

## **Configuring Cloud Key Management Server** 

Provides the Azure Cloud Key Management Server configuration used for software data-at-rest encryption. 

## **Before you begin** 

Before configuring Cloud key management service (KMS) in Prism Central, ensure that you meet the following requirements: 

AOS Security | Security Management Using Prism Central | **93** 

- Cloud KMS requirements: For more information, see Cloud KMS Requirements and Considerations on page 93. 

- Set up an Azure Key Vault: For more information, see Setting Up an Azure Key Vault for Cloud Key Management Server on page 95. 

## **About this task** 

To configure Cloud KMS for software data-at-rest-encryption, follow these steps: 

## **Procedure** 

**1.** Log on to the Prism Central web console as an administrator. 

**2.** From the Application Switcher option, select the **Infrastructure** application. 

**3.** From the navigation bar, go to **Prism Central Settings** > **Security** > **External KMS** . 

**4.** In the **External Key Management Servers (KMS)** page, click **+Add External KMS** . 

**5.** In the **Add External Key Management Server (KMS)** page, enter the following **Azure Key Vault Details** : 

   - **KMS Name** 

   - **Vault URI** 

   - **Key Name ID** 

   - **Directory (Tenant) ID** 

   - **Application (Client) ID** 

   - **Client Secret** 

   - **Client Secret Key Expiry Date** 

For more information on Azure Key Vault configuration, see Setting Up an Azure Key Vault for Cloud Key Management Server on page 95. 

**Note:** Ensure that changes to KMS configuration propagate fully through the Nutanix environment before enabling data-at-rest encryption to avoid failures. 

**6.** (Optional) Verify that the changes to KMS configuration propagate fully through the Nutanix environment: 

`nutanix@PCVM$ mantle_ops kms_info` 

The KMS configuration can take up to 10 minutes to propagate. 

**7.** Go to **Hardware** > **Clusters** and select the cluster to encrypt from the **List** view. 

**8.** Click **Actions** and select **Enable Data-at-Rest Encryption** . 

**9.** Select the **Encryption Type** as **Cluster Encryption** or **Entity Encryption** . 

**10.** Click **Save Encryption Type** . 

**11.** Select the **Key Management Server** as **Cloud** and click **Enable Encryption** . 

AOS Security | Security Management Using Prism Central | **94** 

## **12.** Type **ENCRYPT** for Cluster Encryption or **SET** for **Entity Encryption** . 

**Caution:** To help ensure that your data is secure, you cannot disable software-only data-at-rest encryption after it is enabled. Nutanix recommends regularly backing up your data, encryption keys, and key management server. 

For more information, see Configuring Data-at-Rest Encryption (Software Only) . 

## **Setting Up an Azure Key Vault for Cloud Key Management Server** 

Azure Key Vault details are required for Cloud key management server (KMS) configuration in the Prism Central VM. 

## **About this task** 

To set up an Azure Key Vault in your Azure environment, follow these steps: 

## **Procedure** 

**1.** Create an Azure Key Vault. 

**2.** Generate or import RSA keys. 

**3.** Configure an App Registration in Azure. 

**4.** Set up the key vault access policy to allow the App Registration to perform key management, cryptographic, and rotation policy operations. 

**5.** Retrieve the following details from your Azure configuration to use in the Cloud KMS configuration . 

   - Client Secret Expiry Date: Expiration date of the client secret. 

   - Tenant ID: A unique identifier for your organization in Azure Active Directory. 

   - Client ID: A unique identifier for the App Registration required by Prism Central to connect to the Azure Key Vault. 

   - Key ID: Name of the encryption key in the Azure Key Vault instance used for Cloud KMS. The key must be of type RSA. 

   - Key Vault URI: URI of the Azure Key Vault instance that contains the encryption key. 

For more information on configuring the Azure Key Vault, see _Microsoft Azure documentation_ . 

## **Updating Cloud KMS Configuration** 

Update the existing Azure Cloud KMS configuration. 

## **About this task** 

To update the Azure Cloud KMS configuration, follow these steps: 

## **Procedure** 

**1.** Log on to the Prism Central web console as an administrator. 

**2.** From the Application Switcher option, select the **Infrastructure** application. 

**3.** From the navigation bar, go to **Prism Central Settings** > **Security** > **External KMS** . 

**4.** In the **External Key Management Servers (KMS** ) table, locate the KMS entry, then, in the **Actions** column, click the edit icon. 

AOS Security | Security Management Using Prism Central | **95** 

**5.** In the **Update External Key Management Server (KMS)** , update the KMS configuration details. 

**6.** Click **Update External KMS** . 

## **Deleting Cloud KMS Configuration** 

Remove an existing Azure Cloud KMS configuration. 

## **Before you begin** 

If the Cloud KMS is currently being used for software data-at-rest encryption, you must change the KMS type to another supported type before deleting the Cloud KMS configuration. You cannot delete the Cloud KMS that is actively used for software data-at-rest encryption. 

## **About this task** 

To delete the Cloud KMS configuration, follow these steps: 

## **Procedure** 

**1.** Log on to the Prism Central web console as an administrator. 

**2.** From the Application Switcher option, select the **Infrastructure** application. 

**3.** From the navigation bar, go to **Prism Central Settings** > **Security** > **External KMS** . 

**4.** In the **External Key Management Servers (KMS** ) table, locate the KMS entry, then, in the **Actions** column, click the remove icon. 

## **Changing the KMS Type Using Prism Central** 

Change the KMS type from Cloud KMS to another supported KMS type. 

## **About this task** 

Using Prism Central, you can change the KMS type from Cloud KMS to another supported KMS type, such as Native KMS (Local). Before proceeding, see Key Management Server (KMS) Considerations on page 147. To change the KMS type, follow these steps: 

## **Procedure** 

**1.** Log on to Prism Central as an administrator. 

**2.** In the Application Switcher, select **Infrastructure** . 

**3.** Go to **Hardware** > **Clusters** , and from the **List** view, select the encrypted cluster. 

**4.** Click **Actions** and select **Manage KMS Type** . 

**5.** Select a new KMS type. 

**6.** Click **Change KMS Type** . 

## **Portal Proxy Connection in Prism Central** 

The portal proxy connection enables secure communication between Prism Central and Nutanix Support Portal using API keys. 

The portal proxy connection provides the following capabilities: 

- Direct support case creation from the Prism Central web console 

AOS Security | Security Management Using Prism Central | **96** 

- Automatic updates to the cluster’s license status, which eliminates the manual downloading and uploading of the licensing files 

## **Portal Proxy Connection Considerations** 

Before you configure the portal proxy connection, consider the following points: 

- You can configure only one API key in the portal proxy connection. 

- When you disable the portal proxy connection, the associated API key becomes invalid. You cannot reuse the same API key for a new portal proxy connection. 

## **Configuring the Portal Proxy Connection in Prism Central** 

Configure the portal proxy connection in Prism Central. 

## **Before you begin** 

Ensure that you generated an API Key and downloaded the optional SSL key from the My Nutanix portal. For more information, see Creating an API Key. 

## **About this task** 

To configure the portal proxy connection in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **Settings** . 

**3.** Under Setup, select the **Portal Proxy** . 

**4.** On the **Portal Connection** page, enter the following information: 

   - a. **API Key** : Enter the API key that you generated from the My Nutanix portal. 

   - b. (Optional) **Public Key** : To upload the optional SSL key that you downloaded from the My Nutanix portal, click **Choose File** . 

**5.** Click **Save** . 

## **Updating the Portal Proxy Connection in Prism Central** 

Update the portal proxy connection in Prism Central. 

## **About this task** 

To update the portal proxy connection in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **Settings** . 

**3.** Under Setup, select the **Portal Proxy** . 

**4.** Click **Update** . 

AOS Security | Security Management Using Prism Central | **97** 

**5.** Update the fields in the **Portal Connection** window as needed and click **Save** . 

For more information on the fields available in the **Portal Connection** window, see Configuring the Portal Proxy Connection in Prism Central on page 97. 

## **Disabling the Portal Proxy Connection in Prism Central** 

Disable the portal proxy connection in Prism Central. 

## **Before you begin** 

You cannot reuse the disabled API key for a new portal proxy connection. 

## **About this task** 

To disable the portal proxy connection in Prism Central, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** From the Application Switcher Function , select the **Admin Center** application and from the navigation bar, select **Settings** . 

**3.** Under Setup, select the **Portal Proxy** . 

**4.** Click **Disable** . 

**5.** Click **Yes** . 

AOS Security | Security Management Using Prism Central | **98** 

## **SECURITY MANAGEMENT USING PRISM ELEMENT** 

Nutanix provides several mechanisms to maintain security in a cluster using Prism Element. 

Prism Element provides cluster security by supporting multiple authentication methods, including Active Directory, OpenLDAP, and Common Access Card (CAC). You can configure role mappings, manage local or emergency accounts, and enforce session timeouts. Prism Element also supports SSL certificate management for secure communication, cluster lockdown with SSH key–based access, and data-at-rest encryption using either selfencrypting drives or software encryption with key management. These controls protect access, data, and cluster operations. 

## **Authentication in Prism Element** 

Authentication in Prism Element supports Active Directory, OpenLDAP, and Common Access Card (CAC). 

Prism Element supports multiple authentication methods to secure access to the cluster. You can configure Active Directory for centralized credential management, or use OpenLDAP for directory-based authentication. For environments requiring stronger assurance, Prism Element supports Common Access Card (CAC) authentication with client chain certificates, enabling certificate-based login instead of passwords. 

## **Configuring Active Directory Authentication in Prism Element** 

Configure Active Directory (AD) as the identity provider in Prism Element to enable AD credential-based login for users. 

## **Before you begin** 

Before you configure AD in Prism Element, consider the following points: 

- The service account for AD must have full read permissions on the directory service. 

- Users with the User must change password at next logon attribute enabled cannot authenticate to Prism Element. Ensure that users change their password on a domain workstation before logging into Prism Element. 

- If SSL is enabled on the AD server, ensure that Prism Element's firewall allows access to the appropriate port. 

- Use of the Protected Users group is not supported for Prism Element authentication. For more information, see _Guidance about how to configure protected accounts_ on Microsoft documentation website. 

- Prism Element supports AD with LDAP v2 on Windows Server 2012 R2, Windows Server 2016, and Windows Server 2019. 

**Important:** Prism Element does not support insecure SSLv2 and SSLv3 ciphers. To prevent the SSL fallback situation and being denied access to Prism Element, configure the following settings in your browser: 

- Disable SSLv2 and SSLv3 

- Enable TLS 

## **About this task** 

To configure AD in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

AOS Security | Security Management Using Prism Element | **99** 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Authentication** . 

**4.** Select the **Authentication Types** tab and select **Directory Service** checkbox. 

**5.** Select the **Directory List** tab. 

AOS Security | Security Management Using Prism Element | **100** 

**6.** Click **+ New Directory** and enter the following information: 

   - **Directory Type** : From the dropdown list, select **Active Directory** . 

   - **Name** : Enter a name to identify the directory service on Prism Element. 

   - **Domain** : Enter the domain or sub-domain of the directory service. 

For example nutanix.com 

- **Directory URL** : Enter the URL address of the directory in ldap://host:ldap_port_num format. 

host: The host value is either the IP address or fully qualified domain name. 

ldap_port_num: The default LDAP port number is 389. Nutanix also supports ports 636 for LDAPS, 3268 and 3269 for LDAP/S global catalog servers. 

Examples: 

- Default LDAP (Port 389), when the configuration is single domain, single forest, and not using SSL: 

`ldap://ldap.example.com or ldap://10.1.4.111:389` 

- LDAPS (Port 636), when the configuration is single domain, single forest, and using SSL: 

`ldaps://ldap.example.com or ldaps://10.1.4.111:636` 

LDAPS requires that all AD Domain Controllers have properly installed SSL certificates. 

- LDAP/S Global Catalog non-SSL (Port 3268), when the configuration is multiple domain, single forest, and not using SSL: 

`ldap://globalcatalog.example.com:3268 or ldap://10.1.4.111:3268` 

- LDAP/S Global Catalog SSL/TLS encrypted (Port 3269), when the configuration is multiple domain, single forest, and using SSL: 

`ldaps://globalcatalog.example.com:3269 or ldaps://10.1.4.111:3269` 

When the directory URL is set to the domain FQDN, LDAP authentication can fail if any directory server becomes unavailable. For high availability (HA), configure an LDAP HA cluster and specify the cluster virtual IP (VIP) address in the directory URL instead of the domain FQDN. For more information, see KB-12568 . 

When constructing your LDAP/S URL to use a global catalog server, ensure that the domain control IP address or name belongs to a global catalog server within the domain that you are configuring. Otherwise, queries over port 3268 or 3269 might fail. 

When querying the global catalog, the users `sAMAccountName` field must be unique across the Active Directory (AD) forest. If `sAMAccountName` field is not unique across subdomains, authentication might fail intermittently or consistently. 

Cross-forest trust between multiple AD forests is not supported. 

LDAPS support does not require custom certificates or certificate trust import. 

For the complete list of required ports, see Port Reference . 

- **Search Type** : From the dropdown list, select either **Non Recursive (Default)** or **Recursive** . 

When users are present in the first level group, select **Non Recursive (Default)** . 

When you have multi-level group assignments, select **Recursive** . A recursive search performs a multi-level or nested search during authentication, which might cause slowness. If you experience slowness, select **NonRecursive (Default)** . 

- **Service Account Username** : Enter the service account user name in _`user_name@domain.com`_ format. 

AOS Security | Security Management Using Prism Element | **101** 

**Note:** A service account is created to run only a particular service or application with the credentials specified for the account. According to the requirement of the service or application, the administrator can limit access to the service account. The service account is under the Managed Service Accounts in the AD server. An application or service uses the service account to interact with the operating system. 

- **Service Account Password** : Enter the service account password. 

**Note:** Be sure to update the service account credentials if the password changes or you use a different service account. 

**7.** Click **Save** . 

## **Configuring OpenLDAP Authentication in Prism Element** 

Configure OpenLDAP as the identity provider in Prism Element to enable OpenLDAP credential-based login for users. 

## **Before you begin** 

Before you configure OpenLDAP in Prism Element, consider the following points: 

- The service account for OpenLDAP must have full read permission on the directory service. 

- Prism Element uses a service account to query OpenLDAP directories for user information and does not currently support certificate-based authentication with the OpenLDAP directory. 

- The values for the **User Object Class** , **User Search Base** , **Username Attribute** , **Group Object Class** , **Group Search Base** , **Group Member Attribute** , and **Group Member Attribute** fields depend on your OpenLDAP configuration. 

- Prism Element supports OpenLDAP with LDAP v2. 

**Important:** Prism Element does not support insecure SSLv2 and SSLv3 ciphers. To prevent the SSL fallback situation and being denied access to Prism Element, configure the following settings in your browser: 

- Disable SSLv2 and SSLv3 

- Enable TLS 

## **About this task** 

To configure OpenLDAP in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Authentication** . 

**4.** Select the **Authentication Types** tab and select **Directory Service** checkbox. 

**5.** Select the **Directory List** tab. 

AOS Security | Security Management Using Prism Element | **102** 

**6.** Click **+ New Directory** and enter the following information: 

   - **Directory Type** : From the dropdown list, select **OpenLDAP** . 

   - **Name** : Enter a name to identify the directory service on Prism Element. 

   - **Domain** : Enter the domain or sub-domain of the directory service. 

For example nutanix.com 

- **Directory URL** : Enter the URL address of the directory in ldap://host:ldap_port_num format. 

host: The host value is either the IP address or fully qualified domain name. 

ldap_port_num: The default LDAP port number is 389. Nutanix also supports ports 636 for LDAPS. Examples: 

- Default LDAP Port (389), when the configuration is single domain, single forest, and not using SSL: 

`ldap://ldap.example.com or ldap://10.1.4.111:389` 

- LDAPS (Port 636), when the configuration is single domain, single forest, and using SSL: 

`ldaps://ldap.example.com or ldaps://10.1.4.111:636` 

When the directory URL is set to the domain FQDN, LDAP authentication can fail if any directory server becomes unavailable. For high availability (HA), configure an LDAP HA cluster and specify the cluster virtual IP (VIP) address in the directory URL instead of the domain FQDN. For more information, see KB-12568 . 

LDAPS support does not require custom certificates or certificate trust import. 

For the complete list of required ports, see Port Reference . 

- **Search Type** : From the dropdown list, select either **Non Recursive(Default)** or **Recursive** . 

When users are present in the first level group, you can select **Non Recursive(Default)** . 

When you have multi-level group assignments, you can select **Recursive** . A recursive search performs a multi-level or nested search during authentication, which might cause slowness. If you experience slowness, select **Non-Recursive (Default)** . 

- **User Object Class** : Enter the object class of users in the directory service. 

For example: user, person, inetOrgPerson, organizationalPerson, or posixAccount 

- **User Search Base** : Enter the base Distinguished Name (DN) to search the users. 

   - For example: 

ou=users, dc=nutanix, dc=com 

cn=users, dc=nutanix, dc=com 

- **Username Attribute** : Enter the attribute of the user object class which uniquely identifies a user in the directory. 

For example: uid. 

- **Group Object Class** : Enter the object class of groups in the directory service. 

For example: posixGroup or groupOfNames 

- **Group Search Base** : Enter the base Distinguished Name (DN) to search the user groups. 

For example: 

cn=groups, dc=nutanix, dc=com 

AOS Security | Security Management Using Prism Element | **103** 

ou=groups, dc=nutanix, dc=com 

- **Group Member Attribute** : Enter the attribute of the group object that holds the user association. 

   - For example: member or memberUid 

- **Group Member Attribute Value** : Enter the attribute of the user object that is used to configure the membership in the group object. 

For example: uid 

- **Service Account Username** : Enter the service account user name in cn=username, dc=example, dc=com format. 

For example cn=username, dc=nutanix, dc=com 

**Note:** A service account is created to run only a particular service or application with the credentials specified for the account. According to the requirement of the service or application, the administrator can limit access to the service account. The service account is under the Managed Service Accounts in the OpenLDAP server. An application or service uses the service account to interact with the operating system. 

- **Service Account Password** : Enter the service account password. 

**Note:** Be sure to update the service account credentials if the service account password changes or a different service account is used. 

**7.** Click **Save** . 

## **Common Access Card Authentication with Client Chain Certificate in Prism Element** 

Prism Element uses client chain certificate authentication to enable the Common Access Card (CAC) login. Client chain certificate authentication is a certificate-based authentication mechanism where the user is prompted to select a certificate for login instead of a password. 

Client chain certificate authentication provides enhanced security by requiring users to present a digital certificate instead of a password, reducing the risk of password-based attacks. In a one-way authentication process, Prism Element presents a certificate, and the user's browser verifies it. When the client chain certificate authentication is enabled, the process becomes a two-way authentication. During authentication, Prism Element also verifies the user's identity with a valid digital certificate to ensure that only authorized users can access the cluster. A user must provide a valid certificate when accessing Prism Element. 

Client chain certificate authentication has the following workflow: 

- Certificate Installation: A user installs the certificate on their local machine. 

**Note:** The Certificate Authority (CA) must be the same for both the client chain certificate and the certificate on the local machine. 

- Certificate Presentation and Chain Verification: When the user accesses Prism Element, the system prompts them to select their CAC certificate. Prism Element verifies the presented certificate against its trusted CA chain. The chain establishes trust by linking the client certificate to a trusted root CA. This chain verification step confirms the certificate's validity. 

- EDIPI Extraction and Account Verification: Prism Element extracts the Electronically Data Interchange Personal Identifier (EDIPI) from the validated CAC certificate. It then queries Active Directory (AD) to verify that the user has a valid and active account associated with this EDIPI. 

- Authentication Method Selection: Prism Element supports both certificate-based and basic authentication. When a certificate is present, the system prioritizes certificate-based authentication for the login. If a certificate is not present, the system uses basic authentication. 

- Login: Prism Element logs in the user based on their validated certificate and verified Active Directory account. 

AOS Security | Security Management Using Prism Element | **104** 

Before you configure the CAC, consider the following points: 

- Prism Element supports AD as directory service for CAC. OpenLDAP is not supported. 

- When you enable CAC authentication, Prism Element disables all other directory services and local user login. Only the local admin user login is permitted in this case. 

- When you authenticate Prism Element with client chain certificate, the `Subject name` field must be present. The subject name should match the UserPrincipalName (UPN) in the AD. The UPN is a username with domain address. For example user1@nutanix.com. 

- If you map a Prism role to a CAC user and not to an AD group or organizational unit to which the user belongs, specify the EDIPI UPN or UPN of that user in the role mapping. A user who presents a CAC with a valid certificate is mapped to a role and taken directly to the web console home page. The web console login page is not displayed. 

- By default, users with an admin role can log in to the Prism Element web console using CAC authentication. For other LDAP user roles, ensure the correct role mappings or role assignments are configured to enable CAC authentication. 

- Client chain certificate must be PEM encoded. 

- If you have logged in to Prism Element by using CAC authentication, to successfully log out of Prism Element, close the browser after you click Log Out. 

## **Configuring Common Access Card Authentication in Prism Element** 

Configure Common Access Card (CAC) authentication in Prism Element. 

## **Before you begin** 

Ensure that port 9441 is open in your firewall. After enabling the CAC authentication, the CAC login redirects the browser to use port 9441. 

## **About this task** 

To configure the CAC authentication in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Authentication** . 

**4.** Select the **Client** tab. 

**5.** Select **Configure Client Chain Certificate** checkbox. 

**6.** Click **Choose File** and select a client chain certificate from your local machine. 

**7.** Click **Yes** . 

Prism Element web console restarts and you are logged out automatically. 

**8.** Select **Enable Client Authentication** checkbox. 

**9.** Click **Yes** . 

Prism Element web console restarts and you are logged out automatically. 

AOS Security | Security Management Using Prism Element | **105** 

**10.** Select **Configure Service Account** checkbox and enter the following information: 

   - **Directory** : From the dropdown list, select the directory. 

   - **Service Username** : Enter a service name in _`user name@domain.com`_ format. 

   - **Service Password** : Enter a service password. 

**11.** Click **Save** . 

**12.** Select **Enable CAC Authentication** checkbox. 

**13.** Click **Yes** . 

Prism Element web console restarts and you are logged out automatically. 

## **Authentication Best Practices** 

Authentication best practices in Prism Element ensure secure access to Nutanix clusters. 

Prism Element supports several authentication best practices to improve cluster security. You can use external identity services such as Active Directory to authenticate users and reserve the local admin account for emergency access. You can also change the default Controller VM password to meet complexity requirements across the cluster. Additionally, you can configure session idle timeouts to automatically log out inactive accounts and minimize the risk of unauthorized access. 

## **Emergency Local Account Usage** 

Use the admin account as a local emergency account to ensure access when external services are unavailable. 

The admin account ensures that both the Prism Element web console and the Controller VM are available when the external services such as Active Directory is unavailable. 

For all the external authentication, you must configure the cluster to use an external IAM service such as Active Directory. You must create service accounts in the IAM and the accounts must have access grants to the cluster through Prism Element web console user account management configuration for authentication. 

**Note:** The local emergency account usage does not support any external access mechanisms, specifically for the external application authentication or external REST API authentication. 

## **Modifying the Default CVM Password** 

Change the default Controller VM (CVM) password for `nutanix` user account by adhering to the password complexity requirements. 

## **Before you begin** 

Before you modify the default CVM password, consider the following points: 

- Changing the password on one of the CVM is applied to all CVMs in the cluster. 

- Nutanix recommends using the `admin` user account as the emergency administrative account. 

- Console and SSH direct login are disabled for the `root` account. 

## **About this task** 

To modify the default CVM password, follow these steps: 

AOS Security | Security Management Using Prism Element | **106** 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: 

   - `$ ssh nutanix@` ~~SSS~~ _`cvm_ip_address`_ 

**2.** Change the `nutanix` a user account password: `nutanix@cvm$ sudo passwd nutanix` ~~ee~~ 

**3.** Enter your new password in the command output: 

`Changing password for nutanix. New password: Retype new password: passwd: all authentication tokens updated successfully.` 

Ensure that your password meets the following requirements: 

- At least eight characters long 

- At least one lowercase letter 

- At least one uppercase letter 

- At least one digit 

- At least one special character (allowed special characters are: "#$%&'()*+,-./:;<=>@[]^_`{|}~!\ ) 

- At least four characters different from the old password 

- Must not be among the last five passwords 

- Must not have more than two consecutive occurrences of a character 

- Must contain at least one character from each of the following four character classes: uppercase letters, lowercase letters, digits, and special characters 

- Must not contain the words "nutanix", "ntnx", "password", or any simple dictionary words. 

**Note:** Ensure that you preserve the updated `nutanix` user password. The local authentication (PAM) module requires the previous password of the `nutanix` user to successfully start the password reset process. 

## **Setting Admin Session Timeout** 

By default, Prism Element logs out users after 15 minutes of inactivity. You can change the session timeout and configure overrides for non-admin users. 

## **About this task** 

To configure admin session timeout, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **UI Settings** . 

**4.** From the **Default Session Idle Timeout For Non-Admin Users** dropdown list, select the default session idle timeout for all non-administrative users. 

AOS Security | Security Management Using Prism Element | **107** 

**5.** From the **Session Idle Override For Non-Admin Users** dropdown list, select the appropriate option to override the session timeout for non-administrative users. 

## **Role Mapping** 

You can create and manage the role mapping for directory service users in Prism Element web console. 

When you enable directory service for user authentication, by default no permissions are granted to the directory service users. You must grant permissions to the directory users by specifying the roles for the organizational units (OUs), groups and users within a directory. 

## **Role Mapping Considerations** 

Before you configure the role mapping, consider the following points: 

- If you are using Active Directory, you must also assign roles to entities or users, before upgrading from a previous AOS version. 

- You can create a role map for each authorized directory. 

- You can create multiple maps that apply to a single directory. When a directory has multiple maps, the most specific rule for a user is granted. 

For example, if you assign a cluster admin role to a group, everyone in that group is granted a cluster admin access. However, you can override that access for specific individuals within the group. If you also assign a viewer role to certain users in the group, those specific users only have viewer access, while everyone else in the group has the cluster admin access. 

- When role mapping is not defined for an authorized service directory, all users in that directory receive full administrator permissions. After you create a role map, users who are not explicitly assigned permissions through the mapping are denied access. 

## **Configuring a Role Mapping** 

Configure a role mapping for directory service users. 

## **About this task** 

To configure the role mapping, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Role Mapping** . 

AOS Security | Security Management Using Prism Element | **108** 

**4.** Click **+ New Mapping** and enter the following information: 

   - **Directory or Provider** : From the dropdown list, select the identity provider. 

   - **Type** : From the dropdown list, select the entity type. 

   - **Role** : Choose one of the following roles: 

      - **Viewer** : Users are granted view-only access. 

      - **User Admin** : Users can perform administrative tasks, view system information, and manage user accounts. 

      - **Cluster Admin** : Users can perform administrative tasks and view system information, but cannot manage user accounts. 

      - **Backup Admin** : Users can perform backup-related administrative tasks. 

**Note:** After updating to AOS 6.0 or later, users and user groups with Active Directory (AD) **Backup Admin** role assigned cannot log in to Prism Element using their AD credentials or access v3 APIs. For more information, see KB-14105 . 

- **Values** : Enter the entity names in a comma separated list without spaces. 

**Note:** The entity names are case-sensitive. 

The values represent the actual names of organizational units (OUs), groups, or users assigned to the specified role. If you specify an organizational unit, all users within the unit are included. For groups, all users of the group are included. For users, each named user is included. 

For example, the system grants the cluster admin role to all users in both the admin-gp and support-gp groups when you select Group as the **Type** , Cluster Admin as the **Role** , and enter admin-gp,support-gp in the **Values** field. 

Do not include the domain name in the **Values** field. For example, enter admin-gp not admingp@nutanix.com. However, users must include their domain when logging into the Prism Element web console. 

The AD User Principal Name (UPN) must follow the `user@domain_name` format. 

When configuring role mapping with Active Directory forest setup, administrators can map users with the same name from any domain within the forest. Nutanix recommends configuring role mapping with Active Directory setup that uses a specific domain. 

**5.** Click **Save** . 

## **Editing a Role Mapping** 

Edit a role mapping that you created for the directory service users. 

## **About this task** 

To edit the role mapping, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Role Mapping** . 

AOS Security | Security Management Using Prism Element | **109** 

**4.** Click the pencil icon associated with the role. 

**5.** Edit the fields in the **Update Role Mapping** window as needed and click **Save** . 

## **Deleting a Role Mapping** 

Delete a role mapping that you created for the directory service users. 

## **About this task** 

To delete a role mapping, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Role Mapping** . 

**4.** Click the x icon associated with the role. 

**5.** Click **OK** . 

## **Local User Account in Prism Element** 

Prism Element allows you to create and manage local user accounts. 

During the cluster deployment process, an `admin` user account is created by default. However, you can create additional local user accounts by granting specific permissions for accessing Prism Element. 

## **Local User Account Considerations** 

Before creating a local user account, consider the following points: 

- A local user account cannot SSH into CVM and PCVM. SSH access to CVM and PCVM is limited only to the built-in `nutanix` and `admin` user accounts. 

- By default, no permissions are granted to a local user account. After creating a local user account, you must specify the custom or built-in roles and permissions. 

- A local user account does not have password expiration policy. 

## **Creating a Local User Account in Prism Element** 

You can create a local user account in Prism Central. 

## **About this task** 

To create the local user account in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Local User Management** . 

AOS Security | Security Management Using Prism Element | **110** 

**4.** Click **+ New User** and enter the following information: 

   - **Username** : Enter a user name. 

   - **First Name** : Enter a first name. 

   - **Last Name** : Enter a last name. 

   - **Email** : Enter a valid user email address. 

**Note:** The system uses the email address for client authentication and logging when the local user performs user and cluster tasks in the web console. 

- **Password** : Enter a password. 

Ensure that your password meets the following requirements: 

   - At least eight characters long 

   - At least one lowercase letter 

   - At least one uppercase letter 

   - At least one digit 

   - At least one special character (allowed special characters are: "#$%&'()*+,-./:;<=>@[]^_`{|}~!\ ) 

   - At least four characters different from the old password 

   - Must not be among the last five passwords 

   - Must not have more than two consecutive occurrences of a character 

   - Must contain at least one character from each of the following four character classes: uppercase letters, lowercase letters, digits, and special characters 

   - Must not contain the words "nutanix", "ntnx", "password", or any simple dictionary words. 

- **Language** : From the dropdown list, select the language setting for the user. 

For example, if you select **zh-CN** , the user interface displays in Simplified Chinese when the user logs in Prism Element. 

- **Roles** : Choose one of the following role checkboxes: 

   - **User Admin** : Users can perform administrative tasks, view system information, and manage local user accounts. The user admin role has the highest level of privilege among the other local user roles. 

**Note:** When you select the **User Admin** checkbox, the system automatically selects the **Cluster Admin** and **Backup Admin** checkboxes. 

- **Cluster Admin** : Users can perform administrative tasks and view system information, but cannot manage local user accounts. 

**Note:** When you select the **Cluster Admin** checkbox, the system automatically selects the **Backup Admin** checkbox. 

- **Backup Admin** : Users can perform Nutanix Mine-related administrative tasks. 

The Backup Admin role supports Nutanix Mine integrations starting with AOS version 5.19 and offers minimal functionality in cluster management. For more information about the Backup Admin role and its limitations in Nutanix Mine, see Backup Admin Role Capabilities on page 112. 

AOS Security | Security Management Using Prism Element | **111** 

If you deselect all the role checkboxes, users are granted view-only access. 

## **5.** Click **Save** . 

## **Backup Admin Role Capabilities** 

Understand the limited permissions and read-only access of Backup Admin users in Nutanix Mine clusters. 

Nutanix introduced the Backup Admin role for Mine integrations starting with AOS 5.19 version. The Backup Admin role has minimal functionality in cluster management and restricted access to Nutanix Mine clusters. 

Users with the Backup Admin role have the following limited functionality: 

- View-only access: 

   - **Health, Analysis, and Tasks** : View in read-only mode. 

   - **Cluster, Hardware, and Networking** : View configuration details but cannot expand the cluster, remove hosts, or modify network settings. 

   - **Alerts and Events** : View all alerts and events but cannot acknowledge or resolve them. 

- Available Actions: 

   - **VMs** : Power VMs on and off. 

   - **Image Management** : Upload new images from the **Settings** page. 

- Unavailable features and actions: 

   - Creating VMs or configuring their networks. 

   - Accessing File Server and Data Protection options. 

   - Configuring Alert Policies or Email settings. 

   - Accessing the Cluster Registration widget. 

## **Editing a Local User Account in Prism Element** 

You can edit a local user account settings in Prism Element. 

## **About this task** 

To edit the local user account, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Local User Management** . 

**4.** Click the pencil icon next to the local user account. 

**5.** Edit the fields in the **Update User** window as needed and click **Save** . 

## **Deleting a Local User Account in Prism Element** 

You can delete a local user account in Prism Element. 

AOS Security | Security Management Using Prism Element | **112** 

## **About this task** 

To delete the local user account, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Local User Management** . 

**4.** Click the remove icon next to the local user account. 

**5.** Click **Delete** . 

## **Disabling Login Access to a Local User Account in Prism Element** 

By default, login access is enabled for a local user account. You can disable login access to the local user account in Prism Element. 

## **About this task** 

To disable login access to the local user account, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Local User Management** . 

**4.** Under the **Enabled** column, click the **Yes** toggle button. 

   - The toggle button displays **No** , indicating that the local user account login access is disabled. 

## **Resetting a Local User Account Password Using nCLI** 

Reset a local user account password using the Nutanix command-line interface (nCLI). 

## **About this task** 

To reset the local user account password using nCLI, follow these steps: 

## **Procedure** 

**1.** Do one of the following: 

   - » SSH into any of the Controller VMs (CVMs) as a `nutanix` user: 

      - `$ ssh nutanix@` _`cvm_ip_address`_ 

   - » SSH into Prism Central VM (PCVM) as a `nutanix` user: 

      - `$ ssh nutanix@` _`pcvm_ip_address`_ 

AOS Security | Security Management Using Prism Element | **113** 

**2.** Reset the local user account password: 

   - » `nutanix@cvm$ ncli user reset-password user-name=` _`'user_name'`_ `password=` _`'new_password'`_ 

   - » `nutanix@pcvm$ ncli user reset-password user-name=` _`'user_name'`_ `password=` _`'new_password'`_ 

   - Replace _`'user_name'`_ with the local user account name. 

   - Replace _`'new_password'`_ with the new password. 

Ensure that your password meets the following requirements: 

- At least eight characters long 

- At least one lowercase letter 

- At least one uppercase letter 

- At least one digit 

- At least one special character (allowed special characters are: "#$%&'()*+,-./:;<=>@[]^_`{|}~!\ ) 

- At least four characters different from the old password 

- Must not be among the last five passwords 

- Must not have more than two consecutive occurrences of a character 

- Must contain at least one character from each of the following four character classes: uppercase letters, lowercase letters, digits, and special characters 

- Must not contain the words "nutanix", "ntnx", "password", or any simple dictionary words. 

## **Configuring Your Profile** 

Configure your profile information in Prism Element. 

In the Prism Element web console, you can manage your profile settings by updating personal information and credentials. You can modify your first name, last name, email address, and preferred language. You can also change your password to strengthen security, keeping your account details accurate and ensuring reliable access within the environment. 

## **Updating Your Profile** 

You can update your profile information in Prism Element. 

## **About this task** 

To update your profile information in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

## **2.** Click **User Menu** > **Update Profile** 

AOS Security | Security Management Using Prism Element | **114** 

**3.** On the **Update Profile** window, enter the following information: 

   - **First Name** : Enter a first name. 

   - **Last Name** : Enter a last name. 

   - **Email Address** : Enter a valid user email address. 

   - **Language** : From the dropdown list, select the language setting. 

**4.** Click **Save** . 

## **Changing Your Password** 

You can change your profile's password in Prism Element. 

## **About this task** 

To change your profile's password in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** Click **User Menu** > **Change Password** 

**3.** On the **Change Password** window, enter the following information: 

   - **Current Password** : Enter the current password. 

   - **New Password** : Enter the new password. 

   - **Confirm Password** : Re-enter the new password. 

**4.** Click **Save** . 

## **Exporting an SSL Certificate for Third-Party Backup Applications** 

You can export an SSL certificate from a CVM to use it with third-party backup applications. 

## **About this task** 

To export the SSL certificate from a CVM, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: 

   - `$ ssh nutanix@` ~~SSS~~ _`cvm_ip_address`_ 

**2.** Run the following command to obtain the virtual IP address of the cluster: 

   - `nutanix@cvm$ ncli cluster info` ~~ee~~ Output: 

`Cluster Id           : 0001ab12-abcd-efgh-0123-012345678m89::123456 Cluster Uuid         : 0001ab12-abcd-efgh-0123-012345678m89 Cluster Name         : three Cluster Version      : 6.0 Cluster Full Version : el7.3-release-fraser-6.0-a0b1cd... External IP address  : 10.10.10.10 Node Count           : 3` 

AOS Security | Security Management Using Prism Element | **115** 

`Block Count          : 1` 

`. . . . .` 

The external IP address in the output is the virtual IP address of the cluster. 

**3.** Run the following command to enter into the Python prompt: 

> `nutanix@cvm$ python` ~~ee~~ 

**4.** Run the following command to import the SSL library: 

> `>>> import ssl` ~~SSS~~ 

**5.** Run the following command to print the SSL certificate: 

`>>> print(ssl.get_server_certificate(('` _`virtual_IP_address`_ `',9440), ssl_version=ssl.PROTOCOL_TLSv1_2))` 

Example: 

`>>> print(ssl.get_server_certificate(('10.10.10.10', 9440), ssl_version=ssl.PROTOCOL_TLSv1_2))` 

Example Output: 

`-----BEGIN CERTIFICATE----0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz01 23456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123 456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz012345 6789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz01234567 89ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789 ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789AB CDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCD EFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEF GHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGH IJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJ KLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKL MNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMN OPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOP QRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQR STUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRST UVWXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUV WXYZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWX YZabcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZab cdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcd efghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdef ghij -----END CERTIFICATE-----` 

## **SSL Certificate Management in Prism Element** 

Manage SSL certificates in Prism Element to ensure secure access. 

Prism Element web console supports SSL certificate-based authentication for console access. To enable secure communication with a cluster, Prism Element web console includes a default self-signed SSL certificate. You can replace the default self-signed SSL certificate with your own self-signed SSL certificate or a certificate authority (CA) signed SSL certificate. 

For production purposes, Nutanix recommends that you replace the default self-signed certificate with a CA signed SSL certificate. 

AOS Security | Security Management Using Prism Element | **116** 

**Note:** You can import only a cluster-wide SSL certificate in Prism Element web console. The SSL certificate cannot be customized for an individual controller VM (CVM). Nutanix recommends that you check for the validity of the certificate periodically and replace the certificate if it is invalid. 

## **Generating a Self-signed SSL Certificate with Subject Alternative Name** 

Generate a self-signed SSL certificate with Subject Alternative Name (SAN) using OpenSSL to secure Prism Element. 

## **Before you begin** 

- The certificate import process validates that the key and certificate pair uses the correct signature algorithm to comply with NIST SP800-131a and RFC 6460 (NSA Suite B) standards. Use the correct key types, sizes, curves, and signature algorithms. For more information, see Supported Key Configurations for SSL Certificates on page 127. 

- Nutanix recommends including a DNS name for all CVMs in the self-signed SSL certificate using the SAN extension to avoid SSL certificate errors when accessing a CVM through its DNS name instead of the shared cluster IP address. 

## **About this task** 

To generate a self-signed SSL certificate with SAN, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: 

   - `$ ssh nutanix@` ~~SCS~~ _`cvm_ip_address`_ 

**2.** Generate a private key: 

   - RSA private key with a bit length of 2048: 

      - `nutanix@cvm$ openssl genrsa -out` ~~CSCC~~ _`my_key_name.key`_ `2048` 

   - RSA private key with a bit length of 4096: 

      - `nutanix@cvm$ openssl genrsa -out` ~~ee~~ _`my_key_name.key`_ `4096` 

   - ECDSA private key using the prime256v1 curve: 

> `nutanix@cvm$ openssl ecparam -name prime256v1 -genkey -out` ~~CSS~~ _`my_key_name.pem`_ 

**Important:** While you are generating the private key, ensure that the private key is not password protected. 

AOS Security | Security Management Using Prism Element | **117** 

**3.** Generate the Certificate Signing Request (CSR): 

   - For RSA 2048 and RSA 4096 private keys: 

`nutanix@cvm$ openssl req -new -nodes -key my_key_name.key` _`-signature_algorithm`_ `- out my_csr_name.csr` 

> _`signature_algorithm: Specify sha256 or sha384 or sha512.`_ ~~ee~~ 

For example, to generate a CSR for RSA 2048 private key using SHA-256 signature algorithm: 

`nutanix@cvm$ openssl req -new -nodes -key my_key_name.key -sha256 -out my_csr_name.csr` 

- For ECDSA 256 private key: 

`nutanix@cvm$ openssl req -new -nodes -key my_key_name.pem -sha256 -out my_csr_name.csr` 

For example, to generate a CSR for ECDSA 256 private key using SHA-256 signature algorithm: 

`nutanix@cvm$ openssl req -new -nodes -key my_key_name.pem -sha256 -out my_csr_name.csr` 

**4.** Enter the information in the command output to incorporate into your certificate request: 

`You are about to be asked to enter information that will be incorporated into your certificate request. What you are about to enter is what is called a Distinguished Name or a DN. There are quite a few fields but you can leave some blank For some fields there will be a default value, If you enter '.', the field will be left blank. ----Country Name (2 letter code) []:# Enter the 2 letter country code State or Province Name (full name) []:# Enter the full name of the state or province Locality Name (eg, city) []:# Enter the name of the city or locality Organization Name (eg, company) []: # Enter the legal name of your organization or company Organizational Unit Name (eg, section) []: # Enter the department or business unit Common Name (eg, fully qualified host name) []:# Enter the fully qualified domain name (FQDN) of the server Email Address []:# Enter a valid email address Please enter the following 'extra' attributes to be sent with your certificate request A challenge password []: # (Optional) Enter your password` 

**5.** Create a configuration file in your home directory with your preferred text editor named _`san.cnf`_ a that contains the following text: 

`[req] distinguished_name = req_distinguished_name req_extensions = v3_req [req_distinguished_name] [v3_req] basicConstraints = CA:FALSE keyUsage = nonRepudiation, digitalSignature, keyEncipherment subjectAltName = @alt_names [alt_names] DNS.0 = example1.domain.com # Primary domain or fully qualified domain name (FQDN) of the server DNS.1 = example2.domain.com # Secondary domain or alias for the server DNS.2 = example3.domain.com # Additional domain name that should be trusted` 

AOS Security | Security Management Using Prism Element | **118** 

`DNS.3 = *.domain.com # Wildcard entry to match any subdomain under domain.com IP.0  = x.x.x.x # IP address of the CVM IP.1  = y.y.y.y # Additional IP address of the CVM` 

> _`[alt_names]`_ ~~a~~ Specify your DNS and IP addresses. If you have a range of hosts, use wildcards (*) to match any subdomain of the domain name. 

**6.** Generate a self-signed certificate: 

`nutanix@cvm$ openssl x509 -req -days` _`number_of_days`_ `-in my_csr_name.csr -signkey my_key_name.key -out my_crt_name.crt` _`-signature_algorithm`_ `-extensions v3_req - extfile san.cnf` 

> _`number_of_days`_ ~~a~~ : Specify the number of days until a newly generated certificate expires. 

> _`signature_algorithm`_ ee : Specify sha256, sha384, or sha512. Ensure that you use the same signature algorithm that was used to generate the CSR. 

Example: 

`nutanix@cvm$ openssl x509 -req -days 1460 -in my_csr_name.csr -signkey my_key_name.key -out my_crt_name.crt -sha256 -extensions v3_req -extfile san.cnf` 

**7.** Copy SS _`my_key_name.key`_ and _`my_crt_name.crt`_ from the CVM to your local machine: 

`nutanix@cvm$ scp my_key_name.key my_crt_name.crt` _`username@local-machine:/ local_file_path/`_ 

**8.** Log out of the CVM. 

## **What to do next** 

After generating the self-signed certificate with a private key, follow the procedure described in Importing a Self-Signed SSL Certificate in Prism Element on page 123 to replace the default certificate with your self-signed SSL certificate. The following table lists certificate components and its corresponding file type to choose when SSL certificate window prompts: 

**Table 7: SSL Certificate Import Files** 

|**Certificate Components**|**File type**|
|---|---|
|Private Key|my_key_name.key|
|Public Certificate|my_crt_name.crt|
|CA Certificate/Chain|my_crt_name.crt|



## **Generating a Certificate Signing Request with Subject Alternative Name for Submission to Certificate Authority** 

Generate a Certificate Signing Request (CSR) with Subject Alternative Name (SAN) using OpenSSL for Certificate Authority (CA) submission. 

## **Before you begin** 

- The certificate import process validates that the key and certificate pair uses the correct signature algorithm to comply with NIST SP800-131a and RFC 6460 (NSA Suite B) standards. Use the correct key types, sizes, curves, and signature algorithms. For more information, see Supported Key Configurations for SSL Certificates on page 127. 

AOS Security | Security Management Using Prism Element | **119** 

- Nutanix recommends including a DNS name for all CVMs in the CSR using the SAN extension to avoid SSL certificate errors when accessing a CVM through its DNS name instead of the shared cluster IP address. 

## **About this task** 

To generate a CSR with SAN, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: 

   - `$ ssh nutanix@` ~~SSS~~ _`cvm_ip_address`_ 

**2.** Create a configuration file in your home directory with your preferred text editor named _`ssl.cnf`_ a that contains the following text: 

`[req] distinguished_name = req_distinguished_name req_extensions = v3_req prompt = no [req_distinguished_name] countryName = # Country Name (2 letter code) stateOrProvinceName = # State or Province Name (full name) localityName = # Locality Name (eg, city) organizationName = # Organization Name (eg, company) organizationalUnitName = # Organizational Unit Name (eg, BU) commonName = # Common Name (e.g. server FQDN or YOUR name) emailAddress = # Email Address [v3_req] subjectAltName = @alt_names [alt_names] DNS.0 = example1.domain.com # Primary domain or fully qualified domain name (FQDN) of the server DNS.1 = example2.domain.com # Secondary domain or alias for the server DNS.2 = example3.domain.com # Additional domain name that should be trusted DNS.3 = *.domain.com # Wildcard entry to match any subdomain under domain.com IP.0  = x.x.x.x # IP address of the CVM IP.1  = y.y.y.y # Additional IP address of the CVM` 

> _`[alt_names]`_ ~~a~~ - Specify your DNS and IP addresses. If you have a range of hosts, use wildcards (*) to match any subdomain of the domain name. 

**3.** Generate a private key: 

   - RSA private key with a bit length of 2048: 

      - `nutanix@cvm$ openssl genrsa -out` ~~SCS~~ _`my_key_name.key`_ `2048` 

   - RSA private key with a bit length of 4096: 

      - `nutanix@cvm$ openssl genrsa -out` ~~CSS~~ _`my_key_name.key`_ `4096` 

   - ECDSA private key using the prime256v1 curve: 

> `nutanix@cvm$ openssl ecparam -name prime256v1 -genkey -out` ~~ee~~ _`my_key_name.pem`_ 

**Important:** While you are generating the private key, ensure that the private key is not password protected. 

AOS Security | Security Management Using Prism Element | **120** 

**4.** Generate a CSR: 

   - For RSA 2048 and RSA 4096 private keys: 

`nutanix@cvm$ openssl req -new -nodes -key my_key_name.key` _`-signature_algorithm`_ `- out my_csr_name.csr -config ssl.cnf` 

> _`signature_algorithm: Specify sha256 or sha384 or sha512.`_ ~~SS~~ 

For example, to generate a CSR for RSA 2048 private key using SHA-256 signature algorithm: 

`nutanix@cvm$ openssl req -new -nodes -key my_key_name.key -sha256 -out my_csr_name.csr -config ssl.cnf` 

- For ECDSA 256 private key: 

`nutanix@cvm$ openssl req -new -nodes -key my_key_name.pem -sha256 -out my_csr_name.csr -config ssl.cnf` 

For example, to generate a CSR for ECDSA 256 private key using SHA-256 signature algorithm: 

`nutanix@cvm$ openssl req -new -nodes -key my_key_name.pem -sha256 -out my_csr_name.csr -config ssl.cnf` 

**5.** Copy SS _`my_key_name.key`_ and _`my_crt_name.csr`_ from the CVM to your local machine: 

`nutanix@cvm$ scp my_key_name.key my_csr_name.csr` _`username@local-machine:/ local_file_path/`_ 

**6.** Log out of the CVM. 

**7.** Send your CSR file to the Certificate Authority (CA) of your choice. 

After receiving your CSR, the CA sends the following files: 

- CA signed public certificate 

- CA's public certificate 

- Root CA public certificate (if the CA is intermediate) 

The issuing CA validates the public certificate. If the issuing CA is intermediate, the root CA validates the issuing CA certificate. The system validates the certificate chain to establish trust. 

**8.** Download all the certificate files received from CA to the local file directory. 

**9.** (Optional) If the CA chain certificate provided by the certificate authority is not in a single file, run the following command to concatenate the list of CA certificates into a chain file: 

   - `$ cat intermediateCAcert.crt rootCAcert.crt > ca_chain_certs.crt` ~~i~~ e—SSSCSCSC Start the chain with the signer’s certificate and end it with the root CA certificate. 

Ensure that the chain file only has the root and intermediate certificates. Prism Element fails to import the chain file if it contains public or private certificates. 

## **What to do next** 

Follow Importing a Self-Signed SSL Certificate in Prism Element on page 123 section to replace the default certificate with a CA signed certificate. The following table lists certificate components and its corresponding file type to choose when SSL certificate window prompts: 

AOS Security | Security Management Using Prism Element | **121** 

## **Table 8: SSL Certificate Import Files** 

|**Certificate Components**|**File type**|
|---|---|
|Private Key|my_key_name.key|
|Public Certificate|ca_signed_public_cert.cer|
|CA Certificate/Chain|ca_public_cert.crt or ca_chain_certs.crt|



## **Verifying the Certificate Generation Request** 

Run the following commands to verify the certificate generation request. 

- Verify that the CA certificate chain is valid: 

   - ~~ee~~ `nutanix@cvm$ openssl verify -CAfile ca_chain_certs.crt myPublicCert.cer` 

Example output: 

~~ee~~ `myPublicCert.cer: OK` 

- Verify the private key and signature algorithm details: 

`nutanix@cvm$ openssl x509 -in my_cert_name.crt -text -noout | grep -i 'rsa\|ecdsa\| Public'` 

Example output: 

`Signature Algorithm: ecdsa-with-SHA256 Subject Public Key Info: Public Key Algorithm: id-ecPublicKey Public-Key: (256 bit) Signature Algorithm: ecdsa-with-SHA256` 

- Verify that the CA certificate chain uses SHA 256 as the signature algorithm: 

`nutanix@cvm$ openssl crl2pkcs7 -nocrl -certfile ca_chain_certs.crt | openssl pkcs7 - print_certs -noout -text | grep -Ew '(Subject|Issuer|Signature Algorithm):' | grep - C1 Issuer` 

## **Troubleshooting the Certificate Generation Request** 

The following troubleshooting tips can help you resolve common issues that can occur when generating certificates. 

## **Chain certificate format** 

If your chain certificate file has public or private certificates, it will fail to import in Prism Element web console. Ensure that the chain certificate file only has the root and intermediate certificates. 

For example, if a public certificate is present in a chain file, you can remove it by opening your chain file in your preferred text editor. Ensure that there are no extra white spaces at the bottom of the file. 

## **DER-encoded certificate issue** 

If the certificate is DER encoded, it fails to import in Prism Element web console. You can resolve the issue by converting it to PEM-encoded ASCII format. 

- Ensure that the certificate is DER encoded: 

~~CSS~~ `nutanix@cvm$ openssl x509 -in cert.crt -inform der -text -noout` 

AOS Security | Security Management Using Prism Element | **122** 

- If the certificate is DER encoded, run the following command to convert the certificate from DER to PEMencoded ASCII format: 

`nutanix@cvm$ openssl x509 -in certDER.crt -inform der -outform pem -out cert.crt` 

## **Certificate format** 

Ensure that all the certificates do not have any extra data (or custom attributes) before the beginning `(-----BEGIN CERTIFICATE-----)` or after the end `(-----END CERTIFICATE-----)` of the block. 

## **Importing a Self-Signed SSL Certificate in Prism Element** 

Import a self-signed SSL certificate into Prism Element. 

## **Before you begin** 

Ensure that you generate a self-signed SSL certificate. For information, see Generating a Self-signed SSL Certificate with Subject Alternative Name on page 117. 

## **About this task** 

To import a self-signed SSL certificate into Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **SSL Certificate** . 

**4.** Click **Replace Certificate** . 

**5.** Select **Import Key and Certificate** and click **Next** . 

AOS Security | Security Management Using Prism Element | **123** 

**6.** On the SSL certificate window, provide the following information: 

   - **Private Key Type** : From the dropdown list, select the appropriate private key type for the self-signed certificate. 

**Note:** EC DSA 521 and EC DSA 384 bit private keys are not supported. 

- **Private Key** : Click **Choose file** and select the private key. 

**Note:** The private key that you import must be unencrypted. Password-protected private keys are not supported. 

- **Public Certificate** : Click **Choose file** and select the self-signed certificate corresponding to the private key. 

- **CA Certificate/Chain** : Click **Choose file** select the self-signed certificate corresponding to the private key. 

**Figure 10: Importing self-signed certificate** 

The following table lists certificate components and its corresponding file type to choose when SSL certificate window prompts: 

|**Certificate Components**|**File type**|
|---|---|
|Private Key|my_key_name.key|
|Public Certificate|my_crt_name.crt|
|CA Certificate/Chain|my_crt_name.crt|



AOS Security | Security Management Using Prism Element | **124** 

## **7.** Click **Import Files** . 

**Note:** Prism Element stores only one custom SSL certificate. When you upload a new certificate, Prism Element replaces the existing certificate. 

After you import the new certificate, the Prism Element web console restarts. If the certificate and credentials are valid, Prism Element uses the new certificate immediately. All open browser sessions becomes invalid until you reload the page and accept the new certificate. If the certificate is invalid due to corrupted file or wrong certificate type, Prism Element discards the new certificate and reverts to the default certificate provided by Nutanix. 

## **Importing CA-Signed SSL Certificate in Prism Element** 

Import a certificate authority (CA) signed SSL certificate into Prism Element. 

## **Before you begin** 

Ensure that you generate a Certificate Signing Request (CSR) for submission to a CA. For more information, see Generating a Certificate Signing Request with Subject Alternative Name for Submission to Certificate Authority on page 119. 

## **About this task** 

To import a CA-signed SSL certificate into Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **SSL Certificate** . 

**4.** Click **Replace Certificate** . 

**5.** Select **Import Key and Certificate** and click **Next** . 

AOS Security | Security Management Using Prism Element | **125** 

**6.** On the SSL certificate window, provide the following information: 

   - **Private Key Type** : From the dropdown list, select the appropriate private key type for the CA signed certificate. 

**Note:** Prism Central does not support EC DSA 521 and EC DSA 384 bit private keys. 

- **Private Key** : Click **Choose file** and select the private key. 

**Note:** The private key that you import must be unencrypted. Password-protected private keys are not supported. 

- **Public Certificate** : Click **Choose file** and select the CA signed public portion of the certificate corresponding to the private key. 

- **CA Certificate/Chain** : Click **Choose file** and select the certificate or chain of the signing authority for the public certificate. 

**Note:** For more information on how to create a chain file from the list of CA certificates, see Generating a Certificate Signing Request with Subject Alternative Name for Submission to Certificate Authority on page 67. 

**Figure 11: Importing CA-signed certificate** 

The following table lists certificate components and its corresponding file type to choose when SSL certificate window prompts: 

AOS Security | Security Management Using Prism Element | **126** 

**Table 9: CA Signed Certificate File Type** 

|**Certificate Components**|**File type**|
|---|---|
|Private Key|my_key_name.key|
|Public Certificate|ca_signed_public_cert.cer|
|CA Certificate/Chain|ca_public_cert.crt or ca_chain_certs.crt|



**7.** Click **Import Files** . 

**Note:** Prism Element stores only one custom SSL certificate. When you upload a new certificate, Prism Element replaces the existing certificate. 

After you import the new certificate, the Prism Element web console restarts. If the certificate and credentials are valid, Prism Element uses the new certificate immediately. All open browser sessions becomes invalid until you reload the page and accept the new certificate. If the certificate is invalid due to corrupted file or wrong certificate type, Prism Element discards the new certificate and reverts to the default certificate provided by Nutanix. 

## **Regenerating Self-Signed Certificate in Prism Element** 

Regenerate a self-signed SSL certificate in Prism Element. 

## **About this task** 

To regenerate a self-signed certificate in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **SSL Certificate** . 

**4.** Click **Replace Certificate** . 

## **5.** Select **Regenerate Self Signed Certificate** and click **Apply** . 

**6.** Click **OK** . 

Prism Element generates and applies a new RSA 2048 bit self-signed certificate. 

## **Supported Key Configurations for SSL Certificates** 

Nutanix supports only the following key types, sizes or curves, and signature algorithms for SSL certificates: 

**Table 10: SSL Certificate Key Type Options** 

|**Key Type**|**Size or Curve**|**Signature Algorithm**|
|---|---|---|
|RSA|4096|SHA-256, SHA-384 or SHA512|
|RSA|2048|SHA-256, SHA-384 or SHA512|
|EC DSA 256|prime256v1|ecdsa-with-sha256|



AOS Security | Security Management Using Prism Element | **127** 

In Prism Central versions prior to pc.2024.3, uploading an RSA 4096-bit certificate might cause some issues. For more information, see KB 12775 . 

Nutanix does not support SHA-1 certificates, including root CAs. 

Client and CAC authentication support only RSA 2048-bit certificates. 

## **Cluster Lockdown in Prism Element** 

Nutanix supports key-based SSH access to Prism Element. 

Cluster lockdown in Prism Element enhances security by disabling the password-based SSH login. Instead, authorized user SSH keys provide access to Prism Element, which improves the overall security posture. 

Nutanix supports the following key-based SSH encryption algorithms: 

- AES128-CTR 

- AES192-CTR 

- AES256-CTR 

Nutanix supports the following key types: 

- RSA 

- ECDSA 

## **Cluster Lockdown Considerations** 

Before you configure the cluster lockdown in Prism Element, consider the following points: 

- Generate the public key using the `ssh-keygen` command on Mac or Linux, or the PuTTY application on Windows. For more information, see KB-1895 . 

- When you enable the cluster lockdown, Prism Element does not store the passwords for both the CVM and host, and you cannot change these passwords to access the cluster resources. 

- Adding a user SSH key enables SSH access for both the `nutanix` and `admin` accounts on the CVM and host. 

- Disabling remote login and deleting all user SSH keys locks down cluster SSH access. 

- You can configure multiple user SSH keys. 

- For additional security, you can configure SSH security level for the CVM. For more information, see CVM Security Hardening on page 161. 

## **Configuring Cluster Lockdown in Prism Element** 

Configure cluster lockdown to enable SSH key-based access to your cluster. 

## **Before you begin** 

Ensure that you generate the public key using `ssh-gen` command on Mac or Linux, or the PuTTY application on Windows. For more information, see KB-1895. 

## **About this task** 

To configure the cluster lockdown in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

AOS Security | Security Management Using Prism Element | **128** 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Cluster Lockdown** . 

**4.** Deselect the **Enable Remote Login with Password** checkbox. 

**5.** Click **+ New user SSH Key** and enter the following information: 

   - **Name** : Enter a key name. 

   - **Key** : Paste the public key value. 

**6.** Click **Save** . 

## **Deleting a User SSH Key in Prism Element** 

You can disable the SSH key-based access to your cluster. 

## **About this task** 

**Caution:** Disabling remote login and deleting all user SSH keys locks down the cluster SSH access. Ensure to select the **Enable Remote Login with Password** checkbox before deleting all the user SSH keys. 

To remove the user SSH key, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Cluster Lockdown** . 

**4.** Select the **Enable Remote Login with Password** checkbox. 

**5.** Click the **X** remove icon. 

**6.** Select **OK** . 

## **Data-at-Rest Encryption** 

Nutanix provides an option to secure data while it is at rest using either self-encrypted drives or softwareonly encryption and key-based access management (cluster's native or external KMS for software-only encryption). 

## **Encryption Methods** 

Nutanix provides you with the following options to secure your data. 

- **Self Encrypting Drives (SED) Encryption** - You can use a combination of SEDs and an external KMS to secure your data while it is at rest. 

- **Software-only Encryption** - Nutanix AOS uses the AES-256 encryption standard to encrypt your data. Once enabled, software-only data-at-rest encryption cannot be disabled, thus protecting against accidental data leaks due to human errors. Software-only encryption supports both Nutanix Native Key Manager (local and remote) and External KMS to secure your keys. 

Note the following points regarding data-at-rest encryption. 

AOS Security | Security Management Using Prism Element | **129** 

- Encryption is supported for AHV, ESXi, and Hyper-V. 

   - For ESXi and Hyper-V, software-only encryption can be implemented at a cluster level or container level. 

   - For AHV, encryption can be implemented at the cluster level, VM level, or VG level. For more information about VM or VG level encryption, see Storage Policy Based Encryption . 

- Nutanix recommends using cluster-level encryption. With the cluster-level encryption, the administrative overhead of selecting different containers for the data storage gets eliminated. 

- Encryption cannot be disabled once it is enabled at a cluster level or container level. 

- Encryption can be implemented on an existing cluster with data that exists. If encryption is enabled on an existing cluster (AHV, ESXi, or Hyper-V), the unencrypted data is transformed into an encrypted format in a low priority background task that is designed not to interfere with other workload running in the cluster. 

- Data-at-rest encryption choices can be implemented at the entity level using storage policies in Prism Central. 

Deployments can continue to have data-at-rest encryption capabilities scoped for the entire cluster. Storage policy provides the additional option to control the encryption scope decisions at the entity (VM or VG) level. For more information, see Storage Policies Based Encryption in the _Prism Central Infrastructure Guide_ 

- Data can be encrypted using either self-encrypted drives (SEDs) or software-only encryption. You can change the encryption method from SEDs to software-only. You can perform the following configurations. 

   - For ESXi and Hyper-V clusters, you can switch from SEDs and External Key Management (EKM) combination to software-only encryption and EKM combination. First, you must disable the encryption in the cluster where you want to change the encryption method. Then, select the cluster and enable encryption to transform the unencrypted data into an encrypted format in the background. 

   - For AHV, background encryption is supported. 

- Once the task to encrypt a cluster begins, you cannot cancel the operation. Even if you stop and restart the cluster, the system resumes the operation. 

- In the case of mixed clusters with ESXi and AHV nodes, where the AHV nodes are used for storage only, the encryption policies consider the cluster as an ESXi cluster. So, the cluster-level and container-level encryption are available. 

- You can use a combination of SED and non-SED drives in a cluster. After you encrypt a cluster using the software-only encryption, all the drives are considered as unencrypted drives. In case you switch from the SED encryption to the software-only encryption, you can add SED or non-SED drives to the cluster. 

- Data-at-rest encryption is configured per cluster. Data is encrypted as part of the write operation and decrypted as part of the read operation. During the replication process, the source cluster reads and decrypts the data before sending it to the destination cluster, where the data is encrypted at rest only if the destination cluster also has dataat-rest encryption enabled. Therefore, you must enable encryption on each cluster where data must be encrypted at rest. Starting with AOS 7.3, AOS encrypts replication traffic between clusters by default. For more information, see Replication Traffic Encryption in the _Data Protection and Recovery with Prism Element Guide_ . 

- Software-only encryption does not impact most of data efficiency features such as deduplication, compression, zero block suppression, and so on. The software encryption is the last data transformation performed. For example, during the write operation, compression is performed first, followed by encryption. 

- Software-only encryption requires additional space if erasure coding (EC) is enabled on the cluster (or container). The additional space requirement is temporary, and EC space savings are restored once the encryption process is complete. 

- Enabling encryption on EC-enabled clusters involves completely decoding EC-encoded data and re-encoding the data for EC. This process of decoding and re-encoding requires additional space on the cluster. Encryption is initiated only if the aggregate of EC-based space saving and current space usage is less than 85 percent of the 

AOS Security | Security Management Using Prism Element | **130** 

cluster capacity. Additionally, the cluster continuously checks if the aggregate space usage stays below 85 percent for the encryption to progress. 

## **Key Management** 

Nutanix supports a Native Key Management Server, also called Local Key Manager (LKM), thus avoiding the dependency on an External Key Manager (EKM). Cluster localised Key Management Service support requires a minimum of 3-node in a cluster and is supported only for software-only encryption. So, 1-node and 2-node clusters can use either the Native KMS (remote) option or an EKM. . 

The following types of keys are used for encryption. 

- Data Encryption Key (DEK): A symmetric key, such as AES-256, that is used to encrypt the data. 

- Key Encryption Key (KEK): This key is used to encrypt or decrypt the DEK. 

Note the following points regarding the key management. 

- Nutanix does not support the use of the Local Key Manager with a third party External Key Manager. 

- Dual encryption (both SED and software-only encryption) requires an EKM. For more information, see Configuring Dual Encryption on page 152. 

- You can switch from an EKM to LKM, and inversely. For more information, see Switching between Native Key Manager and External Key Manager on page 148. 

- Rekey of keys stored in the Native KMS is supported for the Master Keys. For more information, see Changing Key Encryption Keys (SEDs) on page 138 and Changing Key Encryption Keys (Software Only) on page 150. 

- You must back up the keys stored in the Native KMS. For more information, see Backing up Keys on page 152. 

- You must backup the encryption keys whenever you create a new container or remove an existing container. Nutanix Cluster Check (NCC) checks the status of the backup and sends an alert if you do not take a backup at the time of creating or removing a container. 

## **Data-at-Rest Encryption (SEDs)** 

For customers who require enhanced data security, Nutanix provides a data-at-rest security option using Self Encrypting Drives (SEDs) included in the Ultimate license. 

**Note:** Starting with AOS 7.3, Nutanix supports NVMe self-encrypting drives (SEDs) for data-at-rest encryption. The support of any NVMe SEDs with a specific platform requires qualification with AOS version 7.3 or later. If you are running the AOS Pro License on G6 platforms and later, you can use SED encryption by installing an add-on license. 

Following features are supported: 

- Data is encrypted on all drives at all times. 

- Data is inaccessible in the event of drive or node theft. 

- Data on a drive can be securely destroyed. 

- A key authorization method allows password rotation at arbitrary times. 

- Protection can be enabled or disabled at any time. 

- No performance penalty is incurred despite encrypting all data. 

- Re-key of the master encryption key (MEK) at arbitrary times is supported. 

AOS Security | Security Management Using Prism Element | **131** 

**Note:** If an SED cluster is present, then while executing the data-at-rest encryption, you will get an option to either select data-at-rest encryption using SEDs or data-at-rest encryption using AOS. 

## **Figure 12: SED and AOS Options** 

This solution provides enhanced security for data on a drive, but it does not secure data in transit. 

## **Data Encryption Model** 

To accomplish these goals, Nutanix implements a data security configuration that uses SEDs with keys maintained through a separate key management device. Nutanix uses open standards (TCG and KMIP protocols) and FIPS validated SED drives for interoperability and strong security. 

**Figure 13: Cluster Protection Overview** 

This configuration involves the following workflow: 

**1.** The security implementation begins by installing SEDs for all data drives in a cluster. 

The drives are FIPS 140-2 validated and use FIPS 140-2 validated cryptographic modules. 

For OEM platforms, contact the respective OEM partners to confirm FIPS certification on SED qualified drives for AOS. 

Creating a new cluster that includes SEDs only is straightforward, but an existing cluster can be converted to support data-at-rest encryption by replacing the existing drives with SEDs (after migrating all the VMs/vDisks off of the cluster while the drives are being replaced). 

**Note:** Contact Nutanix customer support for assistance before attempting to convert an existing cluster. A nonprotected cluster can contain both SED and standard drives, but Nutanix does not support a mixed cluster when protection is enabled. All the disks in a protected cluster must be SED drives. 

**2.** Data on the drives is always encrypted but read or write access to that data is open. By default, the access to data on the drives is protected by the in-built manufacturer key. However, when data protection for the cluster is enabled, the Controller VM must provide the proper key to access data on a SED. The Controller VM 

AOS Security | Security Management Using Prism Element | **132** 

communicates with the SEDs through a Trusted Computing Group (TCG) Security Subsystem Class (SSC) Enterprise protocol. 

A symmetric data encryption key (DEK) such as AES 256 is applied to all data being written to or read from the disk. The key is known only to the drive controller and never leaves the physical subsystem, so there is no way to access the data directly from the drive. 

Another key, known as a key encryption key (KEK), is used to encrypt/decrypt the DEK and authenticate to the drive. (Some vendors call this the authentication key or PIN.) 

Each drive has a separate KEK that is generated through the FIPS compliant random number generator present in the drive controller. The KEK is 32 bytes long to resist brute force attacks. The KEKs are sent to the key management server for secure storage and later retrieval; they are not stored locally on the node (even though they are generated locally). 

In addition to the above, the master encryption key (MEK) is used to encrypt the KEKs. 

Each node maintains a set of certificates and keys in order to establish a secure connection with the external key management server. 

**3.** Keys are stored in a key management server that is outside the cluster, and the Controller VM communicates with the key management server using the Key Management Interoperability Protocol (KMIP) to upload and retrieve drive keys. 

Only one key management server device is required, but it is recommended that multiple devices are employed so the key management server is not a potential single point of failure. Configure the key manager server devices to work in clustered mode so they can be added to the cluster configuration as a single entity that is resilient to a single failure. 

**4.** When a node experiences a full power off and power on (and cluster protection is enabled), the controller VM retrieves the drive keys from the key management server and uses them to unlock the drives. 

   - If the Controller VM cannot get the correct keys from the key management server, it cannot access data on the drives. 

If a drive is re-seated, it becomes locked. 

If a drive is stolen, the data is inaccessible without the KEK (which cannot be obtained from the drive). If a node is stolen, the key management server can revoke the node certificates to ensure they cannot be used to access data on any of the drives. 

## **Preparing for Data-at-Rest Encryption (External KMS for SEDs and Software Only)** 

## **About this task** 

**Caution:** Do not host a key management server vm on the encrypted cluster that is using it. Doing so could result in complete data loss if there is a problem with the VM while it is hosted in that cluster. 

If you are using an external KMS for encryption using AOS, preparation steps outside the web console are required. The information in this section is applicable if you choose to use an external KMS for configuring encryption. 

You must install the license of the external key manager for all nodes in the cluster. For a complete list of the supported key management servers, see Compatibility and Interoperability Matrix . For instructions on how to configure a key management server, refer to the documentation from the appropriate vendor. 

The system accesses the EKM under the following conditions: 

- Starting a cluster 

- Regenerating a key (key regeneration occurs automatically every year by default) 

- Adding or removing a node (only when Self Encrypting Drives is used for encryption) 

- Switching between Native to EKM or EKM to Native 

AOS Security | Security Management Using Prism Element | **133** 

- Starting and restarting a service (only if Software-based encryption is used) 

- Upgrading AOS (only if Software-based encryption is used) 

- NCC heartbeat check if EKM is alive 

## **Procedure** 

**1.** Configure a key management server. 

The key management server devices must be configured into the network so the cluster has access to those devices. For redundant protection, it is recommended that you employ at least two key management server devices, either in active-active cluster mode or stand-alone. 

**Note:** The key management server must support KMIP version 1.0 or later. 

- » SafeNet 

Ensure that **Security** > **High Security** > **Key Security** > **Disable Creation and Use of Global Keys** is checked. 

- » Vormetric 

Set the appliance to compatibility mode. Suite B mode causes the SSL handshake to fail. 

**2.** Generate a certificate signing request (CSR) for each node in the cluster. 

   - The Common Name field of the CSR is populated automatically with _`unique_node_identifier`_ `.nutanix.com` to identify the node associated with the certificate. 

   - (Optional) After generating the CSR from Prism, to change the domain name portion of the Common Name (CN) to your own domain, run the following command: 

`nutanix@cvm$ ncli data-at-rest-encryption-certificate update-csr-information domain-name=` _`abcd.test.com`_ 

> Replace ae _`abcd.test.com`_ with the actual domain name. 

- (Optional) To change the entire CN, contact Nutanix Support. 

- A UID field is populated with a value of `Nutanix` a . This can be useful when configuring a Nutanix group for access control within a key management server, because it is based on fields within the client certificates. 

**Note:** Some vendors when doing client certificate authentication expect the client username to be a field in the CSR. While the CN and UID are pre-generated, many of the user populated fields can be used instead if desired. If a node-unique field such as CN is chosen, users must be created on a per node basis for access control. If a clusterunique field is chosen, customers must create a user for each cluster. 

AOS Security | Security Management Using Prism Element | **134** 

**3.** Send the CSRs to a certificate authority (CA) and get them signed. 

   - » Safenet 

The SafeNet KeySecure key management server includes a local CA option to generate signed certificates, or you can use other third-party vendors to create the signed certificates. 

To enable FIPS compliance, add user `nutanix` to the CA that signed the CSR. Under **Security** > **High Security** > **FIPS Compliance** click **Set FIPS Compliant** . 

**Note:** Some CAs strip the UID field when returning a signed certificate. 

To comply with FIPS, Nutanix does not support the creation of global keys. 

In the SafeNet KeySecure management console, go to **Device** > **Key Server** > **Key Server** > **KMIP Properties** > **Authentication Settings** . 

Then do the following: 

- Set the **Username Field in Client Certificate** option to **UID (User ID)** . 

- Set the **Client Certificate Authentication** option to **Used for SSL session and username** . 

If you do not perform these settings, the KMS creates global keys and fails to encrypt the clusters or containers using the software only method. 

**4.** Upload the signed SSL certificates (one for each node) and the certificate for the CA to the cluster. These certificates are used to authenticate with the key management server. 

**5.** Generate keys (KEKs) for the SED drives and upload those keys to the key management server. 

## **Configuring Data-at-Rest Encryption (SEDs)** 

Nutanix offers an option to use self-encrypting drives (SEDs) to store data in a cluster. When SEDs are used, there are several configuration steps that must be performed to support data-at-rest encryption in the cluster. 

## **Before you begin** 

A separate key management server is required to store the keys outside of the cluster. Each key management server device must be configured and addressable through the network. It is recommended that multiple key manager server devices be configured to work in clustered mode so they can be added to the cluster configuration as a single entity (see step 5) that is resilient to a single failure. 

## **About this task** 

To configure cluster encryption, do the following: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the main menu and then select **Data at Rest Encryption** in the **Settings** page. The **Data at Rest Encryption** dialog box appears. Initially, encryption is not configured, and a message to that effect appears. 

**3.** Click the **Create Configuration** button. 

Clicking the **Continue Configuration** button, **configure it** link, or **Edit Config** button does the same thing, which is display the Data-at-Rest Encryption configuration page. 

**4.** Select the Encryption Type as **Drive-based Encryption** . This option is displayed only when SEDs are detected. 

AOS Security | Security Management Using Prism Element | **135** 

**5.** In the **Certificate Signing Request Information** section, do the following: 

   - a. Enter appropriate credentials for your organization in the **Email** , **Organization** , **Organizational Unit** , **Country Code** , **City** , and **State** fields and then click the **Save CSR Info** button. 

The entered information is saved and is used when creating a certificate signing request (CSR). To specify more than one **Organization Unit** name, enter a comma separated list. 

**Note:** You can update this information until an SSL certificate for a node is uploaded to the cluster, at which point the information cannot be changed (the fields become read only) without first deleting the uploaded certificates. 

- b. Click the **Download CSRs** button, and then in the new screen click the **Download CSRs for all nodes** to download a file with CSRs for all the nodes or click a **Download** link to download a file with the CSR for that node. 

- c. Send the files with the CSRs to the desired certificate authority. 

The certificate authority creates the signed certificates and returns them to you. Store the returned SSL certificates and the CA certificate where you can retrieve them for step 6. 

      - The certificates must be X.509 format. (DER, PKCS, and PFX formats are not supported.) 

      - The certificate and the private key should be in separate files. 

**6.** In the **Key Management Server** section, do the following: 

   - a. Click the **Add New Key Management Server** button. 

   - b. In the **Add a New Key Management Server** screen, enter a name, IP address, and port number for the key management server in the appropriate fields. 

The port is where the key management server is configured to listen for the KMIP protocol. The default port number is 5696. For the complete list of required ports, see Port Reference . 

- » If you have configured multiple key management servers in cluster mode, click the **Add Address** button to provide the addresses for each key management server device in the cluster. 

- » If you have stand-alone key management servers, click the **Save** button. Repeat this step ( **Add New Key Management Server** button) for each key management server device to add. 

If your key management servers are configured into a leader/follower (active/passive) relationship and the architecture is such that the follower cannot accept write requests, do not add the follower into this configuration. The system sends requests (read or write) to any configured key management server, so both read and write access is needed for key management servers added here. 

To prevent potential configuration problems, always use the **Add Address** button for key management servers configured into cluster mode. Only a stand-alone key management server should be added as a new server. 

   - c. To edit any settings, click the pencil icon for that entry in the key management server list to redisplay the add page and then click the **Save** button after making the change. To delete an entry, click the X icon. 

**7.** In the **Add a New Certificate Authority** section, enter a name for the CA, click the **Upload CA Certificate** button, and select the certificate for the CA used to sign your node certificates (see step 4c). Repeat this step for all CAs that were used in the signing process. 

AOS Security | Security Management Using Prism Element | **136** 

**8.** Go to the **Key Management Server** section (see step 5) and do the following: 

   - a. Click the **Manage Certificates** button for a key management server. 

   - b. In the **Manage Signed Certificates** screen, upload the node certificates either by clicking the **Upload Files** button to upload all the certificates in one step or by clicking the **Upload** link (not shown in the figure) for each node individually. 

   - c. Test that the certificates are correct either by clicking the **Test all nodes** button to test the certificates for all nodes in one step or by clicking the **Test CS** (or **Re-Test CS** ) link for each node individually. A status of Verified indicates the test was successful for that node. 

**Note:** Before removing a drive or node from an SED cluster, ensure that the testing is successful and the status is **Verified** . Otherwise, the drive or node will be locked. 

- a. Repeat this step for each key management server. 

**Note:** Before removing a drive or node from an SED cluster, ensure that the testing is successful and the status is **Verified** . Otherwise, the drive or node will be locked. 

**Figure 14: Upload Signed Certificates Screen** 

AOS Security | Security Management Using Prism Element | **137** 

**9.** When the configuration is complete, click the **Protect** button on the opening page to enable encryption protection for the cluster. 

A clear key icon appears on the page. 

The key turns gold when cluster encryption is enabled. 

**Note:** If changes are made to the configuration after protection has been enabled, such as adding a new key management server, you must rekey the disks for the modification to take full effect (see Changing Key Encryption Keys (SEDs) on page 138). 

## **Figure 15: Data-at-Rest Encryption Screen (protected)** 

## **Enabling/Disabling Encryption (SEDs)** 

Data on a self encrypting drive (SED) is always encrypted, but enabling/disabling data-at-rest encryption for the cluster determines whether a separate (and secured) key is required to access that data. 

## **About this task** 

To enable or disable data-at-rest encryption after it has been configured for the cluster (see Configuring Data-atRest Encryption (SEDs) on page 135), do the following: 

**Note:** The key management server must be accessible to disable encryption. 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the main menu and then select **Data at Rest Encryption** in the **Settings** page. 

**3.** In the **Cluster Encryption** page, do one of the following: 

   - » If cluster encryption is enabled currently, click the **Unprotect** button to disable it. 

   - » If cluster encryption is disabled currently, click the **Protect** button to enable it. 

Enabling cluster encryption enforces the use of secured keys to access data on the SEDs in the cluster; disabling cluster encryption means the data can be accessed without providing a key. 

## **Changing Key Encryption Keys (SEDs)** 

The key encryption key (KEK) can be changed at any time. This can be useful as a periodic password rotation security precaution or when a key management server or node becomes compromised. If the key management server is compromised, only the KEK needs to be changed, because the KEK is independent of the drive encryption key (DEK). There is no need to re-encrypt any data, just to re-encrypt the DEK. 

AOS Security | Security Management Using Prism Element | **138** 

## **About this task** 

To change the KEKs for a cluster, do the following: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the main menu and then select **Data at Rest Encryption** in the **Settings** page. 

**3.** In the **Cluster Encryption** page, select **Manage Keys** and click the **Rekey All Disks** button under **Hardware Encryption** . 

Rekeying a cluster under heavy workloads might result in higher-than-normal IO latency, and some data might become temporarily unavailable. To continue with the rekey operation, click **Confirm Rekey** . 

This step resets the KEKs for all the self encrypting disks in the cluster. 

**Note:** The **Rekey All Disks** button appears only when cluster protection is active. If the cluster is already protected and a new key management server is added, you must press the **Rekey All Disks** button to use this new key management server for storing secrets. 

## **Destroying Data (SEDs)** 

Data on a self encrypting drive (SED) is always encrypted, and the data encryption key (DEK) used to read the encrypted data is known only to the drive controller. All data on the drive can effectively be destroyed (that is, become permanently unreadable) by having the controller change the DEK. This is known as a crypto-erase. 

## **About this task** 

To crypto-erase a SED, follow these steps: 

## **Procedure** 

**1.** In the Prism Element web console, go to the Hardware dashboard and select the **Diagram** tab. 

AOS Security | Security Management Using Prism Element | **139** 

**2.** Select the target disk in the diagram (upper section of screen) and then click the **Remove Disk** button (at the bottom right of the following diagram). 

As part of the disk removal process, the DEK for that disk is automatically cycled on the drive controller. The previous DEK is lost and all new disk reads are indecipherable. The key encryption key (KEK) is unchanged, and the new DEK is protected using the current KEK. 

When a node is removed, all SEDs in that node are crypto-erased automatically as part of the node removal process. 

When you run the cluster destroy command to decommission your entire cluster, the command automatically performs a crypto erase on the SED as the final step. 

When you destroy a cluster that uses SEDs, Nutanix permanently deletes the KEK from the system. Any residual data on the drives remains encrypted. The KEK is no longer available in hardware or software, and therefore, the data cannot be decrypted or recovered. 

**Figure 16: Removing a Disk** 

## **Data-at-Rest Encryption (Software Only)** 

For customers who require enhanced data security, Nutanix provides a software-only encryption option for data-at-rest security (SEDs not required). 

Starting with AOS version 7.3, software-based data-at-rest encryption and native key management service (KMS) capabilities are included with the Pro and Ultimate editions of the Nutanix Cloud Infrastructure (NCI) license. For earlier AOS versions, these features require either an Ultimate license or a Pro add-on license. Software encryption supports the following features: 

- For AHV, data can be encrypted at the cluster level. This applies to both empty clusters and clusters with existing data. In addition to cluster-level encryption, you can configure storage policies in Prism Central to encrypt data at the container or VM level. For more information, see Storage Policy-Based Encryption . 

- For ESXi and Hyper-V, the data can be encrypted on a cluster or container level. The cluster or container can be empty or contain existing data. Consider the following points for container level encryption. 

   - Once you enable container level encryption, you can not change the encryption type to cluster level encryption later. 

   - After the encryption is enabled, the administrator needs to enable encryption for every new container. 

- Data is encrypted at all times. 

- Data is inaccessible in the event of drive or node theft. 

- Data on a drive can be securely destroyed. 

AOS Security | Security Management Using Prism Element | **140** 

- Re-key of the master encryption key at arbitrary times is supported. 

- Cluster’s native KMS is supported. 

**Note:** In case of mixed hypervisors, only the following combinations are supported. 

- ESXi and AHV 

- Hyper-V and AHV 

This solution provides enhanced security for data on a drive, but it does not secure data in transit. 

## **Data Encryption Model** 

To accomplish the above mentioned goals, Nutanix implements a data security configuration that uses AOS functionality along with the cluster’s native or an external key management server. Nutanix uses open standards (KMIP protocols) for interoperability and strong security. 

**Figure 17: Cluster Protection Overview** 

This configuration involves the following workflow: 

- For software encryption, data protection must be enabled for the cluster before any data is encrypted. Also, the Controller VM must provide the proper key to access the data. 

- A symmetric data encryption key (DEK) such as AES 256 is applied to all data being written to or read from the disk. The key is known only to AOS, so there is no way to access the data directly from the drive. 

- In case of an external KMS: 

Each node maintains a set of certificates and keys in order to establish a secure connection with the key management server. 

Only one key management server device is required, but it is recommended that multiple devices are employed so the key management server is not a potential single point of failure. Configure the key manager server devices to work in clustered mode so they can be added to the cluster configuration as a single entity that is resilient to a single failure. 

## **Configuring Data-at-Rest Encryption (Software Only)** 

Nutanix offers a software-only option to perform data-at-rest encryption in a cluster or container. 

AOS Security | Security Management Using Prism Element | **141** 

## **Before you begin** 

- You must review Key Management Server (KMS) Considerations on page 147. 

- Starting with AOS version 7.3, software-based data-at-rest encryption and native key management service (KMS) capabilities are included with the Pro and Ultimate editions of the Nutanix Cloud Infrastructure (NCI) license. For earlier AOS versions, these features require either an Ultimate license or a Pro add-on license. 

**Caution:** For security, you cannot disable software-only data-at-rest encryption once it is enabled. 

## **About this task** 

To configure cluster or container encryption, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the main menu and then select **Data at Rest Encryption** in the **Settings** menu. The **Data at Rest Encryption** dialog box appears. Initially, encryption is not configured, and a message to that effect appears. 

**3.** Click the **Create Configuration** button. 

Clicking the **Continue Configuration** button, **configure it** link, or **Edit Config** button does the same thing, which is display to the Data-at-Rest Encryption configuration page 

AOS Security | Security Management Using Prism Element | **142** 

**4.** Select the Encryption Type as **Encrypt the entire cluster** or **Encrypt storage containers** . Then click **Save Encryption Type** . 

**Caution:** You can enable encryption for the entire cluster or just the container. However, if you enable encryption on a container; and you experience any encryption key issues like loss of encryption key, you can encounter the following: 

- The entire cluster data is affected, not just the encrypted container. 

- All the user VMs of the cluster cannot access the data. 

The hardware option is displayed only when SEDs are detected. Else, software based encryption type will be used by default. 

For ESXi and Hyper-V, the data can be encrypted on a cluster or container level. The cluster or container can be empty or contain existing data. Consider the following points for container level encryption. 

- Once you enable container level encryption, you cannot change the encryption type to cluster level encryption later. 

- After the encryption is enabled, the administrator needs to enable encryption for every new container. 

To enable encryption for every new storage container, follow these steps: 

- a. In the web console, select **Storage** from the pull-down main menu (upper left of screen) and then select the **Table** and **Storage Container** tabs. 

- b. To enable encryption, select the target storage container and then click the **Update** link. The **Update Storage Container** window appears. 

- c. In the **Advanced Settings** area, select the **Enable** check box to enable encryption for the storage container you selected. 

**Figure 18: Update storage container** 

AOS Security | Security Management Using Prism Element | **143** 

   - d. Click **Save** to complete. 

**5.** Select the Key Management Service. 

   - » Select **Native KMS (local)** or **Native KMS (remote)** and click **Save KMS type** . If you select Native KMS, skip to step 10 to enable encryption. 

   - » Select the **External KMS** option and click **Save KMS type** . Continue to step 6 to configure certificates and key management servers. 

   - **Native KMS (local)** requires a minimum of 3-node cluster. 1-node and 2-node clusters are not supported. 

   - **Native KMS (remote)** is for ROBO environments (typically, 1 or 2 node clusters) and provides enhanced security for software based encryption of ROBO clusters managed by Prism Central. This option is available only if the cluster is registered to Prism Central. 

**Note:** You can switch between the KMS types at a later stage if the specific KMS prerequisites are met, see Switching between Native Key Manager and External Key Manager on page 148. 

## **6.** 

- (External KMS only) In the **Certificate Signing Request Information** section, follow these steps: 

- a. Enter appropriate credentials for your organization in the **Email** , **Organization** , **Organizational Unit** , **Country Code** , **City** , and **State** fields and then click the **Save CSR Info** button. 

The entered information is saved and is used when creating a certificate signing request (CSR). To specify more than one **Organization Unit** name, enter a comma separated list. 

**Note:** You can update this information until an SSL certificate for a node is uploaded to the cluster, at which point the information cannot be changed (the fields become read only) without first deleting the uploaded certificates. 

- b. Click the **Download CSRs** button, and then in the new screen click the **Download CSRs for all nodes** to download a file with CSRs for all the nodes or click a **Download** link to download a file with the CSR for that node. 

- c. Send the files with the CSRs to the desired certificate authority. 

The certificate authority creates the signed certificates and returns them to you. Store the returned SSL certificates and the CA certificate where you can retrieve them in step 8. 

- The certificates must be X.509 format. (DER, PKCS, and PFX formats are not supported.) 

- The certificate and the private key must be in separate files. 

AOS Security | Security Management Using Prism Element | **144** 

**7.** (External KMS only) In the **Key Management Server** section, follow these steps: 

   - a. Click the **Add New Key Management Server** button. 

   - b. In the **Add a New Key Management Server** screen, enter a name, IP address, and port number for the key management server in the appropriate fields. 

The port is where the key management server is configured to listen for the KMIP protocol. The default port number is 5696. For the complete list of required ports, see Port Reference . 

- » If you have configured multiple key management servers in cluster mode, click the **Add Address** button to provide the addresses for each key management server device in the cluster. 

- » If you have stand-alone key management servers, click the **Save** button. Repeat this step ( **Add New Key Management Server** button) for each key management server device to add. 

If your key management servers are configured into a master/slave (active/passive) relationship and the architecture is such that the follower cannot accept write requests, do not add the follower into this configuration. The system sends requests (read or write) to any configured key management server, so both read and write access is needed for key management servers added here. 

To prevent potential configuration problems, always use the **Add Address** button for key management servers configured into cluster mode. Only a stand-alone key management server must be added as a new server. 

   - c. To edit any settings, click the pencil icon for that entry in the key management server list to redisplay the add page and then click the **Save** button after making the change. To delete an entry, click the X icon. 

**8.** (External KMS only) In the **Add a New Certificate Authority** section, enter a name for the CA, click the **Upload CA Certificate** button, and select the certificate for the CA used to sign your node certificates (see step 6c). Repeat this step for all CAs that were used in the signing process. 

AOS Security | Security Management Using Prism Element | **145** 

**9.** (External KMS only) Go to the **Key Management Server** section (see step 7) and follow these steps: 

   - a. Click the **Manage Certificates** button for a key management server. 

   - b. In the **Manage Signed Certificates** screen, upload the node certificates either by clicking the **Upload Files** button to upload all the certificates in one step or by clicking the **Upload** link (not shown in the figure) for each node individually. 

   - c. Test that the certificates are correct either by clicking the **Test all nodes** button to test the certificates for all nodes in one step or by clicking the **Test CS** (or **Re-Test CS** ) link for each node individually. A status of Verified indicates the test was successful for that node. 

   - d. Repeat this step for each key management server. 

**Note:** Before removing a drive or node from an SED cluster, ensure that the testing is successful and the status is **Verified** . Otherwise, the drive or node will be locked. 

**Figure 19: Upload Signed Certificates Screen** 

**10.** Click the **Enable Encryption** button. 

Enable Encryption window is displayed. 

**Figure 20: Data-at-Rest Encryption Screen (unprotected)** 

**Caution:** To help ensure that your data is secure, you cannot disable software-only data-at-rest encryption once it is enabled. Nutanix recommends regularly backing up your data, encryption keys, and key management server. 

## **11.** Enter **ENCRYPT** . 

AOS Security | Security Management Using Prism Element | **146** 

## **12.** Click the **Encrypt** button. 

The data-at-rest encryption is enabled. To view the status of the encrypted cluster or container, go to **Data at Rest Encryption** in the **Settings** menu. 

When you enable encryption, a low priority background task runs to encrypt all the unencrypted data. This task is designed to take advantage of any available CPU space to encrypt the unencrypted data within a reasonable time. If the system is occupied with other workloads, the background task consumes less CPU space. Depending on the amount of data in the cluster, the background task can take 24 to 36 hours to complete. 

If changes are made to the configuration after protection has been enabled, such as adding a new key management server, you must do the rekey operation for the modification to take full effect. In case of EKM, rekey to change the KEKs stored in the EKM. In case of LKM, rekey to change the master key used by native key manager, see Changing Key Encryption Keys (Software Only) on page 150 for details. 

Once the task to encrypt a cluster begins, you cannot cancel the operation. Even if you stop and restart the cluster, the system resumes the operation. 

## **Figure 21: Data-at-Rest Encryption Screen (protected)** 

## **Key Management Server (KMS) Considerations** 

Before configuring software data-at-rest encryption, ensure that you review the following KMS considerations and requirements: 

- Nutanix provides the option to choose the KMS type as the Native KMS (local), Native KMS (remote), External KMS, and Cloud KMS . 

- Cluster Localised Key Management Service (Native KMS (local)) requires a minimum of 3-node cluster. 1-node and 2-node clusters are not supported. 

AOS Security | Security Management Using Prism Element | **147** 

- Software encryption using Native KMS is supported for remote office/branch office (ROBO) deployments using the Native KMS (remote) KMS type. 

- You cannot enable or switch to Native KMS (remote) on any Nutanix cluster that is configured as a backup target for Prism Central Backup and Restore (PCBR). 

- For external KMS, a separate key management server is required to store the keys outside of the cluster. Each key management server device must be configured and addressable through the network. It is recommended that multiple key manager server devices be configured to work in clustered mode so they can be added to the cluster configuration as a single entity that is resilient to a single failure. 

**Caution:** Do not host a key management server VM on the encrypted cluster that is using it. Doing so could result in complete data loss if there is a problem with the VM while it is hosted in that cluster. 

You must install the license of the external key manager for all nodes in the cluster. See Compatibility and Interoperability Matrix for a complete list of the supported key management servers. For instructions on how to configure a key management server, refer to the documentation from the appropriate vendor. 

## **Switching between Native Key Manager and External Key Manager** 

After software encryption has been established, Nutanix supports the ability to switch the KMS type from the external key manager to the native key manager, from the native key manager to an external key manager, or from the remote KMS to the native KMS or an external KMS, without any down time. 

For external KMS, a separate key management server is required to store the keys outside of the cluster. Each key management server device must be configured and addressable through the network. It is recommended that multiple key manager server devices be configured to work in clustered mode so they can be added to the cluster configuration as a single entity that is resilient to a single failure. 

The Native KMS operates as an integrated solution and requires a minimum three-node cluster. 

Nutanix recommends that you backup and save the encryption keys with identifiable names before and after changing the KMS type. For more information, see Backing up Keys on page 152. 

To migrate from one external KMS to another, see Changing External KMS on page 149. 

To change the KMS type, change the KMS selection by editing the encryption configuration. For details, see step 5 on page 144 in Configuring Data-at-Rest Encryption (Software Only) on page 141 section. 

## **Figure 22: Select KMS type** 

AOS Security | Security Management Using Prism Element | **148** 

**Note:** This operation completes in a few minutes, depending on the number of encrypted objects and network speed. 

## _**Changing External KMS**_ 

Recommended workflow to migrate from one external key management server (KMS) to another external KMS. 

## **About this task** 

To migrate from one external KMS to another external KMS for software data-at-rest encryption, follow these steps: 

## **Procedure** 

**1.** Take a backup of the encryption keys. 

   - For more information, see Backing up Keys on page 152. 

**2.** Switch from the external KMS to either native KMS (local) or native KMS (remote), depending on your environment. 

   - For more information, see Switching between Native Key Manager and External Key Manager on page 148. 

**3.** Remove the existing external KMS configuration and configure the new external KMS. 

   - Ensure that you upload the required certificates for the new external KMS. For more information on updating the software data-at-rest encryption settings, see Configuring Data-at-Rest Encryption (Software Only) on page 141. 

**4.** Switch back from the native KMS (local or remote) used in Step 2 on page 149 to the new external KMS. 

## **Switching Between Local and Remote Native Key Management Servers** 

Switch the KMS type between a local native KMS and a remote native KMS for software data-at-rest encryption. 

## **Before you begin** 

Before switching the KMS, ensure that you meet the following prerequisites: 

- To switch to a local native KMS, the cluster must have a minimum of three nodes. 

- To switch to a remote native KMS, the cluster must be registered to Prism Central. 

- To switch to a remote native KMS, the cluster must not be configured as a backup target for Prism Central Backup and Restore. 

For more information on KMS requirements, see Key Management Server (KMS) Considerations on page 147. 

Nutanix recommends that you back up and save the encryption keys before and after changing the KMS type. For more information, see Backing up Keys on page 152. 

## **About this task** 

To switch between a local native KMS and a remote native KMS after you establish software encryption, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

AOS Security | Security Management Using Prism Element | **149** 

**2.** In the main menu, click the **Settings** icon. 

**3.** On the **Settings** page, select **Data-at-rest Encryption** . 

**4.** Click **Edit Configuration** . 

**5.** In the **Select Key Management Server (KMS)** section, select **Native KMS (local)** or **Native KMS (remote)** . 

**6.** Click **Save KMS type** . 

## **Changing Key Encryption Keys (Software Only)** 

The key encryption key (KEK) can be changed at any time. This can be useful as a periodic password rotation security precaution or when a key management server or node becomes compromised. If the key management server is compromised, only the KEK needs to be changed, because the KEK is independent of the drive encryption key (DEK). There is no need to re-encrypt any data, just to re-encrypt the DEK. 

## **About this task** 

To change the KEKs for a cluster, do the following: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the main menu and then select **Data at Rest Encryption** in the **Settings** page. 

**3.** In the **Cluster Encryption** page, select **Manage Keys** and click the **Rekey** button under **Software Encryption** . 

**Note:** The **Rekey** button appears only when cluster protection is active. If the cluster is already protected and a new key management server is added, you must press the **Rekey** button to use this new key management server for storing secrets. 

## **Figure 23: Cluster Encryption Screen** 

For Native KMS, a master encryption key (MEK) is used to encrypt the key encryption key (KEK). The system automatically regenerates the MEK yearly. 

AOS Security | Security Management Using Prism Element | **150** 

## **Destroying Data (Software Only)** 

Data on the AOS cluster is always encrypted, and the data encryption key (DEK) used to read the encrypted data is known only to the AOS. All data on the drive can effectively be destroyed (that is, become permanently unreadable) by deleting the container or cluster. This is known as a crypto-erase. 

## **About this task** 

**Note:** To help ensure that your data is secure, you cannot disable software-only data-at-rest encryption once it is enabled. Nutanix recommends regularly backing up your data, encryption keys, and key management server. 

To crypto-erase the container or cluster, do the following: 

## **Procedure** 

**1.** Delete the storage container or destroy the cluster. 

   - For information on how to delete a storage container, see Modifying a Storage Container in the _Prism Element Web Console Guide_ . 

   - For information on how to destroy a cluster, see Destroying a Cluster in the _Acropolis Advanced Administration Guide_ . 

When you delete a storage container, the Curator scans and deletes the DEK and KEK keys automatically. 

When you destroy a cluster, then: 

   - Native Key Manager (local) destroys the master key shares and the encrypted DEKs/KEKs. 

   - Native Key Manager (remote) retains the root key on the Prism Central if the cluster is still registered to a Prism Central when it is destroyed. You must unregister a cluster from the Prism Central and then destroy the cluster to delete the root key. 

   - External Key Manager deletes the encrypted DEKs. However, the KEKs remain on the EKM. You must use an external key manager UI to delete the KEKs. 

**2.** Delete the key backup files, if any. 

## **Switching from SED-EKM to Software-LKM** 

This section describes the steps to switch from SED and External KMS combination to software-only and LKM combination. 

## **About this task** 

To switch from SED-EKM to Software-LKM, do the following. 

## **Procedure** 

**1.** Perform the steps for the software-only encryption with External KMS. For more information, see Configuring Data-at-Rest Encryption (Software Only) on page 141. 

   - After the background task completes, all the data gets encrypted by the software. The time taken to complete the task depends on the amount of data and foreground I/O operations in the cluster. 

**2.** Disable the SED encryption. Ensure that all the disks are unprotected. 

   - For more information, see Enabling/Disabling Encryption (SEDs) on page 138. 

**3.** Switch the key management server from the External KMS to Local Key Manager. For more information, see Switching between Native Key Manager and External Key Manager on page 148. 

AOS Security | Security Management Using Prism Element | **151** 

## **Configuring Dual Encryption** 

## **About this task** 

Dual Encryption protects the data on the clusters using both SED and software-only encryption. An external key manager is used to store the keys for dual encryption, the Native KMS is not supported. 

To configure dual encryption, do the following: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the main menu and then select **Data at Rest Encryption** in the **Settings** page. 

**3.** In the Cluster Encryption page, check to enable both **Drive-based** and **Software-based** encryption. 

**4.** Click **Save Encryption Type** . 

**5.** Continue with the rest of the encryption configuration, see: 

   - Configuring Data-at-Rest Encryption (Software Only) on page 141 

   - Configuring Data-at-Rest Encryption (SEDs) on page 135 

## **Backing up Keys** 

## **About this task** 

You can take a backup of encryption keys: 

- When you enable Software-only Encryption for the first time 

- After you regenerate the keys 

Backing up encryption keys is critical in the very unlikely situation in which keys get corrupted. 

You can download key backup file for a cluster on a Prism Element or all clusters on a Prism Central. To download key backup file for all clusters, see Taking a Consolidated Backup of Keys (Prism Central) . 

To download the key backup file for a cluster, do the following: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the main menu and then select **Data at Rest Encryption** in the **Settings** page. 

**3.** In the Cluster Encryption page, select **Manage Keys** . 

**4.** Enter and confirm the password. 

**5.** Click the **Download Key Backup** button. 

The backup file is saved in the default download location on your local machine. Ensure you move the backup key file to a safe location. 

If vTPM is enabled, the downloaded key backup includes vTPM keys, in addition to the encryption keys. 

## **Taking a Consolidated Backup of Keys (Prism Central)** 

If you are using the Native KMS option with software encryption for your clusters, you can take a consolidated backup of all the keys from Prism Central. 

AOS Security | Security Management Using Prism Element | **152** 

## **About this task** 

To take a consolidated backup of keys for software encryption-enabled clusters (Native KMS-only), do the following: 

## **Procedure** 

**1.** Log in to Prism Central as an administrator. 

**2.** Click the hamburger icon, then select **Clusters** > **List** view. 

**3.** Select a cluster, go to **Actions** , then select **Manage & Backup Keys** . 

**4.** Download the backup keys: 

   - a. In **Password** , enter your password. 

   - b. In **Confirm Password** , reenter your password. 

   - c. To change the encryption key, select the **Rekey Encryption Key (KEK)** box . 

   - d. To download the backup key, click **Backup Key** . 

Ensure that you move the backup key file to a safe location. 

If vTPM is enabled, the downloaded key backup includes vTPM keys, in addition to the encryption keys. 

## **Importing Keys** 

You can import the encryption keys from backup. You must note the specific commands in this topic if you backed up your keys to an external key manager (EKM). 

## **About this task** 

**Note:** Nutanix recommends that you contact Nutanix Support for this operation. Extended cluster downtime might result if you perform this task incorrectly. 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a nutanix user: 

> `$ ssh nutanix@` ~~ee~~ _`cvm_ip_address`_ 

**2.** Retrieve the encryption keys stored on the cluster and verify that all the keys you want to retrieve are listed. In this example, the password is Nutanix.123. _`date`_ — is the timestamp portion of the backup file name. 

`nutanix@cvm$ mantle_recovery_util --backup_file_path=/home/nutanix/ encryption_key_backup_` _`date`_ `\ --password=Nutanix.123 --list_key_ids=true` 

**3.** Import the keys into the cluster. 

`nutanix@cvm$ mantle_recovery_util --backup_file_path=/home/nutanix/key_backup \ --password=Nutanix.123 --interactive_mode` 

**4.** If you are using an external key manager such as IBM Security Key Lifecycle Manager, Gemalto Safenet, or Vormetric Data Security Manager, use the --store_kek_remotely option to import the keys into the cluster. In this example, — _`date`_ is the timestamp portion of the backup file name. 

`nutanix@cvm$ mantle_recovery_util --backup_file_path` _`path`_ `/encryption_key_backup_` _`date`_ `\` 

AOS Security | Security Management Using Prism Element | **153** 

`--password` _`key_password`_ `--store_kek_remotely` 

**Tip:** The imported key backup includes vTPM keys (if vTPM is enabled during backup), in addition to the encryption keys. 

## **Data-at-Rest Encryption with Linux Unified Key Setup** 

Describes how Nutanix uses Linux Unified Key Setup (LUKS) to encrypt configuration data at rest for AOS and AHV environments. 

Nutanix provides two types of data-at-rest encryption, each addressing a different aspect of data protection. 

- AOS data-at-rest encryption. For more information, see Data-at-Rest Encryption (Software Only) on page 140. 

- Linux Unified Key Setup (LUKS)-based data-at-rest encryption. 

Nutanix uses LUKS to provide data-at-rest encryption for configuration data associated with AOS and AHV. This encryption protects data stored on the underlying operating system. The configuration data includes Nutanix cluster configuration information and VM configuration metadata. It does not include customer workload data stored inside VM disks. AOS data-at-rest encryption protects workload data stored in AOS. LUKS-based encryption is additive and extends encryption coverage to Nutanix configuration data. LUKS does not replace AOS data-at-rest encryption. 

You can use LUKS encryption in deployments that require protection of configuration data at rest, such as environments subject to compliance standards. 

Nutanix recommends using AOS data-at-rest encryption for workload data and using LUKS encryption only in strict environments where configuration data must be encrypted. 

You can enable LUKS during cluster provisioning or while imaging a node by using Nutanix Foundation. For more information, see the Field Installation Guide . 

## **LUKS Encryption Requirements and Considerations** 

Review the following requirements and considerations before enabling LUKS encryption. 

- LUKS encryption is supported for AHV only. 

- LUKS encryption is supported only on nodes with physical TPM enabled. 

- LUKS encryption is supported for the following software versions: 

   - AOS 7.3 or later 

   - AHV 10.3 or later 

   - Foundation 5.9 or later 

- Enabling LUKS-based encryption increases CPU overhead and can reduce workload throughput. Nutanix has observed approximately 20 to 30 percent additional CPU usage in some environments, depending on workload characteristics and I/O patterns. 

## **Internationalization (i18n)** 

The following table lists all the supported and unsupported entities in UTF-8 encoding. 

AOS Security | Security Management Using Prism Element | **154** 

## **Table 11: Internationalization Support** 

|**Supported Entities**|**Unsupported Entities**|
|---|---|
|Cluster name|Acropolis file server|
|Storage Container name|Share path|
|Storage pool|Internationalized domain names|
|VM name|E-mail IDs|
|Snapshot name|Hostnames|
|Volume group name|Integers|
|Protection domain name|Password fields|
|Remote site name|Any Hardware related names ( for example,|
||vSwitch, iSCSCI initiator, vLAN name)|
|User management||
|Chart name||



**Caution:** The creation of none of the above entities are supported on Hyper-V because of the DR limitations. 

## **Entities Support (ASCII or non-ASCII) for the Active Directory Server** 

- In the New Directory Configuration, **Name** field is supported in non-ASCII. 

- In the New Directory Configuration, **Domain** field is not supported in non-ASCII. 

- In Role mapping, **Values** field is supported in non-ASCII. 

- User names and group names are supported in non-ASCII. 

## **Log Forwarding** 

Configure cluster-wide log forwarding and fingerprinting to ensure log integrity and meet compliance requirements. 

The Controller VM (CVM) supports a cluster-wide log forwarding configuration to ensure log integrity by sending all logs to a central log host. Because of the CVM’s appliance-based design, system and audit logs do not support local retention periods. Retaining logs locally increases the risk of a distributed denial-of-service (DDoS) attack through excessive log traffic. 

Nutanix recommends deploying a central log host within the management enclave to meet compliance and internal policy requirements. This setup supports compliance with internal policies and regulatory requirements. In the event of a system compromise, the central log host helps preserve log integrity. 

By default, the CVM uses the audisp plugin to forward audit logs to the rsyslog daemon, which stores in the/home/ log/messages path. You can search for audispd on the central log host to retrieve the full audit log content. The audit daemon uses a rules engine that complies with the Operating System Security Requirements Guide (OS SRG) and is embedded in the CVM STIG. 

Use the `rsyslog-config` command to enable log forwarding for system, audit, AIDE, and SCMA logs across all CVMs in a cluster at the required log level. For more information, see Send Logs to Remote Syslog Server in the _Acropolis Advanced Administration Guide_ . 

AOS Security | Security Management Using Prism Element | **155** 

## **Documenting the Log Fingerprint** 

Document log fingerprints to support forensic analysis and ensure non-repudiation. 

## **About this task** 

To document the log fingerprint, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: `$ ssh nutanix@` ~~ee~~ _`cvm_ip_address`_ 

**2.** Document the fingerprint for each public key assigned to an individual admin: 

> `nutanix@cvm$ ssh-keygen -lf` ~~SCS~~ _`/file-path/id_rsa.pub`_ The fingerprint compares to the SSH daemon log entries and forwards to the central log host path /home/log/ secure in the CVM. 

**Note:** After completion of the SSH public key inclusion in Prism and verification of connectivity, disable the password authentication for all the CVMs and AHV hosts. For more information, see Configuring Cluster Lockdown in Prism Element on page 128. 

## **Admin Account Password Retry Lockout** 

For enhanced security, Prism Central and Prism Element locks out the default admin account for a period of 15 minutes after three unsuccessful login attempts. Once the account is locked out, the following message is displayed at the login screen. 

~~ee~~ `Account locked due to too many failed attempts` You can attempt entering the password after the 15 minutes lockout period, or contact Nutanix Support in case you have forgotten your password. 

**Note:** You cannot modify the default 15 minutes lock out period. 

## **Portal Proxy Connection in Prism Element** 

The portal proxy connection enables secure communication between Prism Element and Nutanix Support Portal using API keys. 

The portal proxy connection provides the following capabilities: 

- Direct support case creation from the Prism Element web console 

- Automatic updates to the cluster’s license status, which eliminates the manual downloading and uploading of the licensing files 

## **Portal Proxy Connection Considerations** 

Before you configure the portal proxy connection, consider the following points: 

- You can configure only one API key in the portal proxy connection. 

- When you disable the portal proxy connection, the associated API key becomes invalid. You cannot reuse the same API key for a new portal proxy connection. 

## **Configuring the Portal Proxy Connection in Prism Element** 

Configure the portal proxy connection in Prism Element. 

AOS Security | Security Management Using Prism Element | **156** 

## **About this task** 

To configure the portal proxy connection in Prism Element web console, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select the **Portal Connection** . 

**4.** On the **Portal Connection** page, enter the following information: 

   - a. **API Key** : Enter the API key that you generated from the My Nutanix portal. 

   - b. (Optional) **Public Key** : To upload the optional SSL key that you downloaded from the My Nutanix portal, click **Choose File** . 

**5.** Click **Save** . 

## **Updating the Portal Proxy Connection in Prism Element** 

Update the portal proxy connection in Prism Element. 

## **About this task** 

To update the portal proxy connection in Prism Element, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select the **Portal Connection** . 

**4.** Click **Update** . 

**5.** Update the fields in the **Portal Connection** window as needed and click **Save** . 

For more information on the fields available in the **Portal Connection** window, see Configuring the Portal Proxy Connection in Prism Element on page 156. 

## **Disabling the Portal Proxy Connection in Prism Element** 

Disable the portal proxy connection in Prism Element web console. 

## **Before you begin** 

You cannot reuse the disabled API key for a new portal proxy connection. 

## **About this task** 

To disable the portal proxy connection in Prism Element web console, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select the **Portal Connection** . 

AOS Security | Security Management Using Prism Element | **157** 

**4.** Click **Disable** . 

**5.** Click **Yes** . 

AOS Security | Security Management Using Prism Element | **158** 

## **HARDENING INSTRUCTIONS USING NCLI** 

Implement security hardening features for Nutanix AHV, Controller VM, and Prism Central VM. 

You can use nCLI to apply security hardening settings for AHV hosts, Controller VMs, and Prism Central VMs. The hardening parameters cover enabling advanced intrusion detection environment (AIDE) for integrity monitoring, enforcing high-strength passwords, configuring SSH login banners, and controlling core dumps. You can also set SSH security levels, apply IP-based restrictions, enforce lock status, and configure IP set–based firewalls for intracluster communication. 

## **AHV Security Hardening** 

AHV security hardening configurations allow you to mitigate vulnerabilities and strengthen the overall security posture of your cluster. 

**Note:** For the complete list of cluster security parameters, see _Edit the hypervisor security compliance config of a Cluster_ in the Command Reference guide. 

To configure AHV security hardening by modifying the key parameters, follow these steps: 

- SSH into any of the Controller VMs (CVMs) as a nutanix user: 

~~SSS~~ `$ ssh nutanix@cvm_ip_address` 

Replace cvm_ip_address with the IP address of the CVM. 

- View the hypervisor security configuration of the cluster: 

~~SCS~~ `nutanix@cvm$ ncli cluster get-hypervisor-security-config` Output: 

`Enable Aide               : false Enable Core               : false Enable High Strength P... : false Enable Banner             : false Schedule                  : DAILY Enable iTLB Multihit M... : false Enable Retbleed Mitiga... : false Enable Memory Poison      : false Enable user core dump ... : true Enable Fapolicy           : false Enable Kernel Core        : true` 

- **AIDE** : The Advanced Intrusion Detection Environment (AIDE) setting is a file and directory integrity checker. To enable AIDE, run the following command: 

~~SSCS~~ `nutanix@cvm$ ncli cluster edit-hypervisor-security-params enable-aide=true` 

- **Core** : The core dump setting consists of the recorded state of the working memory of a program at a specific time, generally when the program crashes or terminates abnormally. You can use core dumps to assist in diagnosing or debugging errors. You must always disable core dumps to prevent AHV from generating stack traces that might contain sensitive data. 

**Caution:** Nutanix recommends that you do not set the core setting to true unless instructed by the Nutanix support team to ensure data privacy. 

- **High Strength Password** : The high strength password setting enforces stronger password policies on the AHV host. It requires users to have a minimum password length of 15 characters (minlen=15), mandates that at least 8 

AOS Security | Hardening Instructions Using nCLI | **159** 

characters must differ from the old password (difok=8), limits the use of the same character class consecutively to a maximum of 4 characters (maxclassrepeat=4), checks passwords against a user-specific list of prohibited passwords (dictcheck=1), and reduces the maximum number of failed login attempts before account lockout from 5 to 3. To enable the high strength password setting, run the following command: 

`nutanix@cvm$ ncli cluster edit-hypervisor-security-params \ enable-high-strength-password=true` 

- **Banner** : The banner setting allows you to display a banner message upon SSH login. To enable the banner setting, run the following commands on each AHV host in a cluster: 

   - SSH into the AHV host as a root user: 

~~CSCS~~ `$ ssh root@ahv_ip_address` 

- Modify the DoD banner file on the AHV host: 

~~ee~~ `[root@AHV-host ~]# vi /etc/issue` 

   - SSH into any of the Controller VMs (CVMs) as a a `nutanix` user: ~~OO~~ `$ ssh nutanix@cvm_ip_address` 

   - Enable the banner setting: 

      - ~~CSS~~ `nutanix@cvm$ ncli cluster edit-hypervisor-security-params enable-banner=true` 

- **iTLB Multihit Mitigation** : The iTLB multihit mitigation setting mitigates CVE-2018-12207 (iTLB Multihit), a vulnerability in Intel CPUs that allows a user virtual machine (UVM) to perform a denial-of-service (DoS) attack on other co-hosted UVMs, potentially causing a host crash due to a Machine Check Exception (MCE). The iTLB multihit mitigation setting protects UVM against the DoS attacks but might reduce performance. To enable the iTLB multihit mitigation, run the following command: 

`nutanix@cvm$ ncli cluster edit-hypervisor-security-params enable-itlb-multihitmitigation=True` 

- **Retbleed Mitigation** : The Retbleed mitigation setting addresses the Retbleed vulnerability (CVE-2022-29900, CVE-2022-29901). The Retbleed is a speculative execution vulnerability affecting processors using return instructions. Enabling the Retbleed mitigation protects against these attacks by restricting the use of indirect branches and returns, which might reduce performance depending on the workload. To enable the Retbleed mitigation, run the following command: 

`nutanix@cvm$ ncli cluster edit-hypervisor-security-params enable-retbleedmitigation=True` 

- **Memory Poison** : The memory poison technique overwrites freed memory with a specific value (often nonzero) to help detect use-after-free vulnerabilities. When a program tries to access freed and poisoned memory, it might crash, exposing the vulnerability. Without memory poisoning, the program might continue operating with corrupted data, leading to unpredictable behavior and potential security exploits. To enable the memory poison, run the following command: 

~~CSS~~ `nutanix@cvm$ ncli cluster edit-hypervisor-security-params enable-memory-poison=True` 

- **Fapolicy** : Fapolicy is a RHEL 8 security framework for managing access control. 

**Note:** Nutanix recommends that you do not set the Fapolicy setting to true unless there is a strict organisation policy to enable it. Enabling the FaPolicy setting might reduce performance. 

AOS Security | Hardening Instructions Using nCLI | **160** 

- **Kernel Core** : The kernel core dump is a snapshot of the program’s memory at the time of a crash. Enabling the kernel core setting allows capturing a core dump of the kernel if it crashes, which can be helpful in debugging kernel-related issues. To enable kernel core, run the following command: 

~~SSS~~ `nutanix@cvm$ ncli cluster edit-hypervisor-security-params enable-kernel-core=true` 

**Note:** Starting with the AOS 7.5 and AHV 11.0 releases, Nutanix upgrades the AHV hypervisor operating system to Red Hat Enterprise Linux (RHEL) 9. Therefore, AHV 11.0 does not comply with the RHEL 9 Security Technical Implementation Guide (STIG) at this time. The CVM in AOS 7.5 remains based on RHEL 8 and complies with the RHEL 8 STIG. If your environment requires strict STIG compliance, Nutanix recommends using AOS 7.3 with AHV 10.3 because both components are based on RHEL 8 and comply with the RHEL 8 STIG. 

## **CVM Security Hardening** 

The CVM security hardening configurations allow you to mitigate vulnerabilities and strengthen the overall security posture of your cluster. 

**Note:** For the complete list of cluster security parameters, see _Edit the security params of a Cluster_ in the Command Reference guide. 

To configure the CVM security hardening by modifying the key parameters, follow these steps: 

- SSH into any of the Controller VMs (CVMs) as a a `nutanix` user: 

~~CSS~~ `$ ssh nutanix@cvm_ip_address` 

Replace cvm_ip_address with the IP address of the CVM. 

- View the cluster-wide security configuration: 

~~SSS~~ `nutanix@cvm$ ncli cluster get-cvm-security-config` 

Output: 

`Enable Aide               : false Enable Core               : false Enable High Strength P... : false Enable Banner             : false Schedule                  : DAILY Enable Kernel Core        : true Enable Page Poison        : false Enable Slub Debug         : false SSH Security Level        : DEFAULT Enable Lock Status        : false IP Restriction State      : NORMAL Enable DoDin Additiona... : false Enable Fapolicy           : false Enable Processor Mitig... : false SSH whitelisted addres... :` 

- **AIDE** : The Advanced Intrusion Detection Environment (AIDE) setting is a file and directory integrity checker. To enable AIDE, run the following command: 

~~CSCS~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-aide=true` 

- **Core** : The core dump setting consists of the recorded state of the working memory of a program at a specific time, generally when the program is crashed or terminated abnormally. You can use core dumps to assist in diagnosing 

AOS Security | Hardening Instructions Using nCLI | **161** 

or debugging errors. You must always disable core dumps to prevent the CVM from generating stack traces that might contain sensitive data. 

**Caution:** Nutanix recommends that you do not set the core setting to true unless instructed by the Nutanix support team to ensure data privacy. 

- **High Strength Password** : The high strength password setting enforces stronger password policies on the CVM. It requires users to have a minimum password length of 15 characters (minlen=15), mandates that at least 8 characters must differ from the old password (difok=8), limits the use of the same character class consecutively to a maximum of 4 characters (maxclassrepeat=4), checks passwords against a user-specific list of prohibited passwords (dictcheck=1), and reduces the maximum number of failed login attempts before account lockout from 5 to 3. To enable the high strength password setting, run the following command: 

~~SSC~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-high-strength-password=true` 

- **Banner** : The banner setting allows you to display a banner message upon SSH login. To enable the banner setting, run the following commands on each CVM in a cluster: 

   - Backup the DoD banner file: 

`nutanix@cvm$ sudo cp -a /srv/salt/security/CVM/sshd/DODbanner \ /srv/salt/security/CVM/sshd/DODbannerbak` 

- Modify the DoD banner file: 

   - ~~ee~~ `nutanix@cvm$ sudo vi /srv/salt/security/CVM/sshd/DODbanner` 

- Enable the banner setting: 

~~CSS~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-banner=true` 

- **Kernel Core** : The kernel core dump is a snapshot of the program’s memory at the time of a crash. Enabling the kernel core setting allows capturing a core dump of the kernel if it crashes, which can be helpful in debugging kernel-related issues. To enable kernel core, run the following command: 

~~SSC~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-kernel-core=true` 

- **Page Poison** : The page poison setting is a technique that overwrites freed memory with a specific value to help detect use-after-free vulnerabilities. Enabling page poison mitigation helps prevent use-after-free attacks but might reduce performance. To enable page poison, run the following command: 

~~SSCS~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-page-poison=true` 

- **Slub Debug** : The slub debug setting is a memory debugging technique that can help detect memory corruption issues. Enabling slub debug mitigation helps prevent memory corruption but might reduce performance. To enable slub debug, run the following command: 

~~ee~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-slub-debug=true` 

AOS Security | Hardening Instructions Using nCLI | **162** 

- **SSH Security Level** : The SSH security level setting allows you to enable different security levels for the a `nutanix` user for accessing the cluster using SSH login. You can configure the security levels with one of the following options: 

   - **Default** : The `nutanix` a user can start an SSH session with either a password or an SSH key without any change to the account privileges. 

   - **Limited** : For SSH sessions started with a password, the a `nutanix` user is switched to an admin user that has lower operational privileges. The SSH key-based logins do not change the `nutanix` a user's privileges. 

   - **Restricted** : For all SSH sessions, whether started with a password or an SSH key, the a `nutanix` user is switched to an admin user that has lower operational privileges. 

To configure the SSH security level, run the following command: 

## ~~ee~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params ssh-security-level=default` 

The admin user, while having lower privileges than the `nutanix` a user, is designed to have sufficient privileges for administrative tasks. For more information, see Controller VM Access in the AHV Administration Guide. 

In addition to configuring the SSH security level, you can also consider cluster lockdown to disable passwordbased SSH authentication by adding SSH keys. For more information, see Configuring Cluster Lockdown in Prism Element on page 128. 

- **Lock Status** : The lock status setting freezes the security configuration and prevents from making any changes to the security configuration through nCLI or API calls. To enable the lock status, run the following command: 

~~SSCS~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-lock-status=true` 

**Note:** You must contact Nutanix Support to unlock the lock status setting. 

- **IP Restriction State** : The IP restriction state setting controls the different levels of IP restriction for the a `nutanix` user to access the cluster using SSH login. You can set it to normal or restricted depending on the desired level of restriction. When you set the IP restriction state to restricted, only the IPs in the SSH whitelisted addresses can access the system using SSH. To configure the IP Restriction State, run the following command: 

## ~~SSS~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params ip-restriction=normal` 

**Note:** You can use the IP restriction state setting when the SSH client IP addresses are known and stable. 

- **DoDin Additional** : The DoDin additional setting provides enhanced options for maintaining DISA compliance. It allows permanent account locking after an incorrect password is entered, as opposed to implementing a temporary 15-minute timeout period. Also, it makes AIDE send email messages to the account associated with NCC and runs AIDE more frequently. To enable DoDin, run the following command: 

~~SSC~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-dodin-opts=true` 

- **Fapolicy** : Fapolicy is a RHEL 8 security framework for managing access control. 

**Note:** Nutanix recommends that you do not set the Fapolicy setting to true unless there is a strict organisation policy to enable it. Enabling the FaPolicy setting might reduce performance. 

- **Processor Mitigation** : The processor mitigation setting enables mitigations against processor-based vulnerabilities. Enabling processor mitigation protects against certain types of attacks but might reduce performance. To enable processor mitigation, run the following command: 

~~SSCS~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params enable-processor-mitigations=true` 

AOS Security | Hardening Instructions Using nCLI | **163** 

- **SSH Whitelisted Address** : Displays the list of IP addresses added to the CVM SSH whitelist. To add an IP address to the SSH whitelisted addresses, run the following command: 

   - ~~S~~ `nutanix@cvm$ ncli cluster edit-cvm-security-params add-to-ssh-whitelist=` ~~SS~~ _`x.x.x.x`_ 

   - Replace a _`x.x.x.x`_ with the IP address. 

Use the SSH Whitelisted Address setting to add the IP addresses of trusted machines, such as jump boxes, that connect to the CVM using SSH. 

Avoid whitelisting broad IP ranges. Allow only the minimal set of required IP addresses, such as the smallest subnet or specific jump box IP addresses. 

If a whitelisted IP address is misconfigured or changes without being re-added to the whitelist, SSH access to the CVM is blocked. To restore access, disable the restriction using the local console or serial access. 

## **PCVM Security Hardening** 

PCVM security hardening configurations allow you to mitigate vulnerabilities and strengthen the overall security posture of your cluster. 

**Note:** For the complete list of cluster security parameters, see _Edit the security params of a Cluster_ in the Command Reference guide. 

To configure PCVM security hardening by modifying the key parameters, follow these steps: 

- SSH into any of the Prism Central VM (PCVM) as a a `nutanix` user: 

~~SSC~~ `$ ssh nutanix@pcvm_ip_address` 

Replace pcvm_ip_address with the IP address of the PCVM. 

- View the cluster-wide security configuration: 

~~ee~~ `nutanix@pcvm$ ncli cluster get-pcvm-security-config` 

Output: 

`Enable Aide               : false Enable Core               : false Enable High Strength P... : false Enable Banner             : false Schedule                  : DAILY Enable Kernel Core        : false Enable Page Poison        : false Enable Slub Debug         : false SSH Security Level        : DEFAULT Enable Lock Status        : false IP Restriction State      : NORMAL Enable DoDin Additiona... : false Enable Fapolicy           : false Enable Processor Mitig... : false SSH whitelisted addres... :` 

- **AIDE** : The Advanced Intrusion Detection Environment (AIDE) setting is a file and directory integrity checker. To enable AIDE, run the following command: 

~~SSS~~ `nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-aide=true` 

- **Core** : The core dump setting consists of the recorded state of the working memory of a program at a specific time, generally when the program is crashed or terminated abnormally. You can use core dumps to assist in diagnosing 

AOS Security | Hardening Instructions Using nCLI | **164** 

or debugging errors. You must always disable core dumps to prevent the PCVM from generating stack traces that might contain sensitive data. 

**Caution:** Nutanix recommends that you do not set the core setting to true unless instructed by the Nutanix support team to ensure data privacy. 

- **High Strength Password** : The high strength password setting enforces stronger password policies on the PCVM. It requires users to have a minimum password length of 15 characters (minlen=15), mandates that at least 8 characters must differ from the old password (difok=8), limits the use of the same character class consecutively to a maximum of 4 characters (maxclassrepeat=4), checks passwords against a user-specific list of prohibited passwords (dictcheck=1), and reduces the maximum number of failed login attempts before account lockout from 5 to 3. To enable the high strength password setting, run the following command: 

`nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-high-strengthpassword=true` 

- **Banner** : The banner setting allows you to display a banner message upon SSH login. To enable the banner setting, run the following commands on each PCVM in a cluster: 

   - Backup the DoD banner file: 

`nutanix@pcvm$ sudo cp -a /srv/salt/security/PC/sshd/DODbanner \ /srv/salt/security/PC/sshd/DODbannerbak` 

- Modify the DoD banner file: 

~~ee~~ `nutanix@pcvm$ sudo vi /srv/salt/security/PC/sshd/DODbanner` 

   - Enable the banner setting: ~~CSS~~ `nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-banner=true` 

- **Kernel Core** : The kernel core dump is a snapshot of the program’s memory at the time of a crash. Enabling the kernel core setting allows capturing a core dump of the kernel if it crashes, which can be helpful in debugging kernel-related issues. To enable kernel core, run the following command: 

~~CSS~~ `nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-kernel-core=true` 

- **Page Poison** : The page poison setting is a technique that overwrites freed memory with a specific value to help detect use-after-free vulnerabilities. Enabling page poison mitigation helps prevent use-after-free attacks but might reduce performance. To enable page poison, run the following command: 

~~SSCS~~ `nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-page-poison=true` 

- **Slub Debug** : The slub debug setting is a memory debugging technique that can help detect memory corruption issues. Enabling slub debug mitigation helps prevent memory corruption but might reduce performance. To enable slub debug, run the following command: 

~~ee~~ `nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-slub-debug=true` 

AOS Security | Hardening Instructions Using nCLI | **165** 

- **SSH Security Level** : The SSH security level setting allows you to enable different security levels for the a `nutanix` user for accessing the cluster using SSH login. You can configure the security levels with one of the following options: 

   - **Default** : The `nutanix` a user can start an SSH session with either a password or an SSH key without any change to the account privileges. 

   - **Limited** : For SSH sessions started with a password, the a `nutanix` user is switched to an admin user that has lower operational privileges. The SSH key-based logins do not change the `nutanix` a user's privileges. 

   - **Restricted** : For all SSH sessions, whether started with a password or an SSH key, the a `nutanix` user is switched to an admin user that has lower operational privileges. 

To configure the SSH security level, run the following command: 

## ~~ee~~ `nutanix@pcvm$ ncli cluster edit-pcvm-security-params ssh-security-level=DEFAULT` 

The admin user, while having lower privileges than the `nutanix` a user, is designed to have sufficient privileges for administrative tasks. For more information, see Controller VM Access in the AHV Administration Guide. 

In addition to configuring the SSH security level, you can also consider cluster lockdown to disable passwordbased SSH authentication by adding SSH keys. For more information, see Configuring Cluster Lockdown in Prism Element on page 128. 

- **Lock Status** : The lock status setting freezes the security configuration and prevents from making any changes to the security configuration through nCLI or API calls. To enable the lock status, run the following command: 

## ~~SSCS~~ `nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-lock-status=true` 

**Note:** You must contact Nutanix Support to unlock the lock status setting. 

- **DoDin Additional** : The DoDin additional setting provides enhanced options for maintaining DISA compliance. It allows permanent account locking after an incorrect password is entered, as opposed to implementing a temporary 15-minute timeout period. Also, it makes AIDE send email messages to the account associated with NCC and runs AIDE more frequently. To enable DoDin, run the following command: 

~~SSS~~ `nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-dodin-opts=true` 

- **Fapolicy** : Fapolicy is a RHEL 8 security framework for managing access control. 

**Note:** Nutanix recommends that you do not set the Fapolicy setting to true unless there is a strict organisation policy to enable it. Enabling the FaPolicy setting might reduce performance. 

- **Processor Mitigation** : The processor mitigation setting enables mitigations against processor-based vulnerabilities. Enabling processor mitigation protects against certain types of attacks but might reduce performance. To enable processor mitigation, run the following command: 

`nutanix@pcvm$ ncli cluster edit-pcvm-security-params enable-processormitigations=true` 

## **IP Set Based Firewall** 

The IP set-based firewall feature implements strict firewall rules for intra cluster communication. This feature enables individual IP address filtering for intra cluster communication for various internal services. Prism considers only individual Controller VM and hypervisor IP addresses as trusted sources to receive RPC and API request from other CVMs in the same cluster. 

The IP set-based firewall feature is enabled by default in AOS version 6.8. 

AOS Security | Hardening Instructions Using nCLI | **166** 

## **ELIMINATE THE DEFAULT PASSWORDS DURING A CLUSTER CREATION** 

Enhance cluster security during creation by eliminating default passwords for the `nutanix` user and CVMs. 

Hackers often exploit default passwords, which are a well-known security vulnerability. Removing default passwords and providing mechanisms for more secure SSH access configurations, strengthens the overall security posture of your Nutanix clusters. 

During cluster creation, you can configure the following security settings to eliminate the default passwords: 

- Applying `Password Lockdown Mode` to the cluster restricts password-based SSH access to `nutanix` and `admin` accounts. 

- Applying `Lockdown Mode` to the cluster restricts both password-based and public SSH key based authentication. 

- Adding public SSH keys from trusted systems further secures remote access, without relying on password-based authentication. 

The system maintains the new password and public SSH keys during the cluster management workflow, such as adding a node to a cluster, removing a node from the cluster, or performing a break-fix operation. 

**Note:** Elimination of default passwords supported only for `nutanix` user and CVM. 

## **Removing the Default SSH Password of a CVM** 

During a cluster creation, you can remove the default SSH password for the Controller VMs (CVMs) `nutanix` user for enhanced security. 

## **About this task** 

To remove the default SSH password of a CVM during the cluster creation, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` user: 

`$ ssh nutanix@` _`cvm_ip_address`_ 

**2.** Create a cluster and set the SSH access mode: 

`nutanix@cvm$ cluster --cluster_create_password_enforcement=true -- cluster_create_security_opts=true --encrypted_password=` _`"encrypted_string"`_ `create` 

Replace _`encrypted_string`_ with the encrypted password to be applied on the cluster. For more information, see Controller VM Password Complexity Requirements . 

## **Enabling SSH Access to a CVM Using a Public Key** 

During a cluster creation, you can enable SSH access to a Controller VM (CVM) using a public key and remove the default SSH password of a CVM for enhanced security. 

## **About this task** 

To enable SSH access to a CVM using a public key and remove the default SSH password of a CVM during the cluster creation, follow these steps: 

AOS Security | Eliminate the Default Passwords During a Cluster Creation | **167** 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: 

   - `$ ssh nutanix@` ~~SSS~~ _`cvm_ip_address`_ 

**2.** Create a cluster and set the SSH access mode: 

`nutanix@cvm$ cluster --password_lockdown_mode=true -- cluster_create_password_enforcement=true --cluster_create_security_opts=true -- encrypted_password=` _`"encrypted_string"`_ `--external_access_keys=` _`"public_key"`_ `create` 

> Replace ee _`encrypted_string`_ with the encrypted password to be applied on the cluster. For more information on password complexity requirements, see Controller VM Password Complexity Requirements . 

> Replace a _`public_key`_ with the public SSH key value of the trusted system to allow SSH access to the cluster. 

The cluster creation begins by securing the CVM with the following system configuration: 

   - Replaces the CVM's default password with the that password you entered. 

   - Restricts SSH access to the CVM using a password. 

   - Maintains the new password and public SSH key during cluster management workflows, such as adding a node, removing a node, or performing a breakfix operation. 

**3.** (Optional) To create a cluster with multiple public SSH key based access: 

`nutanix@cvm$ cluster --password_lockdown_mode=true -- cluster_create_password_enforcement=true --cluster_create_security_opts=true -- external_access_keys=` _`“public_key_1”`_ `,` _`"public_key_2”`_ `create` 

## **Enabling Cluster Lockdown Mode** 

During a cluster creation, you can enable cluster lockdown mode to disable both password-based and public SSH key-based authentication to a cluster. This method provides the highest level of security for the Controller VM, along with eliminating default password usage. 

## **About this task** 

To enable cluster lockdown mode during the cluster creation, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as a `nutanix` a user: `$ ssh nutanix@` ~~SCS~~ _`cvm_ip_address`_ 

**2.** Create a cluster with lockdown mode enabled: 

`nutanix@cvm$ cluster --lockdown_mode=true --cluster_create_password_enforcement=true --cluster_create_security_opts=true create` 

The cluster creation starts and the system restricts the password-based and SSH key based authentication. 

AOS Security | Eliminate the Default Passwords During a Cluster Creation | **168** 

## **FIREWALL BEST PRACTICES** 

Protect Nutanix components with a stateful firewall that allows traffic only from trusted management endpoints. 

The Ports and Protocols page provides detailed information about ports used by Nutanix products and services including transfer protocol, description, source, destination, and associated service. 

Nutanix recommends that the networks for the hypervisor, CVM, Prism Central, and other Nutanix components are protected behind a stateful firewall in the network environment. This firewall should allow traffic sources only from known management endpoints. 

AOS Security | Firewall Best Practices | **169** 

## **SECURING TRAFFIC THROUGH NETWORK SEGMENTATION** 

Network segmentation enhances security, resilience, and cluster performance by isolating a subset of traffic to its own network. 

You can achieve traffic isolation in one or more of the following ways: 

## Isolating Backplane Traffic by using VLANs (Logical Segmentation) 

You can separate management traffic from storage replication (or backplane) traffic by creating a separate network segment (LAN) for storage replication. For more information about the types of traffic seen on the management plane and the backplane, see Traffic Types In a Segmented Network on page 171. 

To enable the CVMs in a cluster to communicate over these separated networks, the CVMs are multihomed. Multihoming is facilitated by the addition of a virtual network interface card (vNIC) to the Controller VM and placing the new interface on the backplane network. Additionally, the hypervisor is assigned an interface on the backplane network. 

The traffic associated with the CVM interfaces and host interfaces on the backplane network can be secured further by placing those interfaces on a separate VLAN. 

In this type of segmentation, both network segments continue to use the same external bridge and therefore use the same set of physical uplinks. For more information, see Physically Isolating the Backplane Traffic on an AHV Cluster on page 184. 

Isolating backplane traffic from management traffic requires minimal configuration through the Prism Element web console. No manual host (hypervisor) configuration steps are required. 

For more information, see Isolating the Backplane Traffic Logically on an Existing Cluster on page 182. 

## Isolating Backplane Traffic Physically (Physical Segmentation) 

You can physically isolate the backplane traffic (intra-cluster traffic) from the management traffic (Prism Element, SSH, SNMP) in to a separate vNIC on the CVM and using a dedicated virtual network that has its own physical NICs. This type of segmentation offers true physical separation of backplane traffic from management traffic. 

You can use Prism Element to configure the vNIC on the CVM and configure backplane traffic to communicate over the dedicated virtual network. However, you must first manually configure the virtual network on the hosts and associate it with the physical NICs that is required for true traffic isolation. 

For more information, see Physically Isolating the Backplane Traffic on an AHV Cluster on page 184. 

Isolating service-specific traffic 

You can also secure traffic associated with a service (for example, Nutanix Volumes) by confining its traffic to a separate vNIC on the CVM and using a dedicated virtual network that has its own physical NICs. This type of segmentation offers true physical separation for service-specific traffic. 

You can use Prism Element to create the vNIC on the CVM and configure the service to communicate over the dedicated virtual network. However, you must first manually configure the virtual network on the hosts and associate it with the physical NICs that is required for true traffic isolation. You need one virtual network for each service that you plan to isolate. For a list of the services whose traffic you can isolate in the current release, see Cluster Services That Support Traffic Isolation on page 179. 

For more information, see Isolating Service-Specific Traffic on page 197. 

AOS Security | Securing Traffic Through Network Segmentation | **170** 

## Isolating Stargate-to-Stargate traffic over RDMA 

Some Nutanix platforms support remote direct memory access (RDMA) for Stargate-to-Stargate service communication. You can create a separate virtual network for RDMA-enabled NICs. If a node has RDMA-enabled NICs, Foundation passes the NICs through to the CVMs during imaging. The CVMs use only the first of the two RDMA-enabled NICs for Stargate-to-Stargate communications. The virtual NIC on the CVM is named `rdma0` . Foundation does not configure the RDMA LAN. After creating a cluster, you need to enable RDMA by creating an RDMA LAN from Prism Central. For more information, see Remote Direct Memory Access in the _NX Series Hardware Administration Guide_ . 

For more information, see Isolating the Backplane Traffic on an Existing RDMA Cluster on page 209. 

## **Traffic Types In a Segmented Network** 

Backplane and management traffic are two types of traffic entering and leaving a Nutanix cluster in a segmented network: 

## Backplane traffic 

Backplane traffic is intra-cluster traffic that is necessary for the cluster to function, and it comprises traffic between CVMs and traffic between CVMs and hosts for functions such as storage RF replication, host management, high availability, and so on. This traffic uses `eth2` on the CVM. In AHV, VM live migration traffic is also backplane, and uses the AHV backplane interface, VLAN, and virtual switch when configured. For nodes that have RDMA-enabled NICs, CVMs use a separate RDMA LAN for Stargate-to-Stargate communications. 

## Management traffic 

Management traffic refers to administrative traffic or traffic associated with Prism Element and SSH connections, remote logging, SNMP, and so on. The current implementation simplifies the definition of management traffic to include any traffic that is not on the backplane network and includes communications between user VMs and CVMs. This traffic uses `eth0` on the CVM. 

You can further isolate traffic on the management plane based on specific services or features. For example, the traffic that the cluster receives from an external iSCSI initiators (such as Nutanix Volumes iSCSI traffic) is categorized as management traffic. For a list of services supported in the current release, see Cluster Services That Support Traffic Isolation on page 179. 

## **Segmented and Unsegmented Networks** 

Configurations for segmented and unsegmented networks for Nutanix clusters (both ESXi and AHV). 

In a default unsegmented network configuration for Nutanix clusters (both ESXi and AHV), the Controller VM (CVM) has two virtual NICs: `eth0` and `eth1` . 

Interface `eth0` is connected to a default external virtual switch, which is in turn connected to the external network through a bond or NIC team that contains the host physical uplinks. 

Interface `eth1` is connected to an internal network that enables the CVM to communicate with the hypervisor. 

The figures _Unsegmented Network - ESXi Cluster_ , and _Unsegmented Network - AHV Cluster_ shows an unsegmented network, and all external CVM traffic, whether backplane or management traffic, uses interface `eth0` . These interfaces are on the default VLAN on the default virtual switch. 

AOS Security | Securing Traffic Through Network Segmentation | **171** 

## **Figure 24: Unsegmented Network- ESXi Cluster** 

The following figure _Unsegmented Network- AHV Cluster_ shows an unsegmented network on an AHV cluster. In AHV, VM live migration traffic is also backplane and uses the AHV backplane interface, VLAN, and virtual switch when configured. 

AOS Security | Securing Traffic Through Network Segmentation | **172** 

**Figure 25: Unsegmented Network- AHV Cluster** 

If you further isolate service-specific traffic, additional vNICs are created on the CVM. Each service requiring isolation is assigned a dedicated virtual NIC on the CVM. The NICs are named `ntnx0` , `ntnx1` , and so on. Each service-specific NIC is placed on a configurable existing or new virtual network (vSwitch or bridge) and a VLAN and IP subnet are specified. 

## **Network with Segmentation** 

In a segmented network, management traffic uses CVM interface `eth0` and additional services can be isolated to different VLANs or virtual switches. In backplane segmentation, the backplane traffic uses interface `eth2` . The backplane network uses either the default VLAN or a separate VLAN that you specify when segmenting the network. In ESXi, you must select a port group for the new vmkernel interface. In AHV, this internal interface is created automatically in the selected virtual switch. For physical separation of the backplane network, create this new port group on a separate virtual switch in ESXi or select a desired virtual switch in the AHV GUI. 

AOS Security | Securing Traffic Through Network Segmentation | **173** 

To isolate service-specific traffic such as Volumes or Disaster Recovery as well as backplane traffic, additional vNICs are needed on the CVM, but no new vmkernel adapters or internal interfaces are required. AOS creates additional vNICs on the CVM, then assigns each service that requires isolation a dedicated vNIC on the CVM. The NICs are named `ntnx0` , `ntnx1` , and so on. AOS places each service-specific NIC on a configurable existing or new virtual network (vSwitch or bridge) and specifies a VLAN and IP subnet. 

You can choose to perform backplane segmentation alone with no other forms of segmentation. You can also choose to use one or more types of service specific-segmentation with or without backplane segmentation. In all of these cases, you can choose to segment any service to either the existing virtual switch or a new virtual switch for further physical traffic isolation. The combination you select is driven by the security and networking requirements of your deployment. In most cases, Nutanix recommends the default configuration with no segmentation of any kind due to simplicity and ease of deployment. 

The following figure shows an implementation scenario where the backplane and service-specific segmentation are configured with two vSwitches on ESXi hypervisors. 

**Figure 26: Backplane and Service-specific Segmentation Configured with two vSwitches on an ESXi Cluster** 

The following are the CVM to ESXi hypervisor connection details: 

- The `eth0` vNIC on the CVM and `vmk0` on the host are carrying management traffic and connected to the hypervisor through the existing PGm (portgroup) on vSwitch0. 

- The `eth2` vNIC on the CVM and `vmk2` on the host are carrying backplane traffic and connected to the hypervisor through a new user created PGb on the existing vSwitch. 

AOS Security | Securing Traffic Through Network Segmentation | **174** 

- The `ntnx0` vNIC on the CVM is carrying iSCSI traffic and connected to the hypervisor through PGi on the vSwitch1. No new vmkernel adapter is required. 

- The `ntnx1` vNIC on the CVM is carrying DR traffic and connected to the hypervisor through `PGd` on the vSwitch2. No new vmkernel adapter is required. 

The following figure shows an implementation scenario where the backplane and service-specific segmentation are configured with two vSwitches on an AHV hypervisors. In the figure, the interface name `br0-bp` is read as `br0backplane` . 

**Figure 27: Backplane and Service Specific Segmentation Configured with two vSwitches on an AHV Cluster** 

The following are the CVM to AHV hypervisor connection details: 

- The `eth0` vNIC on the CVM is carrying management traffic and connected to the hypervisor through the existing `vnet0` . 

- Other vNICs such as `eth2` , `ntnx0` , and `ntnx1` are connected to the hypervisor through the auto created interfaces on either the existing or new vSwitch. 

The following table describes the vNIC, port group (PG), VM kernel (VMK), virtual network (VNET) and virtual switch connections for CVM and hypervisor in different implementation scenarios. The tables capture information for ESXi and AHV hypervisors: 

AOS Security | Securing Traffic Through Network Segmentation | **175** 

## **Table 12:** 

|**Implementation**|**vNIC on CVM**|**vNIC on CVM**|**Connected to ESXi**|**Connected to ESXi**|**Connected to AHV**|**Connected to AHV**|**Connected to AHV**|**Connected to AHV**|
|---|---|---|---|---|---|---|---|---|
|**Scenario**|||||||||
|Backplane segmentation|eth0:||vmk0via existing PGm||Existing<br>vnet0||||
|with one vSwitch|||on vSwitch||||||
||DR, iSCSI, and||||||||
||Management traffic||||||||
||eth2:||New<br>vmk2|via PGb on|Auto created interfaces||||
||Backplane traffic||vSwitch0||on bridge<br>br0||||
||||CVM vNIC via PGb on||||||
||||vSwitch0||||||
|Backplane segmentation<br>with two vSwitches|eth0:<br>Management traffic||vmk0via existing PGm<br>on vSwitch0||Existing<br>vnet0||||
||eth2:<br>Backplane traffic||New vmk2 via PGb on new<br>vSwitch||Auto created interfaces<br>on new virtual switch||||
||||CVM vNIC via PGb on||||||
||||new vSwitch||||||
|Service-specific<br>segmentation for<br>Volumes with one<br>vSwitch|eth0:<br>DR, Backplane, and<br>Management traffic||vmk0via existing PGm on<br>vSwitch0||Existing<br>vnet0||||
||ntnx0:<br>iSCSI (Volumes) traffic||CVM vNIC via PGi on<br>vSwitch0||Auto created interface on<br>existing<br>br0||||
|Service-specific<br>segmentation for<br>Volumes with two<br>vSwitches|eth0:<br>DR, Backplane, and<br>Management traffic||vmk0via existing PGm on<br>vSwitch0||Existing<br>vnet0||||
||ntnx0:<br>iSCSI (Volumes) traffic||CVM vNIC via PGi on new<br>vSwitch||Auto created interface on<br>new virtual switch||||
|Service-specific<br>segmentation for DR with<br>one vSwitch|eth0:<br>iSCSI, Backplane, and<br>Management traffic||vmk0via existing PGm on<br>vSwitch0||Existing<br>vnet0||||
||ntnx1:||CVM vNIC via PGd on||Auto created interface on||Auto created interface on||
||DR traffic||vSwitch0||existing<br>br0||||
|Service Specific<br>Segmentation for DR<br>with two vSwitches|eth0:<br>iSCSI, Backplane, and<br>Management traffic||vmk0via existing PGm on<br>vSwitch0||Existing<br>vnet0||||
||ntnx1:||CVM vNIC via PGd on||Auto created interface on||Auto created interface on||
||DR traffic||new vSwitch||new virtual switch||||



AOS Security | Securing Traffic Through Network Segmentation | **176** 

|**Implementation**|**vNIC on CVM**|**Connected to ESXi**|**Connected to ESXi**|**Connected to AHV**|**Connected to AHV**|
|---|---|---|---|---|---|
|**Scenario**||||||
|Backplane and Service-<br>specific segmentation<br>with one vSwitch|eth0:<br>Management traffic<br>~~|~~|vmk0via existing PGm on<br>vSwitch0<br>rl||Existing<br>vnet0<br>az||
||eth2:<br>m7|New<br>vmk2via PGb on<br>|||Auto created interfaces||
||Backplane traffic|vSwitch0||on<br>br0<br>||||
|||CVM vNIC via PGb on||||
|||vSwitch0||||
||ntnx0:<br>iSCSI traffic<br>Pe|CVM vNIC via PGi on<br>vSwitch0||Auto created interface on<br>br0<br>||||
||ntnx1:<br>Pe|CVM vNIC via PGd on||Auto created interface on||
||DR traffic|vSwitch0||br0<br>||||
|Backplane and Service-<br>specific segmentation<br>with two vSwitches|eth0:<br>Management traffic<br>m7|vmk0via existing PGm on<br>vSwitch0<br>rl||Existing<br>vnet0<br>a7||
||eth2:<br>Backplane traffic<br>maz|New<br>vmk2via PGb on new<br>vSwitch<br>|||Auto created interfaces<br>on new virtual switch||
|||CVM vNIC via PGb on||||
|||new vSwitch||||
||ntnx0:<br>iSCSI traffic<br>Pe|CVM vNIC via PGi on<br>vSwitch1||Auto created interface on<br>new virtual switch||
|||No new user defined||||
|||vmkernel adapter is||||
|||required.||||
||ntnx1:<br>DR traffic<br>Pe|CVM vNIC via PGd on<br>vSwitch2.||Auto created interface in<br>new virtual switch||
|||No new user defined||||
|||vmkernel adapter is||||
|||required.||||



## **Supported Environment** 

AOS and hypervisor compatibility information to support network segmentation. 

The following are the hypervisor compatibility for network segmentation: 

|**Segmentation Type**|**Supported Hypervisor**|
|---|---|
|Network segmentation by traffic type where you can|AHV, ESXi, Hyper-V|
|separate backplane traffic from management traffic||
|Service-specific traffic isolation|AHV, ESXi|



The following are the AOS requirements for network segmentation: 

AOS Security | Securing Traffic Through Network Segmentation | **177** 

|**Segmentation Type**|**AOS Version**|
|---|---|
|Logical network segmentation|5.5 or later|
|Physical network segmentation|5.11 or later|
|Service-specific traffic isolation|5.11 or later|



## **RDMA Requirements for Network Segmentation** 

The following is the RDMA requirement to support network segmentation: 

Network segmentation is supported with RDMA for AHV and ESXi hypervisors only. For more information about RDMA, see Remote Direct Memory Access in the _NX Series Hardware Administration Guide_ . 

## **Prerequisites** 

Ensure that the following prerequisites are met before you configure network segmentation: 

- Ensure that proxy ARP is disabled within the Nutanix VLAN before configuring network segmentation. 

- If the cluster is registered to Prism Central and currently uses a segmented Data Services IP (DSIP) located in a secondary subnet, you must configure an additional DSIP within the original subnet as Prism Central. This is required because: 

   - Prism Central cannot perform upgrade operations using a segmented DSIP. 

   - The segmented DSIP and the cluster DSIP are distinct entities. 

   - A DSIP in the same subnet ensures direct communication between Prism Central and the cluster during the upgrade process. 

Failure to configure a DSIP in the appropriate subnet will prevent Prism Central from initiating or completing cluster upgrades. 

## **Nutanix Volumes** 

Stargate does not monitor the health of a segmented network. If physical network segmentation is configured, network failures or connectivity issues are not tolerated. To overcome this issue, configure redundancy in the network. That is, use two or more uplinks in a fault tolerant configuration, connected to two separate physical switches. 

## **Disaster Recovery** 

Ensure that the following prerequisites are met before you configure network segmentation for disaster recovery: 

- Ensure that the VLAN and subnet that you plan to use for the network segment are routable. 

- Ensure that you have a pool of IP addresses to specify when configuring segmentation. For each cluster, you need n+1 IP addresses, where _`n`_ is the number of nodes in the cluster. The additional IP address is for the virtual IP address requirement. 

- Ensure that network segmentation for disaster recovery is enabled at both sites (local and remote) before configuring remote sites at those sites. 

## **Limitations** 

## **Nutanix Volumes** 

- When you enable network segmentation for Volumes, you cannot recover volume group attachments during VM recovery. 

AOS Security | Securing Traffic Through Network Segmentation | **178** 

- Nutanix service VMs such as Objects worker nodes continue to communicate with the CVM `eth0` — interface when using Volumes for iSCSI traffic. Other external clients, such as Files, use the new service-specific CVM interface. 

## **Cluster Services That Support Traffic Isolation** 

You can isolate traffic associated with the following services to its own virtual network: 

- Management (the default network that cannot be moved from CVM — `eth0` ) 

- Backplane 

- RDMA 

- Service-specific disaster recovery 

- Service-specific Volumes 

## **Unsupported Configurations for Network Segmentation** 

The following cluster configurations do not support network segmentation: 

- Clusters on which the CVMs have a manually created `eth2` — interface. 

- Clusters on which the — `eth2` interface on one or more CVMs have manually assigned IP addresses. During an upgrade to an AOS release that supports network segmentation, AOS creates an `eth2` — interface on each CVM in a cluster. Even though the cluster does not use these interfaces until you configure network segmentation, you must not manually configure these interfaces in any way. 

- Clusters on which CVM interfaces are connected to port groups backed by NSX NVDS switches. 

## **Caution:** 

Nutanix deprecated support for manual multi-homed CVM network interfaces from AOS version 5.15 and later. Such a manual configuration can lead to unexpected issues on these releases. If you configured an `eth2` interface on the CVM manually, see the KB 9479 and Nutanix Field Advisory #78 for details on how to remove the `eth2` interface. 

## **Troubleshooting Tips** 

This section provides information to assist troubleshooting network segmentation deployments. 

The Failed to restart one or more services after Backplane was enabled error might occur while enabling network segmentation. In such cases, the network segmentation task gets completed, however, restarting one or more services fails to complete on time. 

To ensure that the necessary services starts on time, log in to a CVM over SSH and run the following command: 

## ~~SSS~~ `nutanix@CVM:~$ cluster start` 

Verify the service status and ensure that all services are listed as UP. 

## **Prepare the Network on an AHV Host** 

Host networking on AHV enables traffic segmentation and must be configured on both existing cluster nodes and new, unconfigured nodes before isolation. 

You must configure host networking for physical and service-specific network segmentation on an AHV host. Configuring host networking is a prerequisite for physical and service-specific network segmentation. 

Two scenarios for configuring host networking on an AHV host: 

AOS Security | Securing Traffic Through Network Segmentation | **179** 

- Segmenting traffic on nodes that are already part of a cluster; for more information, see Configuring the Network on Existing Nodes on page 180. 

- Segmenting traffic on an unconfigured node that is not part of a cluster; for more information, see Configuring the Network on New Nodes on page 180. 

For information about the procedures to create, update and delete a virtual switch in Prism Element Web Console, see Configuring a Virtual Network for Guest VMs in the _Prism Element Web Console Guide_ . 

**Note:** The term _unconfigured node_ in the _Configuring the Network on Existing Nodes_ and _Configuring the Network on New Nodes_ procedures refers to a node that is not part of a cluster and is being prepared for cluster expansion. 

If you are configuring host networking on an ESXi host, you create vSwitches and port groups to achieve the same results. For more information, see the ESXi documentation. 

## **Configuring the Network on Existing Nodes** 

Configure host networking for physical and service-specific network segmentation on existing nodes. 

## **About this task** 

Perform the following procedure if you are segmenting traffic on nodes that are already part of a cluster: 

## **Procedure** 

**1.** Remove the uplinks from the default virtual switch `vs0` 

   - . 

**2.** Create a virtual switch for the backplane traffic or service whose traffic you plan to isolate. 

For information about creating a new virtual switch, see Creating or Updating a Virtual Switch in the _Prism Web Console Guide_ . 

**3.** Add the uplinks to the new virtual switch. 

## **What to do next** 

Prism Element can configure a VLAN only on AHV hosts. If the hypervisor is ESXi, in addition to configuring the VLAN on the physical switch, you must configure the VLAN on the port group. 

If you are performing physical network segmentation, see Physically Isolate the Backplane Traffic on an Existing Cluster on page 183. 

If you are performing service-specific traffic isolation, see Service-Specific Traffic Isolation on page 197. 

## **Configuring the Network on New Nodes** 

Configure host networking for physical and service-specific network segmentation on new nodes. 

## **About this task** 

Perform the following procedure if you are segmenting traffic on an unconfigured node (new host) that is not part of a cluster: 

## **Procedure** 

**1.** Log into the new AHV host as a root user using SSH and create a directory: 

`$ ssh root@` _`IP-address-host`_ `"mkdir -p /dev/shm/config/"` 

Replace _`IP-address-host`_ with the IP address of the newly created AHV host. 

AOS Security | Securing Traffic Through Network Segmentation | **180** 

**2.** Create a cluster_config file: 

`$ ssh root@` _`IP-address-host`_ `"echo -e '{\n \"arp_ip\": \"` _`ARP-internal-IP-address`_ `\",\n \"dhcp_ip\": \"\"\n}' > /dev/shm/config/cluster_config.0"` 

Replace: 

- _`IP-address-host`_ ee with the IP address of the newly created AHV host. 

- _`ARP-internal-IP-address`_ ~~LS~~ with an internal IP address from the subnet reserved for Kubernetes infrastructure network, for example `192.168.5.2` ee . 

The newly created node must include a cluster_config file for the `manage_ovs` ae commands to work. 

**3.** Log into the new AHV host as a nutanix user using SSH and create a bridge for the backplane traffic or service: 

O `nutanix@cvm$ manage_ovs --bridge_name` —“CSCSC‘“C(SNSNC(‘#CO”SO _`bridge-name`_ `create_single_bridge` Replace the _`bridge-name`_ ae with a name of the bridge, for example br0. 

Perform this step only on a newly imaged or foundation setup. 

**Note:** After you run the `manage_ovs –bridge_name` command, the system shows the Failed to fetch gflags. Acropolis service might be down error message. You can ignore the error and proceed. 

**4.** From the default bridge `br0` _ , log in to the host CVM and keep only = `eth0` and a `eth1` in `br0` : 

`nutanix@cvm$ manage_ovs --bridge_name br0 --interfaces eth0,eth1 -- bond_name` _`bond_name`_ `--bond_mode` _`bond_mode`_ `update_uplinks` 

Replace: 

   - _`bond-name`_ Pe with the name of the uplink port such as **br0-up** for which you want to set the bond mode. 

   - _`bond_mode`_ a with a valid bond mode such as **active-backup** . 

**5.** Log in to the host CVM and add `eth2` — and `eth3` to the uplink bond of _ `br1` : 

`nutanix@cvm$ manage_ovs --bridge_name br1 --interfaces eth2,eth3 --bond_name` _`bondname`_ `--bond_mode active-backup update_uplinks` 

Replace the _`bond-name`_ ae with a name of the uplink port corresponding to the bridge name br1, for example **br1up** . 

**Note:** If this step is not done correctly, it creates a network loop that causes a network outage. Ensure that no other uplink interfaces exist on this bridge before adding the new interfaces, and always add interfaces into a bond. 

## **What to do next** 

Prism Element can configure a VLAN only on AHV hosts. If the hypervisor is ESXi, in addition to configuring the VLAN on the physical switch, you must configure the VLAN on the port group. 

If you are performing physical network segmentation, see Physically Isolate the Backplane Traffic on an Existing Cluster on page 183. 

If you are performing service-specific traffic isolation, see Service-Specific Traffic Isolation on page 197. 

## **Network Segmentation for Traffic Types- Backplane and Management** 

You can segment the network in a Nutanix cluster to isolate traffic types such as backplane and management in the following ways: 

- **On an existing cluster:** You can segment the network on an existing cluster using the Prism Element web console. 

AOS Security | Securing Traffic Through Network Segmentation | **181** 

- **During cluster creation** : Segment the network when creating a cluster using Nutanix Foundation 3.11.2 or higher versions. 

For more information about segmenting the network when creating a cluster, see the Field Installation Guide . 

Avoid using the following IP addresses when selecting an IP Address range for network segmentation: 

- `169.254.0.0/16` - Automatic private IP Addresses range (APIPA) 

- `192.168.5.0/24` - Internal backplane communication 

- `192.168.7.2/27` - Cross cluster TCP Proxy subnet 

- `10.100.0.0/16 and 10.200.32.0/24` - Objects internal services 

## **Isolate the Backplane Traffic Logically Using VLAN-Based Segmentation** 

Use VLAN-based segmentation to isolate backplane traffic on an existing clusters using the Prism Element web console. 

You must configure a separate VLAN for the backplane network to achieve logical segmentation. The network segmentation process creates a separate network for backplane communications on the existing default virtual switch. The process then places the `eth2` interfaces (that the process creates on the CVMs during an upgrade process) and the host interfaces on the newly created network. This method allows you to achieve logical segmentation of traffic over the selected VLAN. From the specified subnet, assign IP addresses to each new interface. You need two IP addresses per node. When you specify the VLAN ID, AHV places the newly created interfaces on the specified VLAN. 

In this method, logical segmentation (VLAN-based segmentation) is done on the default bridge for AHV nodes. The process creates the host backplane interface (VMkernel) on the backplane network port group on ESXi or **br0backplane** (interface) on `br0` bridge in case of AHV. The `eth2` interface on the CVM is on the CVM backplane network by default. 

You don't need to manually create the VMkernel adapter for backplane segmentation as the workflow creates it on the port group selected as the host port group or default backplane network port group if none was selected. 

You need separate VLANs for the management and backplane networks. For example, configure VLAN 100 as the management network VLAN and VLAN 200 as the backplane network VLAN on the Ethernet links that connect the Nutanix nodes to the physical switch. 

## **Isolating the Backplane Traffic Logically on an Existing Cluster** 

Configure VLAN-based segmentation for backplane traffic on an existing ESXi and Hyper-V clusters. 

## **Before you begin** 

- Use the RDMA-specific procedure if your cluster includes RDMA-enabled NICs. 

For more information, see Isolating the Backplane Traffic on an Existing RDMA Cluster on page 209. 

- For ESXi clusters, you must create and manage port groups that networking uses for CVM and backplane networking. Ensure that you create port groups on the default virtual switch `vs0` for the ESXi hosts and CVMs. 

The backplane traffic segmentation is logical and based on the VLAN that you tag for the port groups. When creating the port groups, ensure that you tag the new port groups created for the ESXi hosts and CVMs with the appropriate VLAN ID. Consult your networking team to acquire the necessary VLANs for use with Nutanix nodes. 

**Note:** Nutanix does not control these VLAN IDs. 

- For new backplane networks, you must specify a non-routable subnet. The interfaces on the backplane network are automatically assigned IP addresses from this subnet, so reserve the entire subnet for the backplane network 

AOS Security | Securing Traffic Through Network Segmentation | **182** 

segmentation. See the Configuring Backplane IP Pool on page 191 topic to create an IP pool for backplane interfaces. 

- To segment the network on an existing AHV cluster for a backplane LAN, follow the procedure described in the Physically Isolating the Backplane Traffic on an AHV Cluster on page 184 topic. 

## **About this task** 

To segment the network on an existing ESXi and Hyper-V clusters for a backplane LAN, follow these steps: 

## **Procedure** 

**1.** Log in to the Prism Element web console as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** In the left pane, click **Network Configuration** . 

The **Network Configuration** dialog box appears. 

## **4.** In the **Internal Interfaces** > **Backplane LAN** row, click **Configure** . 

The **Create Interface** dialog box appears. 

**5.** In the **Create Interface** dialog box, provide the necessary information: 

   **1.** In the **Subnet IP** field, specify a non-routable subnet. 

Ensure that the subnet has sufficient IP addresses. The segmentation process requires two IP addresses per node. Reconfiguring the backplane to increase the size of the subnet involves cluster downtime, so you might also want to make sure that the subnet can accommodate new nodes in the future. 

**2.** In the **Netmask** field, specify the netmask. 

**3.** (Optional) To assign the interfaces on the network to a VLAN, specify the VLAN ID in the **VLAN ID** field. 

Nutanix recommends that you use a VLAN. If you do not specify a VLAN ID, Prism Element uses the default VLAN on the virtual switch. 

**4.** In the **Host Port Group** list, select the port group that you created for the host. 

**5.** In the **CVM Port Group** list, select the port group that you created for the CVM. 

**Note:** For VLAN-based segmentation, Nutanix recommends that you leave the Host Port Group and CVM Port Group fields blank. Prism Element selects the default port group from vSwitch0 when you do not provide the Host Port Group and CVM Port Group details. 

## **6.** Click **Verify and Save** . 

The network segmentation process creates the backplane network if the network settings that you specified pass validation. 

## **Physically Isolate the Backplane Traffic on an Existing Cluster** 

Use the Prism Element web console to configure the `eth2` interface on a separate virtual switch. This setup isolates backplane traffic on a dedicated physical network. 

If you don’t configure a separate virtual switch, backplane traffic uses a different VLAN on the default switch for logical isolation. 

A virtual switch is referred to differently across hypervisors: 

- AHV - Virtual switch 

- VMware ESXi - vSwitch 

- Microsoft Hyper-V - Hyper-V Virtual Switch 

AOS Security | Securing Traffic Through Network Segmentation | **183** 

Network segmentation process creates a separate network for backplane communications on the new virtual switch. The segmentation process places the CVM `eth2` interfaces and the host interfaces on the newly created network. Specify a subnet with a network mask and, optionally, a VLAN ID. From the specified subnet or an IP pool assign IP addresses to each new interface in the new network. You require a minimum of two IP addresses per node. 

If you specify the optional VLAN ID, the segmentation process places the newly created interfaces on VLAN. 

Nutanix highly recommends using separate VLAN for the backplane network to achieve true segmentation. 

## **Requirements** 

- Ensure that physical isolation of backplane traffic is supported by the AOS version deployed. 

- Ensure that you configure the network (port groups or bridges) on the hosts and associate the network with the required physical NICs before you enable physical isolation of the backplane traffic. 

For AHV, see Configuring the Network on Existing Nodes on page 180. For ESXi and Hyper-V, see VMware or Microsoft documentation. 

## **Limitations** 

- On an ESXi cluster with a DVS switch and network segmentation enabled, the ESXi hypervisor might trigger HA events such as host isolation response and reboot all VMs when the management network goes down. To avoid this situation, configure multiple isolation response addresses using the das.isolationaddress Advance Option in VMware vCenter. 

- Segmenting backplane traffic can involve up to two rolling reboots of the CVMs. The first rolling reboot moves the backplane interface ( `eth2` ) of the CVM to the selected port group, virtual switch or Hyper-V switch. This is done only for CVMs whose backplane interface is not already connected to the selected port group, virtual switch or Hyper-V switch. The second rolling reboot migrates the cluster services to the newly configured backplane interface. 

## **Physically Isolating the Backplane Traffic on an AHV Cluster** 

Create a dedicated virtual switch and configure the backplane network to physically segment backplane traffic on an AHV-based Nutanix cluster. 

## **Before you begin** 

Ensure that the network is configured on the AHV hosts. For more information, see Configuring the Network on Existing Nodes on page 180. 

**Note:** Before you perform the following procedure, ensure that the uplinks you added to the virtual switch are in the UP state. 

## **About this task** 

To physically segment the backplane traffic on an AHV cluster, follow these steps: 

## **Procedure** 

**1.** Shut down all the guest VMs in the cluster from within the guest OS or use the Prism Element web console. 

AOS Security | Securing Traffic Through Network Segmentation | **184** 

**2.** Place all AHV hypervisor nodes of a cluster into the maintenance mode. Do not place the CVM nodes into the maintenance mode: 

   - a. Use SSH to log in to a Controller VM in the cluster. 

b. Determine the IP address of all the AHV hypervisor nodes: 

~~CSCC~~ `nutanix@cvm$ acli host.list` 

Note all the hypervisor IP addresses for the cluster. 

- c. Put each node in the maintenance mode: 

`nutanix@cvm$ acli host.enter_maintenance_mode` _`hypervisor-IP-address`_ `[wait="{ true | false }" ] [non_migratable_vm_action="{ acpi_shutdown | block }" ]` 

Replace _`hypervisor-IP-address`_ ~~a~~ with the AHV hypervisor IP address. 

The following are optional parameters: 

- ~~—~~ `wait` 

> • ~~Ts~~ `non_migratable_vm_action` 

For example: 

`nutanix@cvm$ acli host.enter_maintenance_mode 197.116.6.79 EnterMaintenanceMode: pending EnterMaintenanceMode: complete` 

**Note:** Never put Controller VM and AHV hosts into maintenance mode on single-node clusters. Nutanix recommends shutting down user VMs before proceeding with disruptive changes. 

If a cluster has HA reservation enabled, the nodes in the cluster cannot enter maintenance mode. To check if the cluster has HA reservation enabled, go to **Settings > Manage VM High Availability** . 

Do not continue if the node fails to enter the maintenance mode. 

- d. Verify that the node is in the maintenance mode: 

~~SSCs~~ `nutanix@cvm$ acli host.get` _`hypervisor-IP-address`_ 

In the output that is displayed, ensure that **node_state** is **EnteredMaintenanceMode** and **schedulable** is **False** . 

Example output. 

`nutanix@cvm$ acli host.get 197.116.6.79 197.116.6.79 { cpu_usage_ppm: 192941 cvm_memory_size_bytes: 21474836480 cvm_num_vcpus: 8 cvm_num_vnics: 3 cvm_uuid: "e96dbef0-d425-4926-9fe8-4d10b1a69902" host_overhead_bytes: 4957721854 logical_timestamp: 23 max_mem_ha_reserved_bytes: 0 mem_assigned_bytes: 0 mem_usage_bytes: 26654856446 memory_size_bytes: 269842644992 node_state: "EnteredMaintenanceMode" num_cpus: 32 pool_size_bytes: 0 schedulable: False uuid: "cc50ea78-49ce-4767-90b3-a0a2ef891446"` 

AOS Security | Securing Traffic Through Network Segmentation | **185** 

`}` 

**3.** Enable backplane network segmentation: 

   - a. Log on to the Prism web console, click the gear icon in the top-right corner, then click **Network Configuration** in the **Settings** page. 

   - b. On the **Internal Interfaces** tab, in the **Backplane LAN** row, click **Configure** . 

   - c. In the **Backplane LAN** dialog box, follow these steps: 

      **1.** In **Subnet IP** , specify a non-routable subnet that is different from the subnet used by the AHV host and CVMs. 

The AOS CVM default route uses the CVM `eth0` interface, and no route on the backplane interface. Therefore, Nutanix recommends that you use only a non-routable subnet for the backplane network. To avoid split routing, do not use a routable subnet for the backplane network. 

Make sure that the backplane subnet has a sufficient number of IP addresses. Two IP addresses are required per node. Reconfiguring the backplane to increase the size of the subnet involves cluster downtime, so ensure that the subnet can accommodate new nodes in the future. 

You can also use an IP pool to configure network segmentation. For more information, see Configuring Backplane IP Pool on page 191 

**2.** In **Netmask** , specify the network mask. 

**3.** (Optional) To assign the interfaces on the network to a VLAN, specify the VLAN ID in the **VLAN ID** field. 

Nutanix strongly recommends configuring a separate VLAN. If you do not specify a VLAN ID, AOS applies the untagged VLAN on the virtual switch. 

**4.** In the **Virtual Switch** list, select the virtual switch that you created for the backplane traffic. 

## d. Click **Verify and Save** . 

If the network settings that you specified pass validation, the segmentation process creates a backplane network and the CVMs perform a reboot in a rolling fashion (one at a time), after which the services use the new backplane network. You can track the progress of this operation on the Prism Element web console Tasks page. 

**4.** Log in to a CVM in the cluster with SSH and stop Acropolis cluster-wide: 

`nutanix@cvm$ allssh genesis stop acropolis` 

**5.** Restart Acropolis cluster-wide: 

`nutanix@cvm$ cluster start` 

**6.** Remove all nodes from maintenance mode: 

   - a. From any CVM in the cluster, exit the AHV host from the maintenance mode: 

`nutanix@cvm$ acli host.exit_maintenance_mode` _`hypervisor-IP-address`_ 

Replace _`hypervisor-IP-address`_ with the IP address of the node. 

For example. 

`nutanix@CVM$ acli host.exit_maintenance_mode 197.116.6.79 ExitMaintenanceMode: pending` 

AOS Security | Securing Traffic Through Network Segmentation | **186** 

## ~~CSS~~ `ExitMaintenanceMode: complete` 

This command migrates all the VMs that were previously running on the host back to the host. 

b. Verify that the node has exited the maintenance mode: 

~~OO~~ `nutanix@cvm$ acli host.get` _`hypervisor-IP-address`_ Replace _`hypervisor-IP-address`_ ae with the IP address of the node. 

In the output that is displayed, ensure that **node_state** is **kAcropolisNormal** or **AcropolisNormal** and **schedulable** is **True** . 

Example output. 

`nutanix@cvm$ acli host.get 197.116.6.79 197.116.6.79 { cpu_usage_ppm: 192941 cvm_memory_size_bytes: 21474836480 cvm_num_vcpus: 8 cvm_num_vnics: 3 cvm_uuid: "e96dbef0-d425-4926-9fe8-4d10b1a69902" host_overhead_bytes: 4957721854 logical_timestamp: 23 max_mem_ha_reserved_bytes: 0 mem_assigned_bytes: 0 mem_usage_bytes: 26654856446 memory_size_bytes: 269842644992 node_state: "AcropolisNormal" num_cpus: 32 pool_size_bytes: 0 schedulable: True uuid: "cc50ea78-49ce-4767-90b3-a0a2ef891446" }` 

**7.** Power on the guest VMs from the Prism Element web console. 

## **Physically Isolating the Backplane Traffic on an ESXi Cluster** 

Configure a dedicated vSwitch, subnet, and VLAN to physically isolate backplane traffic and apply settings using the Prism Element web console for secure segmentation. 

## **Before you begin** 

On the ESXi hosts, follow these steps: 

**1.** Create a vSwitch for the backplane traffic. 

**2.** From a `vSwitch0` , remove the uplinks (physical NICs) that you plan to add to the vSwitch that you created for the backplane traffic. 

**3.** On the backplane vSwitch, create one port group for the CVM and another for the host. 

   - Ensure that at least one uplink is present in the Active Adaptors list for each port group if you have overridden the failover order. 

You don't need to manually create the VMkernel adapter for backplane segmentation since the workflow takes care of creating it on the port group selected as the host port group. 

For more information, see VMware ESXi documentation. 

Before you perform the following procedure, ensure that the uplinks you added to the vSwitch are in the UP state. 

## **About this task** 

To physically segment the backplane traffic, follow these steps: 

AOS Security | Securing Traffic Through Network Segmentation | **187** 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the top-right corner, then in the **Settings** page, click **Network Configuration** . 

**3.** On the **Internal Interfaces** tab, in the **Backplane LAN** row, click **Configure** . 

**4.** In the **Backplane LAN** dialog box, follow these steps: 

   - a. In **Subnet IP** , specify a non-routable subnet that is different from the subnet used by the ESXi host and CVMs. 

> The AOS CVM default route uses the CVM — `eth0` interface, and no route on the backplane interface. Nutanix recommends that you use only a non-routable subnet for the backplane network. To avoid split routing, do not use a routable subnet for the backplane network. 

Make sure that the subnet has a sufficient number of IP addresses. Each node requires two IP addresses. Reconfiguring the backplane to increase the size of the subnet involves cluster downtime, so you might also want to make sure that the subnet can accommodate new nodes in the future. 

- b. In **Netmask** , specify the network mask. 

- c. (Optional) To assign the interfaces on the network to a VLAN, specify the VLAN ID in the **VLAN ID** field. Nutanix strongly recommends configuring a separate VLAN. If you do not specify a VLAN ID, AOS applies the default VLAN on the virtual switch. 

- d. In the **Host Port Group** list, select the port group that you created for the host. 

A port group can be a standard vSwitch port group, a distributed vSwitch port group, or a NSX segment reflected as a distributed port group in the vCenter. 

- e. In the **CVM Port Group** list, select the port group that you created for the CVM. 

A port group can be a standard vSwitch port group, a distributed vSwitch port group, or a NSX segment reflected as distributed port group in the vCenter. 

## **Note:** 

Nutanix clusters support both vSphere Standard Switches and vSphere Distributed Switches with either normal portgroups or NSX segment backed portgroups. However, you must configure only one type of virtual switches in one cluster. Configure all the backplane and management traffic in one cluster on either vSphere standard switches or vSphere distributed switches. Do not mix standard and distributed vSwitches on a single cluster. Make sure that you configure the same port group type (normal or NSX segment) on all nodes of the cluster. 

## **5.** Click **Verify and Save** . 

If the network settings that you specified pass validation, the segmentation process creates the backplane network and the CVMs perform a reboot in a rolling fashion, after which the services use the new backplane network. You can track the progress of this operation in the Prism Element web console Tasks page. 

## **Physically Isolating the Backplane Traffic on a Hyper-V Cluster** 

Configure a dedicated Hyper-V switch, subnet, and VLAN to physically isolate backplane traffic and apply settings using the Prism Element web console for secure segmentation. 

## **Before you begin** 

On the Hyper-V hosts, perform these steps: 

**1.** Create a Hyper-V Virtual Switch for the backplane traffic. 

AOS Security | Securing Traffic Through Network Segmentation | **188** 

**2.** From the default External Switch, remove the uplinks (physical NICs) that you want to add to the backplane Virtual Switch you created for the backplane traffic. 

**3.** On the backplane Virtual Switch, create a subnet and, optionally, assign a VLAN. 

For more information, see Hyper-V documentation on the Microsoft portal. 

Before you perform the following procedure, ensure that the uplinks that you added to the backplane Virtual Switch are in the UP state. 

## **About this task** 

To physically segment the backplane traffic, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** Click the gear icon in the top-right corner, and then in the **Settings** page, click **Network Configuration** . 

**3.** On the **Internal Interfaces** tab, in the **Backplane LAN** row, click **Configure** . 

**4.** In the **Backplane LAN** dialog box, follow these steps: 

   - a. In **Subnet IP** , specify a non-routable subnet that is different from the subnet used by the Hyper-V host and CVMs. 

The AOS CVM default route uses the CVM `eth0` interface, and no route on the backplane interface. Nutanix recommends that you use only a non-routable subnet for the backplane network. To avoid split routing, do not use a routable subnet for the backplane network. 

Make sure that the subnet has a sufficient number of IP addresses. Each node requires two IP addresses. Reconfiguring the backplane to increase the size of the subnet involves cluster downtime, so you might also want to make sure that the subnet can accommodate new nodes in the future. 

- b. In **Netmask** , specify the network mask. 

- c. (Optionally) To assign the interfaces on the network to a VLAN, specify the VLAN ID in the **VLAN ID** field. Nutanix strongly recommends that you configure a separate VLAN. If you do not specify a VLAN ID, AOS applies the default VLAN on the virtual switch. 

- d. In the **Bridge** list, select the Hyper-V switch that you created for the backplane traffic. 

## **5.** Click **Verify and Save** . 

If the network settings you specified pass validation, the segmentation process creates the backplane network and the CVMs perform a reboot in a rolling fashion, after which the services use the new backplane network. You can track the progress of this operation in the Prism Element web console Tasks page. 

Segmenting backplane traffic can involve up to two rolling reboots of the CVMs. The first rolling reboot moves the backplane interface ( `eth2` ) of the CVM to the selected port group or virtual switch. This is done only for CVMs whose backplane interface is not already connected to the selected port group or bridge virtual switch. The second rolling reboot migrates the cluster services to the newly configured backplane interface. 

## **Enabling Physical Backplane Segmentation on Hyper-V Using CLI** 

Physical backplane segmentation support is now available on a cluster containing Hyper-V nodes. 

## **About this task** 

To enable physical backplane segmentation on a cluster containing Hyper-V node using CLI, follow these steps: 

AOS Security | Securing Traffic Through Network Segmentation | **189** 

## **Procedure** 

**1.** Log on to any CVM in the cluster using SSH. 

**2.** Enable backplane segmentation on a Hyper-V node: 

`nutanix@CVM:~$ network_segmentation --backplane_network --ip_pool=` _`IP-Pool-name`_ `--backplane_vlan=` _`VLAN-ID`_ `--host_physical_network=` _`hyperv_host_physical_network`_ 

- Replace ae _`IP-Pool-name`_ with a user defined IP pool name 

- Replace Le _`VLAN-ID`_ with a backplane VLAN ID 

- Replace ~~eS~~ _`hyperv_host_physical_network`_ with the Hyper-V switch name 

- For example: 

`nutanix@CVM:~$ network_segmentation --backplane_network --ip_pool=BackplanePool --backplane_vlan=1234 --host_physical_network=BackplaneSwitch` 

## **Enabling Backplane Network Segmentation on a Mixed Hypervisor Cluster** 

Enable backplane network segmentation on a mixed hypervisor cluster. 

## **About this task** 

You can enable Backplane Network Segmentation on a mixed hypervisor cluster: 

- ESXi and AHV storage-only nodes 

- Hyper-V and AHV storage-only nodes 

To enable backplane network segmentation on a mixed hypervisor cluster, follow these steps: 

## **Procedure** 

**1.** Log in to any CVM in the cluster using SSH. 

**2.** Enable network segmentation for backplane traffic: 

   - On a cluster containing ESXi and AHV storage only nodes: 

`nutanix@cvm$ network_segmentation --backplane_network --ip_pool=` _`IP-pool-name`_ `--backplane_vlan=` _`VLAN-ID`_ `[--esx_host_physical_network=` _`ESXi-host-portgroup-name`_ `] [--esx_cvm_physical_network=` _`ESXi-cvm-portgroup-name`_ `] [--ahv_host_physical_network=` _`AHV-network-name`_ `]` 

- On a cluster containing Hyper-V and AHV storage-only nodes: 

`nutanix@cvm$ network_segmentation --backplane_network --ip_pool=` _`IP-pool-name`_ `--backplane_vlan=` _`VLAN-ID`_ `[--hyperv_host_physical_network=` _`HyperV-host-network-name`_ `] [--ahv_host_physical_network=` _`AHV-network-name`_ `]` 

- Replace ~~|~~ _`IP-Pool-name`_ with a user defined IP pool name 

AOS Security | Securing Traffic Through Network Segmentation | **190** 

- Replace Pe _`VLAN-ID`_ with a backplane VLAN ID 

- Replace ~~es~~ _`ESXi-host-portgroup-name`_ with the ESXi host network name 

- Replace ~~ee~~ _`ESXi-cvm-portgroup-name`_ with the ESXi CVM network name 

- Replace ee _`AHV-network-name`_ with the AHV storage-only node bridge name 

- Replace ~~es~~ _`HyperV-host-network-name`_ with the Hyper-V switch name 

For example, enable network segmentation on a mixed hypervisor containing ESXi and AHV storage-only nodes: 

`nutanix@cvm$ network_segmentation --backplane_network --ip_pool=BackplanePool --backplane_vlan=1234 --esx_host_physical_network=host-pg --esx_cvm_physical_network=cvm-pg --ahv_host_physical_network=br1` 

## **Backplane IP Pool** 

Create and manage small IP pools for backplane traffic using the CLI to optimize IP usage and avoid full subnet allocation. 

Network segmentation for backplane traffic previously required an entire subnet even if a cluster has a small number of nodes. Allocating an entire subnet resulted in inefficient use of IP addresses. The backplane IP address pool feature creates a small IP address pool instead of an entire subnet. 

You can create an IP address pool using the `network_segmentation ip_pool` command. The named IP address pool includes one or more IP address ranges. For example, you can define one range from 172.16.1.100 to 172.16.1.105 and another range from 172.16.1.120 to 172.16.1.125, all within the same named IP address pool and subnet. 

Currently, the Prism Element web console does not offer an option to create an IP address pool specifically for backplane segmentation. However, it does allow for the creation of small IP address pools for service-specific traffic, such as Nutanix Volumes and Nutanix Disaster Recovery (DR). You can use the new `network_segmentation ip_pool` CLI to create IP address pools for backplane, Nutanix Volumes, and Nutanix DR. You can also manage (edit, delete, and update) IP address pools that are created for backplane, Nutanix Volumes and Nutanix DR using the new CLI. 

## **Configuring Backplane IP Pool** 

Create an IP pool for backplane interfaces using the CLI. 

## **About this task** 

To configure IP address pool for backplane, perform the following steps: 

## **Procedure** 

**1.** Log in to any CVM in the cluster using SSH. 

**2.** Create a new IP address pool and define the IP address ranges: 

`nutanix@cvm$ network_segmentation --ip_pool_name=` _`IP-Pool-name`_ `-ip_pool_netmask=` _`netmask`_ `--ip_ranges="[(‘` _`First-IP-Address`_ `', ‘` _`Last-IP-Address`_ `'), (‘` _`First-IP-Address`_ `', ‘` _`Last-IPAddress`_ `')]" ip_pool create` 

- Replace _IP-Pool-name_ with a user-defined IP address pool name 

- Replace _Netmask_ with a network mask in dot-decimal mask notation 

AOS Security | Securing Traffic Through Network Segmentation | **191** 

- Replace _First-IP-Address_ with the first IP address in the range 

- Replace _Last-IP-Address_ with the last IP address in the same range 

For example: 

`nutanix@cvm$ network_segmentation --ip_pool_name=BackplanePool -- ip_pool_netmask=255.255.255.0 --ip_ranges="[('172.16.1.100', '172.16.1.105'), ('172.16.1.120', '172.16.1.125')]" ip_pool create` 

## **Reconfiguring the Backplane Network** 

Backplane network reconfiguration is a CLI-driven procedure that you perform on any one of the CVMs in the cluster. The change is propagated to the remaining CVMs. 

## **About this task** 

**Caution:** At the end of this procedure, the cluster stops and restarts, even if only the VLAN is changed, and involves cluster downtime. 

To reconfigure the cluster, follow these steps: 

## **Procedure** 

**1.** Log on to any CVM in the cluster using SSH. 

**2.** Reconfigure the backplane network: 

`nutanix@cvm$` backplane_ip_reconfig `[--backplane_vlan=` _`vlan-id`_ `] \ [--backplane_ip_pool=` _`ip_pool_name`_ `]` 

> Replace a _`vlan-id`_ with the new VLAN ID, and ~~a~~ _`ip_pool_name`_ with the newly created backplane IP address pool. 

For information on creating a backplane IP address pool, see Configuring Backplane IP Pool on page 191. 

> For example, reconfigure the backplane network to use VLAN ID = `10` and newly created backplane IP pool _NewBackplanePool_ : 

`nutanix@cvm$ backplane_ip_reconfig --backplane_vlan=10 \ --backplane_ip_pool=NewBackplanePool` 

Example output: 

`This operation will do a 'cluster stop', resulting in disruption of cluster services. Do you still want to continue? (Type "yes" (without quotes) to continue) Type yes to confirm that you want to reconfigure the backplane network.` 

During the reconfiguration process, you might receive an error message similar to the following: 

> `Failed to reach a node.` ~~ee~~ 

You can ignore this error message and therefore do not stop the script manually. 

**Note:** The `backplane_ip_reconfig` command is not supported on ESXi clusters with vSphere Distributed Switches. To reconfigure the backplane network on a vSphere Distributed Switch setup, disable the backplane network (see Disabling Network Segmentation on an ESXi and Hyper-V Clusters on page 194) and enable again with a different subnet or VLAN. 

AOS Security | Securing Traffic Through Network Segmentation | **192** 

**3.** Type yes to confirm. 

The reconfiguration procedure takes a few minutes and includes a cluster restart. If you type anything other than yes, the system aborts the network reconfiguration. 

**4.** After the process completes, verify that the backplane was reconfigured: 

a. Verify that the IP addresses of the — `eth2` interfaces on the CVM are set correctly: 

~~CSS~~ `nutanix@cvm$ svmips -b` Output similar to the following is displayed: ~~CSS~~ `172.30.25.1 172.30.25.3 172.30.25.5` 

b. Verify that the IP addresses of the backplane interfaces of the hosts are set correctly. 

~~OO~~ `nutanix@cvm$ hostips -b` Output similar to the following is displayed: ~~OO~~ `172.30.25.2 172.30.25.4 172.30.25.6` 

The svmips and hostips commands, when used with the option 7 `b` , display the IP addresses assigned to the interfaces on the backplane. 

## **Update Backplane Port Groups** 

Update backplane port groups on ESXi clusters without disabling segmentation. 

You can update the backplane port groups that are assigned to CVM and host nodes. Previously, to change a port group that is assigned to a CVM and host, you had to disable network segmentation and re-enable it with the new port groups. 

This feature is only supported on a cluster running an ESXi hypervisor. 

Updating backplane port groups: 

- Helps you to move from one vSphere Standard Switch (VSS) port group to another VSS port group within the same virtual standard switch 

- Helps you to move from one VSS port group to another VSS port group in a different Virtual Standard Switch 

- Helps you to move from a VSS port group to a vSphere Distributed Switch (VDS) port group 

- Helps you to move from a VDS port group to a VSS 

## **Note:** 

To rename existing VSS or VDS port groups, you need to manually perform the rename operation either through the vCenter application or by using the ESXi CLI. Run the update operation with the new port group name. This process ensures that the configuration in the Nutanix internal database is kept up to date. 

## **Limitations of Updating Backplane Port Group** 

Consider the following limitations before updating backplane port groups: 

- Clusters running on an AHV or Hyper-V hypervisors do not support updating backplane port groups feature. 

- This feature does not support updating any other configuration such as VLAN ID and IP addresses. 

**Note:** This feature does not perform any network validation on the new port groups. You must ensure the port group settings are accurate before proceeding with the port group update operation. If the settings are not accurate, the CVM on that node might not be able to communicate with its peers and this results in a stuck rolling reboot. 

AOS Security | Securing Traffic Through Network Segmentation | **193** 

## **Updating Backplane Port Group** 

Update the backplane port groups that are assigned to CVM and host nodes. 

## **About this task** 

To update the backplane port groups that are assigned to CVM and host nodes, follow these steps: 

## **Procedure** 

**1.** Log in to any CVM in the cluster using SSH. 

**2.** Update the CVM and host port groups: 

`nutanix@cvm$ network_segmentation --backplane_network --host_physical_network=` _`new-host-portgroup-name`_ `--cvm_physical_network=` _`new-cvm-portgroup-name`_ `--update` 

- Replace ~~eS~~ _`new-host-portgroup-name`_ with the new host port group name 

- Replace ~~a~~ _`new-cvm-portgroup-name`_ with the new CVM port group name 

For example: 

`nutanix@cvm$ network_segmentation --backplane_network --host_physical_network=new-bp-host-pgroup --cvm_physical_network=new-bp-cvm-pgroup --update` 

For more information, see Creating Port Groups on the Distributed Switch in _vSphere Administration Guide for Acropolis_ . 

## **Disabling Network Segmentation on an ESXi and Hyper-V Clusters** 

Backplane network reconfiguration is a CLI-driven procedure that you perform on any one of the CVMs in the cluster. The change is propagated to the remaining CVMs. 

## **About this task** 

To disable network segmentation on an ESXi and Hyper-V clusters, follow these steps: 

## **Procedure** 

**1.** Log on to any CVM in the cluster using SSH. 

**2.** Disable network segmentation on an ESXi and Hyper-V cluster: 

> `nutanix@cvm$ network_segmentation --backplane_network --disable` ~~SSS~~ 

Output similar to the following appears: 

`Operation type : Disable Network type : kBackplane Params : {} Please enter [Y/y] to confirm or any other key to cancel the operation` 

Type Y or y to confirm. 

If you type Y or y, network segmentation is disabled and the controller VMs (CVMs) restart in a rolling manner, one CVM at a time. If you type anything other than Y or y, network segmentation is not disabled. 

This method does not involve cluster downtime. 

AOS Security | Securing Traffic Through Network Segmentation | **194** 

**3.** Verify that you successfully disabled network segmentation. 

   - You can verify in one of two ways: 

   - » Verify that the backplane is disabled. 

> `nutanix@cvm$ network_segment_status` ~~ee~~ Output similar to the following is displayed: 

2017-11-23 06:18:23 INFO zookeeper_session.py:110 network_segment_status is attempting to connect to Zookeeper 

Network segmentation is disabled 

- » Verify that the commands to show the backplane IP addresses of the CVMs and hosts list the same management IP addresses. Run the svmips and hostips command with and without the `-b` = option, then compare the IP addresses shown in the output. For example: 

`nutanix@cvm$ svmips 192.127.3.2 192.127.3.3 192.127.3.4 nutanix@cvm$ svmips -b 192.127.3.2 192.127.3.3 192.127.3.4 nutanix@cvm$ hostips 192.127.3.5 192.127.3.6 192.127.3.7 nutanix@cvm$ hostips -b 192.127.3.5 192.127.3.6 192.127.3.7` 

In the example, the outputs of the svmips and hostips commands with and without the -b option are the same, indicating that the backplane network segmentation is disabled. 

## **Disabling Network Segmentation on an AHV Cluster** 

Disable network segmentation on an AHV cluster. 

## **About this task** 

You perform backplane network reconfiguration procedure on any one of the CVMs in the cluster. The change propagates to the remaining CVMs. 

To disable network segmentation on an AHV cluster, follow these steps: 

## **Procedure** 

**1.** Shut down all the guest VMs in the cluster from within the guest OS or use the Prism Element web console. 

AOS Security | Securing Traffic Through Network Segmentation | **195** 

**2.** Place all nodes of a cluster into the maintenance mode: 

   - a. Use SSH to log on to a Controller VM in the cluster. 

   - b. Determine the IP address of the node you want to put into the maintenance mode: 

## ~~CSCC~~ `nutanix@cvm$ acli host.list` 

Note the value of hypervisor IP address for the node you plan to put in the maintenance mode. 

- c. Put the node into the maintenance mode: 

`nutanix@cvm$ acli host.enter_maintenance_mode` _`hypervisor-IP-address`_ `[wait="{ true | false }" ] [non_migratable_vm_action="{ acpi_shutdown | block }" ]` 

Replace _`host-IP-address`_ ~~a~~ with either the IP address or host name of the AHV host you plan to shut down. 

The following are optional parameters for running the acli host.enter_maintenance_mode command: 

- wait 

- non_migratable_vm_action 

Do not continue if the host fails to enter the maintenance mode. 

- d. Verify that the host is in the maintenance mode: 

## ~~ee~~ `nutanix@cvm$ acli host.get` _`host-ip`_ 

In the output that is displayed, ensure that **node_state** is **EnteredMaintenanceMode** and **schedulable** is **False** . 

**3.** Disable backplane network segmentation from the Prism web console. 

   - a. Log in to the Prism Element web console, click the gear icon in the top-right corner, and then click **Settings** > **Network Configuration** . 

   - b. In the **Internal Interfaces** tab, in the **Backplane LAN** row, click **Disable** . 

   - c. Click **Yes** to disable backplane LAN. 

Disabling backplane LAN involves a rolling reboot of CVMs to migrate the cluster services back to the external interface. 

**4.** Log in to a CVM in the cluster with SSH and stop Acropolis cluster-wide: 

> `nutanix@cvm$ allssh genesis stop acropolis` ~~SCS~~ 

**5.** Restart Acropolis cluster-wide: 

> `nutanix@cvm$ cluster start` ~~SSCS~~ 

**6.** Remove all nodes from maintenance mode: 

   - a. From any CVM in the cluster, run the following command to exit the AHV host from the maintenance mode: 

~~O~~ `nutanix@cvm$ acli host.exit_maintenance_mode` ~~O~~ _`host-ip`_ e—S—SSCSCCs 

Replace _`host-ip`_ a with the new IP address of the host. 

This command migrates all the VMs that were previously running on the host back to the host. 

- b. Verify that the host has exited the maintenance mode: 

~~ee~~ `nutanix@cvm$ acli host.get host-ip` 

In the output that is displayed, ensure that **node_state** equals to **kAcropolisNormal** or **AcropolisNormal** and **schedulable** equals **True** . 

AOS Security | Securing Traffic Through Network Segmentation | **196** 

**7.** Power on the guest VMs from the Prism Element web console. 

## **Service-Specific Traffic Isolation** 

Isolating the traffic associated with a specific service is a two-step process: 

- Configure the networks and uplinks on each host manually. The Prism Element only creates the VNIC that the service requires and places that VNIC on the bridge or port group that you specify. You must manually create the bridge or port group on each host and add the required physical NICs as uplinks to that bridge or port group. 

- Configure network segmentation for the service by using the Prism Element. Create an extra vNIC for the service, specify any additional parameters that are required (for example, IP address pools), and the bridge or port group that you plan to dedicate to the service. 

## **Isolating Service-Specific Traffic** 

Isolate a service to a separate virtual network. 

## **Before you begin** 

- Ensure that each host is configured as described in Configuring the Network on Existing Nodes on page 180. 

- Ensure that you meet all Prerequisites on page 178. 

## **About this task** 

To isolate a service to a separate virtual network, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** At the top-right corner of the page, click the gear icon. 

**3.** In the left pane, click **Network Configuration** . 

**4.** In the details pane, on the **Internal Interfaces** tab, click **Create New Interface** . The **Create New Interface** dialog box is displayed. 

AOS Security | Securing Traffic Through Network Segmentation | **197** 

## **5.** On the **Interface Details** tab, follow these steps: 

- a. Specify a descriptive name for the network segment. 

- b. (Optional for AHV) Specify a **VLAN ID** . 

Make sure that the VLAN ID is configured on the physical switch. 

- c. In **Bridge** (on AHV) or **CVM Port Group** (on ESXi), select the bridge or port group that you created for the network segment. 

- d. Specify an IP address pool for the network segment, click **Create New IP Pool** , then in the **IP Pool** dialog box, follow these steps: 

   - In the **Name** field, specify a name for the pool. 

   - In the **Netmask** field, specify the network mask for the pool. 

   - Click **Add an IP Range** and specify the start and end IP addresses in the **IP Range** dialog box that is displayed. 

   - Use **Add an IP Range** to add as many IP address ranges as you need. 

**Note:** Add at least _n_ +1 IP addresses in an IP address range considering _n_ is the number of nodes in the cluster. 

- Click **Save** . 

- (Optional) Use **Add an IP Pool** to add more IP address pools. You can use only one IP address pool at any given time. 

- Select the IP address pool that you plan to use and then click **Next** . 

You can also use an existing unused IP address pool. 

## **6.** On the **Feature Selection** tab, follow these steps: 

- a. Select the service whose traffic you plan to isolate. 

- b. Configure the settings for the selected service. 

The setting shown on this page vary based on the service that you select. For information about servicespecific settings, see Service-Specific Settings and Configurations on page 200. 

## c. Click **Save** . 

You cannot enable network segmentation for multiple services at the same time. Complete the configuration for one service before you enable network segmentation for another service. 

**7.** In the **Create Interface** dialog box, click **Save** . 

The CVMs are rebooted multiple times, one after another. This procedure might trigger more tasks on the cluster. For example, if you configure network segmentation for disaster recovery, the firewall rules are added on the CVM to allow traffic on the specified ports through the new CVM interface and updated when a new recovery cluster is added or an existing cluster is modified. 

## **What to do next** 

See Service-Specific Settings and Configurations on page 200 for any additional tasks that are required after you segment the network for a service. 

AOS Security | Securing Traffic Through Network Segmentation | **198** 

## **Modifying Network Segmentation Configured for a Service** 

To modify network segmentation configured for a service, you must first disable network segmentation for that service, then create the network interface again for that service with the new IP address pool and VLAN. 

## **About this task** 

For example, if the interface of the service you want to modify is `ntnx0` , after the reconfiguration, the same interface ( `ntnx0` ) is assigned to that service if that interface is not assigned to any other service. If `ntnx0` is assigned to another service, a new interface (for example `ntnx1` ) is created and assigned to that service. 

To reconfigure network segmentation configured for a service, follow these steps: 

## **Procedure** 

**1.** Disable the network segmentation configured for a service. 

For instructions, see Disabling Network Segmentation Configured for a Service on page 199. 

**2.** Create the network interface again for that service with the new IP address pool and VLAN. For instructions, see Isolating Service-Specific Traffic on page 197. 

## **Disabling Network Segmentation Configured for a Service** 

To disable network segmentation configured for a service, you must disable the dedicated vNIC. Disabling network segmentation frees the name of the vNIC. The free name is reused in a subsequent network segmentation configuration. 

## **About this task** 

To disable the network segmentation configured for a service, perform the following steps: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** At the top-right corner of the page, click the gear icon. 

**3.** In the left pane, click **Network Configuration** . 

**4.** On the **Internal Interfaces** tab, for the interface that you plan to disable, click **Disable** . 

The defined IP address pool is available even after disabling the network segmentation. 

## **What to do next** 

At the end of this procedure, the cluster performs a rolling restart. Disabling network segmentation disrupts the functioning of the associated service. To restore normal operations, you may need to perform additional tasks immediately after the rolling restart. For information about the follow-up tasks, see Service-Specific Settings and Configurations on page 200. 

## **Deleting a vNIC Configured for a Service** 

If you disable network segmentation for a service, the vNIC for that service is not deleted. AOS reuses the vNIC if you enable network segmentation again. However, you can manually delete a vNIC by logging into any CVM in the cluster with SSH. 

## **Before you begin** 

Ensure that you meet the following pre-requisites before you delete the vNIC configured for a service: 

AOS Security | Securing Traffic Through Network Segmentation | **199** 

- Network segmentation is disabled for a service. For instructions, see Disabling Network Segmentation Configured for a Service on page 199. 

- You observe the limitations specified in Limitation for vNIC Hot-Unplugging topic in _AHV Admin Guide_ . 

## **About this task** 

To delete a vNIC that is configured for a service, follow these steps: 

## **Procedure** 

**1.** Log in to any CVM in the cluster with SSH. 

**2.** Delete the vNIC: 

`nutanix@cvm$ network_segmentation --service_network --interface="` _`interface-name`_ `" -- delete` 

Replace _`interface-name`_ with the name of the interface you plan to delete. For example, `ntnx0` . 

## **Service-Specific Settings and Configurations** 

Settings required by the services that support network segmentation. 

Prism Element supports network segmentation for specific services to isolate traffic and improve security. For Nutanix Volumes, you can configure multiple iSCSI networks and migrate client connections to segmented networks. Disaster recovery configurations also support segmentation, including stretched layer 2 networks. Each service requires specific parameters such as virtual IP address, client subnet, and gateway. 

## **Nutanix Volumes** 

Network segmentation for Volumes also requires you to migrate iSCSI client connections to the new segmented network. If you no longer require segmentation for Volumes traffic, you must also migrate connections back to `eth0` after disabling the vNIC used for Volumes traffic. 

You can create up to two different networks for Nutanix Volumes with different IP address pools, VLANs, and data services IP addresses. For example, you can create two iSCSI networks, one for production and one for nonproduction traffic, on the same Nutanix cluster. 

Follow the instructions in Isolating Service-Specific Traffic on page 197 again to create the second network for Volumes after you create the first network. 

## **Settings to be Specified When Configuring Traffic Isolation** 

When configuring the traffic isolation, specify the following settings: 

- Virtual IP: This is an optional setting. Specifies a Virtual IP address for the service. If specified, select the IP address from the specified IP address pool. If you do not specify an IP address, the system selects a random IP address from the specified IP address pool. 

AOS Security | Securing Traffic Through Network Segmentation | **200** 

- Client subnet: The network (in CIDR notation) that hosts iSCSI clients. Required only if clients are outside the subnet of the iSCSI segment. 

You can specify multiple client subnets while configuring network segmentation for Volumes in the CLI from a CVM. 

To update client subnets for network segmentation for Volumes segmentation: 

`nutanix@cvm$ network_segmentation --service_network --update --interface="` _`interface_name`_ `" --service_name="kVolumes" -- client_subnets="` _`existing_subnet`_ `,` _`new_client_subnet`_ `"` 

For example: 

`nutanix@cvm$ network_segmentation --service_network --update --interface="ntnx0" -- service_name="kVolumes" --client_subnets="10.2.2.0/24,10.2.3.0/24"` 

> For more information on using the CLI, run the ~~a~~ `nutanix@cvm$ network_segmentation --help` command. 

You can specify only one client subnet while configuring network segmentation for Volumes in the Prism Element web console. 

- Gateway: Specifies the gateway to the subnetwork that hosts the iSCSI clients. Required if you specify the client subnet. 

## **Migrating iSCSI Connections to the Segmented Network** 

After you enable network segmentation for Volumes, you must manually migrate connections from existing iSCSI clients to the newly segmented network. Even though support is available to run iSCSI traffic on both the segmented and management networks at the same time, Nutanix recommends that you move the iSCSI traffic for guest VMs to the segmented network to achieve true isolation. 

## **Before you begin** 

Ensure that the task for enabling network segmentation for the service succeeds. 

## **About this task** 

To migrate iSCSI connections to the segmented network, perform the following steps: 

## **Procedure** 

**1.** Log out of all the clients connected to iSCSI targets that are using CVM `eth0` ~~—~~ or the data service IP address. 

**2.** (Optional) Remove all the discovery records for the data services IP address (DSIP) on `eth0` interface. 

**3.** If the clients are allowlisted by their IP address, remove the client IP address that is on the management network: 

`nutanix@cvm$ acli vg.detach_external` _`vg_name`_ `initiator_network_id=` _`old_vm_IP`_ `nutanix@cvm$ acli vg.attach_external` _`vg_name`_ `initiator_network_id=` _`new_vm_IP`_ 

> Replace Le _`vg_name`_ with the name of the volume group and _`old_vm_IP`_ st and _`new_vm_IP`_ with the old and new client IP addresses, respectively. 

**4.** Discover the virtual IP address specified for Volumes. 

**5.** Connect to the iSCSI targets from the client. 

## **Migrating Existing iSCSI Connections to the Management Network** 

If you no longer require segmentation for Volumes traffic, you must migrate connections back to the controller VM — `eth0` after disabling the vNIC used for Volumes traffic. 

AOS Security | Securing Traffic Through Network Segmentation | **201** 

## **About this task** 

To migrate existing iSCSI connections to `eth0` , follow these steps: 

## **Procedure** 

**1.** Log out of all the clients connected to iSCSI targets using the CVM vNIC dedicated to Volumes. 

**2.** Remove all the discovery records for the data service IP address (DSIP) on the new interface. 

**3.** Discover the DSIP for `eth0` . 

**4.** Connect the clients to the iSCSI targets. 

## **Disaster Recovery with Protection Domains** 

The settings for configuring network segmentation for disaster recovery apply to all asynchronous, nearSync, and metro availability replication schedules. 

You can use disaster recovery with asynchronous, nearSync, and metro availability replications only if you have configured network segmentation for both the primary site and the recovery site. Before enabling or disabling the network segmentation on a host, make sure to disable all disaster recovery replication schedules that are currently running on that host. 

When configuring the traffic isolation, specify the following settings: 

- Virtual IP: This is an optional setting. Specifies a Virtual IP address for the service. If specified, select the IP address from the specified IP address pool. If you do not specify an IP address, the system selects a random IP address from the specified IP address pool. 

- Gateway: Gateway to the subnetwork (subnet). 

Consider the following points when configuring network segmentation for disaster recovery: 

- The Prism Central web console does not allow you to configure network segmentation for DR without a gateway. Therefore, you must use the CLI method. 

- While configuring network segmentation for L2 stretched DR using the CLI, specifying a gateway is optional if you use the `stretched_metro` CLI option. 

- When you configure network segmentation for a network which is intended as a stretched Layer 2 network across AOS clusters without a gateway using CLI, disaster recovery (DR) is limited to AOS clusters that are within the same Layer 2 broadcast domain. 

## **Remote Site Configuration** 

After configuring network segmentation for disaster recovery, configure remote sites at both locations. You also need to reconfigure remote sites if you disable network segmentation. 

For information about configuring remote sites, see Remote Site Configuration in the _Protection Domain-Based Disaster Recovery with Prism Element Guide._ 

## **Segmenting a Stretched Layer 2 Network for Disaster Recovery** 

A stretched layer 2 network configuration allows the source and remote metro clusters to be in the same broadcast domain and communicate without a gateway. 

## **Before you begin** 

Create an IP address pool for DR. 

For information on how to create an IP pool, see Configuring Backplane IP Pool on page 191. 

AOS Security | Securing Traffic Through Network Segmentation | **202** 

## **About this task** 

You can enable network segmentation for disaster recovery on a stretched layer 2 network that does not have a gateway. A stretched layer 2 network is usually configured across physically remote clusters, such as a Metro Availability cluster deployment. A stretched layer 2 network allows the source and remote clusters to be configured in the same broadcast domain without the usual gateway. 

See _AOS Release Notes_ for minimum AOS version required to configure a stretched layer 2 network. 

To configure a network segment as a stretched L2 network, perform the following steps: 

## **Procedure** 

**1.** Log in to any CVM in the cluster using SSH. 

**2.** Based on your hypervisor, run the following command: 

   - On a cluster running ESXi hypervisor: 

`nutanix@cvm$ network_segmentation --service_network --service_name=kDR -- ip_pool=` _`DR-ip-pool-name`_ `--service_vlan=` _`DR-vlan-id`_ `--desc_name=` _`Description`_ `-host_physical_network=` _`portgroup`_ `--stretched_metro` 

- On a cluster running AHV hypervisor: 

`nutanix@cvm$ network_segmentation --service_network --service_name=kDR -- ip_pool=` _`DR-ip-pool-name`_ `--service_vlan=` _`DR-vlan-id`_ `--desc_name=` _`Description`_ `--host_virtual_switch=` _`virtualswitch`_ `--stretched_metro` 

Replace the following: 

- _`DR-ip-pool-name`_ es with the name of the **IP Pool** created for the DR service or any existing unused IP address pool. 

- _`DR-vlan-id`_ Le with the **VLAN ID** used for the DR service. 

- _`Description`_ ~~a~~ with a suitable description of this stretched L2 network segment. 

- _`portgroup`_ ~~a~~ with the details of the **CVM Port Group** used for the DR service in the ESXi hypervisor. 

- _`virtual-switch`_ ~~a~~ with the details of the **Virtual Switch** used for the DR service in the AHV hypervisor. 

- The following example shows how to configure network segment as a stretched L2 network on a cluster running ESXi hypervisor: 

`nutanix@cvm$ network_segmentation --service_network --service_name=kDR -- ip_pool=DR_pool --service_vlan=124 --desc_name="L2 Strech for ESXi" -- host_physical_network=portgroup0 --stretched_metro` 

The following example shows how to configure network segment as a stretched L2 network on a cluster running AHV hypervisor: 

`nutanix@cvm$ network_segmentation --service_network --service_name=kDR -- ip_pool=DR_pool --service_vlan=124 --desc_name="L2 Strech for AHV" --host_virtual_switch=vs0 --stretched_metro` 

For more information about the network_segmentation command, see the Command Reference guide. 

AOS Security | Securing Traffic Through Network Segmentation | **203** 

## **Nutanix Disaster Recovery** 

The settings for configuring network segmentation for Nutanix Disaster Recovery apply to all asynchronous, nearSync, and synchronous replication schedules. For more information, see _Network Segmentation_ in the _Nutanix Disaster Recovery Guide_ . 

When configuring the traffic isolation, specify the following settings: 

- Virtual IP: This is an optional setting. Specifies a Virtual IP address for the service. If specified, select the IP address from the specified IP address pool. If you do not specify an IP address, the system selects a random IP address from the specified IP address pool. 

Virtual IP address is different from the external IP address and the data services IP address of the cluster. 

- Gateway: Specifies the gateway to the subnetwork 

## **Remote Direct Memory Access over Converged Ethernet** 

Remote Direct Memory Access (RDMA), RDMA port pass-through mechanism, and how to configure the RDMA network segmentation using Zero-Touch RoCE (ZTR) or Priority-Based Flow Control (PFC) mechanism. 

## **RDMA Overview** 

Remote Direct Memory Access (RDMA) directly transfers data between multiple hosts without involving the CPU, OS, or system cache. RDMA reduces communication latency and increases the bandwidth output for data transfer. It directly uses the network adapters for data transfer and never creates a data copy between network layers. 

When RDMA is enabled, the CPU resources available for other applications running in the cluster enhance the AOS data acceleration mechanism. 

## **RDMA Port Pass-through Mechanism** 

The following table describes the supported RDMA port pass-through mechanisms: 

**Table 13: Supported RDMA Port Pass-through Mechanism** 

|**AOS release**|**Hypervisor**|**Hypervisor**|**RDMA Port Pass-through Imaging**|
|---|---|---|---|
|Prior to AOS 6.6|•|AHV|During Foundation imaging with RDMA<br>pass#through enabled, the Controller VM (CVM)|
||•|ESXi|reserves the entire NIC for RDMA traffic. In this|
||||configuration, the host system cannot use any|
||||remaining NIC ports for other operations, nor|
||||can it change the selected port. If the NIC or port|
||||experiences a failure or connectivity issue, I/O|
||||traffic automatically falls back to TCP/IP mode.|



AOS Security | Securing Traffic Through Network Segmentation | **204** 

|**AOS release**|**AOS release**|**Hypervisor**|**RDMA Port Pass-through Imaging**|
|---|---|---|---|
|AOS 6.6 and later|AOS 6.6 and later|AHV|RDMA port pass-through behaviour during|
||||Foundation setup or after you complete cluster|
||||creation.|
||||During Foundation setup, when RDMA port|
||||pass#through is enabled during Foundation imaging,|
||||the CVM reserves the entire NIC for RDMA. At this|
||||stage, the user cannot select a specific port—only the|
||||pass#through option is available. The host cannot use|
||||the unused NIC port for other operations, nor change|
||||the selected ports.|
||||After cluster creation, a manageability feature allows|
||||you to select port. Administrators can choose which|
||||NIC port to use for RDMA pass#through, providing|
||||flexibility in case of port preferences or hardware|
||||considerations.|
||||**Note:**Ensure the port that you reserve for RDMA|
||||is not allocated to the uplink vswitch of the host.|
||||If the port that you want to reserve for RDMA is|
||||already allocated for uplink vswitch, the system|
||||displays it as disabled for selection and you need|
||||to manually remove it from the vswitch.|
||||•<br>Failure handling: If the NIC or selected port fails,|
||||or if any connectivity issue occurs, I/O traffic|
||||automatically falls back to TCP/IP mode.|
|||ESXi|If RDMA pass#through is enabled during|
||||Foundation imaging, the entire NIC is reserved|
||||by CVM, system does not provide an option to|
||||change the RDMA port during imaging or after|
||||cluster creation.|



For more information, see Configuring Foundation VM by Using the Foundation GUI and RDMA Supported Platforms in _NX Series Hardware Administration Guide_ . 

## **Node Upgrade Case** 

When you add RDMA-capable NICs to an existing node after the cluster is already created and you plan to configure RDMA, follow the instructions provided in the KB 17964 . 

## **Cluster Expansion Case** 

When you add a new node to increase the capacity of a cluster, you cannot change the RDMA ports after the cluster is created. In this situation, the system checks whether RDMA is enabled in the cluster. If RDMA is enabled, the system provisions RDMA for the new node and selects the first available port in the NIC. To change the RDMA port, you must disable RDMA for the entire cluster, then reconfigure it with the new port selection. 

## **ZTR Specifications** 

ZTR is a deployment mechanism for RDMA setup in which the Mellanox NIC firmware handles the entire configurations (card optimizations) without any user intervention or dependency on the customized switch profiles or switch compatibility. ZTR reduces RDMA deployment duration and does not require the support of PFC and end-toend congestion notification (ECN) settings. 

AOS Security | Securing Traffic Through Network Segmentation | **205** 

ZTR functionality is supported with both ESXi and AHV hypervisors. For information about how to enable ZTR for RDMA network segmentation, see Isolating the Backplane Traffic on an Existing RDMA Cluster on page 209. 

Nutanix recommends that you use ZTR only if NVIDIA Mellanox Connect X-5 Ethernet adapters (Cx5 NICs) or NVIDIA Mellanox Connect X-6 Ethernet adapters (Cx6 NICs) are available in your setup. For more information about the NICs' compatibility for ZTR feature, see NIC Compatibility Matrix for RDMA Features on page 206. 

## **Priority-Based Flow Control without Zero-Touch RoCE Specifications** 

The following are the RDMA switch configurations you must perform when using datacenter bridging (DCB) or priority flow control mode (PFC) without ZTR: 

- Ensure that the switches are DCB capable and that DCB or PFC is enabled. 

- Ensure that switch ports mapped to servers are configured with DCB or PFC. 

- Ensure that when you are configuring DCB with Enhanced Transmission Selection (ETS), the sum of all traffic class (TC) bandwidth allocations equals 100%. The bandwidth percentages are assigned across TCs according to workload requirements, such as RDMA#heavy, storage#heavy, or balanced. The exact ETS bandwidth allocation for RDMA traffic is use#case dependent and must follow the switch vendor’s specific recommendations. 

- Connect the RDMA-supported server NIC port to the switch. 

- Configure the connected switch port to allow the selected VLAN. 

- Disable send and receive flow control on the switch port. 

- Enable priority flow control mode (PFC) with priority 3 or priority 4. 

For more information on how to configure these settings, see to the switch vendor documentation or contact the switch vendor for support. 

RDMA switches using PFC with ZTR do not require additional configurations as described before, because the NICs negotiate directly with each other. 

For information on how to configure the RDMA network segmentation using either PFC or ZTR from Prism Element, see Isolating the Backplane Traffic on an Existing RDMA Cluster on page 209. 

## **NIC Compatibility Matrix for RDMA Features** 

The following table provides the information about NIC compatibility with RDMA features, RDMA Port Pass-through and ZTR, and the required workaround: 

**Table 14: NIC Compatibility Matrix for RDMA Features** 

|**NIC Available at Site**|**RDMA Feature**|**Compatibility**|**Workaround**|
|---|---|---|---|
|||**Information**||
|NVIDIA Mellanox<br>ConnectX-4 Ethernet|ZTR|Not supported|None|
|Adapter (CX-4)||Nutanix recommends that<br>you use ZTR only if CX-5||
|||NICs are available in your||
|||setup.||
||RDMA port pass-through|Not supported after|Perform RDMA port|
|||cluster creation.|pass-through during|
||||Foundation setup, then|
||||upgrade to AOS 6.6|
||||release.|



AOS Security | Securing Traffic Through Network Segmentation | **206** 

|**NIC Available at Site**|**RDMA Feature**|**Compatibility**|**Compatibility**|**Workaround**|
|---|---|---|---|---|
|||**Information**|||
|NVIDIA Mellanox|ZTR|Supported||None|
|ConnectX-5 Ethernet|||||
|Adapter (CX-5)|RDMA port pass-through|Supported||None|
|||The RDMA port pass-|||
|||through is supported for the|||
|||following scenarios:|||
|||•|During Foundation||
||||imagining setup:||
||||•<br>The entire NIC is||
||||reserved.||
||||•<br>Ports are configured||
||||for RDMA and||
||||iSER during||
||||imaging.||
||||•<br>The host cannot||
||||use the reserved||
||||NIC ports for other||
||||operations, and the||
||||selected port cannot||
||||be changed.||
|||•|After cluster creation||
||||(AHV only). Supported||
||||exclusively for AHV||
||||environments.||



AOS Security | Securing Traffic Through Network Segmentation | **207** 

|**NIC Available at Site**|**RDMA Feature**|**Compatibility**|**Compatibility**|**Workaround**|
|---|---|---|---|---|
|||**Information**|||
|NVIDIA Mellanox|ZTR|Supported||None|
|ConnectX-6 Ethernet|||||
|Adapter (CX-6)|RDMA port pass-through|Supported||None|
|||The RDMA port pass-|||
|||through is supported for the|||
|||following scenarios:|||
|||•|During Foundation||
||||imagining setup:||
||||•<br>The entire NIC is||
||||reserved.||
||||•<br>Ports are configured||
||||for RDMA and||
||||iSER during||
||||imaging.||
||||•<br>The host cannot||
||||use the reserved||
||||NIC ports for other||
||||operations, and the||
||||selected port cannot||
||||be changed.||
|||•|After cluster creation||
||||(AHV only). Supported||
||||exclusively for AHV||
||||environments.||



AOS Security | Securing Traffic Through Network Segmentation | **208** 

|**NIC Available at Site**|**RDMA Feature**|**Compatibility**|**Compatibility**|**Workaround**|
|---|---|---|---|---|
|||**Information**|||
|NVIDIA Mellanox|ZTR|Supported||None|
|ConnectX-7 Ethernet|||||
|Adapter (CX-7)|RDMA port pass-through|Supported||None|
|||The RDMA port pass-|||
|||through is supported for the|||
|||following scenarios:|||
|||•|During Foundation||
||||imagining setup:||
||||•<br>The entire NIC is||
||||reserved.||
||||•<br>Ports are configured||
||||for RDMA and||
||||iSER during||
||||imaging.||
||||•<br>The host cannot||
||||use the reserved||
||||NIC ports for other||
||||operations, and the||
||||selected port cannot||
||||be changed.||
|||•|After cluster creation||
||||(AHV only). Supported||
||||exclusively for AHV||
||||environments.||



You can have a mix of NIC cards across nodes in a cluster. In this case, the oldest family of cards dictates the feature supported for the cluster. For example, if any CX-4 NIC is present, the system blocks the RDMA live port passthrough (RDMA port pass-through after cluster creation) and ZTR functions, and allows only RDMA data replication. 

**Important:** You cannot have a mix of RDMA NIC families on the same node. For example, you cannot have a combination of NVIDIA Mellanox ConnectX-4 Ethernet adapter (CX-4 NIC) and NVIDIA Mellanox ConnectX-5 Ethernet adapter (CX-5 NIC) or a combination of CX-5 NICs with different speeds such as CX-5 10Gb NIC and CX-5 25Gb NIC, on the same node. If you have any of these combination types of RDMA NIC families on the same node, the system never allows you to enable RDMA on the cluster. 

## **Isolating the Backplane Traffic on an Existing RDMA Cluster** 

Configure the RDMA network segmentation settings using either PFC or ZTR from Prism Element. 

## **About this task** 

The network segmentation process creates a separate network for RDMA communications on the existing default virtual switch and places the rdma0 interface (created on the CVMs during upgrade) and the host interfaces on the newly created network. From the specified subnet, IP addresses are assigned to each new interface. Two IP addresses are therefore required per node. If you specify the optional VLAN ID, the newly created interfaces are placed on the VLAN. A separate VLAN is highly recommended for the RDMA network to achieve true segmentation. 

AOS Security | Securing Traffic Through Network Segmentation | **209** 

## **Before you begin** 

Ensure that the following prerequisites are met before you proceed with RDMA network segmentation: 

- You must specify a non-routable subnet. The interfaces on the backplane network are automatically assigned IP addresses from the subnet. You reserve the entire subnet for the backplane network alone. 

- If you plan to specify a VLAN for the RDMA network, ensure that the VLAN is configured on the physical switch ports to which the nodes are connected. 

- For information on prerequisites related to switch configuration when using datacenter bridging (DCB) or priority flow control mode (PFC) without ZTR, see Priority-Based Flow Control without Zero-Touch RoCE Specifications on page 206. 

- Observe the NICs compatibility information specified in NIC Compatibility Matrix for RDMA Features on page 206. 

- Ensure that the cluster does not have a mixed configuration where some nodes have RDMA port pass-through or ZTR enabled while other nodes have it disabled, such mix configuration is not supported. You must configure all nodes in the cluster uniformly with the RDMA functionality. 

The RDMA configuration option is disabled if any of the following conditions exist in your setup: 

- All the nodes in the cluster do not contain at least two RDMA-enabled NIC cards. 

- A node contains a mix of RDMA NIC families. Examples include: 

   - A combination of NVIDIA Mellanox ConnectX-4 Ethernet Adapter (CX-4) and NVIDIA Mellanox ConnectX-5 Ethernet Adapter (CX-5). 

   - A combination of NVIDIA Mellanox ConnectX-5 Ethernet Adapters (CX-5) with different speeds, such as CX-5 10Gb and CX-5 25Gb. 

   - A combination of NVIDIA Mellanox ConnectX-6 Ethernet Adapters (CX-6) with different speeds, such as CX-6 25Gb and CX-6 100Gb. 

   - A combination of NVIDIA Mellanox ConnectX-7 200G Ethernet Adapter (CX-7) and CX-6 Ethernet Adapters at different speeds, such as CX-6 25Gb and CX-6 100Gb. 

For more information, see NIC Compatibility Matrix for RDMA Features on page 206. 

The following prerequisites are applicable only for ZTR: 

- NVIDIA Mellanox ConnectX-5 Ethernet Adapters (CX-5 NICs), NVIDIA Mellanox Connect X-6 Ethernet Adapters (CX-6 NICs), or NVIDIA Mellanox ConnectX-7 Ethernet Adapters (CX-7 NICs) are supported. For details, see NIC Compatibility Matrix for RDMA Features on page 206. 

- NICs on all nodes are running the same NIC firmware. Perform a NCC health check to verify the minimum driver version recommended and supported by Mellanox NIC running on the Nutanix platforms. For information about how to perform NCC health check, see KB 4289 . 

- AHV or ESXi hypervisor and AHV 6.6 or later are deployed for the cluster. For information about supported AHV or ESXi hypervisor versions, see KB 4289 . 

## **Procedure** 

To isolate the backplane traffic on an existing RDMA cluster, follow these steps: 

**1.** Log in to Prism Element as an administrator. 

**2.** In the top-right corner, click the gear icon, and then click **Network Configuration** in the **Settings** page. The **Network Configuration** dialog box displays. 

AOS Security | Securing Traffic Through Network Segmentation | **210** 

## **3.** Click the **Internal Interfaces** tab. 

The **Internal Interfaces** tab displays. 

**Note:** If the system detects NVIDIA Mellanox CX-5, CX-6, or CX-7 NIC, it provides you the option to configure RDMA. 

**4.** Click **Configure** in the **RDMA** row, and set the following attributes in **RDMA** dialog box: 

   - a. Under **Subnet IP** , specify a non-routable Subnet IP. 

Ensure that the subnet size can accommodate cluster expansion in the future. 

- b. Under **Netmask** , specify a non-routable netmask. 

- c. Under **VLAN** , specify a VLAN ID for the RDMA LAN. 

VLAN ID is optional with ZTR but Nutanix recommends that you use it for true network segmentation and enhanced security. 

- d. To enable ZTR, select either the **Use Zero Touch RoCE** checkbox or select the **PFC** value configured on the physical switch port. 

When you select the Use Zero Touch RoCE checkbox: 

- The PFC field is disabled. 

- The system automatically defines the RDMA port if RDMA port pass-through is done during Foundation setup. You cannot change the RDMA port in this case. 

## **5.** Click **Verify and Save** . 

The selected RDMA network segmentation settings are saved. 

If you do not perform the RDMA port pass-through set-up during Foundation setup and only AHV hypervisor is deployed, the system prompts you to define the RDMA port. 

**6.** (Optional) To define RDMA port, follow these steps: 

   - a. Click **Next** and then select the RDMA port in the **Port Selection** tab. 

   - b. Select the port to be reserved for RDMA on all the hosts. 

   - c. Click **Save** . 

## **Support for iSCSI Extensions for RDMA** 

iSCSI Extensions for RDMA (iSER) enhances traditional iSCSI by using RDMA to eliminate data copies in the data path, improving performance and reducing CPU usage. 

The Internet Small Computer Systems Interface (ISCSI) is a Storage Area Networking (SAN)-based data transfer model that uses TCP/IP to transfer the data between applications to a storage array. iSER is an extension of iSCSI that uses the direct data placement technology of the RDMA protocol to eliminate the copies in the data path. The system appropriately utilizes the CPU resources and reduces latency between AHV host and CVM I/O transfer. 

iSER provides the following key benefits: 

- Enhances I/O flow performance between the AHV host and the CVM. iSER bypasses both the AHV host and CVM kernel spaces and enables you to achieve low latency in the I/O data transfer because the data is sent and received between AHV host and CVM without the involvement of the TCP software stack. iSER enables you to achieve a high bandwidth in the I/O data transfer as it removes the unwanted context switches and system calls between AHV host and CVM. 

- Saves platform resources due to less CPU utilization because an application can read remote memory without any intervention by the remote processor. 

AOS Security | Securing Traffic Through Network Segmentation | **211** 

The following figure shows how I/O traffic flows between the AHV host and the CVM: 

## **Figure 28: I/O Traffic - AHV Host and CVM** 

The following figure shows the detailed network architecture for iSER: 

**Figure 29: iSER Network Architecture** 

The following specifications apply to the iSER architecture: 

- NIC 1 is a non-RDMA capable NIC for regular network functionality, and NIC 2 is a dual-port qualified iSER NIC for RDMA and iSER. For more information about qualified iSER NICs, see NIC Compatibility Matrix for iSER on page 214. 

- The first port on NIC 2 ( `eth2` ) is connected to the uplink switch. The second port `(eth3)` on NIC 2 is disconnected. 

- The unconnected port `eth3` can be used for iSER. 

- The connected port on NIC 2 ( `eth2` in the previous diagram) can be passed through to the CVM for RDMA replication. 

- The VF1 interface on an iSER port is consumed by Stargate using the Linux RDMA stack. 

- The VF2 interface on an iSER port is consumed by AHV Turbo using the Linux RDMA stack. 

AOS Security | Securing Traffic Through Network Segmentation | **212** 

- iSER and RDMA replication can either be enabled during cluster creation or dynamically enabled or disabled on the available ports (through the RDMA manageability workflow) after cluster creation. 

After cluster creation: 

- You can reconfigure RDMA on the `eth2` port. 

- If RDMA is reconfigured on the `eth2` port, you can reconfigure iSER on the `eth3` port. 

**Note:** Currently, iSER uses fixed internal IPs. There is no supported workflow to change iSER IPs for the CVM or the AHV host. 

The following specifications are applicable in the iSER setup: 

|**iSER Specification**|**Component**|**Description**|
|---|---|---|
|iSER initiator|AHV Turbo in AHV|AHV Turbo provid|
|||iSCSI packet pro|
|iSER target|CVM.|Stargate in CVM a|
||Stargate in CVM|transport to perform<br>data from AHV for|



Typically, AHV and CVM communicate with each other using the iSCSI protocol over the Open vSwitch TCP datapath. AHV forwards the I/O data that originates from the guest VM to the CVM, which processes the I/O data request. After the CVM completes the I/O data processing, it returns the response and data for the request to AHV. 

If iSER is enabled, the following actions occur between AHV and the CVM to establish storage connectivity: 

**1.** The system establishes a handshake connection between AHV and the CVM over TCP. 

**2.** On the TCP iSCSI login, the CVM responds to AHV with the status of RDMA as enabled or disabled and returns the target IP address for iSER if RDMA is enabled. 

**3.** If the CVM indicates that RDMA connectivity is possible, AHV closes the TCP connection and attempts to establish an RDMA connection to the CVM iSER IP. 

**4.** If the RDMA connection and TCP iSCSI login over the RDMA connection is successful, AHV submits the guest VM SCSI requests to CVM using the RDMA connection. 

In case of any failure scenarios, the system re-establishes TCP connections between AHV and the CVM through the standard vSwitch path, and the data processing is resumed between AHV and the CVM with zero disruption to the running workload. 

## **CVM Memory Requirements for iSER** 

For iSER, CVM requires a minimum of 64 GiB vRAM for optimum performance. For more information, see Controller VM (CVM) Field Specifications in _Acropolis Advanced Admin Guide_ . 

## **iSER Port-Passthrough Mechanism** 

You can perform an iSER NIC port pass-through either during the Foundation setup or after the cluster creation is complete using the Prism Element web console. 

The following behavior applies to iSER port pass-through: 

- If you perform the iSER port pass-through during Foundation setup, the CVM reserves the whole NIC for both iSER and RDMA data replication. The CVM reserves one port on the NIC for RDMA data replication and configures the other port for iSER. For more information about Foundation setup, see Configuring Foundation VM by Using the Foundation GUI in _Field Installation Guide_ . 

AOS Security | Securing Traffic Through Network Segmentation | **213** 

- If you perform the iSER port pass-through after cluster creation, you can configure iSER on one of the unconnected NIC ports. You can use the other NIC port to configure RDMA if RDMA port-passthrough is not done during Foundation setup. 

**Note:** Ensure that the port that you configure for iSER is not allocated to the uplink vswitch of the host. If the port that you plan to configure for iSER is already allocated for uplink vswitch, the system displays it as disabled for selection and you need to remove it from the vswitch manually. 

## **Node Upgrade Case** 

When you add the iSER-capable NICs to a node and want to configure RDMA after the cluster is created, follow the instructions provided in the KB 17964 . 

## **Cluster Expansion Case** 

When a new node is added to increase the capacity of a cluster, the iSER ports cannot be changed after the cluster is created. In this case, the system checks if iSER is enabled in the cluster. If enabled, the system provisions the iSER for the new node and selects the first port available in the NIC. To change the iSER port, disable the iSER for the whole cluster and reconfigure it with the new port selection. 

## **NIC Compatibility Matrix for iSER** 

The following table provides the information about NIC compatibility for iSER: 

**Table 15: NIC Compatibility Matrix for iSER** 

|**NIC Available at Site**|**Number of NICs**|**iSER Port Pass-**|**Workaround**|
|---|---|---|---|
||**Required**|**through Compatibility**||
|NVIDIA Mellanox<br>ConnectX-4 Ethernet|Not Applicable|Not supported|None|
|Adapter (CX-4)||||



AOS Security | Securing Traffic Through Network Segmentation | **214** 

|**NIC Available at Site**|**Number of NICs**|**iSER Port Pass-**|**iSER Port Pass-**|**Workaround**|
|---|---|---|---|---|
||**Required**|**through Compatibility**|||
|NVIDIA Mellanox<br>ConnectX-5 Ethernet|Two dual-port NICs for<br>RDMA + iSER|Supported from AOS 6.7<br>onward||None|
|Adapter (CX-5)||The iSER port pass-through|||
|||is supported for both the|is supported for both the||
|||following scenarios:|||
|||•|During Foundation||
||||imaging step the entire||
||||NIC is reserved for both||
||||iSER and RDMA data||
||||replication.||
|||•|After cluster creation||
||||for AHV only, where||
||||the system provides you||
||||an option to configure||
||||iSER on one of the||
||||unconnected NIC ports.||
||||You can use the other||
||||NIC port to configure||
||||RDMA if RDMA||
||||port-passthrough is||
||||not configured during||
||||Foundation setup.||
|NVIDIA Mellanox<br>ConnectX-6 Ethernet|Two dual-port NICs for<br>RDMA + iSER|Supported from AOS 6.7.1<br>onward||None|
|Adapter (CX-6)||The iSER port pass-|||
|||through is supported for the|||
|||following scenarios:|||
|||•|During Foundation||
||||imaging step the entire||
||||NIC is reserved for both||
||||iSER and RDMA data||
||||replication.||
|||•|After cluster creation||
||||for AHV only, where||
||||the system provides you||
||||an option to configure||
||||iSER on one of the||
||||unconnected NIC ports.||
||||You can use the other||
||||NIC port to configure||
||||RDMA if RDMA||
||||port-passthrough is||
||||not configured during||
||||Foundation setup.||



AOS Security | Securing Traffic Through Network Segmentation | **215** 

|**NIC Available at Site**|**Number of NICs**|**iSER Port Pass-**|**Workaround**|
|---|---|---|---|
||**Required**|**through Compatibility**||
|NVIDIA Mellanox|Two dual-port NICs to|Supported with AOS 7.3.|None|
|ConnectX-7 Ethernet<br>Adapter (CX-7 200G)|support iSER|Mellanox CX-7 NIC does<br>not support Foundation||
|||passthrough for iSER.||
|||Applicable scenario:||
|||After cluster creation in||
|||case of AHV environments,||
|||you must manually||
|||configure iSER on one of||
|||the unconnected NIC ports.||
|||For more information,||
|||seeConfiguring iSER||
|||Manually on Mellanox||
|||CX-7 NICon page 218.||
|NVIDIA Mellanox|Two dual-port NICs for|Supported from AOS 7.5|None|
|ConnectX-7 Ethernet|RDMA + iSER|onward.||
|Adapter (CX-7)||The iSER port pass-through||
|||is supported for both the||
|||following scenarios:||
|||•<br>During Foundation||
|||imaging step the entire||
|||NIC is reserved for both||
|||iSER and RDMA data||
|||replication.||
|||•<br>After cluster creation||
|||in case of AHV only,||
|||where the system||
|||provides you an option||
|||to configure iSER on||
|||one of the unconnected||
|||NIC ports. You can||
|||use the other NIC||
|||port to configure||
|||RDMA if RDMA port-||
|||passthrough is not done||
|||during Foundation||
|||setup.||



You can have a mix of NIC cards across nodes in a cluster. In this case, the oldest family of cards dictates the feature supported for the cluster. For example, if any CX-4 NIC is present, the system blocks the RDMA live port passthrough (RDMA port pass-through after cluster creation) and ZTR functions, and allows only RDMA data replication. 

**Important:** You cannot have a mix of RDMA NIC families on the same node. For example, you cannot have a combination of NVIDIA Mellanox ConnectX-4 Ethernet adapter (CX-4 NIC) and NVIDIA Mellanox ConnectX-5 Ethernet adapter (CX-5 NIC) or a combination of CX-5 NICs with different speeds such as CX-5 10Gb NIC and CX-5 25Gb NIC, on the same node. If you have any of these combination types of RDMA NIC families on the same node, the system never allows you to enable RDMA on the cluster. 

AOS Security | Securing Traffic Through Network Segmentation | **216** 

## **iSER Limitations** 

The following limitations apply to iSER: 

- iSER is supported with AHV only because it involves changes in I/O traffic flow between the AHV host and CVM. iSER support is not available with the ESXi hypervisor in the Nutanix environment. 

- The total number of VM disks supported per node using iSER is limited to 200 because of memory limitations, and you cannot specify or configure the VM disks for iSER acceleration. 

## **Configuring iSER Port in an Existing RDMA Cluster** 

Configure the iSER port in an existing RDMA cluster. 

## **About this task** 

iSER is enabled by default on all nodes with Mellanox CX-5, ConnectX-6 Lx, ConnectX6 Dx, and ConnectX-7 NICs on which RDMA data replication is successful. The system reserves two IP addresses for iSER. The iSER IP addresses are fixed, and during CVM restart, the system uses these reserved IP addresses for iSER. 

During AOS or AHV upgrade from previous versions, system automatically enables iSER, AHV and the CVM configures an internal IP address for iSER. 

## **Before you begin** 

Ensure that the following prerequisites are met before you configure the iSER port in an existing cluster: 

- AHV is upgraded to AHV-20230302.207 and AOS is upgraded to AOS 6.7 version. 

- Only the qualified iSER NICs; NVIDIA Mellanox dual-port CX-5, ConnectX-6 Lx, ConnectX6 Dx, and ConnectX-7 NICs are present in your setup, or else iSER functionality is disabled. For more information, see NIC Compatibility Matrix for iSER on page 214. 

- Two dual-port qualified iSER NICs are present in your setup for RDMA and iSER. For more information about qualified iSER NICs, see NIC Compatibility Matrix for iSER on page 214. 

- One non-RDMA capable NIC is present in your setup for regular network functionality. 

- The OFED drivers are qualified for iSER. For more information, see _Linux Drivers_ information on the NVIDIA website . 

- NIC firmware upgrade is done through LCM. 

- RDMA for iSER must be enabled for Virtual Function (VF). For more information, see Isolating the Backplane Traffic on an Existing RDMA Cluster on page 209. 

- An unconnected port is available on NIC for iSER traffic. 

- You observe the limitations as specified in iSER Limitations on page 217. 

## **Procedure** 

To configure iSER in an existing RDMA cluster, follow these steps: 

**1.** Log in to Prism Element as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Network Configuration** . The **Network Configuration** dialog box displays. 

AOS Security | Securing Traffic Through Network Segmentation | **217** 

## **4.** Click the **Internal Interfaces** tab. 

The **Internal Interfaces** tab displays. 

If the system detects the NVIDIA Mellanox Cx5 NIC, ConnectX-6 Lx, ConnectX6 Dx, and ConnectX-7 NICs, it provides you the option to configure iSER. 

**5.** Click **Configure** in the **iSER** row, and set the port in following attributes in **Configure iSER** page. 

If you have deployed only AHV and not configured iSER port pass-through during Foundation setup, the system allows you to define the iSER port. 

Define the iSER port: 

- a. Select the port for iSER on all the hosts. 

- b. Click **Save** . 

## **Configuring iSER Manually on Mellanox CX-7 NIC** 

By default, iSER is not enabled on any nodes with Mellanox CX-7 NICs. You must manually configure iSER on Mellanox ConnectX-7 NICs. 

## **Before you begin** 

Before you configure iSER manually on Mellanox CX-7 NIC, consider the following points: 

- AOS version 7.3 is the minimum supported version for manually configuring iSER. 

- Prism Element does not support RDMA replication factor 2. 

- Prism Element with HCI supports iSER over Mellanox ConnectX-7. 

## **About this task** 

To configure iSER manually on Mellanox CX-7 NIC, follow these steps: 

## **Procedure** 

**1.** Log in to Prism Element as an administrator. 

**2.** From the main menu, click the gear icon. 

**3.** On the left navigation bar, select **Network Configuration** . 

**4.** Unassign the uplink port from the vSwitch. 

   - For more information, see Updating a Virtual Switch . 

**5.** Select the **Internal Interfaces** tab. 

**6.** In the iSER row, click **Configure** . 

**7.** Select the NIC port that you freed from the virtual switch in Step 4 to configure iSER on all hosts. 

**8.** Click **Save** . 

## **Customizing IP Addresses for each CVM and Host** 

Create custom IP addresses for each CVM and host for network segmentation. 

## **About this task** 

IP Address customization for each CVM and host feature enables you to allocate an IP address manually to a CVM and host and helps in maintaining similarity between external and segmented IP addresses. Prism Element supports IP 

AOS Security | Securing Traffic Through Network Segmentation | **218** 

Address customization feature for configuring backplane segmentation, service-specific traffic isolation for Nutanix Volumes, and service-specific traffic isolation for disaster recovery. 

## **Procedure** 

**1.** Create a JSON file with a mapping of the CVM external IP address to the new segmented IP address. 

The segmented IP addresses must belong to the same IP address pool that is created from the Prism UI or the CLI command before starting the Network Segmentation operation. 

Example of JSON file format: 

- JSON file format for backplane segmentation: 

`{ "svmips": { `cvm_external_ip1`: `cvm_backplane_ip1`, `cvm_external_ip2`: `cvm_backplane_ip2`, `cvm_external_ip3`: `cvm_backplane_ip3` }, "hostips": { `host_external_ip1`: `host_backplane_ip1`, `host_external_ip2`: `host_backplane_ip2`, `host_external_ip3`: `host_backplane_ip3` } }` 

For example: 

`{ "svmips": { "10.47.240.141": "172.16.10.141", "10.47.240.142": "172.16.10.142", "10.47.240.143": "172.16.10.143" }, "hostips": { "10.47.240.137": "172.16.10.137", "10.47.240.138": "172.16.10.138", "10.47.240.139": "172.16.10.139" } }` 

- JSON file format for service segmentation: 

`{ "svmips": { `cvm_external_ip1`: `cvm_service_ip1`, `cvm_external_ip2`: `cvm_service_ip2`, `cvm_external_ip3`: `cvm_service_ip3` } }` 

For example: `{ "svmips": { "10.47.240.141": "10.47.6.141", "10.47.240.142": "10.47.6.142", "10.47.240.143": "10.47.6.143" } }` 

**2.** Save the JSON file in any CVM in the cluster. 

AOS Security | Securing Traffic Through Network Segmentation | **219** 

**3.** Use SSH to log on to the CVM where the JSON file is stored. 

**4.** Enable service-specific traffic isolation using the JSON file. 

For example, use the following command to enable service-specific traffic isolation for Nutanix Volumes: 

`nutanix@CVM:~$ network_segmentation --service_network --ip_pool=pool1 --desc_name="Volumes Seg 1" --service_name=kVolumes --host_physical_network=dv-volumes-network-1 --service_vlan=151 --ip_map_filepath=/home/nutanix/ip_map.json` 

## **Network Segmentation during Cluster Expansion** 

During cluster expansion, Prism Element allocates IP addresses from backplane IP address pools—two for backplane and one for service-specific isolation. Ensure sufficient IP addresses and consistent VLAN settings across nodes. If IP addresses are unavailable, expand the pool or reconfigure segmentation. 

Keep the following points in mind when expanding a cluster: 

- If you enable backplane network segmentation, Prism Element allocates two IP addresses for every new node from the backplane IP address pool. 

- If you enable service-specific traffic isolation, Prism Element allocates one IP address for every new node from the respective (Volumes or DR) IP address pools. 

- If enough IP addresses are not available in the specified network, the Prism Element web console displays a failure message in the tasks page. To add more IP address ranges to the IP address pool, see Configuring Backplane IP Pool on page 191. 

- If you cannot add more IP addresses to the IP address pool, reconfigure that specific network segmentation. For more information, see Reconfiguring the Backplane Network on page 192. 

- The network settings on the physical switch to which the new nodes are connected must be identical to those of other nodes in the cluster. New nodes communicate with current nodes using the same VLAN ID for segmented networks. Otherwise, the cluster expansion task fails in the network validation stage. 

- After fulfilling the earlier points, you can add nodes to the cluster. For instructions about how to add nodes to your Nutanix cluster, see Expanding a Cluster in the _Prism Element Web Console Guide_ . 

## **Network Segmentation-Related Changes During an AOS Upgrade** 

> During an AOS upgrade to a version supporting network segmentation, Prism Element creates — `eth2` 

> interface automatically on each CVM, but services continue using — `eth0` until you configure segmentation. 

When you upgrade from an AOS version that does not support network segmentation to an AOS version that does, the `eth2` — interface (used to segregate backplane traffic) is automatically created on each CVM. However, the network remains unsegmented, and the cluster services on the CVM continue to use `eth0` — until you configure network segmentation. 

Prism Element does not create vNICs such as `ntnx0` es , `ntnx1` , and so on during an upgrade to a AOS release that supports service-specific traffic isolation. Prism Element creates the vNICs when you configure traffic isolation for a service. 

**Note:** 

Do not delete the eth2 interface that is created on the Controller VMs, even if you are not using the network segmentation feature. 

AOS Security | Securing Traffic Through Network Segmentation | **220** 

## **ACCESSING A LIST OF OPEN SOURCE SOFTWARE RUNNING ON A CLUSTER** 

As an admin user, you can access a text file that lists all of the open source software running on a cluster. 

## **About this task** 

To access a list of the open source software running on a cluster, follow these steps: 

## **Procedure** 

**1.** SSH into any of the Controller VMs (CVMs) as an admin user: 

   - `$ ssh admin@` _`cvm_ip_address`_ 

**2.** Access the text file by running the following command: 

`admin@cvm$ less /usr/local/nutanix/license/blackduck_version_license.txt` 

AOS Security | Accessing a List of Open Source Software Running on a Cluster | **221** 

## **COPYRIGHT** 

Copyright 2026 Nutanix, Inc. 

Nutanix, Inc. 1740 Technology Drive, Suite 150 San Jose, CA 95110 

All rights reserved. This product is protected by U.S. and international copyright and intellectual property laws. Nutanix and the Nutanix logo are registered trademarks of Nutanix, Inc. in the United States and/or other jurisdictions. All other brand and product names mentioned herein are for identification purposes only and may be trademarks of their respective holders. 

AOS Security | Copyright | **222** 


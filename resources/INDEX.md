# Resources Index — Nutanix Dokumentation
<!-- Wird vom Solution Architect Agent als erstes geladen. -->
<!-- Beschreibungen kurz halten — jede Zeile zählt als Token. -->

**Letzte Aktualisierung:** 2026-06-11
**Anzahl Dokumente:** 25

---

## Best Practice Guides (BP)

| Datei | Kategorie | Wann konsultieren |
|-------|-----------|-------------------|
| `BP-2029-AHV.md` | AHV | Standard bei jedem AHV-Deployment — VM-Konfiguration, CPU/Memory, Live Migration, HA, ADS, Disk-Config, Oversubscription |
| `BP-2071-AHV-Networking.md` | Netzwerk | AHV-seitige Netzwerkkonfiguration — Virtual Switches, Bonds, VLANs für Hosts/CVMs/VMs, OVS, Load Balancing |
| `BP-2050-Physical-Networking.md` | Netzwerk | Switch-Konfiguration — VLANs, RDMA, QoS, STP, Oversubscription, Breakout Cables, Switch Fabric Design, Multisite |
| `BP-2005-Data-Protection.md` | DR / Replikation | Alle DR-Fragen — Async/NearSync Replication, Metro Availability (2-Site + Witness), Remote Backup, VSS |
| `BP-2202-NC2-AWS-Networking.md` | NC2 / AWS | NC2 auf AWS — Flow Virtual Networking, VPC, NAT, VPC Peering, Direct Connect |
| `BP-2097-SAP-HANA-on-AHV.md` | SAP HANA | SAP HANA auf AHV — VM-Parameter (q35, vcpu_hard_pin, num_vnuma_nodes), HANA-Zertifizierung, Storage-Layout, HA-Konfiguration |
| `BP-2025-SAP.md` | SAP | SAP (allgemein, nicht nur HANA) auf Nutanix — NetWeaver, S/4HANA, Sizing, Hypervisor-Anforderungen |
| `BP-2015-Microsoft-SQL-Server.md` | SQL Server | SQL Server auf Nutanix — VM-Design, Storage-Konfiguration, AlwaysOn AG, Performance-Tuning, Best Practices |
| `BP-2083-ROBO-Deployment.md` | ROBO / Edge | Remote-/Branch-Office Deployments — kleine Cluster, NCI Edge, Prism Central Remote, eingeschränkte Bandbreite |

---

## Technical Notes (TN)

| Datei | Kategorie | Wann konsultieren |
|-------|-----------|-------------------|
| `TN-2072-AHV-Migration.md` | Migration | Migration von ESXi, Hyper-V oder Public Cloud zu AHV — Move Appliance, Image Service, VM Mobility, Cluster Conversion |
| `TN-2122-Nutanix-Upgrades.md` | LCM / Upgrades | Upgrade-Prozesse — LCM Design, AOS/AHV Upgrades, 3rd-Party Hypervisor, Dark Site, Firmware |
| `TN-2010-Nutanix-Physical-Memory-Configuration.md` | Sizing | Memory-Konfiguration bei Sizings — Intel Emerald/Sapphire Rapids, Empfehlungen pro CPU-Generation |
| `TN-2028-Nutanix-Cloud-Clusters-on-AWS.md` | NC2 / AWS | NC2 auf AWS — Deployment, Storage, Migration, Backup, DR to S3, Encryption, Hibernate/Resume, ADS |
| `TN-2041-Nutanix-Files.md` | Files / Storage | Nutanix Files — Deployment, SMB/NFS, File Analytics, Smart DR, Tiering, Sizing-Grundlagen |
| `TN-2117-Nutanix-Files-Performance.md` | Files / Storage | Files Performance — Benchmarks, Tuning-Parameter, IOPS/Throughput-Richtwerte, Performance-Validierung |
| `TN-2076-SQL-Server-Migration-to-NDB.md` | NDB / SQL Server | Migration bestehender SQL Server Instanzen zu Nutanix Database Service (NDB) — Schritt-für-Schritt, Voraussetzungen |

---

## Nutanix Validated Designs (NVD) & Reference Architectures (RA)

| Datei | Kategorie | Wann konsultieren |
|-------|-----------|-------------------|
| `NVD-2155-Nutanix-Databases.md` | NDB | NDB Design — Cluster-Architektur, Storage-Design, SQL/Oracle/PostgreSQL, Backup (Time Machine), HA, Sizing |
| `NVD-2171-AOS-6-5-AHV-Unified-Storage-Design.md` | Files / Objects / Storage | Unified Storage Design — Files + Objects kombiniert, Tiering, S3-Anbindung, Dimensionierung |
| `NVD-2115-GPT-in-a-Box-6-7-Design.md` | AI / NAI / GPU | GPT-in-a-Box mit NKP — GPU-Cluster-Design, LLM-Deployment, NAI-Architektur, Kubernetes-Integration |
| `RA-2006-Database-Workloads-on-Nutanix.md` | SQL Server | SQL Server Reference Architecture — Validated Design, Sizing-Tabellen, AlwaysOn, Performance-Tests |
| `PA-2189-Nutanix-AI-Platform-Design.md` | AI / NAI / GPU | AI Platform Design Blueprint — GPU-Node-Architektur, NAI-Komponenten, Netzwerk-Design für AI-Workloads |

---

## Security

| Datei | Kategorie | Wann konsultieren |
|-------|-----------|-------------------|
| `Nutanix-Security-Guide-v7_5.md` | Security | IAM, IDP, lokale User, SecDL, Compliance, Encryption, RBAC — ab AOS 6.8 (RHEL 8 Basis) |

---

## Hardware

| Datei | Kategorie | Wann konsultieren |
|-------|-----------|-------------------|
| `Hardware-Admin-Guide.md` | Hardware | NX Series Hardware — Specs, Mixing-Restrictions, Node-Kombinationen im Cluster, IPMI, Firmware-Updates, Shutdown-Prozeduren |
| `PLATFORM_EOL_reference.md` | EOL / EOM | EoL/EoM-Daten für NX G7–G10 Plattformen — konsultieren wenn EoL-Termine für Slides A/F benötigt werden |

---

## Entscheidungshilfe für den Agent

| Projektszenario | Empfohlene Dokumente |
|-----------------|----------------------|
| Standard AHV-Deployment / hw-refresh | `BP-2029-AHV` |
| Netzwerk-Design (AHV-seitig) | `BP-2071-AHV-Networking` |
| Switch-Konfiguration | `BP-2050-Physical-Networking` |
| DR / NearSync / Metro | `BP-2005-Data-Protection` |
| Migration von VMware / Hyper-V | `TN-2072-AHV-Migration` |
| Sizing / Memory-Validierung | `TN-2010-Physical-Memory-Configuration` |
| Upgrade-Planung (LCM) | `TN-2122-Nutanix-Upgrades` |
| NC2 auf AWS | `BP-2202-NC2-AWS-Networking` + `TN-2028-NC2-on-AWS` |
| Security / Compliance / IAM | `Nutanix-Security-Guide-v7_5` |
| SAP HANA auf AHV | `BP-2097-SAP-HANA-on-AHV` + `BP-2025-SAP` |
| SAP (allgemein, non-HANA) | `BP-2025-SAP` |
| SQL Server Deployment | `BP-2015-Microsoft-SQL-Server` + `RA-2006-Database-Workloads-on-Nutanix` |
| SQL Server Migration zu NDB | `TN-2076-SQL-Server-Migration-to-NDB` + `NVD-2155-Nutanix-Databases` |
| Nutanix Files / NUS Deployment | `TN-2041-Nutanix-Files` |
| Files Performance / Sizing | `TN-2117-Nutanix-Files-Performance` |
| Unified Storage (Files + Objects) | `NVD-2171-AOS-6-5-AHV-Unified-Storage-Design` |
| AI-Cluster / NAI / GPU | `PA-2189-Nutanix-AI-Platform-Design` + `NVD-2115-GPT-in-a-Box-6-7-Design` |
| ROBO / Edge / kleine Cluster | `BP-2083-ROBO-Deployment` |
| Hardware-Mixing / Node-Kompatibilität | `Hardware-Admin-Guide` |
| EoL/EoM-Daten für Proposals | `PLATFORM_EOL_reference` |

**Regel: Maximal 3 Dokumente pro Session laden.**
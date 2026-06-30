# XYZ – Solution Proposal – Enriched
**Basis-Proposal:** 2026_06_10-XYZ-slides.md
**Angereichert am:** 10.06.2026
**Konsultierte Dokumente (lokal gelesen):**
- `TN-2028-Nutanix-Cloud-Clusters-on-AWS.md` (v2.6, Mai 2026)
- `BP-2202-NC2-AWS-Networking.md` (v1.4, März 2026)
- `BP-2005-Data-Protection.md` (v7.4, Dezember 2025)

---

## Slide A — Current Situation
**Slide-Titel:** Current Situation

### Inhalt (copy-paste in PowerPoint)

- 3-Node Nutanix Cluster (NX-8170-G8), AHV Hypervisor, Lizenz: NCI Starter
- AOS 7.5.1.2 / AHV 11.0.1.1
- 34 Guest-VMs (Windows und Linux); Workload-Klassifizierung ausstehend
- Prism Central auf demselben Produktionscluster betrieben <!-- SA: Risiko: Single-Point-of-Failure für Management-Plane — bei vollständigem Clusterausfall kein PC-Zugriff; empfohlen: PC Scale-Out oder dedizierte PC-VM auf separater Hardware (TN-2028, Abschnitt Prism Central HA) -->
- Datensicherung via Rubrik mit On-Prem S3-Archiv (Cloudian)
- Keine Cloud-Anbindung und keine offsite DR-Strategie vorhanden

---

## Slide B — Future Situation
**Slide-Titel:** Future Situation

### Inhalt (copy-paste in PowerPoint)

- Lizenz-Upgrade On-Prem: NCI Starter → NCI Pro (Voraussetzung für Multi-site DR und MST)
- **Phase 1:** Nutanix MST konfiguriert – VM-Snapshots asynchron auf AWS S3 (eu-central-2, Zürich) repliziert <!-- SA: MST nutzt Prism Central Protection Policies; RPO-Minimum bei Async: 60 Minuten (BP-2005, Replication Types); Ziel RPO = 1h ist das technische Minimum für Async — kein weiterer Puffer vorhanden -->
- **Phase 2:** 3-Node NC2-Cluster auf AWS (eu-central-2) deployed, im Normalbetrieb suspended <!-- SA: NC2 Suspend speichert Cluster-State inkl. AOS/AHV auf AWS S3; Resume-Zeit typischerweise 45–90 Minuten (Nodes starten, AOS initialisiert, Storage Fabric rebuilt) — dieser Zeitanteil ist im RTO von 2–4h einzuplanen (TN-2028) -->
- Site-to-Site VPN verbindet On-Prem-Cluster permanent mit AWS VPC (Zürich) <!-- SA: AWS S2S VPN bietet zwei redundante Tunnels (Active/Active oder Active/Passive) mit je max. 1.25 Gbps Durchsatz; kritisch für MST Snapshot-Replikation und NC2 Resume-Traffic (AWS VPN Docs) -->
- Bei On-Prem Ausfall: NC2-Cluster wird aktiviert, VMs aus MST-Snapshots auf NC2 wiederhergestellt
- NC2 Resume via AWS NC2 Console ist VPN-unabhängig — DR-Aktivierung auch bei VPN-Ausfall möglich <!-- SA: ergänzt; Quelle: TN-2028 Abschnitt 6 — Resume erfordert nur Zugang zur NC2 Console (cloud.nutanix.com), nicht zum On-Prem-Netzwerk -->
- Mitarbeiterzugriff auf NC2-Workloads via AWS Client VPN sichergestellt

---

## Slide C — Design Decisions
**Slide-Titel:** Design Decisions

### Entscheidungstabelle (copy-paste in PowerPoint als Tabelle)

| Decision Topic | Decision |
|---|---|
| On-Prem Cluster | 3-Node NX-8170-G8, RF2, NCI Pro |
| Cloud DR Cluster (AWS) | 3-Node NC2, eu-central-2 (Zürich), RF2, im Normalbetrieb suspended |
| DR-Modell & Replikation | Warm DR – MST Async (RPO = 1h), Snapshots auf AWS S3 |
| NMST Deployment-Modell | Pilot-Light: 3-Node NC2 für Tier-1 VMs (schneller Restore), S3 direkt für Tier-2 VMs (höherer RTO akzeptierbar) <!-- SA: ergänzt; Quelle: TN-2028 Abschnitt 6.3 — Pilot-Light kombiniert lokalen NVMe-Restore für kritische Workloads mit kostenoptimiertem S3-Restore für unkritische --> |
| NC2 Networking-Modell | Native Networking (kein Flow Virtual Networking) — PC läuft On-Prem, Flow VN auf NC2 würde separaten PC auf AWS erfordern <!-- SA: ergänzt; Quelle: TN-2028 Abschnitt 3 — Flow VN requires Prism Central to run on an NC2 cluster --> |
| Netzwerkanbindung | Site-to-Site VPN (AWS VPN Gateway, 2 redundante Tunnel) <!-- SA: ergänzt; Quelle: BP-2202 Abschnitt 5 — AWS S2S VPN erstellt automatisch 2 Tunnel für Redundanz --> |
| Mitarbeiterzugriff im DR | AWS Client VPN |
| NC2 Instance Type | i3en.metal (vorbehaltlich Sizing-Bestätigung) |
| DNS-Strategie im DR-Fall | AWS Route 53 Failover / On-Prem DNS-Konfiguration (Phase 2) |
| RTO | ~2–4h (NC2 Resume 45–90 Min. + VM Restore aus Snapshot; DR-Test ausstehend) |
| VM HA auf NC2 (post-Resume) | AHV HA Guarantee Mode aktivieren — reserviert RAM für 1-Node-Ausfall bei RF2 <!-- SA: ergänzt; Quelle: TN-2028 Abschnitt 10 — Guarantee Mode is non-default, muss explizit aktiviert werden; deprecated Typ kAcropolisHAReserveHosts nie verwenden --> |
| S3 VPC Gateway Endpoint | Pflicht — S3-Traffic geht über AWS-internes Netz, kein Internet-Umweg, kein Data-Transfer-Out Pricing <!-- SA: ergänzt; Quelle: TN-2028 Abschnitt 1 + BP-2202 Abschnitt 6 --> |

### Constraints & Assumptions (zwei Spalten in PowerPoint)

**Constraints – Limitations**
- MST unterstützt ausschliesslich asynchrone Replikation — kein RPO = 0 möglich
- EC2 bare metal Instanzen nicht hibernate-fähig → NC2 Cluster Suspend als Äquivalent
- Prism Central läuft auf On-Prem Cluster (Single Site, keine Alternative); bei vollständigem Clusterausfall kein PC-Zugriff
- DR-Aktivierung des NC2-Clusters erfordert manuellen Eingriff — kein automatisches Failover
- VPN ist Single Point of Failure für MST-Replikation; jede Unterbrechung führt zur sofortigen RPO-Verletzung
- NC2 Resume via AWS NC2 Console ist VPN-unabhängig; DR-Runbook muss diesen Ablauf explizit abdecken
- NC2 unterstützt ausschliesslich IPv4 — kein IPv6-Support <!-- SA: ergänzt; Quelle: BP-2202 Abschnitt 5 Limitations: "NC2 only supports IPv4" -->
- AWS-Subnetz 192.168.5.0/24 darf im VPC nicht verwendet werden — Nutanix nutzt diesen Range intern für CVM↔AHV-Kommunikation auf jedem Node <!-- SA: ergänzt; Quelle: BP-2202 Abschnitt 5 Limitations -->
- AWS-Subnetz kann nicht zwischen zwei NC2-Deployments geteilt werden — separates Subnetz pro Cluster zwingend <!-- SA: ergänzt; Quelle: TN-2028 Abschnitt 3 + BP-2202 Abschnitt 4 -->
- IP-Adressen der VMs werden beim Restore aus S3-Snapshot ohne NGT + Recovery Plan IP-Mapping nicht erhalten — VMs erhalten AWS-native IPs aus VPC-Subnetz <!-- SA: ergänzt; Quelle: BP-2202 Abschnitt 5 Limitations + BP-2005 Abschnitt 8 -->

**Assumptions**
- AWS Account wird neu erstellt und vollständig unter Kundenkontrolle gestellt
- Ausreichende Upload-Bandbreite On-Prem für stündliche MST-Snapshots vorhanden
- VPN-fähige Firewall/Router On-Prem für Site-to-Site VPN vorhanden
- Mitarbeiter werden mit AWS Client VPN-Zugängen ausgestattet
- Laufende AWS-Kosten im Suspended-Betrieb (S3 Storage) sind für Kunden akzeptabel
- On-Prem IP-Adressbereiche überschneiden sich nicht mit dem AWS VPC CIDR — Koordination mit Netzwerk-Team erforderlich <!-- SA: ergänzt; Quelle: BP-2202 Abschnitt 6 — "Coordinate with network administrator to ensure no overlapping IP addresses" -->
- NGT (Nutanix Guest Tools) wird auf allen 34 VMs installiert — Voraussetzung für IP-Erhalt bei Recovery Plan Failover <!-- SA: ergänzt; Quelle: BP-2005 Abschnitt 8: "If you don't use Nutanix AHV IPAM and need to retain your IP addresses, install NGT on the VMs you must protect" -->

---

## Slide D — Licensing Details
**Slide-Titel:** Licensing Details

### Inhalt (copy-paste in PowerPoint)

**On-Premises Cluster (Upgrade NCI Starter → NCI Pro)**

NCI – Pro
- Live Migration innerhalb des Cluster
- Distributed Storage Fabric
- Multi-site Disaster Recovery + Self-service Restore
- Cross-cluster Live Migration
- Kubernetes Platform (NKP Starter)
- Overlay Networking

**NC2 on AWS – eu-central-2 (Zürich)**

NCI – Pro
- Live Migration innerhalb des Cluster
- Distributed Storage Fabric
- Multi-site Disaster Recovery + Self-service Restore
- Cross-cluster Live Migration
- Kubernetes Platform (NKP Starter)
- Overlay Networking

---

## Slide E — Professional Services
**Slide-Titel:** Offering – Amanox Professional Service

### Inhalt (copy-paste in PowerPoint)

Angebot – Amanox Professional Service

Phase 1 – MST & VPN Setup (3 AT)
- Lizenz-Upgrade On-Prem: NCI Starter → NCI Pro
- AWS Account & S3 Bucket Setup (IAM Policies, Bucket-Konfiguration)
- Site-to-Site VPN Konfiguration (AWS VPN Gateway ↔ On-Prem Firewall)
- MST Konfiguration: Protection Policies & AWS S3 als Snapshot-Target
- Validierung der Snapshot-Replikation auf AWS S3

Phase 2 – NC2 Deployment & DR-Readiness (4 AT)
- AWS VPC Architektur & Setup (Subnets, Routing, Security Groups)
- NC2 Cluster Deployment: 3 Nodes, AOS Installation & Cluster-Konfiguration
- NC2 Cluster Suspend-Konfiguration
- AWS Client VPN Setup (Mitarbeiterzugriff im DR-Fall)
- DR Runbook Erstellung & Übergabe

DR Testing & Abnahme (2 AT)
- Testplan: Scope, Zeitfenster & Verantwortlichkeiten definieren
- NC2 Cluster Aktivierung aus Suspended-Zustand (Resume-Prozess durchführen)
- RTO-Messung: effektive Wiederherstellungszeit dokumentieren
- VM Restore aus MST-Snapshot (repräsentative Auswahl der Workloads)
- Netzwerk-Validierung: VPN-Konnektivität, Routing & DNS-Auflösung
- Mitarbeiterzugriff via AWS Client VPN verifizieren
- Applikations- & Serviceverfügbarkeit auf wiederhergestellten VMs prüfen
- NC2 Cluster zurück in Suspend überführen (Normalbetrieb wiederherstellen)
- DR Runbook basierend auf Testergebnissen finalisieren

Dokumentation (1 AT)
- Standard-Übergabedokumentation
- DR Runbook & Aktivierungsanleitung

---

## Slide F — Amanox Recommendations
**Slide-Titel:** Amanox Recommendations

### Inhalt (copy-paste in PowerPoint)

**Licensing – NCI Pro**
- NCI Starter schliesst Multi-site DR und MST explizit aus — Pro-Upgrade ist zwingende Voraussetzung für dieses Projekt
- NCI Pro deckt alle Anforderungen vollständig ab: MST, Multi-site DR, Cross-cluster Live Migration
- Upgrade auf NCI Ultimate nicht gerechtfertigt: Flow, Encryption und Native KMS werden in diesem Use Case nicht benötigt
- Einheitliche NCI Pro Lizenzebene für On-Prem und NC2 on AWS gewährleistet konsistenten Feature-Set auf beiden Clustern

**Hardware Sizing – NC2 on AWS (3 Nodes, i3en.metal)**
- 3-Node NC2 entspricht RF2-Standard: Ausfall eines AWS-Nodes verursacht keinen Datenverlust
- i3en.metal bietet lokalen NVMe-Storage und hohe Netzwerkbandbreite — optimal für Nutanix Storage Fabric und schnellen VM Restore aus S3
- Suspended-Betrieb minimiert laufende AWS-Kosten: im Normalbetrieb fallen ausschliesslich S3-Storage-Kosten an, keine EC2 bare metal Kosten
- AWS eu-central-2 (Zürich) gewährleistet Datenresidenz in der Schweiz und minimale Latenz zum On-Prem Cluster

**Professional Services**
- 2-Phasen-Ansatz reduziert finanzielles Risiko: MST-Validierung in Phase 1 vor NC2-Investment in Phase 2
- Amanox übernimmt vollständigen AWS-Aufbau (VPC, VPN, IAM, NC2) — Kunde benötigt keine AWS-Vorkenntnisse
- Dedizierter DR-Test mit RTO-Messung schafft Planungssicherheit und validiert das Runbook vor dem Ernstfall
- DR Runbook ermöglicht dem Kunden eigenständige DR-Aktivierung ohne Amanox-Unterstützung im Notfall

### Zusätzliche Empfehlungen vom Agent
> Folgende Punkte könnten noch relevant sein — zur Übernahme oder zum Verwerfen:
> - **NCM Starter:** Mit On-Prem und NC2 betreibt der Kunde neu zwei Nutanix-Cluster; NCM Starter ermöglicht zentrales Capacity Reporting und Automation über beide Umgebungen
> - **Rubrik / MST Konvergenz prüfen:** MST und Rubrik decken teilweise denselben Use Case ab (offsite Archivierung); langfristig könnte MST die Rubrik-Archivierung auf Cloudian konsolidieren und die Backup-Architektur vereinfachen

---

## Technical Architecture Notes

### 1. NC2 Hibernate/Resume — Mechanismus und RTO-Implikationen

**Quelle: TN-2028, Abschnitt 6 "Hibernate and Resume"**

Der Hibernate-Prozess läuft in folgenden Phasen ab (exakt aus TN-2028):
1. NC2 prüft, dass keine VMs, Upgrades oder laufende Cluster-Workflows aktiv sind
2. NC2 erstellt mindestens einen S3-Disk pro Node/CVM — bildet einen Cloud-Storage-Tier
3. Alle Hosts werden in Maintenance Mode versetzt; Curator und Stargate migrieren Extent-Store-Daten und Cassandra-Metadaten nach S3 (RF1 während Hibernation)
4. EBS-Volumes der CVM-Boot-Disks werden als Snapshot gesichert (Zeus-Konfiguration + Cluster-State)
5. Bare-metal Nodes werden an den AWS-Pool zurückgegeben

Der Resume-Prozess (aus TN-2028, Abschnitt 6):
1. NC2 deployed neue EC2 Instanzen und attached Cloud-Disks
2. Cluster-Konfiguration wird aus EBS-Volume-Snapshot wiederhergestellt (Zeus)
3. Genesis startet Cluster-Services
4. Cassandra Dynamic Ring Changer stellt Metadaten wieder her
5. NC2 erstellt mindestens einen S3-Disk pro Node/CVM als Cloud-Storage-Tier
6. Curator startet und schedult kSelectiveClusterHibernate-Scan für Migration Extent-Store S3 → NVMe
7. Genesis wechselt zu kNormal Mode erst wenn alle Daten migriert sind
8. NC2 markiert hibernated Disks als to-remove

**RTO-Konsequenz:** Der NC2-Resume-Prozess erfordert Internet-Zugang zur NC2 Console (`cloud.nutanix.com`). Dieser Zugang muss VPN-unabhängig sichergestellt sein — z.B. via AWS NAT Gateway + Internet Gateway. Das DR-Runbook muss den Resume-Ablauf über die NC2 Console beschreiben, nicht über Prism, da Prism während des Resume-Prozesses noch nicht verfügbar ist.

**Testwert aus TN-2028 (Abschnitt 6.2, DR to S3 Throughput-Test):**
Bei einem 3-Node i3.metal Cluster mit 18 vDisks × 250 GB (= 9.74 TB total):
- Metadata migration: 34 Minuten
- Data migration: 59 Minuten
- Total AOS processing: 1h 38 Min.
- Total NC2 console processing: 1h 50 Min.

Diese Werte sind ein realistischer Richtwert für das RTO-Fenster bei der VM-Restore-Phase.

---

### 2. NMST Deployment-Modell: Pilot-Light

**Quelle: TN-2028, Abschnitt 6.3 "Pilot-Light Deployment"; BP-2005, Abschnitt 5**

Das gewählte Szenario (3-Node NC2 suspended + MST auf S3) entspricht dem **Pilot-Light Deployment Model** gemäss TN-2028:

> "Pilot-light deployment occurs when the NMST service runs on an NC2 deployment with at least three nodes. NMST redirects the AOS snapshots to S3. You can recover snapshots to the NC2 deployment or back to the primary site if a healthy Nutanix cluster is available. If you need additional space to recover the S3-based snapshots to the NC2 deployment, you can add more nodes to the cluster using the NC2 console or the NC2 console API."

**Tier-Strategie (aus TN-2028):**
- Tier-1 VMs (kritische Workloads): Snapshots direkt auf NC2 NVMe restoren → schnellste RTO
- Tier-2 VMs (unkritisch): Bleiben in S3, Restore on-demand → höherer RTO, akzeptierbar

Diese Klassifizierung der 34 VMs muss vor Phase 1 abgeschlossen sein. In Prism Central über Categories (`DR-Tier1`, `DR-Tier2`) abbilden, Protection Policies kategorie-basiert konfigurieren.

**Initiale Seed-Replikation — Bandbreiten-Formel (BP-2005, Abschnitt 5):**
```
Bandwidth needed = (RPO change rate × (1 – compression savings %)) / RPO in seconds
Beispiel (15 GB/h change rate, 30% Kompression):
(15 GB × 0.7) / 3'600 sec = ~2.9 MBps = ~23 Mbps
```
Bei limitierter Bandbreite: initiale Retention auf 3 Monate setzen und Seeding-Zeitfenster (Nacht/Wochenende) planen.

---

### 3. AWS VPC Architektur — Subnet-Anforderungen

**Quelle: BP-2202, Abschnitt 5 "Configuring NC2 on AWS — Required CIDR Subnets"**

Für das NC2-Deployment zwingend erforderliche Subnetze:

| Subnetz-Zweck | CIDR-Grösse |
|---------------|-------------|
| VPC gesamt | /23 |
| Private Management Subnet (AHV + CVM) | /24 |
| Public Subnet (Internet/NAT Gateway) | /28 |
| User VM Subnets | /16 – /25 |
| Prism Central Subnet | /28 |
| Flow Virtual Networking Subnet (falls aktiviert) | /24 |

**Kritische Einschränkungen (aus BP-2202, Abschnitt 5 Limitations):**
- `192.168.5.0/24` darf im AWS VPC nicht verwendet werden — Nutanix-intern reserviert für CVM↔AHV-Kommunikation auf jedem Node
- NC2 unterstützt nur IPv4
- AWS-Subnetz kann nicht zwischen zwei NC2-Deployments geteilt werden
- Minimum Subnet Mask für das Bare-Metal-Node-Subnetz: /25

**ENI-Management (aus TN-2028, Abschnitt 3):** Um ENI-Erschöpfung zu verhindern, ein einzelnes /23-AWS-Subnetz als Target verwenden und daraus mehrere AHV-Subnetze (/24) ableiten. Cloud Port Manager (CNC) kann mehrere AHV-Subnetze auf ein einziges ENI mappen.

**Kritisch: IP-Adress-Konflikte** (BP-2202, Abschnitt 6): "Coordinate with the network administrator and architects to ensure that on-premises and NC2 on AWS don't have any overlapping or conflicting IP addresses." — Muss vor VPC-Erstellung mit Netzwerk-Team koordiniert werden.

---

### 4. Site-to-Site VPN — Konfiguration nach BP-2202

**Quelle: BP-2202, Abschnitt 5 "Creating AWS Site-to-Site VPN Connections"**

Standard AWS S2S VPN Aufbau (aus BP-2202, exakter Workflow):
1. **Customer Gateway erstellen** mit Public IP der On-Prem Firewall, BGP ASN 65000 (Default)
2. **Virtual Private Gateway erstellen** und an NC2-VPC attachen
3. **Site-to-Site VPN Connection** erstellen: Static Routing, On-Prem-CIDRs als Static IP Prefix
4. **Route Tables im VPC aktualisieren:** Routen zu On-Prem-Subnetzen via Virtual Private Gateway
5. **On-Prem VPN-Konfiguration** herunterladen (Vendor/Platform/Software-Version wählbar)

**Best Practice (BP-2202, Abschnitt 6 "Network Connectivity Best Practices"):**
> "Use AWS Transit Gateway and AWS Direct Connect for connectivity to on-premises. You can use a VPN connection until Direct Connect is available."

Für dieses Projekt ist VPN als Produktionslösung geplant — bewusste Kosten-/Komplexitätsentscheidung, im Runbook als Risiko dokumentieren.

**Ports für MST-Replikation (BP-2005, Abschnitt 7 "Remote Site IP Address Best Practices"):**
- TCP 2009, 2020, 9440 (CVM-zu-CVM-Replikationsverkehr)
- UDP 53 (DNS)
Diese Ports müssen in den AWS Security Groups der Management Security Group offen sein.

---

### 5. NC2 Networking-Entscheid: Native Networking statt Flow Virtual Networking

**Quelle: TN-2028, Abschnitt 3; BP-2202, Abschnitt 3**

**Entscheid: Native Networking** für dieses DR-Szenario empfohlen.

Begründung aus TN-2028:
> "Running Flow Virtual Networking requires that you run Prism Central on one of your NC2 on AWS clusters."

Da Prism Central beim Kunden On-Prem läuft und im DR-Fall ausfällt, würde Flow VN auf NC2 einen eigenständigen Prism Central auf AWS erfordern — erhöht Kosten und Komplexität signifikant.

Native Networking (TN-2028, Abschnitt 3):
> "AHV runs an efficient embedded distributed network controller that integrates guest VM networking with AWS networking. AWS allocates all guest VM IP addresses from the AWS subnets in the existing VPCs."

**Kritische Konsequenz für IP-Adressen (BP-2005, Abschnitt 8; BP-2202, Abschnitt 5):**
- Mit Native Networking erhalten VMs nach Restore AWS-native IPs aus dem VPC-Subnetz — nicht automatisch die On-Prem-IPs
- Für IP-Erhalt ist **NGT auf allen 34 VMs zwingend** (BP-2005: "If you don't use Nutanix AHV IPAM and need to retain your IP addresses, install NGT on the VMs you must protect")
- Recovery Plan muss mit Offset-Based oder Custom IP-Mapping konfiguriert werden
- Slide F Formulierung "Flow Virtual Networking gewährleistet IP-Adress-Portabilität" ist für dieses Szenario **nicht korrekt** — Flow VN wird nicht eingesetzt; IP-Erhalt erfolgt über NGT + Recovery Plan Networking. Diese Formulierung sollte in Slide F angepasst werden.

---

### 6. VM High Availability auf NC2 nach Resume

**Quelle: TN-2028, Abschnitt 10 "Virtual Machine High Availability"**

Nach dem NC2-Resume muss VM HA explizit konfiguriert werden:

> "Guarantee: This nondefault configuration reserves space throughout the AHV hosts in the cluster to guarantee that all VMs can restart on other hosts in the AHV cluster during a host failure. To enable Guarantee mode, select the Enable HA Reservation checkbox in Prism Element."

**Konfigurationsparameter (aus TN-2028):**
- `num_host_failures_to_tolerate`: muss kleiner sein als der konfigurierte Storage RF — bei RF2 maximal 1
- Reservation Type: `kAcropolisHAReserveSegments` verwenden
- **Explizit nie verwenden:** `kAcropolisHAReserveHosts` (aus TN-2028: "The VM high availability reservation type kAcropolisHAReserveHosts is deprecated. Never change the VM high availability reservation type to kAcropolisHAReserveHosts.")

CLI-Befehl (aus TN-2028):
```
nutanix@CVM$ acli ha.update num_host_failures_to_tolerate=1
```

**Runbook-Implikation:** HA Guarantee Mode als Post-Resume-Schritt im DR-Runbook dokumentieren — vor VM-Restore aktivieren.

---

### 7. Storage auf NC2 (i3en.metal) und EBS-Erweiterungsoption

**Quelle: TN-2028, Abschnitt 5 "Storage for NC2 on AWS"**

Aus TN-2028:
> "The primary storage for NC2 comes from the locally attached NVMe disks. Each AWS node also consumes two AWS Nitro EBS volumes that are attached to the bare-metal node. One of those EBS volumes is used for AHV and the other for the CVM."

EBS als optionaler Zusatz-Storage:
> "When you provision the NC2 instance in the NC2 console, you can add more Nitro EBS volumes to each bare-metal node in the cluster as remote storage, scaling your storage to meet business needs without adding more bare-metal nodes."

**Einschränkung (aus TN-2028):** "the maximum storage can't be more than 20 percent of the local storage for the bare-metal node" und "If you use EBS volumes for storage, only use homogenous clusters (all the nodes must be the same type)."

**Für dieses DR-Szenario:** Initial ohne zusätzliche EBS-Volumes starten. Bei Kapazitätsmangel beim VM-Restore können EBS-Volumes über die NC2 Console on-demand hinzugefügt werden. Skalierung sollte im DR-Runbook als optionaler Schritt dokumentiert sein.

---

### 8. Prism Central Recovery im DR-Fall — 5-Schritte-Ablauf

**Quelle: TN-2028, Abschnitt 6.1 "Native Backup with Nutanix Cluster Protection"**

NMST verwendet zwei S3-Buckets (aus TN-2028):
1. **Prism Central Bucket:** Backup der PC-Konfiguration via "Prism Central Disaster Recovery"
2. **AOS Snapshot Bucket:** Native AOS Snapshots der Guest VMs via NMST

Recovery-Prozess bei vollständigem On-Prem-Ausfall (aus TN-2028, Abschnitt 6.1):
1. NC2 Console: Cluster Resume durchführen
2. `Add your Prism Central subnet and any guest VM networks to your recreated cloud cluster`
3. `Recover your Prism Central configuration from the S3 bucket`
4. `Register your Prism Central instance with the recovered cluster`
5. `Recover NMST` → dann Recovery Plan ausführen → VM Restore aus S3 Snapshots

Dieser 5-Schritte-Ablauf erklärt das RTO von 2–4 Stunden und muss im Runbook detailliert dokumentiert sein. PC-Recovery aus S3 dauert zusätzlich zur NC2-Resume-Zeit.

---

### 9. Risiko-Register

| # | Risiko | Wahrscheinlichkeit | Impact | Mitigation |
|---|--------|-------------------|--------|-----------|
| R1 | VPN-Ausfall unterbricht MST-Replikation → RPO-Verletzung | Mittel | Hoch | CloudWatch VPN TunnelState Monitoring, proaktives Alerting; 2 AWS-Tunnel-Endpoints konfigurieren |
| R2 | Prism Central nicht verfügbar bei On-Prem-Ausfall | Hoch (im DR-Szenario) | Hoch | PC regelmässig auf S3 sichern (NMST PC Bucket); Recovery Runbook dokumentiert 5-Schritte PC-Restore (TN-2028 Abschnitt 6.1) |
| R3 | NC2 Resume dauert länger als erwartet (>2h) | Mittel | Mittel | DR-Test RTO messen; EBS on-demand hinzufügen wenn Daten-Migration S3→NVMe zu langsam (TN-2028 Abschnitt 5) |
| R4 | IP-Adress-Konflikt On-Prem ↔ AWS VPC | Mittel | Hoch | IP-Adressbereiche vor VPC-Erstellung koordinieren; 192.168.5.0/24 ist Nutanix-intern reserviert (BP-2202 Abschnitt 5) |
| R5 | VMs erhalten falsche IPs nach Restore (keine IP-Portabilität) | Mittel | Hoch | NGT auf allen 34 VMs installieren; Recovery Plan IP-Mapping vor DR-Test konfigurieren (BP-2005 Abschnitt 8) |
| R6 | AWS eu-central-2 AZ-Ausfall trifft NC2-Cluster | Niedrig | Hoch | NC2 ist in single AZ deployed — AZ-Ausfall = vollständiger DR-Cluster-Ausfall. Akzeptiertes Risiko (Budget); als Limitation dokumentieren (TN-2028 Abschnitt 5.2) |
| R7 | Initiale Seed-Replikation schlägt fehl bei limitierter Bandbreite | Mittel | Mittel | Bandbreite vorab messen; Seeding-Zeitfenster planen; staggered Replication Schedules (BP-2005 Abschnitt 7); Retention initial auf 3 Monate setzen (BP-2005 Abschnitt 5) |
| R8 | Slide F erwähnt Flow VN für IP-Portabilität — wird nicht eingesetzt | Hoch (Missverständnis) | Mittel | Slide F Bullet anpassen: IP-Erhalt via NGT + Recovery Plan IP-Mapping, nicht via Flow VN (TN-2028 Abschnitt 3; BP-2005 Abschnitt 8) |

---

### 10. Quellen & Referenzen

| Dokument | Abschnitt | Verwendung |
|----------|-----------|-----------|
| TN-2028 v2.6 | Abschnitt 6 "Hibernate and Resume" | NC2 Suspend/Resume Mechanismus, exakter Ablauf, RTO-Grundlage |
| TN-2028 v2.6 | Abschnitt 6.1 "Native Backup" | Prism Central Recovery aus S3, 5-Schritte-Ablauf, 2 S3-Buckets |
| TN-2028 v2.6 | Abschnitt 6.2 "DR to S3" | NMST Throughput-Testwerte (3-Node i3.metal, ~10 TB, 1h 50 Min.) |
| TN-2028 v2.6 | Abschnitt 6.3 "Pilot-Light" | Pilot-Light Deployment Model, Tier-1/Tier-2 Strategie |
| TN-2028 v2.6 | Abschnitt 3 "Cloud Networking" | Native Networking vs. Flow VN, ENI-Management, IPAM, CNC |
| TN-2028 v2.6 | Abschnitt 5 "Storage" | NVMe + EBS, Skalierungsoption, Homogeneous Cluster Requirement |
| TN-2028 v2.6 | Abschnitt 10 "VM HA" | Guarantee Mode, kAcropolisHAReserveSegments, CLI-Befehl |
| BP-2202 v1.4 | Abschnitt 5 "Configuring NC2" | Required CIDR Subnets Tabelle, IPv4-only, 192.168.5.0/24 Constraint |
| BP-2202 v1.4 | Abschnitt 5 "Creating S2S VPN" | VPN-Konfigurationsschritte, Customer Gateway, VPG |
| BP-2202 v1.4 | Abschnitt 6 "Best Practices" | Transit Gateway + Direct Connect Empfehlung, IP-Konflikt-Vermeidung, DNS |
| BP-2005 v7.4 | Abschnitt 5 "NMST" | Pilot-Light vs. Snapshot-Only, Bandbreiten-Formel, Seeding |
| BP-2005 v7.4 | Abschnitt 7 "Remote Site" | Erforderliche Ports (TCP 2009/2020/9440, UDP 53) |
| BP-2005 v7.4 | Abschnitt 8 "DR Orchestration" | Protection Policies via Categories, NGT-Anforderung für IP-Erhalt, Recovery Plans |

---

*Angereichert von: Nutanix Solution Architect Agent — Axians Amanox AG*
*Alle technischen Angaben basieren auf lokal gelesenen Dokumenten: TN-2028 v2.6, BP-2202 v1.4, BP-2005 v7.4*

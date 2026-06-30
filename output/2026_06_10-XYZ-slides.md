# XYZ – Solution Proposal – Slide Texte
**Erstellt:** 10.06.2026
**Proposal-Typ:** multi-site (Cloud DR – MST + NC2 on AWS)
**Sprache:** DE
**Kunde:** XYZ
**Standort(e):** On-Premises + AWS eu-central-2 (Zürich)

---

## Slide A — Current Situation
**Slide-Titel:** Current Situation

### Inhalt (copy-paste in PowerPoint)

- 3-Node Nutanix Cluster (NX-8170-G8), AHV Hypervisor, Lizenz: NCI Starter
- AOS 7.5.1.2 / AHV 11.0.1.1
- 34 Guest-VMs (Windows und Linux); Workload-Klassifizierung ausstehend
- Prism Central auf demselben Produktionscluster betrieben
- Datensicherung via Rubrik mit On-Prem S3-Archiv (Cloudian)
- Keine Cloud-Anbindung und keine offsite DR-Strategie vorhanden

---

## Slide B — Future Situation
**Slide-Titel:** Future Situation

### Inhalt (copy-paste in PowerPoint)

- Lizenz-Upgrade On-Prem: NCI Starter → NCI Pro (Voraussetzung für Multi-site DR und MST)
- **Phase 1:** Nutanix MST konfiguriert – VM-Snapshots asynchron auf AWS S3 (eu-central-2, Zürich) repliziert
- **Phase 2:** 3-Node NC2-Cluster auf AWS (eu-central-2) deployed – Zero Compute im Normalbetrieb (Cluster Suspend; EC2 bare metal gestoppt, keine Compute-Kosten)
- Site-to-Site VPN verbindet On-Prem-Cluster permanent mit AWS VPC (Zürich); S3 und VPC-Infrastruktur laufen weiter (Fixkosten minimal)
- Bei On-Prem Ausfall: NC2-Cluster Resume (~45–90 Min.), VMs aus MST-Snapshots auf NC2 wiederhergestellt
- Mitarbeiterzugriff auf NC2-Workloads via AWS Client VPN sichergestellt

---

## Slide C — Design Decisions
**Slide-Titel:** Design Decisions

### Entscheidungstabelle (copy-paste in PowerPoint als Tabelle)

| Decision Topic | Decision |
|---|---|
| On-Prem Cluster | 3-Node NX-8170-G8, RF2, NCI Pro |
| Cloud DR Cluster (AWS) | 3-Node NC2, eu-central-2 (Zürich), RF2 – Zero Compute (Cluster Suspend; EC2 bare metal gestoppt) |
| DR-Modell & Replikation | Warm DR – MST Async (RPO = 1h), Snapshots auf AWS S3 |
| Netzwerkanbindung | Site-to-Site VPN (AWS VPN Gateway) |
| Mitarbeiterzugriff im DR | AWS Client VPN |
| NC2 Instance Type | i3en.metal (vorbehaltlich Sizing-Bestätigung) |
| DNS-Strategie im DR-Fall | AWS Route 53 Failover / On-Prem DNS-Konfiguration (Phase 2) |
| RTO | ~2–4h (NC2 Resume 45–90 Min. + VM Restore aus Snapshot; DR-Test ausstehend) |

### Constraints & Assumptions (zwei Spalten in PowerPoint)

**Constraints – Limitations**
- MST unterstützt ausschliesslich asynchrone Replikation — kein RPO = 0 möglich
- EC2 bare metal Instanzen nicht hibernate-fähig → NC2 Cluster Suspend als Äquivalent
- Prism Central läuft auf On-Prem Cluster (Single Site, keine Alternative); bei vollständigem Clusterausfall kein PC-Zugriff
- DR-Aktivierung des NC2-Clusters erfordert manuellen Eingriff — kein automatisches Failover
- VPN ist Single Point of Failure für MST-Replikation; jede Unterbrechung führt zur sofortigen RPO-Verletzung
- NC2 Resume via AWS Console ist VPN-unabhängig; DR-Runbook muss diesen Ablauf explizit abdecken
- Native Networking (kein Flow Virtual Networking) auf NC2; VMs erhalten im DR-Fall AWS-native IPs — IP-Erhalt nur mit NGT möglich
- On-Prem IP-Adressbereich darf sich nicht mit AWS VPC CIDR überschneiden; 192.168.5.0/24 ist Nutanix-intern reserviert (nicht verwenden)
- Site-to-Site VPN wird mit 2 redundanten Tunneln konfiguriert; AWS empfiehlt beide Tunnel aktiv zu halten (kein Failover-only)

**Assumptions**
- AWS Account wird neu erstellt und vollständig unter Kundenkontrolle gestellt
- Ausreichende Upload-Bandbreite On-Prem für stündliche MST-Snapshots vorhanden
- VPN-fähige Firewall/Router On-Prem für Site-to-Site VPN vorhanden
- Mitarbeiter werden mit AWS Client VPN-Zugängen ausgestattet
- Laufende AWS-Kosten im Suspended-Betrieb (S3 Storage + VPC + VPN Gateway) sind für Kunden akzeptabel
- Nutanix Guest Tools (NGT) werden vor Migration/DR-Test auf allen 34 VMs installiert
- On-Prem IP-Adressbereich wird vor Deployment mit Netzwerk-Team koordiniert (kein Überschneiden mit AWS VPC CIDR)

---

## Slide E — Professional Services
**Slide-Titel:** Offering – Amanox Professional Service

### Inhalt (copy-paste in PowerPoint)

Angebot – Amanox Professional Service

Phase 1 – MST & VPN Setup (3 AT)
- AOS / Prism Central Compatibility Matrix prüfen (LCM Update als erster Schritt)
- Lizenz-Upgrade On-Prem: NCI Starter → NCI Pro
- AWS Account & S3 Bucket Setup (IAM Policies, Bucket-Konfiguration, Lifecycle Policy)
- AWS S3 VPC Gateway Endpoint einrichten (Durchsatz & Kostenoptimierung)
- Site-to-Site VPN Konfiguration (AWS VPN Gateway ↔ On-Prem Firewall)
- VPN CloudWatch Monitoring & Alerting konfigurieren
- MST Konfiguration: Protection Policies & AWS S3 als Snapshot-Target
- Initiale Seed-Replikation planen und begleiten
- Validierung der Snapshot-Replikation auf AWS S3

Phase 2 – NC2 Deployment & DR-Readiness (4 AT)
- AWS VPC Architektur & Setup (Subnets, Routing, Security Groups)
- NC2 Cluster Deployment: 3 Nodes, AOS Installation & Cluster-Konfiguration
- NC2 Cluster Suspend-Konfiguration
- DNS-Strategie im DR-Fall konfigurieren (AWS Route 53 Failover)
- AWS Client VPN Setup (Mitarbeiterzugriff im DR-Fall)
- DR Runbook Erstellung & Übergabe (inkl. NC2 Resume via AWS Console)

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
- AWS S3 VPC Gateway Endpoint eliminiert Data Transfer Kosten beim VM Restore und maximiert Durchsatz innerhalb AWS
- IP-Erhalt der VMs im DR-Fall erfordert Nutanix Guest Tools (NGT) auf allen VMs; ohne NGT weist Native Networking neue AWS-IPs zu
- AWS eu-central-2 (Zürich) gewährleistet Datenresidenz in der Schweiz und minimale Latenz zum On-Prem Cluster

**Professional Services**
- 2-Phasen-Ansatz reduziert finanzielles Risiko: MST-Validierung in Phase 1 vor NC2-Investment in Phase 2
- Amanox übernimmt vollständigen AWS-Aufbau (VPC, VPN, IAM, NC2) — Kunde benötigt keine AWS-Vorkenntnisse
- Dedizierter DR-Test mit RTO-Messung schafft Planungssicherheit und validiert das Runbook vor dem Ernstfall
- DR Runbook ermöglicht dem Kunden eigenständige DR-Aktivierung ohne Amanox-Unterstützung im Notfall

### 💡 Zusätzliche Empfehlungen vom Agent
> Folgende Punkte könnten noch relevant sein — zur Übernahme oder zum Verwerfen:
> - **NCM Starter:** Mit On-Prem und NC2 betreibt der Kunde neu zwei Nutanix-Cluster; NCM Starter ermöglicht zentrales Capacity Reporting und Automation über beide Umgebungen
> - **Rubrik / MST Konvergenz prüfen:** MST und Rubrik decken teilweise denselben Use Case ab (offsite Archivierung); langfristig könnte MST die Rubrik-Archivierung auf Cloudian konsolidieren und die Backup-Architektur vereinfachen

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

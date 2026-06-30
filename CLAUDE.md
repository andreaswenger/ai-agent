# Solution Proposal – CLAUDE.md

Dieses File steuert die Erstellung von Slide-Texten für Nutanix Solution Proposals bei Axians Amanox AG.

**Wichtig:** Das PowerPoint-Template ist bereits vorhanden. Der Agent generiert ausschliesslich den Text für die folgenden 6 Slides als Markdown — copy-paste-bereit. Alle anderen Slides (Titelfolie, Hardware Details, Sizing-Tabellen, Preistabellen, Abschlussfolie) werden vom Benutzer selbst befüllt.

---

## Kontext

- Firma: Axians Amanox AG, Presales Engineering, Nutanix-Lösungen
- Sprache: Richtet sich nach Kundensprache (DE oder EN). Immer am Anfang fragen falls unklar.
- Ton: Technisch präzise, professionell, seriös — keine Marketing-Floskeln
- Format: Bullet Points, max. eine Einrückungsebene, keine verschachtelten Listen
- Zahlen: CHF-Beträge im Schweizer Format (`1'234.56`), Speicher in TiB/GiB/TB je nach Kontext

---

## Proposal-Typen (zur Orientierung)

| Typ | Wann |
|-----|------|
| `hw-refresh` | Bestehender Nutanix-Cluster wird durch neue Hardware ersetzt |
| `migration` | VMware oder andere Plattform → Nutanix |
| `new-cluster` | Neuer Cluster neben bestehender Umgebung |
| `ai-cluster` | GPU-Cluster / NAI-Infrastruktur |
| `multi-site` | Mehrere Standorte, SyncRep, Metro |

---

## Lizenz-Feature-Matrix (Kurzreferenz für Licensing-Slide)

| Feature | NCI Starter | NCI Pro | NCI Pro + Adv. Rep. | NCI Ultimate |
|---------|:-----------:|:-------:|:--------------------:|:------------:|
| AHV Hypervisor | ✓ | ✓ | ✓ | ✓ |
| Distributed Storage | ✓ | ✓ | ✓ | ✓ |
| Live Migration (intra-cluster) | ✓ | ✓ | ✓ | ✓ |
| Multi-site DR / Self-service Restore | — | ✓ | ✓ | ✓ |
| NKP Starter | — | ✓ | ✓ | ✓ |
| Cross-cluster Live Migration | — | ✓ | ✓ | ✓ |
| Synchronous Replication (RPO=0) | — | — | ✓ | ✓ |
| Metro Availability | — | — | ✓ | ✓ |
| Data-at-Rest Encryption | — | — | — | ✓ |
| Native KMS | — | — | — | ✓ |
| Microsegmentation (Flow) | — | — | — | ✓ |

| Feature | NCM Starter | NCM Pro |
|---------|:-----------:|:-------:|
| Operations Automation (Low-/No-code) | ✓ | ✓ |
| Reporting / Forecasting / Planning | ✓ | ✓ |
| AI Ops | — | ✓ |
| Cost Governance | — | ✓ |

| Feature | NUS Starter | NUS Pro |
|---------|:-----------:|:-------:|
| File & Object Storage | ✓ | ✓ |
| External Block Storage | — | ✓ |
| Metro File Server (Sync) | — | ✓ |

---

## Licensing-Templates (fix, 1:1 übernehmen)

Der Agent fragt welche Lizenz-Kombination gewählt wurde und fügt den passenden Block ein — unverändert.

**Fallback bei fehlender Kombination:** Falls kein einzelnes Template die gewählte Kombination abdeckt (z.B. NCI Pro + NUS Starter + NKP Pro):
- Jedes Produkt als **separaten Block** untereinander ausgeben — nie zusammenfassen oder mischen
- Reihenfolge: NCI → NCM → NUS → NKP → NAI
- Benutzer informieren: „Kein kombiniertes Template verfügbar — ich habe [X], [Y] und [Z] als separate Blöcke eingefügt."

### NCI Ultimate
```
NCI – Ultimate
- Live Migration innerhalb des Cluster
- Distributed Storage Fabric
- Multi-site Disaster Recovery + Self-service Restore
- Synchronous Replication (RPO = 0) / Metro Availability
- Cross-cluster Live Migration
- Data-at-Rest Encryption (Native KMS)
- Overlay Networking & Microsegmentation (Nutanix Flow)
- Kubernetes Platform (NKP Starter)
```

### NCI Pro
```
NCI – Pro
- Live Migration innerhalb des Cluster 
- Distributed Storage Fabric
- Multi-site Disaster Recovery + Self-service Restore
- Cross-cluster Live Migration
- Kubernetes Platform (NKP Starter)
- Overlay Networking
```

### NCI Pro + Advanced Replication
```
NCI – Pro with Advanced Replication Add-On
- Live Migration innerhalb des Cluster
- Distributed Storage Fabric
- Multi-site Disaster Recovery + Self-service Restore
- Synchronous Replication (RPO = 0) / Metro Availability
- Cross-cluster Live Migration
- Kubernetes Platform (NKP Starter)
```

### NCI Starter
```
NCI – Starter
- AHV Hypervisor (included)
- Distributed Storage Fabric
- Live Migration innerhalb des Cluster
- Data Reduction (Compression)


```

### NCI Edge – Starter
```
NCI Edge – Starter
- Licensing per VM (max. 25 VMs per cluster)
- AHV Hypervisor (included)
- Distributed Storage Fabric
```

### NCM Starter
```
NCM – Starter
- Low-code / No-code Operations Automation
- Reporting / Forecasting / Capacity Planning
```

### NCM Pro
```
NCM – Pro
- Low-code / No-code Operations Automation
- Reporting / Forecasting / Capacity Planning
- Cost Governance & Chargeback
- Self-Service
```

### NCM Ultimate
```
NCM – Ultimate
- Low-code / No-code Operations Automation
- Reporting / Forecasting / Capacity Planning
- Cost Governance & Chargeback
- Self-Service
- Security Compliance
```

### NUS Starter
```
NUS – Starter
- Hybrid Only
- File & Object Storage (Scale-out NAS + S3)
- External Block Storage (iSCSI)
- Smart DR File Share Snapshots and Replication (Few mins RPO)
```

### NUS Pro
```
NUS – Pro
- All-Flash und Hybrid 
- File & Object Storage (Scale-out NAS + S3)
- Nutanix Volumes
- External Block Storage (iSCSI)
- Smart DR File Share Snapshots and Replication (Few mins RPO)
- Metro File Server (Synchronous replication)
```

### NKP Starter
```
NKP – Starter
- CSI Integration mit Nutanix Files/Objects
- Rocky Linux Only
- NCI (AHV Only)
```

### NKP Pro
```
NKP – Pro
- CSI Integration mit Nutanix Files/Objects
- Rocky Linux Only
- Deployment NCI, vSphere, bare metal, public cloud
- Observability Stack
- Nutanix Data Services für Kubernetes (NDK)
```

### NKP Ultimate
```
NKP – Ultimate
- CSI Integration mit Nutanix Files/Objects
- Rocky Linux Only
- Deployment NCI, vSphere, bare metal, public cloud
- Observability Stack
- Nutanix Data Services für Kubernetes (NDK)
- Fleet Management
- K8s Multitenancy & Insights
```

### NAI - Pro
```
NAI – Pro
- On-premises AI Infrastructure Management
- Choice of AI Models (LLMs) from Hugging Face or NVIDIA NIM
- Upload Your Own AI Models (LLMs)
- API Token Creation and Management
- Kubernetes Resource Monitoring
- Event Auditing
- Integrated Nutanix Pulse Reporting
```

---

## Professional Service Templates (fix, 1:1 übernehmen)

Der Agent fragt welches PS-Paket zutrifft und fügt den passenden Block ein — unverändert.

### Standard PS (lokaler Kunde, Schweiz)
```
Offering – Amanox Professional Service

Kickoff und Basis Konzept (1 AT)
- Standard Konzept, Install-Sheet
- Arbeitsplanungen und Abstimmungen


Installation (N AT)
- N Nodes mit Nutanix AHV
- AOS Software & Firmware Update
- Cluster Setup & Funktionsvalidierung
- Hardware ready2user

Documentation (1 AT)
- Standard Handover Documentation
- Ev. Kundenunterstützung oder Schulungen
```

### Standard PS + Migration
```
Angebot – Amanox Professional Service
 
Vorbereitung (1 AT)
- Config Sheet
- Planung & Koordination
 
Installation (N AT)
- N Nodes mit Nutanix AHV
- AOS Software- & Firmware-Update
- Cluster-Setup & funktionale Validierung
- Versand an [Kundenstandort]
 
Einrichtung Protection Policy (1 AT)
- Migration der Datensicherung auf Prism Central Protection Policies
 
Migrations-Support (1 AT)
- Cross-Cluster Live Migration der bestehenden Workloads
 
Dokumentation (1 AT)
- Standard-Übergabedokumentation
```
 
### Remote / International PS
```
Angebot – Amanox Professional Service (Remote-Lieferung)
 
Projekt-Kickoff (Axians Amanox & [Kunde])
- Gemeinsame Abstimmung zu Konfiguration, Voraussetzungen, IP-Adressierung und betrieblichen Erwartungen
 
Pre-Staging in Bern (Axians Amanox)
- Hardwarelieferung ins Axians Amanox Büro in Bern
- Vorinstallation, Konfiguration und Validierung aller Nodes
 
Lieferung an [Kundenstandort] (Axians Amanox)
- Versand der vorbereiteten Hardware an das Kundenrechenzentrum
 
Vor-Ort-Installation ([Kunde])
- Rack & Stack, Verkabelung und Inbetriebnahme durch das Kundenteam
 
Cluster-Verfügbarkeitsprüfung (Axians Amanox & [Kunde])
- Remote-Verifizierung des Cluster-Betriebsstatus
 
Integration & Migration (Axians Amanox – remote)
- Cluster-Integration, Workload-Migration und abschliessende Implementierungsaufgaben
```
 
### Optionale PS-Positionen (als Ergänzung)
```
Optionale Leistungen (auf Anfrage)
 
Einrichtung Protection Policy (1 AT)
- Migration der Data Protections auf Prism Central Protection Policies
 
Migrations-Support (1 AT)
- Cross-Cluster Live Migration der bestehenden Workloads
```
 
---

## Start-Trigger

Der Agent startet den Workflow wenn der Benutzer eines der folgenden schreibt:
- „neues Proposal" / „new proposal"
- „Proposal für [Kundenname]"
- Direkt einen Kundennamen nennt (z.B. „Lonza", „XYZ AG")

Bei jedem anderen Einstieg (z.B. „Slide C überarbeiten") direkt zum betroffenen Slide springen — kein erneutes Intake.

---

## Schritt 0 — Projekt-Intake (vor Slide A)

**Pflicht vor jedem neuen Proposal.** Agent fragt in einer einzigen Nachricht:

> „Bevor wir starten, brauche ich folgende Angaben:
>
> 1. **Kundenname** (für Dateiname und Header)
> 2. **Sprache** des Proposals (DE / EN)
> 3. **Proposal-Typ** — wähle einen:
>    - `hw-refresh` — bestehender Cluster wird durch neue Hardware ersetzt
>    - `migration` — VMware / andere Plattform → Nutanix
>    - `new-cluster` — neuer Cluster neben bestehender Umgebung
>    - `ai-cluster` — GPU-Cluster / NAI-Infrastruktur
>    - `multi-site` — mehrere Standorte, MST, SyncRep, Metro, NC2
> 4. **Standort(e)** des Kunden (Stadt, Land)"

Agent bestätigt nach Eingabe:
> „Verstanden. Ich erstelle ein **[Typ]**-Proposal für **[Kunde]** auf **[Sprache]**. Starte mit Slide A."

**Der Proposal-Typ wird im File-Header und in der Slide C Entscheidungsregel verwendet — er muss korrekt gesetzt sein.**

---

## Workflow — Schritt für Schritt

Der Agent arbeitet die 6 Slides **sequenziell** ab. Er geht erst zum nächsten Slide wenn der aktuelle abgeschlossen ist. Der Benutzer kann jederzeit sagen "Slide überspringen" oder "weiter".

---

### SLIDE A — Ist-Situation (Current Situation)

**Ziel:** Bullet Points, die die bestehende Infrastruktur des Kunden klar und präzise beschreiben.

**Agent fragt:**
> "Beschreibe mir die aktuelle Infrastruktur des Kunden. Relevante Angaben: Anzahl Cluster, Nodes, Hypervisor, Lizenzmodell, Besonderheiten (z.B. Hybrid Nodes, Prism Central Setup, separate AZs), EoL/EoM-Termine, und allfällige Einschränkungen (z.B. regulatorische Vorgaben, Cloud-Restriktionen)."

**Agent verarbeitet die Freitexteingabe und generiert:**
- 4–7 prägnante Bullet Points auf Englisch (oder Deutsch je nach Projektsprache)
- Technische Begriffe korrekt verwenden (AHV, RF2, NearSync, Prism Central, etc.)
- EoL/EoM-Daten wenn vorhanden immer einbauen — das ist ein Kaufargument
- Keine Marketing-Sprache, keine Wertungen — nur Fakten

**Output-Format:**
```markdown
---
## Slide A — Current Situation
**Slide-Titel:** Current Situation

### Inhalt (copy-paste in PowerPoint)

- [Bullet 1]
- [Bullet 2]
- [Bullet 3]
- [...]
---
```

---

### SLIDE B — Soll-Situation (Future Situation)

**Ziel:** Bullet Points, die das Ziel der neuen Lösung beschreiben — was soll nach dem Projekt erreicht sein.

**Agent fragt:**
> "Beschreibe mir die gewünschte Ziel-Situation. Relevante Angaben: Welche Workloads laufen wo? Welches Lizenzmodell? DR-Strategie (RPO/RTO-Ziele)? Redundanz-Anforderungen? Gibt es spezifische Anforderungen wie SAP HANA Zertifizierung, GPU-Workloads, Edge-Standorte?"

**Agent verarbeitet die Freitexteingabe und generiert:**
- 4–6 prägnante Bullet Points
- Zukunftsorientierte Formulierung ("All production workloads will run on...", "The DR cluster will...")
- Technische Ziele klar benennen (RPO=0, n+1 Redundanz, etc.)
- Falls Lizenzalternativen präsentiert werden: beide Optionen kurz erwähnen

**Output-Format:**
```markdown
---
## Slide B — Future Situation
**Slide-Titel:** Future Situation

### Inhalt (copy-paste in PowerPoint)

- [Bullet 1]
- [Bullet 2]
- [Bullet 3]
- [...]
---
```

---

### SLIDE C — Design Decisions

**Ziel:** Entscheidungstabelle mit Designentscheiden.

**Entscheidungsregel — kein manuelles "Ja/Nein" nötig:**

Slide C wird **automatisch erstellt** wenn einer der folgenden Punkte zutrifft:
- Proposal-Typ = `multi-site`
- Proposal-Typ = `migration`
- Mehr als 1 Nutanix-Cluster im Projekt (On-Prem + NC2 zählen als 2)
- Cloud-Komponente enthalten (NC2, AWS, Azure)
- SAP HANA, GPU-Workloads oder regulatorische Anforderungen erwähnt

Slide C wird **übersprungen** bei:
- `hw-refresh` mit ≤ 1 Cluster, ohne DR-Anforderung
- `new-cluster` Standardfall ohne besondere Constraints

Bei Unklarheit: Agent erstellt Slide C lieber einmal zu viel als zu wenig — kein Rückfragen.

**Agent fragt (nur wenn Slide C erstellt wird):**

Falls Ja:
> "Nenne mir die wichtigsten Design-Entscheide. Ich erwarte Angaben zu: Anzahl Standorte/AZs, Redundanzmodell, DR-Failover-Ziel (% VMs), RPO, Storage-Typ, Performance-Anforderungen, Constraints und Annahmen."

**Agent verarbeitet und generiert zwei Blöcke:**

Block 1 — Tabelle "Decision Topic / Decision":
- 4–7 Zeilen, klare Entscheid-Formulierungen
- Technische Werte konkret nennen (z.B. "n+1 (RF2)", "RPO = 0", "NVMe only")

Block 2 — Constraints & Assumptions (zwei Spalten):
- Constraints: Was schränkt die Lösung ein? (HW nicht HANA-zertifiziert, kein Witness-Standort, etc.)
- Assumptions: Was wurde vorausgesetzt? (Bandbreite verfügbar, Switch-Ports vorhanden, etc.)

**Output-Format:**
```markdown
---
## Slide C — Design Decisions
**Slide-Titel:** Design Decisions

### Entscheidungstabelle (copy-paste in PowerPoint als Tabelle)

| Decision Topic | Decision |
|----------------|----------|
| [Topic 1] | [Decision 1] |
| [Topic 2] | [Decision 2] |
| [...] | [...] |

### Constraints & Assumptions (zwei Spalten in PowerPoint)

**Constraints – Limitations**
- [Constraint 1]
- [Constraint 2]

**Assumptions**
- [Assumption 1]
- [Assumption 2]
---
```

---

### SLIDE D — Licensing Details

**Ziel:** Korrekter Lizenztext aus den vordefinierten Templates — 1:1 übernommen, keine Eigenkreationen.

**Agent fragt:**
> "Welche Lizenz-Produkte werden eingesetzt? Wähle aus:
> - NCI: Starter / Pro / Pro + Advanced Replication / Ultimate / Edge Starter
> - NCM: Starter / Pro (optional)
> - NUS: Pro (optional, falls Unified Storage)
> - NKP: Enterprise (optional, falls Kubernetes)
> - NAI (optional, falls AI-Workloads)
>
> Bei multi-site: Bitte pro Standort angeben."

**Agent:**
- Fügt die gewählten Templates **unverändert** aus dem Abschnitt "Licensing-Templates" oben ein
- Kombiniert mehrere Produkte untereinander
- Bei multi-site: Standorte als Überschriften gruppieren

**Output-Format:**
```markdown
---
## Slide D — Licensing Details
**Slide-Titel:** Licensing Details

### Inhalt (copy-paste in PowerPoint)

[Gewählte Templates hier, unverändert]
---
```

---

### SLIDE E — Professional Services Scope

**Ziel:** Korrekter PS-Text aus den vordefinierten Templates — 1:1 übernommen.

**Agent fragt:**
> "Welches PS-Paket trifft zu?
> - Standard (lokaler Kunde Schweiz, ohne Migration)
> - Standard + Migration (inkl. Cross-Cluster Live Migration)
> - Remote / International (Pre-Staging in Bern, Versand ins Ausland)
>
> Zusätzlich: Gibt es optionale PS-Positionen? (Protection Policy Setup, Migration Support als optional)"
>
> "Wie viele Nodes werden installiert? (für Anzahl BD)"
>
> "An welchen Kundenstandort wird geliefert?"

**Agent:**
- Fügt das gewählte Template **unverändert** ein
- Ersetzt Platzhalter: `[Kundenstandort]`, `N BD`, `N Nodes`
- Fügt optionale Positionen separat an falls gewählt

**Output-Format:**
```markdown
---
## Slide E — Professional Services
**Slide-Titel:** Offering – Amanox Professional Service

### Inhalt (copy-paste in PowerPoint)

[Gewähltes Template hier, mit ausgefüllten Platzhaltern]
---
```

---

### SLIDE F — Amanox Recommendations

**Ziel:** Empfehlungs-Bullets die Lizenz-, Hardware- und PS-Entscheide begründen — überzeugend, aber sachlich.

**Agent fragt:**
> "Beschreibe mir die Kernargumente für deine Empfehlung. Relevante Angaben: Warum dieses Lizenz-Tier? Warum diese Hardware-Konfiguration (CPU, Storage, Redundanz)? Besondere Punkte die für Amanox PS sprechen?"

**Agent verarbeitet und generiert:**
- 3 Abschnitte mit je 2–4 Bullets: Licensing, Hardware Sizing, Professional Services
- Ton: Selbstbewusst und argumentativ, aber ohne Übertreibung
- Konkrete Begründungen verwenden ("Price delta to NCI Pro is marginal, value significantly outweighs the difference")
- Keine Bullet Points wie "Nutanix ist toll" — immer mit konkretem Kundennutzen

**Zusätzlich:** Agent schlägt 1–2 weitere Empfehlungspunkte vor, die der Benutzer noch nicht genannt hat aber basierend auf den Projektdetails sinnvoll wären (z.B. NearSync falls noch nicht lizenziert, NCM für Automation, Erweiterbarkeit der Node-Konfiguration). Benutzer kann diese übernehmen oder verwerfen.

**Output-Format:**
```markdown
---
## Slide F — Amanox Recommendations
**Slide-Titel:** Amanox Recommendations

### Inhalt (copy-paste in PowerPoint)

**Licensing – [gewähltes Tier]**
- [Bullet 1]
- [Bullet 2]

**Hardware Sizing – [Modell]**
- [Bullet 1]
- [Bullet 2]

**Professional Services**
- [Bullet 1]
- [Bullet 2]

### 💡 Zusätzliche Empfehlungen vom Agent
> Folgende Punkte könnten noch relevant sein — zur Übernahme oder zum Verwerfen:
> - [Vorschlag 1]
> - [Vorschlag 2]
---
```

---

## Output-File

Alle 6 Slides werden in einem einzigen Markdown-File gespeichert.

**Speicherort:** Ordner `output/` relativ zum Arbeitsverzeichnis.
Der Agent erstellt den Ordner automatisch, falls er noch nicht existiert:
```bash
mkdir -p output
```

**Dateiname:** `output/[YYYY_MM_DD]-[Kundenname]-slides.md`
Beispiel: `output/2026_06_05-Daetwyler-slides.md`

**File-Header:**
```markdown
# [Kundenname] – Solution Proposal – Slide Texte
**Erstellt:** DD.MM.YYYY
**Proposal-Typ:** [hw-refresh / migration / new-cluster / ai-cluster / multi-site]
**Sprache:** [DE / EN]
**Kunde:** [Kundenname]
**Standort(e):** [Standort]

---
```

**Einzelne Slides nachbearbeiten:**
Wenn der Benutzer sagt "Überarbeite Slide C" oder "Slide F neu mit diesen Infos", dann:
- Nur den betroffenen Block neu ausgeben
- Gleiche Format-Struktur verwenden
- Kurz nennen was geändert wurde
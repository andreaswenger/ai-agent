# Nutanix Solution Proposal – AI Agent Setup

Claude Code-basiertes Multi-Agent-Setup zur Erstellung und technischen Anreicherung von Nutanix Solution Proposals (Presales) bei Axians Amanox AG.

---

## Zweck

Dieses Setup unterstützt den Presales-Prozess für Nutanix-Infrastrukturprojekte (NCI, NCM, NKP, NAI, AHV, Files, HYCU, Rubrik etc.). Es erzeugt strukturierte Slide-Texte für Solution Proposals (Current Situation, Future Situation, Design Decisions, Licensing, Professional Services, Recommendations) und reichert diese anschliessend mit technischer Tiefe aus offizieller Nutanix-Dokumentation an.

**Ziel:** Konsistente, technisch fundierte Proposals schneller erstellen — ohne dabei kommerzielle Inhalte, Templates oder Kundendaten zu gefährden.

---

## Architektur — Zwei Agenten

```
projekt-root/
├── CLAUDE.md                  ← Agent 1: Proposal Writer
├── solution-architect/
│   └── CLAUDE.md               ← Agent 2: Nutanix Solution Architect
├── output/                     ← Generierte Proposals (Input/Output beider Agenten)
│   └── [YYYY_MM_DD]-[Kunde]-slides.md
│   └── [YYYY_MM_DD]-[Kunde]-enriched.md
├── resources/                   ← Nutanix Best Practices, TNs, NVDs, RAs, EOL-Daten
│   └── INDEX.md                 ← Pflichtlektüre für Agent 2
└── README.md                    ← Diese Datei
```

### Agent 1 — Proposal Writer (`CLAUDE.md`)

- Führt durch den Proposal-Erstellungsprozess: Intake (Kunde, Sprache, Proposal-Typ, Standort) → Slides A–F
- Nutzt fixe **Licensing-Templates** (NCI/NCM/NUS/NKP/NAI-Kombinationen) und **PS-Templates** — diese werden 1:1 übernommen, nicht frei generiert
- Erstellt strukturierte Slide-Dateien unter `output/[YYYY_MM_DD]-[Kunde]-slides.md`
- Proposal-Typen: `hw-refresh`, `migration`, `new-cluster`, `ai-cluster`, `multi-site`
- Slide C (Design Decisions) wird automatisch je nach Proposal-Typ erstellt oder übersprungen

### Agent 2 — Nutanix Solution Architect (`solution-architect/CLAUDE.md`)

- Liest das fertige Proposal aus `output/`
- Konsultiert `resources/INDEX.md` und schlägt einen **Dokumentenplan** vor (wartet auf Bestätigung)
- Reichert die Slides mit technischen Begründungen, Best-Practice-Referenzen und konkreten Konfigurationsparametern an (z.B. `num_vnuma_nodes`, `machine_type=q35`)
- Ergänzt einen neuen Abschnitt **„Technical Architecture Notes"** inkl. Quellenangaben
- Dokumentiert Abweichungen von Best Practices transparent, ohne Slide-Inhalte eigenmächtig zu ändern
- Ergänzungen werden inline mit `<!-- SA: ergänzt -->` markiert — klar nachvollziehbar, was vom Agent stammt
- Ausgabe: `output/[YYYY_MM_DD]-[Kunde]-enriched.md`

**Beide Agenten arbeiten token-bewusst**: Sequenzieller Workflow, Bestätigung vor jedem Dokumenten-Load, max. 5 Resources pro Session für Agent 2.

---

## Abgedeckte Themen (`resources/`)

| Bereich | Dokumente |
|---|---|
| **AHV / Core** | VM-Konfiguration, CPU/Memory, Live Migration, HA, ADS, NUMA |
| **Netzwerk** | AHV-Networking (Virtual Switches, OVS, VLANs), Physical Networking (Switches, RDMA, QoS) |
| **Disaster Recovery** | Async/NearSync Replication, Metro Availability, Remote Backup |
| **NC2 / AWS** | NC2-Deployment, Networking, Hibernate/Resume, DR to S3 |
| **SAP** | SAP HANA auf AHV (Sizing, VM-Parameter), SAP allgemein (NetWeaver, S/4HANA) |
| **SQL Server / Datenbanken** | SQL Server Best Practices, Reference Architecture, Migration zu NDB, NDB Design |
| **Files / Storage** | Nutanix Files Deployment & Performance, Unified Storage Design (Files + Objects) |
| **AI / GPU** | AI Platform Design Blueprint, GPT-in-a-Box mit NKP |
| **ROBO / Edge** | Remote-/Branch-Office Deployments |
| **Security** | IAM, IDP, Encryption, RBAC, Compliance |
| **Hardware** | NX Series Hardware Admin Guide, Node-Mixing-Restrictions |
| **Sizing / Lifecycle** | Memory-Konfiguration, LCM/Upgrade-Prozesse, Platform EOL/EOM-Referenz (G7–G10) |
| **Migration** | ESXi/Hyper-V/Public Cloud → AHV (Move Appliance, VM Mobility) |

Die vollständige, durchsuchbare Liste mit Konsultations-Empfehlungen findet sich in [`resources/INDEX.md`](resources/INDEX.md).

---

## Voraussetzungen

- [Claude Code](https://docs.claude.com) (CLI oder VS Code Extension)
- Zugriff auf Anthropic API / Claude.ai Account mit Claude Code aktiviert
- Git-Client

---

## Setup / Erste Schritte

```bash
git clone <repo-url>
cd nutanix-proposal-agent
claude
```

Claude Code lädt automatisch die `CLAUDE.md` im Projekt-Root (Agent 1).

**Neues Proposal starten:**
```
neues Proposal
```
oder direkt:
```
Proposal für Kunde XYZ
```

Der Agent fragt im Intake nach Kundenname, Sprache, Proposal-Typ und Standort und führt dann durch Slides A–F.

**Proposal anreichern (Agent 2):**
```bash
cd solution-architect
claude
```
Agent 2 liest automatisch das passende File aus `output/`, schlägt einen Dokumentenplan vor und erstellt nach Bestätigung das `-enriched.md` File.

**Einzelne Slides nachbearbeiten:**
```
Überarbeite Slide B mit BP-2029-AHV
```

---

## Was du NICHT committen solltest

- `output/*-slides.md` und `*-enriched.md` mit **echten Kundendaten** — diese enthalten Kundennamen, Standorte und ggf. kommerzielle Details
- `.env`, API Keys, Credentials

**Empfehlung:** `output/` per `.gitignore` ausschliessen, oder anonymisierte Beispiel-Proposals in einem separaten `examples/`-Ordner ablegen.

```gitignore
output/*
!output/.gitkeep
```

---

## Konventionen

- **Sprache:** Proposals können DE oder EN sein (wird im Intake festgelegt); Agent-Instruktionen (`CLAUDE.md`) sind auf Deutsch
- **CHF-Beträge:** Schweizer Format `1'234.56`
- **Nutanix-Begriffe:** exakt wie in offizieller Doku, keine erfundenen Abkürzungen
- **Dateinamen:** `[YYYY_MM_DD]-[Kunde]-slides.md` / `-enriched.md`
- **Inline-Markierungen:** Ergänzungen von Agent 2 sind mit `<!-- SA: ergänzt -->` gekennzeichnet

---

## Setup pflegen / erweitern

- **Neue Best-Practice-Dokumente:** unter `resources/` ablegen und in `resources/INDEX.md` eintragen (Kategorie + Entscheidungshilfe-Zeile)
- **Neue Lizenz- oder PS-Templates:** in `CLAUDE.md` (Agent 1) ergänzen
- **Platform EOL-Daten aktualisieren:** `resources/PLATFORM_EOL_reference.md` regelmässig gegen [portal.nutanix.com](https://portal.nutanix.com) abgleichen

---

## Fragen / Feedback

Bei Fragen zum Setup: Presales Engineering, Axians Amanox AG
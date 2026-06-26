<div align="center">

<img src="https://img.shields.io/badge/Neptune-CRM-C8952A?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgNjAgNjAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGNpcmNsZSBjeD0iMzAiIGN5PSIzMCIgcj0iMjIiIHN0cm9rZT0iI0M4OTUyQSIgc3Ryb2tlLXdpZHRoPSIxLjUiIG9wYWNpdHk9Ii41Ii8+PHBvbHlnb24gcG9pbnRzPSIzMCw0IDMzLDI2IDMwLDIzIDI3LDI2IiBmaWxsPSIjQzg5NTJBIi8+PHBvbHlnb24gcG9pbnRzPSIzMCw1NiAzMywzNCAzMCwzNyAyNywzNCIgZmlsbD0iI0M4OTUyQSIvPjxwb2x5Z29uIHBvaW50cz0iNCwzMCAyNiwyNyAyMywzMCAyNiwzMyIgZmlsbD0iI0M4OTUyQSIvPjxwb2x5Z29uIHBvaW50cz0iNTYsMzAgMzQsMjcgMzcsMzAgMzQsMzMiIGZpbGw9IiNDODk1MkEiLz48cGF0aCBkPSJNMTQgMzAgUTMwIDE2IDQ2IDMwIFEzMCA0NCAxNCAzMFoiIGZpbGw9IiMwQTBDMEYiIHN0cm9rZT0iI0M4OTUyQSIgc3Ryb2tlLXdpZHRoPSIxLjIiLz48Y2lyY2xlIGN4PSIzMCIgY3k9IjMwIiByPSI3IiBmaWxsPSIjQzg5NTJBIiBvcGFjaXR5PSIuOSIvPjxjaXJjbGUgY3g9IjMwIiBjeT0iMzAiIHI9IjQiIGZpbGw9IiMwQTBDMEYiLz48L3N2Zz4=&labelColor=0A0C0F" />

# NEPTUNE CRM

### Vessel Operations Management System

**Paperless maritime operations for department heads, officers, and masters.**  
Built for real ships. Works offline. Syncs when VSAT connects.

---

![Version](https://img.shields.io/badge/Version-1.0.0-C8952A?style=flat-square&labelColor=111418)
![Status](https://img.shields.io/badge/Status-Active%20Development-16A34A?style=flat-square&labelColor=111418)
![Platform](https://img.shields.io/badge/Platform-Web%20%7C%20Mobile%20%7C%20Desktop-1A5C8A?style=flat-square&labelColor=111418)
![Offline](https://img.shields.io/badge/Offline-First-B7760D?style=flat-square&labelColor=111418)
![Compliance](https://img.shields.io/badge/MARPOL%20%7C%20MLC%202006%20%7C%20ISM-Compliant-C8952A?style=flat-square&labelColor=111418)

</div>

---

## ⚓ Overview

Neptune CRM is a full-stack vessel operations platform designed to replace paper-based maritime workflows with a fast, compliant, offline-capable digital system. It covers every department aboard — deck, engine, and catering — with strict chain of command enforcement, cryptographic document signing, and automatic synchronisation to shore headquarters when satellite connectivity is available.

> **Designed for:** Masters, Chief Mates, Chief Engineers, Chief Stewards, and operational officers aboard commercial vessels.

---

## 📱 Interfaces

| Interface | File | Description |
|-----------|------|-------------|
| **Mobile** | `neptune_crm_mobile.html` | Touch-optimised for phones and tablets. Bottom navigation, swipe-up delegation sheets, full-screen modals, safe area support. |
| **Desktop** | `neptune_crm_desktop.html` | Full sidebar layout for bridge terminals and officer workstations. Includes live VSAT sync panel, offline queue bar, and delegation dropdowns. |

Both interfaces share identical functionality — the experience adapts to the screen, not the other way around.

---

## ✦ Key Features

### 🔐 Permit to Work (PTW)
- Digital PTW register for enclosed space, hot work, working aloft, electrical isolation, and diving
- System-enforced safety checklist gates — work cannot begin until 100% complete
- Routes automatically to Master's signature queue
- Immutable closure log synced to shore HQ

### ⚙ Planned Maintenance System (CMMS)
- Running hours telemetry triggers PMS tasks automatically
- Spare parts reserved from inventory on task assignment
- Auto-generates Purchase Orders when stock falls below minimum
- Full task history with completion timestamps

### 📋 Electronic Oil Record Book (eORB)
- MARPOL Annex I validation on every entry before submission
- Dual-signature lock — Chief Engineer signs, Master countersigns
- Cryptographically immutable once signed
- Sync column shows real-time shore transmission status

### 🧭 Voyage Management
- ECDIS-integrated voyage plan drafting (2nd Mate → Chief Mate → Master)
- UKC validation at every waypoint
- Master signature auto-distributes Standing Orders to all bridge officers
- Locked and version-controlled once signed

### 🍽 Catering & MLC 2006
- 4-week menu planning with nutritional compliance analysis
- IoT cold storage monitoring — auto-alerts on temperature breach
- Pre-arrival hygiene audit workflow triggered 48 hrs before PSC inspection
- Slop chest with payroll-linked deductions

### 🏴 Port Documents
- Auto-compiles NOR, SOF, General Declaration, Crew List (FAL 5), Stores Declaration (FAL 3), and DG Declaration from vessel variables
- Zero manual re-entry — draft, crew list, and cargo propagate automatically
- One-click transmission to Port Agent on Master signature

### ↗ Task Delegation
- Department heads delegate tasks directly from every workflow screen
- Delegation dropdown / bottom sheet shows officer name, rank, and live watch status
- Officer availability board with active task counts
- Delegated task tracker with status updates

---

## 📡 Offline-First Architecture

Neptune is built for the reality of maritime satellite connectivity — intermittent, expensive, and unreliable.

```
Ship LAN (192.168.1.0/24)
├── Ship Server (ruggedised edge node)
│   ├── Neptune API (Node.js / Fastify)
│   ├── SQLite database (WAL mode, offline-first)
│   ├── MinIO file storage (S3-compatible)
│   ├── Mosquitto MQTT broker (IoT sensors)
│   └── Sync Worker (event queue daemon)
└── Crew devices → connect via ship Wi-Fi / LAN

VSAT / Starlink (satellite — intermittent)
└── Shore Infrastructure (AWS)
    ├── PostgreSQL (primary database)
    ├── S3 (document archive)
    └── Shore Operations Portal
```

### How sync works
1. **Every action** is written to the local ship server immediately — no data is ever lost
2. A background sync worker maintains an **append-only event queue**
3. When VSAT reconnects, the queue drains automatically in priority order (compliance documents first)
4. Immutable records (signed eORB, closed PTW) use **event sourcing** — conflicts are impossible by design
5. The VSAT indicator in the topbar shows live status: `ONLINE` · `SYNCING` · `OFFLINE — QUEUED`

---

## 🏗 Tech Stack

| Layer | Technology |
|-------|-----------|
| **Web frontend** | React 18 + TypeScript + Vite (PWA) |
| **Mobile** | React Native (Expo SDK 51) |
| **API** | Node.js 20 + Fastify |
| **Ship database** | SQLite 3.45 (WAL mode) |
| **Shore database** | PostgreSQL 16 (AWS RDS Multi-AZ) |
| **File storage** | MinIO (ship) → AWS S3 (shore) |
| **IoT messaging** | MQTT (Mosquitto) |
| **Job queue** | BullMQ + Redis |
| **Auth** | JWT RS256 + Ed25519 digital signatures |
| **Push notifications** | FCM (Android) + APNs (iOS) |
| **Infrastructure** | Docker Compose (ship) · Kubernetes/EKS (shore) |

---

## 🗄 Database Schema

12 core tables across four domains:

**Operations** — `vessels`, `users`, `voyage_plans`, `tasks`  
**Compliance** — `ptw_records`, `eorb_entries`, `safety_inspections`  
**Engineering** — `pms_tasks`, `inventory_items`, `engine_readings`  
**Catering & Crew** — `cold_storage_readings`, `crew_welfare`

Full schema with field types, constraints, and descriptions is documented in [`neptune_infrastructure.pdf`](./neptune_infrastructure.pdf).

---

## 🔑 Authentication & RBAC

Three permission levels enforced at both API middleware and database row level:

| Level | Role | Key Permissions |
|-------|------|----------------|
| **1** | Master | Final authorization on all compliance documents. Absolute override. |
| **2** | Department Heads (Chief Mate, Chief Engineer, Chief Steward) | Create and validate workflows, assign tasks, sign departmental records. |
| **3** | Officers (2nd/3rd Mate, 2nd/3rd Engineer) | Execute assigned tasks, log readings, request clearance. |

Digital signatures use **Ed25519 keypairs** stored in device secure enclave (iOS) / Android Keystore. Private keys never leave the device. All signing actions require biometric confirmation.

---

## 📜 Regulatory Compliance

| Regulation | Scope | Neptune Module |
|-----------|-------|---------------|
| MARPOL Annex I | Oil pollution prevention | eORB — immutable dual-signed log |
| SOLAS Ch. III | Life-saving appliances | Safety Inspections — ISM schedule |
| SOLAS Ch. V | Navigation safety | Voyage Plan — Master approval |
| MLC 2006 | Crew welfare | Catering — food safety, potable water, slop chest |
| ISM Code | Safety management | PTW — high-risk work authorization |
| IMDG Code | Dangerous goods | Cargo Planning — hazmat stow validation |
| IMO FAL 1–7 | Port formalities | Port Documents — auto-compiled forms |

---

## 🚀 Getting Started (Demo)

The current builds are standalone HTML files — no server required to preview.

```bash
# Clone the repository
git clone https://github.com/eagleconsulting954-a11y/neptune-crm.git
cd neptune-crm

# Open in browser (no build step needed for demo)
open neptune_crm_desktop.html   # macOS
start neptune_crm_desktop.html  # Windows
xdg-open neptune_crm_desktop.html  # Linux
```

### Demo features
- **User switcher** — click your name (top right) to cycle through Master, Chief Mate, Chief Engineer, and Chief Steward views
- **Offline simulation** — the desktop version automatically goes offline after 12 seconds to demonstrate queue behaviour, then reconnects and drains the queue
- **VSAT panel** — click the green `VSAT ONLINE` indicator to open the full sync status panel
- **Delegation** — every task and PTW has a live delegate dropdown with officer availability

---

## 📁 Project Structure

```
neptune-crm/
├── neptune_crm_mobile.html       # Mobile-first interface (iOS / Android / tablet)
├── neptune_crm_desktop.html      # Desktop interface (bridge terminal / workstation)
├── neptune_infrastructure.pdf    # Full technical specification
│                                 #   — API reference (35+ endpoints)
│                                 #   — Database schema (12 tables)
│                                 #   — Offline sync architecture
│                                 #   — IoT integration guide
│                                 #   — DevOps & deployment
│                                 #   — Regulatory compliance map
├── vesselops_crm_workflows.pdf   # Workflow specification
│                                 #   — All departmental workflows
│                                 #   — Step-by-step process diagrams
│                                 #   — Data models
└── README.md                     # This file
```

---

## 🗺 Roadmap

- [ ] React + TypeScript full implementation
- [ ] React Native mobile app (Expo)
- [ ] Ship server Docker Compose setup
- [ ] PostgreSQL + SQLite schema migrations (Drizzle ORM)
- [ ] MQTT IoT ingestion service
- [ ] Ed25519 digital signature implementation
- [ ] Offline sync worker (event sourcing)
- [ ] PDF generation for compliance reports (Puppeteer)
- [ ] Push notifications (FCM + APNs)
- [ ] Shore operations portal
- [ ] ECDIS integration (NMEA / S-57)
- [ ] AIS position tracking

---

## 👤 Built By

**Eagle Consulting** — Maritime technology solutions

---

## ⚠️ Security Notice

If you have a GitHub Personal Access Token committed or shared anywhere, **revoke it immediately** at [github.com/settings/tokens](https://github.com/settings/tokens) and generate a new one. Never commit tokens, passwords, or secrets to a repository.

For production deployments, use environment variables and a secrets manager (AWS Secrets Manager, HashiCorp Vault, or similar).

---

<div align="center">

**Neptune CRM** · Built for the sea · Works without the cloud

*MARPOL · SOLAS · MLC 2006 · ISM · IMDG · IMO FAL*

</div>

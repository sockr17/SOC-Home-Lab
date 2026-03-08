<div align="center">

# 🛡️ SOC Home Lab

### A end-to-end Security Operations Center built from scratch

*Threat Detection · Endpoint Monitoring · Attack Simulation · Incident Investigation*

---

[![Status](https://img.shields.io/badge/Status-In_Progress-f0a500?style=for-the-badge)](https://github.com/kripy17)
[![Phase](https://img.shields.io/badge/Current_Phase-2_of_3-0056D2?style=for-the-badge)](https://github.com/kripy17)
[![Platform](https://img.shields.io/badge/Platform-VMware_Workstation-607078?style=for-the-badge&logo=vmware&logoColor=white)](https://www.vmware.com/)
[![SIEM](https://img.shields.io/badge/SIEM-Wazuh-0056D2?style=for-the-badge)](https://wazuh.com/)
[![OS](https://img.shields.io/badge/Ubuntu-22.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![Windows](https://img.shields.io/badge/Windows-10-0078D6?style=for-the-badge&logo=windows&logoColor=white)](https://microsoft.com)

</div>

---

## 📖 About This Project

This repository documents the design, deployment, and operation of a **home Security Operations Center (SOC)** built entirely from scratch across three progressive phases.

The goal is to simulate how real security teams work — from infrastructure deployment to endpoint monitoring to live attack detection and investigation. Every phase builds on the last, creating a complete blue-team portfolio that demonstrates practical SOC analyst skills.

> Built on an **i3 processor with 8GB RAM** — every design decision reflects real-world constraints and deliberate tradeoffs.

---

## 🏗️ Lab Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    HOST MACHINE                          │
│                VMware Workstation                        │
│                                                          │
│   ┌──────────────────┐      ┌──────────────────────┐    │
│   │  Ubuntu Server   │      │    Windows 10 VM     │    │
│   │  (SOC Node)      │◄────►│  (Monitored Endpoint)│    │
│   │                  │      │                      │    │
│   │  Wazuh Manager   │      │   Wazuh Agent        │    │
│   │  Wazuh Indexer   │      │   Sysmon             │    │
│   │  Wazuh Dashboard │      │                      │    │
│   │  Filebeat        │      │                      │    │
│   └──────────────────┘      └──────────────────────┘    │
│                                                          │
│              VMnet8 — NAT Network                        │
│              192.168.1.0/24                              │
└─────────────────────────────────────────────────────────┘
```

---

## 🗺️ Project Phases

<table>
  <thead>
    <tr>
      <th>Phase</th>
      <th>Title</th>
      <th>Key Topics</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Phase 1</strong></td>
      <td><a href="./phase-1-wazuh-deployment">SIEM Infrastructure & Endpoint Monitoring</a></td>
      <td>Wazuh deployment · Kali agent onboarding · Log pipeline validation · Attack simulation · FIM</td>
      <td>✅ Complete</td>
    </tr>
    <tr>
      <td><strong>Phase 2</strong></td>
      <td><a href="./phase-2-windows-sysmon">Windows Telemetry & Sysmon Integration</a></td>
      <td>Windows agent · Sysmon config · Process telemetry · Centralized log ingestion</td>
      <td>🔄 In Progress</td>
    </tr>
    <tr>
      <td><strong>Phase 3</strong></td>
      <td>Attack Simulation & SOC Investigation</td>
      <td>Brute force · PowerShell abuse · Persistence · Privilege escalation · Incident reports</td>
      <td>📋 Planned</td>
    </tr>
  </tbody>
</table>

---

## 🔬 Phase Summaries

### ✅ Phase 1 — SIEM Infrastructure & Endpoint Monitoring
> [`/phase-1-wazuh-deployment`](./phase-1-wazuh-deployment)

Deployed a fully functional Wazuh SIEM stack on an Ubuntu Server VM. Onboarded a Kali Linux endpoint using key-based agent authentication. Validated the complete log pipeline from endpoint activity through to dashboard alert visibility.

**Highlights:**
- Wazuh all-in-one deployment (Manager + Indexer + Dashboard + Filebeat)
- Incremental UFW firewall configuration
- Key-based agent registration and validation
- Controlled attack simulations — auth abuse, privilege escalation, FIM, suspicious tool install
- Full end-to-end pipeline confirmed ✅

---

### 🔄 Phase 2 — Windows Telemetry & Sysmon Integration
> [`/phase-2-windows-sysmon`](./phase-2-windows-sysmon)

Extended the SOC lab to include a Windows 10 endpoint. Deployed and registered the Wazuh Windows agent and integrated Sysmon using the SwiftOnSecurity configuration for enhanced process-level telemetry.

**Highlights:**
- Windows 10 VM onboarded to Wazuh
- Sysmon configured with SwiftOnSecurity ruleset
- Process creation, registry, and network telemetry enabled
- Sysmon events confirmed visible in Wazuh Dashboard ✅

---

### 📋 Phase 3 — Attack Simulation & SOC Investigation *(Coming Soon)*

Will simulate real attack techniques on the Windows endpoint and investigate each as a SOC analyst — from raw alert through to documented incident report.

**Planned simulations:**
- Brute force login attempts → Windows Event ID 4625
- Suspicious PowerShell execution → Sysmon Event ID 1
- Abnormal process execution → Parent-child chain analysis
- Registry persistence mechanism → Sysmon Event ID 13
- Privilege escalation → Event ID 4672

Each attack will produce a full incident report with MITRE ATT&CK mapping.

---

## 🧰 Tools & Technologies

| Category | Tools |
|---|---|
| Virtualisation | VMware Workstation |
| SIEM | Wazuh (Manager · Indexer · Dashboard · Filebeat) |
| Endpoint Telemetry | Sysmon (SwiftOnSecurity config) |
| Operating Systems | Ubuntu Server 22.04 · Kali Linux · Windows 10 |
| Log Sources | Windows Event Logs · Sysmon · Linux Auth Logs |
| Detection | Wazuh built-in rules · File Integrity Monitoring |

---

## 💡 Skills Demonstrated

- SIEM deployment and configuration
- Endpoint agent onboarding and validation
- Windows and Linux log analysis
- Sysmon telemetry integration
- Attack simulation and detection validation
- SOC analyst investigation workflow
- MITRE ATT&CK framework mapping
- Incident documentation

---

## 📁 Repository Structure

```
SOC-Home-Lab/
│
├── README.md                          ← you are here
│
├── phase-1-wazuh-deployment/
│   ├── README.md
│   └── screenshots/
│       ├── ubuntu/
│       ├── wazuh/
│       ├── agent/
│       └── attacks/
│
├── phase-2-windows-sysmon/
│   ├── README.md
│   └── screenshots/
│       ├── windows/
│       └── wazuh/
│
└── phase-3-attack-simulation/         ← coming soon
    ├── README.md
    ├── screenshots/
    └── incident_reports/
```

---

## ⚙️ Hardware Constraints

This entire lab runs on consumer hardware:

| Resource | Spec |
|---|---|
| CPU | Intel Core i3 |
| RAM | 8 GB total |
| Virtualisation | VMware Workstation |

Every VM sizing decision was made deliberately to keep the full stack stable within these limits — reflecting the kind of real-world resource awareness expected in production SOC environments.

---

<div align="center">

*SOC Home Lab Series by [Krish Patel](https://github.com/kripy17)*

Phase 1: SIEM Infrastructure ✅ &nbsp;·&nbsp; Phase 2: Windows + Sysmon 🔄 &nbsp;·&nbsp; Phase 3: Attack Simulation 📋

</div>

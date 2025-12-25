# SOC Automation Lab – Wazuh, Shuffle, TheHive, Sysmon

## Project Overview

This project demonstrates a **fully automated SOC (Security Operations Center) workflow** using open-source security tooling.
It simulates a real-world detection and response pipeline—from endpoint telemetry to automated enrichment, case creation, and analyst notification.

The lab focuses on **hands-on SOC engineering**, **SIEM detections**, and **SOAR automation**, rather than application development.

---

## Key Objectives

* Collect detailed endpoint telemetry using **Sysmon**
* Ingest and correlate logs with **Wazuh (SIEM/XDR)**
* Detect credential-dumping activity (**Mimikatz**)
* Automate alert handling using **Shuffle (SOAR)**
* Enrich alerts with **VirusTotal**
* Create incidents automatically in **TheHive**
* Notify SOC analysts via **email**

---

## Architecture

### SOC Automation Architecture Overview
<img width="1582" height="695" alt="SOC-Automation-Project-Diagram" src="https://github.com/user-attachments/assets/608c6d58-70f7-488b-bb40-0f48e6d6d99e" />


### High-Level Flow

1. Windows 11 endpoint generates Sysmon telemetry
2. Wazuh agent forwards logs to Wazuh Manager
3. Custom detection rule triggers on Mimikatz execution
4. Alert is sent to Shuffle via webhook
5. Shuffle:

   * Extracts SHA-256 hash
   * Enriches data using VirusTotal
   * Creates alert in TheHive
   * Sends email notification to SOC analyst

---

## Tools & Technologies

| Category         | Tools                         |
| ---------------- | ----------------------------- |
| Endpoint Logging | Sysmon                        |
| SIEM / XDR       | Wazuh                         |
| SOAR             | Shuffle                       |
| Case Management  | TheHive                       |
| Threat Intel     | VirusTotal                    |
| OS               | Windows 11, Ubuntu 24.04      |
| Cloud            | VPS (Wazuh & TheHive servers) |

---

### Infrastructure Setup (Phase 1)

### Secure SSH Access to Ubuntu SOC Servers
> Establishing secure remote access to cloud-based SOC infrastructure.
<img width="1247" height="865" alt="setting-ssh-in-ubuntu-server" src="https://github.com/user-attachments/assets/ef9df6af-49db-476c-b4c3-b4f5287ebbaa" />

### Wazuh SIEM/XDR Installation and Initialization
> Deployment of the Wazuh Manager, Indexer, and Dashboard components.
<img width="1920" height="1080" alt="installing-wazuh" src="https://github.com/user-attachments/assets/ffdfa014-6a68-411c-8deb-655e01141067" />

### TheHive Case Management Platform Installation
> Installing and preparing TheHive for incident and alert management.
<img width="1243" height="732" alt="installing - thehive" src="https://github.com/user-attachments/assets/b3552e31-4821-4c9a-a827-45c0e5691f53" />

### Wazuh Dashboard Initial Credentials Generation
> Verification of successful Wazuh installation and secure credential provisioning.
<img width="506" height="81" alt="username-and-passwd-wazuh" src="https://github.com/user-attachments/assets/59d01dfd-8b87-4b77-a60a-eee259af1f08" />

## Repository Structure

```text
soc-automation-lab/
├── phases/
│   ├── phase-1.txt   # Infrastructure & endpoint setup
│   ├── phase-2.txt   # Wazuh & TheHive configuration
│   ├── phase-3.txt   # Sysmon ingestion & custom detection
│   └── phase-4.txt   # SOAR automation & integrations
│
├── screenshots/
│   ├── config-wazuh-to-shuffle.png
│   ├── config-wazuh-to-shuffle-2.png
│   ├── running-mimikatz-to-get-alert-in-shuffle.png
│   ├── shuffle-completed-flow.png
│   └── hive-got-the-alert-and-hive-dashboard.png
│
├── docs/
│   └── architecture/
│       └── Formatted-SOC-Automation-Project-Diagram.png
│
└── README.md
```

---

## Detection Engineering

### Database & Backend Configuration (Phase 2)

**Apache Cassandra Configuration for TheHive**
> Backend database configuration required for TheHive alert and case storage.
<img width="960" height="1020" alt="config-cassandra" src="https://github.com/user-attachments/assets/7dbdfa2d-189c-43f8-b283-32dd8e757be8" />


### Custom Detection Rule

* Detects **Mimikatz execution**
* Based on **Sysmon Event ID 1 (Process Creation)**
* Matches `originalFileName = mimikatz.exe`
* MITRE ATT&CK mapping: `T1003 – Credential Dumping`

This ensures **high-fidelity detection** with minimal noise.

---

### Detection & Alert Generation (Phase 3)

**Simulated Mimikatz Execution on Windows Endpoint**
> Triggering malicious credential-dumping activity to validate detection rules.
<img width="1285" height="812" alt="running-mimikatze-to-get-alert-in shuffle" src="https://github.com/user-attachments/assets/bd1e4c80-a6ab-40a1-b1ca-58537a8a49d6" />

## SOAR Automation Workflow


### Automated Actions

* Receive Wazuh alert via webhook
* Extract SHA-256 hash using regex
* Query VirusTotal for reputation
* Create alert in TheHive
* Send email notification to SOC analyst

---

### Wazuh → Shuffle Integration (Phase 4)

**Wazuh Alert Forwarding to Shuffle via Webhook**
> Configuration of Wazuh integration to forward alerts into the SOAR platform.
<img width="1917" height="1080" alt="config-wazuh-to-shuffle" src="https://github.com/user-attachments/assets/677f2c32-0680-4b09-9fa3-e3d676e3c0fb" />

**Wazuh Integration Rule Mapping and Alert Filtering**
> Ensuring only relevant detection rules trigger automation workflows.
<img width="1920" height="1080" alt="config-wazuh-to-shuffle-2" src="https://github.com/user-attachments/assets/97f1da33-0dff-407c-8319-a432761a55c9" />

## Validation & Results

**Completed SOAR Automation Workflow Execution**
> End-to-end automation showing enrichment, alert processing, and response actions.
<img width="1920" height="947" alt="shuffle-completed-flow" src="https://github.com/user-attachments/assets/2abeb49b-71a5-4542-a273-36b79d4e96cb" />

**Automated Incident Creation in TheHive Dashboard**
> Final confirmation of successful SOC automation with alert visibility in TheHive.
<img width="1682" height="650" alt="hive-got-the-alert-and-hive-dashboard" src="https://github.com/user-attachments/assets/12ddbfe8-af25-4612-b861-a51fe2bcbead" />


### Confirmed Outcomes

* Sysmon telemetry successfully ingested
* Custom detection rule triggered
* Alert forwarded to Shuffle
* VirusTotal enrichment completed
* Alert automatically created in TheHive
* Email notification delivered

---

## Skills Demonstrated

* SOC architecture design
* SIEM rule creation & tuning
* Endpoint telemetry analysis
* SOAR workflow automation
* Threat intelligence enrichment
* Incident & case management
* Cloud-based SOC infrastructure

---

##  Notes

* This project is **documentation-driven**, not code-heavy
* All work is recorded in **phase files**
* Designed to reflect **real SOC workflows**
* Suitable for **portfolio, resume, and interviews**

---

## Final Status

 **Project Complete – End-to-End SOC Automation Implemented**

---

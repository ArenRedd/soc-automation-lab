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

### Custom Detection Rule

* Detects **Mimikatz execution**
* Based on **Sysmon Event ID 1 (Process Creation)**
* Matches `originalFileName = mimikatz.exe`
* MITRE ATT&CK mapping: `T1003 – Credential Dumping`

This ensures **high-fidelity detection** with minimal noise.

---

## SOAR Automation Workflow


### Automated Actions

* Receive Wazuh alert via webhook
* Extract SHA-256 hash using regex
* Query VirusTotal for reputation
* Create alert in TheHive
* Send email notification to SOC analyst

---

## Validation & Results

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

# Phase 1 – Windows Endpoint, Sysmon, Wazuh Server, and TheHive Infrastructure Setup

## Objective

Set up the foundational infrastructure required for the SOC Automation project, including:

* Windows 11 endpoint with Sysmon logging
* Wazuh SIEM/XDR server
* TheHive case management server
  This phase focuses strictly on installation, validation, and baseline readiness.

---

## 1. VirtualBox Installation

### Version Selection

* **VirtualBox version used:** `7.1.12`
* Reason: More stable compatibility compared to newer releases.

### Installation Steps

1. Download VirtualBox 7.1.12 from **Previous Releases**.
2. Install with default options.
3. Accept network interface reset warning.
4. Complete installation.

### Integrity Verification (Optional but Recommended)

```powershell
Get-FileHash VirtualBox-7.1.12.exe
```

* Confirm SHA-256 checksum matches the official release.

---

## 2. Windows 11 Virtual Machine Setup

### ISO Creation

1. Download **Windows 11 Installation Media Tool**.
2. Select **Create installation media**.
3. Choose **ISO file** option.
4. Save ISO locally.

### Virtual Machine Configuration

* OS Type: Microsoft Windows
* Version: Windows 11
* RAM: **8 GB**
* CPU: **2 vCPUs**
* Disk: **80 GB (VDI, dynamically allocated)**

### Windows Installation

1. Boot VM using ISO.
2. Select **Windows 11 Pro**.
3. Skip product key.
4. Complete standard installation.
5. Sign in with Microsoft account (required during setup).
6. Complete privacy and device configuration.

---

## 3. Sysmon Installation on Windows 11

### Sysmon Download

* Source: Sysinternals
* Version used: `15.15`

### Sysmon Configuration

* Configuration file: **Olaf Hartong’s sysmon-config.xml**
* Download raw XML and save in Sysmon directory.

### Installation Commands

```powershell
sysmon64.exe -i sysmonconfig.xml
```

### Validation

* Check service status:

  * `Sysmon64` running
* Event Viewer:

  * Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
  * Logs should be present and updating

---

## 4. Snapshot Creation

Create a snapshot after Sysmon installation:

* Snapshot Name: `sysmon-installed`
* Purpose: Restore point before agent integration

---

## 5. Wazuh Server Deployment

### Cloud VM Specifications

* OS: Ubuntu 24.04 LTS
* CPU: 4 vCPUs
* RAM: 8 GB
* Role: Wazuh Manager + Indexer + Dashboard

### Initial Server Setup

```bash
apt update && apt upgrade -y
```

### Wazuh Installation

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
bash wazuh-install.sh -a
```

### Services Installed

* Wazuh Manager
* Wazuh Indexer
* Wazuh Dashboard

### Credential Handling

* Default user: `admin`
* Save generated password securely

### Firewall Configuration

```bash
ufw allow 443
```

### Access Validation

* URL: `https://<WAZUH_PUBLIC_IP>`
* Login using admin credentials
* Dashboard access confirms successful deployment

---

## 6. TheHive Server Deployment

### Cloud VM Specifications

* OS: Ubuntu 24.04 LTS
* CPU: 6 vCPUs
* RAM: 16 GB
* Purpose: Case management backend

### System Preparation

```bash
apt update && apt upgrade -y
```

---

## 7. Dependency Installation for TheHive

### Java (Required)

```bash
apt install -y openjdk-11-jre-headless
java -version
```

### Apache Cassandra

```bash
apt install -y cassandra
```

### Elasticsearch

```bash
apt install -y elasticsearch
```

---

## 8. TheHive Installation

### Package Download

```bash
wget https://download.thehive-project.org/thehive_5.5.7_all.deb
```

### Installation

```bash
apt install ./thehive_5.5.7_all.deb
```

> Configuration and service integration are deferred to the next phase.

---

## Phase Completion Criteria

* Windows 11 endpoint running with Sysmon logging enabled
* Snapshot created post-Sysmon installation
* Wazuh dashboard accessible over HTTPS
* TheHive installed with all dependencies resolved

---

**End of Phase 1**
Next phase covers:

* Wazuh agent deployment
* Sysmon log ingestion
* TheHive configuration
* SOC automation workflow integration

---

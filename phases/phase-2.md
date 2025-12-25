# Phase 2 – Wazuh and TheHive Configuration & Agent Integration

## Objective

Configure backend services required for SOC operations:

* Apache Cassandra
* Elasticsearch
* TheHive
* Wazuh agent integration with Windows 11 endpoint

This phase validates end-to-end log ingestion from endpoint to SIEM.

---

## 1. Apache Cassandra Configuration (TheHive)

### Configuration File

```bash
/etc/cassandra/cassandra.yaml
```

### Required Changes

* **cluster_name**

```yaml
cluster_name: my-dfir
```

* **listen_address**

```yaml
listen_address: <HIVE_PUBLIC_IP>
```

* **rpc_address**

```yaml
rpc_address: <HIVE_PUBLIC_IP>
```

* **seed_provider**

```yaml
seeds: "<HIVE_PUBLIC_IP>"
```

### Apply Changes

```bash
systemctl stop cassandra
rm -rf /var/lib/cassandra/*
systemctl start cassandra
systemctl status cassandra
```

Cassandra must show **active (running)**.

---

## 2. Elasticsearch Installation & Configuration (TheHive)

### Installation (if missing)

```bash
apt update
apt install elasticsearch -y
```

### Configuration File

```bash
/etc/elasticsearch/elasticsearch.yml
```

### Required Changes

```yaml
cluster.name: my-dfir
node.name: node-1
network.host: <HIVE_PUBLIC_IP>
http.port: 9200
cluster.initial_master_nodes: ["node-1"]
```

### Enable & Start Service

```bash
systemctl enable elasticsearch
systemctl start elasticsearch
systemctl status elasticsearch
```

Elasticsearch must show **active (running)**.

---

## 3. File Permissions for TheHive

### Directory Ownership Fix

```bash
chown -R hive:hive /opt/thp
```

### Verification

```bash
ls -l /opt
```

Owner and group must be `hive:hive`.

---

## 4. TheHive Configuration

### Configuration File

```bash
/etc/thehive/application.conf
```

### Required Changes

* **Hostname**

```conf
hostname = "<HIVE_PUBLIC_IP>"
```

* **Elasticsearch Connection**

```conf
elasticsearch {
  host = "<HIVE_PUBLIC_IP>"
  port = 9200
}
```

* **Base URL**

```conf
application.baseUrl = "http://<HIVE_PUBLIC_IP>:9000"
```

### Start & Enable TheHive

```bash
systemctl enable thehive
systemctl start thehive
systemctl status thehive
```

---

## 5. Firewall Configuration (TheHive)

```bash
ufw allow 9000
```

### Web Access

```text
http://<HIVE_PUBLIC_IP>:9000
```

### Default Credentials

* Username: `admin@thehive.local`
* Password: `secret`

Successful login confirms TheHive is operational.

---

## 6. Wazuh Agent Deployment (Windows 11)

### Access Wazuh Dashboard

```text
https://<WAZUH_PUBLIC_IP>
```

### Agent Deployment

* Platform: **Windows**
* Server Address: `<WAZUH_PUBLIC_IP>`
* Agent Name: `my-dfir-windows`

### Install Agent (Windows PowerShell – Admin)

```powershell
<copied Wazuh agent install command>
```

### Start Agent Service

```powershell
net start wazuhsvc
```

---

## 7. Firewall Configuration (Wazuh Server)

```bash
ufw allow 1514
ufw allow 1515
```

---

## 8. Agent Validation

### Restart Agent (Windows)

```powershell
Restart-Service wazuhsvc
```

### Dashboard Verification

* Wazuh Dashboard → **Home → Overview**
* Agent status: **Active**

Successful detection confirms:

* Windows endpoint → Wazuh Manager connectivity
* Sysmon telemetry ingestion
* SIEM pipeline functioning correctly

---

## Phase Completion Criteria

* Cassandra running and reachable
* Elasticsearch running and reachable
* TheHive accessible via web UI
* Wazuh agent registered and active
* Endpoint telemetry visible in Wazuh dashboard

---

**End of Phase 2**

Next phase includes:

* Alert generation
* TheHive case creation
* Shuffle SOAR integration
* Automated SOC workflows

---

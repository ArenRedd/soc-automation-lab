# Phase 3 – Sysmon Telemetry Ingestion & Custom Detection Rule (Mimikatz)

## Objective

* Configure the Windows 11 Wazuh agent to ingest **Sysmon Operational logs**
* Validate raw Sysmon telemetry ingestion into Wazuh
* Enable Wazuh archive logging
* Create and validate a **custom detection rule** for Mimikatz execution

---

## 1. Windows 11 – Wazuh Agent Sysmon Configuration

### Backup Existing Agent Configuration

```text
C:\Program Files (x86)\ossec-agent\ossec.conf
```

Create a backup copy of `ossec.conf` before editing.

---

### Modify `ossec.conf`

Open **Notepad as Administrator** and edit:

```text
C:\Program Files (x86)\ossec-agent\ossec.conf
```

Locate the `<log_analysis>` section.

Remove default Windows Application and Security event entries.

Add Sysmon Operational log source:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

---

### Restart Wazuh Agent

```powershell
Restart-Service wazuhsvc
```

---

## 2. Sysmon Telemetry Validation

### Wazuh Dashboard

* Navigate to **Discover**
* Search for `Sysmon`

### Expected Result

* Events indexed under:

```text
Microsoft-Windows-Sysmon/Operational
```

This confirms successful Sysmon log ingestion.

---

## 3. Test Data Generation – Mimikatz Execution

### Endpoint Preparation (Lab Environment Only)

Disable Windows Defender real-time protection temporarily to allow execution.

---

### Mimikatz Execution

```powershell
.\mimikatz.exe
```

> At this stage, no alert is expected because no custom rule exists yet.

---

## 4. Wazuh Manager – Enable Full Event Archiving

### Backup Manager Configuration

```bash
cp /var/ossec/etc/ossec.conf /var/ossec/etc/ossec.conf.bak
```

---

### Modify Manager `ossec.conf`

```bash
nano /var/ossec/etc/ossec.conf
```

Enable full logging:

```xml
<logall>yes</logall>
<logall_json>yes</logall_json>
```

Save and restart manager:

```bash
systemctl restart wazuh-manager
```

---

### Verify Archive Logs

```bash
cat /var/ossec/logs/archives/archives.log
```

Raw event data should now be visible.

---

## 5. Filebeat – Enable Archive Log Shipping

### Edit Filebeat Configuration

```bash
nano /etc/filebeat/filebeat.yml
```

Locate the `archives` section and set:

```yaml
enabled: true
```

Restart Filebeat:

```bash
systemctl restart filebeat
```

---

## 6. Wazuh Dashboard – Create Archive Index Pattern

### Steps

1. Open **Dashboard Management**
2. Select **Index Patterns**
3. Create new index:

```text
wazuh-archives-*
```

4. Time field: `@timestamp`
5. Save index pattern

---

### Validation

* Navigate to **Discover**
* Select `wazuh-archives-*`
* Search for `mimikatz`

Raw execution logs should now be visible.

---

## 7. Custom Detection Rule – Mimikatz

### Edit Local Rules File

```bash
nano /var/ossec/etc/rules/local_rules.xml
```

---

### Custom Rule Definition

```xml
<rule id="100001" level="12">
  <if_group>sysmon_event1</if_group>
  <field name="win.eventdata.originalFileName">mimikatz.exe</field>
  <description>Mimikatz execution detected</description>
  <mitre>T1003</mitre>
</rule>
```

Save and restart manager:

```bash
systemctl restart wazuh-manager
```

---

## 8. Detection Validation

### Re-execute Mimikatz

```powershell
.\mimikatz.exe
```

---

### Wazuh Dashboard Verification

* Navigate to **Discover**
* Index: `wazuh-alerts-*`
* Search: `mimikatz`

### Expected Result

* Alert generated
* Rule description:

```text
Mimikatz execution detected
```

---

## Phase Completion Criteria

* Sysmon Operational logs ingested into Wazuh
* Full archive logging enabled
* Archive logs indexed and searchable
* Custom detection rule triggered successfully
* Alert visible in `wazuh-alerts-*`

---

**End of Phase 3**

Next phase includes:

* Alert forwarding to TheHive
* Shuffle SOAR workflows
* Automated case creation and enrichment


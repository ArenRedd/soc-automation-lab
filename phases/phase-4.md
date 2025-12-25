# Phase 4 – SOAR Automation with Shuffle, VirusTotal Enrichment, TheHive Alerting & Email Notification

## Objective

Integrate all SOC components into a fully automated workflow:

* Wazuh → Shuffle (SOAR)
* Automated hash extraction
* VirusTotal enrichment
* Alert creation in TheHive
* Email notification to SOC analyst

This phase validates **end-to-end SOC automation** from detection to response.

---

## 1. Shuffle Workflow Creation

### Workflow Setup

1. Log in to Shuffle
2. Navigate to **Workflows**
3. Click **Create Workflow**
4. Create workflow **from scratch**

### Workflow Name

```text
my-dfir-soc-automation
```

---

## 2. Webhook Configuration (Shuffle)

### Add Webhook Trigger

* Drag **Webhook** node into the workflow
* Copy the **Webhook URI**

---

## 3. Wazuh Manager → Shuffle Integration

### Edit Wazuh Manager Configuration

```bash
nano /var/ossec/etc/ossec.conf
```

Add the following integration block **between `<global>` and `<alerts>`**:

```xml
<integration>
  <name>shuffle</name>
  <hook_url>https://<SHUFFLE_WEBHOOK_URL></hook_url>
  <rule_id>100001</rule_id>
  <alert_format>json</alert_format>
</integration>
```

> `rule_id` corresponds to the custom Mimikatz detection rule from Phase 3.

### Restart Wazuh Manager

```bash
systemctl restart wazuh-manager
```

---

## 4. Webhook Validation

### Trigger Detection

* Execute `mimikatz.exe` on Windows 11 endpoint

### Shuffle Validation

* Workflow run appears under **Explore Runs**
* Incoming data contains:

  * Wazuh alert
  * Full log payload
  * Rule metadata

This confirms successful alert forwarding.

---

## 5. SHA-256 Hash Extraction (Shuffle Tools)

### Add Regex Node

* App: **Shuffle Tools**
* Function: **Regex capture group**

### Regex Pattern

```regex
SHA256=([0-9a-fA-F]{64})
```

### Input Source

* Runtime argument → `hashes`

### Output

* Extracted SHA-256 hash of detected binary

---

## 6. VirusTotal Integration

### Authentication

1. Generate VirusTotal API key
2. Add authentication in Shuffle
3. Assign authentication to VirusTotal app

---

### VirusTotal Action

* Action: **Get hash report**
* Hash Input:

  * Regex output → `list` (hash value only)

### Validation

* HTTP Status: `200`
* VirusTotal enrichment data returned

---

## 7. TheHive Integration

### TheHive Preparation

#### Create Organization

* Organization Name:

```text
my-dfir-soc-automation
```

#### Create Users

1. Analyst user (interactive)
2. Service account (API access)

#### Generate API Key

* Generate API key for service account
* Copy securely

---

### Shuffle → TheHive Authentication

* App: **TheHive**
* Authentication:

  * API Key
  * URL:

```text
http://<HIVE_PUBLIC_IP>:9000
```

---

## 8. Create Alert in TheHive (Advanced Mode)

### Action

* **Create Alert**

### Key Fields Configuration

```json
{
  "title": "{{title}}",
  "description": "{{title}}",
  "type": "internal",
  "source": "wazuh-alert",
  "sourceRef": "{{rule.id}}",
  "severity": 2,
  "tlp": 2,
  "pap": 2,
  "status": "New",
  "summary": "Mimikatz activity detected on host {{agent.name}}",
  "tags": ["T1003"]
}
```

> Severity values must be numeric to avoid JSON validation errors.

---

### Validation

* HTTP Status: `201`
* Alert visible in **TheHive → Alerts**

---

## 9. Email Notification (SOC Analyst)

### Add Email Node

* App: **Shuffle Email**
* Action: **Send Email (Shuffle)**

### Email Configuration

* Subject:

```text
Mimikatz Detected – SOC Alert
```

* Body:

```text
Mimikatz activity detected on {{agent.name}}.
Alert has been created in TheHive.
```

* Recipient:

```text
soc-analyst@example.com
```

---

### Validation

* Email successfully delivered
* Subject and body rendered correctly

---

## 10. Final Workflow Validation

### Trigger Event

```powershell
.\mimikatz.exe
```

### Expected Automated Flow

1. Sysmon detects execution
2. Wazuh triggers custom rule
3. Alert forwarded to Shuffle
4. SHA-256 extracted
5. VirusTotal enrichment completed
6. Alert created in TheHive
7. Email notification sent

---

## Phase Completion Criteria

* Wazuh → Shuffle integration operational
* Hash extraction successful
* VirusTotal enrichment successful
* TheHive alert created automatically
* SOC email notification delivered

---

## Project Completion

**SOC Automation Project – Complete**

This lab demonstrates:

* Endpoint telemetry ingestion
* Custom detection engineering
* SOAR-based enrichment
* Case management automation
* Analyst notification pipeline

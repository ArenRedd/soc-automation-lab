## Shuffle Setup

This document explains how Shuffle is used as a SOAR platform in this SOC automation lab.

### Webhook Ingestion from Wazuh

Shuffle is configured to receive alerts from Wazuh via a webhook. When Wazuh generates an alert, it sends a POST request to the Shuffle webhook URL. The body of the POST request contains the alert data in JSON format.

### IOC Parsing

Shuffle parses the alert data to extract any indicators of compromise (IOCs), such as IP addresses, domain names, and file hashes. This is done using a custom script that is executed when an alert is received.

### Threat Enrichment

Once the IOCs have been extracted, Shuffle enriches them with threat intelligence from VirusTotal. This is done by making a request to the VirusTotal API for each IOC. The response from the VirusTotal API is then added to the alert data.

### Case Creation

After the alert has been enriched, Shuffle creates a case in TheHive. This is done by making a request to the TheHive API. The case is created with the alert data, including the IOCs and the threat intelligence from VirusTotal.

### Analyst Notification

Once the case has been created, Shuffle sends a notification to the analyst via email. The email contains a link to the case in TheHive, as well as a summary of the alert.

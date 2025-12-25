## Shuffle Automation Workflow

This document describes the SOC automation workflow built in Shuffle.

### Workflow Steps

1.  **Wazuh Alert Ingestion**: The workflow is triggered when an alert is received from Wazuh via a webhook.
2.  **Hash/IP Extraction**: The workflow extracts any hashes or IP addresses from the alert.
3.  **VirusTotal Enrichment**: The workflow enriches the hashes and IP addresses with threat intelligence from VirusTotal.
4.  **TheHive Alert Creation**: The workflow creates an alert in TheHive with the enriched data.
5.  **Email Notification**: The workflow sends an email notification to the analyst with a link to the alert in TheHive.
6.  **Human Approval Step**: The workflow includes a human approval step, which requires an analyst to approve any active response actions before they are taken.

This workflow is designed to be a simple example of how Shuffle can be used to automate the SOC workflow. It can be extended to include additional steps, such as quarantining endpoints, blocking IP addresses, and sending notifications to other systems.
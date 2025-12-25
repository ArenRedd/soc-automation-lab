## SOC Automation Lab Architecture

The architecture of this SOC automation lab is designed to be simple, yet effective. It is comprised of the following components:

- **Endpoint**: A Windows 11 virtual machine with Sysmon and the Wazuh agent installed. Sysmon is used to collect detailed endpoint telemetry, which is then forwarded to the Wazuh manager for analysis.
- **SIEM/XDR**: A Wazuh manager running on an Ubuntu Server virtual machine. The Wazuh manager is responsible for collecting and analyzing the telemetry from the endpoint, and for generating alerts when suspicious activity is detected.
- **SOAR**: A Shuffle instance running on a separate virtual machine. Shuffle is used to orchestrate the response to alerts, including enriching the alert with threat intelligence, creating a case in TheHive, and notifying the analyst.
- **Case Management**: A TheHive instance running on a separate virtual machine. TheHive is used to manage the incident response process, including tracking the status of the incident, assigning tasks to analysts, and documenting the investigation.
- **Threat Intelligence**: VirusTotal is used to enrich alerts with threat intelligence. When an alert is generated, Shuffle will automatically query VirusTotal for information about any indicators of compromise (IOCs) associated with the alert.
- **Response**: Wazuh Active Response is used to take automated actions on the endpoint, such as blocking an IP address.

The data flow is as follows:

1.  Sysmon on the endpoint collects telemetry and forwards it to the Wazuh agent.
2.  The Wazuh agent forwards the telemetry to the Wazuh manager.
3.  The Wazuh manager analyzes the telemetry and generates an alert if suspicious activity is detected.
4.  The Wazuh manager sends the alert to Shuffle via a webhook.
5.  Shuffle parses the alert, enriches it with threat intelligence from VirusTotal, and creates a case in TheHive.
6.  Shuffle sends a notification to the analyst via email.
7.  The analyst reviews the case in TheHive and takes appropriate action.
8.  If necessary, the analyst can initiate an Active Response action from within TheHive, which will be executed by the Wazuh manager on the endpoint.
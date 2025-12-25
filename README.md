# SOC Automation Lab (Wazuh + Shuffle + TheHive)

## Overview

This project demonstrates a fully local SOC automation lab integrating SIEM, SOAR, and case management using open-source tools.

The lab simulates real-world detection, enrichment, alerting, and response workflows commonly used in Security Operations Centers (SOC).

## Architecture

- Endpoint: Windows 11 (Sysmon + Wazuh Agent)
- SIEM / XDR: Wazuh Manager (Ubuntu Server)
- SOAR: Shuffle
- Case Management: TheHive
- Threat Intelligence: VirusTotal
- Response: Wazuh Active Response

## Use Case Implemented

- Credential dumping detection (Mimikatz)
- IOC enrichment using VirusTotal
- Automated alert creation in TheHive
- Analyst email notification
- Human-in-the-loop approval
- Automated IP blocking via active response

## Disclaimer

This project is for educational and lab purposes only.
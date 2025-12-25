## Detection Use Cases

This document describes the detection use cases implemented in this SOC automation lab.

### Mimikatz Credential Dumping

This use case detects the use of Mimikatz to dump credentials from memory. The detection is based on the following:

-   A custom Wazuh rule that looks for the execution of `mimikatz.exe`.
-   A Sysmon configuration that logs process creation events.

When Mimikatz is executed, a Wazuh alert is generated and sent to Shuffle. Shuffle enriches the alert with threat intelligence from VirusTotal and creates a case in TheHive.

### SSH Brute-Force

This use case detects SSH brute-force attacks against the Wazuh manager. The detection is based on the following:

-   A default Wazuh rule that looks for multiple failed SSH login attempts from the same IP address.

When an SSH brute-force attack is detected, a Wazuh alert is generated and sent to Shuffle. Shuffle enriches the alert with threat intelligence from VirusTotal and creates a case in TheHive.
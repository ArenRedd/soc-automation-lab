This directory contains the Sysmon configuration used for endpoint telemetry collection in the SOC automation lab.

- **`sysmonconfig.xml`**: The primary Sysmon configuration file. It is based on the SwiftOnSecurity baseline and is tuned to detect suspicious activity related to credential dumping, process injection, and other common attack techniques.
- This configuration is designed to be used with Sysmon v15+ and is optimized for use in a lab environment.
- It is a critical component of the lab's detection capabilities, providing the necessary telemetry for Wazuh to analyze and generate alerts.
- The configuration is designed to be modular and easily extensible, allowing for the addition of new detection rules as the lab evolves.
- It is recommended to review and customize this configuration to fit the specific needs of your environment.
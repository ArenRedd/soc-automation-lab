## Wazuh Manager Setup

This guide provides a step-by-step guide for setting up a Wazuh manager for a local SOC lab.

### Environment

-   **OS**: Ubuntu Server 22.04
-   **Deployment**: Local VM
-   **Resources**: Resource-constrained system

### Installation

1.  **Update the system**:
    ```
    sudo apt-get update && sudo apt-get upgrade -y
    ```
2.  **Install Wazuh**:
    ```
    curl -sO https://packages.wazuh.com/4.3/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
    ```
3.  **Enable and start the services**:
    ```
    sudo systemctl daemon-reload
    sudo systemctl enable wazuh-manager
    sudo systemctl start wazuh-manager
    sudo systemctl enable wazuh-indexer
    sudo systemctl start wazuh-indexer
    sudo systemctl enable wazuh-dashboard
    sudo systemctl start wazuh-dashboard
    ```

### Agent Registration

1.  **On the Wazuh manager**, run the following command to get the agent registration command:
    ```
    /var/ossec/bin/wazuh-authd -m 127.0.0.1
    ```
2.  **On the endpoint**, run the command from the previous step to register the agent.

### Log Ingestion

1.  **On the Wazuh manager**, edit the `/var/ossec/etc/ossec.conf` file to configure log ingestion.
2.  **Add the following to the `ossec.conf` file** to enable Sysmon log ingestion:
    ```
    <localfile>
      <log_format>eventchannel</log_format>
      <location>Microsoft-Windows-Sysmon/Operational</location>
    </localfile>
    ```
3.  **Restart the Wazuh manager**:
    ```
    sudo systemctl restart wazuh-manager
    ```

### Common Pitfalls

-   **Resource constraints**: Make sure that the VM has enough resources to run the Wazuh manager. It is recommended to have at least 4GB of RAM and 2 CPUs.
-   **Firewall rules**: Make sure that the necessary firewall rules are in place to allow communication between the Wazuh manager and the agents.
-   **Log format**: Make sure that the log format is set correctly for the logs that you are ingesting.
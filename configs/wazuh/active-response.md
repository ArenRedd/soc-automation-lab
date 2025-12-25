## Wazuh Active Response

Wazuh Active Response is a critical component of this SOC automation lab. It allows for automated actions to be taken on endpoints when specific threats are detected.

### Triggers

Active Response is triggered by Wazuh alerts that meet a certain criteria. In this lab, the primary trigger is the detection of Mimikatz activity. When the `mimikatz-detection.xml` rule is triggered, the Active Response module is initiated.

### Action

The action taken by Active Response is to block the IP address of the offending endpoint. This is done by adding a firewall rule to the endpoint's host-based firewall. This action is taken automatically and does not require human intervention.

### Human Approval

While the IP blocking action is automated, a human approval step is included in the Shuffle workflow. This is to ensure that the IP address is not blocked without a human first reviewing the alert and confirming that it is a true positive. This is a critical security consideration, as blocking an IP address can have a significant impact on the availability of a system.

### Security Considerations

- It is important to ensure that the Active Response module is configured correctly and that the actions it takes are appropriate for the environment.
- It is also important to have a process in place for reviewing and approving Active Response actions to prevent false positives from causing an outage.
- In a production environment, it is recommended to have a more sophisticated Active Response workflow that includes additional steps, such as sending a notification to the security team, creating a ticket in a help desk system, and quarantining the endpoint.

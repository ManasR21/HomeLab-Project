# Investigation & Defense with Wazuh

## Overview
After witnessing the attack unfold, the next step is to analyze and investigate the intrusion using Wazuh, a security monitoring and threat detection tool. Wazuh collects logs, detects anomalies, and provides alerts that can help identify the attack path and mitigate threats.

## Log Analysis
### 1. Checking Logs for Suspicious Activities
- Navigate to **Wazuh Dashboard**.
- Go to **Discover** and filter logs based on timestamps when the attack was executed.
- Look for abnormal **SSH login attempts** on the email server (`10.0.0.8`).
- Check logs for brute force attempts, specifically failed login records and eventual success using Hydra.

### 2. Identifying Lateral Movement
- Inspect logs related to **Windows Remote Management (WinRM)** and **RDP (Remote Desktop Protocol)** connections.
- Look for **PowerShell execution logs** that indicate privilege escalation and lateral movement.
- Search for logs showing unauthorized users gaining access to administrative privileges.

### 3. Detecting Data Exfiltration
- Check logs for **SCP (Secure Copy Protocol)** usage.
- Identify any unauthorized file transfers between compromised machines and external systems.
- Analyze **network traffic logs** to detect large outbound data transfers.

## Alerts and Detections
### 1. Brute Force Detection
- Wazuh generates alerts for repeated failed login attempts.
- Review alerts under **Security Events** for excessive SSH and RDP authentication failures.

### 2. Unauthorized Privilege Escalation
- Investigate alerts on newly created **Domain Admins** accounts.
- Look for **scheduled tasks** created for persistence (reverse shell execution).
- Check for unauthorized PowerShell scripts executed by non-admin users.

### 3. Persistence Mechanisms
- Inspect logs for new **user accounts** created (`project-x-user`).
- Look for **Windows Task Scheduler** entries executing PowerShell scripts.
- Detect antivirus and security service disablement under **Event Viewer logs**.

## Response and Mitigation
### 1. Containing the Attack
- **Disable compromised accounts** (`janed@linux-client`, `project-x-user`).
- **Revoke unauthorized privileges** from any newly created accounts.
- **Kill suspicious processes** running malicious PowerShell scripts.

### 2. Removing Persistence
- **Delete scheduled tasks** executing malicious scripts.
- **Restore security configurations**, including firewall rules and antivirus settings.
- **Reset passwords** for all compromised accounts.

### 3. Strengthening Defense
- Enforce **multi-factor authentication (MFA)** for SSH and RDP.
- Implement **network segmentation** to restrict lateral movement.
- Regularly update **Wazuh policies** to detect and prevent future attacks.

## Conclusion
By leveraging Wazuh for investigation, we were able to track the attacker's movements, identify compromised accounts, detect data exfiltration attempts, and remove persistence mechanisms. Strengthening security policies and monitoring suspicious activities can help prevent similar attacks in the future.


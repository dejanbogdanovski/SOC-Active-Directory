# 🛡️ Active Directory & SOC Detection Lab

## 📖 Project Overview
This project demonstrates the deployment of a full-scale corporate network environment within a virtualized infrastructure. The goal was to simulate a real-world **RDP Brute Force attack** and perform deep-dive analysis using **Splunk (SIEM)** to detect and investigate security incidents.

---

## 🛠️ Lab Components & Tools
* **Virtualization:** Oracle VirtualBox (Running 4 machines)
* **SIEM:** Splunk Enterprise (Ubuntu 64-bit)
* **Domain Controller:** Windows Server 2022 (ADDC)
* **Endpoint:** Windows 10 (Target-PC)
* **Attacker Machine:** Kali Linux (Hydra)
* **Telemetry:** Windows Security Auditing & Splunk Universal Forwarder

![Lab Infrastructure](SOC%20Active%20Directory/lab-infrastructure.png)
*Figure 1: VirtualBox Environment showing all active nodes.*

---

## 🏗️ Phase 1: Infrastructure & Active Directory Setup
I established a secure network topology using a dedicated NAT Network.

### 1. Identity & Directory Management
* Configured the `mydfir.local` domain and established a structured **Organizational Unit (OU)** hierarchy (IT, HR, Users).
* Created a domain user **"Jenny Smith"** (`jsmith`) to simulate a corporate identity.

### 2. Network Stability
* Configured **DNS Forwarders** (8.8.8.8) on the ADDC to ensure name resolution for telemetry forwarding.

![Active Directory Structure](SOC%20Active%20Directory/active-directory-users.png)
*Figure 2: ADUC showing the 'IT' Organizational Unit and 'jsmith' user account.*

---

## ⚔️ Phase 2: Simulating the Attack (RDP Brute Force)
To generate actionable telemetry, I performed an RDP brute-force attack from the Kali Linux machine.

* **Tool:** Hydra v9.6
* **Target:** `10.0.2.12` (Windows 10 Endpoint)
* **Strategy:** Using a password list to attempt unauthorized entry via port 3389.

![Hydra Attack Execution](SOC%20Active%20Directory/hydra-attack.png)
*Figure 3: Real-time execution of the brute force attack using Hydra on Kali Linux.*

---

## 🔍 Phase 3: SOC Analysis & Detection
The core of the project involves identifying the attack signature within **Splunk**.

### 1. Traffic Analysis & Spikes
* Identified a significant surge of **4,943 events** occurring during the attack window.
* The histogram clearly shows the density of the automated brute force attempts.

![Splunk Dashboard](SOC%20Active%20Directory/splunk-detection-overview.png)
*Figure 4: Splunk search results highlighting the 4,943 events detected.*

### 2. Event Log Forensics
* **Event Code 4625/4634:** Deep-dive analysis of the security logs revealed thousands of logon failures.
* **User Impact:** Logs confirmed the targeted account was `jsmith` on the `target-PC` host.

![Event Log Analysis](SOC%20Active%20Directory/event-log-analysis.png)
*Figure 5: Detailed forensic view of a failed logon event in Splunk.*

---

## 🎯 Key Takeaways
1. **Auditing is Crucial:** Without **Advanced Audit Policy** configuration, these 4,000+ login attempts would have been invisible.
2. **SIEM Integration:** Splunk successfully ingested telemetry from the **Universal Forwarder**, providing real-time visibility.
3. **Defense in Depth:** This lab demonstrates how monitoring account activity can reveal automated threats.

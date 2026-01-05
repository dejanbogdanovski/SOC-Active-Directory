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

---

## 🏗️ Phase 1: Infrastructure & Active Directory Setup
I established a secure network topology using a dedicated NAT Network.

### 1. Identity & Directory Management
* Configured the `mydfir.local` domain and established a structured **Organizational Unit (OU)** hierarchy (IT, HR, Users).
* Created a domain user **"Jenny Smith"** (`jsmith`) to simulate a corporate identity targeted by attackers.

### 2. Network Stability
* Configured **DNS Forwarders** (8.8.8.8) on the ADDC to ensure name resolution for external telemetry forwarding.

![Active Directory Structure](2.png)
*Figure 1: ADUC showing the 'IT' Organizational Unit and 'jsmith' user account.*

---

## ⚔️ Phase 2: Simulating the Attack (RDP Brute Force)
To generate actionable telemetry, I performed an RDP brute-force attack from the Kali Linux machine.

* **Tool:** Hydra v9.6
* **Target:** `10.0.2.12` (Windows 10 Endpoint)
* **Strategy:** Using a password list (`passwords.txt`) to attempt unauthorized entry via port 3389.

![Hydra Attack Execution](4.jpg)
*Figure 2: Real-time execution of the brute force attack using Hydra on Kali Linux.*

---

## 🔍 Phase 3: SOC Analysis & Detection
The core of the project involves identifying the attack signature within **Splunk**.

### 1. Traffic Analysis & Spikes
* By analyzing the logs over "All Time," I identified a significant surge of **4,943 events** occurring during the attack window.
* The histogram clearly shows the density of the automated brute force attempts.

### 2. Event Log Forensics
* **Event Code 4625/4634:** Deep-dive analysis of the security logs revealed thousands of logon failures.
* **User Impact:** The logs explicitly confirmed the targeted account was `jsmith` on the `target-PC` host, stemming from the attacker's IP.

![Splunk Dashboard](5.png)
*Figure 3: Splunk search results highlighting the 4,943 events detected during the analysis.*

---

## 🎯 Key Takeaways
1. **Auditing is Crucial:** Without **Advanced Audit Policy** (GPO) configuration, these 4,000+ login attempts would have been invisible to the SIEM.
2. **SIEM Correlation:** Splunk successfully ingested telemetry from the **Universal Forwarder**, allowing for real-time visibility into endpoint security.
3. **Attack Patterns:** The data clearly distinguishes between a manual login mistake and a scripted automated attack.

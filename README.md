🛡️ Active Directory & SOC Detection Lab
📖 Project Overview
This project demonstrates the deployment of a full-scale corporate network environment within a virtualized infrastructure. The goal was to simulate a real-world Brute Force attack and perform a deep-dive analysis using Splunk (SIEM) to detect and investigate security incidents.

🛠️ Lab Components & Tools
Virtualization: Oracle VirtualBox.

SIEM: Splunk Enterprise (Ubuntu 64-bit).

Domain Controller: Windows Server 2022 (ADDC).

Endpoint: Windows 10 (Target-PC).

Attacker Machine: Kali Linux (Hydra).

Telemetry: Windows Security Logs & Universal Forwarder.

🏗️ Phase 1: Infrastructure & Active Directory Setup
I built a secure network topology where all machines reside within a dedicated NAT Network.

Active Directory Configuration: I configured the mydfir.local domain and established an Organizational Unit (OU) structure (IT, HR, Users).

Identity Management: Created a domain user "Jenny Smith" (jsmith) to simulate a typical corporate employee identity.

Network Services: Configured DNS Forwarders (8.8.8.8) on the ADDC to ensure stable name resolution and external connectivity for telemetry forwarding.

⚔️ Phase 2: Simulating the Attack (RDP Brute Force)
To test the detection capabilities, I performed a credential-stuffing attack from the Kali Linux machine.

Tool: Hydra.

Command: hydra -l jsmith -P passwords.txt 10.0.2.12 rdp.

Execution: The attack targeted the Windows 10 endpoint, attempting multiple password combinations against the RDP service.

🔍 Phase 3: SOC Analysis & Detection
The core of the project involves monitoring the generated telemetry in Splunk.

Event Correlation: By switching the time range to "All Time," I identified a massive spike of 4,943 events related to the user jsmith.

Log Investigation: Deep analysis of the security logs revealed Event Code 4634/4625 (Logon Failures/Logoffs).

Forensic Findings: The logs explicitly show the targeted account jsmith on the TARGET-PC, providing clear evidence of the brute force attempt.

🎯 Key Takeaways
Visibility is Key: Without proper Group Policy (GPO) auditing, these 4,000+ attempts would have gone unnoticed.

SIEM Integration: Splunk effectively correlated the distributed logs from the Windows endpoint into a readable timeline of the attack.

Defense in Depth: This lab highlights the importance of monitoring account lockout events as a primary defense against automated attacks.

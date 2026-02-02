# Cloud-Native-Threat-Intelligence-SOC-Lab
This project demonstrates the implementation of a cloud-based honeypot designed to attract and monitor live cyber attacks from across the globe. Using Microsoft Azure, I deployed a vulnerable Ubuntu virtual machine to serve as a decoy, then routed the attack data (Syslog) through Log Analytics to Microsoft Sentinel.
The core objective is to visualize real-time "Intrusion Attempts" on a world map, providing insights into attacker geolocations, common usernames used in brute-force attacks, and targeted protocols.
Architecture

The following diagram illustrates the data flow and system components:
<img width="1610" height="746" alt="image" src="https://github.com/user-attachments/assets/bde20c41-c853-4699-a7c2-18b431cb3dfb" />

Components & Workflow:
- Attacker (Bots): Represents automated scripts and malicious actors on the internet attempting to gain unauthorized access.
- Azure VNET / Subnet: A controlled cloud environment with a Firewall configured with open ports to allow all incoming traffic to the honeypot.
- Honeypot VM (Ubuntu Linux): The decoy server that records all unauthorized access attempts.
- Log Analytics (Data Ingestion): Centralizes the collection of Syslog data from the Linux virtual machine.
- Microsoft Sentinel (SIEM & Map): The Security Information and Event Management (SIEM) tool used to analyze the logs and generate a Map Visualization.
- SOC Analyst: The end-user (me) who monitors the dashboard to identify patterns and origin points of the attacks.

Tech Stack
- Cloud Provider: Microsoft Azure.
- Operating System: Ubuntu Linux.
- Security Tools: Microsoft Sentinel, Log Analytics Workspace.
- Documentation: Diagrams-as-Code (Python).

  Phase 2: Implementation & Live Monitoring
1. Real-Time Intrusion Monitoring

Once the Honeypot was exposed to the internet, automated bots immediately began discovery and exploitation attempts. I monitored the /var/log/auth.log file in real-time to observe the attack patterns.

<img width="1832" height="288" alt="image" src="https://github.com/user-attachments/assets/dc6f86e1-f488-400d-9ee6-0c47dd0282c6" />

<img width="1897" height="545" alt="image" src="https://github.com/user-attachments/assets/d936d713-c65f-4783-88f6-974cea2dc44d" />
<img width="1909" height="524" alt="image" src="https://github.com/user-attachments/assets/9c2208a1-f945-4974-af12-d9c707cd5d12" />

        Evidence: The capture above shows a persistent brute-force attack from IP 80.94.92.171.

        Analysis: The attacker targeted common administrative accounts like sol and ubuntu, attempting multiple logins per minute.

2. Data Ingestion & Agent Telemetry

To bridge the gap between the Linux VM and the Azure Cloud, I deployed the Azure Monitor Agent (AMA).
<img width="1093" height="1017" alt="image" src="https://github.com/user-attachments/assets/53633e7a-c9d3-4570-a0bc-8776c48823ec" />
- Agent Setup: As shown in the logs, the installation automatically configured the azuremetricsext user and added it to the syslog group, granting necessary permissions to stream authentication logs to the cloud.
  
Phase 3: SIEM Analytics & Health Validation
1. Connectivity & Pipeline Health Check
Before analyzing the data, I validated the end-to-end data pipeline using Kusto Query Language (KQL) to ensure telemetry was reaching the SIEM.
<img width="987" height="1074" alt="image" src="https://github.com/user-attachments/assets/fd36dacf-dc3c-4747-9663-1e01a1e59378" />

- Validation: The successful "Heartbeat" signal confirmed a healthy connection between the VM and the Log Analytics Workspace at 19:39 UTC.

3. Security Incident Management

Microsoft Sentinel automatically transformed the raw telemetry into actionable security incidents.

<img width="1058" height="787" alt="image" src="https://github.com/user-attachments/assets/02fc6d74-b3c6-498e-be78-2c2147aa1a4e" />

- Metrics: Within the monitoring period, the system identified 46 security incidents, with 43 flagged as High Severity due to coordinated and persistent brute-force activities.
- Efficiency: The SIEM provided a Mean Time to Close (MTTC) of 39 minutes, demonstrating effective automated categorization of global threats.

  Phase 4: Threat Visualization (Geographic Mapping)

I developed custom KQL queries to parse raw IP addresses from Syslog and map them to their physical origin.

- Outcome: Despite standard cloud ingestion latency, the system successfully identified and prepared mapping data for dozens of global unique attackers.

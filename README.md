# Cloud-Native-Threat-Intelligence-SOC-Lab
This project demonstrates the implementation of a cloud-based honeypot designed to attract and monitor live cyber attacks from across the globe. Using Microsoft Azure, I deployed a vulnerable Ubuntu virtual machine to serve as a decoy, then routed the attack data (Syslog) through Log Analytics to Microsoft Sentinel.
The core objective is to visualize real-time "Intrusion Attempts" on a world map, providing insights into attacker geolocations, common usernames used in brute-force attacks, and targeted protocols.
Architecture

The following diagram illustrates the data flow and system components:
<img width="1610" height="746" alt="image" src="https://github.com/user-attachments/assets/bde20c41-c853-4699-a7c2-18b431cb3dfb" />

Components & Workflow:

    Attacker (Bots): Represents automated scripts and malicious actors on the internet attempting to gain unauthorized access.
    Azure VNET / Subnet: A controlled cloud environment with a Firewall configured with open ports to allow all incoming traffic to the honeypot.
    Honeypot VM (Ubuntu Linux): The decoy server that records all unauthorized access attempts.
    Log Analytics (Data Ingestion): Centralizes the collection of Syslog data from the Linux virtual machine.
    Microsoft Sentinel (SIEM & Map): The Security Information and Event Management (SIEM) tool used to analyze the logs and generate a Map Visualization.
    SOC Analyst: The end-user (me) who monitors the dashboard to identify patterns and origin points of the attacks.

Tech Stack

    Cloud Provider: Microsoft Azure.
    Operating System: Ubuntu Linux.
    Security Tools: Microsoft Sentinel, Log Analytics Workspace.
    Documentation: Diagrams-as-Code (Python).

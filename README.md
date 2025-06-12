
This repository contains documentation, configurations, and detection rules for deploying Wazuh (XDR/SIEM) 

Linux Mint

pfSense Firewalls

Windows Server

Debian Systems

This repository provides a complete A-to-Z guide for deploying:

✅ Centralized security monitoring (SIEM/XDR)

✅ Multi-platform threat detection (Linux/Windows/Network)

✅ Real-time alerting system

✅ Critical security integrations (Firewalls, Web/DNS, IDS)

🎯 Key Features
✔ Multi-Platform Support – Wazuh agents on Linux, Windows, and firewalls
✔ Threat Detection Rules – Cobalt Strike, Nmap scans, Sysmon, audit logs
✔ Security Tool Integrations – Apache, Bind9, Suricata
✔ Alerting & Dashboard – Email alerts + Wazuh UI customization

📂 Repository Structure
📂 Linux Mint

installation linux mint.docx	OS setup guide
installation agent wazuh sur linux.docx	Wazuh agent install
apache sur linux-mint.docx	Apache web server config
Bind9 sur linux-mint.docx	DNS server setup
cobalt_regles.txt	Cobalt Strike detection rules
Sysmon sur linux.docx	Sysmon monitoring
📂 pfSense

installation pfsense.docx	Firewall setup
installation suricata sur pfsense.docx	Suricata IDS integration
Wazuh agent on pfSense.docx	Agent deployment
📂 Wazuh Server

installation-wazuh-recommendée.docx	Recommended install
Dashboard sur wazuh.docx	UI customization
email alerte.docx	Email alert setup
regles detection nmap scan.txt	Nmap scan detection
📂 Windows Server

installation agent wazuh sur windows.docx	Windows agent setup
Sysmon sur windows.docx	Sysmon for Windows
📂 Planning
plan de travail.pdf (Project roadmap)

🚀 Getting Started
Set up base systems (Linux, Windows, pfSense).

Install Wazuh (Server + Agents).

Configure detection rules (Nmap, Cobalt Strike, etc.).

Enable alerts & customize dashboard.


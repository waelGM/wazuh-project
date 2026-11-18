## Wazuh Lab

Documentation, configurations and detection rules for deploying Wazuh (XDR/SIEM) on Linux Mint, pfSense, Windows Server and Debian.

## Getting started

1. Set up the base systems (Linux, Windows, pfSense): see the Infrastructure notes below.
2. Install Wazuh (server and agents): see [[Install Wazuh (Recommended)]] and the agent notes.
3. Configure detection rules (Nmap, Cobalt Strike and others): see the Detection notes.
4. Enable alerts and customize the dashboard: [[Configure Email Alerts]], [[Customize the Dashboard]].

## Notes

### Planning

- [[Lab Overview]]

### Infrastructure

- [[Create a VMware Virtual Machine]]
- [[Install Windows Server 2019]]
- [[Configure Active Directory and Server Roles]]
- [[Share and Audit a Folder (Windows Server)]]
- [[Install Linux Mint]]
- [[Install Debian]]
- [[Configure Bind9 DNS Server]]
- [[Configure Apache Web Server]]
- [[Configure pfSense]]

### Wazuh Server

- [[Install Wazuh (Recommended)]]
- [[Deploy Wazuh Agent (Windows)]]
- [[Deploy Wazuh Agent (Linux)]]
- [[Deploy Wazuh Agent (pfSense)]]
- [[Customize Agent Names and Remove an Agent]]
- [[Customize the Dashboard]]
- [[Configure Email Alerts]]

### Telemetry

- [[Install Sysmon (Windows)]]
- [[Install Sysmon (Linux)]]
- [[Install Suricata on pfSense]]
- [[Integrate Nmap with Wazuh]]

### Detection

- [[Detect Port Scan (Suricata)]]
- [[Detect Startup Persistence]]
- [[Detect Web DoS]]
- [[Detect Cobalt Strike]]
- [[Detect Ping]]
- [[Detect RDP Brute Force]]
- [[Detect Critical Folder Access]]


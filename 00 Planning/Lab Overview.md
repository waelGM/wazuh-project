![[Work plan.png]]
![[Network Diagram.png]]
## What the project is

A repository of documentation, configurations and detection rules for deploying Wazuh (XDR/SIEM) on Linux Mint, pfSense firewalls, Windows Server and Debian systems. It is an A-to-Z guide for:

- Centralized security monitoring (SIEM/XDR)
- Multi-platform threat detection (Linux, Windows, network)
- A real-time alerting system
- Critical security integrations (firewalls, web/DNS, IDS)

Key features:

- Multi-platform support: Wazuh agents on Linux, Windows and firewalls
- Threat detection rules: Cobalt Strike, Nmap scans, Sysmon, audit logs
- Security tool integrations: Apache, Bind9, Suricata
- Alerting and dashboard: email alerts and Wazuh UI customization

## Work plan (plan de travail.pdf)

The work plan is a network diagram of the lab. Its elements:

- Internet service provider (FAI), switch (commutateur) with ports Fa0/1 and Fa0/2, RJ45 Ethernet cables, DHCP
- Network `192.168.1.0/24`
- Three desktop computers: Poste 1, Poste 2, Poste 3
- Virtual machines: Windows Server, Linux Mint, Kali Linux, Wazuh, pfSense, Windows 10
- Services on the Windows Server: Apache, DNS, IIS, AD, and the client `Client-win-server`

## Machines documented

| Machine | Documented in |
|---|---|
| Windows Server 2019 (AD DS, DNS, web server, file share) | [[Install Windows Server 2019]], [[Configure Active Directory and Server Roles]], [[Share and Audit a Folder (Windows Server)]] |
| Linux Mint (Bind9, Apache) | [[Install Linux Mint]], [[Configure Bind9 DNS Server]], [[Configure Apache Web Server]] |
| Debian (Wazuh server) | [[Install Debian]], [[Install Wazuh (Recommended)]] |
| pfSense | [[Configure pfSense]], [[Install Suricata on pfSense]] |
| Kali Linux (attacker, used in the attack tests) | [[Detect Port Scan (Suricata)]], [[Detect Web DoS]], [[Detect RDP Brute Force]] |
| Windows endpoint (tests) | [[Detect Ping]], [[Detect Cobalt Strike]], [[Detect Startup Persistence]] |

All machines are virtual, created in VMware: [[Create a VMware Virtual Machine]].

## IP addresses

The different ranges correspond to different stages of the lab.

| Address | Where it appears |
|---|---|
| `192.168.1.0/24` | Network in the work plan |
| `192.168.1.1` | pfSense, target of the port scan: [[Detect Port Scan (Suricata)]] |
| `192.168.1.16` | Windows endpoint, target of the RDP test: [[Detect RDP Brute Force]] |
| `192.168.1.21` | Example web server address: [[Detect Web DoS]] |
| `10.10.10.0/24` | pfSense LAN range: [[Configure pfSense]] |
| `10.10.10.1` | pfSense LAN interface: [[Configure pfSense]] |
| `10.10.10.52` | Example address a VM received from pfSense: [[Configure pfSense]] |
| `10.0.0.2` | Wazuh manager address in the agent install commands, to be replaced with your own: [[Deploy Wazuh Agent (Linux)]] |
| `127.0.0.1` | Default subnet in the Nmap script: [[Integrate Nmap with Wazuh]] |

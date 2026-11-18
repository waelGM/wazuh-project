## Test of the Suricata alerts (port scan)

To test Suricata with Wazuh, go to your Wazuh server, click **Active**, and choose the machine on which Suricata is installed. In this lab it is the pfSense.

Go to your Kali Linux machine and simulate a port scan with Nmap. Run this command (`192.168.1.1` is the pfSense machine of the lab):

```bash
nmap -sS 192.168.1.1
```

When scanning the open ports of the pfSense with Nmap from the Kali Linux machine, you should receive results on the Wazuh server.

The agents of the lab (IDs, names, IP addresses and operating systems):

![[detect-port-scan-suricata--extra-02--agent-list.png]]
*the agents of the lab: Server 192.168.1.5, DESKTOP-JHSFK83 192.168.1.10, kali 192.168.1.20, wagh-GM (Linux Mint 21.3) 192.168.1.19, P102-26 192.168.1.16, PC1-MTL 192.168.1.11, P102-27 192.168.1.13, pfSense.Defender.ca 192.168.1.1*

The output of the scan (ports 53, 80 and 443 open, 997 filtered):

![[detect-port-scan-suricata--extra-06--nmap-scan-output.png]]
*`nmap -sS 192.168.1.1` from Kali*

The Suricata alerts that appear on the Wazuh server (`rule.id` 86601, level 3) are of the form "Suricata: Alert - ET SCAN Suspicious inbound to PostgreSQL port 5432" (also Oracle SQL 1521, MSSQL 1433, mySQL 3306 and "Potential VNC Scan 5800-5820"):

![[detect-port-scan-suricata--extra-05--suricata-scan-alerts.png]]
*Suricata alerts for the scan*

## Related

[[Install Suricata on pfSense]] · [[Deploy Wazuh Agent (pfSense)]]

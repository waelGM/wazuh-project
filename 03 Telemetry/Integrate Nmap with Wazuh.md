## Step 1: Python

Install Python from python.org, and install `python-nmap`. Run this command in `cmd` as administrator:

```powershell
pip install python-nmap
```

## Step 2: Nmap

Install Nmap. Make sure to tick these two options and leave the rest at default.

![[integrate-nmap-with-wazuh--fig-01--nmap-installer-options.png]]
*the two options to tick in the Nmap installer*

## Step 3: The script

Create a script named `nmapscan.py` with this content:

```python
#!/usr/bin/env python3
import nmap
import time
import json
import platform

def scan_subnet(subnet):
    nm = nmap.PortScanner()
    nm.scan(subnet)
    results = []
    for host in nm.all_hosts():
        for proto in nm[host].all_protocols():
            if proto not in ["tcp", "udp"]:
                continue
            lport = list(nm[host][proto].keys())
            lport.sort()
            for port in lport:
                hostname = ""
                json_output = {
                    'nmap_host': host,
                    'nmap_protocol': proto,
                    'nmap_port': port,
                    'nmap_hostname': "",
                    'nmap_hostname_type': "",
                    'nmap_port_name': "",
                    'nmap_port_state': "",
                    'nmap_port_service': ""
                }
                if nm[host]["hostnames"]:
                    hostname = nm[host]["hostnames"][0]["name"]
                    hostname_type = nm[host]["hostnames"][0]["type"]
                    json_output['nmap_hostname'] = hostname
                    json_output['nmap_hostname_type'] = hostname_type
                if 'name' in nm[host][proto][port]:
                    json_output['nmap_port_name'] = nm[host][proto][port]['name']
                if 'state' in nm[host][proto][port]:
                    json_output['nmap_port_state'] = nm[host][proto][port]['state']
                if 'product' in nm[host][proto][port] and 'version' in nm[host][proto][port]:
                    service = nm[host][proto][port]['product'] + " " + nm[host][proto][port]['version']
                    json_output['nmap_port_service'] = service
                results.append(json_output)
    return results

def append_to_log(results, log_file):
    with open(log_file, "a") as active_response_log:
        for result in results:
            active_response_log.write(json.dumps(result))
            active_response_log.write("\n")

subnets = ['127.0.0.1']  # Modify to scan your network (e.g., '192.168.1.0/24')

if platform.system() == 'Windows':
    log_file = "C:\\Program Files (x86)\\ossec-agent\\active-response\\active-responses.log"
elif platform.system() == 'Linux':
    log_file = "/var/ossec/logs/active-responses.log"
else:
    log_file = "/Library/Ossec/logs/active-responses.log"

for subnet in subnets:
    results = scan_subnet(subnet)
    append_to_log(results, log_file)
    time.sleep(2)
```

## Step 4: Build an executable

Install PyInstaller to create executable files. Open `cmd` as administrator and run:

```powershell
pip install pyinstaller
```

Then go to the directory, or give the path, and run:

```powershell
pyinstaller -F <path to NMAPSCAN.PY>
```

If you use PowerShell:

```powershell
pyinstaller.exe -F <path to NMAPSCAN.PY>
```

## Step 5: Run it from the Wazuh agent

Open `ossec.conf` of your Wazuh agent, or from your Wazuh server, and add the block below. It indicates the location of the script and that it will be run once a week. Just change `USERNAME` and restart the agent.

```xml
<localfile>

  <log_format>full_command</log_format>

  <command>C:\Users\USERNAME\Documents\nmapscan.exe</command>

  <frequency>604800</frequency>  <!-- Runs weekly (in seconds) -->

</localfile>
```

## Step 6: The rule

Create a rule on the Wazuh server, name it `nmapscan.xml`, add these lines and restart the service:

```xml
<group name="linux,nmap,network_scan,">
  <rule id="100100" level="3">
    <decoded_as>json</decoded_as>
    <field name="nmap_port">\.+</field>
    <field name="nmap_port_service">\.+</field>
      <description>NMAP:La Port $(nmap_port) est ouvert et en march $(nmap_port_service).</description>
    <options>no_full_log</options>
  </rule>
</group>
```

The description is in French: "NMAP: Port `<port>` is open and running `<service>`."

## Step 7: Quick test

Go to the location of your `nmapscan.exe` file, run it as administrator and wait for it to finish. Here is the result on the Wazuh server:

![[integrate-nmap-with-wazuh--fig-02--result-on-wazuh-server.png]]
*the scan result on the Wazuh server: "NMAP: La Port 135 est ouvert et en march Microsoft Windows RPC service.", agent `DESKTOP-JHSFK83`, level 3*

## Related

[[Deploy Wazuh Agent (Windows)]] · [[Detect Port Scan (Suricata)]]

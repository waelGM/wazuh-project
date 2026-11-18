Monitoring of the Suricata logs.

## Install Suricata

If Suricata is not installed in your pfSense, follow these steps. Search for `suricata`; a package appears; click **Install**. If all goes well you will find it here:

![[install-suricata-on-pfsense--fig-01--suricata-installed.png]]
*where Suricata appears after installation*

## Add the interface

After installing Suricata, connect to your pfSense box and go to **Services → Suricata**. You should see the list of interfaces on which Suricata runs. You will probably find no interface. Add the interface that goes toward your Wazuh manager; in the author's case it is LAN `em1`. You can also enable it on several interfaces. Tick here if it is not already done:

![[install-suricata-on-pfsense--fig-02--interface-enable.png]]
*the box to tick to enable the interface*

## Offloading

Go to **System → Advanced → Networking** and make sure to tick the 3 offloading options (hardware checksum offloading, hardware TCP segmentation offloading and hardware large receive offloading are disabled).

![[install-suricata-on-pfsense--extra-12--offloading-options.png]]
*the three offloading options ticked*

Click here to restart the service on the interfaces you associated with Suricata:

![[install-suricata-on-pfsense--fig-03--restart-service.png]]
*the button to restart the service on the interfaces*

## Emerging Threats rules

Before configuring Suricata, install the Emerging Threats rule set. Go to **Services → Suricata → Global Settings** and tick **Install ETOpen Emerging Threats rules**.

![[install-suricata-on-pfsense--fig-04--etopen-rules.png]]
*install ETOpen Emerging Threats rules*

Then go to **Updates** and click **Update**. The rule installation starts. When it finishes, go to **Interfaces → Edit interface Suricata → LAN Categories**, choose **Select all**, and remove certain rules such as `dnp3-event` or `modbus-events`, or any rule you judge incompatible with what you want to do. Click **Save**, then **LAN Rules** to see the changes.

![[install-suricata-on-pfsense--fig-05--edit-interface.png]]
*editing the Suricata interface*

![[install-suricata-on-pfsense--fig-06--rules-installation.png]]
*the installation of the rules*

![[install-suricata-on-pfsense--fig-07--rules-verification.png]]
*verification of the rules*

## Rules permissions

Type this command in the pfSense shell. The folder name is the name of the Suricata interface (see the interface name figure below), which will probably differ for you:

```bash
chmod 640 /usr/local/etc/suricata/<Suricata interface name>/rules
```

In this lab the interface folder is `suricata_21578_em1`, so the path is `/usr/local/etc/suricata/suricata_21578_em1/rules`.

![[install-suricata-on-pfsense--fig-08--interface-name.png]]
*the name of the Suricata interface*

## Send the Suricata log to Wazuh

Edit `/var/ossec/etc/ossec.conf` and add this part at the end of the file:

```xml
<localfile>

    <log_format>json</log_format>

    <location>/var/log/suricata/*/eve.json</location>

  </localfile>
```

![[install-suricata-on-pfsense--fig-09--ossec-conf.png]]
*modifying `ossec.conf`*

## `suricata.yaml`

Go to `/usr/local/etc/suricata/suricata.yaml` and make the following changes. To find the interface name, run `ifconfig` or `ip a`.

- Put the IP address of the Wazuh server.
- Comment out the first entry and uncomment the second.
- Put the correct interface.
- Put the correct location of the rules.

![[install-suricata-on-pfsense--fig-10--suricata-yaml-wazuh-ip.png]]
*put the IP address of the Wazuh server*

![[install-suricata-on-pfsense--fig-11--suricata-yaml-comment.png]]
*comment the first entry and uncomment the second*

![[install-suricata-on-pfsense--fig-12--suricata-yaml-interface.png]]
*put the correct interface*

![[install-suricata-on-pfsense--fig-13--suricata-yaml-rules-location.png]]
*put the correct rules location*

The values:

```yaml
# Wazuh IP / address group
HOME_NET: "[192.168.1.1]"

# first entry commented, second active
#EXTERNAL_NET: "!$HOME_NET"
EXTERNAL_NET: "any"

# interface
af-packet:
  - interface: em1

# rules location
default-rule-path: /usr/local/etc/suricata/suricata_21578_em1/rules
rule-files:
  - suricata.rules
```

## The most important

Edit `/etc/rc.conf`. Replace `em1` with your interface and save:

```text
suricata_enable="YES"
suricata_interface="em1"
```

`/etc/rc.conf` also contains the line `wazuh_agent_enable="YES"` above these two lines (see [[Deploy Wazuh Agent (pfSense)]]):

![[install-suricata-on-pfsense--extra-19--rc-conf.png]]
*`/etc/rc.conf`*

In the shell, paste this command to improve performance:

```bash
sysctl -w net.bpf.zerocopy_enable=1
```

Finally:

```bash
service suricata enable
service suricata restart
```

![[install-suricata-on-pfsense--fig-14--enable-and-start.png]]
*enable and start the service*

## Related

[[Deploy Wazuh Agent (pfSense)]] · [[Detect Port Scan (Suricata)]] · [[Configure pfSense]]

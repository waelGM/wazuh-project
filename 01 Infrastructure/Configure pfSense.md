## Interfaces

The setup starts with two interfaces, LAN and WAN, and the step to assign them and configure the necessary settings.

![[configure-pfsense--fig-01--lan-wan-interfaces.png]]
*the LAN and WAN labels*

![[configure-pfsense--fig-02--interface-assignment.png]]
*assign the interfaces and configure the necessary settings*

The VLANs are configured graphically. The WAN interface gets an IP address using DHCP because it is in bridge mode, so only the LAN is configured. Press Enter here. After the assignment the console shows WAN `em0` (DHCP, `192.168.102.60/24`) and LAN `em1` (static):

![[configure-pfsense--extra-03--console-after-assignment.png]]
*the console after the interface assignment*

![[configure-pfsense--fig-03--press-enter.png]]
*the screen where you press Enter*

## Access to pfSense

To access pfSense you need a virtual machine (Windows, Linux and so on) with a web browser. The machine must be in the same LAN segment as pfSense. Once connected to the network, the machine receives an IP address from pfSense. In the author's case the address is `10.10.10.52`.

Open a web browser on this virtual machine and type the IP address of the LAN interface assigned to pfSense. In the example this is `10.10.10.1`. To sign in to the pfSense management interface use the user name `admin` and the default password `pfsense`.

Then make sure that all machines receive IP addresses from the pool created on the LAN card.

The LAN address in the setup wizard is `10.10.10.1` with mask `24`:

![[configure-pfsense--extra-09--wizard-lan-interface.png]]
*wizard step "Configure LAN Interface": `10.10.10.1`, mask 24*

Later in the lab the LAN uses `192.168.1.1/24`.

The wizard step "General Information":

![[configure-pfsense--extra-06--wizard-general-information.png]]
*wizard step "General Information": hostname `pfSense`, domain `Defender.com`*

## Related

[[Install Suricata on pfSense]] · [[Deploy Wazuh Agent (pfSense)]]

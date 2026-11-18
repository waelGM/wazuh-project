## Customize agent names

Use this to name your machines properly, if several machines have the same hostname and you want to customize them, for example by adding the domain name.

1. Stop your Wazuh agent.
2. In the `client` section of the agent configuration, add the following lines, replacing `name+domain` with the name you want:

```xml
<enrollment>
      <enabled>yes</enabled>
      <agent_name>name+domain</agent_name>
</enrollment>
```

The `client` section, with the enrollment name `client1.gestion` and the manager at `192.168.1.17`:

```xml
<ossec_config>
  <client>
    <server>
      <address>192.168.1.17</address>
      <port>1514</port>
      <protocol>tcp</protocol>
    </server>
    <config-profile>windows, windows10</config-profile>
    <crypto_method>aes</crypto_method>
    <notify_time>10</notify_time>
    <time-reconnect>60</time-reconnect>
    <auto_restart>yes</auto_restart>
    <enrollment>
      <enabled>yes</enabled>
      <agent_name>client1.gestion</agent_name>
    </enrollment>
  </client>
</ossec_config>
```

![[customize-agent-names-and-remove-an-agent--extra-05--client-enrollment-block.png]]
*`ossec.conf` of the agent*

The result on the Wazuh server: agent 017, `client1.gestion`, `192.168.1.22`, Windows 10 Pro, node `node01`, version 4.11.2, active.

![[customize-agent-names-and-remove-an-agent--extra-06--renamed-agent-row.png]]
*the renamed agent in the agent list*

3. If you already started the Wazuh agent before this step, you must delete the machine on your Wazuh server only (see the removal procedure below). After deleting the agent from the Wazuh server, start the agent again after the change.

## Remove an agent

### On the Wazuh manager

```bash
cd /var/ossec/bin
./manage_agents
```

Press `R` to remove. Choose the ID of the machine you want to remove. The tool lists the available agents (`001` Server, `008` P102-27, `003` DESKTOP-JHSFK83, `004` kali, `005` wagh-GM, `006` P102-26, `007` PC1-MTL, `013` pfSense.Defender.ca, `014` server.Gestion.Local with IP `192.168.1.5`), and agent `014` is entered and confirmed with `y`:

![[customize-agent-names-and-remove-an-agent--extra-04--manage-agents-list.png]]
*`manage_agents` removal prompt*

### On the machine itself

Then remove the agent on the machine itself. Relaunch; it gives you the option to remove the agent automatically.

To remove the agent on a Linux machine:

```bash
apt-get remove --purge wazuh-agent
```

## Related

[[Deploy Wazuh Agent (Windows)]] · [[Deploy Wazuh Agent (Linux)]] · [[Deploy Wazuh Agent (pfSense)]]

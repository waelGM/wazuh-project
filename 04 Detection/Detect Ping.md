## Ping detection

To detect ping you must create a rule. A decoder already exists for it: `windows-date-format`.

```xml
<group name="Ping">

 <rule id="100001" level="3">

  <decoded_as>windows-date-format</decoded_as>

  <match>ICMP</match>

  <description>Windows Firewall detected ICMP (ping) traffic</description>

 </rule>

</group>
```

## Make the agent send the firewall log

1. In Windows Firewall, click **Advanced settings**.
2. In each of the **Domain**, **Private** and **Public** profiles, click **Customize**. Set **Log dropped packets** and **Log successful connections** to **Yes** in the 3 sections.

![[detect-ping--fig-01--advanced-settings.png]]
*advanced settings*

The log file is `%systemroot%\system32\LogFiles\Firewall\pfirewall.log`, with a maximum size of 4096 KB:

![[detect-ping--extra-17--firewall-log-settings.png]]
*customizing the logging settings of a Windows Firewall profile (the dialog is in French)*

3. Modify the configuration file of your Wazuh agent and add the following block, then restart the service:

```xml
<localfile>

    <location>C:\Windows\System32\LogFiles\Firewall\pfirewall.log</location>

    <log_format>syslog</log_format>

  </localfile>
```

## Test

Ping the machine:

![[detect-ping--fig-02--ping-to-p102-26.png]]
*ping to P102-26: from the Kali machine, `ping 192.168.1.16`*

The ping alerts:

![[detect-ping--fig-03--ping-alerts.png]]
*ping alerts: "Windows Firewall detected ICMP (ping) traffic", rule 100001, level 3, agent `P102-26`*

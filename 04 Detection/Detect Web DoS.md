## Simulate a DoS attack against the web server

First make sure that the Wazuh agent sends the web server logs. Put these locations in the agent configuration and restart the `wazuh-agent` service:

![[detect-web-dos--fig-01--ossec-conf-web-logs.png]]
*`/var/ossec/etc/ossec.conf` with the web server log locations*

The locations:

```xml
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/apache2/access.log</location>
</localfile>

<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/apache2/error.log</location>
</localfile>

<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/*.log</location>
</localfile>
```

Use your Kali Linux machine to simulate the attack. Install `slowhttptest` if it is not already installed:

```bash
apt update
apt install slowhttptest
```

Then run this command. The target is the IP address of the web server or its name, for example `https://mypage.com` or `https://192.168.1.21`:

```bash
slowhttptest -c 5000 -H -g -o slowhttp -i 10 -r 200 -t GET -u https://mypage.com -x 24 -p 2
```

![[detect-web-dos--fig-02--attack-launch.png]]
*launching the attack*

If you try to browse to your web page, it will fail.

## Detection rule

To detect this attack, create a rule that is a combination of two rules, 31100 and 31151:

```xml
<rule id="100012" level="10" frequency="14" timeframe="90">

     <decoded_as>web-accesslog</decoded_as>

        <if_sid>31100</if_sid>

        <if_sid>31151</if_sid>

        <if_matched_sid>31101</if_matched_sid>

        <if_matched_sid>31151</if_matched_sid>

         <id>^4</id>

        <same_source_ip />

        <description>Potential DoS attack detected from $(srcip) targeting Apache web server.</description>

        <mitre>

            <id>T1499</id>

            <id>T1595.002</id>

        </mitre>

        <group>attack,pci_dss_6.5,pci_dss_11.4,gdpr_IV_35.7.d,nist_800_53_SA.11,nist_800_53_SI.4,tsc_CC6.6,tsc_CC7.1,tsc_CC8.1,tsc_CC6.1,tsc_CC6.8,tsc_CC7.2,tsc_CC7.3,web_scan,recon,pci_dss_6.5,pci_dss_11.4,gdpr_IV_35.7.d,nist_800_53_SA.11,nist_800_53_SI.4,tsc_CC6.6,tsc_CC7.1,tsc_CC8.1,tsc_CC6.1,tsc_CC6.8,tsc_CC7.2,tsc_CC7.3,</group>

  </rule>
```

If you redo the attack you will find the alerts above:

![[detect-web-dos--fig-03--dos-alerts.png]]
*the alerts produced when the attack is redone (the "alerts above"): "Potential DoS attack detected from 192.168.1.20 targeting Apache web server.", agent `wagh-GM`, level 10*

## Related

[[Configure Apache Web Server]]

## Scenario

Suppose you are not at work but still want to know whether someone logged on to your website. In that case Wazuh can send you an email when an alert is triggered by the rule that concerns the website access log. To test, the author installs smtp4dev to simulate a personal mailbox that receives the Wazuh alert.

## Install smtp4dev

1. Install smtp4dev for Windows.
2. Extract the files and click `Rnwood.smtp4dev.exe`. The SMTP server starts listening on port 25.
3. You can see the messages from your browser at `http://localhost:5000/`. For now there will be no message, because the SMTP server must be enabled on the Wazuh manager.

## Configure the Wazuh manager

1. On your Debian machine where the Wazuh manager is installed, go to `/var/ossec/etc` and edit the file `ossec.conf`.
2. Set **email notification** to `yes`, and as **smtp server** put the IP address of the machine where smtp4dev is installed. Save and restart the service.
3. Choose the rule for which you want an email alert when it triggers and put this line inside it. Save and restart the service:

```xml
<options>alert_by_email</options>
```

The `global` section of `ossec.conf`:

```xml
<global>
  <jsonout_output>yes</jsonout_output>
  <alerts_log>yes</alerts_log>
  <logall>yes</logall>
  <logall_json>no</logall_json>
  <email_notification>yes</email_notification>
  <smtp_server>192.168.1.16</smtp_server>
  <email_from>wazuh@example.wazuh.com</email_from>
  <email_to>recipient@example.wazuh.com</email_to>
  <email_maxperhour>12</email_maxperhour>
  <email_log_source>alerts.log</email_log_source>
  <agents_disconnection_time>10m</agents_disconnection_time>
  <agents_disconnection_alert_time>0</agents_disconnection_alert_time>
  <update_check>yes</update_check>
</global>
```

![[configure-email-alerts--extra-11--global-email-settings.png]]
*`ossec.conf`, global section*

The rule is rule 31108, redefined with the `alert_by_email` option:

```xml
<group name="web,accesslog">
  <rule id="31108" level="3" overwrite="yes">
    <if_sid>31100</if_sid>
    <id>^2|^3</id>
    <compiled_rule>is_simple_http_request</compiled_rule>
    <description>Access log messages </description>
    <options>alert_by_email</options>
  </rule>
</group>
```

![[configure-email-alerts--extra-13--rule-31108.png]]
*rule 31108 with `alert_by_email`*

## Test

Log in to your website from a machine. You will receive an email from your Wazuh manager.

The inbox of smtp4dev after the test, and the e-mail for rule 31108 (agent `wagh-GM`, source `192.168.1.16`, access-log line):

![[configure-email-alerts--extra-08--smtp4dev-inbox.png]]
*smtp4dev inbox; the first messages are "Wazuh notification - (DESKTOP-JHSFK83) any - Alert level 12"*

![[configure-email-alerts--extra-16--received-email.png]]
*e-mail received for rule 31108 (level 3), "Access log messages"*

## Related

[[Detect Web DoS]] · [[Customize the Dashboard]]

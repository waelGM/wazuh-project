## Detect a brute-force attack

1. First enable remote connection: search for **Settings → Remote Desktop settings** and enable Remote Desktop.
2. On your Kali Linux machine, create a text file with 10 random passwords, or the number you want.
3. Run this command with the IP address of the endpoint:

```bash
sudo hydra -l badguy -P /home/kali/Desktop/pass.txt rdp://192.168.1.16
```

Detection result on the Wazuh server:

![[detect-rdp-brute-force--fig-01--detection-result.png]]
*detection result on the Wazuh server*

The alerts are "Logon Failure - Unknown user or bad password", `rule.id` 60122 (a built-in Wazuh rule), level 5, agent `P102-26`.

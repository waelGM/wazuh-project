## Malware persistence technique detection

Attackers place malware in the Windows startup so that it runs at every logon, maintaining their access. Wazuh automatically monitors the startup folder, with no action needed from the user. To test the configuration, download the EICAR file into the directory `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup`.

Run PowerShell as administrator and run these commands:

```powershell
cd "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
Invoke-WebRequest -Uri https://secure.eicar.org/eicar.com.txt -OutFile eicar.txt
```

Then delete the file:

```powershell
Remove-Item eicar.txt
```

The alerts on the agent `P102-26` are "File added to the system" (rule 554), "File deleted." (rule 553) and "Service startup type was changed" (rule 61104):

![[detect-startup-persistence--extra-07--startup-folder-alerts.png]]
*alerts for the Startup folder test*

If you did not find anything and there are no alerts, try adding the following manually to the configuration file of your agent, or by using the group in the Wazuh manager, whether you created groups or use the default group, depending on how you manage things:

```xml
<syscheck>

  <directories realtime="yes">%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\Startup</directories>

</syscheck>
```

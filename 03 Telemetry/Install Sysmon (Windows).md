Sysmon is installed on the machines to improve the visibility of the logs. This applies to Windows or Windows Server.

## Step 1: Download Sysmon

Follow the link `https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon`, or search for Microsoft Sysmon yourself, and download the compressed folder.

## Step 2: Download the configuration files

`https://github.com/SwiftOnSecurity/sysmon-config`

Install the configuration files, or create your own. Extract the two folders to the same place, or wherever you like. Then go to where you extracted your files, copy the file `sysmonconfig-export.xml` and put it with the Sysmon files.

![[install-sysmon-windows--fig-01--config-with-sysmon-files.png]]
*joining the configuration file with the Sysmon files*

## Step 3: Install

Use the command line as administrator and run:

```powershell
Sysmon.exe -accepteula -i sysmonconfig-export.xml
```

To load or update the configuration of an installed Sysmon (v15.15, schema 4.90), run from `C:\temp`:

```powershell
sysmon.exe -c sysmonconfig-export.xml
```

![[install-sysmon-windows--extra-04--sysmon-config-update.png]]
*`sysmon.exe -c sysmonconfig-export.xml`: "Configuration file validated. Configuration updated."*

To see the logs, press **Win + R**, type `eventvwr`, then click **Applications and Services Logs → Microsoft → Windows → Sysmon**.

![[install-sysmon-windows--fig-02--sysmon-logs-event-viewer.png]]
*Sysmon logs in Event Viewer*

## Related

[[Detect Cobalt Strike]] · [[Install Sysmon (Linux)]]

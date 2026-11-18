## Step 1: Add the Microsoft repository

Sysmon is not in the standard Debian or Ubuntu repositories. `apt install sysmonforlinux` fails without first adding the Microsoft repository, so start by downloading the `.deb` package that configures the Microsoft repositories on your system (Debian 11 shown):

```bash
wget -q https://packages.microsoft.com/config/debian/11/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
```

![[install-sysmon-linux--extra-01--microsoft-repo-wget.png]]
*downloading the Microsoft repository package (prompt `root@wagh-GM`)*

## Step 2

```bash
dpkg -i packages-microsoft-prod.deb
```

This allows you to install Microsoft software through `apt` (such as `sysmonforlinux`).

## Step 3: Update the packages and install Sysmon

```bash
apt-get update
apt-get install sysmonforlinux
```

## Step 4: Configuration

Go to `/opt/sysmon` and create a configuration file, or modify it if it already exists. Put these configurations:

```xml
<EventFiltering>

    <!-- Event ID 1 == ProcessCreate. Log all newly created processes -->

    <RuleGroup name="" groupRelation="or">

      <ProcessCreate onmatch="exclude"/>

    </RuleGroup>

    <!-- Event ID 3 == NetworkConnect Detected. Log all network connections -->

    <RuleGroup name="" groupRelation="or">

      <NetworkConnect onmatch="exclude"/>

    </RuleGroup>

    <!-- Event ID 5 == ProcessTerminate. Log all processes terminated -->

    <RuleGroup name="" groupRelation="or">

      <ProcessTerminate onmatch="exclude"/>

    </RuleGroup>

    <!-- Event ID 9 == RawAccessRead. Log all raw access read -->

    <RuleGroup name="" groupRelation="or">

      <RawAccessRead onmatch="exclude"/>

    </RuleGroup>

    <!-- Event ID 10 == ProcessAccess. Log all open process operations -->

    <RuleGroup name="" groupRelation="or">

      <ProcessAccess onmatch="exclude"/>

    </RuleGroup>

    <!-- Event ID 11 == FileCreate. Log every file creation -->

    <RuleGroup name="" groupRelation="or">

      <FileCreate onmatch="exclude"/>

    </RuleGroup>

    <!--Event ID 23 == FileDelete. Log all files being deleted -->

    <RuleGroup name="" groupRelation="or">

      <FileDelete onmatch="exclude"/>

    </RuleGroup>

  </EventFiltering>
```

Apply these configurations so that Sysmon uses them. `config.xml` is the configuration file name used here. If you have a different name, replace it with yours:

```bash
sysmon -accepteula -i /opt/sysmon/config.xml
```

The `-accepteula` option accepts the End User License Agreement.

![[install-sysmon-linux--fig-01--sysmon-active-status.png]]
*Sysmon active status, everything is fine*

## Sysmon logs on Linux

For the Wazuh server to detect the Sysmon logs you must add rules and decoders.

On the Wazuh server:

1. Go to **Server Management → Rules** and click **Add New Rules File**.
2. Name the file appropriately (for example `linux_sysmon`), then copy the rules into it.
3. Save and restart the Wazuh server.

Repeat the same procedure for the decoders, using the decoders file instead of the rules file.

- Rules: `https://github.com/socfortress/Wazuh-Rules/blob/main/Sysmon%20Linux/200150-sysmon_for_linux_rules.xml`
- Decoders: `https://github.com/socfortress/Wazuh-Rules/blob/main/Sysmon%20Linux/decoder-linux-sysmon.xml`

## Related

[[Install Sysmon (Windows)]] · [[Deploy Wazuh Agent (Linux)]]

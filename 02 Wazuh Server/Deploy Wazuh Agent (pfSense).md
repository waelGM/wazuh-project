## Enable the package repositories

Edit `/usr/local/etc/pkg/repos/FreeBSD.conf` with `vi`. The file name is case-sensitive, so write `FreeBSD.conf` with exactly those capital letters.

```bash
vi /usr/local/etc/pkg/repos/FreeBSD.conf
```

Change `no` to `yes` to enable it. In `vi`:

- `x` deletes letters
- `:w!` saves the changes
- Press `s`: a `$` mark appears, where you can write `yes`. Then `:q` to quit
- Reopen the file to make sure you exited and saved the changes properly

![[deploy-wazuh-agent-pfsense--fig-01--freebsd-conf.png]]
*editing `FreeBSD.conf`*

Then edit the second file, changing `no` to `yes` in the same way:

```bash
vi /usr/local/etc/pkg/repos/pfsense.conf
```

![[deploy-wazuh-agent-pfsense--fig-02--pfsense-conf.png]]
*editing `pfsense.conf`*

### Alternative without `vi`

Connect to the pfSense web interface by entering the IP address of the LAN card in the browser, go to the **Diagnostics → Edit File** tab, open the first file, make the change as shown below, and click **Save**.

![[deploy-wazuh-agent-pfsense--fig-03--gui-login.png]]
*connecting to the pfSense GUI*

![[deploy-wazuh-agent-pfsense--fig-04--edit-file.png]]
*choose Edit File*

![[deploy-wazuh-agent-pfsense--fig-05--both-files-modified.png]]
*modifications of the two files*

## Install the agent

```bash
pkg update
pkg search wazuh
```

The latest version, 4.11.1, was not available in the pfSense repository. `pkg search wazuh` lists `wazuh-agent-4.11.0`, `wazuh-dashboard-4.11.0`, `wazuh-indexer-4.11.0`, `wazuh-manager-4.11.0_1` and `wazuh-server-4.11.0`:

![[deploy-wazuh-agent-pfsense--extra-07--pkg-search-wazuh.png]]
*`pkg search wazuh` on pfSense*

![[deploy-wazuh-agent-pfsense--fig-06--wazuh-agent-install.png]]
*installing wazuh-agent: `pkg install wazuh-agent-4.11.0`*

At the end of the installation you are asked to make some configuration changes:

```bash
cp /etc/localtime /var/ossec/etc/
```

Then edit `/var/ossec/etc/ossec.conf`:

```xml
<server>
	<address>WAZUH-MANAGER-IP-ADDRESS</address>
</server>
```

Put the IP address of your Wazuh manager. Be careful with the protocol: it must be `tcp` and not `udp`.

## Enable the service

```bash
sysrc wazuh_agent_enable="YES"
```

In this lab this command changed nothing, so it is done manually with a symbolic link (`ln` = link). The link follows any change made in the original file, and lets you make changes by editing only `wazuh-agent.sh`, without changing the original file.

```bash
ln -s /usr/local/etc/rc.d/wazuh-agent /usr/local/etc/rc.d/wazuh-agent.sh
```

Start the service:

```bash
service wazuh-agent start
```

## Save disk space

This command deletes every day all the logs that are more than 30 days old. Open the crontab with `crontab -e` and add the line below. Test the `find` command with `-print` before enabling it, because it deletes directories:

```text
0 4 * * * find /var/ossec/logs/ossec/ -d 1 -mtime +30 -type d -exec rm -rf {} \; > /dev/null
```

## Related

[[Configure pfSense]] · [[Install Suricata on pfSense]] · [[Deploy Wazuh Agent (Linux)]]

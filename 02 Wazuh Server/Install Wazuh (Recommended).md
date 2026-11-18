The short, recommended way.

## Steps

1. Go to `https://wazuh.com/install/` and click **Quickstart**.
2. Before starting, make sure `curl` is installed on your system:

```bash
apt install curl
```

3. Copy this command, which installs everything you need to get started:

```bash
curl -sO https://packages.wazuh.com/4.11/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
```

![[install-wazuh-recommended--fig-01--install-command.png]]
*the install command*

4. Wait for the installation to finish. At the end it gives you the user name, probably `admin`, and the password. Copy the password.
5. In your browser, type the IP address of your Debian machine, enter your user name and password, and you are ready to start.

## Related

[[Install Debian]] · [[Deploy Wazuh Agent (Windows)]] · [[Deploy Wazuh Agent (Linux)]] · [[Deploy Wazuh Agent (pfSense)]]

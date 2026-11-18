To install an agent on Linux, go to the Wazuh website and click **Install Wazuh**. At the end you find this window. Choose **Linux**.

![[deploy-wazuh-agent-linux--fig-01--install-page-window.png]]
*the window where you choose Linux*

If there is a difference between this note and the website, follow the website, because they may have updated it.

## Steps

Install the GPG key:

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg
```

Add the repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
```

Update the packages:

```bash
apt-get update
```

Install the agent. Put the IP address of your Wazuh manager in place of `10.0.0.2`:

```bash
WAZUH_MANAGER="10.0.0.2" apt-get install wazuh-agent
```

Enable and start the agent:

```bash
systemctl daemon-reload
systemctl enable wazuh-agent
systemctl start wazuh-agent
```

Stop the updates of `wazuh-agent`, for compatibility reasons:

```bash
sed -i "s/^deb/#deb/" /etc/apt/sources.list.d/wazuh.list
apt-get update
```

## Related

[[Deploy Wazuh Agent (Windows)]] · [[Deploy Wazuh Agent (pfSense)]] · [[Customize Agent Names and Remove an Agent]]

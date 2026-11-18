## Steps

1. Go to the Wazuh website and click **Install Wazuh** (the Windows agent package page is `https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html`). At the end you find this window. Choose **Windows** and click **Windows installer**.

![[deploy-wazuh-agent-windows--fig-01--install-page-window.png]]
*the window where you choose Windows*

2. Run the downloaded file (found in Downloads) and follow the default instructions. Leave everything at default unless you want to change the location.
3. Enter the IP address of your Wazuh manager machine, then click **Manage**, **Start**, **Refresh**, then **Save**.

![[deploy-wazuh-agent-windows--fig-02--agent-manager-markers.png]]
*the agent manager window*

The window shows Wazuh v4.11.1, the agent `LAPTOP-I90MSRHS (002)`, Manager IP `0.0.0.0` and status Stopped, before the manager address is entered.

## Related

[[Deploy Wazuh Agent (Linux)]] · [[Customize Agent Names and Remove an Agent]]

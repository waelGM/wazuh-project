## Add the Active Directory Domain Services role

1. Open **Server Manager** from the Start menu or the taskbar.
2. In Server Manager, click **Add roles and features**.
3. Click **Next** in the wizard until you reach **Select installation type**.
4. Select the server on which to install the roles. By default the local server is selected.
5. Select **Active Directory Domain Services (AD DS)**.
6. Leave the default features ticked and click **Next** until the confirmation, then click **Install**.

## Configure AD DS

1. Choose **Add a new forest** and name it as you like. The domain in this lab is `Gestion.local`.
2. Click **Next** until the configuration check, then **Install**.
3. When the installation finishes, the server restarts automatically.

![[configure-active-directory-and-server-roles--fig-01--domain-name.png]]
*the domain name*

![[configure-active-directory-and-server-roles--fig-02--ad-ds.png]]
*AD DS*

## DNS server and web server

![[configure-active-directory-and-server-roles--fig-03--dns-server.png]]
*DNS server*

![[configure-active-directory-and-server-roles--fig-04--web-server.png]]
*web server*

![[configure-active-directory-and-server-roles--fig-05--default-site.png]]
*accessing the default site*

![[configure-active-directory-and-server-roles--fig-06--services-running.png]]
*all services running*

The three roles are ticked in one pass in the wizard: DNS Server, Web Server (IIS) and Active Directory Domain Services. The DNS zone contains the A records of the server `WIN-6DVS2UHM60N` at `10.10.10.51`.

![[configure-active-directory-and-server-roles--extra-11--roles-selected.png]]
*the roles selected in the Add Roles and Features wizard (the wizard is in French)*

![[configure-active-directory-and-server-roles--extra-16--domain-login.png]]
*sign-in screen of the domain: `GESTION\Administrateur`*

## Related

[[Install Windows Server 2019]] · [[Share and Audit a Folder (Windows Server)]]

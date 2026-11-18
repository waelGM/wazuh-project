## Install Apache

```bash
apt install apache2
```

![[configure-apache-web-server--fig-01--apache2-install.png]]
*installing apache2, the web server*

## Disable the default site

Disable the default site and restart the `apache2` service:

```bash
a2dissite 000-default.conf
systemctl reload apache2.service
```

![[configure-apache-web-server--extra-02--a2dissite-default.png]]
*disabling the default site*

## Create the site

Go to `/var/www`, then create a folder that will contain an `index.html` file. Build your page as you like.

```bash
mkdir mypage
cd mypage
gedit index.html   # or nano
```

The `index.html`:

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>MYPAGE</title>
</head>
<body>
<p>Bienvenu au site mypage.com</p>
</body>
</html>
```

![[configure-apache-web-server--extra-03--index-html-location.png]]
*`index.html` of the site*
## Virtual host configuration

Go to `/etc/apache2/sites-available` and create the configuration file `mypage.conf` (enabled under `/etc/apache2/sites-enabled`). It contains the following directives:

- `VirtualHost *:443>`: the start of a virtual host definition that listens on port 443. Everything between this line and the matching `</VirtualHost>` concerns this virtual host.
- `ServerName www.mypage.com`: the name clients use to reach this website. It must be a valid domain name.
- `ServerAdmin webmaster@localhost`: the webmaster's email address, used by Apache to send error messages.
- `DocumentRoot /var/www/mypage`: the directory where Apache looks for the files of this website.

The complete file:

```apache
<VirtualHost *:443>
    ServerName www.mypage.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/mypage
    SSLEngine on
    SSLCertificateFile      /etc/certifications/mypage.com.crt
    SSLCertificateKeyFile   /etc/certifications/mypage.com.key
    SSLCertificateChainFile /etc/certifications/mypage.com.crt

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

![[configure-apache-web-server--extra-04--virtual-host-file.png]]
*the virtual host file*

## HTTPS certificate

For the site `mypage.com` certificates are needed. Otherwise you can use port 80 and skip this step. Replace the name of the site in the commands with yours.

```bash
openssl req -out mypage.com.csr -new -newkey rsa:2048 -nodes -keyout mypage.com.key
```

![[configure-apache-web-server--extra-05--openssl-req.png]]
*the first command, run in `/etc/certifications`*

Answer the questions, at least Country Name, State Name and Common Name. Then run:

```bash
openssl x509 -signkey mypage.com.key -in mypage.com.csr -req -days 730 -out mypage.com.crt
```

![[configure-apache-web-server--extra-06--openssl-x509.png]]
*the second command: certificate request self-signature ok, subject `CN = mypage.com`*

Then enable the SSL module so the certificate directives are taken into account:

```bash
a2enmod ssl
```

![[configure-apache-web-server--extra-07--a2enmod-ssl.png]]
*enabling the SSL module; Apache asks for `systemctl restart apache2`*

The site `mypage.com` is reachable:

![[configure-apache-web-server--fig-02--site-reachable.png]]
*the site `mypage.com` accessed (line "Capable d'accès à mon site mypage.com")*

After restarting your Apache server, if the site does not work, restart your browser.

## Access from Windows Server

The site is accessed from the Windows Server. For that, add the IP address of the Linux machine where the DNS server (Bind9) is installed, and put it first in your DHCP server, or set it manually if you do not use a DNS server. This way the machine looks in your DNS server before consulting another server.

![[configure-apache-web-server--fig-03--site-from-windows-server.png]]
*accessing the web page from Windows Server*

## Related

[[Configure Bind9 DNS Server]] · [[Install Linux Mint]]

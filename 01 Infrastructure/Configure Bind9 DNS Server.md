## Network configuration

### 1. Disable NetworkManager

The network is configured manually, so disable the NetworkManager service:

```bash
sudo systemctl stop NetworkManager
sudo systemctl disable NetworkManager
```

### 2. Install resolvconf

The `networking` service, which is probably already installed, is used to configure the network cards. Install `resolvconf`, a tool that helps configure DNS settings:

```bash
sudo apt install resolvconf
```

### 3. Configure each network interface

Use `ip a` or `ifconfig -a` to see the available interfaces. For each interface you want to control, create a file named `ifcfg-<interface name>`:

```bash
sudo nano /etc/network/interfaces.d/ifcfg-<interface name>
```

For example, for an interface called `ens33` configured with DHCP, the file contains:

```text
auto ens33
iface ens33 inet dhcp
```

Save and close the file. Repeat for each interface you want to configure. If you want to use the interface with a static address, for example:

![[configure-bind9-dns-server--fig-01--static-interface-example.png]]
*the static interface example*

Example `ifcfg-ens37`:

```text
auto ens37
iface ens37 inet static
address 192.168.80.102
netmask 255.255.255.0
broadcast 192.168.80.255
gateway 192.168.80.101
dns-nameservers 192.168.80.102 192.168.80.100
```

### 4. Restart the network service

```bash
sudo systemctl restart networking
```

## Bind9

### 5. Install Bind9

Install the Bind9 DNS server, which helps manage different websites.

```bash
sudo apt install bind9
```

![[configure-bind9-dns-server--extra-05--apt-install-bind9.png]]
*the install command*

### 6. Make the DNS server responsible for the hosts file

After the installation, edit `/etc/nsswitch.conf` and indicate next to the word `hosts` that the DNS server is responsible for the hosts file, as in the photo:

![[configure-bind9-dns-server--fig-02--nsswitch-hosts-line.png]]
*the `hosts` line in `/etc/nsswitch.conf`*

### 7. Create the zones

In the file `named.conf.local`, create the forward and reverse zones:

```bash
sudo gedit /etc/bind/named.conf.local
```

```text
//zone direct
zone "mypage.com" {
  type master;
  file "/etc/bind/db.mypage.com";
};

//zone inverse
zone "192.168.1.21.in-addr.arpa" {
  type master;
  file "/etc/bind/reverse.mypage.com";
};
```

![[configure-bind9-dns-server--extra-08--named-conf-local.png]]
*`named.conf.local`*

### 8. Create the zone files

Make copies of the file `db.local` to make editing easier:

```bash
sudo cp db.local db.mypage.com
sudo cp db.local reverse.mypage.com
```

Write the files carefully, because small errors cause the configuration to be processed wrongly. The contents:

`/etc/bind/db.mypage.com`:

```text
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     mypage.com. root.mypage.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      mypage.com.
@       IN      A       192.168.1.21
mypage  IN      A       192.168.1.21
www     IN      CNAME   mypage
```

![[configure-bind9-dns-server--extra-09--forward-zone-file.png]]
*the forward zone file*

`/etc/bind/reverse.mypage.com`:

```text
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     mypage.com. root.mypage.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      mypage.com.
@       IN      PTR     mypage.com.
```

![[configure-bind9-dns-server--extra-10--reverse-zone-file.png]]
*the reverse zone file*

### 9. Check the zone files

Check your zone files with `named-checkzone`, followed by the domain name and the full path of the zone file, as shown below. If you do not see `OK`, there is an error in the configuration of the file `db.<your site name>`:

```bash
sudo named-checkzone <domain name> <full path of the zone file>
```

![[configure-bind9-dns-server--fig-03--checkzone-forward.png]]
*checking the forward zone file*

Check the reverse zone with the associated IP address `.in-addr.arpa` followed by the full path of the zone file. You get `OK` if everything is correct:

```bash
sudo named-checkzone <IP address>.in-addr.arpa <full path of the reverse zone file>
```

![[configure-bind9-dns-server--fig-04--checkzone-reverse.png]]
*checking the reverse zone file*

Restart the service after all these changes:

```bash
sudo systemctl restart bind9
```

### 10. Resolver configuration

Edit the file `head` in `/etc/resolvconf/resolv.conf.d`. Put your search domains in the `search` section, and the IP addresses of your DNS servers. Restart the `resolvconf` service and make the changes permanent:

```bash
systemctl restart resolvconf
resolvconf --enable-updates
resolvconf -u
```

The `head` file contains:

```text
search mypage.com
nameserver 192.168.1.21
```

![[configure-bind9-dns-server--extra-14--resolv-conf-head.png]]
*`/etc/resolvconf/resolv.conf.d/head`*
## Related

[[Install Linux Mint]] · [[Configure Apache Web Server]]

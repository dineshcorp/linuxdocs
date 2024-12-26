# Primary DNS Server Setup

## Primary DNS Server Details

| Parameter         | Value                      |
|-------------------|----------------------------|
| Operating System  | CentOS 7 CLI              |
| Hostname          | `masterdns.convox.in`     |
| IP Address        | `172.16.14.164/24`        |

## Client Server Details

| Parameter         | Value                      |
|-------------------|----------------------------|
| Operating System  | CentOS 7.9 GUI            |
| Hostname          | `client.convox.in`        |
| IP Address        | `172.16.14.127/24`        |

---

## Setup Primary (Master) DNS Server

### Install Required Packages
```bash
yum install bind bind-utils vim -y
```

### Modify the Configuration File
Edit the file `/etc/named.conf`:
```bash
vim /etc/named.conf
```
Add the following configuration:
```conf
options {
    listen-on port 53 { 127.0.0.1; 172.16.13.77; };
    listen-on-v6 port 53 { ::1; };
    directory       "/var/named";
    dump-file       "/var/named/data/cache_dump.db";
    statistics-file "/var/named/data/named_stats.txt";
    memstatistics-file "/var/named/data/named_mem_stats.txt";
    recursing-file  "/var/named/data/named.recursing";
    secroots-file   "/var/named/data/named.secroots";
    allow-query     { any; };

    recursion yes;
    dnssec-enable yes;
    dnssec-validation yes;
    bindkeys-file "/etc/named.root.key";
    managed-keys-directory "/var/named/dynamic";
    pid-file "/run/named/named.pid";
    session-keyfile "/run/named/session.key";
};

logging {
    channel default_debug {
        file "data/named.run";
        severity dynamic;
    };
};

zone "convoxccs.in" IN {
    type master;
    file "forward.convoxccs";
    allow-update { none; };
};

zone "13.16.172.in-addr.arpa" IN {
    type master;
    file "reverse.convoxccs";
    allow-update { none; };
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```

### Create Zone Files

#### Forward Zone File
Create the forward zone file:
```bash
vim /var/named/forward.convox
```
Add the following content:
```bind
$TTL 86400
@   IN  SOA     masterdns.convox.in. root.convox.in. (
        2011071001  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
@       IN  NS          masterdns.convox.in.
@       IN  A           172.16.14.164
@       IN  A           172.16.14.127
masterdns       IN  A   172.16.14.164
client          IN  A   172.16.14.127
```

#### Reverse Zone File
Create the reverse zone file:
```bash
vim /var/named/reverse.convox
```
Add the following content:
```bind
$TTL 86400
@   IN  SOA     masterdns.convox.in. root.convox.in. (
        2011071001  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
@       IN  NS          masterdns.convox.in.
@       IN  PTR         convox.in.
masterdns       IN  A   172.16.14.164
client          IN  A   172.16.14.127
164     IN  PTR         masterdns.convox.in.
127     IN  PTR         client.convox.in.
```

### Start and Enable DNS Service
```bash
systemctl start named
systemctl enable named
```

### Configure Firewall
Allow DNS traffic through the firewall:
```bash
firewall-cmd --permanent --add-port=53/tcp
firewall-cmd --permanent --add-port=53/udp
firewall-cmd --reload
```

### Configure Permissions and SELinux
```bash
chgrp named -R /var/named
chown -v root:named /etc/named.conf
restorecon -rv /var/named
restorecon /etc/named.conf
```

### Test DNS Configuration

#### Test Named Configuration
```bash
named-checkconf /etc/named.conf
```

#### Check Forward Zone
```bash
named-checkzone convox.in /var/named/forward.convox
```

#### Check Reverse Zone
```bash
named-checkzone convox.in /var/named/reverse.convox
```

### Update Network Configuration
Edit the network interface configuration:
```bash
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
Edit `/etc/resolv.conf`:
```bash
vim /etc/resolv.conf
```
Add the DNS server IP address:
```conf
nameserver 172.16.14.164
```
Restart the network:
```bash
systemctl restart network
```

### Test DNS Server
Test the DNS server using the following commands:
```bash
dig masterdns.convox.in
nslookup convox.in
```

## DNS Server Setup Complete!

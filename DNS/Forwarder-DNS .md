A **Forwarder Nameserver** is a DNS server that forwards client queries to an upstream DNS server—such as Google's public DNS (8.8.8.8, 8.8.4.4)—instead of resolving queries recursively on its own. This method improves query resolution efficiency and reduces DNS lookup latency due to cached responses.

## Configure BIND for DNS Forwarding

- Edit the Main Configuration File

   ```bash
   vim /etc/named.conf
   ```

- Insert Forwarding Configuration

   ```bash
   recursion yes;
        forwarders {
        8.8.8.8;
        8.8.4.4;
        };
   ```


## Full `named.conf` Example

```bash
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

acl ns_ip_add {
    127.0.0.1;
    192.168.112.145;
    192.168.112.146;
    192.168.112.147;
};

acl mynetwork {
    127.0.0.1;
    192.168.112.0/24;
};

options {
    listen-on port 53 { ns_ip_add; };
    listen-on-v6 port 53 { ::1; };
    directory "/var/named";
    dump-file "/var/named/data/cache_dump.db";
    statistics-file "/var/named/data/named_stats.txt";
    memstatistics-file "/var/named/data/named_mem_stats.txt";
    secroots-file "/var/named/data/named.secroots";
    recursing-file "/var/named/data/named.recursing";
    allow-query { mynetwork; };

    recursion yes;

    forwarders {
        8.8.8.8;
        8.8.4.4;
    };

    dnssec-validation yes;

    managed-keys-directory "/var/named/dynamic";
    geoip-directory "/usr/share/GeoIP";

    pid-file "/run/named/named.pid";
    session-keyfile "/run/named/session.key";

    include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
    channel default_debug {
        file "data/named.run";
        severity dynamic;
    };
};

zone "." IN {
    type hint;
    file "named.ca";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```



## Restart and Enable Services

- Restart BIND

  ```bash
  systemctl restart named.service
  ```

- Restart Firewalld

   ```bash
   systemctl restart firewalld.service
   ```



## Testing the Setup

- Use `dig` to Test DNS Resolution

  ```bash
  dig google.com @192.168.112.145
  dig youtube.com @192.168.112.145
  ```



## Capture DNS Traffic (Optional)

You can use `tcpdump` to monitor DNS queries:

```bash
tcpdump -t udp
```
```
tcpdump -i enp0s3 port 53
```
```
tcpdump -n udp port 53
```
```
tcpdump -t udp -i enp0s3 port 53
```
```
tcpdump -n -t udp -i enp0s3 port 53
```
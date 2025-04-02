# Cache server Configuration Guide (Debian)

## Introduction

A **Domain Name System (DNS) server** translates human-readable domain names (e.g., google.com) into IP addresses. Setting up a DNS server improves network efficiency, security, and reduces dependency on external DNS services. This guide provides a step-by-step approach to configuring a DNS server using BIND.

### What is a Cache-Only DNS Server?
A **cache-only DNS server** temporarily stores DNS query results to improve response time and reduce the number of requests to external servers. It does not store authoritative zone data but speeds up name resolution for frequently accessed domains.

---

## 1. Check Hostname

### Why?
The hostname identifies the server within the network, aiding in management and troubleshooting.

### Command:
```bash
hostnamectl
```
Assign a proper hostname, e.g., `ns1.example.local`.

### Change Hostname:
```bash
hostnamectl set-hostname ns1.example.local
```

### Reboot System:
```bash
reboot
```

---

## 2. Install DNS (BIND)

### Why?
BIND (Berkeley Internet Name Domain) is the most widely used DNS server software.

### Command:
```bash
apt update && apt install bind9 bind9-utils bind9-doc
```

---

## 3. Verify Installation

### Why?
Ensure BIND is correctly installed and functional.

### Check Installed Package:
```bash
dpkg -l | grep bind9
```

### Get Package Information:
```bash
dpkg -s bind9
```

### List All Files:
```bash
dpkg -L bind9
```

### Check Configuration Files:
```bash
dpkg -c /var/lib/dpkg/info/bind9.list
```

### Configuration Files:
```
/etc/bind/named.conf
/etc/bind/named.conf.local
/etc/bind/named.conf.options
```

### Zone Files:
```
/var/cache/bind/
```

---

## 4. Check Current DNS Server

### Why?
Verify the current DNS resolver before making changes.

### Command:
```bash
cat /etc/resolv.conf
```

---

## 5. Convert Public to Cache-Only DNS Server

### Why?
A **cache-only DNS server** stores previously resolved queries, reducing external DNS requests and speeding up name resolution. It does not store zone records but caches responses for future use.

### Configuration Steps:

#### Modify Network Settings:
```bash
vim /etc/systemd/resolved.conf
```

#### Update the DNS Resolver:
Replace external DNS with `127.0.0.1`:
```ini
[Resolve]
DNS=127.0.0.1
Domains=~.
```

#### Restart the Resolver:
```bash
systemctl restart systemd-resolved
```

---

## 6. Enable and Start BIND Service

### Enable Service:
```bash
systemctl enable bind9
```

### Start Service:
```bash
systemctl start bind9
```

### Restart Service:
```bash
systemctl restart bind9
```

### Verification:
To confirm that your system is using the local cache-only DNS server:
```bash
dig example.com @127.0.0.1
```
If the response includes `SERVER: 127.0.0.1`, the configuration is successful.

---

## 7. Configure `/etc/bind/named.conf.options`

### Why?
This file controls DNS functionality, security, and recursion settings.

### Modify Configuration:
```bash
options {
    directory "/var/cache/bind";
    listen-on { 127.0.0.1; 192.168.1.1; [server IP]; };
    allow-query { localhost; 192.168.1.0/24; };
    recursion yes;
};
```

---

## 8. Configure Firewall

### Why?
Firewalls may block DNS traffic, so required ports must be opened.

### Allow TCP:
```bash
ufw allow 53/tcp
```

### Allow UDP:
```bash
ufw allow 53/udp
```

### Reload Firewall:
```bash
ufw reload
```

---

## 9. Define ACLs (Access Control Lists)

### Why?
ACLs restrict DNS queries to authorized devices, enhancing security.

### Example ACLs:
```bash
acl anuj_ip {
    127.0.0.1;
    192.168.1.50;
};
```

```bash
acl anuj_network {
    127.0.0.1;
    192.168.1.0/24;
};
```

### Apply ACLs:
```bash
options {
    allow-query { anuj_network; };
};
```

---

## 10. Restart Services

### Restart BIND:
```bash
systemctl restart bind9
```

### Restart Systemd Resolver:
```bash
systemctl restart systemd-resolved
```

### Restart Network Service:
```bash
systemctl restart networking
```


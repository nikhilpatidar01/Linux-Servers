**Access Control List (ACL)** in Squid allows fine-grained control over **who can access what**, based on IPs, domains, time, protocols, HTTP methods, and more.

> 📚 **Docs**: [Squid ACLs – Official Wiki](https://wiki.squid-cache.org/SquidFaq/SquidAcl)



##  ACL Element Types

Commonly used ACL types in Squid:

| **Type**        | **Description**                            |
|  --------------- | --------------- |
| `src`           | Match source IP addresses                  |
| `dstdomain`     | Match destination domain names             |
| `url_regex`     | Match full URLs using regex                |
| `urlpath_regex` | Match only URL paths                       |
| `port`          | Match destination port numbers             |
| `method`        | Match HTTP methods (GET, POST, etc.)       |
| `time`          | Match days/times (e.g., business hours)    |
| `proxy_auth`    | Match authenticated usernames              |
| `browser`       | Match browser types via user-agent strings |
| `maxconn`       | Limit client connections                   |

> 💡 Use multiple ACLs and combine them with `http_access allow|deny` rules.



## ACL Syntax

```bash
acl <name> <type> <value>
```

### Example:

```bash
acl localnet src 192.168.1.0/24
http_access allow localnet
```



## Examples of Common ACL Use Cases



### 1. Allow Local Network

```apache
acl localnet src 192.168.0.0/16
http_access allow localnet
http_access deny all
```



### 2. Restrict Access to Safe Ports

```apache
acl SSL_ports port 443
acl Safe_ports port 80 443 21 70 210 1025-65535 280 488 591 777
acl CONNECT method CONNECT

http_access deny !Safe_ports
http_access deny CONNECT !SSL_ports
http_access allow localnet
http_access deny all
```



## Website Filtering

### Allow Only Specific Websites

**Inline:**

```apache
acl allow_website dstdomain .google.com
http_access allow allow_website
http_access deny all
```

**Multiple sites:**

```apache
acl allow_website dstdomain .google.com .youtube.com
```

**Using a list file:**

Create list:

```bash
vim /etc/squid/allow_website_list
```

Content:

```
.google.com
.youtube.com
.facebook.com
```

In `squid.conf`:

```apache
acl allow_website dstdomain "/etc/squid/allow_website_list"
http_access allow allow_website
http_access deny all
```



### Deny Specific Websites

**Inline:**

```apache
acl deny_website dstdomain .facebook.com .twitter.com
http_access deny deny_website
```

**Using external file:**

```bash
vim /etc/squid/deny_website_list
```

```
.facebook.com
.youtube.com
```

In config:

```apache
acl deny_website dstdomain "/etc/squid/deny_website_list"
http_access deny deny_website
```



### Block by URL Keywords

**Inline:**

```apache
acl deny_keywords url_regex -i torrent porn
http_access deny deny_keywords
```

**From file:**

```bash
vim /etc/squid/deny_keywords
```

```
torrent
sports
game
```

Config:

```apache
acl deny_keywords url_regex -i "/etc/squid/deny_keywords"
http_access deny deny_keywords
```



## Deny Clients by IP

### Single IP:

```apache
acl deny_client src 192.168.1.51
http_access deny deny_client
```

### Multiple IPs inline:

```apache
acl deny_client src 192.168.1.11 192.168.1.12 192.168.1.13
http_access deny deny_client
```

### External file:

```bash
vim /etc/squid/deny_clients
```

```
192.168.1.51
192.168.1.52
192.168.1.100
```

Config:

```apache
acl deny_client src "/etc/squid/deny_clients"
http_access deny deny_client
```



## Test and Monitor

### Restart Squid:

```bash
systemctl restart squid
```

### Monitor real-time access log:

```bash
tail -f /var/log/squid/access.log
```

Try browsing allowed and blocked sites to confirm behavior.
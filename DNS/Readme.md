
The **Domain Name System (DNS)** translates human-readable domain names (e.g., `example.com`) into IP addresses that computers use to identify each other on the network. Proper DNS setup is essential for internet access, internal name resolution, and services such as email.



## DNS Server Types

### Caching-Only DNS Server

- A **caching-only DNS server** does not host any DNS zones. Instead, it forwards queries to other DNS servers and caches the responses to improve performance and reduce upstream traffic.

### Primary (Master) DNS Server

- A **primary DNS server** is **authoritative** for one or more zones. It contains the original zone files and is the main source for DNS data.

  #### Forward DNS

    Maps **hostnames to IP addresses**.
  
    Example: `server1.example.com → 192.168.1.10`

  #### Reverse DNS

  Maps **IP addresses to hostnames**.
  Example: `192.168.1.10 → server1.example.com`
  Used in email server verification and troubleshooting.



### Secondary (Slave) DNS Server

- A **secondary DNS server** obtains zone data from the primary server via **zone transfers (AXFR)**. It provides redundancy and load balancing but cannot modify DNS records directly.



### Forwarders DNS Server

- A **forwarding DNS server** passes queries it cannot resolve locally to an upstream server (e.g., Google DNS or the ISP’s DNS). Often used in combination with caching.



## DNS Record Types

### A Record

- Translates a domain name to an **IPv4 address**.

  Example: `example.com → 93.184.216.34`

### AAAA Record

- Translates a domain name to an **IPv6 address**.
  
  Example: `example.com → 2606:2800:220:1:248:1893:25c8:1946`

### MX Record

- Specifies the **mail server** responsible for receiving email on behalf of a domain.

### NS Record

- Identifies the **name servers** for a domain. These servers are authoritative for that domain.

### CNAME Record

- An alias that points one domain name to another.

  Example: `www.example.com → example.com`

### SOA Record

- Contains administrative information about the zone, including the primary DNS server and zone serial number.

### PTR Record

- Used for **reverse DNS** lookups, translating IP addresses back to hostnames.
Commonly used for identifying the origin of email or network traffic.
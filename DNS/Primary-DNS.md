A **Primary (Master) DNS server** hosts the original zone files and responds to DNS queries with authoritative answers. 


## **Set the Hostname**

* **Edit Hostname File**

  ```bash
  vim /etc/hostname
  ```

  **Example:**

  ```
  ns1.armour.local
  ```



## **Configure `/etc/hosts`**

* **Edit Hosts File**

  ```bash
  vim /etc/hosts
  ```

  **Example:**

  ```
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
  192.168.112.145 ns1 ns1.armour.local
  ```



## **Remove Forwarders (Disable Recursion)**

* **Edit BIND Config File**

  ```bash
  vim /etc/named.conf
  ```

  **Remove this block if present:**

  ```bash
  forwarders {
      8.8.8.8;
      8.8.4.4;
  };
  ```



## **Edit DNS Zone Configuration**

* **Edit Zone Definition File**

  ```bash
  vim /etc/named.rfc1912.zones
  ```

  **Add or Modify:**

  ```bash
  zone "armour.local" IN {
      type master;
      file "forward.armour.local";
      allow-update { none; };
  };
  ```



## **Default DNS Zones (for reference)**

These zones already exist by default and are often included for completeness:

```bash
zone "localhost.localdomain" IN {
    type master;
    file "named.localhost";
    allow-update { none; };
};

zone "localhost" IN {
    type master;
    file "named.localhost";
    allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
    type master;
    file "named.loopback";
    allow-update { none; };
};

zone "0.in-addr.arpa" IN {
    type master;
    file "named.empty";
    allow-update { none; };
};
```



## **Create Forward Zone File**

* **Copy Template File**

  ```bash
  cd /var/named/
  cp -v named.localhost forward.armour.local
  ```



## **Edit Forward Zone File**

* **Edit Zone File**

  ```bash
  vim /var/named/forward.armour.local
  ```

* **Example:**

  ```bash
  $TTL 1D
  @       IN SOA  ns1.armour.local. root.armour.local. (
                              20250324        ; serial
                              3600            ; refresh
                              1800            ; retry
                              604800          ; expire
                              86400 )         ; minimum
  @               IN              NS      ns1.armour.local.
  ns1             IN              A       192.168.112.145
  armour.local.   IN              A       192.168.112.145
  www             IN              CNAME   armour.local.
  router          IN              A       192.168.112.98
  emp1            IN              A       192.168.112.200
  emp2            IN              A       192.168.112.201

  ; Mail exchange
  @               IN              MX      10 mail.armour.local.
  mail            IN              A       192.168.112.150

  ; Text record for SPE
  @               IN              TXT     "v=spr1 mx a ~all"

  ; Additional Services
  ftp             IN              CNAME   armour.local.
  dev             IN              A       192.168.112.100
  fileserver      IN              A       192.168.112.105
  db              IN              A       192.168.112.110
  test            IN              A       192.168.112.115
  vpn             IN              A       192.168.112.120
  git             IN              A       192.168.112.125
  webapp          IN              A       192.168.112.130
  logs            IN              A       192.168.112.135
  ```



## **Set Permissions**

* **Adjust Group and File Mode**

  ```bash
  chgrp named /var/named/forward.armour.local
  chmod 640 /var/named/forward.armour.local
  ```



## **Restart BIND Service**

* **Apply Configuration**

  ```bash
  systemctl restart named
  ```



## **Test DNS Resolution**

* **Simple Query Test**

  ```bash
  dig armour.local
  ```

* **Query with Server IP**

  ```bash
  dig @192.168.112.145 armour.local
  dig @192.168.112.145 emp1.armour.local
  dig @192.168.112.145 ftp.armour.local
  ```



## ✅ **Validate Configuration**

* **Check Syntax**

  ```bash
  named-checkconf
  named-checkconf /etc/named.conf
  named-checkconf /etc/named.rfc1912.zones
  named-checkconf /etc/named.root.key
  ```

* **Verify Zone File**

  ```bash
  named-checkzone armour.local /var/named/forward.armour.local
  ```

  **Expected Output:**

  ```
  zone armour.local/IN: loaded serial 20250324
  OK
  ```
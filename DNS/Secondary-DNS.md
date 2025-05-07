The Master DNS Server holds the primary zone data, while the Slave DNS Server synchronizes zone data from the Master server.


## **Server Details**

| Role        | IP Address      | Description          |
|  -----------| ----------- | ------------- |
| Master DNS  | 192.168.112.145 | Primary DNS Server   |
| Slave DNS   | 192.168.112.160 | Secondary DNS Server |
| Domain Name | armour.local    | Example Domain       |



## Set the Hostname

* **Set Hostname:**

  ```bash
  hostnamectl set-hostname ns1.armour.local
  ```

* **Check Hostname:**

  ```bash
  hostname
  ```



## Install BIND and Utilities

* **Install BIND and Utilities:**

  ```bash
  yum install bind bind-utils -y
  ```



## Configure the DNS Zones

* **Edit Zone Configuration File:**

  ```bash
  vim /etc/named.rfc1912.zones
  ```

* **Add Zone Configuration:**

  ```bash
  zone "armour.local" IN {
      type master;
      file "forward.armour.local";
      allow-update { none; };
      allow-transfer { 192.168.1.160; };        
  };

  zone "infosec.local" IN {
      type master;
      file "forward.infosec.local";
      allow-update { none; };
      allow-transfer { 192.168.1.160; };
  };

  zone "ai.local" IN {
      type master;
      file "forward.ai.local";
      allow-update { none; };
      allow-transfer { 192.168.1.160; };
  };
  ```



## Create the Zone File

### Forward Zone for `armour.local`

* **Edit Zone File:**

  ```bash
  vim /var/named/forward.armour.local
  ```

* **Zone Configuration Example:**

  ```bash
  $TTL 1D
  @       IN SOA  ns1.armour.local. root.armour.local. (
                                      20250324        ; serial
                                      3600            ; refresh
                                      1800            ; retry
                                      604800          ; expire
                                      86400 )         ; minimum
  @               IN              NS      ns1.armour.local.
  @               IN              NS      ns2.armour.local.

  ns1             IN              A       192.168.112.145
  ns2             IN              A       192.168.112.160
  armour.local.   IN              A       192.168.112.145
  www             IN              CNAME   armour.local.
  ```



## Configure Firewall Rules (Firewalld)

* **Allow DNS Ports in Firewall:**

  ```bash
  firewall-cmd --permanent --add-port=53/udp
  firewall-cmd --permanent --add-port=53/tcp
  firewall-cmd --reload
  ```



## Validate Configuration Check

* **Run Validation:**

  ```bash
  named-checkconf
  named-checkconf /etc/named.conf
  named-checkconf /etc/named.rfc1912.zones
  ```



## Verifying Forward Zone

### Create and Verify Zone:

* **Check Zone File:**

  ```bash
  named-checkzone armour.local /var/named/forward.armour.local
  ```

* **Expected Output:**

  ```
  zone armour.local/IN: loaded serial 20250324
  OK
  ```



## Start and Enable BIND

* **Start BIND:**

  ```bash
  systemctl restart named.service
  ```

* **Enable BIND at Boot:**

  ```bash
  systemctl enable named.service
  ```



# **Secondary DNS Server (Slave) Configuration**


## Install and Configure BIND

* **Check BIND Installation:**

  ```bash
  rpm -qa | grep bind
  ```

* **Install BIND Utilities:**

  ```bash
  yum install bind-utils -y
  ```

* **Verify BIND Installation:**

  ```bash
  rpm -qa | grep bind
  ```



## Set the Hostname for the DNS Server
* **Set Hostname:**

  ```bash
  vim /etc/hostname
  ```

  **Example:**

  ```
  ns2.armour.local
  ```



## Edit the Host File as per the Zone

* **Edit `/etc/hosts`:**

  ```bash
  vim /etc/hosts
  ```

  **Configuration Example:**

  ```bash
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
  192.168.112.160 ns2 ns2.armour.local
  ```



## Configure the Zones on the Slave

* **Edit Zone Configuration File:**

  ```bash
  vim /etc/named.rfc1912.zones
  ```

  **Add Slave Zone Configuration:**

  ```bash
  zone "armour.local" IN {
      type slave;
      masters { 192.168.112.145; };
      file "slaves/forward.armour.local";
  };

  zone "infosec.local" IN {
      type slave;
      masters { 192.168.112.145; };
      file "slaves/forward.infosec.local";
  };

  zone "ai.local" IN {
      type slave;
      masters { 192.168.112.145; };
      file "slaves/forward.ai.local";
  };
  ```



## Main File Configuration

* **Edit `/etc/named.conf`:**

  ```bash
  vim /etc/named.conf
  ```

  **Update Options Block:**

  ```bash
  options {
      listen-on port 53 { 127.0.0.1; 192.168.112.160; };
      listen-on-v6 port 53 { ::1; };
      directory       "/var/named";
      dump-file       "/var/named/data/cache_dump.db";
      statistics-file "/var/named/data/named_stats.txt";
      memstatistics-file "/var/named/data/named_mem_stats.txt";
      secroots-file   "/var/named/data/named.secroots";
      recursing-file  "/var/named/data/named.recursing";
      allow-query     { localhost; 192.168.112.160; };
  };
  ```



## Configure Firewall Rules (Firewalld)

* **Allow DNS Ports in Firewall:**

  ```bash
  firewall-cmd --permanent --add-port=53/udp
  firewall-cmd --permanent --add-port=53/tcp
  firewall-cmd --reload
  ```



## Validate Configuration Check

* **Run Validation:**

  ```bash
  named-checkconf
  named-checkconf /etc/named.conf
  ```



## Verifying Forward Zone

* **Check Zone File:**

  ```bash
  named-checkzone armour.local /var/named/forward.armour.local
  ```

* **Expected Output:**

  ```
  zone armour.local/IN: loaded serial 20250324
  OK
  ```



## Start and Enable BIND

* **Start BIND:**

  ```bash
  systemctl restart named.service
  ```

* **Enable BIND at Boot:**

  ```bash
  systemctl enable named.service
  ```



## Test Setup

* **Use `dig` to Test DNS Resolution:**

  ```bash
  dig armour.local @192.168.112.160
  dig infosec.local @192.168.112.160
  ```

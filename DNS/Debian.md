# Caching Nameserver Configuration

## Check and Update Hostname

- Display Current Hostname

    ```bash
    hostnamectl
    ```

- Set a New Hostname

    ```bash
    hostnamectl set-hostname your-new-hostname
    ```

- Verify Hostname

    ```bash
    hostnamectl
    ```



## Check DNS Resolver Configuration

- View `/etc/resolv.conf`

    ```bash
    cat /etc/resolv.conf
    ```

**Example:**

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```



## Install and Configure BIND

- Check BIND Installation

    ```bash
    dpkg -l | grep bind9
    ```

- Install BIND and Utilities

    ```bash
    apt update
    apt install bind9 bind9-utils -y
    ```

- Verify Installed Packages

    ```bash
    dpkg -l | grep bind9
    dpkg -s bind9
    ```



## Manage BIND Service

- Check Status

    ```bash
    systemctl status named
    ```

- Start BIND

    ```bash
    systemctl start named
    ```

- Enable on Boot

    ```bash
    systemctl enable named
    ```

- Restart Service

    ```bash
    systemctl restart named
    ```



## Configure `named.conf.options`

- Edit the File

    ```bash
    nano /etc/bind/named.conf.options
    ```

- Sample Configuration:

    ```bash
    options {
        directory "/var/cache/bind";

        listen-on port 53 { 127.0.0.1; 192.168.112.53; };
        allow-query { localhost; 192.168.112.0/24; };
        recursion yes;
        dnssec-validation auto;
    };
    ```

- Restart BIND

```bash
systemctl restart bind9
```



# Primary DNS Server (Master)

- Set Hostname

    ```bash
    nano /etc/hostname
    ```

  **Example:**

    ```
    ns1.armour.local
    ```

- Edit Hosts File

    ```bash
    nano /etc/hosts
    ```

  **Example:**

    ```
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost.localdomain6
    192.168.112.53 ns1 ns1.armour.local
    ```


- Define DNS Zones

    ```bash
    nano /etc/bind/named.conf.local
    ```

  **Sample:**

    ```bash
    zone "armour.local" {
        type master;
        file "/etc/bind/forward.armour.local";
        allow-update { none; };
    };
    ```



- Create Forward Zone File

    ```bash
    cp /etc/bind/db.local /etc/bind/forward.armour.local
    ```



- Edit Forward Zone File

    ```bash
    nano /etc/bind/forward.armour.local
    ```

  **Example:**

    ```bash
    $TTL 1D
    @       IN SOA  ns1.armour.local. root.armour.local. (
                        20250324        ; serial
                        3600            ; refresh
                        1800            ; retry
                        604800          ; expire
                        86400 )         ; minimum
    @               IN      NS      ns1.armour.local.
    ns1             IN      A       192.168.112.53
    armour.local.   IN      A       192.168.112.53
    www             IN      CNAME   armour.local.
    router          IN      A       192.168.112.98
    emp1            IN      A       192.168.112.200
    emp2            IN      A       192.168.112.201
    @               IN      MX      10 mail.armour.local.
    mail            IN      A       192.168.112.150
    @               IN      TXT     "v=spr1 mx a ~all"
    ftp             IN      CNAME   armour.local.
    dev             IN      A       192.168.112.100
    fileserver      IN      A       192.168.112.105
    db              IN      A       192.168.112.110
    test            IN      A       192.168.112.115
    vpn             IN      A       192.168.112.120
    git             IN      A       192.168.112.125
    webapp          IN      A       192.168.112.130
    logs            IN      A       192.168.112.135
    ```



- Set Permissions

    ```bash
    chown bind:bind /etc/bind/forward.armour.local
    chmod 640 /etc/bind/forward.armour.local
    ```



- Restart BIND

    ```bash
    systemctl restart bind9
    ```



- Check DNS Resolution

    ```bash
    dig armour.local
    ```



- Validate Configuration

    ```bash
    named-checkconf
    named-checkconf /etc/bind/named.conf
    named-checkconf /etc/bind/named.conf.local
    ```



- Verify Forward Zone

    ```bash
    named-checkzone armour.local /etc/bind/forward.armour.local
    ```

  **Expected Output:**

    ```
    zone armour.local/IN: loaded serial 20250324
    OK
    ```

- Test Name Resolution

    ```bash
    dig @192.168.112.53 armour.local
    dig @192.168.112.53 emp1.armour.local
    dig @192.168.112.53 ftp.armour.local
    ```



# Master and Slave DNS Server Setup

## Server Information

| Role        | IP Address     | Description          |
| ------------- |  ------------- | ------------- |
| Master DNS  | 192.168.112.53 | Primary DNS Server   |
| Slave DNS   | 192.168.112.54 | Secondary DNS Server |
| Domain Name | armour.local   | Domain Name Example  |



## Master DNS Zone Configuration

- Set Hostname

```bash
hostnamectl set-hostname ns1.armour.local
```

- Install BIND

  ```bash
  apt update
  apt install bind9 bind9-utils -y
  ```

- Configure Zone File

  ```bash
  nano /etc/bind/named.conf.local
  ```

  ```bash
  zone "armour.local" {
      type master;
      file "/etc/bind/forward.armour.local";
      allow-update { none; };
      allow-transfer { 192.168.112.54; };
  };

  zone "infosec.local" {
      type master;
      file "/etc/bind/forward.infosec.local";
      allow-update { none; };
      allow-transfer { 192.168.112.54; };
  };

  zone "ai.local" {
      type master;
      file "/etc/bind/forward.ai.local";
      allow-update { none; };
      allow-transfer { 192.168.112.54; };
  };
  ```



## Forward Zone File: `armour.local`

  ```bash
  nano /etc/bind/forward.armour.local
  ```

  ```bash
  $TTL 1D
  @       IN SOA  ns1.armour.local. root.armour.local. (
                      20250324        ; serial
                      3600            ; refresh
                      1800            ; retry
                      604800          ; expire
                      86400 )         ; minimum
  @               IN      NS      ns1.armour.local.
  @               IN      NS      ns2.armour.local.
  ns1             IN      A       192.168.112.53
  ns2             IN      A       192.168.112.54
  armour.local.   IN      A       192.168.112.53
  www             IN      CNAME   armour.local.
  ```



## Configure Firewall (UFW)

  ```bash
  ufw allow 53/udp
  ufw allow 53/tcp
  ufw reload
  ```



## Validate Configuration

```bash
named-checkconf
named-checkzone armour.local /etc/bind/forward.armour.local
```



## Start and Enable BIND

```bash
systemctl restart bind9
systemctl enable bind9
```



# Secondary DNS Server (Slave)

- Install BIND

  ```bash
  apt update
  apt install bind9 bind9-utils -y
  ```

- Set Hostname

  ```bash
  hostnamectl set-hostname ns2.armour.local
  ```

- Edit Hosts File

  ```bash
  nano /etc/hosts
  ```

  ```bash
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost localhost.localdomain localhost6 localhost.localdomain6
  192.168.112.54 ns2 ns2.armour.local
  ```

- Configure Zones

  ```bash
  nano /etc/bind/named.conf.local
  ```

  ```bash
  zone "armour.local" {
      type slave;
      masters { 192.168.112.53; };
      file "/var/lib/bind/forward.armour.local";
  };

  zone "infosec.local" {
      type slave;
      masters { 192.168.112.53; };
      file "/var/lib/bind/forward.infosec.local";
  };

  zone "ai.local" {
      type slave;
      masters { 192.168.112.53; };
      file "/var/lib/bind/forward.ai.local";
  };
  ```

- Configure `named.conf.options`

  ```bash
  nano /etc/bind/named.conf.options
  ```

  ```bash
  options {
      listen-on port 53 { 127.0.0.1; 192.168.112.54; };
      listen-on-v6 port 53 { ::1; };
      directory "/var/lib/bind";
      dump-file "/var/cache/bind/cache_dump.db";
      statistics-file "/var/cache/bind/named_stats.txt";
      allow-query { localhost; 192.168.112.54; };
  };
  ```


# Forwarder DNS Configuration
- Edit BIND Options

  ```bash
  vim /etc/bind/named.conf.options
  ```

- Add Forwarders Section

  ```bash
  forwarders {
      8.8.8.8;
      8.8.4.4;
  };
  ```

- Test Forwarder Queries

  ```bash
  tcpdump -n -t udp port 53
  ```
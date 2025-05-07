A **caching nameserver** stores DNS query results temporarily to improve resolution speed and reduce traffic to upstream servers.



## **Set and Verify Hostname**

- Display Current Hostname

    ```bash
    hostname
    ```

- Update Hostname


    ```bash
    vim /etc/hostname
    ```

    Example:

    ```
    dns1.example.local
    ```

- Reboot to Apply the Change

    ```bash
    reboot
    ```

- Confirm Hostname

    ```bash
    hostname
    ```



## **Configure Network and DNS**

- Edit NetworkManager Configuration

    ```bash
    vim /etc/NetworkManager/system-connections/enp0s3.nmconnection
    ```

- Example:

    ```
    [connection]
    id=enp0s3
    type=ethernet

    [ipv4]
    address1=192.168.112.145/24
    dns=127.0.0.1;
    gateway=192.168.112.98
    method=manual

    [ipv6]
    method=disabled
    ```

- Reload Connection Settings

    ```bash
    nmcli connection reload
    nmcli connection up enp0s3
    ```

- Confirm UUID (Optional)

    ```bash
    nmcli connection show enp0s3 | grep uuid
    ```


-  Install Required Packages

    ```bash
    dnf install bind bind-utils -y
    

- Verify Installation

    ```bash
    rpm -qa | grep bind
    ```

- View Package Info (Optional)

    ```bash
    rpm -qi bind
    ```

- List Installed Files

    ```bash
    rpm -ql bind
    ```

- View Config Files

    ```bash
    rpm -qc bind
    ```



## **Configure BIND (`/etc/named.conf`)**

- Edit the main BIND configuration file:

    ```bash
    vim /etc/named.conf
    ```

- Update the `options` block (adjust IPs as necessary):

    ```bash
    options {
        listen-on port 53 { 127.0.0.1; 192.168.112.145; };
        listen-on-v6 port 53 { ::1; };
        directory "/var/named";
        dump-file "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file "/var/named/data/named.secroots";
        recursing-file "/var/named/data/named.recursing";

        recursion yes;
        allow-query { localhost; 192.168.112.0/24; };
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



## **Start and Enable BIND**

- Start the BIND Service

    ```bash
    systemctl start named.service
    ```

- Enable BIND on Boot

    ```bash
    systemctl enable named.service
    ```

- Check BIND Service Status

    ```bash
    systemctl status named.service
    ```

## **Allow DNS Through the Firewall (Firewalld)**

- Open DNS Ports

    ```bash
    firewall-cmd --permanent --add-port=53/udp
    firewall-cmd --permanent --add-port=53/tcp
    firewall-cmd --reload
    ```

- Verify Open Ports

    ```bash
    firewall-cmd --list-ports
    ```

## **Verify DNS Resolution**

- Check if Port 53 is Listening

    ```bash
    ss -nltup | grep :53
    ```

- Test Using `dig`

    ```bash
    dig google.com @127.0.0.1
    dig example.com @192.168.112.145
    ```
# Configuring the Client PC for Squid Transparent Proxy

## Set IP Address on Client PC

1. Open **Network Connections** by running `ncpa.cpl`.
2. Right-click on the active network adapter and choose **Properties**.
3. Select **Internet Protocol Version 4 (TCP/IPv4)** and click **Properties**.
4. Configure the following static IP settings:

   * **IP Address**: `192.168.1.10`
   * **Subnet Mask**: `255.255.255.0`
   * **Default Gateway**: `192.168.1.1`
   * **Preferred DNS Server**: `8.8.8.8`
   * **Alternate DNS Server**: `192.168.1.1` (if using internal DNS)



## Disable Proxy Settings in the Browser

### In the Browser

1. Open your web browser (e.g., Firefox, Chrome).
2. Navigate to **Settings** > **Network** or **Connection Settings**.
3. Locate the proxy section and disable or remove any configured proxy settings.


## Verify Network Connectivity (Ping Test)

1. Open **Command Prompt** or terminal.

2. Run the following commands to confirm connectivity:

   ```bash
   ping 192.168.1.1        # Test gateway
   ping 8.8.8.8            # Test internet
   ```

3. Optionally, ping other local devices to ensure LAN functionality.



# Squid Transparent Proxy Setup

- Open the BIND main configuration file:

    ```bash
    vim /etc/named.conf
    ```

- Add ACLs and Permissions

    ```bash
    acl ns_ip_add {
        192.168.1.1;
    };

    acl mynetwork {
        192.168.1.0/24;
    };
    ```

- Example of `named.conf`

    ```bash
    acl ns_ip_add {
        127.0.0.1;
        192.168.12.145;
        192.168.12.146;
        192.168.12.147;
        192.168.1.1;
    };

    acl mynetwork {
        127.0.0.1;
        192.168.12.0/24;
        192.168.1.0/24;
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

- Restart the BIND service:

    ```bash
    systemctl restart named.service
    ```

- Optionally, test with:

    ```bash
    dig google.com
    nslookup example.com
    ```

## Configure Squid as a Transparent Proxy


- Open the Squid configuration file:

    ```bash
    vim /etc/squid/squid.conf
    ```

- Update the ports to enable transparent mode:

    ```bash
    http_port 3128
    http_port 3128 transparent
    ```

- Restart Squid

    ```bash
    systemctl restart squid
    ```

- To view an uncommented config:

    ```bash
    cat /etc/squid/squid.conf | grep -v '^#' | sed '/^$/d'
    ```



## Configure Firewalld

- Enable IP Forwarding

    ```bash
    sysctl -w net.ipv4.ip_forward=1
    ```

- Make it permanent:

    ```bash
    vim /etc/sysctl.conf
    ```

    Add:

    ```bash
    net.ipv4.ip_forward=1
    ```

    Apply changes:

    ```bash
    sysctl -p
    ```

### Firewalld Zone Configuration

- Assign interfaces to zones:

    ```bash
    firewall-cmd --zone=public --add-interface=enp0s3 --permanent
    firewall-cmd --zone=trusted --add-interface=enp0s8 --permanent
    ```

    Allow loopback traffic:

    ```bash
    firewall-cmd --zone=trusted --add-source=127.0.0.1 --permanent
    ```

    Allow DNS traffic:

    ```bash
    firewall-cmd --zone=public --add-port=53/udp --permanent
    firewall-cmd --zone=public --add-service=dns --permanent
    ```

    Enable masquerading for NAT:

    ```bash
    firewall-cmd --zone=public --add-masquerade --permanent
    ```

    Forward HTTP traffic to Squid:

    ```bash
    firewall-cmd --zone=trusted --add-forward-port=port=80:proto=tcp:toport=3128 --permanent
    firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=3128 --permanent
    ```

    ### Reload Firewalld

    ```bash
    firewall-cmd --reload
    ```



## Verify the Transparent Proxy

- Check active firewall rules:

    ```bash
    firewall-cmd --list-all
    ```

    Test from a client machine:

* Configure browser proxy to:

  * **IP**: `192.168.2.1`
  * **Port**: `3128`

- Or test from terminal:

    ```bash
    curl http://example.com
    ```



## Make IP Forwarding Persistent

- Ensure the following is in `/etc/sysctl.conf`:

    ```bash
    net.ipv4.ip_forward=1
    ```

- Apply with:

    ```bash
    sysctl -p
    ```

The **DHCP exclusion range** allows administrators to prevent certain IP addresses within a subnet from being assigned dynamically to clients. This is useful when reserving addresses for **static IPs**, **servers**, **printers**, or **network appliances**.

## Configuration

- Open the main DHCP server configuration file:

    ```bash
    vim /etc/dhcp/dhcpd.conf
    ```

### Exclude IP Range `192.168.1.51` to `192.168.1.70`

- This configuration excludes 192.168.1.51–70 by starting the DHCP-assigned range after it.

    ```bash
    subnet 192.168.1.0 netmask 255.255.255.0 {
        range 192.168.1.71 192.168.1.250;

        option routers 192.168.1.1;
        option domain-name-servers 8.8.8.8, 8.8.4.4;
        option broadcast-address 192.168.1.255;

        default-lease-time 600;
        max-lease-time 7200;
    }
    ```


### Exclude Multiple Ranges

- This example excludes:
 
    `192.168.1.51` to `192.168.1.60`
    
    `192.168.1.211` to `192.168.1.230`
    

    ```bash
    authoritative;

    subnet 192.168.1.0 netmask 255.255.255.0 {
        range 192.168.1.50 192.168.1.50;       # Optional single reserved IP
        range 192.168.1.61 192.168.1.210;
        range 192.168.1.231 192.168.1.250;

        option routers 192.168.1.1;
        option domain-name-servers 8.8.8.8, 8.8.4.4;
        option broadcast-address 192.168.1.255;

        default-lease-time 600;
        max-lease-time 7200;
    }
    ```


- To apply your changes, restart the DHCP server:

```bash
systemctl restart isc-dhcp-server
```
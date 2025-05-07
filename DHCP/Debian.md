## **DHCP Server Installation & Configuration**


- Update the package list and upgrade installed packages:

    ```bash
    apt update
    ```

- Install the ISC DHCP server package:

    ```bash
    apt install isc-dhcp-server -y
    ```

- Check if the DHCP server package is installed:

    ```bash
    dpkg -l | grep isc-dhcp-server
    ```

- List configuration files

    ```bash
    dpkg -L isc-dhcp-server
    ```



## **Start and Manage DHCP Service**

- Check service status

    ```bash
    systemctl status isc-dhcp-server
    ```

- Restart DHCP service

    ```bash
    systemctl restart isc-dhcp-server
    ```

- Enable DHCP service to start at boot

    ```bash
    systemctl enable isc-dhcp-server
    ```

- Check if DHCP port is open

    ```bash
    netstat -nltup | grep dhcp
    ```



- **Check DHCP Leases**

    ```bash
    cat /var/lib/dhcp/dhcpd.leases
    ```



## **Exclusion Range**
You can define an **exclusion range** in the DHCP configuration file to prevent certain IP addresses within the subnet from being assigned to clients. This is useful for reserving IP addresses for **static assignments** or **specific devices**
- Open the configuration file to define exclusion ranges:

    ```bash
    vim /etc/dhcp/dhcpd.conf
    ```

- Example: Exclude IP range `192.168.1.51` to `192.168.1.70`

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


## **Reserving an IP Address**
You can **reserve specific IP addresses** for particular clients based on their **MAC addresses** in the DHCP configuration file. This ensures that the device **always receives the same IP address**, which is useful for **servers, printers, and critical devices**.
- Edit DHCP configuration file

    ```bash
    vim /etc/dhcp/dhcpd.conf
    ```

- Example: Reserve a single IP address

    Reserve IP `192.168.1.201` for a client with MAC `08:00:27:ab:46:cf`:

    ```bash
    authoritative;

    subnet 192.168.1.0 netmask 255.255.255.0 {
        range 192.168.1.50 192.168.1.50;
        range 192.168.1.61 192.168.1.210;
        range 192.168.1.231 192.168.1.250;
        option routers 192.168.1.1;
        option domain-name-servers 8.8.8.8, 8.8.4.4;
        option broadcast-address 192.168.1.255;
        default-lease-time 600;
        max-lease-time 7200;
    }

    host win11-1 {
        hardware ethernet 08:00:27:ab:46:cf;
        fixed-address 192.168.1.201;
    }
    ```

- Example: Reserve multiple IP addresses

    ```bash
    host win11-1 {
        hardware ethernet 08:00:27:ab:46:cf;
        fixed-address 192.168.1.201;
    }

    host win11-2 {
        hardware ethernet 08:00:27:d6:86:94;
        fixed-address 192.168.1.150;
    }
    ```



## **Whitelist and Blacklist in DHCP**
You can configure a DHCP server to **only provide IP addresses to specific known clients** by using the `deny unknown-clients` directive. This ensures that **only devices with specified MAC addresses can receive an IP address** from the DHCP server.
- Edit DHCP configuration file

    ```bash
    vim /etc/dhcp/dhcpd.conf
    ```

-  Example: Allow only specific MAC addresses

    ```bash
    authoritative;

    subnet 192.168.1.0 netmask 255.255.255.0 {
        range 192.168.1.50 192.168.1.50;
        range 192.168.1.61 192.168.1.210;
        range 192.168.1.231 192.168.1.250;
        option routers 192.168.1.1;
        option domain-name-servers 8.8.8.8, 8.8.4.4;
        option broadcast-address 192.168.1.255;
        default-lease-time 600;
        max-lease-time 7200;
        deny unknown-clients;
    }

    host device1 {
        hardware ethernet 08:00:27:d6:86:94;
        fixed-address 192.168.1.100;
    }

    host device2 {
        hardware ethernet 08:00:27:fd:12:34;
        fixed-address 192.168.1.101;
    }
    ```

# **Reserving an IP Address**

Reserving an IP address ensures a specific device always receives the same IP based on its **MAC address**. This is useful for **servers**, **printers**, and other **critical infrastructure** that require a consistent network presence.


## Configuration

* Open the DHCP server configuration file:

  ```bash
  vim /etc/dhcp/dhcpd.conf
  ```

### Reserve a Single IP Address

This example reserves IP `192.168.1.201` for a device with MAC address `08:00:27:ab:46:cf`.

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

### Reserve Multiple IP Addresses

This configuration reserves IP addresses for multiple clients, each identified by its MAC address.

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



- After editing the configuration file, restart the DHCP service:

```bash
systemctl restart isc-dhcp-server
```

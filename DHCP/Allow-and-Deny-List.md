# **Allow List**

Whitelisting allows you to configure the DHCP server to assign IP addresses **only to known clients**, based on their **MAC addresses**. This improves network security by blocking unknown or unauthorized devices from obtaining IPs.



## Configuration

* Open the DHCP server configuration file:

  ```bash
  vim /etc/dhcp/dhcpd.conf
  ```

### Allow Only Specific Clients

This example allows only the devices with MAC addresses `08:00:27:d6:86:94` and `08:00:27:fd:12:34` to receive IP addresses.

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

* After editing the configuration file, restart the DHCP service:

  ```bash
  systemctl restart isc-dhcp-server
  ```




# **Deny List (Blacklist)**

A deny list blocks specific devices from receiving an IP address, even if they are on the network. This is useful for banning rogue or compromised clients.



## Configuration

* Open the DHCP server configuration file:

  ```bash
  vim /etc/dhcp/dhcpd.conf
  ```

### Block Specific Clients

This configuration denies DHCP service to devices with the listed MAC addresses.

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
    deny booting;
}

host win11-2 {
    hardware ethernet 08:00:27:d6:86:94;
    deny booting;
}
```

* After editing the configuration file, restart the DHCP service:

  ```bash
  systemctl restart isc-dhcp-server
  ```


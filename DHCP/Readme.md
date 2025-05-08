DHCP automatically assigns IP addresses to devices on a network. This simplifies network management by eliminating the need for manual IP configuration on each client device.



## **How DHCP Works**

When a device connects to a network, the DHCP process occurs in the following phases:

* **Discovery**
  The client broadcasts a request to locate a DHCP server.

* **Offer**
  The DHCP server replies with an available IP address and configuration details.

* **Request**
  The client requests the offered IP address.

* **Acknowledgment**
  The server confirms the lease, and the client begins using the IP address.



## **Install DHCP Server**

* Install the DHCP server package

  ```bash
  yum install dhcp-server -y
  ```



## **Configure DHCP Server**

* Edit the DHCP configuration file

  ```bash
  vim /etc/dhcp/dhcpd.conf
  ```

* Sample configuration

  ```bash
  # DHCP Server Configuration File

  authoritative;

  subnet 192.168.88.0 netmask 255.255.255.0 {

      range 192.168.88.100 192.168.88.200;

      option routers 192.168.88.1;

      option domain-name-servers 8.8.8.8, 8.8.4.4;

      option broadcast-address 192.168.88.255;

      default-lease-time 600;

      max-lease-time 7200;
  }
  ```



## **Start and Manage DHCP Service**

* Check status of the DHCP service

  ```bash
  systemctl status dhcpd
  ```

* Restart the DHCP service

  ```bash
  systemctl restart dhcpd
  ```

* Enable DHCP service at system startup

  ```bash
  systemctl enable dhcpd
  ```



## **Update Firewall Rules**

* Allow DHCP traffic through the firewall

  ```bash
  firewall-cmd --permanent --add-port=67/udp
  ```

* Reload the firewall to apply changes

  ```bash
  firewall-cmd --reload
  ```
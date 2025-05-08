TFTP is a simple file transfer protocol, typically used for bootstrapping embedded systems, routers, and PXE booting.



## Installation


* **Check if TFTP client or server is installed:**

  ```bash
  rpm -qa | grep tftp
  ```


* **Install the TFTP client and server:**

  ```bash
  yum install tftp tftp-server
  ```





### Directory Setup for TFTP Boot


* **Navigate to the default TFTP root directory:**

  ```bash
  cd /var/lib/tftpboot/
  ```


* **Check if the directory exists:**

  ```bash
  ls -lha /var/lib/ | grep "tftpboot"
  ```


* **Set full permissions (for testing only):**

  ```bash
  chmod -R 777 /var/lib/tftpboot/
  ```


* **Copy test files into the TFTP directory:**

  ```bash
  cp -v /var/log/* /var/lib/tftpboot/
  ```




### Systemd Configuration for TFTP

Configure custom service and socket unit files to manage the TFTP service using systemd.

* **Copy the systemd service and socket unit files:**

  ```bash
  cp -v /usr/lib/systemd/system/tftp.service /etc/systemd/system/tftp-server.service
  cp -v /usr/lib/systemd/system/tftp.socket /etc/systemd/system/tftp-server.socket
  ```

  Copies the default unit files to custom locations for editing.

* **Review the TFTP daemon documentation:**

  ```bash
  man in.tftpd
  ```

  Displays the manual page for the TFTP daemon.



### TFTP Server Service File Configuration

Here is the custom service file for TFTP, configured to point to the correct directory.

* **File: `/etc/systemd/system/tftp-server.service`**

  ```ini
  [Unit]
  Description=TFTP Server
  Requires=tftp-server.socket
  Documentation=man:in.tftpd

  [Service]
  ExecStart=/usr/sbin/in.tftpd -p -s /var/lib/tftpboot
  StandardInput=socket

  [Install]
  WantedBy=multi-user.target
  Also=tftp-server.socket
  ```



### TFTP Server Socket File Configuration

This socket file listens on UDP port 69 for TFTP requests.

* **File: `/etc/systemd/system/tftp-server.socket`**

  ```ini
  [Unit]
  Description=TFTP Server Activation Socket

  [Socket]
  ListenDatagram=69
  BindIPv6Only=both

  [Install]
  WantedBy=sockets.target
  ```



### Firewall Configuration Using firewalld

We need to allow TFTP traffic through the system firewall.

* **Check if firewalld is active:**

  ```bash
  systemctl status firewalld
  ```


* **Start and enable firewalld:**

  ```bash
  systemctl start firewalld
  systemctl enable firewalld
  ```


* **Add TFTP service to the public zone:**

  ```bash
  firewall-cmd --zone=public --add-service=tftp --permanent
  ```


* **Open UDP port 69 manually:**

  ```bash
  firewall-cmd --zone=public --add-port=69/udp --permanent
  ```


* **Reload the firewall:**

  ```bash
  firewall-cmd --reload
  ```


* **Verify applied rules:**

  ```bash
  firewall-cmd --zone=public --list-all
  ```




### Starting and Enabling the TFTP Server


* **Restart the TFTP socket:**

  ```bash
  systemctl restart tftp-server.socket
  ```


* **Enable TFTP service at boot:**

  ```bash
  systemctl enable tftp-server
  ```




### Verifying TFTP Server Status

Confirm that the TFTP server is listening on the correct port.

* **Check using netstat:**

  ```bash
  netstat -nltup | grep 69
  ```


* **Check using ss:**

  ```bash
  ss -uln | grep 69
  ```

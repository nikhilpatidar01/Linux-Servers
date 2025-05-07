## TFTP Server Setup

This guide explains how to install, configure, and verify a TFTP server on Debian-based systems like Kali Linux and Ubuntu. TFTP is a simple file transfer protocol typically used for bootstrapping embedded systems, routers, and PXE booting.



## Installation

* **Check if TFTP client or server is installed:**

  ```bash
  apt list --installed | grep tftp
  ```


* **Install the TFTP client and server:**

  ```bash
  apt update
  apt install tftpd-hpa tftp
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


* **Set full permissions (testing only):**

  ```bash
  chmod -R 777 /var/lib/tftpboot/
  ```


* **Copy test files into the TFTP directory:**

  ```bash
  cp -v /var/log/* /var/lib/tftpboot/
  ```




### TFTP Server Configuration

Edit the configuration file for `tftpd-hpa` to specify the root directory and options.

* **File: `/etc/default/tftpd-hpa`**

  ```ini
  TFTP_USERNAME="tftp"
  TFTP_DIRECTORY="/var/lib/tftpboot"
  TFTP_ADDRESS="0.0.0.0:69"
  TFTP_OPTIONS="--secure"
  ```




### Start and Enable the TFTP Server

* **Restart and enable the TFTP service:**

  ```bash
  systemctl restart tftpd-hpa
  systemctl enable tftpd-hpa
  ```




### Verifying TFTP Server Status

* **Check using `ss`:**

  ```bash
  ss -uln | grep 69
  ```


* **Check using `netstat`:**

  ```bash
  netstat -nltup | grep 69
  ```

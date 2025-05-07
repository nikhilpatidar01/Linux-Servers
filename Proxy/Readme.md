
Squid is a caching and forwarding HTTP proxy server that improves web performance by caching frequently requested web content and controlling access to the internet. It supports HTTP, HTTPS, FTP, and more.



## Check Repositories and Existing Installation

* **List all enabled YUM repositories**

  ```bash
  yum repolist all
  ```

* **Check if Squid is already installed**

  ```bash
  rpm -qa | grep squid
  ```



## Install Squid

* **Install Squid and related packages**

  ```bash
  yum install -y squid*
  ```

> *For CentOS 8+, replace `yum` with `dnf` if needed.*


* **Confirm installation**

  ```bash
  rpm -qa | grep squid
  ```

* **Detailed package info**

  ```bash
  rpm -qi squid
  ```

* **List config files**

  ```bash
  rpm -qc squid
  ```

* **List documentation files**

  ```bash
  rpm -qd squid
  ```

* **List all installed files**

  ```bash
  rpm -ql squid
  ```



## Configure Squid

* **Edit the main configuration**

  ```bash
  vim /etc/squid/squid.conf
  ```

* **Basic configuration snippet**

  ```conf
  acl localnet src 192.168.1.0/24
  http_access allow localnet
  http_port 3128
  ```

> Configure ACLs, access rules, caching, or authentication as needed.



## Manage Squid Service

* **Enable at startup**

  ```bash
  systemctl enable squid
  ```

* **Start or restart Squid**

  ```bash
  systemctl restart squid
  ```

* **Check Squid service status**

  ```bash
  systemctl status squid
  ```



## Verify Squid Listening Port

* **Using `netstat`**

  ```bash
  netstat -tulpn | grep squid
  ```

* **Or using `ss`**

  ```bash
  ss -tulpn | grep squid
  ```

> Expected output: `3128/tcp` listening on `0.0.0.0`.



## Configure Firewalld

* **Check and start firewalld**

  ```bash
  systemctl status firewalld
  systemctl start firewalld
  systemctl enable firewalld
  ```

* **Allow Squid port (3128)**

  ```bash
  firewall-cmd --permanent --add-port=3128/tcp
  ```

* **(Optional) Allow DNS**

  ```bash
  firewall-cmd --permanent --add-port=53/tcp
  firewall-cmd --permanent --add-port=53/udp
  ```

* **Reload firewall**

  ```bash
  firewall-cmd --reload
  ```

* **View rules**

  ```bash
  firewall-cmd --list-all
  ```



# Proxy Server with Dual Network Interfaces



## Dual NIC Concept

**Proxy VM has two interfaces:**

* `eth0` – External (e.g. bridged to host or Internet)
* `eth1` – Internal (for LAN clients only)



## IP Addressing Scheme

```text
eth0 (External): 192.168.95.145
eth1 (Internal): 192.168.1.1/24
Client PC:       192.168.1.10/24 → Gateway: 192.168.1.1
```



## Assign IP to eth1

```bash
ip addr add 192.168.1.1/24 dev eth1
ip link set eth1 up
```



## Client Configuration

On the client system:

* IP: `192.168.1.10`
* Subnet: `255.255.255.0`
* Gateway: `192.168.1.1`



## Test Connectivity

* Internal ping:

  ```bash
  ping 192.168.1.1
  ```

* External (no direct Internet):

  ```bash
  ping 8.8.8.8
  ```
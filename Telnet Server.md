# Telnet Installation and Configuration Guide

## Introduction
Telnet is a network protocol that allows users to communicate with remote devices over a command-line interface. It enables remote login and command execution on another machine over a TCP/IP network. While Telnet is largely replaced by SSH due to security concerns, it is still used for debugging, network testing, and accessing legacy systems.

## What is a Telnet Server?
A Telnet server is a software application that listens for incoming Telnet connections on a specific port (typically port 23). Once a client connects, it provides a command-line interface for remote interaction with the server.

## How Does Telnet Work?
1. A client initiates a connection to the Telnet server over TCP/IP.
2. The server responds by establishing a session and providing a login prompt.
3. Once authenticated, the user can execute commands as if they were physically present on the server.
4. The connection remains active until the user logs out or the session is terminated.

## Key Features of Telnet
- **Remote Access:** Allows users to manage and control remote systems.
- **Command Execution:** Enables command-line access on remote machines.
- **Lightweight:** Requires minimal system resources.
- **No Encryption:** Data is transmitted in plain text (making it less secure than SSH).
- **Supports Legacy Systems:** Still used in environments where SSH is not an option.

## 1. Install Telnet
```sh
rpm -qa | grep telnet
yum install telnet*
rpm -qa | grep telnet
```

## 2. Verify Installation
```sh
rpm -qi telnet-server
rpm -ql telnet-server
rpm -qd telnet-server
rpm -qc telnet-server
```

## 3. Configure Telnet Socket
Edit the Telnet socket configuration:
```sh
vim /etc/systemd/system/telnet.socket
```

Content of `telnet.socket`:
```ini
[Unit]
Description=Telnet Server Activation Socket

[Socket]
ListenStream=23
Accept=yes

[Install]
WantedBy=sockets.target
```

## 4. Configure Telnet Service
Edit the Telnet service configuration:
```sh
vim /etc/systemd/system/telnet@.service
```

Content of `telnet@.service`:
```ini
[Unit]
Description=Telnet Server Service

[Service]
ExecStart=-/usr/sbin/in.telnetd
StandardInput=socket
```

## 5. Verify Telnet Port (23) Status
```sh
netstat -nltup | grep 23
```

## 6. Configure Firewall (iptables)
Edit the iptables configuration:
```sh
vim /etc/sysconfig/iptables
```

Ensure the following rule exists:
```sh
-A INPUT -p tcp -m tcp --dport 23 -j ACCEPT
```

## 7. Restart Services
```sh
systemctl restart telnet.socket
systemctl restart iptables.service
```

## 8. How to Use Telnet After Configuration
Once the Telnet server is configured and running, you can connect to it using the following command:
```sh
telnet <server-ip>
```
Replace `<server-ip>` with the actual IP address of your Telnet server. If the connection is successful, you will see a login prompt where you can enter your credentials and start using the remote system.

To exit the Telnet session, type:
```sh
exit
```

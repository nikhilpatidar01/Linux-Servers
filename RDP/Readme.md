# XRDP Server

XRDP is an open-source Remote Desktop Protocol (RDP) server for Linux systems. It allows users to access a Linux desktop environment remotely using Microsoft’s Remote Desktop Client (RDP), typically from a Windows machine or any RDP-compatible client.

## Server Setup

- Update the Package Index

    ```bash
    yum update -y
    ```

- Install the XRDP Server

    ```bash
    yum install epel-release -y
    yum install xrdp -y
    ```

- Enable XRDP to Start Automatically at Boot

    ```bash
    systemctl enable xrdp
    ```



- Start or Restart XRDP Services

    ```bash
    systemctl restart xrdp
    systemctl restart xrdp-sesman
    ```

Note: `xrdp-sesman` may be included in the `xrdp` systemd service and may not require separate management on CentOS.



- Verify Listening Ports

    ```bash
    ss -nltup | grep xrdp
    ```



- Check XRDP Service Status

    ```bash
    systemctl status xrdp
    ```

# RDP Clients on Linux

Remote Desktop Protocol (RDP) allows you to connect to remote systems using a graphical interface. Below are common RDP clients that work on CentOS 9 (install via EPEL or source when needed).


## 1. rdesktop

- Install rdesktop

    ```bash
    yum install rdesktop -y
    ```

- Usage

    ```bash
    rdesktop <remote-ip>
    ```



## 2. xfreerdp

- Install FreeRDP

    ```bash
    yum install freerdp -y
    ```

- Usage

    ```bash
    xfreerdp /u:<username> /p:<password> /v:<remote-ip>
    ```

    Example:

    ```bash
    xfreerdp /u:admin /p:password123 /v:192.168.1.100
    ```



## 3. Remmina

- Install Remmina and Plugins

    ```bash
    yum install remmina remmina-plugins-rdp -y
    ```

- Launch Remmina

    ```bash
    remmina
    ```

Use the GUI to add and manage remote connections.



## 4. NoMachine

- Download and Install

1. Go to: [https://www.nomachine.com/download](https://www.nomachine.com/download)
2. Download the `.rpm` package for RHEL/CentOS.

- Install NoMachine

    ```bash
    rpm -ivh nomachine_<version>.rpm
    ```

- Resolve dependencies if needed:

    ```bash
    yum install -y <missing-packages>
    ```

- Launch

    ```bash
    nomachine
    ```
XRDP is an open-source Remote Desktop Protocol (RDP) server for Linux systems. It allows users to access a Linux desktop environment remotely using Microsoft’s Remote Desktop Client (RDP), typically from a Windows machine or any RDP-compatible client.

# Server Setup

- Update the Package Index


    ```bash
    sudo apt update
    ```
- Install the XRDP server:

    ```bash
    sudo apt install xrdp
    ```

- Enable XRDP to start automatically at system boot:

    ```bash
    sudo systemctl enable xrdp
    ```



- Start or Restart XRDP Services


    ```bash
    sudo systemctl restart xrdp
    sudo systemctl restart xrdp-sesman
    ```


- Verify Listening Ports

    ```bash
    sudo netstat -nltup | grep xrdp
    ```



- Alternative: Enable XRDP at Startup Using `update-rc.d`


    ```bash
    sudo update-rc.d xrdp enable
    ```

-  Ensure the XRDP service is running:

    ```bash
    sudo service xrdp status
    ```


## Notes

* Make sure a desktop environment such as **XFCE**, **MATE**, or **GNOME** is installed. XRDP does not provide a GUI on its own.

  Example (install XFCE):

  ```bash
  sudo apt install xfce4 xfce4-goodies -y
  echo "startxfce4" > ~/.xsession
  ```

* Kali Linux may block incoming RDP connections. Ensure TCP port `3389` is open in the firewall.

* For troubleshooting, check these log files:

  * `/var/log/xrdp.log`
  * `/var/log/xrdp-sesman.log`



# RDP Client

Remote Desktop Protocol (RDP) allows you to connect to remote Windows or Linux systems using a graphical interface. Below are four common RDP clients you can use on Debian-based systems.



## 1. rdesktop

### Install rdesktop

```bash
sudo apt update
sudo apt install rdesktop -y
```

### Usage

```bash
rdesktop <remote-ip>
```

Example:

```bash
rdesktop 192.168.1.100
```

### Notes

* `rdesktop` is a legacy client and may not support the latest RDP protocols.
* Useful for older Windows systems (up to Windows 7 or Server 2008 R2).



## 2. xfreerdp

`xfreerdp` is part of the FreeRDP project and supports modern RDP features, including NLA, clipboard sharing, and USB redirection.

### Install FreeRDP

```bash
sudo apt update
sudo apt install freerdp2-x11 -y
```

### Usage

```bash
xfreerdp /u:<username> /p:<password> /v:<remote-ip>
```

Example:

```bash
xfreerdp /u:admin /p:password123 /v:192.168.1.100
```

### Common Options

| Option         | Description               |
| - |  |
| `/u:`          | Username                  |
| `/p:`          | Password                  |
| `/v:`          | Target IP or hostname     |
| `/cert:ignore` | Ignore certificate errors |
| `/sound`       | Enable remote sound       |
| `/clipboard`   | Enable clipboard sharing  |



## 3. Remmina

Remmina is a graphical RDP client that supports multiple protocols, including RDP, VNC, and SSH.

### Install Remmina

```bash
sudo apt update
sudo apt install remmina remmina-plugin-rdp -y
```

### Launch Remmina

```bash
remmina
```

### Usage

1. Open Remmina.
2. Click the "+" icon to create a new connection.
3. Set protocol to "RDP".
4. Enter server IP, username, and password.
5. Save and connect.

### Features

* Supports multiple remote protocols.
* Graphical interface with saved sessions.
* Clipboard and file sharing support.



## 4. NoMachine

NoMachine is a high-performance remote desktop solution for Linux, Windows, and macOS.

### Download NoMachine

1. Visit: [https://www.nomachine.com/download](https://www.nomachine.com/download)
2. Choose the `.deb` package for Debian/Ubuntu.

### Install NoMachine

After downloading:

```bash
sudo dpkg -i nomachine_<version>.deb
```

If there are missing dependencies, fix them:

```bash
sudo apt --fix-broken install
```

### Start NoMachine

Launch the GUI via:

```bash
nomachine
```

### Usage

* Detects other NoMachine hosts on the LAN.
* Connect by IP address if needed.
* Use your system credentials to log in.



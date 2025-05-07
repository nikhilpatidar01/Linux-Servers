Telnet transmits data in **plain text**, including usernames and passwords. It is **not secure** and should only be used in **trusted** environments for **testing or legacy systems**. For secure remote access, prefer **SSH**.



## Install Telnet Server Package

- Telnet server is provided by the `telnet-server` package, and the client by `telnet`.

  ```bash
  yum install telnet telnet-server -y
  ```



## Enable and Start Telnet via `xinetd`

- Install `xinetd`, which manages Telnet connections:

  ```bash
  yum install xinetd -y
  ```

- Enable the Telnet service:

  ```bash
  systemctl enable xinetd
  systemctl start xinetd
  ```



## Configure Telnet Service

Edit the Telnet service configuration:

```bash
vim /etc/xinetd.d/telnet
```

Ensure the following settings are present:

```ini
service telnet
{
    disable = no
    flags           = REUSE
    socket_type     = stream
    wait            = no
    user            = root
    server          = /usr/sbin/in.telnetd
    log_on_failure  += USERID
}
```

Save and exit.



## Allow Telnet Through Firewall

Open port **23** (default for Telnet):

```bash
firewall-cmd --permanent --add-port=23/tcp
firewall-cmd --reload
```



## Restart `xinetd` and Enable Telnet

Restart the service to apply changes:

```bash
systemctl restart xinetd
```

Check if Telnet is listening:

```bash
netstat -tnlp | grep :23
```



## Enable Telnet Access for Users

Ensure the user account exists and has a valid shell:

```bash
useradd telnetuser
passwd telnetuser
```

Verify the user's shell is not `/sbin/nologin` or `/bin/false`.



## Connect via Telnet

From a **remote** client or the same machine, run:

```bash
telnet <server-ip>
```

Example:

```bash
telnet 192.168.1.10
```

Enter the username and password when prompted.



## Optional: Disable SELinux (if you run into issues)

Only if you encounter SELinux-related errors:

```bash
setenforce 0
```

To permanently disable:

```bash
vim /etc/selinux/config
```

Change:

```bash
SELINUX=disabled
```

Reboot system for changes to take effect.

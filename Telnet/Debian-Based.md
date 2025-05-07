Telnet transmits data in plain text and is **not secure**. It is strongly recommended to use **SSH** for remote access. Use Telnet **only in trusted environments** or for testing/legacy systems.



## Install the Telnet Server

Install both the Telnet daemon and client:

```bash
apt update
apt install telnetd telnet -y
```

This installs `telnetd` (the server) and `telnet` (the client tool).



## Enable the Telnet Service

Telnet is managed by `inetd` or `openbsd-inetd`. Verify its status:

```bash
systemctl status inetd
```

If not active, start and enable it:

```bash
systemctl start inetd
systemctl enable inetd
```

If `inetd` is not installed, install it:

```bash
apt install openbsd-inetd -y
```



## Verify That Telnet is Listening

Ensure that port 23 is open and `telnetd` is listening:

```bash
ss -tnlp | grep :23
```

You should see output indicating that `telnetd` is bound to port 23.



## Create a Telnet User

Create a user who will log in via Telnet:

```bash
adduser telnetuser
```

Set a password and ensure the user has a valid shell (e.g., `/bin/bash`):

```bash
getent passwd telnetuser
```

The output should confirm the shell is not `/usr/sbin/nologin` or `/bin/false`.



## Test Telnet Connection

From the client or the same server, test the connection:

```bash
telnet <server-ip>
```

Example:

```bash
telnet 192.168.1.100
```

Login using the username and password created earlier.
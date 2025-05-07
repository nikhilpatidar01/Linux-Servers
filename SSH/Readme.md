**SSH (Secure Shell)** is a secure network protocol used for remote system login and management. It provides encrypted communication, strong authentication, and secure data transfer over untrusted networks.


## Install OpenSSH Packages

* Install all SSH-related packages

  ```bash
  yum install openssh*
  ```

* Install OpenSSH server only

  ```bash
  yum install openssh-server
  ```

* Install SSH client tools

  ```bash
  yum install openssh-clients
  ```


## Manage SSH Daemon with systemd

* Check SSH service status

  ```bash
  systemctl status sshd.service
  ```

* Start SSH service

  ```bash
  systemctl start sshd.service
  ```

* Enable SSH service at boot

  ```bash
  systemctl enable sshd.service
  ```



## Change Default SSH Port

Changing the default port from `22` helps reduce exposure to automated attacks.

* Edit SSH configuration file

  ```bash
  vim /etc/ssh/sshd_config
  ```

* Modify the port directive (uncomment if necessary)

  ```conf
  Port 2222
  ```

* Allow the new SSH port through firewall

  ```bash
  firewall-cmd --permanent --add-port=2222/tcp
  ```

* (Optional) Remove default SSH port from firewall

  ```bash
  firewall-cmd --permanent --remove-service=ssh
  ```

* Reload firewall rules

  ```bash
  firewall-cmd --reload
  ```

* Verify allowed ports

  ```bash
  firewall-cmd --list-ports
  ```



## Restart Required Services

* Restart firewall service (if using iptables)

  ```bash
  systemctl restart iptables.service
  ```

* Restart SSH service

  ```bash
  systemctl restart sshd.service
  ```



## Bind SSH to a Specific IP Address

By default, SSH listens on all interfaces. You can restrict it to a specific IP for tighter control.

* Edit SSH configuration

  ```bash
  vim /etc/ssh/sshd_config
  ```

* Add or update IP binding and port

  ```conf
  ListenAddress 192.168.1.38
  Port 2222
  ```

* Restart SSH service

  ```bash
  systemctl restart sshd.service
  ```



## Prevent Root Login via SSH

To improve security, disable root login via SSH and enforce login through regular users.

* Edit SSH configuration

  ```bash
  vim /etc/ssh/sshd_config
  ```

* Disable root login

  ```conf
  PermitRootLogin no
  ```

* Restart SSH service

  ```bash
  systemctl restart sshd.service
  ```



## Enable Root Login via SSH (if required)

* Edit SSH configuration

  ```bash
  vim /etc/ssh/sshd_config
  ```

* Ensure the following line is present and not commented

  ```conf
  PermitRootLogin yes
  ```

* Restart SSH service

  ```bash
  systemctl restart sshd.service
  ```

## Public Key Authentication

- Open the SSH configuration file:

  ```bash
  vim /etc/ssh/sshd_config
  ```

- Modify the following settings:

  ```bash
  PubkeyAuthentication yes
  PasswordAuthentication no
  ```

* Switch to the non-root user

  ```bash
  su - armour
  ```

* Generate RSA key pair

  ```bash
  ssh-keygen -t rsa
  ```

This creates two files in the `~/.ssh/` directory:

* `id_rsa` – private key

* `id_rsa.pub` – public key

* Copy public key to root’s authorized\_keys

  ```bash
  cp /home/armour/.ssh/id_rsa.pub /root/.ssh/authorized_keys
  ```

* Set correct permissions

  ```bash
  chmod 600 /root/.ssh/authorized_keys
  ```

* Restart SSH service to apply changes

  ```bash
  systemctl restart sshd.service
  ```


### Use the Private Key to Connect

* Create the private key file

  ```bash
  vim id_rsa
  ```

Paste the private key content manually into the file.

* Set the correct file permission

  ```bash
  chmod 600 id_rsa
  ```

* Connect to the server using private key

  ```bash
  ssh -i id_rsa root@192.168.95.145
  ```

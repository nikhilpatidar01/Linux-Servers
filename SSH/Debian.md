## Install OpenSSH Packages

* Install all SSH-related packages

  ```bash
  apt install openssh*
  ```

* Install OpenSSH server only

  ```bash
  apt install openssh-server
  ```

* Install SSH client tools

  ```bash
  apt install openssh-client
  ```



## Manage SSH Daemon with systemd

* Check SSH service status

  ```bash
  systemctl status ssh
  ```

* Start SSH service

  ```bash
  systemctl start ssh
  ```

* Enable SSH service at boot

  ```bash
  systemctl enable ssh
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


## Restart Required Services

* Restart firewall service (if using iptables)

  ```bash
  systemctl restart netfilter-persistent
  ```

* Restart SSH service

  ```bash
  systemctl restart ssh
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
  systemctl restart ssh
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
  systemctl restart ssh
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
  systemctl restart ssh
  ```



## Public Key Authentication

* Edit SSH configuration

  ```bash
  vim /etc/ssh/sshd_config
  ```

* Modify or ensure the following settings exist

  ```conf
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

This creates the following in `~/.ssh/`:

* `id_rsa` – private key

* `id_rsa.pub` – public key

* Copy public key to root’s authorized keys

  ```bash
  cp /home/armour/.ssh/id_rsa.pub /root/.ssh/authorized_keys
  ```

* Set correct permissions

  ```bash
  chmod 600 /root/.ssh/authorized_keys
  ```

* Restart SSH service

  ```bash
  systemctl restart ssh
  ```



## Use the Private Key to Connect

* Create the private key file

  ```bash
  vim id_rsa
  ```

Paste the private key content manually into the file.

* Set correct file permission

  ```bash
  chmod 600 id_rsa
  ```

* Connect to the server using private key

  ```bash
  ssh -i id_rsa root@192.168.95.145
  ```

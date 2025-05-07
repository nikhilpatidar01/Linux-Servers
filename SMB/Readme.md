# Samba

Samba is an open-source implementation of the SMB/CIFS networking protocol, allowing Linux systems to share files and printers with Windows and other systems using the SMB protocol.



## Installation

* Install the base Samba package

  ```bash
  yum install samba
  ```

* Install all related Samba packages (clients, tools, etc.)

  ```bash
  yum install samba*
  ```



## Configuration

* Edit the Samba configuration file

  ```bash
  nano /etc/samba/smb.conf
  ```

  Refer to `/etc/samba/smb.conf.example` for syntax guidance.



## Managing Samba Users

* Create a new Linux user

  ```bash
  useradd smb-user1
  ```

* View help for the Samba password command

  ```bash
  smbpasswd -h
  ```

  Or

  ```bash
  smbpasswd --help
  ```

* Add a Linux user to Samba

  ```bash
  smbpasswd -a smb-user1
  ```

  ```bash
  smbpasswd -a armour
  ```



## Managing Samba Services

* Check the status of the Samba service

  ```bash
  systemctl status smb.service
  ```

* Start the Samba service

  ```bash
  systemctl start smb.service
  ```

* Enable the Samba service to start on boot

  ```bash
  systemctl enable smb.service
  ```



## Firewall Configuration with Firewalld

Samba requires access to the following ports: `137/udp`, `138/udp`, `139/tcp`, `445/tcp`.

* Start firewalld

  ```bash
  systemctl start firewalld
  ```

* Enable firewalld at boot

  ```bash
  systemctl enable firewalld
  ```

* Check the status of firewalld

  ```bash
  firewall-cmd --state
  ```

* Add Samba service to the firewall permanently

  ```bash
  firewall-cmd --permanent --add-service=samba
  ```

* Alternatively, open Samba-related ports individually

  ```bash
  firewall-cmd --permanent --add-port=137/udp
  firewall-cmd --permanent --add-port=138/udp
  firewall-cmd --permanent --add-port=139/tcp
  firewall-cmd --permanent --add-port=445/tcp
  ```

* Reload the firewall to apply changes

  ```bash
  firewall-cmd --reload
  ```

* Verify currently active firewall rules

  ```bash
  firewall-cmd --list-all
  ```



## Testing Samba Shares from a Client

* List all available shares anonymously

  ```bash
  smbclient -L 192.168.112.145
  ```

* List available shares with user authentication

  ```bash
  smbclient -L 192.168.112.145 -U armour
  ```

* Connect to a specific share

  ```bash
  smbclient //192.168.112.145/armour -U armour
  ```



## Common SMB Shell Commands

* Download a file

  ```bash
  get confidential.txt
  ```

* Upload a file

  ```bash
  put notes.txt
  ```

* Download multiple files

  ```bash
  mget *.txt
  ```

* Toggle confirmation for multiple transfers

  ```bash
  prompt
  ```

* Upload multiple files

  ```bash
  mput *.pdf
  ```

* Go back to the previous directory

  ```bash
  cd
  ```

* Go to the root of the share

  ```bash
  cd /
  ```

* Create a new directory

  ```bash
  mkdir backups
  ```

* Delete a file

  ```bash
  del filename
  ```

* Remove a directory

  ```bash
  rmdir dirname
  ```

* Exit the SMB shell

  ```bash
  exit
  ```
  

## Anonymous Samba Share

Anonymous shares allow access to shared folders without user authentication. This is commonly used in internal or trusted networks where public file access is needed.



### Prepare Public Directory

* Create a publicly accessible directory

  ```bash
  mkdir /Public
  ```

* Set permissions to allow full access

  ```bash
  chmod 777 /Public/
  ```

* Recursively set full permissions (if needed)

  ```bash
  chmod -R 777 /Public/
  ```

* Verify the directory and permissions

  ```bash
  ls -lh / | grep Public
  ```

* Set ownership to the nobody user and group

  ```bash
  chown -R nobody:nobody /Public/
  ```



### Configure Samba for Anonymous Access

* Open the Samba configuration file

  ```bash
  nano /etc/samba/smb.conf
  ```

* Add or update the following share definition

  ```ini
  [Public]
  path = /Public
  writable = yes
  guest ok = yes
  guest only = yes
  read only = no
  create mode = 0777
  directory mode = 0777
  force user = nobody
  ```

This configuration enables:

* Anonymous (guest-only) access
* Read, write, delete permissions for all users
* File operations executed as `nobody` user



### Restart Samba Service

* Restart the Samba service to apply changes

  ```bash
  systemctl restart smb.service
  ```



### Test Anonymous Share Access

* List all available shares

  ```bash
  smbclient -L 192.168.112.145
  ```

* Connect anonymously to the public share

  ```bash
  smbclient //192.168.1.25/Public/
  ```

* Explicitly use guest access with no password

  ```bash
  smbclient -L 192.168.112.145 -U "" -N
  ```

* Directly connect to the share as guest

  ```bash
  smbclient //192.168.112.145/Public -U "" -N
  ```

* Alternative form of guest connection

  ```bash
  smbclient -U "" //192.168.1.27/Public -N
  ```

## Installation

* Update the package list

  ```bash
  apt update
  ```

* Install the base Samba package

  ```bash
  apt install samba
  ```

* Optionally, install all related Samba utilities

  ```bash
  apt install samba samba-common samba-client smbclient
  ```



## Configuration

* Edit the main Samba configuration file

  ```bash
  nano /etc/samba/smb.conf
  ```

  Refer to `/etc/samba/smb.conf.example` for syntax guidance.



## Managing Samba Users

* Create a new Linux user

  ```bash
  adduser smb-user1
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
  systemctl status smbd.service
  ```

* Start the Samba service

  ```bash
  systemctl start smbd.service
  ```

* Enable Samba to start on boot

  ```bash
  systemctl enable smbd.service
  ```


## Testing Samba Shares from a Client

* List available shares anonymously

  ```bash
  smbclient -L 192.168.112.145
  ```

* List shares with user authentication

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

* Toggle interactive confirmation

  ```bash
  prompt
  ```

* Upload multiple files

  ```bash
  mput *.pdf
  ```

* Go back to previous directory

  ```bash
  cd
  ```

* Go to root of the share

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

Anonymous shares allow unauthenticated access, useful for public or shared access environments.

### Prepare Public Directory

* Create a public directory

  ```bash
  mkdir /Public
  ```

* Set full permissions

  ```bash
  chmod 777 /Public/
  ```

* Recursively apply permissions (if needed)

  ```bash
  chmod -R 777 /Public/
  ```

* Verify the directory and permissions

  ```bash
  ls -lh / | grep Public
  ```

* Change ownership to `nobody`

  ```bash
  chown -R nobody:nogroup /Public/
  ```

### Configure Samba for Anonymous Access

* Edit the Samba configuration

  ```bash
  nano /etc/samba/smb.conf
  ```

* Add the following share definition

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

### Restart Samba Service

* Restart the Samba service to apply changes

  ```bash
  systemctl restart smbd.service
  ```



### Test Anonymous Share Access

* List shares

  ```bash
  smbclient -L 192.168.112.145
  ```

* Connect anonymously

  ```bash
  smbclient //192.168.1.25/Public/
  ```

* Use guest access explicitly

  ```bash
  smbclient -L 192.168.112.145 -U "" -N
  ```

* Directly connect as guest

  ```bash
  smbclient //192.168.112.145/Public -U "" -N
  ```

* Alternate guest connection

  ```bash
  smbclient -U "" //192.168.1.27/Public -N
  ```



## Configure Samba Share for Selected Users

### Edit Samba Configuration

* Edit the Samba config file

  ```bash
  vim /etc/samba/smb.conf
  ```

* Add or modify the following configuration

  ```ini
  [global]
  workgroup = SAMBA
  security = user
  passdb backend = tdbsam

  printing = cups
  printcap name = cups
  load printers = yes
  cups options = raw

  [homes]
  comment = Home Directories
  valid users = %S, %D%w%S%
  browseable = no
  read only = no
  inherit acls = yes

  [printers]
  comment = All Printers
  path = /var/tmp
  printable = yes
  create mask = 0600
  browseable = no

  [print$]
  comment = Printer Drivers
  path = /var/lib/samba/drivers
  write list = @printadmin, root
  force group = @printadmin
  create mask = 0664
  directory mask = 0775

  [data]
  comment = Data
  path = /opt/data
  public = yes
  writable = yes
  guest ok = no
  guest only = no

  [backup]
  comment = Server Backup
  path = /backup
  public = yes
  writable = yes
  valid users = infosec, armour
  ```

The `[backup]` share is accessible only to `infosec` and `armour`.



### Access the Restricted Share

* Attempt access as unauthorized user

  ```bash
  smbclient //192.168.112.145/backup -U smb-user1
  ```

* Access the share as `infosec`

  ```bash
  smbclient -L 192.168.112.145 -U infosec
  ```

* Access the share as `armour`

  ```bash
  smbclient -L 192.168.112.145 -U armour
  ```

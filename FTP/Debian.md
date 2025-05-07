## Install vsftpd and FTP Client

* Install the FTP server and client packages

  ```bash
  apt update
  apt install vsftpd ftp
  ```



## Configure Anonymous FTP Access

* Edit the main configuration file

  ```bash
  nano /etc/vsftpd.conf
  ```

* Example configuration:

  ```ini
  anonymous_enable=YES
  local_enable=YES
  write_enable=YES
  local_umask=022
  anon_upload_enable=YES
  anon_mkdir_write_enable=YES
  dirmessage_enable=YES
  xferlog_enable=YES
  connect_from_port_20=YES
  xferlog_std_format=YES
  listen=NO
  listen_ipv6=YES
  pam_service_name=vsftpd
  userlist_enable=YES
  pasv_min_port=55000
  pasv_max_port=55999
  pasv_enable=YES
  ```

This enables:

* Anonymous and local user logins
* Anonymous uploads and directory creation
* Passive FTP support



## FTP Client Usage

* Connect to an FTP server

  ```bash
  ftp <ftp_connection_ip>
  ```



## Create Local Users

* Add FTP users

  ```bash
  adduser u1
  adduser u2
  ```

* These commands will prompt you to set passwords and user information.



## Configure Local User Access

* Edit the vsftpd configuration

  ```bash
  nano /etc/vsftpd.conf
  ```

* Recommended configuration:

  ```ini
  anonymous_enable=NO
  local_enable=YES
  write_enable=YES
  local_umask=022
  dirmessage_enable=YES
  xferlog_enable=YES
  connect_from_port_20=YES
  xferlog_std_format=YES
  chroot_local_user=YES
  allow_writeable_chroot=YES
  pasv_enable=YES
  pasv_min_port=55000
  pasv_max_port=55999
  pam_service_name=vsftpd
  userlist_enable=YES
  listen=NO
  listen_ipv6=YES
  ```



## Restart vsftpd Service

* Apply configuration changes

  ```bash
  systemctl restart vsftpd
  ```



## Set Home Directory Permissions

* Ensure user directories exist

  ```bash
  mkdir -p /home/u1 /home/u2
  chown u1:u1 /home/u1
  chown u2:u2 /home/u2
  chmod 755 /home/u1 /home/u2
  ```

Use `chmod 700` for private directories.



## Allow Root Login (Not Recommended)

* Edit `/etc/ftpusers` and `/etc/vsftpd.user_list`, comment out `root`

  ```bash
  nano /etc/ftpusers
  nano /etc/vsftpd.user_list
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd
  ```



## Block or Unblock Users

### Disable Anonymous Access

* Edit configuration

  ```bash
  nano /etc/vsftpd.conf
  ```

  Change:

  ```ini
  anonymous_enable=YES
  ```

  to:

  ```ini
  anonymous_enable=NO
  ```

* Restart service

  ```bash
  systemctl restart vsftpd
  ```

### Enable Anonymous Access

* Reverse the change above and restart service

  ```bash
  anonymous_enable=YES
  systemctl restart vsftpd
  ```

### Unblock Local User

* Ensure these lines exist and are uncommented in `vsftpd.conf`

  ```ini
  chroot_local_user=YES
  allow_writeable_chroot=YES
  ```

* Restart service

  ```bash
  systemctl restart vsftpd
  ```

### Unblock Root User

* Comment out root in both deny files

  ```bash
  nano /etc/ftpusers
  nano /etc/vsftpd.user_list
  ```

* Restart service

  ```bash
  systemctl restart vsftpd
  ```

### Block Root User Again

* Uncomment root in both files

  ```bash
  nano /etc/ftpusers
  nano /etc/vsftpd.user_list
  ```

* Restart service

  ```bash
  systemctl restart vsftpd
  ```



## Change Default FTP Directory

* Create a new directory

  ```bash
  mkdir /ftp_home
  ```

* Update vsftpd configuration

  ```bash
  nano /etc/vsftpd.conf
  ```

  Add or update:

  ```ini
  local_root=/ftp_home
  ```

* Restart service

  ```bash
  systemctl restart vsftpd
  ```



## Configure Deny List

* Add usernames to these files to deny access

  ```bash
  nano /etc/ftpusers
  nano /etc/vsftpd.user_list
  ```

* Restart service

  ```bash
  systemctl restart vsftpd
  ```



## Configure Allow List

* Create a custom allow list

  ```bash
  nano /etc/vsftpd.allow_users
  ```

  Add allowed usernames.

* Update configuration

  ```bash
  nano /etc/vsftpd.conf
  ```

  Add:

  ```ini
  #chroot_local_user=YES
  userlist_enable=YES
  userlist_file=/etc/vsftpd.allow_users
  userlist_deny=NO
  ```

* Restart service

  ```bash
  systemctl restart vsftpd
  ```

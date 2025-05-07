## What is FTP

FTP (File Transfer Protocol) is a standard network protocol used to transfer files between a client and server on a computer network. It supports both anonymous and authenticated access and can be configured to allow uploads, downloads, and directory management.



## Install vsftpd and FTP Client

* Install the FTP server and client if not already installed

  ```bash
  yum install vsftpd ftp
  ```



## Configure Anonymous FTP Access

* Edit the main configuration file for vsftpd

  ```bash
  nano /etc/vsftpd/vsftpd.conf
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

This configuration enables:

* Anonymous and local user login
* Upload and directory creation for anonymous users
* Passive mode support



## FTP Client

* Connect to the FTP server

  ```bash
  ftp <ftp_connection_ip>
  ```



## Create Local Users

* Add FTP users

  ```bash
  useradd u1
  useradd u2
  ```

* Set passwords for new users

  ```bash
  passwd u1
  passwd u2
  ```



## Configure Local FTP Access

* Edit the configuration file

  ```bash
  vim /etc/vsftpd/vsftpd.conf
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



## SELinux Configuration (If Applicable)

* Check SELinux status

  ```bash
  cat /etc/sysconfig/selinux
  ```

* Enable FTP home directory access if SELinux is enforcing

  ```bash
  setsebool -P ftp_home_dir 1
  ```



## Restart vsftpd Service

* Restart the service to apply changes

  ```bash
  systemctl restart vsftpd.service
  ```



## Set Up Proper Home Directory Permissions

* Create home directories

  ```bash
  mkdir -p /home/u1 /home/u2
  ```

* Assign ownership

  ```bash
  chown u1:u1 /home/u1
  chown u2:u2 /home/u2
  ```

* Set permissions

  ```bash
  chmod 755 /home/u1 /home/u2
  ```

You may use `chmod 700` for private directories.



## Allow Root Login

* Edit deny list

  ```bash
  nano /etc/vsftpd/ftpusers
  ```

* Comment out the root user line:

  ```text
  #root
  ```



## Block or Unblock Users

### Block Default User (Anonymous)

* Disable anonymous access

  ```bash
  vim /etc/vsftpd/vsftpd.conf
  ```

  Change this line:

  ```ini
  anonymous_enable=YES
  ```

  to:

  ```ini
  anonymous_enable=NO
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd.service
  ```

### Unlock Default User (Anonymous)

* Enable anonymous access

  ```bash
  vim /etc/vsftpd/vsftpd.conf
  ```

  Change this line:

  ```ini
  anonymous_enable=NO
  ```

  to:

  ```ini
  anonymous_enable=YES
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd.service
  ```

### Unblock Regular User

* Enable local user access

  ```bash
  vim /etc/vsftpd/vsftpd.conf
  ```

  Ensure these lines are active:

  ```ini
  chroot_local_user=YES
  allow_writeable_chroot=YES
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd.service
  ```

### Unblock Root User

* Comment out root in both files

  ```bash
  vim /etc/vsftpd/user_list
  vim /etc/vsftpd/ftpusers
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd.service
  ```

### Block Root User

* Uncomment root in both files

  ```bash
  vim /etc/vsftpd/user_list
  vim /etc/vsftpd/ftpusers
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd.service
  ```



## Change Default FTP Directory

* Create a new folder

  ```bash
  mkdir /folder_name
  ```

* Set it as the default FTP directory

  ```bash
  vim /etc/vsftpd/vsftpd.conf
  ```

  Add this line:

  ```ini
  local_root=/folder_name
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd.service
  ```



## Configure Deny List

* Add users to block from FTP access

  ```bash
  vim /etc/vsftpd/user_list
  vim /etc/vsftpd/ftpusers
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd.service
  ```



## Configure Allow List

* Create allow user file

  ```bash
  vim /etc/vsftpd/allow_users
  ```

  Add allowed usernames to this file.

* Update configuration

  ```bash
  vim /etc/vsftpd/vsftpd.conf
  ```

  Modify as follows:

  ```ini
  #chroot_local_user=YES
  userlist_enable=YES
  userlist_file=/etc/vsftpd/allow_users
  userlist_deny=NO
  ```

* Restart the service

  ```bash
  systemctl restart vsftpd.service
  ```
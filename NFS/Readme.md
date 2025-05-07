A **Network File System (NFS) Server** allows users to access, share, and manage files over a network as if they were stored locally. 

## **Install NFS Packages**

* Check if NFS packages are installed

  ```bash
  rpm -qa | grep nfs
  ```

* Install NFS utilities

  ```bash
  yum install nfs-utils
  ```

* Install `rpcbind` (required for NFS)

  ```bash
  yum install rpcbind
  ```

* View package details

  ```bash
  rpm -qi nfs-utils
  ```

* List installed files

  ```bash
  rpm -ql nfs-utils
  ```

* List documentation

  ```bash
  rpm -qd nfs-utils
  ```

* List configuration files

  ```bash
  rpm -qc nfs-utils
  ```



## **Enable and Start NFS Services**

* Enable services to start on boot

  ```bash
  systemctl enable rpcbind
  systemctl enable nfs-server
  systemctl enable nfs-lock
  systemctl enable nfs-idmap
  ```

* Start services now

  ```bash
  systemctl start rpcbind
  systemctl start nfs-server
  systemctl start nfs-lock
  systemctl start nfs-idmap
  ```



## **Create a Directory to Share**

* Create shared directory

  ```bash
  mkdir -p /data
  ```

* Set full permissions

  ```bash
  chmod 777 /data
  ```



## **Configure NFS Exports**

* Edit the exports file

  ```bash
  vim /etc/exports
  ```

* Example `/etc/exports` configuration

  ```apache
  /data/ *(rw,sync)
  ```

* Additional examples:

  ```apache
  # /data/ 192.168.1.0/24(ro,sync)
  # /data/ 192.168.1.0/24(rw,sync,no_root_squash,no_all_squash)
  ```



## **Export the Shared Directory**

* Export the share

  ```bash
  exportfs -rav
  ```

* Optional: Verify exports

  ```bash
  exportfs -v
  ```



## **Restart the NFS Server**

* Restart service

  ```bash
  systemctl restart nfs-server
  ```



## **Configure Firewalld for NFS**

* Enable and start firewalld

  ```bash
  systemctl enable firewalld
  systemctl start firewalld
  ```

* Open necessary ports

  ```bash
  firewall-cmd --permanent --add-port=2049/tcp
  firewall-cmd --permanent --add-port=111/tcp
  firewall-cmd --permanent --add-port=20048/tcp
  ```

* Add NFS services to firewall

  ```bash
  firewall-cmd --permanent --add-service=nfs
  firewall-cmd --permanent --add-service=mountd
  firewall-cmd --permanent --add-service=rpc-bind
  ```

* Reload firewall to apply changes

  ```bash
  firewall-cmd --reload
  ```

* Optional: List current firewall rules

  ```bash
  firewall-cmd --list-all
  ```



## **NFS Client Setup**

* Install required packages

  ```bash
  yum install rpcbind nfs-utils
  yum install showmount
  ```



## **Check Server Status**

* Check NFS server status

  ```bash
  systemctl status nfs-server
  ```

* View logs for errors

  ```bash
  journalctl -xe
  ```

* Check available NFS shares from client

  ```bash
  showmount -e 192.168.1.10
  ```



## **Mount NFS Share on Client**

* Create a mount point

  ```bash
  mkdir /mnt/d1
  ```

* Mount the NFS share

  ```bash
  mount -t nfs 192.168.1.10:/data /mnt/d1/
  ```

* Verify the mount

  ```bash
  df -h
  ```



## **Notes on Common Options**

* `rw` – Writable access
* `ro` – Read-only access
* `sync` – Ensures data is written before confirmation
* `no_root_squash` – Gives root on the client root privileges on the share
* `no_all_squash` – Keeps client users' UID/GID when accessing files


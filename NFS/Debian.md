## **Install NFS Packages**

* Update package list

  ```bash
  apt update
  ```

* Install NFS server and client utilities

  ```bash
  apt install nfs-kernel-server nfs-common
  ```



## **Enable and Start NFS Services**

* Enable and start the NFS server

  ```bash
  systemctl enable nfs-server
  systemctl start nfs-server
  ```

* Ensure `rpcbind` service is also running

  ```bash
  systemctl enable rpcbind
  systemctl start rpcbind
  ```



## **Create a Directory to Share**

* Create the shared directory

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
  nano /etc/exports
  ```

* Example `/etc/exports` configuration

  ```apache
  /data *(rw,sync,no_subtree_check)
  ```

* Additional examples:

  ```apache
  # /data 192.168.1.0/24(ro,sync,no_subtree_check)
  # /data 192.168.1.0/24(rw,sync,no_root_squash,no_subtree_check)
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

* Restart the NFS service

  ```bash
  systemctl restart nfs-server
  ```


## **NFS Client Setup**

* Install required packages

  ```bash
  apt install nfs-common
  ```



## **Check Server Status**

* Check NFS server status

  ```bash
  systemctl status nfs-server
  ```

* View logs for issues

  ```bash
  journalctl -xe
  ```

* Check exported shares from client

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
  mount 192.168.1.10:/data /mnt/d1
  ```

* Verify the mount

  ```bash
  df -h
  ```



## **Optional: Make Mount Persistent**

* Edit `/etc/fstab` on the client

  ```bash
  nano /etc/fstab
  ```

* Add this line to auto-mount at boot

  ```fstab
  192.168.1.10:/data /mnt/d1 nfs defaults 0 0
  ```

## **Common Export Options**

* `rw` – Read/write access
* `ro` – Read-only access
* `sync` – Writes data to disk before replying
* `no_root_squash` – Gives client root user full access
* `no_subtree_check` – Disables subtree checking (improves performance)

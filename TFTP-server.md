

---

# **TFTP (Trivial File Transfer Protocol) Guide**

### **What is TFTP?**  
TFTP is a simple protocol used to transfer files between two computers over a network. It's often used when advanced file transfer features are not needed.

### **How TFTP Works:**  
- **Protocol:** TFTP uses the **User Datagram Protocol (UDP)** for data transfer.  
- **Server & Client:** TFTP servers allow clients to send and receive files.  
- **No Authentication:** TFTP doesn’t require user login; if you have network access, you can use it.  

### **Common Uses of TFTP:**  
- Booting diskless computers, routers, and terminals.  
- Uploading web pages or downloading log files.  
- Copying configuration and system files to a server.  

### **Limitations of TFTP:**  
- **No Security:** It lacks encryption and secure access controls.  
- **No Error Checking:** TFTP doesn’t verify if the data is received correctly.  
- **Limited File Management:** You can’t delete, rename, or move files with TFTP.  

### **Related Protocols:**  
- **FTP (File Transfer Protocol):** Uses TCP for secure file transfers.  
- **BOOTP (Bootstrap Protocol):** Works with TFTP to help network devices boot up.  

---

## **Setting Up a TFTP Server on Linux**

### **1. Install TFTP and TFTP Server**  
```bash
yum install tftp tftp-server
```

### **2. Check the TFTP Server Package**  
```bash
rpm -qi tftp-server
```

### **3. Configure the TFTP Service**  
- The main configuration directory is `/var/lib/tftpboot/`.  
- By default, the TFTP service isn’t active, so you need to enable it.  

#### **Copy Service Files**  
```bash
cp -v /usr/lib/systemd/system/tftp.service /etc/systemd/system/tftp-server.service
cp -v /usr/lib/systemd/system/tftp.socket /etc/systemd/system/tftp-server.socket
```

#### **View the TFTP Server Manual**  
```bash
man in.tftpd
```

#### **Edit the Service Files**  
```bash
vim /etc/systemd/system/tftp-server.service
vim /etc/systemd/system/tftp-server.socket
```
- In the socket file, define the port (usually **69**).

### **4. Set Up the TFTP Boot Directory**  
```bash
cd /var/lib/tftpboot
chmod -R 777 /var/lib/tftpboot
```
- Place the files you want to transfer in this folder.

### **5. Restart the TFTP Service**  
```bash
systemctl daemon-reexec
systemctl restart tftp-server.socket
systemctl enable tftp-server.socket
```

### **6. Open Port 69 in the Firewall**  
```bash
iptables -A INPUT -p udp --dport 69 -j ACCEPT
service iptables restart
```

---

## **Using TFTP as a Client**

1. **Connect to the TFTP Server:**  
```bash
tftp 192.168.1.31
```

2. **Download (Get) a File:**  
```bash
get filename
```

3. **Upload (Put) a File:**  
```bash
put localfile remotefile
```

> **Note:** For file transfers, the file name must be known in advance because TFTP doesn’t support browsing directories.

---

## **Testing from a Windows PC**

- Install a TFTP client (like TFTPD32 or TFTP Client in Windows).  
- Use similar commands to download/upload files.

---


- **TFTP is simple but not secure.** Use it only in trusted environments.  
- **For larger or sensitive transfers, consider using FTP or SFTP.**

---


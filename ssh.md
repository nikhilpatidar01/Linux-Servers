# Introduction to SSH

• SSH (Secure Shell) is a cryptographic network protocol used for securely accessing and managing remote servers over an unsecured network. It provides strong authentication, encrypted communication, and data integrity.

• SSH is commonly used by system administrators and developers to:

• Remotely access command-line interfaces of Linux/Unix systems.

• Securely transfer files using scp or sftp.

•Tunnel other network protocols (port forwarding).

•Automate administrative tasks using key-based authentication.

# SSH Server Setup Guide (CentOS/RHEL Based)

## 1. Install / Reinstall OpenSSH

```bash
# Remove and reinstall OpenSSH server
yum remove openssh-server
yum install openssh*

# Verify package
rpm -qi openssh-server
rpm -ql openssh-server
rpm -qc openssh-server
rpm -qd openssh-server
```

---

## 2. Manage SSH Service

```bash
# Start and enable SSH service
systemctl start sshd.service
systemctl enable sshd.service
systemctl status sshd.service
```

---

## 3. Change SSH Default Port

```bash
vim /etc/ssh/sshd_config

# Modify:
#Port 22
Port 2222

# Restart SSH and iptables
systemctl restart sshd.service
systemctl restart iptables.service
```

---

## 4. Allow SSH Port in Firewall (iptables)

```bash
vim /etc/sysconfig/iptables

# Add this line:
-A INPUT -p tcp --dport 2222 -j ACCEPT

# Restart firewall
systemctl restart iptables.service

# Confirm port is open
netstat -nltup | grep ssh
```

---

## 5. Connect via New SSH Port (From Windows)

```powershell
ssh -p 2222 root@192.168.1.51
```

---

## 6. Restrict SSH Access to a Specific IP

```bash
vim /etc/ssh/sshd_config

# Modify:
#ListenAddress 0.0.0.0
ListenAddress 192.168.1.54

# Restart SSH
systemctl restart sshd.service
```

---

## 7. Disable Root Login

```bash
vim /etc/ssh/sshd_config.d/01-permitrootlogin.conf

# Modify:
#PermitRootLogin yes
PermitRootLogin no

# Restart SSH
systemctl restart sshd.service
```

---

## 8. Enforce Key-Based Authentication Only

```bash
vim /etc/ssh/sshd_config

# Ensure the following:
PubkeyAuthentication yes
PasswordAuthentication no

# Restart services
systemctl restart sshd.service
systemctl restart iptables.service
```

---

## 9. Generate SSH Keys (On Client/User Machine)

```bash
# As user (e.g. armour)
ssh-keygen

mkdir ~/.ssh
cd ~/.ssh

# Move public key
cp id_rsa.pub authorized_keys
```

---

## 10. Connect Using Private Key

```bash
ssh -i ~/.ssh/id_rsa armour@192.168.1.51 -p 2222
```

---

## 🔒 Notes

- `Port 2222` must be open in firewall.
- `PermitRootLogin no` disables root SSH access.
- Use `authorized_keys` for key-based authentication.
- Keep your `id_rsa` private key secure and **never share it**.

---

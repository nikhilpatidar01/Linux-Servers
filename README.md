<h1 align="center">Linux Servers</h1>

Comprehensive guides to install, configure, and manage various Linux-based server services. Each section contains detailed steps and use-case-specific configuration examples.



## DHCP (Dynamic Host Configuration Protocol) Server

- [DHCP Overview](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DHCP/Readme.md)
- [Exclusion of IP Addresses](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DHCP/IP-Exclusion.md)
- [IP Reservation](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DHCP/Reserved-IP.md)
- [Allow and Deny Lists](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DHCP/Allow-and-Deny-List.md)


## DNS (Domain Name System) Server

- [Introduction](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DNS/Readme.md)
- [DNS Client Tools](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DNS/DNS-Client-Tools.md)
- [Caching Server](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DNS/Cache-Only-DNS.md)
- [Primary (Master) DNS](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DNS/Primary-DNS.md)
- [Secondary (Slave) DNS](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DNS/Secondary-DNS.md)
- [Reverse Zone Configuration](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DNS/Reverse-Zone.md)
- [Forwarding Server](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/DNS/Forwarder-DNS%20.md)


## FTP (File Transfer Protocol) Server

- [Installation](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/FTP/Readme.md)
- [Anonymous FTP Access](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/FTP/Readme.md#configure-anonymous-ftp-access)
- [FTP Client Usage](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/FTP/Readme.md#ftp-client)
- [Change FTP Directory](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/FTP/Readme.md#Change-Default-FTP-Directory)
- [Root Login on vsftpd](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/FTP/Readme.md#allow-root-login)
- [Selected User Login on vsftpd](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/FTP/Readme.md#configure-allow-list)


## Apache Web Server

- [Overview and Installation](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Apache/Readme.md)
- [IP-based Binding](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Apache/Binding-with-IP.md)
- [Port-based Binding](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Apache/Binding-with-Ports.md)
- [Domain Name Binding](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Apache/Binding-with-Domain-Names.md)
- [User Home Directory Binding](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Apache/Binding-with-Home-Directory.md)
- [SSL/TLS Binding](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Apache/Binding-with-SSL-TLS.md)
- [CGI Script Handling](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Apache/CGI-Scripts.md)


## PHP, MySQL, and WordPress

- [PHP Installation](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Wordpress/PHP-Installation.md)
- [MySQL Installation](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Wordpress/MySQL.md)
- [PHPMyAdmin Setup](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Wordpress/PHPMyAdmin.md)
- [WordPress Setup](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Wordpress/Readme.md)


## SMB (Samba) Server

- [Overview and Installation](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SMB/Readme.md)
- [Managing Samba Users](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SMB/Readme.md#managing-samba-users)
- [Managing Samba Services](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SMB/Readme.md#managing-samba-services)
- [Firewall Configuration](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SMB/Readme.md#firewall-configuration-with-firewalld)
- [SMB Client Usage](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SMB/Readme.md#testing-samba-shares-from-a-client)
- [Anonymous Samba Share](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SMB/Readme.md#anonymous-samba-share)

## NFS (Network File System) Server

- [Overview](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/NFS/Readme.md)
- [Install and Setup NFS Server](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/NFS/Readme.md#install-nfs-packages)
- [Mount NFS Share](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/NFS/Readme.md#Mount-NFS-Share-on-Client)



## TFTP (Trivial File Transfer Protocol) Server

- [Overview](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/TFTP/Readme.md)

- [TFTP Installation](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/TFTP/Readme.md#installation)

## SSH (Secure Shell) Server

- [What is SSH](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SSH/Readme.md#ssh-secure-shell)
- [Install OpenSSH](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SSH/Readme.md#install-openssh-packages)
- [SSH Client Tools](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SSH/SSH-Tools.md)
- [Change Default SSH Port](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SSH/Readme.md#change-default-ssh-port)
- [Bind SSH with Specific IP Address](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SSH/Readme.md#bind-ssh-to-a-specific-ip-address)
- [Prevent Root Login via SSH](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SSH/Readme.md#prevent-root-login-via-ssh)
- [Enable Root Login via SSH](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SSH/Readme.md#enable-root-login-via-ssh-if-required)
- [Public Key Authentication](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/SSH/Readme.md#public-key-authentication)


## Proxy Server (Squid)

- [Proxy Setup](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Proxy/Readme.md)
- [Access Control Lists (ACL)](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Proxy/ACL.md)
- [User Authentication](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Proxy/User-Authentication.md)
- [Transparent Proxy](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Proxy/Transparent-Proxy.md)
- [SSL Bump](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Proxy/SSL-Bump.md)

## Telnet Server

- [Installation](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Telnet/Readme.md#install-telnet-server-package)
- [Configure Telnet Service](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Telnet/Readme.md#configure-telnet-service)
- [Enable Telnet Access for Users](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Telnet/Readme.md#enable-telnet-access-for-users)
- [Connect via Telnet](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/Telnet/Readme.md#connect-via-telnet)

## RDP (Remote Desktop Protocol)


- [XRDP Overview](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/RDP/Readme.md#xrdp-server)
- [Server Setup](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/RDP/Readme.md#server-setup)
- [RDP Clients](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/RDP/Readme.md#rdp-clients-on-linux)
  - [rdesktop](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/RDP/Readme.md#1-rdesktop)
  - [xfreerdp](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/RDP/Readme.md#2-xfreerdp)
  - [Remmina](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/RDP/Readme.md#3-remmina)
  - [NoMachine](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/RDP/Readme.md#4-nomachine)


## LDAP (Lightweight Directory Access Protocol)


- [Introduction](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/LDAP/Readme.md#ldap-lightweight-directory-access-protocol)
- [Setup Static IP Address](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/LDAP/Readme.md#configure-static-ip-address)
- [Configure DNS](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/LDAP/Readme.md#configure-dns-server-with-bind9)
- [Configure LDAP](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/LDAP/Readme.md#configure-ldap-server)
- [Add User to LDAP](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/LDAP/Readme.md#add-user-to-ldap)
- [Join LDAP Client](https://github.com/InfoSecWarrior/Linux-Servers/blob/main/LDAP/Readme.md#join-ldap-client)


## Contributing

Contributions are always welcome. Whether it's fixing bugs, improving documentation, or adding new features, your input is valuable.


<a href="https://github.com/InfoSecWarrior/Linux-Servers/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=InfoSecWarrior/Linux-Servers" />
</a>

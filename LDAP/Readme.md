**LDAP (Lightweight Directory Access Protocol)** is an open, vendor-neutral protocol used to access and manage directory information over a network. A directory in this context is a hierarchical database optimized for reading, searching, and managing information about users, groups, systems, and resources.

### Key Concepts

* **Directory Structure:** Hierarchical and tree-like (similar to a file system), with entries such as `dc=example,dc=com`, `ou=People`, `cn=John Doe`.
* **Entries:** Each entry is uniquely identified by a **Distinguished Name (DN)** and consists of attributes (e.g., `cn`, `sn`, `uid`, `mail`).
* **Schema:** Defines object types and the attributes they must or can have (e.g., `inetOrgPerson`, `organizationalUnit`).
* **Bind:** The process of authenticating to the LDAP server.
* **Base DN:** Starting point of a directory search, e.g., `dc=server,dc=local`.

## Configure Static IP Address

* Open network interfaces file

  ```bash
  vim /etc/network/interfaces
  ```

* Define a static IP configuration

  ```ini
  allow-hotplug enp0s3
  iface enp0s3 inet static
      address 192.168.1.200
      netmask 255.255.255.0
      gateway 192.168.1.1
      dns-nameserver 8.8.8.8
  ```



## Disable IPv6

* Open sysctl configuration file

  ```bash
  vim /etc/sysctl.conf
  ```

* Add these lines to disable IPv6

  ```conf
  net.ipv6.conf.all.disable_ipv6 = 1
  net.ipv6.conf.default.disable_ipv6 = 1
  net.ipv6.conf.lo.disable_ipv6 = 1
  ```

* Apply changes immediately

  ```bash
  sysctl -p
  ```



## Configure DNS Server with BIND9

* Set system hostname

  ```bash
  vim /etc/hostname
  ```

  ```
  deb.server.local
  ```

* Reboot the system to apply hostname

  ```bash
  reboot
  ```

* Install BIND9 and related packages

  ```bash
  apt install bind9 dnsutils -y
  ```

* Define a new DNS zone

  ```bash
  vim /etc/bind/named.conf.local
  ```

  ```conf
  zone "server.local" {
      type master;
      file "/etc/bind/db.server.local";
  };
  ```

* Create zone file based on default template

  ```bash
  vim /etc/bind/db.server.local
  ```

  ```conf
  $TTL    604800
  @       IN      SOA     ns.server.local. admin.server.local. (
                                2         ; Serial
                           604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                           604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      ns.server.local.
  @       IN      A       192.168.1.200
  ns      IN      A       192.168.1.200
  ldap    IN      A       192.168.1.200
  ```

* Restart DNS server

  ```bash
  systemctl restart bind9
  ```



## Configure LDAP Server

* Install LDAP and related utilities

  ```bash
  apt install slapd ldap-utils libpam-ldapd rsyslog ldapscripts -y
  ```

During installation, use these settings:

* LDAP URI: `ldap://deb.server.local`

* Base DN: `dc=server,dc=local`

* Services: `passwd`, `group`, `shadow`

* Admin DN password: `admin`

* Create LDAP config file to change admin DN

  ```bash
  vim ldap-config.ldif
  ```

  ```ldif
  dn: olcDatabase={1}mdb,cn=config
  changetype: modify
  replace: olcSuffix
  olcSuffix: dc=server,dc=local

  dn: olcDatabase={1}mdb,cn=config
  changetype: modify
  replace: olcRootDN
  olcRootDN: cn=admin,dc=server,dc=local

  dn: olcDatabase={1}mdb,cn=config
  changetype: modify
  replace: olcRootPW
  olcRootPW: {SSHA}+V8UkCHqkbPwyhlA7yweBebcnx6TXpBz
  ```

* Apply the new configuration

  ```bash
  ldapmodify -Y EXTERNAL -H ldapi:/// -f ldap-config.ldif
  ```

Admin login becomes: `admin:root`

* Create base DN structure

  ```bash
  vim base.ldif
  ```

  ```ldif
  dn: dc=server,dc=local
  objectClass: top
  objectClass: dcObject
  objectclass: organization
  o: server.local
  dc: server

  dn: cn=admin,dc=server,dc=local
  objectClass: simpleSecurityObject
  objectClass: organizationalRole
  cn: admin
  description: Directory Manager
  userPassword: {SSHA}+V8UkCHqkbPwyhlA7yweBebcnx6TXpBz
  ```

* Add base DN to LDAP

  ```bash
  ldapadd -x -D cn=admin,dc=server,dc=local -W -f base.ldif
  ```

* Define Organizational Units (People and Groups)

  ```bash
  vim ou-structure.ldif
  ```

  ```ldif
  dn: ou=People,dc=server,dc=local
  objectClass: organizationalUnit
  ou: People

  dn: ou=Groups,dc=server,dc=local
  objectClass: organizationalUnit
  ou: Groups
  ```

* Add OUs to LDAP

  ```bash
  ldapadd -x -D cn=admin,dc=server,dc=local -W -f ou-structure.ldif
  ```

* Test LDAP directory

  ```bash
  ldapsearch -x -LLL -b dc=server,dc=local -D cn=admin,dc=server,dc=local -W
  ```



## Add User to LDAP

* Generate password hash

  ```bash
  slappasswd
  ```

* Create LDIF file for new user

  ```bash
  vim testuser1.ldif
  ```

  ```ldif
  dn: uid=testuser1,ou=People,dc=server,dc=local
  uid: testuser1
  cn: testuser1
  objectClass: shadowAccount
  objectClass: top
  objectClass: person
  objectClass: inetOrgPerson
  objectClass: posixAccount
  userPassword: {SSHA}5rMM/3f8Ki13IyarGTtwzieoTu7KMgwc
  shadowLastChange: 17016
  shadowMin: 0
  shadowMax: 99999
  shadowWarning: 7
  loginShell: /bin/bash
  uidNumber: 1002
  gidNumber: 1002
  homeDirectory: /home/testuser1
  sn: testuser1
  mail: testuser1@server.local
  ```

* Add user to LDAP

  ```bash
  ldapadd -x -D "cn=admin,dc=server,dc=local" -W -f testuser1.ldif
  ```

* Test user authentication

  ```bash
  ldapwhoami -x -D "uid=testuser1,ou=People,dc=server,dc=local" -W
  ```



## Testing & Restarting Services

* Check LDAP configuration

  ```bash
  slaptest
  ```

* Restart required services

  ```bash
  systemctl restart nscd
  systemctl restart slapd
  systemctl restart rsyslog
  ```



# Join LDAP Client

Referance:- https://www.server-world.info/en/note?os=Debian_11&p=openldap&f=3

* Install required LDAP client packages

  ```bash
  apt -y install libnss-ldapd libpam-ldapd ldap-utils
  ```

During installation, use the following settings:

* LDAP URI: `ldap://192.168.1.200`

* Base DN: `dc=server,dc=local`

* Services to configure: `passwd`, `group`, `shadow`

* Enable automatic home directory creation on login

  ```bash
  vim /etc/pam.d/common-session
  ```

* Add the following line at the end of the file

  ```conf
  session required pam_mkhomedir.so skel=/etc/skel/ umask=077
  ```

* Restart name service and LDAP client daemon

  ```bash
  systemctl restart nscd
  systemctl restart nslcd
  ```

* Verify LDAP user is recognized by the system

  ```bash
  getent passwd testuser2
  ```
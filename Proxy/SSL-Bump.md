## Creating SSL Certificates for Squid


- Create a directory to store Squid’s SSL certificates:

   ```bash
   mkdir -p /etc/squid/ssl_cert
   cd /etc/squid/ssl_cert
   ```

- Generate a self-signed certificate:

   ```bash
   openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -keyout squidCA.key -out squidCA.crt
   cat squidCA.crt squidCA.key > squidCA.pem
   ```

- Check that the certificate files exist:

   ```bash
   ls -lh /etc/squid/ssl_cert
   ```

- You should see `squidCA.key`, `squidCA.crt`, and `squidCA.pem`.

## Preparing the SSL Database

- Create and initialize the database Squid uses for dynamic certificate generation:

   ```bash
   mkdir -p /var/lib/squid
   /usr/lib64/squid/security_file_certgen -c -s /var/lib/squid/ssl_db -M 20MB
   chown -R squid:squid /var/lib/squid/ssl_db/
   ```

   Verify the database:

   ```bash
   ls -lh /var/lib/squid/ssl_db/
   ```



## Disabling SELinux (Optional)

- Disabling SELinux can help avoid permission issues when using SSL bump.

   Edit the SELinux configuration:

   ```bash
   vim /etc/sysconfig/selinux
   ```

   Set the following:

   ```bash
   SELINUX=disabled
   ```

   Check the status:

   ```bash
   sestatus
   ```



## Configuring Squid for SSL Bump

- Edit the main Squid configuration file:

   ```bash
   vim /etc/squid/squid.conf
   ```

   Add or update with the following:

   ```bash
   http_port 3128 ssl-bump cert=/etc/squid/ssl_cert/squidCA.pem generate-host-certificates=on dynamic_cert_mem_cache_size=4MB

   acl step1 at_step SslBump1
   ssl_bump peek step1
   ssl_bump bump all

   http_access allow all

   sslcrtd_program /usr/lib64/squid/security_file_certgen -s /var/lib/squid/ssl_db -M 4MB
   sslcrtd_children 5
   dns_v4_first on
   ```



## Example Squid Configuration (Full)

Includes ACLs, access rules, SSL bump settings, and other best practices:

```bash
acl localnet src 10.0.0.0/8
acl localnet src 192.168.0.0/16
acl SSL_ports port 443
acl Safe_ports port 80
acl Safe_ports port 443
acl Safe_ports port 1025-65535
acl CONNECT method CONNECT

http_access deny !Safe_ports
http_access deny CONNECT !SSL_ports
http_access allow localnet
http_access allow localhost
http_access deny all

http_port 3128 ssl-bump cert=/etc/squid/ssl_cert/squidCA.pem generate-host-certificates=on dynamic_cert_mem_cache_size=4MB

acl step1 at_step SslBump1
ssl_bump peek step1
ssl_bump bump all

sslcrtd_program /usr/lib64/squid/security_file_certgen -s /var/lib/squid/ssl_db -M 4MB
sslcrtd_children 5
dns_v4_first on

coredump_dir /var/spool/squid

refresh_pattern ^ftp:           1440    20%     10080
refresh_pattern -i (/cgi-bin/|\?) 0     0%      0
refresh_pattern .               0       20%     4320
```



## Restart Squid

- Apply the changes:

   ```bash
   systemctl restart squid.service
   ```

   Check that Squid is listening:

   ```bash
   netstat -nltup | grep squid
   ```



## Enable IP Forwarding

- Edit sysctl:

   ```bash
   vim /etc/sysctl.d/ipv4_forward.conf
   ```

   Add:

   ```bash
   net.ipv4.ip_forward = 1
   ```

   Apply changes:

   ```bash
   sysctl --system
   ```

   Verify:

   ```bash
   sysctl net.ipv4.ip_forward
   ```

   Expected output:

   ```
   net.ipv4.ip_forward = 1
   ```



## Configure Firewalld

-  Open required ports:

   ```bash
   firewall-cmd --permanent --zone=public --add-port=3128/tcp
   firewall-cmd --permanent --zone=public --add-port=80/tcp
   firewall-cmd --permanent --add-masquerade
   firewall-cmd --reload
   ```

   Check configuration:

   ```bash
   firewall-cmd --list-all
   ```



## Distribute the Squid CA Certificate to Clients

- Copy the CA certificate to your web server directory:

   ```bash
   cp -v /etc/squid/ssl_cert/squidCA.crt /var/www/html/
   ```

   Access it via browser:

   ```
   http://<proxy-server-ip>/squidCA.crt
   ```

   Example:

   ```
   http://192.168.1.1/squidCA.crt
   ```



## Install Squid CA Certificate in Firefox

Go to:
**Settings → Privacy & Security → Certificates → View Certificates**

Import the `squidCA.crt` file.
Enable the trust options:

* Trust this CA to identify websites
* (Optional) Trust this CA to identify email users



## Configure Proxy on Client Browser

Go to your browser’s proxy settings and manually enter:

* IP Address: `192.168.1.1` (replace as needed)
* Port: `3128`

Save the settings.



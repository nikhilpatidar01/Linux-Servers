# Binding with SSL/TLS in Apache


* Install `mod_ssl` module for Apache

  ```bash
  yum install mod_ssl
  ```

* Check if `mod_ssl` is installed

  ```bash
  rpm -qi mod_ssl
  ```

* List installed files for `mod_ssl`

  ```bash
  rpm -ql mod_ssl
  ```

* Display documentation for `mod_ssl`

  ```bash
  rpm -qd mod_ssl
  ```

* Edit SSL configuration file

  ```bash
  vim /etc/httpd/conf.d/ssl.conf
  ```

* Set SSL certificate and key paths

  ```apache
  SSLCertificateFile /etc/pki/tls/certs/localhost.crt
  SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
  ```

* Restart Apache to apply SSL settings

  ```bash
  systemctl restart httpd.service
  ```

* Confirm Apache is listening on port 443

  ```bash
  netstat -nltup | grep httpd
  ```

* Allow SSL/TLS ports through the firewall

  ```bash
  firewall-cmd --permanent --add-port=443/tcp
  firewall-cmd --permanent --add-port=8080/tcp
  firewall-cmd --reload
  ```



## Create Self-Signed SSL Certificates

* Create directory to store SSL certificates

  ```bash
  mkdir /opt/ssl
  cd /opt/ssl
  ```

* Generate self-signed certificate and private key

  ```bash
  openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365
  ```

* Verify generated files

  ```bash
  ls -lh
  file key.pem
  file cert.pem
  ```

* Copy SSL files to proper system directories

  ```bash
  cp -v /opt/ssl/cert.pem /etc/pki/tls/certs/cert.pem
  cp -v /opt/ssl/key.pem /etc/pki/tls/private/key.pem
  ```



## Update Apache Configuration with SSL Certificates

* Edit SSL configuration file

  ```bash
  vim /etc/httpd/conf.d/ssl.conf
  ```

* Update certificate and key paths

  ```apache
  SSLCertificateFile /etc/pki/tls/certs/cert.pem
  SSLCertificateKeyFile /etc/pki/tls/private/key.pem
  ```

* Restart Apache to apply certificate updates

  ```bash
  systemctl restart httpd.service
  ```



## Verify SSL Binding in Apache

* Check if Apache is bound to ports 80 and 443

  ```bash
  netstat -nltup | grep httpd
  ```



## Update Firewall for SSL/TLS Ports

* Allow HTTPS (443) and custom ports through the firewall

  ```bash
  firewall-cmd --permanent --add-port=443/tcp
  firewall-cmd --permanent --add-port=8080/tcp
  firewall-cmd --reload
  ```



## Configure Apache Virtual Hosts with SSL/TLS

* Edit Apache main configuration file

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Add HTTP and HTTPS virtual host configurations

  ```apache
  <VirtualHost 192.168.112.145:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.145:443>
      SSLEngine on
      SSLCertificateFile /etc/pki/tls/certs/cert.pem
      SSLCertificateKeyFile /etc/pki/tls/private/key.pem
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.112:80>
      ServerName armour.local
      ServerAlias www.armour.local
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.145:443>
      SSLEngine on
      SSLCertificateFile /etc/pki/tls/certs/cert.pem
      SSLCertificateKeyFile /etc/pki/tls/private/key.pem
      ServerName armour.local
      ServerAlias www.armour.local
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>
  ```



## Advanced Configuration with Custom CA

* Navigate to SSL directory

  ```bash
  cd /opt/ssl
  ```

* Remove old certificate files

  ```bash
  rm -rf cert.pem key.pem
  ```

* Generate private key for custom CA

  ```bash
  openssl genrsa -out ca.key 2048
  ```

* Create Certificate Signing Request (CSR)

  ```bash
  openssl req -new -key ca.key -out ca.csr
  ```

* Sign the certificate with the CA key

  ```bash
  openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt
  ```

* Copy CA certificate and key to system directories

  ```bash
  cp -v /opt/ssl/ca.crt /etc/pki/tls/certs/ca.crt
  cp -v /opt/ssl/ca.key /etc/pki/tls/private/ca.key
  ```

* Edit Apache config for new CA certificates

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Configure virtual hosts with CA certificate

  ```apache
  <VirtualHost 192.168.112.145:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.145:443>
      SSLEngine on
      SSLCertificateFile /etc/pki/tls/certs/ca.crt
      SSLCertificateKeyFile /etc/pki/tls/private/ca.key
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.112:80>
      ServerName armour.local
      ServerAlias www.armour.local
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.145:443>
      SSLEngine on
      SSLCertificateFile /etc/pki/tls/certs/ca.crt
      SSLCertificateKeyFile /etc/pki/tls/private/ca.key
      ServerName armour.local
      ServerAlias www.armour.local
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Edit SSL defaults in `ssl.conf`

  ```bash
  vim /etc/httpd/conf.d/ssl.conf
  ```

* Revert certificate lines to defaults

  ```apache
  SSLCertificateFile /etc/pki/tls/certs/localhost.crt
  SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
  ```

* Restart Apache to apply changes

  ```bash
  systemctl restart httpd.service
  ```



## Final Steps

* Restart Apache to ensure all configurations are applied

  ```bash
  systemctl restart httpd.service
  ```

* Verify Apache is listening on required ports

  ```bash
  netstat -nltup | grep httpd
  ```

* Access test domains via browser using HTTP and HTTPS:

| Domain                                       | Port | Expected Output              |
| -- | - | - |
| [http://armour.local](http://armour.local)   | 80   | Site1 homepage               |
| [https://armour.local](https://armour.local) | 443  | Site1 (SSL-enabled) homepage |

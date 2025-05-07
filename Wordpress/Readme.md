WordPress is a free and open-source **Content Management System (CMS)** built on **PHP and MySQL**. It enables users to easily build and manage websites, from blogs to e-commerce platforms.


## Configure Wordpress

* Download the latest version of WordPress

  ```bash
  wget https://wordpress.org/latest.zip
  ```

* Extract the downloaded archive

  ```bash
  unzip latest.zip
  ```

* Move WordPress files to the Apache web directory

  ```bash
  cp -vr wordpress/ /var/www/html/
  ```

* Navigate to the Apache web directory

  ```bash
  cd /var/www/html/
  ```

* Set Apache as the owner of the WordPress directory

  ```bash
  chown -Rv apache:apache wordpress/
  ```

* Navigate into the WordPress directory

  ```bash
  cd wordpress/
  ```

* Set proper permissions on key WordPress directories

  ```bash
  chmod -Rv 0755 wp-includes/ wp-admin/js/ wp-content/themes/ wp-content/plugins/
  ```



## Apache VirtualHost Configuration


* Edit the Apache configuration file

  ```bash
  nano /etc/httpd/conf/httpd.conf
  ```

* Add the following block for secure HTTPS access

  ```apache
  <VirtualHost 192.168.112.146:443>
      SSLEngine on
      SSLCertificateFile /etc/pki/tls/certs/localhost.crt
      SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
      ServerName armour.local
      DocumentRoot /var/www/html/wordpress
      DirectoryIndex index.php
      ServerAlias www.armour.local
  </VirtualHost>
  ```

### Configure VirtualHost for HTTP

* Add the following block for non-secure HTTP access

  ```apache
  <VirtualHost 192.168.112.146:80>
      ServerName armour.local
      DocumentRoot /var/www/html/wordpress
      DirectoryIndex index.php
      ServerAlias www.armour.local
  </VirtualHost>
  ```



## Restart Services

* Restart Apache to apply the changes

  ```bash
  systemctl restart httpd
  ```

* Restart the firewall service

  ```bash
  systemctl restart firewalld
  ```

* Restart the DNS service (if configured)

  ```bash
  systemctl restart named
  ```

* Test domain resolution

  ```bash
  dig armour.local
  ```



## Access WordPress in Browser

* Open WordPress in a browser via HTTP

  ```
  http://192.168.112.145
  http://armour.local
  ```

* Open WordPress in a browser via HTTPS

  ```
  https://192.168.112.145
  https://armour.local
  ```
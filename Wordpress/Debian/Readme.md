## Configure WordPress

* Download the latest version of WordPress from the official website:

  ```bash
  wget https://wordpress.org/latest.zip
  ```

* Extract the downloaded package:

  ```bash
  unzip latest.zip
  ```

* Move WordPress files to the Apache web directory:

  ```bash
  cp -vr wordpress/ /var/www/html/
  ```

* Navigate to the web root directory:

  ```bash
  cd /var/www/html/
  ```

* Set the correct ownership to allow Apache to manage WordPress files:

  ```bash
  chown -Rv www-data:www-data wordpress/
  ```

* Navigate to the WordPress directory:

  ```bash
  cd wordpress/
  ```

* Set proper permissions for essential WordPress directories:

  ```bash
  chmod -Rv 0755 wp-includes/ wp-admin/js/ wp-content/themes/ wp-content/plugins/
  ```



## Apache VirtualHost Configuration

A **VirtualHost** configuration allows Apache to serve multiple websites from the same server.

* Open the Apache configuration file for editing:

  ```bash
  nano /etc/apache2/sites-available/wordpress.conf
  ```

* Configure VirtualHost for SSL
  To enable **HTTPS** support for your website, add the following configuration in the Apache VirtualHost file:

  ```apache
  <VirtualHost 192.168.112.55:443>
      SSLEngine on
      SSLCertificateFile /etc/ssl/certs/ssl-cert-snakeoil.pem
      SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
      ServerName armour.local
      DocumentRoot /var/www/html/wordpress
      DirectoryIndex index.php
      ServerAlias www.armour.local
  </VirtualHost>
  ```

* Configure VirtualHost for HTTP
  If you want your site to be accessible over **HTTP (non-secure)**, use this configuration:

  ```apache
  <VirtualHost 192.168.112.55:80>
      ServerName armour.local
      DocumentRoot /var/www/html/wordpress
      DirectoryIndex index.php
      ServerAlias www.armour.local
  </VirtualHost>
  ```

* Enable site and necessary modules:

  ```bash
  a2ensite wordpress.conf
  a2enmod ssl
  a2enmod rewrite
  ```



## Restart apache2

* Restart Apache to apply changes:

  ```bash
  systemctl restart apache2
  ```

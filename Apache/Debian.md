# Apache Web Server Setup and Configuration

## Install Apache Web Server

* Install Apache using `apt`:

  ```bash
  apt update
  apt install apache2
  ```

* Install all related Apache packages:

  ```bash
  apt install apache2*
  ```

## Manage Apache Service

* Check Apache Status:

  ```bash
  systemctl status apache2.service
  ```

* Start Apache Service:

  ```bash
  systemctl start apache2.service
  ```

* Enable Apache to Start on Boot:

  ```bash
  systemctl enable apache2.service
  ```

* Restart Apache Service:

  ```bash
  systemctl restart apache2.service
  ```

## Create or Edit Web Files

* Navigate to the document root:

  ```bash
  cd /var/www/html
  ```

* Create or edit an `index.html` file:

  ```bash
  vim index.html
  ```

* Remove Existing File:

  ```bash
  rm -f index.html
  ```

* Set Ownership of Web Files:

  ```bash
  chown -Rv www-data:www-data /var/www/html/*
  ```

## Load CSS Files

* Download and Extract Template Files:

  ```bash
  wget -4 "https://www.free-css.com/assets/files/free-css-templates/download/page296/oxer.zip"
  ```

* Unzip the template:

  ```bash
  unzip templatemo_571_hexashop.zip
  ```

* Remove the ZIP file:

  ```bash
  rm -rf templatemo_571_hexashop.zip
  ```

* Move extracted folder:

  ```bash
  mv -v templatemo_571_hexashop.zip /var/www/html/
  ```

* Configure Apache to Load CSS Files:
  Add the following line to Apache config file (usually `apache2.conf` or a site conf file):

  ```bash
  AddType text/html .shtml
  AddType text/css .css
  AddOutputFilter INCLUDES .shtml
  ```

* Edit the configuration file:

  ```bash
  nano /etc/apache2/apache2.conf
  ```

* Restart Apache:

  ```bash
  systemctl restart apache2.service
  ```

* Set ownership:

  ```bash
  chown -Rv www-data:www-data /var/www/html/*
  ```

## Binding with IP

* Open the Apache Configuration File:

  ```bash
  nano /etc/apache2/ports.conf
  ```

* Modify the *Listen* directive:

  ```bash
  Listen 127.0.0.1:80
  ```

* Configure Main File:
  Search for Listen Area:

  ```bash
  #Listen 12.34.56.78:80
  Listen 127.0.0.1:80
  ```

* Restart Apache After Modifying the Configuration:

  ```bash
  systemctl restart apache2.service
  ```

### Binding with IP Address in Apache

* Create Website Directories:

  ```bash
  cd /var/www/html/
  mkdir site1 site2 site3
  chown -Rv www-data:www-data site1 site2 site3
  ```

* Download and extract template:

  ```bash
  wget "https://www.free-css.com/assets/files/free-css-templates/download/page296/oxer.zip"
  unzip oxer.zip
  rm -rf oxer.zip
  mv -v oxer /var/www/html/site1/
  ```

* Test on browser:

  ```bash
  192.168.112.53/site1/
  ```

* Modify Apache Configuration:
  Open the main Apache configuration:

  ```bash
  nano /etc/apache2/apache2.conf
  ```

  Make sure this line is present:

  ```apache
  IncludeOptional sites-enabled/*.conf
  ```

  Virtual Host Entries:

  ```apache
  <VirtualHost 192.168.112.53:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.54:80>
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.55:80>
      DocumentRoot /var/www/html/site3/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Restart Apache:

  ```bash
  systemctl restart apache2.service
  ```

* Create Virtual Host Files

  site1.conf:

  ```bash
  nano /etc/apache2/sites-available/site1.conf
  ```

  ```apache
  <VirtualHost 192.168.112.53:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>
  ```

  Enable site:

  ```bash
  a2ensite site1.conf
  ```

  Restart Apache:

  ```bash
  systemctl restart apache2.service
  ```

  Repeat for site2.conf and site3.conf accordingly, with their IPs.

* Binding to All IPs:

  ```bash
  nano /etc/apache2/sites-available/site1.conf
  ```

  ```apache
  <VirtualHost *:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Validate Configuration:

  ```bash
  apache2ctl configtest
  ```

* Restart Apache:

  ```bash
  systemctl restart apache2.service
  ```

## Binding with Port

* Modify Apache Configuration:
  Open the main Apache configuration file:

  ```bash
  vim /etc/apache2/ports.conf
  ```

* Add Listen Directives for New Ports:
  The Listen directive tells Apache which ports to bind to. Add the following lines to allow Apache to listen on ports 80, 8080, 8081, and 8082:

  ```apache
  Listen 80
  Listen 8080
  Listen 8081
  Listen 8082
  ```

* Save and exit.

### Create Virtual Hosts in a Single File

* Open the main Apache configuration file:

  ```bash
  nano /etc/apache2/apache2.conf
  ```

  Ensure the following line is present to allow virtual hosts *(should be at the last line)*:

  ```apache
  IncludeOptional sites-enabled/*.conf
  ```

* Add multiple Virtual Host entries:

  ```apache
  <VirtualHost 192.168.112.145:8080>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.145:8081>
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.145:8082>
      DocumentRoot /var/www/html/site3/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Enable the configuration (if created in a separate file):

  ```bash
  a2ensite ports.conf
  ```

* Apply the changes by restarting Apache:

  ```bash
  systemctl restart apache2.service
  ```

## Binding with Domain Names

* Configure Hostnames (DNS or `/etc/hosts`):
  To simulate DNS resolution, define the domain names in the `/etc/hosts` file.

  Edit `/etc/hosts`:

  ```bash
  vim /etc/hosts
  ```

  Add the following entries:

  ```bash
  127.0.0.1   localhost
  ::1         localhost ip6-localhost ip6-loopback
  192.168.112.53 ns1 ns1.armour.local armour.local www.armour.local infosec.local www.infosec.local ai.local www.ai.local
  ```

* Virtual Host for `armour.local`:
  Create a virtual host file:

  ```bash
  nano /etc/apache2/sites-available/armour.conf
  ```

  Configuration:

  ```apache
  <VirtualHost 192.168.112.53:80>
     ServerName armour.local
     ServerAlias www.armour.local
     DocumentRoot /var/www/html/site1/
     DirectoryIndex index.html
  </VirtualHost>
  ```

  Enable the site:

  ```bash
  a2ensite armour.conf
  ```

### Virtual Host for `infosec.local`:

* Create:

  ```bash
  nano /etc/apache2/sites-available/infosec.conf
  ```

  Configuration:

  ```apache
  <VirtualHost 192.168.112.53:80>
     ServerName infosec.local
     ServerAlias www.infosec.local
     DocumentRoot /var/www/html/site2/
     DirectoryIndex index.html
  </VirtualHost>
  ```

  Enable:

  ```bash
  a2ensite infosec.conf
  ```

### Virtual Host for `ai.local`:

* Create:

  ```bash
  nano /etc/apache2/sites-available/ai.conf
  ```

  Configuration:

  ```apache
  <VirtualHost 192.168.112.53:80>
     ServerName ai.local
     ServerAlias www.ai.local
     DocumentRoot /var/www/html/site3/
     DirectoryIndex index.html
  </VirtualHost>
  ```

  Enable:

  ```bash
  a2ensite ai.conf
  ```

  Restart Apache:

  ```bash
  systemctl restart apache2
  ```

## Binding with SSL/TLS

### Install and Configure SSL/TLS in Apache

* Install `mod_ssl`:

  ```bash
  apt update
  apt install apache2
  a2enmod ssl
  ```

* Verify `mod_ssl` Installation:

  ```bash
  apache2ctl -M | grep ssl
  ```

* Configure SSL in Apache:
  Edit the SSL config file:

  ```bash
  nano /etc/apache2/sites-available/default-ssl.conf
  ```

  Ensure these paths are used:

  ```apache
  SSLCertificateFile /etc/ssl/certs/ssl-cert-snakeoil.pem
  SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
  ```

* Enable the Default SSL Site:

  ```bash
  a2ensite default-ssl
  ```

* Restart Apache:

  ```bash
  systemctl restart apache2
  ```

* Verify Apache Listening on SSL Port:

  ```bash
  netstat -nltup | grep apache2
  ```

* Update Firewall for SSL:

  ```bash
  ufw allow 443/tcp
  ufw allow 8080/tcp
  ufw reload
  ```



### Create Self-Signed SSL Certificates

* Create SSL Directory:

  ```bash
  mkdir /opt/ssl
  cd /opt/ssl
  ```

* Generate SSL Certificate and Key:

  ```bash
  openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365
  ```

* Check the Generated Files:

  ```bash
  ls -lh
  file key.pem
  file cert.pem
  ```

* Copy SSL Files:

  ```bash
  cp -v cert.pem /etc/ssl/certs/cert.pem
  cp -v key.pem /etc/ssl/private/key.pem
  ```



### Update Apache Configuration with SSL Certificates

* Edit SSL config:

  ```bash
  nano /etc/apache2/sites-available/default-ssl.conf
  ```

  Update lines:

  ```apache
  SSLCertificateFile /etc/ssl/certs/cert.pem
  SSLCertificateKeyFile /etc/ssl/private/key.pem
  ```

* Restart Apache:

  ```bash
  systemctl restart apache2
  ```



### Verify SSL Binding in Apache

* Check if Apache is listening on SSL port:

  ```bash
  netstat -nltup | grep apache2
  ```



### Configure Apache Virtual Hosts with SSL/TLS

* Enable `ssl` module (if not done):

  ```bash
  a2enmod ssl
  ```

* Create HTTP and HTTPS Virtual Host Files:

  ```bash
  nano /etc/apache2/sites-available/site1.conf
  ```

  ```apache
  <VirtualHost 192.168.112.53:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.53:443>
      SSLEngine on
      SSLCertificateFile /etc/ssl/certs/cert.pem
      SSLCertificateKeyFile /etc/ssl/private/key.pem
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Enable Sites:

  ```bash
  a2ensite site1.conf
  a2ensite armour.conf
  ```

* Restart Apache:

  ```bash
  systemctl restart apache2
  ```

## CIG Scripts



CGI (Common Gateway Interface) is a standard protocol for executing external programs on a web server, allowing them to generate dynamic content. It supports multiple scripting languages including Perl, Python, Shell, and PHP (though PHP is typically run via mod\_php or PHP-FPM, not CGI).



### **Install Apache and Enable CGI Module**

* Enable CGI module

  ```bash
  a2enmod cgi
  ```

* Install Perl and required modules

  ```bash
  apt install perl libcgi-pm-perl
  ```

* (Optional) Install Python for Python-based CGI scripts

  ```bash
  apt install python3
  ```

* Edit default Apache site configuration

  ```bash
  nano /etc/apache2/sites-available/000-default.conf
  ```

* Add inside `<VirtualHost *:80>` block

  ```apache
  <Directory "/usr/lib/cgi-bin">
      AllowOverride None
      Options +ExecCGI
      AddHandler cgi-script .cgi .pl .py .sh
      Require all granted
  </Directory>
  ```

* Ensure the CGI alias is present

  ```apache
  ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
  ```

* Restart Apache to apply changes

  ```bash
  systemctl restart apache2
  ```

### **Create and Test CGI Scripts**

* Create Perl CGI script

  ```bash
  nano /usr/lib/cgi-bin/test.cgi
  ```

  ```perl
  #!/usr/bin/perl
  print "Content-type: text/html\n\n";
  print "<h1>Server Memory Usage</h1>\n";
  print "<pre>\n";
  system("free -h");
  print "</pre>\n";
  ```

* Make script executable

  ```bash
  chmod 755 /usr/lib/cgi-bin/test.cgi
  ```

* Access in browser

  ```
  http://<your-ip>/cgi-bin/test.cgi
  ```



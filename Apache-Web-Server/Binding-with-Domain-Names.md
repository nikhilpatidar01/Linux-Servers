Thanks for providing your content and structure. Here's your "Binding with Domain Names in Apache" section rewritten strictly following your formatting rules:



# Binding with Domain Names in Apache

Apache can bind to specific domain names using the `ServerName` and `ServerAlias` directives in Virtual Hosts. This allows you to host multiple websites on the same IP address, each accessible by a unique domain name.



## Configure Hostnames (DNS or /etc/hosts)

* Edit the `/etc/hosts` file to simulate DNS resolution

  ```bash
  vim /etc/hosts
  ```

* Add domain entries pointing to the server IP

  ```bash
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
  192.168.112.145 ns1 ns1.armour.local armour.local www.armour.local infosec.local www.infosec.local ai.local www.ai.local
  ```



## Test DNS Resolution

* Verify DNS resolution for `www.armour.local`

  ```bash
  dig www.armour.local +short
  ```

* Verify DNS resolution for `www.infosec.local`

  ```bash
  dig www.infosec.local +short
  ```

* Edit zone file if IP address is incorrect

  ```bash
  nano /var/named/forward.infosec.local
  ```

* Update A record to correct IP

  ```bash
  infosec.local.  IN  A  192.168.112.145
  ```

* Restart named service

  ```bash
  systemctl restart named
  ```

* Re-check DNS resolution

  ```bash
  dig www.infosec.local +short
  ```



## Create Apache Virtual Host Files

* Open main Apache config file

  ```bash
  nano /etc/httpd/conf/httpd.conf
  ```

* Delete existing virtual host files

  ```bash
  cd /etc/httpd/conf.d/
  rm -rf site*
  ```



## Virtual Host for `armour.local`

* Create virtual host configuration file

  ```bash
  nano /etc/httpd/conf.d/armour.conf
  ```

* Add virtual host configuration

  ```apache
  <VirtualHost 192.168.112.145:80>
     ServerName armour.local
     ServerAlias www.armour.local
     DocumentRoot /var/www/html/site1/
     DirectoryIndex index.html
  </VirtualHost>
  ```



## Virtual Host for `infosec.local`

* Create virtual host configuration file

  ```bash
  nano /etc/httpd/conf.d/infosec.conf
  ```

* Add virtual host configuration

  ```apache
  <VirtualHost 192.168.112.145:80>
     ServerName infosec.local
     ServerAlias www.infosec.local
     DocumentRoot /var/www/html/site2/
     DirectoryIndex index.html
  </VirtualHost>
  ```



## Virtual Host for `ai.local`

* Create virtual host configuration file

  ```bash
  nano /etc/httpd/conf.d/ai.conf
  ```

* Add virtual host configuration

  ```apache
  <VirtualHost 192.168.112.145:80>
     ServerName ai.local
     ServerAlias www.ai.local
     DocumentRoot /var/www/html/site3/
     DirectoryIndex index.html
  </VirtualHost>
  ```

* Restart Apache to apply changes

  ```bash
  systemctl restart httpd
  ```



## Binding Domains with Different Ports

* Create a new virtual host for `armour.local` on port 8080

  ```bash
  nano /etc/httpd/conf.d/armour-8080.conf
  ```

* Add virtual host configuration

  ```apache
  <VirtualHost 192.168.112.145:8080>
     ServerName armour.local
     ServerAlias www.armour.local
     DocumentRoot /var/www/html/site4/
     DirectoryIndex index.html
  </VirtualHost>
  ```



## Validate Configuration

* Check Apache configuration syntax

  ```bash
  httpd -t
  ```



## Restart Apache

* Apply all configuration changes

  ```bash
  systemctl restart httpd.service
  ```



## Test the Configuration

* Check Apache is listening on expected ports

  ```bash
  netstat -nltup | grep httpd
  ```
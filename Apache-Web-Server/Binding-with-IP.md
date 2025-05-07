# Binding with IP

Apache can bind to specific IP addresses and ports, allowing you to host multiple websites on the same server using Virtual Hosts.

## Types of Apache Binding

### Bind to All Available IP Addresses

* Apache listens on all available interfaces by default:

  ```bash
  Listen 80
  ```

* Confirm configuration in main file:

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Restart Apache to apply changes:

  ```bash
  systemctl restart httpd.service
  ```

* Verify listening ports:

  ```bash
  netstat -nltup | grep httpd
  ```

### Bind to a Specific IP Address

* Edit configuration file:

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Set Apache to listen only on localhost:

  ```bash
  Listen 127.0.0.1:80
  ```

* Restart Apache:

  ```bash
  systemctl restart httpd.service
  ```

* Verify with netstat:

  ```bash
  netstat -nltup | grep httpd
  ```

### Bind to a LAN IP Address

* Edit configuration:

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Set Apache to listen on a specific LAN IP:

  ```bash
  Listen 192.168.112.145:80
  ```

* Restart Apache:

  ```bash
  systemctl restart httpd.service
  ```

* Confirm with netstat:

  ```bash
  netstat -nltup | grep httpd
  ```

### Bind to Multiple Ports

* Allow extra ports through firewall:

  ```bash
  firewall-cmd --permanent --add-port=81/tcp
  firewall-cmd --permanent --add-port=82/tcp
  firewall-cmd --reload
  ```

* Add port directives:

  ```bash
  Listen 80
  Listen 81
  Listen 82
  ```

* Restart Apache:

  ```bash
  systemctl restart httpd.service
  ```

* Verify all ports are in use:

  ```bash
  netstat -nltup | grep httpd
  ```



## Apache Binding with IP for Virtual Hosts

### Create Website Directories

* Navigate to document root:

  ```bash
  cd /var/www/html/
  ```

* Create site folders:

  ```bash
  mkdir site1 site2 site3
  ```

* Set permissions:

  ```bash
  chown -Rv apache:apache site1 site2 site3
  ```

* Download and extract a template:

  ```bash
  wget "https://www.free-css.com/assets/files/free-css-templates/download/page296/oxer.zip"
  unzip templatemo_571_hexashop.zip
  rm -rf templatemo_571_hexashop.zip
  mv -v templatemo_571_hexashop /var/www/html/site1/
  ```

* Access in browser:

  ```bash
  http://192.168.112.145/site1/
  ```



### Modify Main Apache Configuration

* Edit the main config file:

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Ensure this line is included at the bottom:

  ```apache
  IncludeOptional conf.d/*.conf
  ```

* Example inline Virtual Hosts:

  ```apache
  <VirtualHost 192.168.112.145:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.146:80>
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>

  <VirtualHost 192.168.112.147:80>
      DocumentRoot /var/www/html/site3/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Restart Apache:

  ```bash
  systemctl restart httpd.service
  ```



### Create Separate Virtual Host Files

* Create virtual host for site1:

  ```bash
  vim /etc/httpd/conf.d/site1.conf
  ```

* Add configuration:

  ```apache
  <VirtualHost 192.168.112.145:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Restart Apache:

  ```bash
  systemctl restart httpd.service
  ```

* Repeat for other sites:

  ```bash
  vim /etc/httpd/conf.d/site2.conf
  ```

  ```apache
  <VirtualHost 192.168.112.146:80>
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>
  ```

  ```bash
  vim /etc/httpd/conf.d/site3.conf
  ```

  ```apache
  <VirtualHost 192.168.112.147:80>
      DocumentRoot /var/www/html/site3/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Optionally bind to all IPs:

  ```bash
  vim /etc/httpd/conf.d/site1.conf
  ```

  ```apache
  <VirtualHost *:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>
  ```



### Validate Apache Configuration

* Check for syntax errors:

  ```bash
  httpd -t
  ```



### Restart Apache

* Restart the service:

  ```bash
  systemctl restart httpd.service
  ```



### Verify Listening Ports

* Confirm bindings:

  ```bash
  netstat -nltup | grep httpd
  ```

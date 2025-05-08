# Binding with Port

Apache can bind to specific ports, allowing you to run multiple websites on different ports using Virtual Hosts. This is useful when you want to test different sites on the same IP but on unique ports.



## Modify Apache Configuration

* Open Apache main configuration file

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Add Listen directives to allow Apache to listen on ports 80, 8080, 8081, and 8082

  ```apache
  Listen 80
  Listen 8080
  Listen 8081
  Listen 8082
  ```



## Open Ports in Firewall

* Add firewall rules for new ports

  ```bash
  firewall-cmd --permanent --add-port=8080/tcp
  firewall-cmd --permanent --add-port=8081/tcp
  firewall-cmd --permanent --add-port=8082/tcp
  firewall-cmd --reload
  ```



## Create Virtual Hosts in Main File

* Open Apache main configuration file

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Ensure virtual host config inclusion

  ```apache
  IncludeOptional conf.d/*.conf
  ```

* Add Virtual Host entries

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

* Restart Apache to apply changes

  ```bash
  systemctl restart httpd.service
  ```



## Create Virtual Hosts as Separate Files

* Create `site1.conf` for port 80

  ```bash
  vim /etc/httpd/conf.d/site1.conf
  ```

  ```apache
  <VirtualHost 192.168.112.145:80>
      DocumentRoot /var/www/html/site1/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Create `site2.conf` for port 8080

  ```bash
  vim /etc/httpd/conf.d/site2.conf
  ```

  ```apache
  <VirtualHost 192.168.112.145:8080>
      DocumentRoot /var/www/html/site2/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Create `site3.conf` for port 8081

  ```bash
  vim /etc/httpd/conf.d/site3.conf
  ```

  ```apache
  <VirtualHost 192.168.112.145:8081>
      DocumentRoot /var/www/html/site3/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Create `site4.conf` for port 8082

  ```bash
  vim /etc/httpd/conf.d/site4.conf
  ```

  ```apache
  <VirtualHost 192.168.112.145:8082>
      DocumentRoot /var/www/html/site4/
      DirectoryIndex index.html
  </VirtualHost>
  ```

* Restart Apache to apply changes

  ```bash
  systemctl restart httpd.service
  ```



## Set Permissions

* Change ownership of all site directories to Apache

  ```bash
  chown -Rv apache:apache /var/www/html/site1 /var/www/html/site2 /var/www/html/site3 /var/www/html/site4
  ```



## Validate Configuration

* Test Apache configuration for syntax errors

  ```bash
  httpd -t
  ```

* Example output

  ```bash
  Syntax OK
  ```



## Restart Apache Service

* Restart Apache to apply all configurations

  ```bash
  systemctl restart httpd.service
  ```



## Test Apache Binding

* Use netstat to verify Apache is listening on correct ports

  ```bash
  netstat -nltup | grep httpd
  ```
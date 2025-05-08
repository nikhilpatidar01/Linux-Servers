### **Host a Web Application from a User's Home Directory**

This setup enables Apache to serve an application directly from a user's home directory using the `mod_userdir` module. It's typically used for development or lightweight hosting without needing `/var/www`.



## Enable `mod_userdir` in Apache

* Edit the Apache userdir config

  ```bash
  vim /etc/httpd/conf.d/userdir.conf
  ```

* Use the following configuration

  ```apache
  <IfModule mod_userdir.c>
      UserDir enabled
      UserDir public_html
  </IfModule>

  <Directory /home/*/public_html>
      AllowOverride All
      Options Indexes FollowSymLinks
      Require all granted
  </Directory>
  ```



## Create Application Directory for User `armour`

* Create the required structure

  ```bash
  mkdir /home/armour/public_html
  ```

* Set correct permissions

  ```bash
  chmod 711 /home/armour
  chown -R armour:armour /home/armour/public_html
  chmod -R 755 /home/armour/public_html
  ```



## Place Application Files in User Directory

* Copy your app files (e.g. PHP or HTML)

  ```bash
  cp -r /path/to/app/* /home/armour/public_html/
  ```

* Ensure index file exists

  ```bash
  ls /home/armour/public_html/index.php
  ```



## SELinux and Firewall Adjustments

* Enable SELinux access for Apache

  ```bash
  setsebool -P httpd_enable_homedirs 1
  chcon -R -t httpd_sys_content_t /home/armour/public_html
  ```

* Allow HTTP/HTTPS in firewall

  ```bash
  firewall-cmd --add-service=http --permanent
  firewall-cmd --add-service=https --permanent
  firewall-cmd --reload
  ```



## Restart Apache to Apply Changes

* Restart Apache

  ```bash
  systemctl restart httpd
  ```



## Test in Browser

* Add domain to `/etc/hosts` if DNS is not set

  ```bash
  192.168.129.147    armour.com
  ```

* Access via browser

  ```
  http://armour.com/
  ```
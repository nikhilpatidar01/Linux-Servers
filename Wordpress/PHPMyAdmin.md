phpMyAdmin is a free and open-source web-based tool written in PHP, used to manage MySQL and MariaDB databases via a graphical user interface.

Visit the official website:
[https://www.phpmyadmin.net/](https://www.phpmyadmin.net/)



## Download and Extract phpMyAdmin

* Download the phpMyAdmin archive

  ```bash
  wget https://files.phpmyadmin.net/phpMyAdmin/5.2.2/phpMyAdmin-5.2.2-all-languages.zip
  ```

* Extract the downloaded archive

  ```bash
  unzip phpMyAdmin-5.2.2-all-languages.zip
  ```

* Create the target directory for phpMyAdmin

  ```bash
  mkdir /var/www/html/phpmyadmin
  ```

* Copy all extracted files into the web root

  ```bash
  cp -vr phpMyAdmin-5.2.2-all-languages/* /var/www/html/phpmyadmin/
  ```



## Configure phpMyAdmin

* Change to the web root directory

  ```bash
  cd /var/www/html/
  ```

* Copy the sample config file to the active config

  ```bash
  cp /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/config.inc.php
  ```

* Generate a secure blowfish secret

  ```bash
  pwgen 32 -1
  ```

* Open the config file to insert the secret

  ```bash
  nano /var/www/html/phpmyadmin/config.inc.php
  ```

* Replace the following line with your generated secret

  ```php
  $cfg['blowfish_secret'] = 'leikaT0ooyohW5au7chaip3sa4ahSuqu'; /* COOKIE AUTH ENCRYPTION KEY */
  ```



## Set Proper Ownership

* Set Apache as the owner of phpMyAdmin directory

  ```bash
  chown -Rv apache:apache /var/www/html/phpmyadmin
  ```



## Restart Apache Web Server

* Restart the Apache service to apply changes

  ```bash
  systemctl restart httpd.service
  ```



## Access phpMyAdmin Admin Panel

* Access phpMyAdmin in a browser

  ```
  http://192.168.112.146/phpmyadmin
  ```

  ```
  http://192.168.112.147/phpmyadmin
  ```

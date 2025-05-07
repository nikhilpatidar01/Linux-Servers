## phpMyAdmin Installation and Configuration

Visit the phpMyAdmin website:
  [phpMyAdmin](https://www.phpmyadmin.net/)

* Download and Extract phpMyAdmin

  ```bash
  wget https://files.phpmyadmin.net/phpMyAdmin/5.2.2/phpMyAdmin-5.2.2-all-languages.zip
  ```
  ```
  unzip phpMyAdmin-5.2.2-all-languages.zip
  ```
  ```
  mkdir /var/www/html/phpmyadmin
  ```
  ```
  cp -vr phpMyAdmin-5.2.2-all-languages/* /var/www/html/phpmyadmin/
  ```

* Configure phpMyAdmin

  ```bash
  cd /var/www/html/
  ```
  ```
  cp /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/
  ```
  ```
  config.inc.php
  ```

* Generate Random Secret Passphrase

  ```bash
  pwgen 32 -1
  ```

* Paste into config

  ```bash
  nano /var/www/html/phpmyadmin/config.inc.php
  ```

  Replace the following line:

  ```apache
  $cfg['blowfish_secret'] = ''; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */
  ```

  With the generated secret passphrase:

  ```apache
  $cfg['blowfish_secret'] = 'leikaT0ooyohW5au7chaip3sa4ahSuqu'; /* COOKIE AUTH ENCRYPTION KEY */
  ```

* Set Proper Ownership

  ```bash
  chown -Rv www-data:www-data /var/www/html/phpmyadmin
  ```

* Restart Apache

  ```bash
  systemctl restart apache2.service
  ```



## Access phpMyAdmin Admin Panel

* Open in Browser

  ```
  http://192.168.112.146/phpmyadmin
  http://192.168.112.147/phpmyadmin
  ```


## PHP Installation and Configuration

* Install Required Repositories and Tools

  ```bash
  apt update
  apt install software-properties-common apt-transport-https lsb-release ca-certificates wget unzip pwgen nano
  ```

* Add PHP Repository
  For the latest PHP versions:

  ```bash
  add-apt-repository ppa:ondrej/php
  apt update
  ```

* Search for Available PHP Versions

  ```bash
  apt search php
  apt show php
  apt show php8.1
  apt show php8.2
  ```

* Install PHP and Required Extensions

  ```bash
  apt install php php-common php-cli php-opcache php-gd php-curl php-mysql php-xml php-mbstring php-pear php-mbstring php-http php-session
  ```

  > Note: `php-http` is sometimes part of `pecl`. If needed, install via:

  ```bash
  pecl install pecl_http
  ```

* Verify PHP Installation

  ```bash
  php -v
  ```

* Restart Apache

  ```bash
  systemctl restart apache2.service
  ```

* Create phpinfo.php for Testing

  ```bash
  nano /var/www/html/phpinfo.php
  ```

  Add the following content:

  ```php
  <?php
      phpinfo();
  ?>
  ```

* Access phpinfo.php in Browser
  You can access the `phpinfo.php` page by navigating to the following URLs in your browser:

  ```
  http://192.168.112.147/phpinfo.php
  http://192.168.112.148/phpinfo.php
  ```



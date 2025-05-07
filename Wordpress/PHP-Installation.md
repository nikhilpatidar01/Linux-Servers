PHP (Hypertext Preprocessor) is an open-source server-side scripting language widely used for web development. It allows embedding logic within HTML and dynamically generates web pages.



## Install Required Repositories

* Install EPEL and YUM utilities

  ```bash
  yum install epel-release yum-utils
  ```

* List all enabled and available repositories

  ```bash
  yum repolist all
  ```

* Install the Remi repository

  ```bash
  yum install http://rpms.remirepo.net/enterprise/remi-release-9.rpm
  ```

* Recheck available repositories

  ```bash
  yum repolist all
  ```

* Show configuration file path for remi-release

  ```bash
  rpm -qc remi-release
  ```

* List installed files from remi-release

  ```bash
  rpm -ql remi-release
  ```

* Open remi.repo file for manual configuration

  ```bash
  nano /etc/yum.repos.d/remi.repo
  ```

* Refresh repository list again

  ```bash
  yum repolist all
  ```



## Search for Available PHP Versions

* Search all PHP packages

  ```bash
  yum search php
  ```

* View details of default PHP package

  ```bash
  yum info php.x86_64
  ```

* View details for PHP 8.1

  ```bash
  yum info php81.x86_64
  ```

* View details for PHP 8.4

  ```bash
  yum info php84.x86_64
  ```



## Install PHP and Required Extensions

* Install PHP and commonly used modules

  ```bash
  yum install php php-common.x86_64 php-cli.x86_64 php-opcache.x86_64 php-gd.x86_64 php-curl php-mysqlnd.x86_64 php-xml.x86_64 php-mbstring.x86_64 php-pear php-mbstring php-pecl-http php-session
  ```



## Verify PHP Installation

* Check installed PHP version

  ```bash
  php -v
  ```



## Restart Apache Web Server

* Restart the Apache service

  ```bash
  systemctl restart httpd.service
  ```



## Create a phpinfo.php File for Testing

* Create a PHP test file

  ```bash
  nano /var/www/html/phpinfo.php
  ```

* Add the following content

  ```php
  <?php
      phpinfo();
  ?>
  ```

* Access the file via web browser

  ```
  http://192.168.112.147/phpinfo.php
  ```

  ```
  http://192.168.112.148/phpinfo.php
  ```
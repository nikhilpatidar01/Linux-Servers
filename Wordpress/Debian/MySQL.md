## MySQL Installation and Configuration

* Download and Install MySQL APT Repository

  ```bash
  wget https://dev.mysql.com/get/mysql-apt-config_0.8.29-1_all.deb
  ```

  ```bash
  dpkg -i mysql-apt-config_0.8.29-1_all.deb
  ```

  * When prompted, use the arrow keys to select MySQL 8.0.
  * Press `Enter` to confirm and exit.

  ```bash
  apt update
  ```

* Enable and Disable MySQL Versions (If multiple versions are listed)

  > *(Skip if you selected 8.0 during APT repo config)*

  ```bash
  dpkg-reconfigure mysql-apt-config
  ```

* Search for MySQL Packages

  ```bash
  apt-cache search mysql-server
  ```

* Install MySQL Server and Development Packages

  ```bash
  apt install mysql-server libmysqlclient-dev
  ```

* Check MySQL Service Status

  ```bash
  systemctl status mysql
  ```

  ```bash
  systemctl enable mysql
  ```

  ```bash
  netstat -nltup | grep mysql
  ```

* Start and Restart MySQL Service

  ```bash
  systemctl start mysql
  ```

  ```bash
  systemctl restart mysql
  ```

  ```bash
  netstat -nltup | grep mysql
  ```

* Retrieve MySQL Root Password

  ```bash
  grep 'temporary password' /var/log/mysql/error.log
  ```

  > *(If no password is shown, root may be using auth\_socket plugin. We'll reset below.)*

* Secure MySQL Installation

  ```bash
  mysql -u root -p
  ```

  Enter the temporary password when prompted.

  ```bash
  mysql_secure_installation
  ```

  Example new password:

  ```
  Armour@123
  ```

* Login with New MySQL Root Password

  ```bash
  mysql -u root -p
  ```

  Enter Password:

  ```
  Armour@123
  ```

* Change Root Password (If Needed)

  ```bash
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'Armour@123';
  SHOW DATABASES;
  EXIT;
  ```

  Change Password:

  ```bash
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'Patidar@123';
  ```

  Show Result Output:

  ```bash
  mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Patidar@123';
  Query OK, 0 rows affected (0.05 sec)
  ```

---

* Show Databases

  ```bash
  SHOW DATABASES;
  ```

  Show Database Result Output:

  ```bash
  mysql> SHOW DATABASES;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  4 rows in set (0.00 sec)
  ```

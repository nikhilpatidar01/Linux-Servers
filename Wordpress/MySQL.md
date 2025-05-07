MySQL is an open-source relational database management system (RDBMS) that uses SQL (Structured Query Language) for data management and querying. It is maintained by Oracle Corporation.



## Download and Install MySQL Repository

* Download the MySQL repository RPM package

  ```bash
  wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
  ```

* Install the MySQL repository package

  ```bash
  yum install ./mysql80-community-release-el9-1.noarch.rpm
  ```

* List all enabled and available repositories

  ```bash
  yum repolist all
  ```



## Enable and Disable MySQL Versions

* Enable MySQL 8.0 repository

  ```bash
  yum-config-manager --enable mysql80-community
  ```

* Enable MySQL 5.7 repository (if needed)

  ```bash
  yum-config-manager --enable mysql57-community
  ```

* Disable MySQL 5.7 repository (if not needed)

  ```bash
  yum-config-manager --disable mysql57-community
  ```



## Search for MySQL Packages

* Search available MySQL-related packages

  ```bash
  yum search mysql
  ```



## Install MySQL Server and Development Libraries

* Install MySQL server and development packages

  ```bash
  yum install mysql-community-server mysql-community-devel
  ```



## Check MySQL Service Status

* Check current status of MySQL

  ```bash
  systemctl status mysqld
  ```

* Enable MySQL to start at boot

  ```bash
  systemctl enable mysqld
  ```

* Verify MySQL is listening on the correct port

  ```bash
  netstat -nltup | grep mysql
  ```



## Start and Restart MySQL Service

* Start the MySQL service

  ```bash
  systemctl start mysqld
  ```

* Restart the MySQL service

  ```bash
  systemctl restart mysqld
  ```

* Confirm MySQL service is running

  ```bash
  netstat -nltup | grep mysql
  ```



## Retrieve MySQL Root Temporary Password

* Locate the MySQL log file

  ```bash
  ls -lh /var/log/mysql/mysqld.log
  ```

* Extract the temporary password from the log

  ```bash
  cat /var/log/mysql/mysqld.log | grep 'temporary password'
  ```

* Example output:

  ```
  A temporary password is generated for root@localhost: s;r=oAt25UuR
  ```



## Secure MySQL Installation

* Log in with the temporary root password

  ```bash
  mysql -u root -p
  ```

* Run the security script

  ```bash
  mysql_secure_installation
  ```

* Example new root password:

  ```
  Armour@123
  ```



## Login with New MySQL Root Password

* Log in using the updated root password

  ```bash
  mysql -u root -p
  ```

* Enter password:

  ```
  Armour@123
  ```



## Change Root Password (If Needed)

* Update root password

  ```bash
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'Armour@123';
  ```

* Show existing databases

  ```bash
  SHOW DATABASES;
  ```

* Exit MySQL shell

  ```bash
  EXIT;
  ```



## Change Root Password Again (If Required)

* Run the password change query

  ```bash
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'Patidar@123';
  ```

* Example output:

  ```
  mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Patidar@123';
  Query OK, 0 rows affected (0.05 sec)
  ```



## Show All Databases

* List all available databases

  ```bash
  SHOW DATABASES;
  ```

* Example result:

  ```
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

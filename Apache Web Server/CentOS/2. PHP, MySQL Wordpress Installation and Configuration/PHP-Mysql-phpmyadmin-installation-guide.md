# PHP, MySQL, and phpMyAdmin Installation Guide (CentOS/RHEL)

This guide provides a complete and structured procedure for installing **PHP**, **MySQL**, and **phpMyAdmin** on CentOS/RHEL-based systems. It includes repository setup, package installation, service management, and basic configuration.

---

## 🧠 Introduction

- **PHP:** A server-side scripting language used to create dynamic web pages.
- **MySQL:** A relational database system used to store and manage data.
- **phpMyAdmin:** A web-based tool to manage MySQL databases easily through a browser.
---

## 🔧 PHP Installation

### 🔹 Step 1: Install Required Repositories

#### 🔸 Enable EPEL Repository
The EPEL (Extra Packages for Enterprise Linux) repository provides additional packages not included in default repos.

```bash
yum install epel-release -y
```

#### 🔸 Install Yum Utilities

```bash
yum install yum-utils -y
```

#### 🔸 Add REMI Repository
Used for installing the latest versions of PHP.

```bash
yum install -y http://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

#### 🔸 Enable REMI PHP Module

```bash
yum module enable php:remi-8.2 -y
```

#### 🔸 Verify REMI Installation

```bash
rpm -qa | grep remi
```

---

### 🔹 Step 2: Install PHP and Required Extensions

#### 🔸 Install PHP with Common Modules

```bash
yum install -y \
php php-common php-cli php-fpm php-mysqlnd php-gd \
php-mbstring php-xml php-pdo php-opcache php-pear \
php-process php-pecl-http nginx-filesystem
```

#### 🔸 Verify PHP Installation

```bash
php -v
```

#### 🔸 Restart Apache to Apply Changes

```bash
systemctl restart httpd.service
```

#### 🔸 Create PHP Info File

```bash
vim /var/www/html/phpinfo.php
```

Insert the following content:

```php
<?php
phpinfo();
?>
```

#### 🔸 Access in Browser

Visit:  
```
http://<server-ip>/phpinfo.php
```

✅ **PHP installation and verification completed.**

---

## 🛢️ MySQL Installation

### 🔹 Step 1: Add MySQL Repository

```bash
yum install -y https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
```

### 🔹 Step 2: Install MySQL Server

```bash
yum install -y mysql-community-server
```

### 🔹 Step 3: Enable & Start MySQL Service

```bash
systemctl enable mysqld.service
systemctl start mysqld.service
```

### 🔹 Step 4: Check MySQL Status

```bash
systemctl status mysqld.service
```

### 🔹 Step 5: Retrieve Temporary MySQL Root Password

```bash
grep 'temporary password' /var/log/mysqld.log
```

### 🔹 Step 6: Secure MySQL Installation

Run the security script and follow the prompts:

```bash
mysql_secure_installation
```

### 🔹 Step 7: Login to MySQL

```bash
mysql -u root -p
```

(Enter the new root password set during the secure installation process)

### ① (Optional) Change Root Password in SQL

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'your_new_secure_password';
```

✅ **MySQL installation and basic configuration complete.**

---

## 🖥️ phpMyAdmin Installation

### 🔹 Step 1: Download phpMyAdmin

```bash
cd /tmp
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.2/phpMyAdmin-5.2.2-all-languages.zip
```

### 🔹 Step 2: Extract phpMyAdmin

```bash
unzip phpMyAdmin-5.2.2-all-languages.zip
```

### 🔹 Step 3: Move to Web Directory

```bash
mkdir /var/www/html/phpmyadmin
mv phpMyAdmin-5.2.2-all-languages/* /var/www/html/phpmyadmin/
```

### 🔹 Step 4: Configure phpMyAdmin

```bash
cp /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/config.inc.php
vim /var/www/html/phpmyadmin/config.inc.php
```

Edit the file to set the **blowfish secret**:

```php
$cfg['blowfish_secret'] = 'your_random_string_here'; // Must be 32 chars
```

Generate a 32-character random string using:

```bash
pwgen 32 1
```

### 🔹 Step 5: Set Ownership and Permissions

```bash
chown -Rv apache:apache /var/www/html/phpmyadmin/
```

### 🔹 Step 6: Restart Services

```bash
systemctl restart httpd.service
systemctl restart mysqld.service
```

### 🔹 Step 7: Access phpMyAdmin via Browser

```
http://<server-ip>/phpmyadmin
```

### 🔐 Login Credentials

- **Username:** root  
- **Password:** (your MySQL root password)

✅ **phpMyAdmin is now installed and accessible.**

---

## ✅ Final Checklist

| Component  | Test Command / URL                            |
| ---------- | --------------------------------------------- |
| PHP        | `php -v`, `http://<server-ip>/phpinfo.php`    |
| MySQL      | `mysql -u root -p`                            |
| phpMyAdmin | `http://<server-ip>/phpmyadmin`               |

---

> 👨‍💻 Guide by - [Anuj Rawal](https://www.linkedin.com/in/anuj-r-70b364310/)

---

 

---

# **Apache Web Server Configuration Guide**

## **1. Basic Website Access**

- **Restart All Services:**  
  ```bash
  sudo systemctl restart httpd
  ```

- **Access Websites:**  
  - `http://192.168.1.31:80` → **Site 1**  
  - `http://www.armour.local` → **WordPress Website**  
  - `http://192.168.1.32` → **Testing Website**  
  - `http://192.168.1.32/armour-wp` → **Armour WP Directory**

## **2. Directory Listing and Indexing**

- **Navigate to Website Folder:**  
  ```bash
  cd /var/www/html/
  ls -R armour-wp
  ```

- **View Uploads:**  
  ```bash
  browse http://192.168.1.32/armour-wp/wp-content/uploads
  ```

- **Apache Default Behavior:**  
  - By default, Apache allows **directory listing** if there's no index file.  
  - Example:  
    - `http://armour.local/wp-content` → Shows an empty page (due to an empty `index.php`).  
    - Copy `index.php` to `/tmp/` to display files:  
      ```bash
      cp -v /var/www/html/armour-wp/wp-content/index.php /tmp/
      ```

- **Plugins Folder:**  
  - `http://armour.local/wp-content/plugins` → Shows nothing.  
  - Remove `index.php` to display files:  
    ```bash
    rm /var/www/html/armour-wp/wp-content/plugins/index.php
    ```

## **3. Disabling Directory Listing**

- **Edit Apache Configuration:**  
  ```bash
  sudo vim /etc/httpd/conf/httpd.conf
  ```

- **Disable Directory Listing Globally:**  
  - Find and modify:  
    ```
    Options -Indexes
    ```

- **Allow Directory Listing for Specific Folder (e.g., `/backup`):**  
  ```bash
  sudo vim /etc/httpd/conf/httpd.conf
  ```
  Add:  
  ```
  <Directory "/var/www/html/armour-wp/backup">
      Options +Indexes
  </Directory>
  ```

- **Restart Apache:**  
  ```bash
  sudo systemctl restart httpd
  ```

- **Test:**  
  - `http://www.armour.local/backup` → Displays files inside the folder.

## **4. IP-Based Access Control**

- **Allow Access from Specific IPs:**  
  ```bash
  <Directory "/var/www/html/armour-wp/backup">
      Order allow,deny
      Allow from 192.168.1.10 192.168.1.20
  </Directory>
  ```

- **Allow Only Localhost:**  
  ```bash
  Allow from 127.0.0.1
  ```

## **5. Authentication (Allow/Deny Users)**

### **Case 1: Using .htaccess**

1. **Create `.htaccess` File:**  
   ```bash
   sudo vim /var/www/html/armour-wp/backup/.htaccess
   ```

2. **Add Authentication Rules:**  
   ```
   AuthType Basic
   AuthName "Restricted Area"
   AuthUserFile /etc/httpd/htpasswd
   Require valid-user
   ```

3. **Create Password File:**  
   ```bash
   sudo htpasswd -c /etc/httpd/htpasswd user1
   sudo htpasswd /etc/httpd/htpasswd user2
   sudo htpasswd /etc/httpd/htpasswd user3
   ```

4. **Change Ownership:**  
   ```bash
   sudo chown -R apache:apache /etc/httpd/htpasswd
   ```

5. **Restart Apache:**  
   ```bash
   sudo systemctl restart httpd
   ```

### **Case 2: Without .htaccess (Direct Config Changes)**

1. **Remove `.htaccess` File:**  
   ```bash
   sudo rm /var/www/html/armour-wp/backup/.htaccess
   ```

2. **Modify Apache Config:**  
   ```bash
   sudo vim /etc/httpd/conf/httpd.conf
   ```

3. **Add Authentication Directly:**  
   ```
   <Directory "/var/www/html/armour-wp/backup">
       AuthType Basic
       AuthName "Restricted Area"
       AuthUserFile /etc/httpd/htpasswd
       Require valid-user
   </Directory>
   ```

4. **Restart Apache:**  
   ```bash
   sudo systemctl restart httpd
   ```

## **6. Hosting Websites from User's Home Directory**

1. **Disable Default Bindings:**  
   ```bash
   sudo vim /etc/httpd/conf/httpd.conf
   ```

2. **Enable User Directory Support:**  
   ```bash
   sudo vim /etc/httpd/conf.d/userdir.conf
   ```

3. **Create User’s Public Directory:**  
   ```bash
   sudo mkdir /home/armour/public_html
   sudo chmod 755 /home/armour/public_html
   sudo chown -R armour:armour /home/armour/public_html
   ```

4. **Set Home Directory Permissions:**  
   ```bash
   sudo chmod 711 /home/armour
   ```

5. **Copy Website Files:**  
   ```bash
   sudo cp -vr /var/www/html/site1/* /home/armour/public_html/
   ```

6. **Restart Apache:**  
   ```bash
   sudo systemctl restart httpd
   ```

7. **Access Website:**  
   - `http://192.168.1.31` → Displays the website hosted from the user’s home directory.

---

### **Summary of Key Points:**  
- **Directory Indexing:** Serves default files like `index.php`.  
- **Directory Listing:** Shows folder contents if no index file exists.  
- **Control Access:** Use `.htaccess` or direct Apache config.  
- **Host from User Directories:** Enable `userdir.conf` and set correct permissions.


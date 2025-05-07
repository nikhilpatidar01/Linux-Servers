# Apache Web Server

Apache is an open-source web server software maintained by the **Apache Software Foundation**. It is known for reliability, flexibility, and a strong community.

## Check if Apache is Installed

* Check if Apache (`httpd`) is installed

  ```bash
  rpm -qa | grep httpd
  ```

## Install Apache Web Server

* Install Apache using `yum`

  ```bash
  yum install httpd
  ```

* Install all related Apache packages

  ```bash
  yum install httpd*
  ```

## Verify Apache Installation

* Show package information

  ```bash
  rpm -qi httpd
  ```

* List installed files

  ```bash
  rpm -ql httpd
  ```

* List configuration files

  ```bash
  rpm -qc httpd
  ```

* List documentation files

  ```bash
  rpm -qd httpd
  ```

## Apache Configuration

* View the main configuration file

  ```bash
  tail /etc/httpd/conf/httpd.conf
  ```

* Find `conf.modules.d` directive

  ```bash
  grep conf\.modules\.d /etc/httpd/conf/httpd.conf
  ```

* Find `conf.d` directive

  ```bash
  grep conf\.d /etc/httpd/conf/httpd.conf
  ```

## Manage Apache Service

* Check Apache service status

  ```bash
  systemctl status httpd.service
  ```

* Start Apache service

  ```bash
  systemctl start httpd.service
  ```

* Enable Apache on boot

  ```bash
  systemctl enable httpd.service
  ```

* Restart Apache service

  ```bash
  systemctl restart httpd.service
  ```

## Network and Port Verification

* List open ports and services

  ```bash
  netstat -nltup
  ```

* Check if port 80 is open

  ```bash
  netstat -nltup | grep 80
  ```

* Check if Apache is listening

  ```bash
  netstat -nttup | grep httpd
  ```

## Firewall Configuration

### Firewalld Basic Setup

* Check `firewalld` status

  ```bash
  systemctl status firewalld
  ```

* Start `firewalld`

  ```bash
  systemctl start firewalld
  ```

* Enable `firewalld` on boot

  ```bash
  systemctl enable firewalld
  ```

### List Current Firewall Rules

* List all active zones and rules

  ```bash
  firewall-cmd --list-all
  ```

* List active services

  ```bash
  firewall-cmd --list-services
  ```

* List open ports

  ```bash
  firewall-cmd --list-ports
  ```

### Allow Apache Through Firewall

* Allow HTTP service

  ```bash
  firewall-cmd --permanent --add-service=http
  ```

* Allow HTTPS service

  ```bash
  firewall-cmd --permanent --add-service=https
  ```

* Reload firewall settings

  ```bash
  firewall-cmd --reload
  ```

### Open Specific Ports (Optional)

* Open port 80 manually

  ```bash
  firewall-cmd --permanent --add-port=80/tcp
  ```

* Open port 443 manually

  ```bash
  firewall-cmd --permanent --add-port=443/tcp
  ```

* Open optional port 8080

  ```bash
  firewall-cmd --permanent --add-port=8080/tcp
  ```

* Reload firewall

  ```bash
  firewall-cmd --reload
  ```

### Remove Firewall Rules (Optional)

* Remove HTTP service

  ```bash
  firewall-cmd --permanent --remove-service=http
  ```

* Remove HTTPS service

  ```bash
  firewall-cmd --permanent --remove-service=https
  ```

* Remove port 80

  ```bash
  firewall-cmd --permanent --remove-port=80/tcp
  ```

* Remove port 443

  ```bash
  firewall-cmd --permanent --remove-port=443/tcp
  ```

* Reload firewall

  ```bash
  firewall-cmd --reload
  ```

### Configure Zones (Optional)

* List available zones

  ```bash
  firewall-cmd --get-zones
  ```

* Assign HTTP service to public zone

  ```bash
  firewall-cmd --zone=public --add-service=http --permanent
  ```

* Assign HTTPS service to public zone

  ```bash
  firewall-cmd --zone=public --add-service=https --permanent
  ```

* Reload firewall settings

  ```bash
  firewall-cmd --reload
  ```

### Enable Logging (Optional)

* Enable logging for dropped packets

  ```bash
  firewall-cmd --set-log-denied=all
  ```

* View firewall logs

  ```bash
  journalctl -f -u firewalld
  ```

## Apache Process and User Management

* Check Apache processes

  ```bash
  ps -aux | grep httpd
  ```

* Check Apache user

  ```bash
  cat /etc/passwd | grep apache
  ```

* Check Apache group

  ```bash
  cat /etc/group | grep apache
  ```

## Configure Website Files

* Navigate to the document root

  ```bash
  cd /var/www/html
  ```

* Create or edit `index.html`

  ```bash
  vim index.html
  ```

* Remove existing file

  ```bash
  rm -f index.html
  ```

* Set ownership to Apache user

  ```bash
  chown -Rv apache:apache /var/www/html/*
  ```

## Load CSS Files

* Download CSS template

  ```bash
  wget "https://www.free-css.com/assets/files/free-css-templates/download/page296/oxer.zip"
  ```

* Unzip the file

  ```bash
  unzip oxer.zip
  ```

* Remove ZIP file

  ```bash
  rm -rf oxer.zip
  ```

* Move files to web root

  ```bash
  mv -v oxer /var/www/html/
  ```

* Edit Apache config to support CSS

  ```bash
  vim /etc/httpd/conf/httpd.conf
  ```

* Add the following lines near line 311

  ```bash
  AddType text/html .shtml
  AddType text/css .css
  AddOutputFilter INCLUDES .shtml
  ```

* Restart Apache

  ```bash
  systemctl restart httpd.service
  ```

* Set ownership

  ```bash
  chown -Rv apache:apache /var/www/html/*
  ```

## Test Apache Setup

* Create a test file

  ```bash
  echo 'Test123' > /var/www/html/test.html
  ```

* Use `curl` to test Apache

  ```bash
  curl http://localhost
  ```

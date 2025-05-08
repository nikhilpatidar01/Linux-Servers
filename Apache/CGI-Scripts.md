CGI (Common Gateway Interface) is a standard for executing external programs on a web server to produce dynamic content. It supports multiple scripting languages including Perl, Python, and Shell.



### **Install Apache and Enable CGI Module (CentOS 9)**

* Install Apache:

  ```bash
  yum install httpd -y
  ```

* Enable and start the Apache service:

  ```bash
  systemctl enable --now httpd
  ```

* CGI module is typically included with Apache, but enable if needed:

  ```bash
  vi /etc/httpd/conf.modules.d/00-cgi.conf
  ```

  Make sure this line exists and is not commented:

  ```apache
  LoadModule cgi_module modules/mod_cgi.so
  ```



### **Install CGI Script Languages**

* Install Perl:

  ```bash
  yum install perl -y
  ```

* (Optional) Install Python:

  ```bash
  yum install python3 -y
  ```



### **Configure Apache for CGI**

* Edit Apache configuration or create a virtual host file:

  ```bash
  vi /etc/httpd/conf.d/cgi.conf
  ```

* Example configuration:

  ```apache
  ScriptAlias /cgi-bin/ /var/www/cgi-bin/

  <Directory "/var/www/cgi-bin">
      AllowOverride None
      Options +ExecCGI
      AddHandler cgi-script .cgi .pl .py .sh
      Require all granted
  </Directory>
  ```

* Create the CGI directory if it doesn't exist:

  ```bash
  mkdir -p /var/www/cgi-bin
  ```

* Restart Apache to apply changes:

  ```bash
  systemctl restart httpd
  ```



### **Create and Test CGI Script**

* Create a Perl CGI script:

  ```bash
  vi /var/www/cgi-bin/test.cgi
  ```

  Paste this content:

  ```perl
  #!/usr/bin/perl
  print "Content-type: text/html\n\n";
  print "<h1>Server Memory Usage</h1>\n";
  print "<pre>\n";
  system("free -h");
  print "</pre>\n";
  ```

* Make the script executable:

  ```bash
  chmod 755 /var/www/cgi-bin/test.cgi
  ```

* Access in browser:

  ```
  http://<your-server-ip>/cgi-bin/test.cgi
  ```



### **Secure the `/cgi-bin` Directory with HTTP Authentication**

* Install `httpd-tools` if not already installed:

  ```bash
  yum install httpd-tools -y
  ```

* Create password file:

  ```bash
  htpasswd -c /etc/httpd/.htpasswd youruser
  ```

* Edit `cgi.conf` to add authentication:

  ```apache
  <Directory "/var/www/cgi-bin">
      AuthType Basic
      AuthName "Restricted CGI"
      AuthUserFile /etc/httpd/.htpasswd
      Require valid-user
  </Directory>
  ```

* Restart Apache:

  ```bash
  systemctl restart httpd
  ```

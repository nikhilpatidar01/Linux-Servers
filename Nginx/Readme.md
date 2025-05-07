# Nginx

Nginx is a high-performance web server and reverse proxy server designed to handle large volumes of concurrent connections efficiently. It is commonly used for serving static content, load balancing, proxying HTTP and HTTPS requests, and acting as a gateway for APIs and microservices.

## Installing Nginx

- Enable the **EPEL** repository and install Nginx:

    ```bash
    yum install epel-release -y
    yum install nginx -y
    ```

### Start and Enable Nginx

- Check the service status:

    ```bash
    systemctl status nginx
    ```

- If Nginx is not active, start and enable it:

    ```bash
    systemctl start nginx
    systemctl enable nginx
    ```



## Configuring the Firewall

Allow HTTP and HTTPS traffic through **firewalld**:

```bash
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```

Verify the rules:

```bash
firewall-cmd --list-all
```



## Nginx Server Configuration
You can either modify the main configuration file or create a new virtual host file.

### Option A: Edit the Main Configuration File

```bash
vi /etc/nginx/nginx.conf
```

### Option B: Create a Custom Server Block

```bash
vi /etc/nginx/conf.d/default.conf
```

#### Sample Server Block

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /usr/share/nginx/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

> Replace `yourdomain.com` with your actual domain or server IP.



## Validate and Apply Configuration

Test the Nginx configuration for syntax errors:

```bash
nginx -t
```

If the test passes, reload Nginx to apply changes:

```bash
systemctl reload nginx
```



## Deploying Your Website

Place your web files under:

```
/usr/share/nginx/html/
```

### Example: Create a Test Page

```bash
vi /usr/share/nginx/html/index.html
```

Paste the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Website</title>
</head>
<body>
    <h1>Success! Nginx is working!</h1>
</body>
</html>
```

Save and exit the editor.



## Directory

```
/etc/nginx/nginx.conf              # Main configuration file
/etc/nginx/conf.d/default.conf     # Site-specific configuration
/usr/share/nginx/html/index.html  # Web root directory
```

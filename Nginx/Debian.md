## Installing Nginx

Update package index and install Nginx:

```bash
apt update
apt install nginx -y
```



### Start and Enable Nginx

- Check Nginx service status:

```bash
systemctl status nginx
```

If it's not running, start and enable it:

```bash
systemctl start nginx
systemctl enable nginx
```


## Nginx Server Configuration

- You can edit the main configuration file or create a virtual host file.

### Option A: Edit the Main Configuration File

```bash
nano /etc/nginx/nginx.conf
```

### Option B: Create a Custom Server Block

```bash
nano /etc/nginx/sites-available/yourdomain.com
```

#### Sample Server Block

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable the site:

```bash
ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
```

> Replace `yourdomain.com` with your actual domain or server IP.



## Validate and Reload Nginx

Check for syntax errors:

```bash
nginx -t
```

Reload Nginx to apply changes:

```bash
systemctl reload nginx
```



## Deploying Your Website

Place your website files in:

```
/var/www/html/
```

### Example: Create a Test Page

```bash
nano /var/www/html/index.html
```

Paste:

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

Save and exit.



## Directory Overview

```
/etc/nginx/nginx.conf                  # Main Nginx config
/etc/nginx/sites-available/           # Available server blocks
/etc/nginx/sites-enabled/             # Enabled server blocks (symlinks)
/var/www/html/                        # Default web root
```
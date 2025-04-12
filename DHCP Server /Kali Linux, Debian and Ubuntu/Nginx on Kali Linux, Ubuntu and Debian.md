
# How to Configure Nginx on Ubuntu/Kali Linux/Debian

This guide explains how to install, configure, and secure an Nginx web server on Ubuntu.

---

## 1. Update System Packages

Before installing anything, make sure your system packages are up to date:

```bash
 apt update
 apt upgrade -y
```

---

## 2. Install Nginx

Install the Nginx web server:

```bash
 apt install nginx -y
```

### Check Nginx Status:

```bash
 systemctl status nginx
```

If it’s not running, start and enable it:

```bash
 systemctl start nginx
 systemctl enable nginx
```

---

## 3. Configure UFW Firewall

Allow Nginx HTTP and HTTPS traffic through the firewall:

```bash
 ufw allow 'Nginx Full'
```

### Check Firewall Status:

```bash
 ufw status
```

---

## 4. Basic Nginx Configuration

Edit the default Nginx site configuration file:

```bash
 nano /etc/nginx/sites-available/default
```

### Sample Configuration:

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

Save and exit: Press `CTRL + X`, then `Y`, then `ENTER`

---

## 5. Test and Reload Nginx

### Test Configuration:

```bash
 nginx -t
```

### Reload Nginx:

```bash
 systemctl reload nginx
```

---

## 6. Place Your Website Files

Upload your website content into:

```
/var/www/html/
```

### Example: Create a Sample `index.html`

```bash
 nano /var/www/html/index.html
```

Paste this HTML:

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

---

## 7. Optional: Enable HTTPS with Let’s Encrypt (Certbot)

Install Certbot and the Nginx plugin:

```bash
 apt install certbot python3-certbot-nginx -y
```

### Get and Install SSL Certificate:

```bash
 certbot --nginx
```

Follow the on-screen prompts to configure HTTPS.

---

## Done

Your website should now be live at:

```
http://yourdomain.com
```

or (if SSL is configured):

```
https://yourdomain.com
```

---

## Example Folder Structure

```
/etc/nginx/nginx.conf
/etc/nginx/sites-available/default
/var/www/html/index.html
```

---

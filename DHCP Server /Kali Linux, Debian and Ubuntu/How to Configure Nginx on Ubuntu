How to Configure Nginx on Ubuntu
This guide explains how to install and configure Nginx on Ubuntu.

1. Update System Packages
sudo apt update
sudo apt upgrade -y

sudo apt update
sudo apt upgrade -y
2. Install Nginx

sudo apt install nginx -y
Check if Nginx is running:

sudo systemctl status nginx
If it's inactive, start it:

sudo systemctl start nginx
Enable it to start on boot:

sudo systemctl enable nginx
3. Configure Firewall
Allow HTTP (port 80) and HTTPS (port 443) traffic:

sudo ufw allow 'Nginx Full'
Check firewall status:

sudo ufw status
4. Basic Nginx Configuration
Edit the default Nginx site:

sudo nano /etc/nginx/sites-available/default
Example configuration:

nginx

server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
Save and exit (CTRL + X, then Y, then ENTER).

5. Test and Restart Nginx
Check if configuration is correct:


sudo nginx -t
Reload Nginx:


sudo systemctl reload nginx
6. Place Your Website Files
Upload your website files (HTML, CSS, JS) into:


/var/www/html/
Example:


sudo nano /var/www/html/index.html
Write something like:


<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Website</title>
</head>
<body>
    <h1>Success! Nginx is working!</h1>
</body>
</html>
Save and exit.

7. Optional: Setting Up SSL with Let's Encrypt (Free HTTPS)
Install Certbot:

sudo apt install certbot python3-certbot-nginx -y
Get an SSL certificate:


sudo certbot --nginx
Follow the prompts to secure your site.

✅ Done! Now your website should be live.

Folder Structure Example
swift
/nginx/nginx.conf
/etc/nginx/sites-available/default
/var/www/html/index.html

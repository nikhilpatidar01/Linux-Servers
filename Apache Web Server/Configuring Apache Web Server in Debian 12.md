# Configuring Apache Web Server in Debian 12

To install and configure the Apache web server on **Debian 12**, follow these steps:

## Step 1: Update System Packages

Open a terminal and update your package list to ensure you're installing the latest available version.

```bash
sudo apt update && sudo apt upgrade -y
```

## Step 2: Install Apache

Install the Apache web server package using:

```bash
sudo apt install apache2 -y
```

## Step 3: Verify Installation

Once installed, check the status of Apache to confirm it is running:

```bash
sudo systemctl status apache2
```

## Step 4: Configure the Firewall

Manually allow ports **80 (HTTP)** and **443 (HTTPS)**:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

If you are running Apache on a **custom port (e.g., 8080)**, allow it too:

```bash
sudo ufw allow 8080/tcp
```

Reload UFW to apply changes:

```bash
sudo ufw reload
```

Check the active firewall rules:

```bash
sudo ufw status
```

If it shows **Apache** or ports **80, 443** are allowed, then your firewall is correctly configured.

## Step 5: Test Apache Web Server

Open a web browser and visit:

```
http://192.168.118.97  # Replace with your server's IP
```

## Step 6: Create a Website Directory

Let's say your website will be called **"html"**. Create a directory for it inside `/var/www/`:

```bash
sudo mkdir -p /var/www/html
```

Set proper permissions:

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

## Step 7: Create an Index Page

Create a simple HTML file for testing:

```bash
sudo nano /var/www/html/index.html
```

Add the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Website</title>
</head>
<body>
    <h1>Success! Your website is working.</h1>
</body>
</html>
```

Save and exit (`CTRL + X`, then `Y`, then `Enter`).

## Step 8: Enable the Site

Ensure the default Apache configuration is enabled:

```bash
sudo a2ensite 000-default.conf
```

Disable the default Apache site (optional):

```bash
sudo a2dissite 000-default.conf
```

Reload Apache to apply the changes:

```bash
sudo systemctl reload apache2
```

## Step 9: Test the Website

If your domain is properly configured with DNS, visit:

```
http://192.168.61.97  # Replace with your current server IP
```

or find your server's IP with:

```bash
ip a
```

Then open in a browser:

```
http://192.168.61.97
```

---

✅ **Done!** Your HTML template is now hosted on Apache in Debian 12. 🚀


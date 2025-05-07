


# User Authentication in Squid

**User Authentication** ensures only authorized users can access the internet through your Squid proxy. This is especially useful in shared or enterprise environments to improve security and accountability.


## Supported Authentication Methods

1. **Basic (NCSA)** – Uses a simple username/password file.
2. **Digest** – Hashes credentials (more secure).
3. **NTLM / LDAP / Kerberos** – For integration with Active Directory.



## Basic Authentication with NCSA

- Install Required Tools

  ```bash
  yum install httpd-tools -y
  ```

- Create the password file and add the first user:

  ```bash
  htpasswd -c /etc/squid/passwd ul
  ```

- Add more users (without `-c` to avoid overwriting):

  ```bash
  htpasswd /etc/squid/passwd u2
  ```

- View the file contents:

  ```bash
  cat /etc/squid/passwd
  ```

- Set permissions:

  ```bash
  chgrp squid /etc/squid/passwd
  chmod 640 /etc/squid/passwd
  ```

- Configure Squid for Authentication

  ```bash
  vim /etc/squid/squid.conf
  ```

  Add this section (usually near the custom rule section):

  ```apache
  auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwd
  auth_param basic realm Squid Proxy Authentication
  acl ncsa_users proxy_auth REQUIRED
  http_access allow ncsa_users
  ```

  > 🧠 *Ensure `http_access allow ncsa_users` comes before `http_access deny all`.*

  **Restrict to specific users (optional):**

  ```apache
  acl allowed_users proxy_auth u1 u2
  http_access allow allowed_users
  ```



- Restart Squid

  ```bash
  systemctl restart squid
  ```
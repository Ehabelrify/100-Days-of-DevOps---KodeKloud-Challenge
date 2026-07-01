# Day 20 � Nginx + PHP-FPM Unix Socket

**Date:** 2026-07-01 | **Duration:** ~0.5h | **Status:** Completed

---

## ?? Objective

Install and configure Nginx and PHP-FPM on App Server 3 so PHP pages are served from `/var/www/html` over port `8098`, using a Unix socket at `/var/run/php-fpm/default.sock`.

---

## ?? Problem Statement

The Nautilus application team needs a PHP application served by Nginx on App Server 3. Nginx must listen on port `8098`, use `/var/www/html` as the document root, and send PHP requests to PHP-FPM via the Unix socket `/var/run/php-fpm/default.sock`.

### Requirements
- Install `nginx` on App Server 3
- Install `php-fpm` version `8.3`
- Configure `nginx` to listen on port `8098`
- Configure `php-fpm` to use the Unix socket `/var/run/php-fpm/default.sock`
- Connect Nginx and PHP-FPM correctly
- Verify the setup with `curl http://stapp03:8098/index.php`

---

## ?? Prerequisites

- SSH access to App Server 3
- Sudo privileges on App Server 3
- The PHP application files already present under `/var/www/html`

---

## ?? Step-by-Step Instructions

### Step 1: Connect to App Server 3

Use SSH to log in to the target machine:

```bash
ssh <your-user>@stapp03
```

If `stapp03` does not resolve, use the IP address provided by your environment.

---

### Step 2: Install Nginx and PHP-FPM 8.3

On RHEL/CentOS/AlmaLinux:

```bash
sudo dnf module reset php -y
sudo dnf module enable php:8.3 -y
sudo dnf install -y nginx php-fpm
```

On Debian/Ubuntu:

```bash
sudo apt update
sudo apt install -y nginx php8.3-fpm
```

Confirm the PHP-FPM version:

```bash
php-fpm -v
```

---

### Step 3: Prepare the Document Root

Ensure the site directory exists:

```bash
sudo mkdir -p /var/www/html
sudo ls -l /var/www/html
```

Do not modify `index.php` or `info.php` if they are already present.

---

### Step 4: Configure PHP-FPM to Use a Unix Socket

Locate the PHP-FPM pool configuration file:

```bash
sudo find /etc -name www.conf | grep php
```

Common locations:
- `/etc/php-fpm.d/www.conf`
- `/etc/php/8.3/fpm/pool.d/www.conf`

Edit the correct file and set these values:

```ini
listen = /var/run/php-fpm/default.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

Create the socket directory and ensure ownership:

```bash
sudo mkdir -p /var/run/php-fpm
sudo chown -R nginx:nginx /var/run/php-fpm
```

---

### Step 5: Configure Nginx

Open or create the Nginx site configuration file:

```bash
sudo vi /etc/nginx/conf.d/default.conf
```

Use this server block:

```nginx
server {
    listen 8098;
    server_name stapp03;
    root /var/www/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

Save the file and exit the editor.

---

### Step 6: Test the Configuration

Test Nginx configuration:

```bash
sudo nginx -t
```

Test PHP-FPM configuration if supported:

```bash
sudo php-fpm -t
```

or on Debian/Ubuntu:

```bash
sudo php8.3-fpm -t
```

Fix any errors before continuing.

---

### Step 7: Start and Enable Services

Enable and start the services:

```bash
sudo systemctl enable --now php-fpm nginx
sudo systemctl restart php-fpm nginx
```

Check service status:

```bash
sudo systemctl status nginx
sudo systemctl status php-fpm
```

---

### Step 8: Verify the Socket and Listener

Confirm the socket exists:

```bash
sudo ls -l /var/run/php-fpm/default.sock
```

Confirm Nginx is listening on port 8098:

```bash
sudo ss -tulpn | grep 8098
```

---

### Step 9: Verify the Application from the Jump Host

From the jump host, run:

```bash
curl http://stapp03:8098/index.php
```

If configured correctly, the PHP page should return HTML content.

---

## ? Completion Checklist

- [x] `nginx` installed on App Server 3
- [x] `php-fpm` 8.3 installed on App Server 3
- [x] Nginx configured to listen on port `8098`
- [x] PHP-FPM configured to use `/var/run/php-fpm/default.sock`
- [x] Nginx and PHP-FPM connected through the Unix socket
- [x] `curl http://stapp03:8098/index.php` returns the expected page

---

## ?? Key Takeaways

- Nginx can forward PHP requests to PHP-FPM using Unix sockets.
- PHP-FPM socket directories must exist and have the correct ownership.
- `nginx -t` and `php-fpm -t` are essential for validating configuration before starting services.
- Using port `8098` avoids conflicts with default HTTP port `80`.

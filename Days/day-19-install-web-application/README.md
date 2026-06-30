# Day 19 — Install Web Application

**Date:** 2026-06-30 | **Duration:** ~0.5h | **Status:** In Progress

---

## 🎯 Objective

Set up Apache on App Server 1 in Stratos Datacenter to host two static websites from backup folders and make them available at:

- http://localhost:5000/blog/
- http://localhost:5000/demo/

---

## 📋 Problem Statement

xFusionCorp Industries wants two static websites hosted on the application server. The website content is available in backup folders on the jump host, and Apache must be configured to serve them on port 5000.

### Requirements
- Install the Apache HTTP server package on App Server 1
- Configure Apache to listen on port 5000
- Deploy the website content from the backup folders
- Make both websites accessible using the required URLs
- Verify the setup with curl

---

## 📚 Prerequisites

- SSH access to the app server
- Sudo privileges on the app server
- Access to the backup folders on the jump host

---

## 💻 Step-by-Step Instructions

### Step 1: Connect to the App Server

Use SSH to log in to the target application server:

```bash
ssh tony@stapp01
```

---

### Step 2: Install Apache

Install the Apache package:

```bash
sudo yum install httpd -y
```

---

### Step 3: Create Directories for the Websites

Create the destination folders for the two websites:

```bash
sudo mkdir -p /var/www/html/blog /var/www/html/demo
```

---

### Step 4: Copy Website Content from the Jump Host

From the jump host, copy the backup contents to the app server.

```bash
scp -r /home/thor/blog/* tony@stapp01:/var/www/html/blog/
scp -r /home/thor/demo/* tony@stapp01:/var/www/html/demo/
```

If the copy fails due to permissions, fix the target folder ownership first:

```bash
sudo chown -R tony:tony /var/www/html/blog /var/www/html/demo
sudo chmod -R 755 /var/www/html/blog /var/www/html/demo
```

---

### Step 5: Configure Apache to Listen on Port 5000

Create a new Apache configuration file:

```bash
sudo vi /etc/httpd/conf.d/websites.conf
```

Add the following content:

```apache
Listen 5000

Alias /blog/ /var/www/html/blog/
Alias /demo/ /var/www/html/demo/

<Directory "/var/www/html/blog">
    Require all granted
</Directory>

<Directory "/var/www/html/demo">
    Require all granted
</Directory>
```

Save and exit the file.

---

### Step 6: Start and Enable Apache

Start the Apache service and enable it on boot:

```bash
sudo systemctl enable httpd
sudo systemctl start httpd
```

Verify the service status:

```bash
sudo systemctl status httpd
```

---

### Step 7: Verify the Websites

Test the two website endpoints locally on the app server:

```bash
curl http://localhost:5000/blog/
curl http://localhost:5000/demo/
```

You should see the HTML content from the respective website folders.

---

## ✅ Completion Checklist

- [ ] Apache installed on App Server 1
- [ ] Apache configured to listen on port 5000
- [ ] Website content copied to the correct directories
- [ ] Blog website available at http://localhost:5000/blog/
- [ ] Demo website available at http://localhost:5000/demo/
- [ ] curl verification succeeds for both URLs

---

## 🎓 Key Takeaways

- Apache can serve static content from custom directories using aliases
- Port 5000 can be enabled by adding a Listen directive in Apache configuration
- Proper ownership and permissions are required for secure file transfers
- curl is a quick way to verify that web content is being served correctly

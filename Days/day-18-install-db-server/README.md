# Day 18 — Install DB Server

**Date:** 2026-06-23 | **Duration:** ~0.5h | **Status:**  Completed

---

## 🎯 Objective

Set up a MariaDB database server on Nautilus DB Server in Stratos Datacenter with a pre-configured database and user account with appropriate permissions.

---

## 📋 Problem Statement

The Nautilus infrastructure team requires a database server to be installed and configured with specific database and user credentials. The database will be used for application deployments.

Key requirements:
- Install and configure MariaDB server
- Create a database named `kodekloud_db7`
- Create a user `kodekloud_top` with password `8FmzjvFU6S`
- Grant full permissions to the user on the database
- Apply the changes on Nautilus DB Server in Stratos Datacenter

---

## 📚 Prerequisites

- SSH access to Nautilus DB Server with root or sudo privileges
- Basic knowledge of MariaDB/MySQL database administration
- Understanding of user permissions and database grants

---

## 💻 Step-by-Step Solution

### Step 1: SSH to Nautilus DB Server

**Description:**
Connect to the Nautilus DB Server using the challenge-provided SSH credentials.

**Commands:**
```bash
ssh <username>@<db-server-ip>
```

**Expected Output:**
```
# login prompt or direct shell on the database server
```

---

### Step 2: Update Package Manager and Install MariaDB Server

**Description:**
Update the system package manager and install MariaDB server package.

**Commands:**
```bash
sudo yum update -y
sudo yum install mariadb-server -y
```

**Expected Output:**
```
Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
Setting up Install Process
mariadb-server.x86_64 1:5.5.x-xx.el6 will be installed
```

---

### Step 3: Start and Enable MariaDB Service

**Description:**
Start the MariaDB service and enable it to automatically start on system boot.

**Commands:**
```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

**Verify Status:**
```bash
sudo systemctl status mariadb
```

**Expected Output:**
```
mariadb.service - MariaDB database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled)
   Active: active (running) since ... 
```

---

### Step 4: Create the Database

**Description:**
Connect to MariaDB as root and create the `kodekloud_db7` database.

**Commands:**
```bash
sudo mysql -u root
```

**Inside MySQL Prompt:**
```sql
CREATE DATABASE kodekloud_db7;
SHOW DATABASES;
```

**Expected Output:**
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| kodekloud_db7      |
| mysql              |
| performance_schema |
+--------------------+
```

---

### Step 5: Create the Database User

**Description:**
Create a new database user `kodekloud_top` with password `8FmzjvFU6S`.

**Inside MySQL Prompt:**
```sql
CREATE USER 'kodekloud_top'@'localhost' IDENTIFIED BY '8FmzjvFU6S';
```

**Expected Output:**
```
Query OK, 0 rows affected (0.00 sec)
```

---

### Step 6: Grant Full Permissions to the User

**Description:**
Grant all privileges on the `kodekloud_db7` database to the `kodekloud_top` user and apply the changes.

**Inside MySQL Prompt:**
```sql
GRANT ALL PRIVILEGES ON kodekloud_db7.* TO 'kodekloud_top'@'localhost';
FLUSH PRIVILEGES;
```

**Expected Output:**
```
Query OK, 0 rows affected (0.00 sec)
```

---

### Step 7: Verify the Setup

**Description:**
Verify that the user was created correctly and has the appropriate permissions.

**Inside MySQL Prompt:**
```sql
-- Check all users
SELECT User, Host FROM mysql.user;

-- Check grants for the new user
SHOW GRANTS FOR 'kodekloud_top'@'localhost';
```

**Expected Output:**
```
+-------+------+------+-----+---------+-------+
| User  | Host |
+-------+------+------+-----+---------+-------+
| root  | localhost |
| kodekloud_top | localhost |
+-------+------+------+-----+---------+-------+

GRANT ALL PRIVILEGES ON `kodekloud_db7`.* TO 'kodekloud_top'@'localhost'
```

---

### Step 8: Exit MySQL and Verify User Login

**Description:**
Exit the MySQL prompt and verify that the new user can log in and access the database.

**Commands:**
```bash
# Inside MySQL prompt
EXIT;

# Test login with the new user
mysql -u kodekloud_top -p kodekloud_db7
# Enter password: 8FmzjvFU6S

# If successful, verify database access:
SHOW DATABASES;

# Exit
EXIT;
```

**Expected Output:**
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| kodekloud_db7      |
+--------------------+
```

---

## ✅ Completion Checklist

- [ ] MariaDB server installed and running
- [ ] MariaDB service enabled to start on boot
- [ ] Database `kodekloud_db7` created
- [ ] User `kodekloud_top` created with correct password
- [ ] Full permissions granted to user on the database
- [ ] User can successfully log in and access the database

---

## 🎓 Key Takeaways

- **MariaDB Installation**: Essential database server setup for Linux environments
- **User Management**: Creating database users with specific permissions for application access
- **Security**: Using strong passwords and limiting user permissions to specific databases
- **Service Management**: Enabling services to start automatically on system boot
- **Database Administration**: Basic SQL commands for database and user creation


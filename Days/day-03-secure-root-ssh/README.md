# Day 3 — Secure Root SSH

**Date:** 2026-05-27 | **Duration:** ~0.2h | **Status:** ✅ Complete

---

## 🎯 Objective

Disable direct SSH root login on all application servers within the Stratos Datacenter as per security audit requirements by xFusionCorp Industries security team.

---

## 📋 Problem Statement

Following security audits, xFusionCorp Industries security team has implemented new protocols that restrict direct root SSH login. Your task is to:
- Disable direct SSH root login on all application servers (App Server 1, 2, and 3)
- Modify SSH configuration to prevent root user from logging in directly
- Verify that root SSH access is properly restricted across all servers

---

## 📚 Prerequisites

- Access to all application servers (App Server 1, 2, 3)
- User account with sudo privileges on each server
- SSH client configured for server access
- Basic knowledge of SSH configuration and systemctl

---

## 💻 Step-by-Step Solution

### Step 1: Login to Application Server

**Description:**
Connect to the first application server using SSH with a standard user account, then escalate to root privileges.

**Commands:**
```bash
ssh <user>@<hostname>
sudo su
```

**Expected Output:**
```
Connection established
[user@hostname ~]$ sudo su
[root@hostname ~]#
```

---

### Step 2: Edit SSH Configuration File

**Description:**
Open the SSH daemon configuration file using vi editor to modify the PermitRootLogin setting.

**Commands:**
```bash
vi /etc/ssh/sshd_config
```

**Configuration Change:**
Search for `PermitRootLogin` and change it from `yes` to `no`.

**Expected Output:**
```
PermitRootLogin no
```
![Screenshot](./image.png)

---

### Step 3: Restart SSH Service

**Description:**
Restart the SSH service to apply the new configuration changes.

**Commands:**
```bash
systemctl restart sshd
```

**Expected Output:**
```
[root@hostname ~]# systemctl restart sshd
[root@hostname ~]#
```

---

### Step 4: Verify Root SSH Access Restriction

**Description:**
From a new terminal, attempt to login directly as root to verify that access is denied.

**Commands:**
```bash
ssh root@<hostname>
```

**Expected Output:**
```
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

**Screenshot:**
![Step 4 Verification](./image-1.png)

---

### Step 5: Repeat for Remaining Application Servers

**Description:**
Perform Steps 1-4 on Application Server 2 and Application Server 3 to ensure all servers have root SSH access disabled.

---

## ✅ Verification

**How to verify the solution is correct:**
- [ ] Root SSH login is denied on App Server 1
- [ ] Root SSH login is denied on App Server 2
- [ ] Root SSH login is denied on App Server 3
- [ ] SSH service restarted successfully on all servers
- [ ] PermitRootLogin is set to `no` in `/etc/ssh/sshd_config`

**Verification Command:**
```bash
ssh root@<hostname>  # Should return "Permission denied"
```

---

## 📊 Results & Evidence

| Item | Details |
|------|---------|
| **Status** | ✅ Success |
| **Time Taken** | ~0.2h |
| **Servers Modified** | App Server 1, 2, 3 |
| **Configuration Changed** | PermitRootLogin: yes → no |
| **Service Restarted** | sshd (all 3 servers) |
| **Verification** | Root SSH access denied on all servers |

---

# Day 02 — Temporary User Setup with Expiry

**Date:** 2026-05-26 | **Duration:** ~0.2h | **Status:** ✅ Complete

---

## 🎯 Objective

Create a temporary user account named `mark` on App Server 3 with an expiry date of 2027-02-17 for a limited-duration project assignment.

---

## 📋 Problem Statement

The Nautilus project requires temporary access for a developer named Mark. The system administrator team needs to create a time-limited user account that automatically expires after the project duration.

Key requirements:
- Create a user named `mark` (lowercase)
- Set the expiry date to 2027-02-17
- Apply the change on App Server 3 (`stapp03`)
- Verify the account expiry is correctly configured

---

## 📚 Prerequisites

- Access to App Server 3 (`stapp03`) with sufficient privileges to create users
- SSH access to `stapp03`
- Ability to escalate to root with `sudo su`
- Understanding of Linux user account expiry management

---

## 💻 Step-by-Step Solution

### Step 1: SSH to App Server 3 (stapp03)

**Description:**
Connect to App Server 3 using the challenge-provided SSH credentials.

**Commands:**
```bash
ssh <username>@stapp03
```

**Expected Output:**
```
# login prompt or direct shell on stapp03
```

---

### Step 2: Escalate to root privileges

**Description:**
Use `sudo su` to switch to the root user before creating the user account.

**Commands:**
```bash
sudo su
```

**Expected Output:**
```
# root prompt, e.g. root@stapp03:/#
```

**Screenshot:**
![Root Access after SSH](./image.png)

---

### Step 3: Create the temporary user with expiry date

**Description:**
Use the `adduser` command with the `-e` flag to create the user `mark` with an expiry date of 2027-02-17. This ensures the account will be automatically disabled after the specified date.

**Commands:**
```bash
adduser -e 2027-02-17 mark
```


**Screenshot:**
![Create User with Expiry](./image-2.png)

---

### Step 4: Verify the account expiry configuration

**Description:**
Use the `chage` command to display the account expiry details and confirm the expiry date is correctly set.

**Commands:**
```bash
chage -l mark
```

**Expected Output:**
```
Last password change                   : May 26, 2026
Password expires                       : never
Password inactive                      : never
Account expires                        : Feb 17, 2027
Minimum number of days between changes : 0
Maximum number of days between changes : 99999
Number of days of warning before expire: 7
```

**Screenshot:**
![Verify Account Expiry](./image-1.png)

---

## ✅ Verification

**How to verify the solution is correct:**
- [ ] User `mark` is created on App Server 3
- [ ] Account expiry date is set to February 17, 2027
- [ ] `chage -l mark` output shows `Account expires : Feb 17, 2027`
- [ ] User can log in before the expiry date
- [ ] User account will be locked after the expiry date

**Verification Command:**
```bash
chage -l mark
id mark
```

**Final Screenshot (Evidence of Completion):**
![Completion Verification](./image-1.png)

---

## 📊 Results & Evidence

| Item | Details |
|------|---------|
| **Status** | ✅ Success |
| **Time Taken** | ~0.2h |
| **User Created** | mark |
| **Expiry Date** | 2027-02-17 |
| **Server** | App Server 3 (stapp03) |
| **Verification Method** | chage -l mark |

---

## 🎓 Key Learnings

- **User Expiry Management:** The `-e` flag in `adduser` allows setting an expiry date in YYYY-MM-DD format during user creation
- **chage Command:** Used to change user password expiry information and view account details
- **Temporary Access:** Useful for contractors, interns, and project-based team members
- **Account Locking:** After expiry, the account is locked and the user cannot log in even with the correct password

---

## 🔗 Related Commands

```bash
# View account expiry details
chage -l <username>

# Set expiry date for existing user
chage -E 2027-02-17 <username>

# Create user with expiry date
adduser -e YYYY-MM-DD <username>

# Check user information
id <username>
```

---

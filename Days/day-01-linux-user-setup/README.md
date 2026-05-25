# Day 01 — Linux User Setup

**Date:** 2026-05-25 | **Duration:** ~0.5h | **Status:** ✅ Complete

---

## 🎯 Objective

Create a new Linux user named `yousuf` with a non-interactive shell on App Server 3 to satisfy the backup agent tool requirement.

---

## 📋 Problem Statement

The system administrator team at xFusionCorp Industries needs a user account for the backup agent tool. The account must be created with a non-interactive shell so it cannot be used for normal logins.

Key requirements:
- Create a user named `yousuf`
- Assign a non-interactive shell
- Apply the change on App Server 3

---

## 📚 Prerequisites

- Access to App Server 3 with sufficient privileges to create users
- Familiarity with Linux user management commands
- Knowledge of non-interactive shells such as `/sbin/nologin` or `/usr/sbin/nologin`

---

## 💻 Step-by-Step Solution

### Step 1: Create the user with a non-interactive shell

**Description:**
Create a local Linux user named `yousuf` and set the shell to `/sbin/nologin` so the account cannot be used for interactive login sessions.

**Commands:**
```bash
sudo useradd -s /sbin/nologin yousuf
```

**Expected Output:**
```
# no output when the command succeeds
```

**Screenshot:**
![Step 1 Screenshot](./image.png)

---

### Step 2: Verify the user's shell configuration

**Description:**
Confirm that the `yousuf` user exists and that the shell is configured as non-interactive.

**Commands:**
```bash
getent passwd yousuf
```

**Expected Output:**
```
yousuf:x:1001:1001::/home/yousuf:/sbin/nologin
```

**Screenshot:**
![Step 2 Screenshot](./image-1.png)

---

## ✅ Verification

**How to verify the solution is correct:**
- [ ] Confirm the `yousuf` user exists in `/etc/passwd`
- [ ] Confirm the shell is set to `/sbin/nologin`
- [ ] Confirm the user cannot obtain an interactive shell session

**Verification Command:**
```bash
getent passwd yousuf
```

**Final Screenshot (Evidence of Completion):**
![Completion Screenshot](./image-2.png)

---

## 📊 Results & Evidence

| Item | Details |
|------|---------|
| **Status** | ✅ Success |
| **Time Taken** | ~0.5h |
| **Key Output** | `yousuf` created with `/sbin/nologin` shell |
| **Challenges** | Minimal; task was straightforward once the required shell was identified |

---

## 🔑 Key Concepts Learned

1. **Linux user creation** — How to create a new account with `useradd`
2. **Non-interactive shell configuration** — Using `/sbin/nologin` to prevent interactive logins
3. **User verification** — Checking user details with `getent passwd`

---

## ⚠️ Common Mistakes & Troubleshooting

### Issue 1: Wrong shell path
**Cause:** Using an interactive shell like `/bin/bash` instead of a non-interactive shell
**Solution:** Set the shell to `/sbin/nologin` or `/usr/sbin/nologin`

```bash
sudo usermod -s /sbin/nologin yousuf
```

### Issue 2: User already exists
**Cause:** The account `yousuf` already exists on the system
**Solution:** Check the existing account and update the shell if necessary

```bash
getent passwd yousuf
sudo usermod -s /sbin/nologin yousuf
```

---

## 📌 Key Takeaways

- **What worked well:** Creating the user with `useradd` and verifying the shell using `getent passwd`
- **What was challenging:** Identifying the correct non-interactive shell path for the environment
- **Next steps:** Practice creating service accounts and enforcing non-interactive shells for automation tasks

---

## 🔗 Resources & References

- [Linux `useradd` man page](https://man7.org/linux/man-pages/man8/useradd.8.html)
- [Linux `nologin` shell documentation](https://man7.org/linux/man-pages/man8/nologin.8.html)
- [Kubernetes service account concepts](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

---

## 📁 Files & Configurations

### Files Created/Modified:
- `README.md` — Day 1 challenge documentation and solution summary

### Useful Scripts/Configs:
```bash
sudo useradd -s /sbin/nologin yousuf
getent passwd yousuf
```

---

## 🏷️ Tags

`linux` `user-management` `security` `backup-agent` `shell`

---

## 📝 Notes

- The `yousuf` account is configured for non-interactive use only.
- If App Server 3 uses `/usr/sbin/nologin`, adjust the shell path accordingly.
- Use `sudo` when running user management commands if you are not root.

---

**Challenge Completed:** ✅ YES
**Difficulty Level:** ⭐⭐ (1-5 stars)
**Time Investment:** Worth it ✅

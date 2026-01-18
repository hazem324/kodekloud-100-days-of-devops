# 🔐 Linux SSH Automation – Password-less Access (Day 007)

## 📌 Objective

The system admins team of **xFusionCorp Industries** uses automation scripts on a **jump host** to run regular tasks across multiple **application servers** in the **Stratos Datacenter**.

To ensure these scripts run **without manual intervention**, the jump host user must authenticate to all application servers using **SSH key-based authentication** instead of passwords.

---

## 🎯 Goal of the Task

* Configure **password-less SSH authentication**
* Enable secure, automated access from the jump host
* Use **sudo users** on each application server
* Support automation tools, cron jobs, and scripts

---

## 🛠️ Steps to Complete the Task

### 1️⃣ Generate an SSH Key Pair on the Jump Host

Run the following command on the jump host as the automation user:

```bash
ssh-keygen -t ed25519
```

* Accept the default key location
* Leave the passphrase empty for automation use

This generates:

* A **private key** (kept secure on the jump host)
* A **public key** (shared with app servers)

---

### 2️⃣ Copy the Public Key to an Application Server

Use the following **example command** to install the public key on an application server:

```bash
ssh-copy-id user@server_ip
```

What this does:

* Creates the `.ssh` directory if it does not exist
* Appends the public key to `authorized_keys`
* Sets correct permissions automatically

---

### 3️⃣ Verify Password-less Access

Test SSH access using the same **example format**:

```bash
ssh user@server_ip
```

If configured correctly, login will occur **without a password prompt**.

---

### 4️⃣ Repeat for All Application Servers

Repeat the key-copy and verification steps for **each application server**, using its respective sudo user.

---

## ⚡ Optimized Automated Solution

For automation or scripting, the key deployment process can be simplified using:

```bash
ssh-copy-id user@server_ip
```

This is the **recommended approach** for securely enabling SSH access at scale.

---

## 🧠 Good to Know 

### 🔑 SSH Key Authentication Basics

* **Supported Key Types**

  * RSA (2048 / 4096 bits)
  * **Ed25519 (recommended)**
  * ECDSA

* **Key Components**

  * Private Key → never share
  * Public Key → safe to distribute

---

### 📁 SSH Files & Permissions

SSH enforces **strict permissions** to protect keys and prevent unauthorized access.  
If permissions are too open, **SSH will refuse key-based login**.

#### `~/.ssh/` → `700`
- Stores all SSH configuration and keys
- Must be accessible **only by the owner**
- Prevents other users from reading or modifying SSH data

#### `~/.ssh/id_ed25519` → `600`
- Private key used for authentication
- Must **never** be readable by others
- SSH will ignore the key if permissions are too open

#### `~/.ssh/id_ed25519.pub` → `644`
- Public key (safe to share)
- Readable by anyone
- Used to verify the private key during login

#### `~/.ssh/authorized_keys` → `600`
- Contains public keys allowed to log in
- Must be writable **only by the user**
- Prevents key tampering and unauthorized access

> ⚠️ Incorrect permissions are a common cause of SSH key authentication failures.

---

### 🤖 Why Password-less SSH Matters

* Enables cron jobs and automation scripts
* Eliminates interactive password prompts
* Reduces risk of brute-force attacks
* Improves operational efficiency

---

### ✅ Best Practices

* Use modern key types (Ed25519)
* Rotate SSH keys regularly
* Remove unused keys from `authorized_keys`
* Use `ssh-agent` if passphrases are enabled
* Disable password authentication in production environments

---

## 🏁 Conclusion

SSH key-based authentication is a **foundational DevOps practice**.
By using example-based commands, this setup remains **secure, reusable, and automation-friendly** across environments.
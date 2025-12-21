# Day 001: Create a Non-Interactive Service Account User on Linux (Nautilus Project)

**Difficulty:** 🟢 Beginner  
**Time:** 10 minutes  
**Category:** Linux Administration  
**Project:** Nautilus App Server (stapp03)  
**Date:** December 21, 2025

## 🎯 Objective

Create a service account user named `anita` with a non-interactive shell on the Nautilus application server `stapp03`. This user will be used for automated processes and must not allow direct interactive login. Additionally, connect to the server securely using SSH key-based authentication (passwordless).

---

## 📋 Prerequisites

- Local machine with `ssh-keygen` and `ssh-copy-id` available
- Initial access credentials for the server:
  - Server: `stapp03`
  - IP: `172.16.238.12`
  - Hostname: `stapp03.stratos.xfusioncorp.com`
  - User: `banner`
  - Password (one-time only): `BigGr33n`
  - `sudo` privileges on the server

---

## 🔧 Technologies Used

- SSH key-based authentication
- Linux user management (`useradd`, `/etc/passwd`)
- Secure remote access

---

## Steps

### 1. Generate SSH Key Pair (Local Machine)

Run on your local machine:

```bash
ssh-keygen -t rsa -b 4096
```

- Accept default location (`~/.ssh/id_rsa`)
- Optionally set a passphrase for extra security

### 2. Copy Public Key to Server (One-Time Password Login)

```bash
ssh-copy-id banner@172.16.238.12
```

- Enter password `BigGr33n` when prompted

### 3. Connect to Server Using SSH Key (Passwordless)

```bash
ssh banner@172.16.238.12
# or
ssh banner@stapp03.stratos.xfusioncorp.com
```

### 4. Create Non-Interactive User anita

On the server (as a sudo-capable user):

```bash
sudo useradd -m -s /usr/sbin/nologin anita
```

- `-m`: Create home directory `/home/anita`
- `-s /usr/sbin/nologin`: Prevent interactive login

### 5. Verify User Creation

Check `/etc/passwd`:

```bash
grep 'anita:' /etc/passwd
```

Expected output:

```
anita:x:1001:1001::/home/anita:/usr/sbin/nologin
```

Test login restriction:

```bash
sudo su anita
```

Expected output:

```
This account is currently not available.
```

---

## Verification & Troubleshooting

Common Issues:

- SSH key not working:
  - Check permissions:
    - `chmod 700 ~/.ssh`
    - `chmod 600 ~/.ssh/authorized_keys` (on server)
- User already exists:
  - `grep anita /etc/passwd`
- Shell not found:
  - Confirm path: `ls /usr/sbin/nologin`

---

## Useful Commands

```bash
# List all non-interactive users
grep nologin /etc/passwd

# User details
id anita

# Remove user (if needed)
sudo userdel -r anita
```

---

## Key Takeaways

- SSH keys provide secure, passwordless access — always preferred over passwords
- Non-interactive shells (`/usr/sbin/nologin` or `/bin/false`) enhance security for service accounts
- Always verify changes in `/etc/passwd`
- Follow the principle of least privilege

---

## Good to Know — Linux User Management

- User Types: Regular users, system users, service accounts
- Shell Types:
  - `/bin/bash` (interactive)
  - `/usr/sbin/nologin` (non-interactive)
  - `/bin/false` (deny access)
- User Database:
  - `/etc/passwd` stores user information
  - `/etc/shadow` stores passwords

### `useradd` Command Options

- `-m`: Create home directory
- `-s`: Specify shell
- `-d`: Custom home directory path
- `-g`: Primary group
- `-G`: Additional groups
- `-e`: Account expiry date

---

## Security Best Practices

- Use SSH keys for all production servers
- Disable password authentication after key setup
- Service accounts should never have interactive shells
- Keep the principle of least privilege in mind when granting access
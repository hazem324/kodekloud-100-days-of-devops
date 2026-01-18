#  Linux Bash Scripts

##  Task Overview

The production support team needs to automate daily backup operations using **Bash scripting**.

A **static website** is running on an application server under the directory:

```
/var/www/html/news
```

To ensure data safety, a backup script must be created to archive this website and securely transfer it to a **backup server** using **SSH key-based authentication**.

---

##  Objective

Create a Bash script named **`news_backup.sh`** that performs the following tasks:

1. Create a **zip archive** of the `/var/www/html/news` directory  
2. Save the archive locally in `/backup/`  
3. Copy the archive to a **remote backup server**  
4. Ensure the script runs **without password prompts**  
5. Allow execution by the respective server user  
6. Store the script under the `/scripts` directory  

---

##  Environment (Logical Example)

| Component         | Value (Example)              |
|------------------|------------------------------|
| Website Path     | `/var/www/html/news`         |
| Local Backup Dir | `/backup/`                   |
| Script Location  | `/scripts/news_backup.sh`    |
| Remote Server    | Backup Server (example)      |
| Transfer Method  | SSH + SCP                    |

> ⚠️ Server names and users are placeholders for learning purposes.

---

##  Steps

### 1. Install Required Packages

The script requires the `zip` utility:

```bash
sudo yum install zip -y
```

### 2. Configure Password-less SSH Access

SSH key-based authentication must be configured between the application server and the backup server to allow automated, password-less file transfers.

Generate an SSH key pair on the application server:

```bash
ssh-keygen -t rsa -b 2048
```

Copy the public key to the backup server:

```bash
ssh-copy-id backup_user@backup.server.example
```

After this step, SSH and SCP connections will work without asking for a password, which is required for automation.

---

### 3. Create the Backup Script

Create the script file:

```bash
nano /scripts/news_backup.sh
```

---

### 4. Example Bash Script (Generic & Reusable)

```bash
#!/bin/bash

# Variables

SOURCE_DIR="/var/www/html/news"
BACKUP_DIR="/backup"
ARCHIVE_NAME="xfusioncorp_news.zip"

REMOTE_USER="backup_user"
REMOTE_HOST="backup.server.example"
REMOTE_DIR="/backup"

# Create zip archive
zip -r "${BACKUP_DIR}/${ARCHIVE_NAME}" "${SOURCE_DIR}"

# Copy archive to backup server
scp "${BACKUP_DIR}/${ARCHIVE_NAME}" "${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}"
```

### 5. Set Execute Permission

```bash
chmod +x /scripts/news_backup.sh
```

### 6. Execute and Verify

Run the script:

```bash
/scripts/news_backup.sh
```

Expected behavior:

* Zip archive is created successfully
* Archive is transferred to the backup server
* No password prompt appears

---

##  Output

**Local archive**

```
/backup/xfusioncorp_news.zip
```

**Remote archive**

```
/backup/xfusioncorp_news.zip
```

(on the backup server)

---

## 🧠 Good to Know

###  Bash Scripting Best Practices

* Always start scripts with a **shebang** (`#!/bin/bash`)
* Use **variables** instead of hard-coded values
* Use **absolute paths**
* Keep scripts readable and reusable

---

###  Backup Concepts

* **Full Backup**: Complete copy of data
* **Incremental Backup**: Only changed files
* **Differential Backup**: Changes since last full backup
* **Compression**: Reduces storage and transfer size

---

###  Secure Remote Copy

* `scp`: Simple and secure file transfer
* `rsync`: Optimized for repeated backups
* **SSH keys**: Required for automation and cron jobs

# Git Manage Remotes

The xFusionCorp development team maintains a project under `/opt/apps.git`, which is cloned on the Storage Server at `/usr/src/kodekloudrepos/apps`.
Recent updates required adding a **new Git remote**, committing a file, and pushing changes to that remote.

This document explains **the steps and commands used** to complete the task.

---

## Requirements

* Add a new remote `dev_apps` pointing to `/opt/xfusioncorp_apps.git`
* Copy `/tmp/index.html` into the repository
* Add and commit the file to the `master` branch
* Push `master` to the new remote

---

## Steps

### 1. Login to the Storage Server

**Purpose:** Access the server where the repository exists.

```sh
ssh natasha@<storage-server-ip>
```

### 2. Move into the Git repository

**Purpose:** Work inside the correct Git project directory.

```sh
cd /usr/src/kodekloudrepos/apps
```

### 3. Check current branch and remotes

**Purpose:** Verify repository status before making changes.

```sh
sudo git branch
sudo git remote -v
```

### 4. Add the new remote `dev_apps`

**Purpose:** Link the local repository to the new remote repository.

```sh
sudo git remote add dev_apps /opt/xfusioncorp_apps.git
sudo git remote -v
```

### 5. Copy `index.html` into the repository

**Purpose:** Add the required file to be tracked by Git.

```sh
sudo cp /tmp/index.html .
```

### 6. Ensure you are on the `master` branch

**Purpose:** Commit changes to the correct branch.

```sh
sudo git checkout master
```

### 7. Add and commit the file

**Purpose:** Save changes into Git history.

```sh
sudo git add index.html
sudo git commit -m "Add index.html file"
```

### 8. Push `master` branch to `dev_apps` remote

**Purpose:** Send local commits to the new remote repository.

```sh
sudo git push dev_apps master
```

---

## 🧠 Good to Know

### Git Remotes

* **Remote**: A reference to another Git repository
* **origin**: Default remote created during clone
* **Multiple remotes**: Useful for mirrors, backups, or environments

### Useful Commands

| Action         | Command                   |
| -------------- | ------------------------- |
| List remotes   | `git remote -v`           |
| Add remote     | `git remote add name url` |
| Remove remote  | `git remote remove name`  |
| Push to remote | `git push remote branch`  |


# Git Branch Create – Beta Repository

Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/beta`. Recently, they decided to implement new features and want to maintain those changes in a separate branch. Below are the requirements shared with the DevOps team:

* On the **Storage Server in Stratos DC**, create a new branch `xfusioncorp_beta` from the `master` branch in the `/usr/src/kodekloudrepos/beta` Git repository.
* **Do not make any code changes.**

---

## Steps

### 1. Login to the Storage Server

```sh
ssh natasha@172.16.238.15
```

### 2. Navigate to the Repository

```sh
cd /usr/src/kodekloudrepos/beta
```

### 3. Handle Git Safe Directory Warning

Since the repository ownership differs, Git may show a **dubious ownership** error.
To avoid modifying global Git configuration, execute Git commands using `sudo`.

### 4. Check Existing Branches

```sh
sudo git branch
```

Example output:

```text
  kodekloud_beta
  master
```

### 5. Switch to the Master Branch

Ensure you are on the `master` branch before creating a new one:

```sh
sudo git switch master
```

### 6. Create a New Branch from Master

```sh
sudo git checkout -b xfusioncorp_beta
```

### 7. Verify Branch Creation

```sh
sudo git branch
```

Expected output:

```text
  kodekloud_beta
  master
* xfusioncorp_beta
```

---

## 🧠 Good to Know

### . Git Branching Fundamentals

* **Purpose**: Enables parallel development without affecting stable code
* **Lightweight**: A branch is just a pointer to a commit
* **Isolation**: Changes in one branch don’t impact others
* **Safe**: No code modification is required to create branches

### . Common Branch Commands

| Action          | Command                       |
| --------------- | ----------------------------- |
| List branches   | `git branch`                  |
| Switch branch   | `git switch branch-name`      |
| Create branch   | `git branch branch-name`      |
| Create & switch | `git checkout -b branch-name` |

### . Best Practices

* Always create new branches from `master` (or main)
* Avoid committing directly to `master`
* Use meaningful branch names (e.g., feature-based)
* Keep branches short-lived and focused

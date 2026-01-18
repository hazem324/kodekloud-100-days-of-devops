# Git Branch Merge – Official Repository

The Nautilus application development team has been working on a project repository located at `/opt/official.git`. This repository is cloned on the Storage Server in Stratos DC at `/usr/src/kodekloudrepos/official`.

They shared the following requirement with the DevOps team:

 Create a new branch **devops** from **master** in `/usr/src/kodekloudrepos/official`, copy the `/tmp/index.html` file (present on the storage server itself) into the repository, add and commit this file in the new branch, merge the branch back into **master**, and finally push the changes to the origin for both branches.

---

## Steps

### 1. Login to the Storage Server

```sh
ssh natasha@172.16.238.15
```

### 2. Move into the Git Repository

```sh
cd /usr/src/kodekloudrepos/official
```

Verify the current branch:

```sh
sudo git branch
```

### 3. Create a New Branch from Master

```sh
sudo git checkout -b devops
```

Confirm branch creation:

```sh
sudo git branch
```

### 4. Copy the Required File

```sh
sudo cp /tmp/index.html .
```

Verify the file exists:

```sh
ls -al
```

### 5. Add and Commit the Changes

```sh
sudo git add *
sudo git commit -m "added index.html file"
```

### 6. Switch Back to Master Branch

```sh
sudo git switch master
```

### 7. Merge the `devops` Branch into Master

```sh
sudo git merge devops
```

This merge completes as a **fast-forward merge** since there were no conflicting changes.

### 8. Push Changes to the Remote Repository

```sh
sudo git push
```

This pushes the merged changes to the `master` branch on `/opt/official.git`.

---

## 🧠 Good to Know

###  Git Merge Types

* **Fast-forward Merge :**
  Occurs when the target branch has no new commits since branching.

* **Three-way Merge :**
  Creates a merge commit when histories have diverged.

* **Squash Merge :**
  Combines multiple commits into one before merging.

* **Rebase :**
  Reapplies commits on top of another branch for linear history.

###  Merge Workflow

1. Checkout the target branch (`master`)
2. Merge the source branch (`devops`)
3. Resolve conflicts if any
4. Commit the merge (if required)
5. Push changes to the remote repository

###  Best Practices

* Always verify the active branch before committing
* Keep commits small and meaningful
* Test changes before merging
* Use descriptive commit messages
* Clean up feature branches after merging if no longer needed
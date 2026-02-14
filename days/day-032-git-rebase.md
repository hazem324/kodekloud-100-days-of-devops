# Git Rebase

The Nautilus application development team has been working on a project repository located at `/opt/official.git`. This repository is cloned under `/usr/src/kodekloudrepos` on the storage server in Stratos DC.

They shared the following requirement with the DevOps team:

> One of the developers is working on the `feature` branch and their work is still in progress. Meanwhile, some new changes have been pushed to the `master` branch. The developer wants to rebase the `feature` branch on top of the latest `master` branch **without losing any feature branch data** and **without creating a merge commit**.
>
> Complete the task as requested and push the changes once done.

---

## Steps

### 1. Login to Storage Server and Navigate to Repository

```sh
ssh natasha@<storage-server-ip>
cd /usr/src/kodekloudrepos/official
```

### 2. Verify Current Branch

Ensure that you are on the `feature` branch before starting the rebase.

```sh
sudo git branch
```

Expected output:

```text
* feature
  master
```

### 3. Rebase Feature Branch with Master

Reapply feature commits on top of the latest master commits without creating a merge commit.

```sh
sudo git rebase master
```

If conflicts occur, resolve them manually, then continue:

```sh
git add <file_name>
git rebase --continue
```

### 4. Push the Rebased Feature Branch

Since rebase rewrites commit history, force push is required.

```sh
sudo git push --force --set-upstream origin feature
```

---

## Good to Know

### Rebase vs Merge

* **Rebase**: Replays commits on top of another branch (linear history)
* **Merge**: Combines branches using a merge commit
* **History**: Rebase produces a clean, linear history
* **Usage**: Preferred for feature branches before integration

---

### Benefits of Rebase

* Clean and professional commit history
* No unnecessary merge commits
* Easier debugging and `git bisect`
* Ideal for keeping feature branches up to date

---

### Risks of Rebase

* Should not be used on shared/public branches
* Requires force push if commits were already pushed
* Conflict resolution may be needed multiple times
* Original branch divergence context is lost

---

### Interactive Rebase (Advanced)

```sh
git rebase -i HEAD~n
```

Common actions:

* **squash** – Combine commits
* **reword** – Edit commit messages
* **reorder** – Change commit order
* **drop** – Remove commits

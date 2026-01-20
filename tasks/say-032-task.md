# Day 32 - Git Rebase

## Task Description

The Nautilus development team needs to synchronize an in-progress feature branch with the latest changes from the master branch while keeping a clean commit history.

---

## Requirements

### 1. Repository Context

* Central repository:

```
/opt/official.git
```

* Local clone:

```
/usr/src/kodekloudrepos
```

---

### 2. Branch Synchronization

* Update the **feature** branch with the latest changes from:

```
master
```

* Perform this using **rebase**, not merge.
* Ensure:

  * No data is lost from the feature branch.
  * No merge commits are created.

---

### 3. Synchronization

* Push the rebased feature branch to the remote repository.

---

## Expected Result

* The feature branch includes all updates from master.
* Commit history remains linear and clean.
* Remote repository reflects the rebased feature branch.

---

End of Task
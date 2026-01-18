# Day 25 - Git Merge Branches

## Task Description

The Nautilus application development team maintains a repository located at:

```
/usr/src/kodekloudrepos/official
```

The central repository is hosted at:

```
/opt/official.git
```

You are required to perform branching, committing, merging, and pushing operations as described below.

---

## Requirements

### 1. Create a New Branch

From the **master** branch, create a new branch named:

```
devops
```

---

### 2. Add File in New Branch

A file is available on the same server:

```
/tmp/index.html
```

Actions:

* Copy this file into the repository.
* Add it to Git tracking.
* Commit the file in the **devops** branch.

---

### 3. Merge to Master

* Merge the **devops** branch back into **master**.

---

### 4. Push to Remote

Push both branches to the remote repository:

* `master`
* `devops`

---

## Expected Result

* Branch `devops` exists and contains the committed `index.html`.
* The `master` branch includes the merged changes.
* Both branches are successfully pushed to the origin remote.

---

End of Task

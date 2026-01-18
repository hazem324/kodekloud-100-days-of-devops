# DevOps Day 26 – Git Remote Management Task

## Task Description

The xFusionCorp development team updated the project stored on the Git server.
The local clone is available at:

```
/usr/src/kodekloudrepos/apps
```

New Git remotes were added on the server side, and the local repository must be updated accordingly.

---

## Requirements

### 1. Add a New Remote

Inside the repository:

```
/usr/src/kodekloudrepos/apps
```

Add a new remote named:

```
dev_apps
```

Point it to:

```
/opt/xfusioncorp_apps.git
```

---

### 2. Add and Commit File

A file is available at:

```
/tmp/index.html
```

Actions:

* Copy this file into the repository.
* Add it to Git tracking.
* Commit the file on the **master** branch.

---

### 3. Push to Remote

Finally, push the **master** branch to the new remote:

```
dev_apps
```

---

## Expected Result

* The repository contains the new remote `dev_apps`.
* The file `index.html` is committed to the master branch.
* The master branch is successfully pushed to `dev_apps`.

---

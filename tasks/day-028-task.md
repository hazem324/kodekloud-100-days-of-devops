# Selective Commit Merge Task – Nautilus Project

## Task Description

The Nautilus development team needs to merge a specific change from the feature branch into the master branch without merging all ongoing work.

---

## Requirements

### 1. Repository Context

* Central repository:

```
/opt/apps.git
```

* Local clone:

```
/usr/src/kodekloudrepos
```

---

### 2. Branch Operation

* Identify the commit on the `feature` branch with the message:

```
Update info.txt
```

* Merge only this commit into:

```
master
```

---

### 3. Synchronization

* Push the updated `master` branch to the remote repository.

---

## Expected Result

* The change from commit “Update info.txt” is present on the master branch.
* No other feature branch changes are merged.
* Remote repository reflects the update.

---

End of Task

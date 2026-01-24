Here is the clean README-style version:

# Day 34 - Git Hook

## Task Description

The Nautilus team needs to automate release tagging whenever changes are pushed to the `master` branch of the cluster repository.

---

## Requirements

### 1. Repository Context

* Central repository:

```
/opt/cluster.git
```

* Local clone:

```
/usr/src/kodekloudrepos
```

* Perform all actions as user:

```
natasha
```

---

### 2. Branch Merge

* Merge the following branch into `master`:

```
feature
```

---

### 3. Post-Update Hook

* Create a **post-update** Git hook in the repository.
* The hook must:

  * Run whenever changes are pushed to `master`.
  * Automatically create a release tag using the format:

```
release-YYYY-MM-DD
```

(where the date must be the current system date).

---

### 4. Validation

* Push changes to `master`.
* Confirm that a release tag for today is created.

---

### 5. Synchronization

* Push all changes to the remote repository.

---

## Expected Result

* The `feature` branch is merged into `master`.
* A post-update hook exists and runs successfully.
* A new release tag with today’s date is created automatically.

---

## Notes

* Do not change file or directory permissions.
* Do not modify repository ownership.

---

End of Task

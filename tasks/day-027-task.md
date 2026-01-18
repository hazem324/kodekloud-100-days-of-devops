# DevOps Day 27 – Git Revert Task

## Task Description

The Nautilus application development team reported issues with the latest commits in the following repository:

```
/usr/src/kodekloudrepos/cluster
```

Your objective is to revert the repository to the previous stable commit.

---

## Requirements

1. Navigate to the repository:

```
/usr/src/kodekloudrepos/cluster
```

2. Revert the latest commit (HEAD) to the previous commit
   (Note: The previous commit corresponds to the initial commit message.)

3. While reverting, create a new commit using the following message:

```
revert cluster
```

*(The commit message must be in all lowercase.)*

---

## Expected Result

* The HEAD of the repository should point to the previous commit.
* A new revert commit should exist in the history with the message:

```
revert cluster
```

---

## Notes

This task must be performed using `git revert`, not `git reset`, to ensure commit history is preserved.

---

End of Task

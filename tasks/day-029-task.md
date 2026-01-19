# Pull Request Review and Merge Task – Gitea

## Task Description

The team must enforce a controlled workflow where changes are merged into the `master` branch only through a reviewed Pull Request.

---

## Requirements

### 1. Repository Verification

* SSH to the Storage Server as:

```
max
```

* Validate the cloned repository:

  * Confirm existing stories are present.
  * Review commit history using `git log`.

---

### 2. Pull Request Creation

Using the Gitea Web UI:

* Login as:

```
max
```

* Create a Pull Request with:

```
Title: Added fox-and-grapes story
Source Branch: story/fox-and-grapes
Target Branch: master
```

---

### 3. Reviewer Assignment

* Add the following reviewer to the PR:

```
tom
```

---

### 4. Review and Merge

* Logout and login as:

```
tom
```

* Review, approve, and merge the Pull Request.

---

## Expected Result

* The `story/fox-and-grapes` branch is merged into `master`.
* The master branch contains the approved story.
* The PR shows successful approval and merge.

---

## Notes

* Capture screenshots or a screen recording as proof of completion.

---

End of Task

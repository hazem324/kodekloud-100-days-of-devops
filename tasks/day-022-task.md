Perfect, I will format this one the same way.

Here is the README-style version, keeping the task description exactly as given:

# Git Clone Task – Nautilus Project

## Task Description

The DevOps team established a new Git repository last week, which remains unused at present. However, the Nautilus application development team now requires a copy of this repository on the Storage Server in the Stratos DC. Follow the provided details to clone the repository:

The repository to be cloned is located at /opt/blog.git
Clone this Git repository to the /usr/src/kodekloudrepos directory. Perform this task using the natasha user, and ensure that no modifications are made to the repository or existing directories, such as changing permissions or making unauthorized alterations.

---

## Execution Steps

1. Switch to the natasha user.
2. Navigate to:

   ```
   /usr/src/kodekloudrepos
   ```
3. Clone the repository:

   ```
   /opt/blog.git
   ```

---

## Verification

* A new directory named `blog` exists inside `/usr/src/kodekloudrepos`.
* The repository is successfully cloned without errors.
* No files or permissions were modified outside the clone operation.

---

## Notes

* Do not edit, add, or remove any files after cloning.

---

End of Task


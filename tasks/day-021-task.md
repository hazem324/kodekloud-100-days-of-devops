Here is the README version for this task, keeping the task description exactly as given:

# Git Repository Setup Task – Nautilus Project

## Task Description

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:

Utilize yum to install the git package on the Storage Server.

Create a bare repository named /opt/apps.git (ensure exact name usage).

---

## Execution Steps

1. Install Git using yum.
2. Create a bare Git repository at:

   ```
   /opt/apps.git
   ```

---

## Verification

* Git is successfully installed on the server.
* Directory `/opt/apps.git` exists and is initialized as a bare repository.

---

## Notes

* The repository must be bare.
* Do not create a working tree inside this directory.

---

End of Task

You can continue sending the next task when ready.

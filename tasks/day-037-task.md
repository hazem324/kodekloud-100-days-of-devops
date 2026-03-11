Here is the README-style version for this task:

# Day 37 - Copy File to Docker Container

## Task Description

The Nautilus DevOps team must securely copy an encrypted file from the Docker host into a running container on App Server 1.

---

## Requirements

### 1. Source and Destination

* Source file on host:

```
/tmp/nautilus.txt.gpg
```

* Target container:

```
ubuntu_latest
```

* Destination path inside container:

```
/opt/
```

---

### 2. Transfer Constraints

* The file must be copied without modification.
* The container must remain running.

---

## Expected Result

* The file exists inside the container at `/opt/nautilus.txt.gpg`.
* File integrity is preserved.

---

End of Task

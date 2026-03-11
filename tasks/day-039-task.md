# Day 39 - Create a Docker Image From Container

## Task Description

The Nautilus DevOps team needs to create a backup image from an existing running container for future use.

---

## Requirements

### 1. Image Creation

* On **Application Server 1**, create a Docker image with:

```
Image name: apps
Tag: datacenter
```

* Source container:

```
ubuntu_latest
```

---

## Expected Result

* A new image `apps:datacenter` exists on Application Server 1.
* The image is created from the running container without errors.

---

End of Task

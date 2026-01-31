# Pull Docker Image

Nautilus project developers are planning to start testing on a new project. Following discussions with the DevOps team, they require validation of containerized application features. As part of this preparation, a specific Docker image operation must be completed on **App Server 2** in the Stratos Datacenter.

## Task Description

* Pull the Docker image **`busybox:musl`** on **App Server 2**
* Re-tag the pulled image as **`busybox:news`**

---

## Steps 

### 1. Connect to App Server 2

```sh
ssh steve@172.16.238.11
```

### 2. Pull the BusyBox Image

```sh
docker pull busybox:musl
```

This command downloads the `busybox:musl` image from Docker Hub to the local Docker host.

### 3. Verify the Image

```sh
docker images
```

Expected output includes:

```text
REPOSITORY   TAG    IMAGE ID       SIZE
busybox      musl   <image-id>     1.51MB
```

### 4. Re-Tag the Image

```sh
docker tag busybox:musl busybox:news
```

This creates a new tag (`news`) pointing to the same image ID.

### 5. Confirm the New Tag

```sh
docker images
```

Expected output:

```text
REPOSITORY   TAG    IMAGE ID       SIZE
busybox      musl   <image-id>     1.51MB
busybox      news   <image-id>     1.51MB
```

---

## Good to Know

### Docker Image Tagging

* Tags are **labels**, not copies of images.
* Multiple tags can reference the **same image ID**.
* Re-tagging is instantaneous and storage-efficient.

### BusyBox Image Overview

* **Ultra-lightweight** Linux image (~1–2 MB)
* Includes common Unix utilities in a single binary
* Frequently used for:

  * Testing container behavior
  * Debugging networking and volumes
  * Minimal base images

### Common Docker Image Commands

* Pull image

  ```sh
  docker pull image:tag
  ```

* List images

  ```sh
  docker images
  ```

* Tag image

  ```sh
  docker tag source:tag target:tag
  ```

* Remove image

  ```sh
  docker rmi image:tag
  ```

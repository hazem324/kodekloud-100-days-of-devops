Below is a **clean, task-accurate README.md** generated from **your actual work** on **Day 43**, following the same structure and quality as your example.
# Docker Ports Mapping 

The Nautilus DevOps team is planning to host an application on an **nginx-based container**. One of the tickets requires setting up an nginx container on **Application Server 1** in the **Stratos Datacenter**.

This document describes the steps performed to complete the task successfully.

---

## Task Requirements

* Pull `nginx:alpine` docker image on **Application Server 1**
* Create a container named `ecommerce`
* Map host port `6300` to container port `80`
* Ensure the container is in **running state**

---

## Steps

### 1. Login to Application Server 1

```sh
ssh tony@stapp01
```

### 2. Pull the Docker Image

```sh
docker pull nginx:alpine
```

### 3. Create and Run the Container with Port Mapping

```sh
docker run -d --name ecommerce -p 6300:80 nginx:alpine
```

**Explanation:**

* `-d` → Run container in detached mode
* `--name ecommerce` → Assign container name
* `-p 6300:80` → Map host port 6300 to container port 80
* `nginx:alpine` → Image used

### 4. Verify Container Status

```sh
docker ps
```

Expected output:

* Container name: `ecommerce`
* Image: `nginx:alpine`
* Port mapping: `6300->80`
* Status: `Up`

---

## Good to Know

### Docker Port Mapping

* **Purpose**: Expose container services to the host network
* **Access**: Allows external users to reach containerized apps
* **Traffic Flow**: Host port forwards traffic to container port
* **Isolation**: Containers stay isolated while remaining accessible

**Syntax:**

* `-p host_port:container_port`
* Example: `-p 6300:80`

### NGINX Container

* **Web Server**: High-performance HTTP server
* **Default Port**: Listens on port 80 inside the container
* **Lightweight**: Alpine image reduces size and attack surface
* **Common Use**: Static sites, reverse proxy, load balancing

### Container States

* **Running**: Container is active and serving requests
* **Exited**: Container stopped
* **Paused**: Processes temporarily halted
* **Restarting**: Container repeatedly failing and restarting


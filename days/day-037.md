# Copy File to Docker Container

The Nautilus DevOps team stores confidential data on **App Server 1** in the Stratos Datacenter. As part of a security and deployment validation task, an encrypted file must be transferred from the Docker host into a running container without altering its contents.

A container named **`ubuntu_latest`** is already running on the server.

---

## Steps

1. Log in to **App Server 1**.
2. Execute the following command to copy the file into the container:

```sh
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/
```

3. (Optional) Verify the file inside the container:

```sh
docker exec ubuntu_latest ls -l /opt/nautilus.txt.gpg
```

[![Verify the file inside the container](../screenshots/Screenshot-day-037-verify-the-file-inside-the-container.png)](../screenshots/Screenshot-day-037-verify-the-file-inside-the-container.png)


---

## Good to Know

### Docker `cp` Command

* **Purpose**: Transfers files between a Docker host and a container
* **Integrity**: Performs a byte-level copy (file contents are not modified)
* **Container State**: Works with both running and stopped containers
* **Permissions**: Preserves file permissions and ownership by default

### Command Syntax

* **Host → Container**

  ```sh
  docker cp /host/path container:/container/path
  ```

* **Container → Host**

  ```sh
  docker cp container:/container/path /host/path
  ```

### Common Use Cases

* Copy encrypted or confidential files securely
* Inject configuration files for testing
* Extract logs for debugging
* One-time data transfer without volumes

### Best Practices

* Use **Docker volumes** for persistent or frequently updated data
* Use **COPY** in a Dockerfile for build-time file inclusion
* Limit use of `docker cp` to controlled or temporary operations
* Always verify file integrity after copying sensitive data


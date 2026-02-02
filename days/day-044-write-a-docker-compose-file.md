# Hosting Static Website Using httpd with Docker Compose

The Nautilus application development team shared static website content that needs to be hosted on an **httpd web server** using a **containerised platform**.
The DevOps team is responsible for setting up the environment on **App Server 3** in **Stratos DC** according to the following requirements.

---

## Steps

### 1. Login to App Server 3

```sh
ssh tony@stapp03
```

### 2. Verify Docker Is Running

```sh
docker ps
```

If Docker is running, the command will execute without errors.

### 3. Create Docker Compose File

```sh
sudo vi /opt/docker/docker-compose.yml
```

### 4. Edit `docker-compose.yml`

Open the file and add the following content:

Open the file in editor and copy paste the contents from this [docker-compose](../files/docker-compose.yaml)

### 5. Start the Container


```sh
sudo docker compose -f /opt/docker/docker-compose.yml up -d
```

### 6. Verify Container Status

```sh
docker ps
```

Expected output:

```shell
CONTAINER ID   IMAGE          COMMAND              CREATED          STATUS          PORTS                  NAMES
abcd1234efgh   httpd:latest  "httpd-foreground"   1 minute ago     Up 1 minute     0.0.0.0:6100->80/tcp   httpd
```

### 7. Verify Website Access

```sh
curl http://localhost:6100
```

---

## Good to Know

### Docker Compose

* **Purpose**: Define and run containerized applications
* **Configuration**: Uses YAML files
* **Management**: Start, stop, and manage containers as a single unit
* **Default on Nautilus**: Docker Compose v2 (`docker compose`)

---

### Compose File Structure

* **version**: Compose file format version
* **services**: Application containers
* **ports**: Host ↔ container port mapping
* **volumes**: Persistent data mapping

---

### Common Docker Compose Commands

* Start services:

  ```sh
  docker compose up -d
  ```
* Stop and remove services:

  ```sh
  docker compose down
  ```
* View logs:

  ```sh
  docker compose logs
  ```
* List running containers:

  ```sh
  docker ps
  ```
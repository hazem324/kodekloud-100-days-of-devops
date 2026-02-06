# Deploy an App on Docker Containers

The Nautilus Application development team recently finished development of one of the applications that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a Docker Compose file.

Below are the details of the task.

---

## Task Requirements

* On **App Server 3** in **Stratos Datacenter**, create a Docker Compose file
  **`/opt/data/docker-compose.yml`** (should be named exactly).

* The compose file should deploy **two services (web and DB)**.

---

### Web Service Requirements

* Container name must be **`php_host`**
* Use image **`php` with any apache tag**
* Map container port **80** to host port **3000**
* Map container volume **`/var/www/html`** with host volume **`/var/www/html`**

---

### Database Service Requirements

* Container name must be **`mysql_host`**
* Use image **`mariadb`** with any tag (preferably latest)
* Map container port **3306** to host port **3306**
* Map container volume **`/var/lib/mysql`** with host volume **`/var/lib/mysql`**
* Set:

  * `MYSQL_DATABASE=database_host`
  * Use any **custom user (except root)** with a **complex password**

---

After running Docker Compose, the application should be accessible using:

```sh
curl <server-ip or hostname>:3000/
```

---

## Steps


### 1. Login to App Server 3

```sh
ssh banner@stapp03
```

### 2. Create the Docker Compose file

```sh
sudo mkdir -p /opt/data
sudo touch /opt/data/docker-compose.yml
```

### 3. Add the following content to `/opt/data/docker-compose.yml`

```yaml
services:
  web:
    image: php:7.2-apache
    container_name: php_host
    ports:
      - "3000:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    image: mariadb:latest
    container_name: mysql_host
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_host
      MYSQL_USER: appuser
      MYSQL_PASSWORD: Str0ngP@ssw0rd!
      MYSQL_ROOT_PASSWORD: R00tP@ssw0rd!
```

---

### 4. Deploy the stack

> Docker Compose v2 is installed on the server.

```sh
cd /opt/data
docker compose up -d
```

### 5. Verify running containers

```sh
docker ps
```

Expected containers:

* `php_host`
* `mysql_host`

### 6. Test the application

```sh
curl localhost:3000
```

[![Test the application](../screenshots/Screenshot-day-46-test-docker-compose.png)](../screenshots/Screenshot-day-46-test-docker-compose.png)

---

## Good to Know

### Multi-Container Applications with Docker Compose

- **Clear separation of concerns** — each service (web, database, cache, etc.) runs in its own container
- **Independent lifecycle** — you can update, restart, scale or debug one service without affecting others
- **Automatic service discovery** — containers can reach each other using service names as hostnames
- **Reproducible environments** — the same `docker-compose.yml` works on dev, test, and (with minor changes) production

### Typical Two-Tier (LAMP-like) Architecture

- **Presentation / Application tier** → PHP + Apache (or Nginx)  
- **Data tier** → MariaDB / MySQL / PostgreSQL  
- Containers communicate over a **Docker network** created automatically by Compose

### Docker Compose Key Advantages

- Single declarative file for the entire stack
- One command to start/stop/rebuild (`docker compose up -d`, `down`, etc.)
- Built-in volume and network management
- Easy environment variable and secret handling
- Great for local development, CI/CD testing, and small-to-medium production setups

### Database Container Best Practices

- Never use the `root` user for application connections
- Always create a dedicated application user with limited privileges
- Use **strong, unique passwords** (preferably managed via secrets in production)
- Persist database data using **named volumes** or host-mounted directories
- Avoid storing sensitive credentials directly in the compose file in real projects
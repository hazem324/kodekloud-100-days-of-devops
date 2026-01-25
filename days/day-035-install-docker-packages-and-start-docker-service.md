# Install Docker Packages and Start Docker Service

The Nautilus DevOps team plans to containerize multiple applications after a discussion with the application development team.
As part of initial testing, Docker Engine and Docker Compose are installed on **App Server 1**, and the Docker service is started.

## Objective

* Install **docker-ce** and **docker compose plugin** on App Server 1
* Start and enable the Docker service


---

##  Steps

### 1. Install Repository Management Tools

Install `dnf-plugins-core`, which provides the `dnf config-manager` command:

```sh
sudo dnf install -y dnf-plugins-core
```

### 2. Add the Official Docker Repository

Add the Docker CE repository so DNF can locate Docker packages:

```sh
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### 3. Install Docker Engine and Compose Plugin

Install Docker Engine, CLI, container runtime, Buildx, and Docker Compose (v2 plugin):

```sh
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 4. Enable and Start Docker Service

Enable Docker to start on boot and start it immediately:

```sh
sudo systemctl enable --now docker
```

Verify Docker status:

```sh
sudo systemctl status docker
```

### 5. Verify Installation

Check Docker version:

```sh
docker --version
docker compose version
```

---

## Good to Know

### Docker Fundamentals

* **Containerization**: Packages applications with all dependencies
* **Image**: Immutable template used to create containers
* **Container**: A running instance of an image
* **Dockerfile**: Defines how an image is built

### Docker Architecture

* **Docker Engine**: Core container runtime
* **Docker Daemon (`dockerd`)**: Manages containers, images, and networks
* **Docker CLI**: User interface to interact with Docker
* **Docker Registry**: Stores and distributes images (e.g., Docker Hub)


### Key Benefits of Docker

* **Portability**: Run the same container across environments
* **Consistency**: Eliminates “works on my machine” issues
* **Efficiency**: Lightweight compared to virtual machines
* **Scalability**: Simplifies horizontal scaling and orchestration

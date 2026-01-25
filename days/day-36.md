# Deploy Nginx Container on Application Server

The Nautilus DevOps team is conducting application deployment tests on selected application servers. As part of this task, an NGINX container must be deployed on **Application Server 2** using Docker.

The objective is to ensure the container is created with the correct image and is running successfully.


## Steps

Run the following command to create and start the container:

```sh
sudo docker run -d --name nginx_2 nginx:alpine
```

Verify the container status:

```sh
sudo docker ps -a
```

The output should show the container `nginx_2` with status **Up**.

## Good to Know?

### Docker Container Basics

* **Container**: A running instance of a Docker image
* **Image**: A read-only template used to create containers
* **Lightweight**: Containers share the host OS kernel
* **Isolation**: Applications run independently from each other

### Docker Run Options

* **-d**: Runs the container in detached (background) mode
* **--name**: Assigns a custom name to the container
* **-p**: Maps host ports to container ports (not required in this task)
* **--rm**: Automatically removes container when it stops (optional)

### NGINX Alpine Image

* **Alpine Linux**: Minimal base image (~5MB)
* **Efficient**: Faster startup and lower memory usage
* **Secure**: Smaller attack surface
* **Production-ready**: Commonly used in real-world deployments

### Container Management Commands

* **List running containers**:

  ```sh
  docker ps
  ```
* **List all containers**:

  ```sh
  docker ps -a
  ```
* **Stop a container**:

  ```sh
  docker stop nginx_2
  ```
* **Start a container**:

  ```sh
  docker start nginx_2
  ```
* **Remove a container**:

  ```sh
  docker rm nginx_2
  ```

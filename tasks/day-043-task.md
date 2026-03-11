# Day 43 - Docker Ports Mapping

## Task Description

Set up a running Nginx-based container on the application server to host an application with a custom port mapping.

## Requirements

* Pull the following Docker image on Application Server 1:

```
nginx:alpine
```

* Create a container with the following name:

```
ecommerce
```

* Configure port mapping from host to container:

```
Host Port: 6300
Container Port: 80
```

* Ensure the container remains running after creation.

## Expected Result

* The `nginx:alpine` image is available on the server.
* A container named `ecommerce` is created from the image.
* Traffic to host port `6300` is forwarded to container port `80`.
* The container is in a running state.

End of Task

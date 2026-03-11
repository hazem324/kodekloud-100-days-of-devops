# Day 47 - Docker Python App

## Task Description

Containerize a Python application, build a custom image, and deploy it as a running container for local testing.

## Requirements

* Create a Dockerfile at the following location:

```
/python_app/Dockerfile
```

* Use any Python base image.
* Install application dependencies from:

```
/python_app/src/requirements.txt
```

* Configure the container to expose:

```
6000
```

* Set the container startup to run:

```
server.py
```

* Build a Docker image with the name:

```
nautilus/python-app
```

* Create a container with the name:

```
pythonapp_nautilus
```

* Configure port mapping:

```
Host Port: 8098
Container Port: 6000
```

* Perform all steps on App Server 2.

## Expected Result

* A valid Dockerfile exists in `/python_app`.
* The Docker image `nautilus/python-app` builds successfully.
* A container named `pythonapp_nautilus` is running.
* The application is accessible via host port `8098`.

End of Task

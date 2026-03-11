# Day 41 - Write a Docker File

## Task Description

Create a Dockerfile to build a custom Ubuntu-based image with Apache installed and configured to listen on a non-default port.

## Requirements

* Create the Dockerfile at the following path:

```
/opt/docker/Dockerfile
```

* Use the following base image:

```
ubuntu:24.04
```

* Install the Apache web server package:

```
apache2
```

* Configure Apache to listen on the following port:

```
6400
```

* Do not modify any Apache settings other than the listening port (e.g., document root must remain unchanged).

## Expected Result

* A valid Dockerfile exists at the specified path.
* The Dockerfile builds an image based on Ubuntu 24.04.
* Apache is installed within the image.
* Apache is configured to listen on port `6400` only.

## Notes

* The filename must be exactly `Dockerfile` with a capital `D`.

End of Task

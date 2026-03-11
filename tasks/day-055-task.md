# Day 55 - Kubernetes Sidecar Containers

## Task Description

Implement the Sidecar pattern to stream Nginx access and error logs using a shared `emptyDir` volume within a single pod.

## Requirements

* Create a pod with the following name:

```
webserver
```

### Volume Configuration

* Define a shared volume:

```
Name: shared-logs
Type: emptyDir
```

* Mount the volume at:

```
/var/log/nginx
```

on both containers.

### Nginx Container

* Name:

```
nginx-container
```

* Image:

```
nginx:latest
```

* Mount the shared volume at:

```
/var/log/nginx
```

### Sidecar Container

* Name:

```
sidecar-container
```

* Image:

```
ubuntu:latest
```

* Mount the shared volume at:

```
/var/log/nginx
```

* Configure the container command as:

```
"sh","-c","while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"
```

* Ensure all containers remain in a running state.
* Use the `kubectl` utility configured on `jump_host` for deployment.

## Expected Result

* A pod named `webserver` is running with two containers.
* Both containers share the `emptyDir` volume `shared-logs`.
* Nginx writes logs to `/var/log/nginx`.
* The sidecar container continuously reads and outputs the access and error logs.

End of Task

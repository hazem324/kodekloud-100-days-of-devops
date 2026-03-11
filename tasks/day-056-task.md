# Day 56 - Deploy Nginx Web Server on Kubernetes Cluster

## Task Description

Deploy a scalable and highly available Nginx application using a Deployment with multiple replicas and expose it via a NodePort service.

## Requirements

### Deployment

* Name:

```
nginx-deployment
```

* Image:

```
nginx:latest
```

* Container name:

```
nginx-container
```

* Replica count:

```
3
```

### Service

* Name:

```
nginx-service
```

* Type:

```
NodePort
```

* NodePort:

```
30011
```

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* A deployment named `nginx-deployment` is running with 3 replicas.
* All pods are in a ready and running state.
* A NodePort service named `nginx-service` exposes the application on port `30011`.
* The application is accessible through the assigned NodePort.

End of Task

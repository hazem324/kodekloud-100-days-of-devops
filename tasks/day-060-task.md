# Day 60 - Persistent Volumes in Kubernetes
## Task Description

Provision persistent storage for a web application, mount it to an Nginx pod, and expose the application using a NodePort service.

## Requirements

### PersistentVolume

* Name:

```
pv-devops
```

* Storage class:

```
manual
```

* Capacity:

```
3Gi
```

* Access mode:

```
ReadWriteOnce
```

* Volume type:

```
hostPath
```

* Host path:

```
/mnt/dba
```

### PersistentVolumeClaim

* Name:

```
pvc-devops
```

* Storage class:

```
manual
```

* Requested storage:

```
1Gi
```

* Access mode:

```
ReadWriteOnce
```

### Pod

* Name:

```
pod-devops
```

* Container name:

```
container-devops
```

* Image:

```
nginx:latest
```

* Mount the volume using claim:

```
pvc-devops
```

* Mount path (document root):

```
/usr/share/nginx/html
```

### Service

* Name:

```
web-devops
```

* Type:

```
NodePort
```

* NodePort:

```
30008
```

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* PersistentVolume `pv-devops` is created and available.
* PersistentVolumeClaim `pvc-devops` is bound to the volume.
* Pod `pod-devops` is running with the volume mounted at the Nginx document root.
* NodePort service `web-devops` exposes the application on port `30008`.
* The web application is accessible via the assigned NodePort.

End of Task

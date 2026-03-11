# Day 50 - Set Resource Limits in Kubernetes Pods

## Task Description

Deploy an Apache HTTP Server pod with defined CPU and memory requests and limits to control resource usage.

## Requirements

* Create a pod with the following name:

```
httpd-pod
```

* Define a container with the name:

```
httpd-container
```

* Use the image:

```
httpd:latest
```

* Configure resource requests:

  * Memory:

```
15Mi
```

* CPU:

```
100m
```

* Configure resource limits:

  * Memory:

```
20Mi
```

* CPU:

```
100m
```

* Perform the deployment using `kubectl` configured on `jump_host`.

## Expected Result

* A pod named `httpd-pod` is created in the cluster.
* The container `httpd-container` runs using the `httpd:latest` image.
* Resource requests and limits are correctly applied.
* The pod is in a running state.

End of Task

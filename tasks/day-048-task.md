# Day 48 - Deploy Pods in Kubernetes Cluster

## Task Description

Create a Kubernetes pod running an Apache HTTP Server container with defined naming and labeling conventions.

## Requirements

* Create a pod with the following name:

```
pod-httpd
```

* Use the container image:

```
httpd:latest
```

* Define the container name as:

```
httpd-container
```

* Apply the following label to the pod:

```
app=httpd_app
```

* Use the Kubernetes cluster accessible from the configured `kubectl` on `jump_host`.

## Expected Result

* A pod named `pod-httpd` exists in the cluster.
* The pod runs a container named `httpd-container` using the `httpd:latest` image.
* The pod has the label `app=httpd_app`.
* The pod is in a running state.

End of Task

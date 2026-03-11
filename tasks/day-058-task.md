# Day 58 -  Deploy Grafana on Kubernetes Cluster

## Task Description

Deploy the Grafana application on the Kubernetes cluster and expose it externally using a NodePort service.

## Requirements

### Deployment

* Name:

```
grafana-deployment-datacenter
```

* Use any valid Grafana image.
* Configure deployment parameters as needed (e.g., replicas, labels, container name).
* Ensure the pod runs successfully.

### Service

* Type:

```
NodePort
```

* NodePort:

```
32000
```

* Expose the Grafana application through the service.

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* The deployment `grafana-deployment-datacenter` is running successfully.
* A NodePort service exposes Grafana on port `32000`.
* The Grafana login page is accessible externally.
* No internal configuration changes are required within the Grafana application.

End of Task

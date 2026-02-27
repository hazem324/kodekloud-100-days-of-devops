#  Deploy Redis Deployment on Kubernetes

The DevOps team identified performance bottlenecks in a Kubernetes-based application and decided to introduce **Redis** as an in-memory caching layer for the database service.

For testing purposes, Redis is deployed inside the Kubernetes cluster with a controlled memory configuration using a ConfigMap.

This deployment includes:

* A custom `ConfigMap` for Redis configuration
* A `Deployment` with 1 replica
* CPU resource request
* Two mounted volumes (emptyDir + ConfigMap)
* Redis exposed on port `6379`

---

#  Architecture Overview

* **Image**: `redis:alpine`
* **Deployment Name**: `redis-deployment`
* **Container Name**: `redis-container`
* **Replica Count**: 1
* **CPU Request**: 1 core
* **Memory Limit via ConfigMap**: 2MB
* **Volumes**:

  * `emptyDir` → `/redis-master-data`
  * `ConfigMap` → `/redis-master`

---

#  Steps

## 1. Create the ConfigMap

Create a file called:

```bash
vi redis-configmap.yaml
```
Add the following content: [`redis-configmap file`](../files/k8s_redis_configmap_d65.yml)

Apply it:

```bash
kubectl apply -f redis-configmap.yaml
```

Verify:

```bash
kubectl describe configmap my-redis-config
```

## 2. Create the Deployment

Create the deployment file:

```bash
vi redis-deployment.yaml
```
Add the following content: [`redis-deployment file`](../files/k8s_redis_deployment_d63.yml)

Apply it:

```bash
kubectl apply -f redis-deployment.yaml
```

## 3. Verify Deployment Status

Check deployment:

```bash
kubectl get deployments
```

Check pod:

```bash
kubectl get pods
```

Check logs:

```bash
kubectl logs <redis-pod-name>
```

Redis should be running successfully 

---

#  Good to Know

##  Why Use ConfigMap for Redis?

* Separates configuration from container image
* Easy to update without rebuilding image
* Keeps deployment clean and maintainable
* Follows 12-factor app principles


##  Why Add the `command` Section?

Mounting a ConfigMap does NOT automatically make Redis use it.

Without:

```yaml
command:
  - redis-server
  - /redis-master/redis-config
```

Redis would start with default settings.

This is a common mistake in Kubernetes deployments 


##  Why Use emptyDir?

* `emptyDir` provides temporary storage
* Data persists as long as the Pod is running
* Deleted when Pod is removed
* Good for testing environments

For production, use:

* PersistentVolume
* StatefulSet

---

##  CPU Request = 1

Setting:

```yaml
resources:
  requests:
    cpu: "1"
```

Ensures:

* Kubernetes scheduler reserves 1 CPU
* Pod won’t be placed on overloaded nodes
* More predictable performance

---

##  Production Improvements 

For production deployment, consider:

* Adding **liveness & readiness probes**
* Adding a **Service** object
* Setting memory limits
* Using **PersistentVolume**
* Enabling authentication (`requirepass`)
* Deploying Redis Sentinel or Redis Cluster
* Monitoring with Prometheus + Grafana
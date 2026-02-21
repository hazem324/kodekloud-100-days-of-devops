#  Fixing Redis Deployment Failure in Kubernetes

Last week, the Nautilus DevOps team deployed a Redis application on the Kubernetes cluster. It was running correctly until a team member introduced configuration mistakes in the existing setup. As a result, the `redis-deployment` pods stopped working and were stuck in a non-running state.

The objective was to investigate the issue directly from the `jump_host` and restore the application.


---

#  Step-by-Step


## 1. Check Pod Logs

First, we attempted to check container logs:

```bash
kubectl logs redis-deployment-54cdf4f76d-zx56k
```

Result:

```
container "redis-container" is waiting to start: ContainerCreating
```

⚠️ This means the container never started — so logs are unavailable.

When logs fail, the next step is always:

## 2. Inspect Pod YAML

```bash
kubectl get pod redis-deployment-54cdf4f76d-zx56k -o yaml
```

###  Critical Findings

Two configuration errors were identified:

---

###  Issue 1 — Wrong Image Tag

```yaml
image: redis:alpin
```

Correct tag:

```
redis:alpine
```

The tag `alpin` does **not exist**, so Kubernetes could not pull the image.

###  Issue 2 — Incorrect ConfigMap Name

```yaml
name: redis-conig
```

Correct name:

```
redis-config
```

The deployment was referencing a ConfigMap that does not exist.

This caused volume mount failure during pod initialization.


## 3. Edit the Deployment

Instead of modifying the pod directly (since it is managed by a ReplicaSet), we edited the deployment:

```bash
kubectl edit deployment redis-deployment
```

### Fix 1 — Correct Image

```yaml
image: redis:alpine
```

### Fix 2 — Correct ConfigMap Name

```yaml
name: redis-config
```

Save and exit.

## 4. Restart the Deployment

```bash
kubectl rollout restart deployment redis-deployment
```

## 5. Verify Pod Status

```bash
kubectl get pods
```

Expected output:

```
STATUS: Running
```

## 6. Confirm Deployment Health

```bash
kubectl get deployment redis-deployment
```

---

#  Root Cause Summary

| Issue                                | Impact                                      |
| ------------------------------------ | ------------------------------------------- |
| Wrong image tag (`alpin`)            | Image pull failed → container never created |
| Misspelled ConfigMap (`redis-conig`) | Volume mount failed → pod stuck in Pending  |


---

#  Good to Know 

##  Why `kubectl logs` Didn't Work

Logs only work when a container has started.
If a pod is stuck in `ContainerCreating`, the issue is typically:

* Image pull failure
* Volume mount failure
* Missing ConfigMap or Secret
* Storage/PVC issue

---

##  Understanding Pod Phases vs Container States

**Pod Phase**

* `Pending` → Not fully scheduled or initializing
* `Running` → At least one container running
* `Failed` → Pod terminated unsuccessfully

**Container State**

* `Waiting` → Not started yet
* `Running` → Executing normally
* `Terminated` → Finished or crashed


##  Systematic Troubleshooting Pattern 

When a Kubernetes app goes down:

1. `kubectl get pods`
2. `kubectl logs <pod>`
3. If logs unavailable → `kubectl describe pod`
4. If still unclear → inspect full YAML
5. Validate:

   * Image names
   * ConfigMap/Secret names
   * Volume mounts
   * Ports
6. Fix at the Deployment level
7. Restart rollout
8. Verify health

This structured approach prevents random guessing.

#  Kubernetes Sidecar Containers

##  Overview

This task demonstrates the **Sidecar pattern** in Kubernetes.

We deploy a Pod named `webserver` that contains:

* **nginx-container** → serves web traffic
* **sidecar-container** → reads and ships nginx logs

Both containers share an `emptyDir` volume so that logs written by nginx can be accessed by the sidecar container.

The logs are **ephemeral** and do not require persistent storage.

---

#  Objectives

* Create a Pod named `webserver`
* Create an `emptyDir` volume named `shared-logs`
* Deploy two containers:

  * `nginx:latest`
  * `ubuntu:latest`
* Mount `/var/log/nginx` in both containers
* Continuously read logs from the sidecar container
* Ensure both containers are running

---

#  Steps 

## 1. Create the YAML file

```bash
vi webserver.yml
```

Paste the [`FILE`](../files/k8s_webserver_55d.yml) and save.

## 2. Deploy the Pod

```bash
kubectl apply -f webserver.yml
```

## 3. Verify Pod Status

```bash
kubectl get pods
```
[![Verify Pod Status](../screenshots/Screenshot-day-55-verify-pod-running.png)](../screenshots/Screenshot-day-55-verify-pod-running.png)

## 4. Describe the Pod

```bash
kubectl describe pod webserver
```

Verify:

* Both containers are listed
* `shared-logs` volume exists
* Volume type is `emptyDir`

## 5. Verify Log Sharing

```bash
kubectl logs webserver -c sidecar-container
```

[![Verify Log Sharing](../screenshots/Screenshot-day-55-verify-sidecar-container-reading-log.png)](../screenshots/Screenshot-day-55-verify-sidecar-container-reading-log.png)

---

## Good to Know 

###  Why Use the Sidecar Pattern Here?

* **Clear Responsibility Split**:
  The `nginx-container` focuses only on serving HTTP traffic.
  The `sidecar-container` focuses only on handling logs.

* **Cleaner Architecture**:
  Instead of mixing log processing inside the nginx image, we attach a helper container.

* **Better Maintainability**:
  If log shipping logic changes, we update the sidecar — not nginx.

* **Production-Ready Design**:
  This mirrors real-world logging architectures used in large Kubernetes clusters.


###  Why Use emptyDir Instead of Persistent Storage?

* **Ephemeral Storage**:
  `emptyDir` exists only during the Pod lifecycle.

* **Automatic Cleanup**:
  When the Pod is deleted, logs are automatically removed.

* **No Storage Overhead**:
  Since logs are not critical business data, there is no need for long-term storage.

* **Perfect for Temporary Data**:
  Ideal for logs, cache, or inter-container communication.


###  How Log Sharing Works Inside the Pod

* Both containers mount the same `emptyDir` volume at `/var/log/nginx`.
* Nginx writes logs normally.
* The sidecar reads the same files from the shared directory.
* Because they run in the same Pod, they share:

  * Network namespace
  * Storage (via volume)
  * Lifecycle

This enables seamless communication without external services.


###  Why Not Let Nginx Handle Log Shipping?

* Violates the **Single Responsibility Principle**
* Makes the nginx container heavier
* Reduces modularity
* Harder to scale independently
* Harder to debug issues

Using a sidecar keeps the system modular and easier to evolve.


###  What Happens If the Pod Restarts?

* The `emptyDir` volume is deleted.
* All logs stored inside it are lost.
* This behavior is expected because logs are considered temporary.

In real production systems, logs would typically be shipped to:

* Elasticsearch
* Loki
* Splunk
* or other centralized logging systems


###  When Is This Pattern Commonly Used?

The Sidecar pattern is widely used for:

* Log shipping
* Monitoring agents
* Metrics exporters
* Service mesh proxies (e.g., Envoy)
* Configuration synchronization


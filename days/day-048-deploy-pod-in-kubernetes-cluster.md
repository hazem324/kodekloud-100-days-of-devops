#  Deploy Pods in Kubernetes Cluster

The Nautilus DevOps team is diving into Kubernetes for application management. As part of **Day 48**, a team member was assigned a task to create a Kubernetes Pod using the `httpd` image.

---

##  Task Details

* Create a pod named **`pod-httpd`**
* Use the **`httpd` image** with the **`latest` tag** (`httpd:latest`)
* Set the **app label** to `httpd_app`
* Name the container **`httpd-container`**

---

##  Steps

### 1. Create the Pod Manifest File

Create a YAML file named `pod-httpd.yml` on the jump host:

```sh
vi pod-httpd.yml
```

Add the following content:

[`pod-httpd.yml`](../files/k8s_pod-httpd_48d.yml)


### 2. Deploy the Pod to the Cluster

Run the following command to create the pod:

```sh
kubectl apply -f pod-httpd.yml
```

Expected output:

```text
pod/pod-httpd created
```

### 3. Verify Pod Status

Check if the pod is running:

```sh
kubectl get pods
```

Expected output:

```text
NAME        READY   STATUS    RESTARTS   AGE
pod-httpd   1/1     Running   0          <time>
```

---

##  Good to Know

###  Kubernetes Pods

* **Smallest Deployable Unit** in Kubernetes
* Can run **one or more containers**
* Containers share:

  * Network namespace (same IP & ports)
  * Storage volumes


###  Pod Lifecycle States

* **Pending** – Pod accepted but not scheduled
* **Running** – Pod is running successfully
* **Succeeded** – All containers exited successfully
* **Failed** – One or more containers failed
* **Unknown** – Status cannot be determined


###  YAML Manifest Structure

* **apiVersion** – Kubernetes API version
* **kind** – Resource type (Pod)
* **metadata** – Name and labels
* **spec** – Desired state of the pod


###  Common kubectl Commands

* Create / Update:

  ```sh
  kubectl apply -f pod-httpd.yml
  ```
* List pods:

  ```sh
  kubectl get pods
  ```
* Describe pod:

  ```sh
  kubectl describe pod pod-httpd
  ```
* View logs:

  ```sh
  kubectl logs pod-httpd
  ```
* Delete pod:

  ```sh
  kubectl delete pod pod-httpd
  ```

# Persistent Volumes with Nginx on Kubernetes

The Nautilus DevOps team is designing a Kubernetes template to deploy a web application that requires persistent storage for application data.

In this task, we manually configure storage using a `PersistentVolume`, bind it using a `PersistentVolumeClaim`, mount it inside an `nginx` container, and expose the application using a `NodePort` service.

---

##  Objectives

* Create a `PersistentVolume` named **pv-devops**
* Create a `PersistentVolumeClaim` named **pvc-devops**
* Create a `Pod` named **pod-devops** using image `nginx:latest`
* Mount the persistent storage at `/usr/share/nginx/html`
* Expose the application using a `NodePort` service named **web-devops** on port `30008`


---

#  Steps


## 1. Create the PersistentVolume

Create file: [`pv.yaml`](../files/k8s_pv_d60.yml)

### Apply the PV:

```bash
kubectl apply -f pv.yaml
```

### Verify:

```bash
kubectl get pv
```

## 2. Create the PersistentVolumeClaim

Create file: [`pvc.yaml`](../files/k8s_pvc_d60.yml)

### Apply the PVC:

```bash
kubectl apply -f pvc.yaml
```

### Verify:

```bash
kubectl get pvc
```
 PVC automatically binds to `pv-devops` because:

* Same `storageClassName`
* Requested size (1Gi) ≤ PV capacity (3Gi)
* Same access mode

## 3. Create the Pod and Mount the Volume

Create file: `pod.yaml`

### Apply the Pod:

```bash
kubectl apply -f pod.yaml
```

### Verify:

```bash
kubectl get pods
```
 Why mount at `/usr/share/nginx/html`?
Because this is the default document root for `nginx`. Any file inside `/mnt/dba` on the node will be served as web content.

## 4. Create the NodePort Service

Create file: [`service.yaml`](../files/k8s_service_d60.yml)

### Apply the Service:

```bash
kubectl apply -f service.yaml
```

### Verify:

```bash
kubectl get svc
```

---

#  Good to Know


##  PersistentVolume (PV)

* Represents physical storage in the cluster
* Cluster-scoped resource
* Independent of Pod lifecycle
* In this task, we used `hostPath` (local node storage)
* Capacity: `3Gi`


##  PersistentVolumeClaim (PVC)

* A request for storage by an application
* Binds automatically to a matching PV
* Decouples storage from application definition
* Request: `1Gi`

##  Volume Mounting

* A `volume` connects the Pod to the PVC
* A `volumeMount` attaches storage to a container path
* In this setup:

```
Node: /mnt/dba
        ↓
Container: /usr/share/nginx/html
```

This ensures:

* Data persists even if Pod restarts
* Storage survives container recreation


##  Access Modes

* `ReadWriteOnce (RWO)` → Mounted by one node
* `ReadOnlyMany (ROX)` → Multiple nodes, read-only
* `ReadWriteMany (RWX)` → Multiple nodes, read-write


##  hostPath 

* Uses local node filesystem
* Suitable for:

  * Labs
  * Testing
* Not recommended for production
* In production, use:

  * NFS
  * Cloud block storage
  * Distributed storage systems

---

##  Storage Lifecycle

| Action     | Result                       |
| ---------- | ---------------------------- |
| Delete Pod | Data remains                 |
| Delete PVC | PV becomes Released          |
| Delete PV  | Storage removed from cluster |


#  Validation Checklist

```bash
kubectl get pv
kubectl get pvc
kubectl get pods
kubectl get svc
```


#  Architecture Overview

```
Browser
   ↓
NodeIP:30008
   ↓
Service (web-devops)
   ↓
Pod (pod-devops)
   ↓
nginx:latest
   ↓
Mounted Volume
   ↓
PVC (pvc-devops)
   ↓
PV (pv-devops)
   ↓
Node Path (/mnt/dba)
```


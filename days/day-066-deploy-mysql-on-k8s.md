# Deploy MySQL on Kubernetes

A new MySQL server needs to be deployed on a Kubernetes cluster. The deployment includes persistent storage, secrets management, and service exposure.

---

# Architecture Overview

```
PersistentVolume → PersistentVolumeClaim → MySQL Pod
Secrets → Environment Variables → Container
NodePort Service → External Access (30007)
```

---

# Steps

## 1. Create PersistentVolume

Create file:

```bash
vi mysql-pv.yml
```

Add the following content: [`mysql-pv file`](../files/k8s_mysql_pv_d66.yml)

Apply:

```bash
kubectl apply -f mysql-pv.yml
```

## 2. Create PersistentVolumeClaim

Create file:

```bash
vi mysql-pvc.yml
```

Add the following content: [`mysql-pvc file`](../files/k8s_mysql_pvc_d66.yml)

Apply:

```bash
kubectl apply -f mysql-pvc.yml
```

Verify binding:

```bash
kubectl get pv
kubectl get pvc
```

## 3. Create Secrets

Create file:

```bash
vi mysql-secrets.yml
```

Add the following content: [`mysql-secrets file`](../files/k8s_mysql_secrets_d66.yml)

Apply:

```bash
kubectl apply -f mysql-secrets.yml
```

Verify:

```bash
kubectl get secrets
```

## 4. Create MySQL Deployment

Create file:

```bash
vi mysql-deployment.yml
```

Add the following content: [`mysql-deployment file`](../files/k8s_mysql_deployment_d66.yml)

Apply:

```bash
kubectl apply -f mysql-deployment.yml
```

## 5. Create NodePort Service

Create file:

```bash
vi mysql-service.yml
```

Add the following content: [`mysql-service file`](../files/k8s_mysql_service_d66.yml)

Apply:

```bash
kubectl apply -f mysql-service.yml
```

# Final Verification

Run:

```bash
kubectl get all
kubectl get pv
kubectl get pvc
kubectl get secrets
```

Everything should be:

* PV → Bound
* PVC → Bound
* Pod → Running
* Service → NodePort 30007

---

# Good to Know

##  Secrets Best Practice

* Never store passwords directly in deployment YAML.
* Use `stringData` for easier secret creation.
* Secrets are Base64 encoded (not encrypted by default).
* Enable encryption at rest for production clusters.


##  Persistent Storage Notes

* MySQL stores data in `/var/lib/mysql`
* If volume is not mounted → data loss on Pod restart
* `Retain` policy preserves data even if PVC is deleted


##  NodePort Info

* Default NodePort range: `30000–32767`
* Accessible via:

```
<NodeIP>:30007
```

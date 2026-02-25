# Manage Secrets in Kubernetes

The Nautilus DevOps team needs to deploy license-based tools inside a Kubernetes cluster. Since license numbers are sensitive information, they must be stored securely using **Kubernetes Secrets** instead of hardcoding them in manifests or images.

---

##  Requirements

* A secret key file `news.txt` already exists under `/opt` on the jump host.
* Create a generic secret named `news` using the content of `/opt/news.txt`.
* Create a pod named `secret-xfusion`.
* Configure the container as:

  * Name: `secret-container-xfusion`
  * Image: `ubuntu:latest`
  * Use `sleep` command to keep the container running.
* Mount the secret inside the container at `/opt/demo`.
* Verify the secret inside the running container.

---

#  Steps


## 1️. Create the Kubernetes Secret

Create a generic secret from the existing file:

```sh
kubectl create secret generic news --from-file=/opt/news.txt
```

### Verify Secret

```sh
kubectl get secret news
kubectl describe secret news
```

You should see the secret listed with type `Opaque`.

---

## 2️. Create the Pod YAML File

Create a file named: [`secret-xfusion.yml`](../files/k8s_secret-xfusion_d62.yml)

Add the following content:

## 3️. Deploy the Pod

```sh
kubectl apply -f secret-xfusion.yaml
```

---

## 4️. Verify Pod Status

```sh
kubectl get pods
```

## 5️. Verify Secret Inside Container

Access the container:

```sh
kubectl exec -it secret-xfusion -- bash
```

Check mounted secret:

```sh
ls /opt/demo
```
[![Verify Pod Status](../screenshots/Screenshot-day-62-verify-secret.png)](../screenshots/Screenshot-day-62-verify-secret.png)

---

#  Good to Know

---

##  What Is a Kubernetes Secret?

Kubernetes Secrets are objects used to store sensitive information such as:

* Passwords
* API keys
* Tokens
* License numbers

---

##  Secret Characteristics

* **Base64 Encoded**: Data is encoded (not encrypted by default).
* **Namespace Scoped**: Secrets belong to a specific namespace.
* **Stored in etcd**: Saved inside Kubernetes data store.
* **Must Be Explicitly Mounted**: Not automatically available in pods.

---

##  Secret Types

| Type                    | Purpose                           |
| ----------------------- | --------------------------------- |
| `generic`               | Arbitrary user-defined data       |
| `docker-registry`       | Docker registry credentials       |
| `tls`                   | TLS certificates and private keys |
| `service-account-token` | Auto-generated service tokens     |

---

##  Ways to Use Secrets

1. **Volume Mounts** (Used in this task)

   * Mounted as files inside container filesystem.

2. **Environment Variables**

   * Inject secret values as container env variables.

3. **Image Pull Secrets**

   * Authenticate to private container registries.

---

##  Security Best Practices

* Enable **RBAC** to restrict secret access.
* Enable **encryption at rest** for etcd.
* Rotate secrets regularly.
* Follow **least privilege principle**.
* Consider external secret managers like:

  * HashiCorp Vault
  * AWS Secrets Manager
  * Azure Key Vault
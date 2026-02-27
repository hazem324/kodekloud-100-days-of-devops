# Fix Python App Deployed on Kubernetes Cluster

A Python Flask application was deployed on a Kubernetes cluster but was not accessible. The deployment and service already existed, however the application was not coming up due to configuration errors.

### Environment Details

* Deployment name: `python-deployment-xfusion`
* Image (correct): `poroko/flask-demo-app`
* Required `nodePort`: `32345`
* Required `targetPort`: `5000` (Flask default port)
* `kubectl` configured on jump host

---

# Steps


## 1. Check Pod Status

```sh
kubectl get pods
kubectl describe pod python-deployment-xfusion-xxxxx
```

### Observed Error

```output
Reason: ImagePullBackOff
Failed to pull image "poroko/flask-app-demo"
repository does not exist
```

 The deployment was using the wrong Docker image.

## 2. Fix Wrong Docker Image

Export deployment YAML:

```sh
kubectl get deploy python-deployment-xfusion -o yaml > deployment.yaml
```

Edit the file:

```sh
vi deployment.yaml
```

Replace:

```yaml
image: poroko/flask-app-demo
```

With:

```yaml
image: poroko/flask-demo-app
```

Recreate deployment:

```sh
kubectl delete deploy python-deployment-xfusion
kubectl apply -f deployment.yaml
```

Verify:

```sh
kubectl get pods
```

Wait until:

```output
1/1 Running
```

## 3. Verify Service Configuration

Check service:

```sh
kubectl get svc
kubectl describe svc python-service-xfusion
```

### Observed Misconfiguration

```output
Port:        8080/TCP
TargetPort:  8080/TCP
NodePort:    32345/TCP
```

 Problem: Flask default port is **5000**, but service was forwarding to **8080**.


## 4. Fix Service Target Port

Export service YAML:

```sh
kubectl get svc python-service-xfusion -o yaml > service.yaml
```

Edit:

```sh
vi service.yaml
```

Modify:

```yaml
ports:
  - port: 8080
    targetPort: 5000
    nodePort: 32345
```

Apply changes:

```sh
kubectl apply -f service.yaml
```

Verify:

```sh
kubectl describe svc python-service-xfusion
```

## 5. Test Application Access

Get node IP:

```sh
kubectl get nodes -o wide
```

Test:

```sh
curl http://<NODE-IP>:32345
```

---

# Good to Know 


##  Image Pull Issues

If you see:

```
ImagePullBackOff
ErrImagePull
```

Check:

* Image name spelling
* DockerHub repository existence
* Private registry authentication
* Network connectivity from node

---

##  Port Mapping Logic (Important)

Traffic flow in NodePort service:

```
User → NodeIP:32345
        ↓
Service Port (8080)
        ↓
TargetPort (5000)
        ↓
Container (5000)
```

If `targetPort` does not match the container's listening port, the app will not respond.


##  Label & Selector Matching

Service must match deployment labels:

```yaml
selector:
  app: python_app
```

Verify:

```sh
kubectl get pods --show-labels
```

Mismatch = no endpoints = no traffic.


##  Production Debugging Workflow

Always follow this order:

```sh
kubectl get deploy
kubectl get pods
kubectl describe pod <pod>
kubectl logs <pod>
kubectl get svc
kubectl describe svc <svc>
kubectl get endpoints
```
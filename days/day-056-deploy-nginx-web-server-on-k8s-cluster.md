# Deploy Nginx Web Server on Kubernetes Cluster

The Nautilus development team is building a static website and requires it to be deployed on a Kubernetes cluster with high availability and scalability.

To meet these requirements, we created:

- A Deployment with 3 replicas using `nginx:latest`
- A NodePort Service exposing the application on port `30011`

---

## Steps

### 1. Create Deployment

[Deployment File](../files/k8s_deploy_nginx_d56.yml)
Apply the deployment configuration:

```sh
kubectl apply -f nginx-deployment.yml
````

### 2. Verify Deployment

```sh
kubectl get deployment nginx-deployment
kubectl get pods
```
[![Verify Deployment](../screenshots/Screenshot-day-56-verify-deployment.png)](../screenshots/Screenshot-day-56-verify-deployment.png)

All 3 replicas should be running successfully.


### 3. Create NodePort Service

[Service File](../files/k8s_nginx_service_d56.yml)
Apply the service configuration:

```sh
kubectl apply -f nginx-service.yml
```

### 4. Verify Service

```sh
kubectl get svc
```
[![Verify Service](../screenshots/Screenshot-day-56-verify-service.png)](../screenshots/Screenshot-day-56-verify-service.png)

#  Good to Know

##  Deployment Controller

* Ensures the desired number of replicas is always running
* Automatically replaces failed pods
* Supports rolling updates and rollback
* Maintains application availability during updates


##  Labels & Selectors

* Labels are key-value pairs attached to resources
* Services use selectors to target pods
* Deployment selector must match pod template labels
* If labels mismatch → Service endpoints will be empty


##  Kubernetes Service Types

| Type         | Usage                                  |
| ------------ | -------------------------------------- |
| ClusterIP    | Internal communication inside cluster  |
| NodePort     | Exposes service externally via node IP |
| LoadBalancer | Integrates with cloud provider LB      |
| ExternalName | Maps service to external DNS           |


##  NodePort Deep Dive

* Default port range: `30000–32767`
* Accessible on every node IP
* Acts as a basic external exposure method
* Often used for testing and lab environments


##  High Availability Strategy Used

* 3 replicas ensure redundancy
* Service distributes traffic across pods
* If one pod fails, traffic continues to others
* Kubernetes self-healing replaces unhealthy pods automatically
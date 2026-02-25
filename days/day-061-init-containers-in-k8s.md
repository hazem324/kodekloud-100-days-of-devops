# Init Containers in Kubernetes

Some workloads require preparation steps before the main application starts.
Instead of modifying the container image itself, Kubernetes **Init Containers** allow us to execute initialization logic during Pod startup.

In this exercise, we deploy a workload where:

* An **init container** creates a file inside a shared volume.
* The **main container** continuously reads and prints the content of that file.
* A shared `emptyDir` volume enables communication between both containers.

---

## Deployment Requirements

* Deployment name: `ic-deploy-datacenter`
* Replicas: `1`
* App label: `ic-datacenter`
* Init container:

  * Name: `ic-msg-datacenter`
  * Image: `ubuntu:latest`
  * Command:
    `/bin/bash -c "echo Init Done - Welcome to xFusionCorp Industries > /ic/official"`
  * Mount path: `/ic`
* Main container:

  * Name: `ic-main-datacenter`
  * Image: `ubuntu:latest`
  * Command:
    `/bin/bash -c "while true; do cat /ic/official; sleep 5; done"`
  * Mount path: `/ic`
* Volume:

  * Name: `ic-volume-datacenter`
  * Type: `emptyDir`

---

# Steps

### 1. Create the Deployment YAML

Create a file:

```bash
vi ic-deploy-datacenter.yaml
```
Create file: [`ic-deploy-datacenter.yaml`](../files/k8s_ic_deploy_datacenter_d61.yml)

### 2. Apply the Deployment

```bash
kubectl apply -f ic-deploy-datacenter.yaml
```

### 3. Verify Resources

Check deployment:

```bash
kubectl get deployment ic-deploy-datacenter
```

Verify init container completed successfully:

```bash
kubectl describe pod <pod-name>
```

---

# Good to Know

##  Why Init Containers?

Init containers allow separation between:

* Initialization logic
* Application runtime logic

They guarantee that required setup tasks complete before the main container starts.


##  How They Work

* Run **before** application containers
* Execute sequentially
* Must exit with code `0`
* If they fail → Pod stays in `Init` state
* Share volumes with main containers
* Share network namespace


##  emptyDir Volume Behavior

* Created when Pod starts
* Deleted when Pod is removed
* Accessible by all containers in the Pod
* Ideal for temporary shared data

In this task, it acts as a communication bridge between init and main container.


##  Real Production Use Cases

Init containers are commonly used for:

* Waiting for database readiness
* Running schema migrations
* Injecting configuration files
* Downloading artifacts before startup
* Generating certificates
* Setting file permissions


##  Troubleshooting Tips

If Pod is stuck in:

```
Init:CrashLoopBackOff
```

Check init container logs:

```bash
kubectl logs <pod-name> -c ic-msg-datacenter
```
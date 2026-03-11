# Day 61 - Init Containers in Kubernetes

## Task Description

Create a Deployment that uses an init container to generate configuration data before the main application container starts.

## Requirements

### Deployment

* Name:

```
ic-deploy-datacenter
```

* Replicas:

```
1
```

* Labels:

```
app=ic-datacenter
```

* Ensure both deployment selector and pod template labels match `app=ic-datacenter`.

### Init Container

* Name:

```
ic-msg-datacenter
```

* Image:

```
ubuntu:latest
```

* Command:

```
'/bin/bash', '-c', 'echo Init Done - Welcome to xFusionCorp Industries > /ic/official'
```

* Volume mount:

```
Name: ic-volume-datacenter
Mount Path: /ic
```

### Main Container

* Name:

```
ic-main-datacenter
```

* Image:

```
ubuntu:latest
```

* Command:

```
'/bin/bash', '-c', 'while true; do cat /ic/official; sleep 5; done'
```

* Volume mount:

```
Name: ic-volume-datacenter
Mount Path: /ic
```

### Volume

* Name:

```
ic-volume-datacenter
```

* Type:

```
emptyDir
```

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* Deployment `ic-deploy-datacenter` is created with one replica.
* Init container runs first and creates the file `/ic/official`.
* Main container continuously reads and outputs the file content.
* Pod remains in a running state with the shared `emptyDir` volume.

End of Task

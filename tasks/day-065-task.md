# Redis Deployment with ConfigMap and Volumes

## Task Description

Deploy a Redis instance for testing with a custom memory configuration, resource requests, and required volume mounts.

## Requirements

### ConfigMap

* Name:

```
my-redis-config
```

* Key:

```
redis-config
```

* Value:

```
maxmemory 2mb
```

### Deployment

* Name:

```
redis-deployment
```

* Replicas:

```
1
```

* Image:

```
redis:alpine
```

* Container name:

```
redis-container
```

* Resource request:

```
CPU: 1
```

* Exposed container port:

```
6379
```

### Volumes

1. Data Volume

   * Name:

```
data
```

* Type:

```
emptyDir
```

* Mount path:

```
/redis-master-data
```

2. Config Volume

   * Name:

```
redis-config
```

* Type:

```
ConfigMap
```

* Source ConfigMap:

```
my-redis-config
```

* Mount path:

```
/redis-master
```

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* ConfigMap `my-redis-config` is created with the specified configuration.
* Deployment `redis-deployment` runs a single replica.
* Redis container starts successfully using `redis:alpine`.
* Both volumes are mounted correctly.
* Pod is in running state and listening on port `6379`.

End of Task

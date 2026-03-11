# Day 67 - Deploy Guest Book App on Kubernetes

## Task Description

Deploy a three-tier Guestbook application consisting of Redis master, Redis slave, and frontend components, and expose the frontend via NodePort.

---

# Back-End Tier

## Redis Master Deployment

* Name:

```
redis-master
```

* Replicas:

```
1
```

* Container name:

```
master-redis-datacenter
```

* Image:

```
redis
```

* Resource requests:

  * CPU:

```
100m
```

* Memory:

```
100Mi
```

* Container port:

```
6379
```

## Redis Master Service

* Name:

```
redis-master
```

* Port:

```
6379
```

* TargetPort:

```
6379
```

---

## Redis Slave Deployment

* Name:

```
redis-slave
```

* Replicas:

```
2
```

* Container name:

```
slave-redis-datacenter
```

* Image:

```
gcr.io/google_samples/gb-redisslave:v3
```

* Resource requests:

  * CPU:

```
100m
```

* Memory:

```
100Mi
```

* Environment variable:

```
GET_HOSTS_FROM=dns
```

* Container port:

```
6379
```

## Redis Slave Service

* Name:

```
redis-slave
```

* Port:

```
6379
```

* TargetPort:

```
6379
```

---

# Front-End Tier

## Frontend Deployment

* Name:

```
frontend
```

* Replicas:

```
3
```

* Container name:

```
php-redis-datacenter
```

* Image:

```
gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
```

* Resource requests:

  * CPU:

```
100m
```

* Memory:

```
100Mi
```

* Environment variable:

```
GET_HOSTS_FROM=dns
```

* Container port:

```
80
```

## Frontend Service

* Name:

```
frontend
```

* Type:

```
NodePort
```

* Port:

```
80
```

* NodePort:

```
30009
```

* Use any appropriate labels for selectors.
* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* Redis master and slave deployments are running with required replicas.
* All services expose correct ports internally.
* Frontend deployment runs with 3 replicas.
* Frontend service exposes the application on NodePort `30009`.
* Guestbook application is accessible and functional.

End of Task

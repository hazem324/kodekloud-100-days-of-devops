# Day 63 - Deploy Iron Gallery App on Kubernetes

## Task Description

Deploy the Iron Gallery application and its database within a dedicated namespace, configure required volumes and resources, and expose services appropriately.

## Requirements

### Namespace

* Create namespace:

```
iron-namespace-devops
```

---

## Iron Gallery Deployment

### Deployment

* Name:

```
iron-gallery-deployment-devops
```

* Namespace:

```
iron-namespace-devops
```

* Replicas:

```
1
```

* Labels:

```
run=iron-gallery
```

* Selector matchLabels:

```
run=iron-gallery
```

* Template metadata labels:

```
run=iron-gallery
```

### Container

* Name:

```
iron-gallery-container-devops
```

* Image:

```
kodekloud/irongallery:2.0
```

* Resource limits:

  * Memory:

```
100Mi
```

* CPU:

```
50m
```

### Volumes

* Volume 1:

  * Name:

```
config
```

* Type:

```
emptyDir
```

* Mount path:

```
/usr/share/nginx/html/data
```

* Volume 2:

  * Name:

```
images
```

* Type:

```
emptyDir
```

* Mount path:

```
/usr/share/nginx/html/uploads
```

---

## Iron DB Deployment

### Deployment

* Name:

```
iron-db-deployment-devops
```

* Namespace:

```
iron-namespace-devops
```

* Replicas:

```
1
```

* Labels:

```
db=mariadb
```

* Selector matchLabels:

```
db=mariadb
```

* Template metadata labels:

```
db=mariadb
```

### Container

* Name:

```
iron-db-container-devops
```

* Image:

```
kodekloud/irondb:2.0
```

### Environment Variables

* Database name:

```
database_blog
```

* Define:

  * `MYSQL_ROOT_PASSWORD` with a strong complex password
  * `MYSQL_PASSWORD` with a strong complex password
  * `MYSQL_USER` with a custom non-root username

### Volume

* Name:

```
db
```

* Type:

```
emptyDir
```

* Mount path:

```
/var/lib/mysql
```

---

## Iron DB Service

* Name:

```
iron-db-service-devops
```

* Namespace:

```
iron-namespace-devops
```

* Type:

```
ClusterIP
```

* Selector:

```
db=mariadb
```

* Protocol:

```
TCP
```

* Port:

```
3306
```

* TargetPort:

```
3306
```

---

## Iron Gallery Service

* Name:

```
iron-gallery-service-devops
```

* Namespace:

```
iron-namespace-devops
```

* Type:

```
NodePort
```

* Selector:

```
run=iron-gallery
```

* Protocol:

```
TCP
```

* Port:

```
80
```

* TargetPort:

```
80
```

* NodePort:

```
32678
```

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* Namespace `iron-namespace-devops` exists.
* Both deployments are running with one replica each.
* Required volumes are mounted correctly.
* Database service is accessible internally via ClusterIP on port 3306.
* Iron Gallery application is accessible externally via NodePort 32678.
* Installation page of Iron Gallery loads successfully.

End of Task

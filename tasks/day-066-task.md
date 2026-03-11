# Day 66 - Deploy MySQL on Kubernetes

## Task Description

Deploy a MySQL instance on Kubernetes with persistent storage and secure credential management using Secrets.

## Requirements

### PersistentVolume

* Name:

```
mysql-pv
```

* Capacity:

```
250Mi
```

* Configure other parameters as appropriate.
* Volume should support mounting at:

```
/var/lib/mysql
```

### PersistentVolumeClaim

* Name:

```
mysql-pv-claim
```

* Requested storage:

```
250Mi
```

* Bind to `mysql-pv`.

---

### Secrets

1. Secret:

```
mysql-root-pass
```

* Key:

```
password
```

* Value:

```
YUIidhb667
```

2. Secret:

```
mysql-user-pass
```

* Keys and values:

  * `username`:

```
kodekloud_roy
```

* `password`:

```
ksH85UJjhb
```

3. Secret:

```
mysql-db-url
```

* Key:

```
database
```

* Value:

```
kodekloud_db9
```

---

### Deployment

* Name:

```
mysql-deployment
```

* Use any valid MySQL image.
* Mount PersistentVolumeClaim:

```
mysql-pv-claim
```

* Mount path:

```
/var/lib/mysql
```

#### Environment Variables (from Secrets)

* `MYSQL_ROOT_PASSWORD`

  * From:

```
mysql-root-pass
```

* Key:

```
password
```

* `MYSQL_DATABASE`

  * From:

```
mysql-db-url
```

* Key:

```
database
```

* `MYSQL_USER`

  * From:

```
mysql-user-pass
```

* Key:

```
username
```

* `MYSQL_PASSWORD`

  * From:

```
mysql-user-pass
```

* Key:

```
password
```

---

### Service

* Name:

```
mysql
```

* Type:

```
NodePort
```

* NodePort:

```
30007
```

* Perform all operations using the `kubectl` utility configured on `jump_host`.

## Expected Result

* PersistentVolume `mysql-pv` is created and bound to `mysql-pv-claim`.
* Secrets are created with correct key-value pairs.
* Deployment `mysql-deployment` runs successfully with mounted storage.
* Environment variables are sourced securely from Secrets.
* NodePort service `mysql` exposes the database on port `30007`.
* MySQL pod is in running state.

End of Task

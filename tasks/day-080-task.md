# Day 80 - Jenkins Chained Builds

## Task Description

Configure Jenkins chained jobs to deploy application updates and trigger a downstream job to restart Apache only after a successful deployment.

## Requirements

### Access Jenkins

* Open Jenkins from the top navigation.
* Login using:

```
Username: admin
Password: Adm!n321
```

### Access Gitea

* Open Gitea from the top navigation or port 3000.
* Login using:

```
Username: sarah
Password: Sarah_pass123
```

* Repository:

```
web
```

---

## Job 1: Deployment

### Job

* Name:

```
datacenter-app-deployment
```

### Configuration

* Pull latest changes from:

```
master branch
```

* Repository location on App Server 1:

```
/var/www/html
```

* Ensure deployment runs with sufficient privileges (sudo if required).
* The job should update the working directory with the latest repository content.

---

## Job 2: Service Management (Downstream)

### Job

* Name:

```
manage-services
```

### Configuration

* Restart Apache service on:

```
App Server 1
```

* Service:

```
httpd
```

### Trigger Condition

* Configure as downstream job of:

```
datacenter-app-deployment
```

* Trigger only when upstream job status is:

```
Stable (successful)
```

---

## Application Access

* Load balancer URL:

```
http://<LBR-URL>
```

* Application must load directly from root (no subdirectory).

---

## Plugin Requirement

* Install any required plugins for chained jobs if not already available.
* Restart Jenkins if prompted after plugin installation.

---

## Expected Result

* Job `datacenter-app-deployment` pulls latest code from the master branch.
* Job `manage-services` is triggered automatically after successful deployment.
* Apache (`httpd`) service is restarted on App Server 1.
* Application reflects latest changes on the main URL.
* Both jobs execute successfully on repeated runs.

End of Task

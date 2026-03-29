# Day 81 - Jenkins Multistage Pipeline

## Task Description

Create a Jenkins pipeline to deploy a static website to the application server and validate its availability after deployment.

## Requirements

### Access Jenkins

* Open Jenkins from the top navigation.
* Login using:

```
Username: admin
Password: Adm!n321
```

### Access Gitea

* Open Gitea from the top navigation.
* Login using:

```
Username: sarah
Password: Sarah_pass123
```

* Repository:

```
sarah/web
```

* Location on App Server 1:

```
/var/www/html
```

---

## Code Update

* Update file:

```
/var/www/html/index.html
```

* Content:

```
Welcome to xFusionCorp Industries
```

* Push changes to:

```
master branch
```

---

## Jenkins Agent Configuration

* Name:

```
App Server 1
```

* Label:

```
stapp01
```

* Remote root directory:

```
/home/sarah/jenkins_agent
```

* Connection:

```
SSH (user: sarah)
```

* Ensure Java is installed:

```
java-17-openjdk
```

---

## Jenkins Pipeline Job

### Job

* Name:

```
deploy-job
```

* Type:

```
Pipeline (not Multibranch)
```

### Execution Node

* Run pipeline on:

```
stapp01
```

### Stages

#### Deploy

* Deploy repository content to:

```
/var/www/html
```

* Ensure full repository content is deployed.

#### Test

* Verify application accessibility using:

```
http://stlb01:8091
```

* Ensure stage fails if:

  * Deployment fails
  * Website is not accessible

---

## Application Access

* URL:

```
http://stlb01:8091
```

* Application must load from root (no subdirectory).

---

## Plugin Requirement

* Install required plugins if prompted.
* Restart Jenkins if necessary.

---

## Documentation

* Capture screenshots or record configuration steps for verification.

## Expected Result

* Jenkins agent `App Server 1` is configured and online.
* Pipeline job `deploy-job` is created with stages `Deploy` and `Test`.
* Code is successfully deployed to `/var/www/html`.
* Website is accessible at the main URL.
* Test stage validates application availability.
* Pipeline succeeds only if deployment and validation are successful.

End of Task

# Day 78 - Jenkins Deployment Job

## Task Description

Configure a Jenkins job to automatically deploy application updates to the app server whenever changes are pushed to the master branch.

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
web
```

* Local path on App Server 1:

```
/home/sarah/web
```

---

## Server Preparation

* Ensure Apache service is installed and running on:

```
App Server 1 (port 8080)
```

* Ensure correct ownership of deployment directory:

```
/var/www/html → user: sarah
```

---

## Jenkins Job

### Job

* Name:

```
devops-app-deployment
```

### Trigger

* Configure automatic trigger on:

```
Git push to master branch
```

### Deployment Logic

* Source repository path:

```
/home/sarah/web
```

* Deploy entire repository content to:

```
/var/www/html
```

* Ensure deployment is executed with sufficient privileges (sudo if required).
* Optionally include Apache restart as part of deployment.

---

## Code Update

* Modify file:

```
/home/sarah/web/index.html
```

* Update content:

```
Welcome to the xFusionCorp Industries
```

* Commit and push changes to:

```
master branch
```

* Ensure this push triggers the Jenkins job automatically.

---

## Application Access

* Verify application via:

```
http://<LBR-URL>
```

* Application must load directly from root (no subdirectory).

---

## Plugin Requirement

* Install required plugins for Git integration and webhook triggering if needed.
* Restart Jenkins if prompted.

---

## Expected Result

* Jenkins job `devops-app-deployment` is created.
* Job is triggered automatically on push to master branch.
* Full repository content is deployed to `/var/www/html`.
* Apache serves updated content successfully.
* Application reflects latest changes at the main URL.
* Job executes successfully on repeated runs.

End of Task

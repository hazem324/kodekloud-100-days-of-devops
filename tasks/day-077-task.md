# Day 77 - Jenkins Deploy Pipeline

## Task Description

Create a Jenkins pipeline to deploy a static website from a repository to the application server document root.

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
web_app
```

---

## Jenkins Agent Configuration

### Agent

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

---

## Repository Information

* Repository name:

```
web_app
```

* Location on server:

```
/var/www/html
```

---

## Jenkins Pipeline Job

### Job

* Name:

```
xfusion-webapp-job
```

* Type:

```
Pipeline (not Multibranch)
```

### Pipeline Stage

* Stage name:

```
Deploy
```

### Deployment Target

* Application document root:

```
/var/www/html
```

### Server Configuration

* Apache already installed and running on:

```
Port 8080
```

---

## Application Access

* Load balancer URL:

```
https://<LBR-URL>
```

* Application must load directly from the root URL without a subdirectory.

---

## Plugin Requirement

* Install any required Jenkins plugins if prompted.
* Restart Jenkins if necessary after plugin installation.

---

## Documentation

* Capture screenshots or record the configuration process for verification and review.

## Expected Result

* Jenkins agent `App Server 1` is configured and online.
* Pipeline job `xfusion-webapp-job` is created.
* Pipeline contains a single stage named `Deploy`.
* Code from the `web_app` repository is deployed to `/var/www/html`.
* Website loads successfully through the load balancer URL without a subdirectory.

End of Task

# day 78 - Jenkins Conditional Pipeline

## Task Description

Create a parameterized Jenkins pipeline that deploys a selected branch of a static website repository to the application server.

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

* Repository:

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
datacenter-webapp-job
```

* Type:

```
Pipeline (not Multibranch)
```

### Parameter

* Type:

```
String
```

* Name:

```
BRANCH
```

### Pipeline Stage

* Stage name:

```
Deploy
```

### Deployment Logic

* If parameter value is:

```
master
```

Deploy the **master** branch.

* If parameter value is:

```
feature
```

Deploy the **feature** branch.

* Deployment target:

```
/var/www/html
```

### Server Configuration

* Apache service:

```
Running on port 8080
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

* Install any required plugins if prompted.
* Restart Jenkins if necessary after plugin installation.

---

## Documentation

* Capture screenshots or record the configuration process for verification and review.

## Expected Result

* Jenkins agent `App Server 1` is configured and online.
* Pipeline job `datacenter-webapp-job` exists with parameter `BRANCH`.
* Pipeline contains a single stage named `Deploy`.
* Correct branch is deployed based on the parameter value.
* Website loads successfully via the load balancer URL without a subdirectory.

End of Task

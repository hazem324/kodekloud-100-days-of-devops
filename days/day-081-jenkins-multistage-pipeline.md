# Jenkins Multistage Pipeline

##  Task Overview

Access the required tools from the top bar:

* **Jenkins UI**

  * Username: `admin`
  * Password: `Adm!n321`

* **Gitea UI**

  * Username: `sarah`
  * Password: `Sarah_pass123`

A repository named `sarah/web` is already cloned on **App Server 1** under:

```bash
/var/www/html
```

---

## Objectives

1. Update the content of `index.html` to:

```text
Welcome to xFusionCorp Industries
```

2. Push the changes to the `master` branch in Gitea.

3. Configure **App Server 1** as a Jenkins agent:

   * Name: `App Server 1`
   * Label: `stapp01`
   * Remote directory: `/home/sarah/jenkins_agent`
   * Launch via SSH using user `sarah`

4. Create a Jenkins pipeline job:

   * Name: `deploy-job`
   * Type: Pipeline (NOT Multibranch)
   * Stages:

     * `Deploy`
     * `Test`

---

##  Pipeline Behavior

###  Deploy Stage

* Pull latest code from Gitea
* Update `/var/www/html` (Apache document root)

###  Test Stage

* Validate application availability using:

```bash
curl -f http://stlb01:8091
```

* Pipeline fails if:

  * Deployment fails
  * Website is not accessible

---

#  Steps

### 0. Prepare Environment

* Install required Jenkins plugins:

  * Pipeline
* Restart Jenkins if needed

### 1. Update Application Code

```bash
ssh sarah@stapp01
cd /var/www/html
vi index.html
```

Update content and push:

```bash
git add index.html
git commit -m "Update homepage content"
git push origin master
```

### 2. Install Java on App Server (if needed)

```bash
sudo yum install java-17-openjdk -y
```

### 3. Configure Jenkins Agent

* Go to: **Manage Jenkins → Nodes → New Node**
* Configure:

  * Name: `App Server 1`
  * Label: `stapp01`
  * Remote dir: `/home/sarah/jenkins_agent`
  * Launch via SSH
  * Credentials: `sarah / Sarah_pass123`

### 4. Create Pipeline Job

* Name: `deploy-job`
* Type: Pipeline

Paste this pipeline script: [`jenkinsfile`](../files/jenkins_multistage_pipeline_d81.jenkinsfile)

### 5. Run Pipeline

* Click **Build Now**
* Verify successful execution of both stages

---

##  Good to Know

### Pipeline-Based Deployment

* **Automated Deployment**: Code changes are deployed automatically via pipeline
* **Agent Execution**: Jobs run on remote nodes (App Server 1 in this case)
* **Environment Targeting**: Use labels (`stapp01`) to control where jobs execute
* **Consistency**: Same deployment steps executed every time

---

### Pipeline Stages

* **Deploy**: Pull latest code and update application files
* **Test**: Validate application accessibility after deployment
* **Sequential Flow**: Stages execute in order (Deploy → Test)
* **Failure Handling**: If Deploy fails, Test stage is skipped

---

### Deployment Strategy

* **Git-Based Deployment**: Application updated directly from repository
* **Hard Reset Strategy**: Ensures clean state using `git reset --hard`
* **No Build Required**: Static website → no compilation step needed
* **Direct Web Root Deployment**: Files deployed to `/var/www/html`

---

### Validation & Testing

* **Smoke Testing**: Quick check using `curl` to verify app is live
* **HTTP Failure Detection**: `curl -f` fails on bad responses (≥ 400)
* **Load Balancer Testing**: Validate via `http://stlb01:8091`
* **End-to-End Check**: Ensures full request path works (LB → App Server)

---

### Pipeline Design Principles

* **Single Responsibility**: Each stage has a clear purpose (Deploy or Test)
* **Fail Fast**: Stop pipeline immediately on errors
* **Idempotency**: Safe to run multiple times without side effects
* **Simplicity**: Minimal pipeline for faster execution and debugging

---

### Common Issues

* **Wrong Agent Label**: Pipeline won’t run on intended server
* **Agent Offline**: Job stuck or skipped
* **Permission Issues**: Cannot write to `/var/www/html`
* **Incorrect URL**: Test stage fails due to wrong endpoint

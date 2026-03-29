# Jenkins Chained Builds

This project demonstrates how to implement **Jenkins chained jobs** to automate deployment and service management in a DevOps environment.

The objective is to:

* Deploy updated code from a Git repository
* Restart Apache service **only if deployment succeeds**
* Execute everything on **App Server 1 using a Jenkins agent**

---

##  Access Details

* **Jenkins**

  * URL: `http://<jenkins-url>`
  * Username: `admin`
  * Password: `Adm!n321`

* **Gitea**

  * URL: `http://<gitea-url>:3000`
  * Username: `sarah`
  * Password: `Sarah_pass123`

* Repository:

  ```
  web
  ```

---

#  Architecture Overview

```
Jenkins Master
      ↓ (SSH)
App Server 1 (Agent: stapp01)
      ↓
/var/www/html (local git repo)
      ↓
Apache (httpd)
```

---

#  Steps

## 1. Configure Jenkins Agent (App Server 1)

* Go to:

  ```
  Manage Jenkins → Manage Nodes → New Node
  ```

* Configure:

  * Name: `app-server`
  * Type: Permanent Agent
  * Remote root directory:

    ```
    /home/sarah
    ```
  * Label:

    ```
    app-server
    ```
  * Launch method: **via SSH**
  * Credentials:

    * User: `sarah`
    * Password: `Sarah_pass123`

 Ensure agent connects successfully

## 2. Create Job: `datacenter-app-deployment`

* Type: Freestyle Project

###  Restrict Execution

```
Restrict where this project can be run → app-server
```

###  Source Code Management

* Type: Git
* Repository:

```
/var/www/html
```

* Branch:

```
*/master
```

###  Build Step

```bash
cd /var/www/html

sudo git fetch --all
sudo git reset --hard origin/master
```

###  Post-Build Action (Chaining )

* Build other projects:

```
manage-services
```

* Trigger:
   Only if build is stable

---

## 3. Create Job: `manage-services`

* Type: Freestyle Project

###  Restrict Execution

```
app-server
```

###  Build Step

```bash
sudo systemctl restart httpd
```

 IMPORTANT:

* Do **NOT** configure “Build after other projects are built”
* Chaining is handled only from upstream job

## 4. Configure Sudo Access

On App Server 1:

```bash
sudo visudo
```

Add:

```bash
sarah ALL=(ALL) NOPASSWD: ALL
```

## 5. Fix Git & Permissions

```bash
sudo chown -R sarah:sarah /var/www/html

git config --global --add safe.directory /var/www/html
```

## 6. Run the Pipeline

 Build:

```
datacenter-app-deployment
```

### Expected Flow:

1. Code pulled from Git 
2. Deployment successful 
3. `manage-services` triggered 
4. Apache restarted 

## 7. Validate Application

Open:

```
http://stlb01:8091
```

 Application loads directly
 No `/web` in URL

---

#  Good to Know

##  Chained Builds

* **Upstream / Downstream**:
  `datacenter-app-deployment` → triggers → `manage-services`
* **Build Pipeline**:
  Deployment and service management are split into separate jobs
* **Conditional Trigger**:
  Downstream job runs **only if upstream is SUCCESS**
* **Single Trigger Strategy**:
  Chaining is configured only in the upstream job to avoid duplicate executions


##  Build Dependencies

* **Sequential Execution**:
  Jobs run in a strict order:
  `Deploy → Restart Service`
* **Dependency Management**:
  `manage-services` depends entirely on the success of deployment
* **Failure Handling**:
  If deployment fails, Apache restart is **not triggered**
* **Status Propagation**:
  Build result (SUCCESS/FAILURE) determines downstream execution


##  Post-Build Actions

* **Trigger Builds**:
  Upstream job uses *Post-build Action* to trigger `manage-services`
* **Conditional Execution**:
  Trigger happens only when build is **stable**
* **Automation Flow Control**:
  Eliminates manual intervention between stages


##  Pipeline Benefits

* **Separation of Concerns**:
  Deployment and service restart are isolated
* **Reusability**:
  `manage-services` job can be reused independently
* **Reliability**:
  Prevents unnecessary service restarts on failed deployments
* **Idempotency**:
  Safe to run multiple times due to clean Git reset strategy


##  Practical Insights (From This Setup)

* Avoid configuring triggers in **both jobs** → causes duplicate runs
* Always use **clean deployment (`git reset --hard`)** for repeatability
* Run jobs on **target node (agent)** for accurate execution environment
* Ensure **sudo without password** for automation stability

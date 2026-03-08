# Jenkins Scheduled Jobs

## Overview

The **DevOps team at xFusionCorp Industries** is preparing to implement a centralized logging system to simplify log monitoring and troubleshooting across multiple servers.

Until the centralized system is fully deployed, the team decided to **automate the collection of Apache logs** from the application server using **Jenkins scheduled jobs**.

A Jenkins job will periodically collect the following logs from **App Server 1**:

* `access_log`
* `error_log`

These logs will be transferred to the **Storage Server** so they can be reviewed if Apache-related issues occur.

---

# Environment

| Server         | Hostname        | Purpose                        | User      |
| -------------- | --------------- | ------------------------------ | --------- |
| Jenkins Server | `172.16.238.19` | Automation server              | `jenkins` |
| App Server 1   | `stapp01`       | Source of Apache logs          | `tony`    |
| Storage Server | `ststor01`      | Destination for collected logs | `natasha` |

Apache logs location on App Server:

```
/var/log/httpd/
```

Destination directory on Storage Server:

```
/usr/src/sysops
```

---

# Jenkins Login

Access Jenkins UI from the top bar.

Login using:

```
Username: admin
Password: Adm!n321
```

---

# Objective

Create a Jenkins job named:

```
copy-logs
```

The job must:

* Run **every 9 minutes**
* Copy Apache logs from **App Server 1**
* Store them on the **Storage Server**

---

# Steps

## 1. Connect to Jenkins Server

From the jump host connect to the Jenkins server:

```bash
ssh jenkins@172.16.238.19
```

## 2. Generate SSH Key

To enable secure automation, generate an SSH key for the Jenkins user.

```bash
ssh-keygen -t rsa
```

Default key location:

```
/var/lib/jenkins/.ssh/id_rsa
/var/lib/jenkins/.ssh/id_rsa.pub
```

## 3. Configure Passwordless SSH

Allow Jenkins to access both servers without password prompts.

### Copy key to App Server

```bash
ssh-copy-id tony@stapp01
```

### Copy key to Storage Server

```bash
ssh-copy-id natasha@ststor01
```

## 4. Verify SSH Connectivity

Ensure Jenkins can connect successfully.

```bash
ssh tony@stapp01
ssh natasha@ststor01
```

Exit after verification.

## 5. Create Jenkins Job

Open Jenkins UI.

1. Click **New Item**
2. Enter job name:

```
copy-logs
```

3. Select:

```
Freestyle Project
```

4. Click **OK**

## 6. Configure Job Trigger

Enable automatic scheduling.

Navigate to:

```
Build Triggers
```

Enable:

```
Build periodically
```

Add the cron expression:

```
*/9 * * * *
```

This schedules the job to run **every 9 minutes**.

## 7. Add Build Step

Add a build step:

```
Execute Shell
```

Add the following script:

```bash
scp tony@stapp01:/var/log/httpd/access_log .
scp tony@stapp01:/var/log/httpd/error_log .
scp access_log error_log natasha@ststor01:/usr/src/sysops/
```

## 8. Save the Job

Click:

```
Save
```

## 9. Test the Job

Run the job manually:

```
Build Now
```

Check the **Console Output**.

Expected result:

```
Finished: SUCCESS
```

## 10. Verify Logs on Storage Server

SSH to the storage server:

```bash
ssh natasha@ststor01
```

Check the directory:

```bash
ls /usr/src/sysops
```

Expected output:

```
access_log
error_log
```

---

# Workflow Diagram

```
App Server (stapp01)
      │
      │  SCP
      ▼
Jenkins Server
      │
      │  SCP
      ▼
Storage Server (ststor01)
/usr/src/sysops
```

---

# Good to Know

## Jenkins Scheduled Jobs

Jenkins allows jobs to run automatically using **cron expressions**.

This is useful for tasks like:

* Log collection
* Database backups
* Automated deployments
* System health checks

Scheduled automation reduces manual work and ensures consistent operations.


## Cron Expression Basics

Cron expressions follow this format:

```
minute hour day-of-month month day-of-week
```

Example:

```
*/9 * * * *
```

Meaning:

```
Every 9 minutes
```

Jenkins also supports a special character:

```
H
```

Example:

```
H/9 * * * *
```

This distributes jobs across different times to prevent server overload.


## Apache Log Files

Apache stores logs in:

```
/var/log/httpd/
```

Important log files include:

| Log File   | Description                        |
| ---------- | ---------------------------------- |
| access_log | Records all HTTP requests          |
| error_log  | Records server errors and warnings |

These logs help identify issues such as:

* Application errors
* Failed requests
* Security attacks
* Performance problems


## SCP (Secure Copy Protocol)

`scp` is used to securely transfer files between Linux servers.

Example:

```
scp source destination
```

Example used in this task:

```
scp access_log error_log natasha@ststor01:/usr/src/sysops/
```

This copies files to the **storage server** securely using SSH.


## Why Passwordless SSH is Important

Automation tools like Jenkins must run commands **without manual input**.

Passwordless SSH allows Jenkins to:

* connect to servers automatically
* execute scripts without interruption
* perform scheduled operations reliably

This is commonly used in:

* CI/CD pipelines
* configuration management
* server orchestration

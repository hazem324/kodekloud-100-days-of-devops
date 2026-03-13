# Jenkins Deploy Pipeline

The development team of **xFusionCorp Industries** is developing a new static website and plans to deploy it on a **Nautilus App Server** using a **Jenkins Pipeline**.

The repository containing the website code is hosted on **Gitea** and already cloned on **App Server 1** under `/var/www/html`. Our task is to configure Jenkins so that it can deploy the latest code automatically using a pipeline.

Click on the **Jenkins** button on the top bar to access the Jenkins UI. Login using:

```
Username: admin
Password: Adm!n321
```

Similarly, click on the **Gitea** button on the top bar and login using:

```
Username: sarah
Password: Sarah_pass123
```

Under user **sarah**, you will find the repository named:

```
web_app
```

This repository is already cloned on **App Server 1** under:

```
/var/www/html
```

Apache is already installed on the App Server and running on port:

```
8080
```

Our objective is to create a **Jenkins pipeline job** that deploys the code to the web server.

---

# Task Requirements

1. Add a Jenkins slave node named **App Server 1**.
2. Label the node as:

```
stapp01
```

3. Set the remote root directory:

```
/home/sarah/jenkins_agent
```

4. Create a Jenkins pipeline job named:

```
xfusion-webapp-job
```

5. The pipeline must contain a **single stage named `Deploy`** (case sensitive).

6. The pipeline should deploy the code from the repository located in:

```
/var/www/html
```

7. The Load Balancer server is already configured. The website should be accessible through:

```
https://<LBR-URL>
```

The page should load directly from the root URL, **not** from a subdirectory such as:

```
https://<LBR-URL>/web_app
```

---

# Steps

## 0. Login to App Server and Prepare Environment

SSH into the App Server:

```bash
ssh tony@stapp01
```

Install Java (required for Jenkins agent):

```bash
sudo yum install java-17-openjdk -y
```

Create Jenkins agent directory:

```bash
sudo mkdir -p /home/sarah/jenkins_agent
sudo chown -R tony:tony /home/sarah/jenkins_agent
sudo chmod 755 /home/sarah
```

Fix Git safe directory warning:

```bash
git config --global --add safe.directory /var/www/html
```

Fix repository permissions:

```bash
sudo chown -R tony:tony /var/www/html
```

# 1. Install Jenkins Plugins

Go to:

```
Manage Jenkins → Manage Plugins
```

Install the following plugins:

* Pipeline
* Pipeline: Nodes and Processes
* Pipeline: Stage View
* SSH Build Agents

Restart Jenkins after installation.

# 2. Add App Server Credentials

Navigate to:

```
Manage Jenkins → Credentials → System → Global Credentials → Add Credentials
```

Add:

```
Username: tony
Password: Ir0nM@n
ID: stapp01
```

Save.

# 3. Add Jenkins Agent (Slave Node)

Go to:

```
Manage Jenkins → Nodes → New Node
```

Configure the node:

| Setting               | Value                             |
| --------------------- | --------------------------------- |
| Node Name             | App Server 1                      |
| Remote Root Directory | /home/sarah/jenkins_agent         |
| Labels                | stapp01                           |
| Usage                 | Use this node as much as possible |

Launch Method:

```
Launch agents via SSH
```

SSH Configuration:

```
Host: stapp01.stratos.xfusioncorp.com
Credentials: tony / Ir0nM@n
Host Key Verification Strategy: Non verifying Verification Strategy
```

Save and ensure the node status becomes:

```
Online
```

# 4. Create Jenkins Pipeline Job

Go to:

```
Dashboard → New Item
```

Create job:

```
xfusion-webapp-job
```

Select:

```
Pipeline
```

Important: **Do NOT select Multibranch Pipeline**.


# 5. Configure Pipeline Script

Scroll to the **Pipeline** section and add the following script:

```groovy
pipeline {
    agent { label 'stapp01' }

    stages {
        stage('Deploy') {
            steps {
                sh '''
                cd /var/www/html
                git pull origin master
                '''
            }
        }
    }
}
```

Save the job.

# 6. Run the Pipeline

Click:

```
Build Now
```

Expected console output:

```
Running on App Server 1
[Pipeline] stage
Deploy
cd /var/www/html
git pull origin master
Already up to date.
Finished: SUCCESS
```

# 7. Verify Deployment

SSH into the App Server:

```bash
ssh tony@stapp01
```

Check the website file:

```bash
cat /var/www/html/index.html
```

Expected output:

```
Welcome to xFusionCorp Industries!
```

---

# Good to Know

## Jenkins Pipelines

* **Pipeline as Code** – CI/CD pipelines defined using code
* **Declarative Pipeline** – Structured and readable pipeline syntax
* **Scripted Pipeline** – More flexible Groovy-based pipelines


## Pipeline Components

| Component | Description                    |
| --------- | ------------------------------ |
| Agent     | Node where pipeline runs       |
| Stage     | Logical phase of the pipeline  |
| Steps     | Commands executed in the stage |



## Best Practices

* Use **Pipeline as Code** for CI/CD automation
* Keep **deployment stages simple**
* Ensure **correct permissions for repositories**
* Validate deployments through **Load Balancer endpoints**

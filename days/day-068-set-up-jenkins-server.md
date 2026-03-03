# Set Up Jenkins Server

The DevOps team at **xFusionCorp Industries** initiated the setup of CI/CD pipelines using **Jenkins** as the automation server.

This task covers installing Jenkins on a CentOS Stream 9 server using the `yum` package manager and configuring the initial admin user.

---

##  Objective

* Install Jenkins using **yum only**
* Start and enable Jenkins service
* Create admin user:

  * **Username:** `theadmin`
  * **Password:** `Adm!n321`
  * **Full Name:** `John`
  * **Email:** `john@jenkins.stratos.xfusioncorp.com`

---

##  Server Access


```bash
ssh root@jenkins
```

Password:

```
S3curePass
```

##  Step 1 — Verify Operating System

```bash
cat /etc/os-release
```

Output:

```bash
NAME="CentOS Stream"
VERSION="9"
PRETTY_NAME="CentOS Stream 9"
```

##  Step 2 — Update System & Install Prerequisites

```bash
yum update -y
yum install -y wget
```

##  Step 3 — Add Jenkins Repository (RedHat Stable)

Following official Jenkins documentation for RedHat/Fedora systems:

```bash
wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

Import Jenkins GPG key:

```bash
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

##  Step 4 — Install Java (Required)

Jenkins requires Java runtime:

```bash
yum install -y java-21-openjdk
```

Verify installation:

```bash
java -version
```

##  Step 5 — Install Jenkins

```bash
yum install -y jenkins
```

Reload systemd:

```bash
systemctl daemon-reload
```

##  Step 6 — Start and Enable Jenkins Service

```bash
systemctl enable --now jenkins
```

Verify:

```bash
systemctl status jenkins
```

##  Step 7 — Retrieve Initial Admin Password

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the generated password.

---

##  Step 8 — Access Jenkins Web Interface

Open in browser:

```
http://jenkins.stratos.xfusioncorp.com:8080
```

##  Step 9 — Configure Jenkins Admin User

During initial setup:

1. Paste the initial password
2. Click **Install Suggested Plugins**
3. Create admin user with:

| Field     | Value                                                                               |
| --------- | ----------------------------------------------------------------------------------- |
| Username  | `theadmin`                                                                          |
| Password  | `Adm!n321`                                                                          |
| Full Name | John                                                                                |
| Email     | [john@jenkins.stratos.xfusioncorp.com](mailto:john@jenkins.stratos.xfusioncorp.com) |

4. Save and Finish
5. Start using Jenkins

---

#  Good to Know


##  Jenkins Core Concepts

###  Continuous Integration & Continuous Delivery (CI/CD)

* **Continuous Integration (CI)** ensures that code changes are automatically built and tested whenever developers push updates to the repository.
* **Continuous Delivery/Deployment (CD)** automates the release process, allowing applications to be deployed quickly and reliably.

This approach reduces integration issues and accelerates software delivery.


###  Automation in DevOps

Jenkins automates:

* Application builds
* Unit and integration testing
* Code quality checks
* Packaging and deployments

Automation eliminates repetitive manual tasks and increases reliability.


###  Pipeline

A **pipeline** represents a structured workflow that defines the steps required to build, test, and deploy software.

It is typically defined in a `Jenkinsfile` and may include:

* Source code checkout
* Compilation
* Testing
* Docker image creation
* Deployment

Pipelines allow teams to implement **Pipeline as Code**, improving traceability and version control.


###  Plugin Ecosystem

Jenkins provides an extensive plugin system that enables integration with:

* Git / GitHub / GitLab
* Docker
* Kubernetes
* Maven / Gradle
* SonarQube
* Cloud platforms

Plugins extend Jenkins’ functionality and adapt it to almost any DevOps workflow.


##  Jenkins System Design

###  Controller

The controller:

* Manages Jenkins configuration
* Schedules and monitors jobs
* Provides the web interface
* Distributes tasks to agents

It is the central management component.


###  Agent Nodes

Agents are worker machines responsible for executing builds.

They:

* Run pipeline steps
* Execute shell commands
* Build applications
* Perform testing

Agents enable **distributed and scalable builds**.


###  Jobs

A job is a defined task in Jenkins.

Examples:

* Build a Java application
* Run automated tests
* Deploy to a staging server

Each job contains configuration that defines what should happen.


###  Builds

A build is a single execution instance of a job.
Each build stores logs, artifacts, and status (Success / Failed / Unstable).

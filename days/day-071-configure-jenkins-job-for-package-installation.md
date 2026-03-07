# Configure Jenkins Job for Package Installation

New infrastructure requirements were introduced in the **Stratos Datacenter** to automate package installation on servers. The Nautilus DevOps team deployed a **Jenkins server** and now needs to create a Jenkins job that installs packages dynamically on the **storage server (`ststor01`)**.

The job must allow users to specify a package name using a parameter and Jenkins will install that package remotely on the server using SSH.

---

# Objective

Create a Jenkins job named **`install-packages`** that:

* Accepts a **string parameter `PACKAGE`**
* Connects to the **storage server**
* Installs the specified package using:

```bash
sudo yum install -y $PACKAGE
```

This automation removes the need for manual package installation and ensures consistent execution through Jenkins.

---

# Steps

## 1. Access Jenkins UI

Click the **Jenkins** button in the top navigation bar.

Login using:

```
Username: admin
Password: Adm!n321
```

## 2. Install Required Jenkins Plugins

Navigate to:

```
Manage Jenkins → Manage Plugins → Available
```

Install the following plugins:

* **SSH**
* **SSH Credentials**
* **SSH Build Agents**

After installation:

Select

```
Restart Jenkins when installation is complete and no jobs are running
```

Refresh the Jenkins UI after restart.

## 3. Configure SSH Access from Jenkins to Storage Server

Jenkins must connect to the storage server using **SSH key authentication**.

Login to the Jenkins server and generate an SSH key:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
```

Explanation:

* `mkdir -p ~/.ssh` → creates the SSH directory
* `chmod 700 ~/.ssh` → secures the directory
* `ssh-keygen` → generates SSH public/private key pair

## 4. Copy SSH Key to Storage Server

Install the Jenkins public key on the storage server:

```bash
ssh-copy-id natasha@ststor01.stratos.xfusioncorp.com
```

Explanation:

* Copies Jenkins public key to the remote server
* Enables **passwordless SSH authentication**

Verify connection:

```bash
ssh natasha@ststor01.stratos.xfusioncorp.com
```

If no password is required → SSH trust is configured correctly.

## 5. Configure Jenkins Global SSH Settings

Navigate to:

```
Manage Jenkins → Configure System
```

Scroll to **Publish over SSH**.

### Jenkins SSH Key

Set:

```
Path to key:
/var/lib/jenkins/.ssh/id_rsa
```

This tells Jenkins where the SSH private key is located.

### SSH Server Configuration

Add a new SSH server:

| Field            | Value                            |
| ---------------- | -------------------------------- |
| Name             | ststor01                         |
| Hostname         | ststor01.stratos.xfusioncorp.com |
| Username         | natasha                          |
| Remote Directory | /tmp                             |
| Port             | 22                               |

Click:

```
Test Configuration
```

Expected result:

```
Success
```

Save the configuration.

## 6. Create Jenkins Job

Navigate to:

```
Dashboard → New Item
```

Create a new job:

```
install-packages
```

Select:

```
Freestyle Project
```

Click **OK**.

## 7. Add Job Parameter

Inside job configuration:

Enable:

```
This project is parameterized
```

Add a parameter:

```
Add Parameter → String Parameter
```

Configure:

| Field         | Value              |
| ------------- | ------------------ |
| Name          | PACKAGE            |
| Default Value | git                |
| Description   | Package to install |

This allows users to specify which package Jenkins should install.

## 8. Configure Build Step

Scroll to **Build** section.

Click:

```
Add build step
```

Select:

```
Send files or execute commands over SSH
```

Choose SSH server:

```
ststor01
```

Add command:

```bash
sudo yum install -y $PACKAGE 
```

Explanation:

* `sudo yum install` → installs the package
* `$PACKAGE` → value provided by Jenkins parameter

Save the job configuration.

## 9. Execute the Job

Run the Jenkins job:

```
Dashboard → install-packages → Build with Parameters
```

Enter a package name:

```
PACKAGE = git
```

Jenkins will connect to the storage server and install the package automatically.

Example console output:

```
Installing:
 git.x86_64
Complete!
Finished: SUCCESS
```

---

# Good to Know

## Jenkins Parameterized Jobs

Parameterized jobs allow users to provide dynamic input during execution.

Example:

```
PACKAGE=git
PACKAGE=httpd
PACKAGE=vim
```

Jenkins reads the parameter and uses it inside the build command.


## Jenkins Remote Execution via SSH

Jenkins can execute commands on remote servers using SSH.

This is useful for:

* Infrastructure automation
* Remote configuration
* Server management
* Deployment pipelines


## SSH Key Authentication

SSH keys provide secure authentication between servers.

Advantages:

* No password required
* More secure than password authentication
* Ideal for automation tools like Jenkins

Flow used in this task:

```
Jenkins Server
      ↓
SSH Key Authentication
      ↓
Storage Server (ststor01)
      ↓
Execute package installation
```


## Idempotent Package Installation

The command:

```bash
sudo yum install -y package
```

is **idempotent**, meaning it can run multiple times safely.

If the package already exists:

```
Nothing to do.
```

This ensures Jenkins jobs can run repeatedly without causing issues.

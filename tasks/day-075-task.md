# Day 75 - Jenkins Slave Nodes

## Task Description

Configure the application servers as Jenkins SSH build agents to allow Jenkins jobs to run tasks on those servers.

## Requirements

### Access Jenkins

* Open Jenkins from the top navigation.
* Login using:

```
Username: admin
Password: Adm!n321
```

---

### Agent: App Server 1

* Node name:

```
App_server_1
```

* Label:

```
stapp01
```

* Remote root directory:

```
/home/tony/jenkins
```

* Connection method:

```
SSH Build Agent
```

---

### Agent: App Server 2

* Node name:

```
App_server_2
```

* Label:

```
stapp02
```

* Remote root directory:

```
/home/steve/jenkins
```

* Connection method:

```
SSH Build Agent
```

---

### Agent: App Server 3

* Node name:

```
App_server_3
```

* Label:

```
stapp03
```

* Remote root directory:

```
/home/banner/jenkins
```

* Connection method:

```
SSH Build Agent
```

---

### Agent Status

* Ensure each node connects successfully via SSH.
* Confirm all agents appear **online** in Jenkins.

### Plugin Requirement

* Install any required plugin for SSH build agents if not already installed.
* Restart Jenkins if prompted after plugin installation.

### Documentation

* Capture screenshots or record the configuration steps for review.

## Expected Result

* Three Jenkins agents (`App_server_1`, `App_server_2`, `App_server_3`) are configured.
* Each agent has the correct label and remote root directory.
* All nodes connect successfully via SSH.
* All agents appear online and ready to execute Jenkins jobs.

End of Task

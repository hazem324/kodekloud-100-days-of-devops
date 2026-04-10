# Day 91 - Ansible Lineinfile Module

## Task Description

Install and configure Apache on all application servers and deploy a sample web page with required content, ownership, and permissions.

## Requirements

### Inventory

* Use existing inventory file:

```
/home/thor/ansible/inventory
```

* Ensure it includes all application servers.

---

### Playbook

* Create playbook at:

```
/home/thor/ansible/playbook.yml
```

---

### Apache Installation

* Install package:

```
httpd
```

* Ensure service is:

```
Running and enabled
```

---

### Web Page Creation

* Create file:

```
/var/www/html/index.html
```

* Initial content:

```
This is a Nautilus sample file, created using Ansible!
```

---

### Content Update

* Use module:

```
lineinfile
```

* Add line:

```
Welcome to xFusionCorp Industries!
```

* Ensure this line is inserted at the top of the file.

---

### File Configuration

* Owner:

```
apache
```

* Group:

```
apache
```

* Permissions:

```
0755
```

---

### Execution

* Run playbook using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Apache is installed and running on all app servers.
* File `/var/www/html/index.html` exists with both required lines.
* The welcome message appears at the top of the file.
* File ownership is `apache:apache`.
* File permissions are set to `0755`.
* Playbook executes successfully without errors.

End of Task

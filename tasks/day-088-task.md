# Day 88 - Ansible Blockinfile Module

## Task Description

Use Ansible to install and configure Apache on all application servers and deploy a sample web page with specific content, ownership, and permissions.

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

### Apache Setup

* Install package:

```
httpd
```

* Ensure service is:

```
Running and enabled
```

---

### Web Page Content

* File:

```
/var/www/html/index.html
```

* Use Ansible module:

```
blockinfile
```

* Content to add:

```
Welcome to XfusionCorp!

This is Nautilus sample file, created using Ansible!

Please do not modify this file manually!
```

* Do not define custom or empty markers.

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
0644
```

---

### Execution

* Run playbook using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Apache (`httpd`) is installed on all application servers.
* Apache service is running and enabled.
* File `/var/www/html/index.html` contains the specified content.
* File ownership is set to `apache:apache`.
* File permissions are set to `0644`.
* Playbook executes successfully on all servers.

End of Task

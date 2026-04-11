# Day 92 - Managing Jinja2 Templates Using Ansible

## Task Description

Enhance an existing Ansible role by adding a Jinja2 template and updating tasks to deploy a dynamic index page on App Server 2.

## Requirements

### Playbook Update

* Update playbook:

```
~/ansible/playbook.yml
```

* Configure it to run the role:

```
httpd
```

* Target host:

```
stapp02
```

---

### Jinja2 Template

* Create template at:

```
/home/thor/ansible/role/httpd/templates/index.html.j2
```

* Content:

```
This file was created using Ansible on {{ inventory_hostname }}
```

* Do not hardcode server name.

---

### Role Task Update

* Update tasks file:

```
/home/thor/ansible/role/httpd/tasks/main.yml
```

* Add task to deploy template to:

```
/var/www/html/index.html
```

* Set file permissions:

```
0777
```

---

### Ownership

* Set file owner and group to respective sudo user:

```
stapp02 → steve
```

---

### Execution

* Run playbook using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Playbook runs httpd role on App Server 2.
* Template file is created dynamically using `inventory_hostname`.
* File `/var/www/html/index.html` exists on App Server 2.
* Permissions are set to `0777`.
* Ownership is set to the correct sudo user.
* Playbook executes successfully without errors.

End of Task

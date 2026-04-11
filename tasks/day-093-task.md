# Day 93 - Using Ansible Conditionals

## Task Description

Use Ansible conditionals to copy different files to specific application servers with defined ownership and permissions.

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

* Target all hosts:

```
hosts: all
```

* Use Ansible conditionals (`when`) based on:

```
ansible_nodename
```

---

### File Distribution

#### App Server 1

* Source:

```
/usr/src/itadmin/blog.txt
```

* Destination:

```
/opt/itadmin/blog.txt
```

* Owner/Group:

```
tony
```

* Permissions:

```
0744
```

#### App Server 2

* Source:

```
/usr/src/itadmin/story.txt
```

* Destination:

```
/opt/itadmin/story.txt
```

* Owner/Group:

```
steve
```

* Permissions:

```
0744
```

#### App Server 3

* Source:

```
/usr/src/itadmin/media.txt
```

* Destination:

```
/opt/itadmin/media.txt
```

* Owner/Group:

```
banner
```

* Permissions:

```
0744
```

---

### Execution

* Run playbook using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Correct file is copied to each respective server.
* Ownership and permissions are applied as specified.
* Conditional logic ensures tasks run only on intended servers.
* Playbook executes successfully without errors.

End of Task

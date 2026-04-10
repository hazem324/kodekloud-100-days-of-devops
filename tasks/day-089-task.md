# Day 89 - Ansible Manage Services

## Task Description

Create an Ansible playbook to install Apache and ensure the service is started and enabled on all application servers.

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

* Target all application servers.

### Package Installation

* Install package:

```
httpd
```

### Service Management

* Ensure service:

```
httpd
```

is:

```
Started and enabled
```

---

### Execution

* Playbook must be executable by:

```
thor user
```

* Run using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Apache (`httpd`) is installed on all application servers.
* Apache service is running and enabled on all servers.
* Playbook executes successfully without errors.
* User `thor` can run the playbook without issues.

End of Task

# Day 84 - Copy Data to App Servers using Ansible

## Task Description

Create an Ansible setup to copy a file from the jump host to all application servers in the environment.

## Requirements

### Inventory

* Create inventory file at:

```
/home/thor/ansible/inventory
```

* Use INI format.
* Include all application servers:

```
stapp01
stapp02
stapp03
```

* Ensure required connection variables are defined for successful SSH access.

---

### Playbook

* Create playbook at:

```
/home/thor/ansible/playbook.yml
```

* Source file:

```
/usr/src/itadmin/index.html
```

* Destination directory:

```
/opt/itadmin
```

* Ensure the file is copied to all application servers.

---

### Execution

* Playbook must run using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Inventory file includes all application servers.
* Playbook successfully copies `index.html` to `/opt/itadmin` on all servers.
* Playbook executes without errors using the specified command.

End of Task

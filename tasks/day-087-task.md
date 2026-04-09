# Day 87 - Ansible Install Package

## Task Description

Automate the installation of the Chrony package on all application servers using Ansible.

## Requirements

### Inventory

* Create inventory file at:

```
/home/thor/playbook/inventory
```

* Use INI format.
* Include all application servers:

```
stapp01
stapp02
stapp03
```

* Ensure proper connection variables are configured.

---

### Playbook

* Create playbook at:

```
/home/thor/playbook/playbook.yml
```

* Target all application servers.
* Use Ansible module:

```
yum
```

* Install package:

```
chrony
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

* Inventory file includes all application servers.
* Playbook installs `chrony` on all servers.
* Installation completes successfully without errors.
* Playbook runs correctly using the specified command.

End of Task

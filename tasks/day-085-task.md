# Day 85 - Create Files on App Servers using Ansible

## Task Description

Configure Ansible to create a file on all application servers with specific permissions and ownership settings.

## Requirements

### Inventory

* Create inventory file at:

```
~/playbook/inventory
```

* Use INI format.
* Include all application servers:

```
stapp01
stapp02
stapp03
```

* Ensure required connection variables are defined.

---

### Playbook

* Create playbook at:

```
~/playbook/playbook.yml
```

* Target all application servers.
* Create an empty file:

```
/usr/src/opt.txt
```

* Set file permissions:

```
0655
```

### Ownership

* Set owner and group as:

```
stapp01 → tony
stapp02 → steve
stapp03 → banner
```

---

### Execution

* Playbook must run using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Inventory includes all application servers.
* File `/usr/src/opt.txt` is created on all servers.
* Permissions are set to `0655`.
* Ownership is correctly assigned per server.
* Playbook executes successfully without errors.

End of Task

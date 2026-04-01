# Day 83 - Troubleshoot and Create Ansible Playbook

## Task Description

Update the Ansible inventory and create a playbook to generate an empty file on App Server 3.

## Requirements

### Inventory Configuration

* Update inventory file:

```
/home/thor/ansible/inventory
```

* Ensure it includes:

```
stapp03
```

* Configure necessary connection variables for Ansible execution.

---

### Playbook

* Create playbook at:

```
/home/thor/ansible/playbook.yml
```

* Add a task to create an empty file at:

```
/tmp/file.txt
```

* Ensure the playbook targets App Server 3.

---

### Execution

* The playbook must run using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Inventory correctly defines App Server 3.
* Playbook is created at the specified location.
* File `/tmp/file.txt` is successfully created on App Server 3.
* Playbook executes successfully using the given command.

End of Task

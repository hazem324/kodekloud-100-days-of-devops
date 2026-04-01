# Day 82 - Create Ansible Inventory for App Server Testing

## Task Description

Create an Ansible inventory file to enable execution of playbooks on App Server 3.

## Requirements

* Create an inventory file at:

```
/home/thor/playbook/inventory
```

* Use INI format.
* Add App Server 3 with hostname:

```
stapp03
```

* Configure required connection variables for Ansible:

  * Remote user
  * SSH password
  * Disable strict host key checking
* Ensure the inventory is usable directly with:

```
ansible-playbook -i inventory playbook.yml
```

## Expected Result

* Inventory file exists at the specified path.
* App Server 3 is correctly defined with required variables.
* Ansible playbook runs successfully using the inventory without additional arguments.

End of Task

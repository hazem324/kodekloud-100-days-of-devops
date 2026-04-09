# Day 86 - Ansible Ping Module Usage

## Task Description

Establish passwordless SSH access from the Ansible controller to a managed node and verify connectivity using Ansible ping.

## Requirements

### Controller Setup

* Use the following host as Ansible controller:

```
Jump Host
```

* Execute tasks using user:

```
thor
```

---

### Inventory

* Use existing inventory file:

```
/home/thor/ansible/inventory
```

* Ensure App Server 2 is correctly defined:

```
stapp02
```

---

### SSH Configuration

* Configure passwordless SSH from:

```
Jump Host (thor user)
```

to:

```
App Server 2
```

---

### Connectivity Test

* Verify connection using Ansible ping module targeting:

```
stapp02
```

* Ensure no password prompt occurs during execution.

## Expected Result

* Passwordless SSH is successfully configured from jump host to App Server 2.
* Ansible ping command executes successfully.
* Target host responds with a successful ping result.
* No additional authentication input is required.

End of Task

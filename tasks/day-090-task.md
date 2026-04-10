# Day 90 - Managing ACLs Using Ansible

## Task Description

Create files on specific application servers and assign ACL permissions while maintaining root ownership.

## Requirements

### Inventory

* Use existing inventory file:

```
/home/thor/ansible/inventory
```

---

### Playbook

* Create playbook at:

```
/home/thor/ansible/playbook.yml
```

---

### App Server 1

* Create file:

```
/opt/sysops/blog.txt
```

* Owner:

```
root
```

* Set ACL:

```
Group: tony
Permissions: r
```

---

### App Server 2

* Create file:

```
/opt/sysops/story.txt
```

* Owner:

```
root
```

* Set ACL:

```
User: steve
Permissions: rw
```

---

### App Server 3

* Create file:

```
/opt/sysops/media.txt
```

* Owner:

```
root
```

* Set ACL:

```
Group: banner
Permissions: rw
```

---

### Execution

* Run playbook using:

```
ansible-playbook -i inventory playbook.yml
```

* No additional arguments should be required.

## Expected Result

* Files are created on respective app servers.
* Ownership is set to `root`.
* ACL permissions are correctly applied per server.
* Playbook executes successfully without errors.

End of Task

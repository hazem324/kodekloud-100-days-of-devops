#  Ansible Ping Module Usage

The Nautilus DevOps team needs to validate connectivity between the Ansible controller and managed nodes before executing playbooks.

In this task, we configure the inventory file with correct SSH credentials and test connectivity using the Ansible **ping module**.

---

##  Task Requirements

* Use **jump host** as Ansible controller
* Execute commands as **thor user**
* Use inventory file:

  ```
  /home/thor/ansible/inventory
  ```
* Test connectivity to **App Server 2 (stapp02)**

---

##  Steps


###  1. Update Inventory File

Edit the inventory file:

```bash
vi /home/thor/ansible/inventory
```

Add the correct configuration:

```ini
stapp01 ansible_host=stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_host=stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_host=stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### 2. Test Ansible Ping (App Server 2)

```bash
ansible -i /home/thor/ansible/inventory stapp02 -m ping
```

---

#  Good to Know

##  Ansible Ping Module

* **Purpose**: Validate connectivity between controller and nodes
* **Not ICMP**: Uses SSH, not network ping
* **Checks**:

  * SSH connectivity
  * Authentication (user/password)
  * Python availability
  * Remote execution capability


##  Inventory File Concepts

| Variable                  | Description           |
| ------------------------- | --------------------- |
| `ansible_host`            | Target hostname or IP |
| `ansible_user`            | SSH user              |
| `ansible_ssh_pass`        | SSH password          |
| `ansible_ssh_common_args` | Extra SSH options     |


##  Why `StrictHostKeyChecking=no` is Important

* Prevents SSH from asking for host verification
* Required when using `sshpass` (password auth)
* Ensures **non-interactive execution** 

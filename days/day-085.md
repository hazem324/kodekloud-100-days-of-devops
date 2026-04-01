#  Create Files on App Servers using Ansible

The Nautilus DevOps team is testing Ansible modules on servers in Stratos DC.
In this task, we automate **file creation, permissions, and ownership management** across multiple app servers.

---

##  Task Requirements

a. Create an inventory file `~/playbook/inventory` on jump host and include all app servers.

b. Create a playbook `~/playbook/playbook.yml` to create a blank file `/usr/src/opt.txt` on all app servers.

c. Set the permissions of `/usr/src/opt.txt` to `0655`.

d. Ensure ownership:

* `tony` → app server 1 (`stapp01`)
* `steve` → app server 2 (`stapp02`)
* `banner` → app server 3 (`stapp03`)

>  Validation runs:

```bash
ansible-playbook -i inventory playbook.yml
```

---

#  Steps

## 0. Navigate to working directory

```bash
cd ~/playbook
```

---

## 1. Create Inventory File

```ini
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

## 2. Create Ansible Playbook

Add the following content: [`PLAYBOOK`](../files/ansible_playbook_Create_files_on_app_servers_d85.yml).

## 3. Run Playbook

```bash
ansible-playbook -i inventory playbook.yml
```


---

#  Good to Know

## File Creation with Ansible

* **file Module**: Used to create, delete, or modify files and directories
* **State Parameter**: `touch` creates an empty file if it does not exist
* **Idempotency**: Running the playbook multiple times does not duplicate changes
* **Privilege Escalation**: `become: yes` is required for system paths like `/usr/src`


## File Attributes

* **Owner**: Defines which user owns the file (e.g., `tony`, `steve`, `banner`)
* **Group**: Defines the group ownership (usually same as owner in this task)
* **Mode**: File permissions using octal format (e.g., `0655`)
* **Path**: Absolute path of the file being managed


## Permission Management

* **Octal Notation**: `0655 = rw-r-xr-x`
* **Execution Bit**: Allows users to execute the file
* **Consistency**: Permissions must be explicitly defined to avoid defaults
* **Security**: Incorrect permissions can expose or restrict access


## Inventory & SSH Authentication

* **Correct Variables**: Must use `ansible_ssh_pass` (not `ansible_ssh_pss`)
* **Authentication Flow**:

  * If password missing → Ansible tries SSH keys
  * If both fail → host becomes `UNREACHABLE`
* **Testing Connectivity**:

  ```bash
  ansible all -i inventory -m ping
  ```


## Ansible Module Strictness

* **Exact Parameter Names Required**:

  * `group` 
  * `groupe` 
* **No Auto-Correction**: Typos cause immediate task failure
* **Error Messages**: Always read them carefully — they point directly to the issue


## Conditional Execution

* **Host-Based Logic**:

  ```yaml
  when: inventory_hostname == "stapp01"
  ```
* Allows different configurations per server
* Useful when infrastructure is not identical


## Debugging & Best Practices

* **Verbose Mode**:

  ```bash
  ansible-playbook playbook.yml -vvv
  ```
* **Validate Before Execution**:

  ```bash
  ansible all -m ping
  ```
* **Common Pitfalls**:

  * Wrong variable names
  * YAML indentation errors
  * Incorrect module parameters


## Cross-Environment Considerations

* **User Mapping**: Users must exist on target systems
* **Permissions Model**: Linux uses rwx model (unlike Windows ACLs)
* **Path Sensitivity**: Linux paths are case-sensitive
* **Privilege Requirements**: Some directories require root access


##  Key Takeaway

* Small typos can break automation (inventory or module level)
* Always validate connectivity before running playbooks
* Understand how Ansible handles authentication and modules
* Write playbooks that are **idempotent, predictable, and secure**


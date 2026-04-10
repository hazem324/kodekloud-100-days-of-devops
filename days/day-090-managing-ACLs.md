#  Managing File ACLs with Ansible

The Nautilus DevOps team needs to create specific files across multiple app servers in Stratos DC while enforcing strict ownership and fine-grained access control.

All files must be:

* Owned by **root**
* Accessible to specific users/groups via **ACLs**

To automate this, an Ansible playbook is used.

---

##  Task Requirements

Create a playbook named `playbook.yml` under:

```bash
/home/thor/ansible
```

An inventory file is already available in the same directory.

---

###  Objectives

1. **App Server 1 (`stapp01`)**

   * Create file: `/opt/sysops/blog.txt`
   * Owner: `root`
   * ACL: Give **read (r)** permission to **group `tony`**

2. **App Server 2 (`stapp02`)**

   * Create file: `/opt/sysops/story.txt`
   * Owner: `root`
   * ACL: Give **read + write (rw)** permission to **user `steve`**

3. **App Server 3 (`stapp03`)**

   * Create file: `/opt/sysops/media.txt`
   * Owner: `root`
   * ACL: Give **read + write (rw)** permission to **group `banner`**


---

##  Steps

### 1. Navigate to Ansible directory

```bash
cd /home/thor/ansible
ls
```


### 2. Verify inventory

```bash
cat inventory
```

Make sure hosts like `stapp01`, `stapp02`, and `stapp03` exist.


### 3. Create playbook

```bash
touch playbook.yml
```

### 4. Add playbook content

* Use:

  * `file` module → create files
  * `acl` module → assign permissions
  * `become: yes` → required for root-level operations
  * `when` conditions → target specific servers

  Paste the following configuration  [`PLAYBOOK`](../files/ansible_playbook_managing_ACLs_using_ansible_d90.yml).


### 5. Run playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

#  Good to Know

##  What are ACLs?

**Access Control Lists (ACLs)** extend standard Linux permissions.

Instead of just:

```
owner | group | others
```

You can define:

```
specific user → custom permissions
specific group → custom permissions
```

 This is critical in real-world infrastructures where multiple teams share resources.


##  Ansible ACL Module

* Module: `ansible.posix.acl`
* Used to manage file-level permissions beyond `chmod`

###  Key Parameters

| Parameter     | Description           |
| ------------- | --------------------- |
| `path`        | Target file           |
| `entity`      | User or group name    |
| `etype`       | `user` or `group`     |
| `permissions` | `r`, `w`, `rw`, `rwx` |
| `state`       | `present` or `absent` |


##  Useful ACL Commands

| Command                     | Purpose    |
| --------------------------- | ---------- |
| `getfacl file`              | View ACL   |
| `setfacl -m u:user:rw file` | Add ACL    |
| `setfacl -x u:user file`    | Remove ACL |


##  Why ACL Instead of chmod?

| chmod               | ACL                    |
| ------------------- | ---------------------- |
| Basic permissions   | Fine-grained control   |
| Limited flexibility | Highly customizable    |
| Only 3 roles        | Unlimited users/groups |

 ACL is preferred in **enterprise environments** for better security and flexibility.


##  Best Practices

* Always use **least privilege principle**
* Keep ownership with `root` for sensitive files
* Use ACLs instead of modifying system groups
* Validate changes using `getfacl`
* Use `when` conditions for multi-host playbooks


#  Ansible Blockinfile Module

The Nautilus DevOps team needs to provision a simple **Apache (httpd) web server** across all application servers in Stratos Datacenter. Additionally, a sample web page must be deployed using **Ansible automation only**.

This task ensures proper service configuration, file management, and idempotent infrastructure practices.

---

##  Task Requirements

* An inventory file already exists at:

```bash
/home/thor/ansible/inventory
```

* Create the playbook:

```bash
/home/thor/ansible/playbook.yml
```

---

###  Objectives

* Install `httpd` on all app servers
* Ensure the `httpd` service is:

  * Started
  * Enabled on boot
* Use `blockinfile` to insert content into:

```bash
/var/www/html/index.html
```

* Content must be **exactly**:

```
Welcome to XfusionCorp!

This is Nautilus sample file, created using Ansible!

Please do not modify this file manually!
```

* Set file ownership:

  * User → `apache`
  * Group → `apache`

* Set file permissions:

  * `0644`


---

##  Steps

### 1. Navigate to Ansible directory

```bash
cd /home/thor/ansible
```

### 2. Create the playbook

```bash
touch playbook.yml
```

### 3. Edit the playbook

```bash
vi playbook.yml
```

Paste the following configuration  [`PLAYBOOK`](../files/ansible_playbook_blockinfile_module_d88.yml).

### 4. Run the playbook

```bash
ansible-playbook -i inventory playbook.yml
```

##  Verification (Optional but Recommended)

Run on any app server:

```bash
cat /var/www/html/index.html
```


---

##  Good to Know

###  `blockinfile` Module Deep Dive

* **Purpose**: Manage multi-line text blocks inside files

* **Idempotent**: Won’t duplicate content on multiple runs

* **Markers**:

  * Automatically adds:

    ```
    # BEGIN ANSIBLE MANAGED BLOCK
    # END ANSIBLE MANAGED BLOCK
    ```
  * ❗ Do not override markers in this task (requirement)

* **Important Behavior**:

  * Does NOT create file unless:

    ```yaml
    create: yes
    ```

---

###  File Ownership & Permissions

| Attribute | Value  | Meaning                       |
| --------- | ------ | ----------------------------- |
| Owner     | apache | Web server user               |
| Group     | apache | Web server group              |
| Mode      | 0644   | Read for all, write for owner |

 Ensures Apache can safely serve the file without permission issues

---

###  Apache (httpd) Basics

* Default document root:

  ```
  /var/www/html
  ```
* Default index file:

  ```
  index.html
  ```
* Service name:

  ```
  httpd
  ```

---

### 🔹 Ansible Module Responsibilities

| Module        | Responsibility                 |
| ------------- | ------------------------------ |
| `package`     | Install software               |
| `service`     | Manage services                |
| `blockinfile` | Manage file content            |
| `file`        | Manage permissions & ownership |

👉 Separation of concerns = cleaner, maintainable automation

---

### 🔹 Idempotency Principle

Running the playbook multiple times will:

* NOT reinstall packages unnecessarily
* NOT duplicate file content
* Maintain consistent system state

👉 This is a **core DevOps principle** 🔁

---

## 🎯 Summary

This task demonstrates:

* Infrastructure as Code (IaC)
* Configuration management with Ansible
* Service provisioning
* File and permission handling
* Idempotent automation design

---

If you want to level this up next 🚀:

* Convert into an **Ansible Role**
* Add **handlers (restart httpd on change)**
* Integrate with **CI/CD pipeline**

Just tell me 👍

# Ansible Manage Services

The Nautilus DevOps team is working on automating package installation and service management across app servers in Stratos DC. To streamline operations, an Ansible playbook must be created and executed from the jump host.

This task focuses on installing and managing the Apache HTTP server (`httpd`) across all application servers.

---

##  Objective

Configure an Ansible playbook to:

* Install `httpd` on all app servers
* Start the `httpd` service
* Enable the service at boot
* Ensure execution works using the predefined inventory

---

##  Requirements

a. Create an Ansible playbook at:

```bash
/home/thor/ansible/playbook.yml
```

b. Install `httpd` on all app servers

c. Start and enable the `httpd` service

d. Use the existing inventory:

```bash
/home/thor/ansible/inventory
```

e. Ensure user `thor` can execute the playbook


##  Steps

### 1. Navigate to Ansible directory

```bash
cd /home/thor/ansible
ls
```

### 2. Create the playbook file

```bash
touch playbook.yml
```

### 3. Add playbook configuration

Paste the following configuration  [`PLAYBOOK`](../files/ansible_playbook_manage_services_d89.yml).

### 4. Execute the playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

##  Good to Know

###  Ansible Package Management

* **package module**: универсal module for installing packages (works with `yum`, `dnf`, `apt`)
* **state: present** → ensures the package is installed
* Avoids OS-specific dependency issues


###  Service Management in Ansible

* **service module**: used to control system services
* Works across most Linux distributions

#### Common Service States:

* **started** → service is running
* **stopped** → service is stopped
* **restarted** → service restarts
* **reloaded** → reloads configuration without full restart


###  Boot-Time Configuration

* **enabled: yes** → service starts automatically on boot
* **enabled: no** → service does not start on boot


###  Privilege Escalation

* **become: yes** → runs tasks with root privileges
* Required for:

  * Installing packages
  * Managing services


###  Inventory Usage

* Inventory defines target servers
* Using:

```bash
-i inventory
```

ensures Ansible applies changes to correct hosts


###  Idempotency (Core DevOps Concept)

* Ansible tasks are **idempotent**
* Running the playbook multiple times:

  * Will NOT reinstall packages unnecessarily
  * Will NOT restart services if already running

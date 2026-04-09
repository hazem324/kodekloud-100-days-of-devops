#  Ansible Install Package

The Nautilus Application Development team needs to prepare app servers in the Stratos Datacenter by installing required system packages. To ensure consistency and automation, this task must be completed using **Ansible**.

---

##  Objective

* Create an inventory file at:

  ```
  /home/thor/playbook/inventory
  ```

* Create an Ansible playbook at:

  ```
  /home/thor/playbook/playbook.yml
  ```

* Install the **`chrony` package** on all app servers using the **Ansible `yum` module**

* Ensure the playbook runs successfully using the `thor` user

---

##  Steps

### 1. Navigate to Working Directory

```sh
cd /home/thor/playbook
```

### 2. Create Required Files

```sh
touch inventory playbook.yml
```

### 3. Configure Inventory File

```ini
[app_servers]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### 4. Create Ansible Playbook

Add the following content: [`PLAYBOOK`](../files/ansible_playbook__install_chrony_d87.yml).

### 5. Set Proper Permissions

```sh
chown -R thor:thor /home/thor/playbook
```

### 6. Run the Playbook

```sh
ansible-playbook -i inventory playbook.yml
```


---

#  Good to Know

##  What is Chrony?

* `chrony` is a time synchronization service (NTP alternative)
* Keeps system clocks accurate across servers
* Essential for:

  * Distributed systems
  * Logging consistency
  * Security (certificates, tokens)


##  Ansible Yum Module

* Used for managing packages on **RHEL/CentOS systems**
* Handles:

  * Installation
  * Removal
  * Updates
* Automatically resolves dependencies

### Example:

```yaml
yum:
  name: chrony
  state: present
```


##  Privilege Escalation (`become`)

* Required for installing system packages
* Runs tasks as `root`

```yaml
become: yes
```


##  Package States

| State   | Meaning                          |
| ------- | -------------------------------- |
| present | Install if not already installed |
| absent  | Remove package                   |
| latest  | Update to latest version         |


##  Idempotency (Core DevOps Concept)

* Ansible ensures:

  * Running playbook multiple times = safe 
  * No duplicate installs
  * No unintended changes


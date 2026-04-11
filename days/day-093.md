# Using Ansible Conditions

The Nautilus DevOps team is exploring different ways to train engineers in automation using **Ansible**. One of the key concepts is **conditional task execution**, which allows tasks to run only on specific hosts based on defined criteria.

In this task, we use **Ansible `when` conditionals** along with **gathered facts** to selectively copy files to different application servers.

---

##  Task Description

An inventory file is already available at:

```bash
/home/thor/ansible/inventory
```

Create a playbook:

```bash
/home/thor/ansible/playbook.yml
```

The playbook must perform the following:

---

###  Requirements

1. Copy `blog.txt` from `/usr/src/itadmin` (jump host) to:

   * **App Server 1 (`stapp01`)**
   * Destination: `/opt/itadmin/blog.txt`
   * Owner: `tony`
   * Group: `tony`
   * Permissions: `0744`

---

2. Copy `story.txt` from `/usr/src/itadmin` to:

   * **App Server 2 (`stapp02`)**
   * Destination: `/opt/itadmin/story.txt`
   * Owner: `steve`
   * Group: `steve`
   * Permissions: `0744`

---

3. Copy `media.txt` from `/usr/src/itadmin` to:

   * **App Server 3 (`stapp03`)**
   * Destination: `/opt/itadmin/media.txt`
   * Owner: `banner`
   * Group: `banner`
   * Permissions: `0744`

---

##  Important Notes

* Use:

  ```yaml
  hosts: all
  ```
* Use **`ansible_nodename`** for conditions
* Ensure playbook runs with:

  ```bash
  ansible-playbook -i inventory playbook.yml
  ```
* No extra arguments should be required

---

##  Steps

### 1. Navigate to Ansible directory

```bash
cd /home/thor/ansible
ls -la
cat inventory
```

### 2. Create the playbook

```bash
touch playbook.yml
```

### 3. Add the playbook content

  Paste the following configuration  [`PLAYBOOK`](../files/ansible_playbook_conditionals_d93.yml).

### 4. Run the playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

##  Good to Know

###  Ansible Facts

* Facts are system details gathered automatically
* `ansible_nodename` → returns the hostname of the target machine
* Must enable:

  ```yaml
  gather_facts: yes
  ```


###  `when` Conditionals

* Control task execution dynamically
* Example:

  ```yaml
  when: ansible_nodename == "stapp01"
  ```
* Only executes on matching host


###  Common Conditional Operators

| Operator     | Meaning              |
| ------------ | -------------------- |
| `==`         | बराबर (equal)        |
| `!=`         | not equal            |
| `in`         | value exists in list |
| `not in`     | value not in list    |
| `is defined` | variable exists      |


###  File Permissions 

```bash
0744
```

Breakdown:

| User   | Permission           |
| ------ | -------------------- |
| Owner  | read, write, execute |
| Group  | read                 |
| Others | read                 |


###  Copy Module Behavior

* `src` → from **Ansible controller (jump host)**
* `dest` → on **target servers**
* Automatically creates file if it doesn’t exist
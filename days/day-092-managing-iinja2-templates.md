# Managing Jinja2 Templates Using Ansible

The Nautilus DevOps team is finalizing an Ansible role for installing and configuring the Apache (`httpd`) web server. As part of this setup, a dynamic `index.html` file must be deployed using a Jinja2 template.

This task ensures that the deployed web page reflects the correct server hostname dynamically without hardcoding values, following Ansible best practices.


##  Steps

### 1. Navigate to Ansible Directory

```bash
cd ~/ansible
ls
```

### 2. Create Jinja2 Template

```bash
vi role/httpd/templates/index.html.j2
```

#### Add:

```j2
This file was created using Ansible on {{ inventory_hostname }}
```

### 3. Update Role Task

```bash
vi role/httpd/tasks/main.yml
```

####  Add:

```yaml
- name: Deploy index.html using Jinja2 template
  ansible.builtin.template:
    src: index.html.j2
    dest: /var/www/html/index.html
    mode: '0777'
    owner: "{{ ansible_user }}"
    group: "{{ ansible_user }}"
```

---

### 4. Update Playbook

```bash
vi playbook.yml
```

####  Final Version:

```yaml
---
- name: Run httpd role on App Server 2
  hosts: stapp02
  become: yes

  roles:
    - httpd
```

### 5. Run the Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

## 6. Verification Steps

###  Check File Content

```bash
ansible stapp02 -i inventory -m shell -a "cat /var/www/html/index.html"
```

---

##  Good to Know 

###  `inventory_hostname` vs `ansible_hostname`

* `inventory_hostname` → comes from inventory (`stapp02`) 
* `ansible_hostname` → actual machine hostname (may differ)

---

###  Why use `template` instead of `copy`

* `template` = supports **dynamic variables (Jinja2)** 
* `copy` = static files only


###  Why `mode: '0777'` as string

* YAML may misinterpret numeric values
* Always wrap permissions in quotes to avoid unexpected behavior


###  Dynamic Ownership (Best Practice)

```yaml
owner: "{{ ansible_user }}"
```

 Prevents hardcoding (`tony`, `steve`, etc.)
 Works across environments automatically


###  Common Failure Pitfalls 

| Issue                    | Cause                              |
| ------------------------ | ---------------------------------- |
| Wrong hostname in output | Used hardcoded value               |
| Permission not applied   | Missing quotes around `0777`       |
| Playbook fails           | Wrong host (`stapp02` mismatch)    |
| Owner incorrect          | Hardcoded user instead of variable |
| Template not applied     | Used `copy` instead of `template`  |


###  Fast Debug Trick 

```bash
ansible stapp02 -i inventory -m debug -a "msg={{ inventory_hostname }}"
```
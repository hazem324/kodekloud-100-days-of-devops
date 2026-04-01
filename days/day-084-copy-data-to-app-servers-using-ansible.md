#  Copy Data to App Servers using Ansible

The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using **Ansible**.

###  Objective

* Create an inventory file `/home/thor/ansible/inventory` on the jump host
* Define all application servers with their respective SSH credentials
* Create a playbook `/home/thor/ansible/playbook.yml`
* Copy the file:

  ```
  /usr/src/itadmin/index.html
  ```

  ➜ to all application servers at:

  ```
  /opt/itadmin/index.html
  ```

 **Important:**
Validation runs using:

```bash
ansible-playbook -i inventory playbook.yml
```

So the setup must work **without extra flags or arguments**.

---

#  Steps

## 1. Navigate to Ansible Directory

```bash
cd /home/thor/ansible
```

## 2. Create Inventory File

```bash
vi inventory
```

###  Add the following configuration:

```ini
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

## 3. Create Playbook

```bash
vi playbook.yml
```

###  Add the following content:

Add the following content: [`PLAYBOOK`](../files/ansible_playbook_copy_data_d84.yml).


## 4. Execute the Playbook

```bash
ansible-playbook -i inventory playbook.yml
```



---

#  Good to Know

##  Authentication in Ansible

* When servers use **different credentials**, define them per host in the inventory
* `ansible_ssh_pass` requires **sshpass** installed on the control node
* For production:

  * Use **SSH keys** instead of passwords 🔑
  * Or encrypt credentials using **Ansible Vault**


##  copy Module Deep Dive

* Copies files **from control node → managed nodes**
* Key parameters:

  * `src`: local file path (on jump host)
  * `dest`: remote destination path
  * `mode`: file permissions

 Common mistake:

* Missing or mis-indented `dest` → causes:

  ```
  "msg": "dest is required"
  ```

##  Privilege Escalation (`become`)

* Required when writing to protected directories like `/opt`
* Equivalent to running commands with `sudo`

```yaml
become: yes
```


##  Idempotency (Core Ansible Concept)

* Running the playbook multiple times:

  * Will NOT duplicate files
  * Will NOT recreate existing directories unnecessarily
* Ensures safe, repeatable deployments 


##  Execution Behavior

* Tasks run **in parallel** across all hosts
* If one host fails:

  * Others continue unless it's a critical failure
* Results summary:

  * `ok` → no change needed
  * `changed` → modification applied


##  Debugging Tips

* Increase verbosity:

  ```bash
  ansible-playbook -i inventory playbook.yml -vvv
  ```
* Check file exists on source:

  ```bash
  ls /usr/src/itadmin/index.html
  ```
* Install sshpass if needed:

  ```bash
  sudo yum install -y sshpass
  ```


##  Best Practices

* Always **create directories before copying files**
* Use **explicit permissions** (`mode`) for security
* Avoid hardcoding passwords → prefer **Vault or SSH keys**
* Keep playbooks **simple and readable**


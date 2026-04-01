# Troubleshoot and Create Ansible Playbook

An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details:

* The inventory file `/home/thor/ansible/inventory` requires adjustments. The playbook must run on **App Server 3 in Stratos DC**. Update the inventory accordingly.

* Create a playbook `/home/thor/ansible/playbook.yml`. Include a task to create an empty file `/tmp/file.txt` on **App Server 3**.

---

## Steps

### 0. Move into ansible directory

```sh
cd /home/thor/ansible
ls
```

### 1. Fix the inventory file

Edit the inventory file:

```sh
vi /home/thor/ansible/inventory
```

Update it with the correct configuration for **App Server 3**:

```ini
[app]
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

> Ensure:
>
> * You are targeting `stapp03`
> * Correct username: `banner`
> * Correct password: `BigGr33n`
> * Avoid using variables like `$pwd` (Ansible does not interpret shell variables)

### 2. Create the playbook

Create the playbook file:

```sh
vi /home/thor/ansible/playbook.yml
```

Add the following content: [`playbook`](../files/ansible_playbook__create_file_d83.yml).



### 3. Run the playbook

Execute the playbook using:

```sh
ansible-playbook -i inventory playbook.yml
```

---

## Good to Know

### Ansible Troubleshooting

* **Connection Issues**: Verify SSH connectivity and credentials (user/password must match target server)
* **Inventory Problems**: Ensure correct host (`stapp03`) and variables are defined
* **Module Errors**: Validate correct syntax of the `file` module
* **Permission Issues**: Use `become: yes` to ensure file creation permissions


### Common Issues

* **Host Unreachable**: Incorrect hostname or SSH configuration
* **Authentication Failed**: Wrong username or password (e.g., using `$pwd` instead of actual password)
* **Module Not Found**: Incorrect module usage or typo in YAML
* **Syntax Errors**: Improper YAML indentation or formatting


### Debugging Techniques

* **Verbose Mode**:

  ```sh
  ansible-playbook -i inventory playbook.yml -vvv
  ```
* **Check Mode (Dry Run)**:

  ```sh
  ansible-playbook -i inventory playbook.yml --check
  ```
* **Limit Hosts**:

  ```sh
  ansible-playbook -i inventory playbook.yml --limit stapp03
  ```
* **Ping Test**:

  ```sh
  ansible -i inventory app -m ping
  ```


### File Operations (Relevant to This Task)

* **file Module**:

  * Used to create, delete, or modify files
  * `state: touch` → creates file if it does not exist

* **copy Module**:

  * Copy files from control node to managed nodes

* **template Module**:

  * Generate dynamic files using Jinja2 templates

* **lineinfile Module**:

  * Modify specific lines in existing files




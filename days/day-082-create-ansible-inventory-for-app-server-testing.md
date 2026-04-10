#  Create Ansible Inventory for App Server Testing

The Nautilus DevOps team is validating Ansible playbooks stored under `/home/thor/playbook/` on the jump host. To enable execution against **App Server 3 in Stratos DC**, an inventory file must be created with the correct configuration.

---

##  Objective

* Create an **INI-based Ansible inventory**
* Target **App Server 3 (`stapp03`)**
* Ensure compatibility with validation command:

```bash
ansible-playbook -i inventory playbook.yml
```

---

##  Requirements

*  Inventory file path: `/home/thor/playbook/inventory`
*  Target host: `stapp03`
*  Provide SSH credentials
*  No extra CLI arguments allowed during execution

---

##  Steps

### 1. Navigate to Playbook Directory

```bash
cd /home/thor/playbook
```

### 2. Create the Inventory File

```bash
vi inventory
```

### 3. Add the Following Configuration

```ini
[app_servers]
stapp03 ansible_host=stapp03

[app_servers:vars]
ansible_user=tony
ansible_ssh_pass=Ir0nM@n
```

### 4. Validate Connectivity 

```bash
ansible -i inventory app_servers -m ping
```

[![verify connection](../screenshots/Screenshot-day-82-verify-connection.png)](../screenshots/Screenshot-day-82-verify-connection.png)

---

##  Good to Know

###  Ansible Inventory

* **Purpose**: Defines the infrastructure Ansible manages
* **Role**: Maps control node → managed nodes
* **Formats**: Supports **INI, YAML, and JSON**
* **Dynamic Inventory**: Can fetch hosts from cloud providers (AWS, Azure, etc.)


###  Inventory Structure

* **Hosts**: Individual machines (e.g., `web01`, `db01`)
* **Groups**: Logical collections of hosts (e.g., `webservers`, `databases`)
* **Nested Groups**: Groups can contain other groups
* **Host Variables**: Variables applied to a specific host
* **Group Variables**: Variables shared across a group


###  Connection Variables

* **ansible_user**: SSH username used for connection
* **ansible_password / ansible_ssh_pass**: SSH password (avoid in production)
* **ansible_host**: Target hostname or IP address
* **ansible_port**: SSH port (default: `22`)
* **ansible_connection**: Connection type (e.g., `ssh`, `local`)
* **ansible_ssh_private_key_file**: Path to SSH private key 


###  Execution Behavior

* **Ad-hoc Commands**: Quick one-line operations (`ansible all -m ping`)
* **Playbooks**: YAML files defining automation workflows
* **Idempotency**: Tasks run only if changes are needed
* **Parallelism**: Executes tasks on multiple hosts simultaneously


###  Security Practices

* Use **SSH keys instead of passwords** 
* Store sensitive data with **Ansible Vault**
* Avoid hardcoding credentials in inventory files
* Limit privilege escalation (`become`) carefully


###  Best Practices

* **Use meaningful group names** (e.g., `app_servers`, `db_servers`)
* **Separate environments** (dev, staging, prod)
* **Keep inventory clean and modular**
* **Document variables and structure clearly**
* **Test connectivity before running playbooks** (`ansible -m ping`)



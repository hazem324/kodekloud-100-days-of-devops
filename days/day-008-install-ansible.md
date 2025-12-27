# Install Ansible Version 4.8.0 on Jump Host

## Task Description

The Nautilus DevOps team has decided to adopt Ansible as their primary automation and configuration management tool due to its simplicity, agentless architecture, and minimal prerequisites. To begin testing Ansible, the jump host will be used as the Ansible control node to manage and automate tasks on other servers in the environment.

### Objective

Install Ansible version **4.8.0** on the jump host using `pip3` only. Ensure the Ansible binary is available globally so that all users on the system can run Ansible commands.

## 🛠️ Steps to Complete the Task

### 1- Update System Packages

Make sure your system packages are up to date:

```bash
sudo dnf update -y
```

---

### 2- Install Python 3 and pip3

Ansible installed via `pip` requires Python:

```bash
sudo dnf install python3 python3-pip -y
```

---

### 3- Remove Existing Ansible Packages (If Any)

To avoid conflicts with OS-installed or older versions:

```bash
sudo pip3 uninstall ansible ansible-base -y
```

---

### 4- Install Ansible 4.8.0 Using pip3

Install Ansible **system-wide** so all users can run it:

```bash
sudo pip3 install ansible==4.8.0
```

---

### 5- Verify the Installation

Check the installed Ansible version:

```bash
ansible --version
```

View full package details:

```bash
pip3 show ansible
```

🔎 **Note**

* The `ansible --version` command shows the **ansible-core** version.
* For **Ansible 4.8.0**, the core engine version is:

```
ansible-core 2.11.12
```

---
## 🧠 Good to Know ?

### ❓ What Is Ansible?

**Ansible** is an open-source automation tool used for:

* Configuration management
* Application deployment
* Infrastructure provisioning
* Orchestration

#### Why Ansible?

*  Agentless (no software installed on managed nodes)
*  Uses human-readable **YAML**
*  Simple, powerful, and scalable
*  Secure (SSH-based)

---

### ⚙️ Ansible in DevOps

Ansible is a key DevOps tool that bridges **development** and **operations** through automation:

* **Infrastructure as Code (IaC)**
  Manage infrastructure like application code

* **CI/CD Automation**
  Automate deployments and environment setup

* **Consistency & Reliability**
  Eliminate configuration drift

* **Faster Delivery**
  Automate repetitive operational tasks

Used widely for:

* Multi-tier application deployments
* Cloud provisioning
* Network automation
* Complex workflow orchestration

---

### 📘 Ansible Fundamentals

* **Agentless** – No agent required on managed nodes
* **SSH-Based** – Secure remote communication
* **YAML Playbooks** – Easy-to-read automation definitions
* **Idempotent** – Safe to run multiple times
* **Push-Based** – Control node pushes changes
* **Modular** – Large library of reusable modules

---

### 🧩 Core Ansible Components

| Component         | Description                               |
| ----------------- | ----------------------------------------- |
| **Control Node**  | Machine running Ansible (e.g., jump host) |
| **Managed Nodes** | Target systems (no agent required)        |
| **Inventory**     | List of managed hosts (INI or YAML)       |
| **Playbooks**     | YAML files defining desired state         |
| **Modules**       | Small programs performing tasks           |
| **Roles**         | Structured, reusable playbook components  |
| **Facts**         | System data collected from nodes          |

---

###  Additional Key Concepts

* **Collections**
  Since Ansible 2.10+, modules and plugins are bundled as collections

* **Python Requirement**
  Control node requires **Python 3.6+**

* **Versioning Model**
  The `ansible` package bundles:

  * `ansible-core`
  * Community collections

* **Ad-hoc Commands**
  Quick one-liners for simple tasks:

  ```bash
  ansible all -m ping
  ```

* **Idempotency Advantage**
  Ensures safe re-execution in production environments

* **Community & Ecosystem**
  Backed by Red Hat with thousands of roles on **Ansible Galaxy**

---


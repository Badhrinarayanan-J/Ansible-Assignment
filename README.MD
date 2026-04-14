# 🚀 Ansible Assignment Repository

## 📌 Overview

This repository contains a complete set of **hands-on Ansible assignments** covering core concepts such as:

* Inventory management
* Ad-hoc commands
* Playbooks
* Roles
* Multi-node configuration
* Environment-based deployments (Test & Prod)

Each assignment builds progressively toward real-world DevOps automation.

---

## 📁 Repository Structure

```bash
Ansible-Assignment/
├── assignment-1/
├── assignment-2/
├── assignment-3/
├── assignment-4/
├── assignment-5/
└── README.md
```

---

## 🧠 Assignments Summary

| Assignment   | Description                                     |
| ------------ | ----------------------------------------------- |
| Assignment 1 | Basic Ansible setup & connectivity              |
| Assignment 2 | Script execution using Ansible                  |
| Assignment 3 | Role-based installation (Apache & Nginx)        |
| Assignment 4 | File deployment using roles (custom index.html) |
| Assignment 5 | Multi-node setup with Test & Prod environments  |

---

# ⚙️ Ansible Installation & Setup

## 🔹 Install Ansible (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install ansible -y
```

---

## 🔹 Verify Installation

```bash
ansible --version
```

---

## 🔹 Setup SSH Access (IMPORTANT)

```bash
ssh-keygen
ssh-copy-id user@host
```

👉 Required for passwordless authentication

---

## 🔹 Basic Inventory File

```ini
[servers]
node1 ansible_host=127.0.0.1 ansible_user=node1
node2 ansible_host=127.0.0.1 ansible_user=node2
```

---

## 🔹 Test Connectivity

```bash
ansible all -i inventory.ini -m ping
```

---

# 🚀 Common Ansible Commands

## 🔹 Ad-hoc Commands

```bash
ansible all -i inventory.ini -m ping
ansible all -i inventory.ini -a "uptime"
ansible all -i inventory.ini -a "df -h"
```

---

## 🔹 Install Packages

```bash
ansible all -i inventory.ini -m apt -a "name=nginx state=present" --become
```

---

## 🔹 Manage Services

```bash
ansible all -i inventory.ini -m service -a "name=nginx state=started" --become
```

---

## 🔹 Copy Files

```bash
ansible all -i inventory.ini -m copy -a "src=file.txt dest=/tmp/file.txt"
```

---

## 🔹 Run Playbook

```bash
ansible-playbook -i inventory.ini site.yml
```

---

# 📜 Ansible Playbook Example

```yaml
- name: Install Nginx
  hosts: all
  become: yes

  tasks:
    - name: Install package
      apt:
        name: nginx
        state: present
```

---

# 🏗️ Ansible Roles Structure

```bash
roles/
├── role_name/
│   ├── tasks/
│   │   └── main.yml
│   ├── handlers/
│   ├── files/
│   ├── templates/
│   └── vars/
```

---

# ⚡ Ansible Cheat Sheet

## 🔹 Inventory

```bash
ansible-inventory -i inventory.ini --list
ansible all --list-hosts
```

---

## 🔹 Debugging

```bash
ansible-playbook site.yml -vvv
```

---

## 🔹 Privilege Escalation

```bash
--become
```

---

## 🔹 Check Mode (Dry Run)

```bash
ansible-playbook site.yml --check
```

---

## 🔹 Limit Execution

```bash
ansible-playbook site.yml --limit test
```

---

## 🔹 Tags

```bash
ansible-playbook site.yml --tags "install"
```

---

## 🔹 Gather Facts

```bash
ansible all -m setup
```

---

# 🧠 Key Concepts Learned

* Infrastructure as Code (IaC)
* Idempotency in automation
* Role-based architecture
* Multi-environment deployments
* Debugging Ansible errors
* YAML syntax handling

---

# 📊 Real-World Skills Gained

* Managing multiple nodes using Ansible
* Automating software installation
* Handling production vs test environments
* Writing reusable roles
* Troubleshooting deployment issues

---

# 👨‍💻 Author

**Badhrinarayanan J**

---

# ⭐ Final Note

This repository demonstrates practical Ansible skills suitable for:

* DevOps Engineer roles
* Automation Engineer roles
* Infrastructure Management

---

🔥 *From basic commands to real-world automation — this repo covers it all.*

# 📘 Ansible Assignment-1 – Multi-Node Setup & Package Installation

## 🎯 Objective

The goal of this assignment is to:

1. Set up an Ansible environment with multiple nodes (1 control node + 2 managed nodes)
2. Install:

   * Java on **slave1**
   * MySQL Server on **slave2**
3. Automate the entire process using Ansible Playbooks

---

## 🧱 Project Structure

```
assignment-1/
├── inventory.ini
└── setup.yml
```

---

## ⚙️ Prerequisites

* Ansible installed on control node
* SSH access configured between control node and managed nodes
* Passwordless SSH (recommended)
* Sudo privileges on remote users
* Linux-based environment (Ubuntu preferred)

---

## 🌐 Architecture Overview

```
Control Node (Ansible)
        |
        | SSH
        ↓
-------------------------
|       |              |
slave1  slave2
(Java)  (MySQL)
```

---

## 📄 Inventory File (`inventory.ini`)

Defines target hosts and connection details:

```ini
[java]
slave1 ansible_host=127.0.0.1 ansible_user=slave1

[mysql]
slave2 ansible_host=127.0.0.1 ansible_user=slave2
```

---

## 🧾 Playbook (`setup.yml`)

This playbook performs:

* Java installation on slave1
* MySQL installation and service setup on slave2

```yaml
---
- name: Install Java on Slave1
  hosts: java
  become: yes

  tasks:
    - name: Install Java
      apt:
        name: openjdk-11-jdk
        state: present
        update_cache: yes

- name: Install MySQL on Slave2
  hosts: mysql
  become: yes

  tasks:
    - name: Install MySQL Server
      apt:
        name: mysql-server
        state: present
        update_cache: yes

    - name: Start MySQL Service
      service:
        name: mysql
        state: started
        enabled: yes
```

---

## ▶️ Execution

Run the playbook using:

```bash
ansible-playbook -i inventory.ini setup.yml
```

---

## ✅ Verification

### 🔹 Verify Java Installation

```bash
ssh slave1@localhost
java -version
```

---

### 🔹 Verify MySQL Installation

```bash
ssh slave2@localhost
mysql --version
```

---

## 🧠 Key Learnings

* Setting up an Ansible control node and managed nodes
* Writing inventory files and grouping hosts
* Using Ansible modules:

  * `apt` for package installation
  * `service` for managing services
* Privilege escalation using `become: yes`
* Remote automation using SSH
* Understanding idempotency (safe re-runs)

---

## ⚠️ Common Issues Faced

| Issue                   | Cause                   | Solution                                         |
| ----------------------- | ----------------------- | ------------------------------------------------ |
| SSH connection failed   | No key setup            | Configure SSH keys                               |
| Missing sudo password   | No privilege escalation | Use `become: yes` or configure passwordless sudo |
| Host unreachable        | Wrong inventory config  | Verify IP/hostnames                              |
| Package install failure | Missing update          | Use `update_cache: yes`                          |

---

## 🚀 Improvement (Production Approach)

* Use **roles** instead of single playbook
* Separate inventory for environments (dev, prod)
* Use variables for package versions
* Add handlers for service restart

---

## 🎉 Conclusion

This assignment demonstrates:

* Infrastructure automation using Ansible
* Multi-node configuration management
* Real-world DevOps workflow for software provisioning

It builds the foundation for advanced topics like roles, CI/CD, and infrastructure as code.

---

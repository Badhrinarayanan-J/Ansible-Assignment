# 📦 Assignment 5 - Ansible Multi-Node Deployment (Test & Prod)

## 📌 Objective

This assignment demonstrates how to configure and manage a **5-node Ansible cluster** using **role-based automation**.

### Tasks Performed:

1. Created a new Ansible cluster with 5 nodes
2. Grouped nodes into:

   * **Test Nodes** (2 nodes)
   * **Production Nodes** (2 nodes)
3. Installed **Java** on test nodes
4. Installed **MySQL Server** on production nodes
5. Implemented all configurations using **Ansible roles**

---

## 🏗️ Project Structure

```
assignment-5/
├── inventory.ini
├── site.yml
├── java_role/
│   └── tasks/
│       └── main.yml
├── mysql_role/
│   └── tasks/
│       └── main.yml
```

---

## ⚙️ Inventory Configuration

```ini
[test]
node1
node2

[prod]
node3
node4

[all]
node1
node2
node3
node4
node5
```

---

## 🚀 Roles Implementation

### 🔹 Java Role (Test Nodes)

```yaml
- name: Install Java
  apt:
    name: openjdk-11-jdk
    state: present
    update_cache: yes
```

---

### 🔹 MySQL Role (Production Nodes)

```yaml
- name: Install MySQL
  apt:
    name: mysql-server
    state: present
    update_cache: yes

- name: Start MySQL
  service:
    name: mysql
    state: started
    enabled: yes
```

---

## 📜 Playbook (site.yml)

```yaml
- name: Install Java on test nodes
  hosts: test
  become: yes
  roles:
    - java_role

- name: Install MySQL on prod nodes
  hosts: prod
  become: yes
  roles:
    - mysql_role
```

---

## ▶️ Execution

Run the playbook using:

```bash
ansible-playbook -i inventory.ini site.yml
```

---

## ✅ Verification

### 🔹 Check Java Installation

```bash
ansible test -i inventory.ini -a "java -version"
```

### 🔹 Check MySQL Service

```bash
ansible prod -i inventory.ini -a "systemctl status mysql"
```

---

## 📊 Result

* Java successfully installed on **test nodes (node1, node2)** ✅
* MySQL successfully installed and running on **production nodes (node3, node4)** ✅
* Playbook executed without errors ✅

---

## 🧠 Key Learnings

* Inventory grouping (**test vs prod**)
* Role-based architecture in Ansible
* Service management using Ansible modules
* Handling permissions and privilege escalation (`become`)
* Debugging YAML and runtime issues

---

## 👨‍💻 Author

**Badhrinarayanan J**

---

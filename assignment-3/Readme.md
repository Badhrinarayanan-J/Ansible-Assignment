# 📘 Assignment-3: Ansible Roles for Apache & Nginx Setup

## 🎯 Objective

This assignment demonstrates how to use **Ansible Roles** to install and configure:

* **Apache2** on `slave1`
* **Nginx** on `slave2`

Each service is managed using **separate Ansible roles**, following best practices.

---

## 🏗️ Project Structure

```
assignment-3/
│── inventory.ini
│── site.yml
│
├── apache_role/
│   ├── tasks/
│   │   └── main.yml
│
├── nginx_role/
│   ├── tasks/
│   │   └── main.yml
```

---

## 🖥️ Inventory Configuration

```ini
[java]
slave1 ansible_host=127.0.0.1 ansible_user=slave1

[mysql]
slave2 ansible_host=127.0.0.1 ansible_user=slave2
```

---

## ⚙️ Roles Implementation

### 🔹 Apache Role (`apache_role`)

Installs and starts Apache on `slave1`.

```yaml
---
- name: Install Apache
  apt:
    name: apache2
    state: present
    update_cache: yes

- name: Start Apache
  service:
    name: apache2
    state: started
    enabled: yes
```

---

### 🔹 Nginx Role (`nginx_role`)

Installs and runs Nginx on `slave2` using a custom port to avoid conflicts.

```yaml
---
- name: Install Nginx
  apt:
    name: nginx
    state: present
    update_cache: yes

- name: Change Nginx port to 9090
  replace:
    path: /etc/nginx/sites-available/default
    regexp: 'listen 80 default_server;'
    replace: 'listen 9090 default_server;'

- name: Change IPv6 port to 9090
  replace:
    path: /etc/nginx/sites-available/default
    regexp: 'listen \[::\]:80 default_server;'
    replace: 'listen [::]:9090 default_server;'

- name: Validate Nginx configuration
  command: nginx -t

- name: Start Nginx
  service:
    name: nginx
    state: restarted
    enabled: yes
```

---

## 🚀 Playbook (`site.yml`)

```yaml
---
- name: Install Apache on slave1
  hosts: java
  become: yes
  roles:
    - apache_role

- name: Install Nginx on slave2
  hosts: mysql
  become: yes
  roles:
    - nginx_role
```

---

## ▶️ Execution

Run the playbook:

```bash
ansible-playbook -i inventory.ini site.yml
```

---

## 🧪 Verification

### ✅ Apache (Port 80)

```bash
curl http://localhost
```

### ✅ Nginx (Port 9090)

```bash
curl http://localhost:9090
```

---

## ⚠️ Challenges Faced

* **Port Conflict Issue**

  * Apache was using port `80`
  * Jenkins was using port `8080`
  * Nginx initially failed to start

* **Solution**

  * Configured Nginx to run on port `9090`

---

## 🧠 Key Learnings

* Creating and using **Ansible roles**
* Managing services with **Ansible playbooks**
* Handling **port conflicts**
* Debugging service failures using:

  ```bash
  nginx -t
  systemctl status nginx
  ```
* Writing modular and reusable automation code

---

## 📌 Final Status

| Component          | Status       |
| ------------------ | ------------ |
| Apache Role        | ✅ Working    |
| Nginx Role         | ✅ Working    |
| Playbook Execution | ✅ Successful |

---

## 🚀 Conclusion

This assignment successfully demonstrates how to:

* Use **Ansible roles** for modular automation
* Deploy multiple services on different hosts
* Handle real-world issues like **port conflicts**

---

🔥 This setup reflects real DevOps practices and prepares for advanced automation tasks.

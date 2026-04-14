# 📘 Assignment-4: Deploy Custom Web Page Using Ansible Role

## 🎯 Objective

This assignment demonstrates how to:

* Reuse an existing **Ansible role (nginx_role)**
* Deploy a **custom `index.html` file**
* Replace the default Nginx web page
* Apply changes **only on the Nginx node (slave2)**

---

## 🏗️ Project Structure

```bash
assignment-4/
│── inventory.ini
│── site.yml
│
└── nginx_role/
    ├── tasks/
    │   └── main.yml
    └── files/
        └── index.html
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

## 📄 Custom Web Page

File: `nginx_role/files/index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Assignment 4</title>
</head>
<body>
    <h1>🚀 Assignment 4 Success</h1>
    <p>Deployed using Ansible Role</p>
</body>
</html>
```

---

## ⚙️ Role Implementation (`nginx_role`)

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

- name: Copy custom index.html
  copy:
    src: index.html
    dest: /var/www/html/index.html
    owner: www-data
    group: www-data
    mode: '0644'

- name: Validate Nginx configuration
  command: nginx -t

- name: Restart Nginx
  service:
    name: nginx
    state: restarted
    enabled: yes
```

---

## 🚀 Playbook (`site.yml`)

```yaml
---
- name: Deploy custom web page on Nginx server
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

```bash
curl http://localhost:9090
```

Or open in browser:

```text
http://localhost:9090
```

👉 Output:

```
🚀 Assignment 4 Success
Deployed using Ansible Role
```

---

## ⚠️ Challenges Faced

* **Port Conflict Issue**

  * Apache (port 80)
  * Jenkins (port 8080)

* **Solution**

  * Configured Nginx to run on port `9090`

---

## 🧠 Key Learnings

* Reusing existing Ansible roles
* Using `files/` directory in roles
* Deploying static web content via Ansible
* Managing port conflicts effectively
* Validating configurations using:

  ```bash
  nginx -t
  ```

---

## 📌 Final Status

| Component          | Status       |
| ------------------ | ------------ |
| Nginx Role         | ✅ Working    |
| File Deployment    | ✅ Successful |
| Playbook Execution | ✅ Completed  |

---

## 🚀 Conclusion

This assignment demonstrates:

* Efficient reuse of roles
* Real-world deployment of web content
* Clean separation of environments

---

## ✍️ Author

**Badhrinarayanan J**

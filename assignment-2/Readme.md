# 📘 Ansible Assignment-2 – Run Custom Script on All Hosts

## 🎯 Objective

The goal of this assignment is to:

1. Create a custom shell script that writes text to a file (`/tmp/1.txt`)
2. Execute this script on multiple remote hosts using Ansible

---

## 🧱 Project Structure

```
assignment-2/
├── inventory.ini
├── run_script.yml
└── add_text.sh
```

---

## ⚙️ Prerequisites

* Ansible installed on control node
* SSH access configured (passwordless recommended)
* Two target hosts (slave1, slave2)
* Sudo privileges configured for remote users

---

## 📄 Inventory File (`inventory.ini`)

Defines the target hosts:

```ini
[java]
slave1 ansible_host=127.0.0.1 ansible_user=slave1

[mysql]
slave2 ansible_host=127.0.0.1 ansible_user=slave2
```

---

## 📝 Script File (`add_text.sh`)

This script appends a line to `/tmp/1.txt`.

```bash
#!/bin/bash
echo "This text has been added by custom script" >> /tmp/1.txt
```

### 🔐 Permissions

```bash
chmod +x add_text.sh
```

---

## 📘 Playbook (`run_script.yml`)

This playbook:

* Copies the script to remote hosts
* Executes it with elevated privileges

```yaml
---
- name: Run custom script on all hosts
  hosts: all
  become: yes

  tasks:
    - name: Copy script to remote machines
      copy:
        src: add_text.sh
        dest: /tmp/add_text.sh
        mode: '0755'

    - name: Execute script
      command: /tmp/add_text.sh
      become: yes
```

---

## ▶️ Execution

Run the playbook using:

```bash
ansible-playbook -i inventory.ini run_script.yml
```

---

## ✅ Verification

Check the output file on each host:

```bash
ssh slave1@localhost
cat /tmp/1.txt

ssh slave2@localhost
cat /tmp/1.txt
```

Expected output:

```
This text has been added by custom script
```

---

## 🧠 Key Learnings

* How to execute shell scripts remotely using Ansible
* Importance of `become: yes` for privilege escalation
* File permission and ownership handling in Linux
* Difference between local and remote execution
* Debugging common issues like:

  * SSH connectivity
  * Sudo permissions
  * File access errors

---

## ⚠️ Known Limitation

This script is **not idempotent**:

* Running the playbook multiple times will append duplicate lines

---

## 🚀 Improvement (Production Approach)

Instead of using a script, use Ansible’s built-in module:

```yaml
- name: Add text safely (no duplicates)
  lineinfile:
    path: /tmp/1.txt
    line: "This text has been added by custom script"
    create: yes
  become: yes
```

---

## 🎉 Conclusion

This assignment demonstrates how Ansible automates:

* File transfer
* Remote execution
* Multi-host management

It also highlights real-world debugging scenarios involving permissions and execution contexts.

---

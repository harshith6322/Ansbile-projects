# Ansible Projects 🚀

## 📌 Overview

This repository contains two Ansible-based automation projects:

1. **Exploring Ansible Modules** → Installing packages, managing users, copying files, cloning repos, and more.
2. **Deploying a React App on Apache/HTTPD** → End-to-end deployment of a React portfolio website.

Both projects use **Ansible playbooks**, secrets stored with **Ansible Vault**, and host groups defined in an `inventory.ini`. Images and example outputs are placed in an `images/` folder so the README can show architecture and run results.

---

## 🏗️ How Ansible Works (Architecture)

Ansible uses a **control node → managed nodes** model (agentless):

* **Control Node (Master):** Runs playbooks, connects to targets over SSH.
* **Managed Nodes (Slaves):** Target servers where modules execute. No agent required.
* **Inventory:** Host groups and hosts (in `inventory.ini`).
* **Playbooks:** YAML files (`ansible-playbook.yml`) that list plays and tasks.
* **Modules:** Units of work like `yum`, `apt`, `user`, `git`, `service`.

**Flow:** Control Node runs playbook → connects via SSH → executes modules → target converges to desired state.

### 📊 Architecture Diagram

Relative (preferred):

```markdown
![Ansible Architecture]("https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png")


```
 ```html
<div align="center">
  <img src="https://raw.githubusercontent.com/harshith6322/Ansbile-projects/blob/main/images/Ansible_Architecture.png" alt="Ansible Architecture" width="720px" hight="400px">
</div>
``` 



---

## ⚖️ Why Ansible (vs Chef / Puppet)

* **Agentless** — only SSH required (no agent on managed nodes).
* **Simple syntax** — YAML playbooks are readable and easy to maintain.
* **Fast to adopt** — quick to set up, widely adopted by DevOps teams.
* **Push model** — control node initiates changes, good for CI/CD integration.

---

## 📂 Project Structure

Use this structure in your repo:

```
.
├── project-1-modules/
│   ├── ansible-playbook.yml
│   ├── vault.yml
│   ├── ansible.cfg
│   └── inventory.ini
│
├── project-2-deploy-react-httpd/
│   ├── ansible-playbook.yml
│   ├── vault.yml
│   ├── ansible.cfg
│   └── inventory.ini
│
├── images/
│   ├── Ansible_Architecture.png
│   ├── project1-output.png
│   ├── project2-output.png
│   └── react-website.png
│
└── README.md
```

---

## 🔹 Project 1 — Playing with Ansible Modules

**Purpose:** Learn and demonstrate modules like `yum`, `apt`, `user`, `copy`, `git`, `shell`, and `debug`.

**Playbook highlights:**

* Conditional package installs using `ansible_facts['os_family']`.
* Create users with correct group/shell.
* Copy files and register command output, then display via `debug`.
* Clone GitHub repos using `git` module and secrets from `vault.yml`.

**Commands**

```bash
# Test connectivity
ansible -i inventory.ini all -m ping

# Run the playbook
ansible-playbook -i inventory.ini ansible-playbook.yml

# Vault: encrypt file
ansible-vault encrypt vault.yml

# Vault: decrypt file
ansible-vault decrypt vault.yml

# Vault: edit file
ansible-vault edit vault.yml
```

### ▶️ Sample Output

Add your run screenshots to `images/project1-output.png` and reference them:

```markdown
![Modules Playbook Output](images/project1-output.png)
```

---

## 🔹 Project 2 — Deploy React App on Apache/HTTPD

**Purpose:** Automate deploying a React portfolio to Apache (httpd / apache2 depending on OS).

**Playbook highlights:**

* Install `git` and `httpd`/`apache2` depending on OS family.
* Enable and start the web service.
* Clone React repo to `/tmp/website` and copy `index.html` + `assets/` to `/var/www/html/`.

**Commands**

```bash
# Run deployment (asks for vault password)
ansible-playbook -i inventory.ini ansible-playbook.yml --ask-vault-pass

# Check Apache service (on target node)
systemctl status httpd   # (RedHat)
systemctl status apache2 # (Debian/Ubuntu)
```

### ▶️ Sample Output

Place screenshots in:

* `images/project2-output.png` — playbook run output
* `images/react-website.png` — screenshot of served website

Reference them:

```markdown
![React App Deployment Output](images/project2-output.png)
![Website Running on Apache](images/react-website.png)
```

---

## 📖 Supported States

In playbooks you used state values like:

* `present` → ensure installed
* `absent` → ensure removed
* `latest` → upgrade to latest
* `started` / `stopped` / `restarted` → service control

---

## 🛠️ Common Commands Summary

* Ping all hosts:

  ```bash
  ansible -i inventory.ini all -m ping
  ```

* Vault:

  ```bash
  ansible-vault create vault.yml
  ansible-vault encrypt vault.yml
  ansible-vault decrypt vault.yml
  ansible-vault edit vault.yml
  ```

* Run playbook:

  ```bash
  ansible-playbook -i inventory.ini ansible-playbook.yml --ask-vault-pass
  ```

---

## 🔎 Example `inventory.ini`

Use this inventory (based on the hosts you gave). Save to `inventory.ini` and commit.

```ini
[slaves]
107.21.181.236
54.174.249.239

[amazon]
107.21.181.236

[ubuntu]
54.174.249.239
```

If you use SSH user/key, you can add per-host vars or a `group_vars/` folder. Example per-host with user/key:

```ini
[slaves]
107.21.181.236 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/id_rsa
54.174.249.239 ansible_user=ubuntu   ansible_ssh_private_key_file=~/.ssh/id_rsa
```

---





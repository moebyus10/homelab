# 🚀 Lab 27 — Complete Automation with Ansible

## 🎯 Lab Objective

The goal of this lab is to implement a Linux automation environment using **Ansible**.

The objective is to move from manual system administration to an **Infrastructure as Code (IaC)** approach by automating:

- SSH-based remote administration
- User management
- Package installation
- SSH security hardening
- System maintenance
- Server configuration through reusable Ansible roles

This lab introduces professional Ansible project organization and automation principles.

---

# 🏗️ Infrastructure Architecture

## Environment

| Host | Role | Operating System | IP Address |
|---|---|---|---|
| NOVATECH-ADMIN01 | Ansible Controller | Ubuntu 26.04 LTS | 192.168.56.10 |
| NODE01 | Managed Node | Ubuntu 26.04 LTS | 192.168.56.11 |

---

## Architecture Diagram

```
                    SSH ED25519
                         |
                         |
                         v

+----------------------+              +----------------------+
| NOVATECH-ADMIN01     |              | NODE01               |
| Ansible Controller   | ------------> | Managed Node         |
| Ubuntu 26.04 LTS     |              | Ubuntu 26.04 LTS     |
+----------------------+              +----------------------+

                 Ansible Automation
                         |
                         v

              Automated Linux Configuration
```

---

# 🛠️ Prerequisites

Requirements:

- Ubuntu 26.04 LTS
- Ansible installed
- SSH connectivity configured
- ED25519 SSH key authentication
- Sudo privileges on the managed node

Check Ansible installation:

```bash
ansible --version
```

Example:

```
ansible-core 2.20
```

---

# 📁 Project Structure

```
ansible-lab/
│
├── ansible.cfg
├── inventory
├── site.yml
│
├── files/
│   └── adminops.pub
│
├── roles/
│   │
│   ├── packages/
│   │   └── tasks/
│   │       └── main.yml
│   │
│   ├── users/
│   │   └── tasks/
│   │       └── main.yml
│   │
│   ├── ssh-hardening/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── handlers/
│   │       └── main.yml
│   │
│   └── update/
│       └── tasks/
│           └── main.yml
│
└── playbooks/
    ├── packages.yml
    ├── users.yml
    ├── ssh-hardening.yml
    └── update.yml
```

---

# 🔐 SSH Configuration

Communication between the Ansible controller and the managed node uses an ED25519 SSH key.

Test connection:

```bash
ssh moebyus@192.168.56.11
```

Successful connection:

```
Welcome to Ubuntu 26.04 LTS
```

SSH authentication is performed without password login.

---

# 📋 Ansible Inventory

File:

```
inventory
```

Configuration:

```ini
[servers]
NODE01 ansible_host=192.168.56.11 ansible_user=moebyus
```

Connection test:

```bash
ansible all -m ping
```

Expected result:

```
NODE01 | SUCCESS => {
    "ping": "pong"
}
```

---

# ⚙️ Ansible Configuration

File:

```
ansible.cfg
```

Configured features:

- Custom inventory location
- SSH configuration
- Host key checking management

---

# 🧩 Ansible Roles

The project uses reusable Ansible roles to separate responsibilities.

Implemented roles:

```
roles/
├── packages
├── users
├── ssh-hardening
└── update
```

---

# 📦 Role: packages

## Objective

Automatically install administration tools on managed nodes.

Installed packages:

- htop
- curl
- wget
- git
- vim
- net-tools
- ufw

Execution:

```bash
ansible-playbook site.yml
```

---

# 👤 Role: users

## Objective

Automate administrator account creation.

Actions performed:

- Create `adminops` user
- Add user to sudo group
- Create SSH directory
- Deploy SSH authorized key

Verification:

```bash
id adminops
```

Example:

```
uid=1004(adminops)
groups=sudo
```

---

# 🔒 Role: ssh-hardening

## Objective

Apply SSH security best practices.

Configuration applied:

```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Verification:

```bash
sudo sshd -T | grep -E "permitrootlogin|passwordauthentication|pubkeyauthentication"
```

Expected output:

```
permitrootlogin no
passwordauthentication no
pubkeyauthentication yes
```

SSH restart is managed through an Ansible handler.

---

# 🔄 Role: update

## Objective

Automate system maintenance tasks.

Actions performed:

- Update APT package cache
- Upgrade installed packages
- Remove unused packages

---

# ▶️ Full Deployment

The main entry point is:

```
site.yml
```

Example:

```yaml
---
- name: Homelab Configuration
  hosts: servers
  become: true

  roles:
    - packages
    - users
    - ssh-hardening
    - update
```

Run deployment:

```bash
ansible-playbook site.yml
```

Successful execution:

```
PLAY RECAP

NODE01 : ok=12 changed=0 failed=0
```

---

# 🔁 Idempotency Testing

The playbook was executed multiple times.

Second execution:

```
changed=0
```

This confirms:

- Ansible detects the current system state
- No unnecessary changes are applied
- Configuration remains reproducible

Idempotency is a core principle of Infrastructure as Code.

---

# ✅ Skills Acquired

During this lab, the following skills were developed:

- Ansible installation and configuration
- Inventory management
- SSH remote administration
- SSH key-based authentication
- Ansible role creation
- Infrastructure as Code principles
- Linux user automation
- SSH security hardening
- Automated system maintenance
- Idempotent deployments
- Professional Ansible project organization

---

# 📌 Conclusion

This lab establishes the foundation of automated Linux infrastructure management with Ansible.

The managed node can now be fully configured from the Ansible controller using a reusable and reproducible deployment process.

This automation framework will be reused in upcoming labs:

- Lab 28 — Multi-Server Infrastructure
- Lab 29 — SSH Bastion Host
- Lab 30 — PKI & TLS
- Lab 31 — Professional Backup
- Lab 32 — Virtualization & Infrastructure
- Lab 33 — SIEM / Wazuh

# 🤖 Ansible Basics

## 🎯 Objective

The goal of this lab was to discover Ansible and learn the fundamentals of infrastructure automation.

During this lab, the following concepts were covered:

- Installing Ansible
- Creating an inventory
- Running ad hoc commands
- Collecting system facts
- Creating playbooks
- Using Ansible modules
- Working with variables and registered outputs
- Running privileged tasks with `become`

---

## 📦 Installing Ansible

The package lists were updated and Ansible was installed.

```bash
sudo apt update
sudo apt install -y ansible
```

The installation was verified:

```bash
ansible --version
```

**Result:**

```text
ansible [core 2.20.1]
Python 3.14.4
```

Ansible was successfully installed and ready to use.

---

## 📁 Creating the Working Directory

A dedicated directory was created for the lab.

```bash
mkdir ~/ansible-lab
cd ~/ansible-lab
```

---

## 🖥️ Creating the Inventory

A local inventory file was created.

**inventory**

```ini
[local]
localhost ansible_connection=local
```

The inventory was verified:

```bash
cat inventory
```

---

## 📡 Testing Connectivity

The Ansible ping module was used to verify communication with the managed host.

```bash
ansible all -i inventory -m ping
```

**Result:**

```text
localhost | SUCCESS => {
    "ping": "pong"
}
```

Communication with the managed host was successful.

---

## ⚡ Running Ad Hoc Commands

Several one-time commands were executed without using a playbook.

Hostname:

```bash
ansible all -i inventory -m command -a "hostname"
```

Result:

```text
Vega
```

Kernel version:

```bash
ansible all -i inventory -m command -a "uname -r"
```

Result:

```text
7.0.0-28-generic
```

Disk usage:

```bash
ansible all -i inventory -m command -a "df -h /"
```

Result:

```text
/dev/sda2   40G   13G   25G   33%
```

Memory usage:

```bash
ansible all -i inventory -m command -a "free -h"
```

Docker version:

```bash
ansible all -i inventory -m command -a "docker --version"
```

Result:

```text
Docker version 29.7.1
```

---

## 🔎 Collecting System Facts

The `setup` module was used to retrieve system information.

Operating system:

```bash
ansible all -i inventory -m setup -a "filter=ansible_distribution*"
```

Result:

```text
Ubuntu 26.04
```

Kernel:

```bash
ansible all -i inventory -m setup -a "filter=ansible_kernel"
```

Result:

```text
7.0.0-28-generic
```

Processor:

```bash
ansible all -i inventory -m setup -a "filter=ansible_processor*"
```

Result:

```text
Intel(R) Core(TM) i5-1035G1 CPU @ 1.00GHz
```

---

## 📄 Creating the First Playbook

A playbook named `system-info.yml` was created.

```yaml
---
- name: System Information
  hosts: local
  gather_facts: true

  tasks:

    - name: Display hostname
      ansible.builtin.debug:
        msg: "Hostname: {{ ansible_hostname }}"

    - name: Display operating system
      ansible.builtin.debug:
        msg: "OS: {{ ansible_distribution }} {{ ansible_distribution_version }}"

    - name: Display kernel
      ansible.builtin.debug:
        msg: "Kernel: {{ ansible_kernel }}"

    - name: Display processor
      ansible.builtin.debug:
        msg: "CPU: {{ ansible_processor[2] }}"

    - name: Display memory
      ansible.builtin.debug:
        msg: "Memory: {{ ansible_memtotal_mb }} MB"
```

Execution:

```bash
ansible-playbook -i inventory system-info.yml
```

The playbook successfully displayed system information using Ansible facts.

---

## 🏥 Creating a Health Check Playbook

A second playbook named `homelab-check.yml` was created.

```yaml
---
- name: Homelab Health Check
  hosts: local
  become: true

  tasks:

    - name: Check SSH service
      ansible.builtin.service_facts:

    - name: Display SSH status
      ansible.builtin.debug:
        msg: "{{ ansible_facts.services['ssh.service'].state }}"

    - name: Check Docker version
      ansible.builtin.command: docker --version
      register: docker_version
      changed_when: false

    - name: Display Docker version
      ansible.builtin.debug:
        var: docker_version.stdout

    - name: Disk usage
      ansible.builtin.command: df -h /
      register: disk
      changed_when: false

    - name: Show disk usage
      ansible.builtin.debug:
        var: disk.stdout_lines
```

The playbook was executed using privilege escalation.

```bash
ansible-playbook -i inventory homelab-check.yml --ask-become-pass
```

Successful results included:

```text
SSH service: running
Docker version: 29.7.1
Disk usage: 33%
```

---

## 📚 Ansible Concepts Practiced

During this lab, the following Ansible features were used:

- Inventory
- Modules
- Ad hoc commands
- Playbooks
- Facts
- Variables
- Debug module
- Command module
- Service facts
- Register
- Become
- Privilege escalation

---

## ✅ Lab Validation

The following objectives were successfully completed:

- [x] Ansible installed
- [x] Inventory created
- [x] Local host managed
- [x] Connectivity verified
- [x] Ad hoc commands executed
- [x] Facts collected
- [x] First playbook created
- [x] Variables used
- [x] Registered outputs used
- [x] Health check playbook created
- [x] Privileged execution tested

---

## 🧠 Skills Practiced

- Infrastructure as Code
- Configuration Management
- Linux Automation
- YAML
- Playbook Development
- System Administration
- SSH Automation
- Infrastructure Inventory

---

## 🏁 Conclusion

This lab introduced the fundamentals of Ansible for infrastructure automation. A local inventory was configured, ad hoc commands were executed, system facts were collected, and two playbooks were created to automate administrative tasks and perform health checks. These concepts provide a solid foundation for managing multiple Linux servers through Infrastructure as Code.

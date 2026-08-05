# 🖥️ Lab 32 – Virtualization & Infrastructure (KVM / Libvirt)

## 📌 Objective

The objective of this lab was to deploy a complete virtualization infrastructure using **KVM**, **QEMU** and **libvirt** on an Ubuntu 26.04 host.

Unlike previous labs that focused on Linux administration inside a single virtual machine, this lab introduced a professional hypervisor environment capable of hosting multiple Linux virtual machines.

---

# 🏗️ Infrastructure

```
Windows 11 Host
        │
        ▼
Oracle VirtualBox
        │
        ▼
NOVATECH-ADMIN01 (Ubuntu Desktop 26.04)
│
├── KVM Hypervisor
├── Libvirt
├── Default Virtual Network (virbr0)
├── Storage Pool
│
├── NOVATECH-NODE01
│     ├── Ubuntu Server 26.04
│     ├── OpenSSH Server
│     ├── UFW
│     └── QEMU Guest Agent
│
└── NOVATECH-NODE02
      ├── Ubuntu Server 26.04
      ├── OpenSSH Server
      ├── UFW
      └── QEMU Guest Agent
```

---

# 🎯 Objectives

- Install KVM and Libvirt
- Enable Nested Virtualization
- Configure the Libvirt service
- Create a Storage Pool
- Create QCOW2 virtual disks
- Deploy Ubuntu Server virtual machines
- Configure networking
- Enable SSH access
- Install QEMU Guest Agent
- Validate the virtualization infrastructure

---

# 🖥️ Host Information

| Component | Value |
|------------|-------|
| Hostname | NOVATECH-ADMIN01 |
| OS | Ubuntu Desktop 26.04 LTS |
| Hypervisor | KVM |
| Virtualization | Nested (VirtualBox → KVM) |
| Virtual Machine Manager | Libvirt |
| Disk Format | QCOW2 |
| Network | virbr0 (192.168.122.0/24) |

---

# 📦 Installing KVM

Installed packages:

```bash
sudo apt update

sudo apt install \
qemu-system-x86 \
qemu-utils \
libvirt-daemon-system \
libvirt-clients \
virt-manager \
virtinst \
bridge-utils \
ovmf \
dnsmasq-base
```

---

# 🔍 Verifying Hardware Virtualization

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

Result:

```text
4
```

Checking KVM:

```bash
ls -l /dev/kvm
```

---

# ✅ Host Validation

```bash
sudo virt-host-validate
```

Validation confirmed:

- Hardware virtualization
- KVM support
- cgroups
- networking
- libvirt configuration

---

# 🔧 Configuring Libvirt

Checking service:

```bash
sudo systemctl status libvirtd
```

Starting service:

```bash
sudo systemctl enable --now libvirtd
```

---

# 👤 Permissions

Current user added to required groups:

```bash
sudo usermod -aG kvm,libvirt $USER
```

Groups verified:

```bash
groups
```

---

# 💾 Storage Pool

Creating default storage pool:

```bash
virsh pool-define-as default dir \
--target /var/lib/libvirt/images
```

Starting pool:

```bash
virsh pool-start default
```

Autostart:

```bash
virsh pool-autostart default
```

Verification:

```bash
virsh pool-list --all
virsh pool-info default
```

---

# 💽 Creating Virtual Disks

NODE01

```bash
sudo qemu-img create \
-f qcow2 \
/var/lib/libvirt/images/NOVATECH-NODE01.qcow2 \
20G
```

NODE02

```bash
sudo qemu-img create \
-f qcow2 \
/var/lib/libvirt/images/NOVATECH-NODE02.qcow2 \
20G
```

Verification:

```bash
qemu-img info
```

---

# 📀 Ubuntu Server Installation

Ubuntu Server 26.04 ISO copied to:

```text
/var/lib/libvirt/images/
```

Installation performed using:

```bash
virt-install
```

Parameters:

- UEFI
- 2 vCPU
- 2 GB RAM
- QCOW2 storage
- Default libvirt network
- SPICE graphics

---

# 🌐 Network Configuration

Default libvirt network:

```text
192.168.122.0/24
```

Host:

```text
192.168.122.1
```

Guest virtual machines:

| Machine | Address |
|----------|---------|
| NOVATECH-NODE01 | 192.168.122.191 |
| NOVATECH-NODE02 | 192.168.122.163 |

---

# 🔐 SSH Configuration

OpenSSH installed on both guests.

SSH service:

```bash
systemctl status ssh
```

Firewall:

```bash
sudo ufw allow 22/tcp
```

Remote access tested successfully.

---

# ⚙️ QEMU Guest Agent

Installed on both guests:

```bash
sudo apt install qemu-guest-agent
```

Service:

```bash
systemctl status qemu-guest-agent
```

Status:

```text
Active (running)
```

---

# 🖧 Host Resolution

`/etc/hosts` updated:

```text
192.168.122.191 NOVATECH-NODE01
192.168.122.163 NOVATECH-NODE02
```

Connectivity tested:

```bash
ping NOVATECH-NODE01
ping NOVATECH-NODE02

ssh moebyus@NOVATECH-NODE01
ssh moebyus@NOVATECH-NODE02
```

---

# 🔍 Virtual Machine Management

Listing VMs:

```bash
virsh list --all
```

Displaying guest IPs:

```bash
virsh domifaddr NOVATECH-NODE01
virsh domifaddr NOVATECH-NODE02
```

Managing storage:

```bash
virsh vol-list default
```

---

# 🧪 Validation

Infrastructure successfully validated.

- ✅ KVM operational
- ✅ Nested Virtualization enabled
- ✅ Libvirt running
- ✅ Storage Pool configured
- ✅ QCOW2 virtual disks
- ✅ Ubuntu Server deployed
- ✅ SSH operational
- ✅ QEMU Guest Agent running
- ✅ Network connectivity validated
- ✅ Hostname resolution configured

---

# 📚 Skills Acquired

- KVM Hypervisor
- Nested Virtualization
- Libvirt administration
- Virtual storage management
- QCOW2 image management
- Virtual networking
- SSH infrastructure
- Linux server deployment
- QEMU Guest Agent
- Infrastructure management
- Enterprise virtualization fundamentals

---

# Conclusion

This lab successfully introduced a complete enterprise virtualization environment based on **KVM**, **QEMU**, and **libvirt**.

A Linux workstation was transformed into a virtualization host capable of running multiple Ubuntu Server virtual machines while using **nested virtualization** inside Oracle VirtualBox. Throughout the lab, several real-world issues—including KVM activation, libvirt configuration, storage pool creation, ISO permissions, networking, and SSH connectivity—were identified and resolved.

Two independent virtual servers (**NOVATECH-NODE01** and **NOVATECH-NODE02**) were successfully deployed, configured, and managed from the virtualization host. Communication between all systems was validated, confirming that the infrastructure is fully operational.

This lab provided practical experience with enterprise virtualization concepts such as hypervisor management, virtual networking, storage provisioning, guest administration, and infrastructure troubleshooting. These skills form a solid foundation for advanced Linux system administration and enterprise infrastructure management.


**Completed successfully ✔️**

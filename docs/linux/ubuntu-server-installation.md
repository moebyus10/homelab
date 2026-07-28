# Ubuntu Server VM Deployment on VirtualBox

## 📌 Objective

The purpose of this lab is to deploy an Ubuntu Server 24.04 virtual machine using an existing virtual appliance on Oracle VirtualBox.

This lab focuses on understanding virtual machine deployment, hardware configuration, network settings and preparing a Linux environment for future administration and cybersecurity exercises.
---

## 🖥️ Lab Environment

| Component | Value |
|-----------|-------|
| Host Operating System | Windows 11 |
| Hypervisor | Oracle VirtualBox 7.2.12 (r174389) |
| Guest Operating System | Ubuntu Server 24.04 LTS |
| Network Mode | NAT |
---

## 📦 Virtual Machine Source

The virtual machine was imported from a pre-built Ubuntu Server image provided by LinuxVMImages.com.

This approach allows a quick deployment of a ready-to-use Linux environment for learning system administration and networking concepts.
---

## 🎯 Learning Objectives

By completing this lab, I aim to:

- Create and configure a virtual machine using Oracle VirtualBox.
- Install Ubuntu Server 24.04 LTS.
- Understand the role of virtual hardware (CPU, RAM, storage, network).
- Verify that the operating system is correctly installed.
- Prepare a clean environment for future Linux and networking labs.
---

## 🖥️ Step 1 – Create the Virtual Machine

The first step is to create a new virtual machine using Oracle VirtualBox.

### Virtual Machine Configuration

| Setting | Value |
|---------|-------|
| Name | Ubuntu_24.04_VB_LinuxVMImages.COM |
| Type | Linux |
| Version | Ubuntu (64-bit) |
| Memory | 4096 MB |
| Processors | 2 vCPU |
| Virtual Disk | 25 GB (Dynamically Allocated VDI) |
| Network | NAT |

### Why these settings?

- **4096 MB of RAM** provides enough memory for Ubuntu Server while leaving sufficient resources for the host operating system.
- **2 virtual CPUs** offer good performance for administration and networking labs.
- A **25 GB dynamically allocated virtual disk** optimizes disk space usage while providing enough storage for future experiments.
- **NAT networking** allows the virtual machine to access the Internet without requiring additional network configuration.
  
### Virtual Machine Overview

![Virtual Machine Overview](../../screenshots/01-virtual-machine-overview.jpg)

## 🔎 Initial System Verification

After deploying the virtual machine, initial checks were performed to verify the operating system status, hardware configuration and network connectivity.

The virtual machine had already been used for network configuration exercises, therefore some settings may differ from the default imported image.

### System Information

The first verification step was to collect information about the operating system and virtual environment.

Command used:

```bash
hostnamectl
```

Main information collected:

| Parameter | Value |
|-----------|-------|
| Hostname | vega |
| Operating System | Ubuntu 24.04 LTS |
| Kernel | Linux 6.8.0-31-generic |
| Architecture | x86-64 |
| Hypervisor | Oracle VirtualBox |

### Ubuntu Version

Command used:

```bash
lsb_release -a
```

Result:

```
Ubuntu 24.04 LTS (Noble)
```

### Network Configuration

Command used:

```bash
ip a
```

Main network interface:

| Interface | IPv4 Address |
|-----------|--------------|
| enp0s3 | 10.0.2.20/24 |

The virtual machine uses a NAT network configuration provided by VirtualBox.

### Internet Connectivity Test

Command used:

```bash
ping -c 4 google.com
```

Result:

```
4 packets transmitted, 4 received, 0% packet loss
```

The test confirms that the virtual machine has working network connectivity and DNS resolution.

### Routing Configuration

Command used:

```bash
ip route
```

Current routing table:

```
default via 10.0.2.2 dev enp0s3
10.0.2.0/24 dev enp0s3
```

The virtual machine uses the VirtualBox NAT gateway:

```
10.0.2.2
```

During verification, two IPv4 configurations were detected on the same interface:

- Static address: `10.0.2.20/24`
- DHCP address: `10.0.2.15/24`

This configuration comes from previous networking exercises. A future lab will focus on cleaning and standardizing the network configuration.
---

## 🛠️ Commands Used

During this lab, the following commands were used to verify the system:

```bash
hostnamectl
lsb_release -a
ip a
ip route
ping -c 4 google.com
```

These commands allowed verification of:
- system information;
- Ubuntu version;
- network interfaces;
- routing configuration;
- Internet connectivity.

---

## 📖 What I Learned

Through this lab, I learned how to:

- Deploy a Linux virtual machine using VirtualBox.
- Verify the configuration of an Ubuntu Server system.
- Analyze network interfaces and routing tables.
- Understand the relationship between VirtualBox NAT networking and Linux network configuration.
- Document a technical environment using Markdown.

---

## 🚀 Next Steps

Future labs will focus on:

- Cleaning and configuring a static IP address using Netplan.
- Managing Linux users and permissions.
- Setting up SSH remote administration.
- Exploring Linux services and system administration.

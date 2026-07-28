# Static IP Configuration on Ubuntu Server using Netplan

## 📌 Objective

The purpose of this lab is to configure a static IPv4 address on an Ubuntu Server virtual machine using Netplan.

This exercise aims to understand Linux network configuration, including IP addressing, gateway configuration and DNS settings.
---

## 🖥️ Lab Environment

| Component | Value |
|-----------|-------|
| Host Operating System | Windows 11 |
| Hypervisor | Oracle VirtualBox 7.2.12 |
| Guest Operating System | Ubuntu Server 24.04 LTS |
| Network Mode | NAT |
| Network Interface | enp0s3 |
---

## 📋 Initial Situation

The virtual machine was previously used for networking exercises.

During verification, the network interface had both:
- a static IPv4 address;
- a DHCP-assigned IPv4 address.

The objective of this lab is to clean the configuration and keep a single static network configuration.

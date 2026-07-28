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
---

## 🔎 Initial Network Configuration

Before making any changes, the existing Netplan configuration was inspected.

Command used:

```bash
sudo cat /etc/netplan/01-network-manager-all.yaml
```

Current configuration:

```yaml
network:
  version: 2
  renderer: NetworkManager

  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 10.0.2.20/24
      routes:
        - to: default
          via: 10.0.2.2
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```

---

## 🧠 Configuration Analysis

The configuration defines a static IPv4 address for the `enp0s3` network interface.

Main parameters:

| Parameter | Value |
|-----------|-------|
| Interface | enp0s3 |
| DHCP | Disabled |
| Static IP | 10.0.2.20/24 |
| Gateway | 10.0.2.2 |
| DNS Servers | 8.8.8.8 / 1.1.1.1 |

This configuration allows the server to keep a fixed IP address while maintaining Internet connectivity through VirtualBox NAT.

---

## 🛠️ Troubleshooting: DHCP Address Persistence

During validation, an unexpected DHCP address was detected on the network interface.

Initial state:

```
inet 10.0.2.20/24
inet 10.0.2.15/24 secondary dynamic
```

The Netplan configuration correctly defined a static IP address, but NetworkManager was still configured with:

```
ipv4.method: auto
```

This caused DHCP to remain active.

### Investigation

The following commands were used:

```bash
ip a show enp0s3
nmcli device status
nmcli connection show
nmcli connection show netplan-enp0s3 | grep ipv4
```

### Resolution

The NetworkManager profile was changed from DHCP to static mode:

```bash
sudo nmcli connection modify netplan-enp0s3 ipv4.method manual
```

The network connection was then restarted:

```bash
sudo nmcli connection down netplan-enp0s3
sudo nmcli connection up netplan-enp0s3
```

### Final Validation

Final interface state:

```
inet 10.0.2.20/24
```

Routing:

```bash
ip route
```

Connectivity test:

```bash
ping -c 4 google.com
```

Result:

```
4 packets transmitted, 4 received, 0% packet loss
```

The Ubuntu server now uses a stable static IPv4 configuration.

---

## 📚 Skills Demonstrated

- Linux network configuration
- Netplan configuration
- NetworkManager administration
- IPv4 addressing
- DHCP troubleshooting
- Routing analysis
- VirtualBox NAT networking
- Technical documentation with Markdown

The objective of this lab is to clean the configuration and keep a single static network configuration.

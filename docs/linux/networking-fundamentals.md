# Lab 13 — Networking Fundamentals

## 🎯 Objective

The objective of this lab was to understand and troubleshoot network configuration on a Linux system.

During this lab, the following concepts were studied:

- Network interfaces
- IPv4 addressing
- Routing
- DNS resolution
- Open ports and listening services
- HTTP connectivity testing
- Network diagnostic scripting

The goal was to build the foundations required for Linux system and network administration.

---

# 🌐 Network Interface Analysis

The network interfaces were inspected using:

```bash
ip addr
```

The main network interface identified was:

```
enp0s3
```

Configuration:

```
IPv4 Address : 10.0.2.15/24
Network      : 10.0.2.0/24
State        : UP
```

The interface corresponds to the VirtualBox NAT network configuration.

The VM receives an internal IP address from the VirtualBox NAT DHCP service.

---

# 🛣️ Routing Configuration

The routing table was checked with:

```bash
ip route
```

Current default route:

```
default via 10.0.2.2 dev enp0s3
```

Explanation:

- `10.0.2.2` is the VirtualBox NAT gateway
- `enp0s3` is the active network interface
- All external traffic is routed through this gateway

Network path:

```
Ubuntu VM
    |
    |
10.0.2.15
    |
    |
VirtualBox NAT Gateway
    |
    |
Internet
```

---

# 🔌 Port and Service Analysis

Listening ports were identified using:

```bash
ss -tulnp
```

Important services detected:

## SSH

```
TCP 22
```

SSH service is listening on:

```
0.0.0.0:22
[::]:22
```

This means SSH accepts connections on all IPv4 and IPv6 interfaces.

SSH verification:

```bash
systemctl status ssh
```

---

## DNS Resolver

Local DNS services:

```
127.0.0.53:53
127.0.0.54:53
```

These correspond to:

```
systemd-resolved
```

Ubuntu uses this local resolver to forward DNS queries.

---

## CUPS

Detected service:

```
127.0.0.1:631
```

This corresponds to the Linux printing service.

The service is only accessible locally.

---

# 🌍 DNS Testing

DNS utilities were verified:

```bash
sudo apt install dnsutils
```

The package was already installed.

DNS resolution test:

```bash
nslookup google.com
```

Result:

```
Server: 127.0.0.53

Name:
google.com

Address:
172.217.22.14
```

DNS resolution is working correctly.

---

# 🌐 HTTP Connectivity Test

Internet connectivity was tested using:

```bash
curl -I https://google.com
```

Result:

```
HTTP/2 301
location: https://www.google.com/
```

The response confirms:

- DNS resolution works
- TCP connection works
- HTTPS communication works
- Internet access is available

---

# 🌎 Public IP Identification

The public IP address was obtained using:

```bash
curl ifconfig.me
```

Result:

```
86.253.182.69
```

This confirms that the VM accesses the Internet through the host network using NAT.

---

# 🛠️ Network Diagnostic Script

A diagnostic script was created to collect network information automatically.

Directory:

```bash
~/network_lab
```

Script:

```bash
network_report.sh
```

The script collects:

- Hostname
- Network interfaces
- Routing table
- Listening ports
- DNS resolution

Execution:

```bash
chmod +x network_report.sh

./network_report.sh
```

Example output:

```
==============================
 Linux Network Report
==============================

Hostname:
Vega

Interfaces:
enp0s3

Routes:
default via 10.0.2.2

Listening Ports:
SSH TCP/22

DNS Test:
google.com resolved successfully
```

---

# 📚 Commands Learned

| Command | Purpose |
|---|---|
| `ip addr` | Display network interfaces and IP addresses |
| `ip route` | Display routing table |
| `ss` | Display sockets and listening ports |
| `ping` | Test network connectivity |
| `nslookup` | Test DNS resolution |
| `dig` | Advanced DNS queries |
| `curl` | Test HTTP/HTTPS communication |
| `systemctl` | Manage services |

---

# ✅ Skills Acquired

- Linux network administration
- TCP/IP fundamentals
- IPv4 addressing
- Network troubleshooting
- DNS troubleshooting
- Port analysis
- SSH networking
- VirtualBox NAT networking
- Bash network diagnostics

---

# 📌 Conclusion

This lab provided a practical introduction to Linux networking fundamentals.

The system network configuration was analyzed, including interfaces, routing, DNS resolution and listening services.

The creation of a network diagnostic script introduced basic automation techniques used by system administrators for troubleshooting and monitoring.

The knowledge acquired during this lab provides the foundation for the next step:

**Lab 14 — Firewall & Network Security**

where network traffic control and system protection mechanisms will be studied.

# 🧪 Network Administration & Security Hardening

## 🎯 Objective

The goal of this lab is to deploy and validate a professional Linux server network configuration focused on administration, diagnostics and security hardening.

The server is configured as an administration node with:

* Static hostname configuration
* Dedicated management network interface
* Network diagnostic tools
* Service exposure analysis
* SSH hardening
* Firewall configuration
* Local service security verification

Environment:

| Component     | Configuration                          |
| ------------- | -------------------------------------- |
| OS            | Ubuntu 26.04 LTS                       |
| Hostname      | NOVATECH-ADMIN01                       |
| Hypervisor    | VirtualBox                             |
| Kernel        | Linux 7.0.0-28-generic                 |
| User          | moebyus                                |
| Network 1     | NAT (Internet access)                  |
| Network 2     | Host-Only Adapter (Administration LAN) |
| Management IP | 192.168.56.10/24                       |

---

# 🖥️ System Identity Configuration

The default hostname was replaced with a professional server naming convention.

Command used:

```bash
sudo hostnamectl set-hostname NOVATECH-ADMIN01
```

Verification:

```bash
hostnamectl
```

Result:

```
Static hostname: NOVATECH-ADMIN01
Operating System: Ubuntu 26.04 LTS
Kernel: Linux 7.0.0-28-generic
Virtualization: oracle
```

The local hosts file was updated:

```bash
sudo nano /etc/hosts
```

Configuration:

```text
127.0.0.1 localhost
127.0.1.1 NOVATECH-ADMIN01

127.0.0.1 homelab.local
```

Hostname resolution test:

```bash
ping -c 2 NOVATECH-ADMIN01
```

Result:

```
2 packets transmitted, 2 received, 0% packet loss
```

---

# 🌐 Network Configuration

## Network Interfaces

The server uses two network interfaces:

```bash
ip a
```

Configured interfaces:

| Interface | Role                | Address       |
| --------- | ------------------- | ------------- |
| enp0s3    | NAT Internet access | 10.0.2.15     |
| enp0s8    | Management network  | 192.168.56.10 |

---

## Management Network

The host-only adapter was configured with a static address.

NetworkManager profile:

```bash
nmcli connection show
```

Configuration:

```bash
sudo nmcli connection modify "Connexion filaire 1" \
ipv4.method manual \
ipv4.addresses 192.168.56.10/24 \
ipv4.gateway "" \
ipv4.dns ""
```

The connection was restarted:

```bash
sudo nmcli connection down "Connexion filaire 1"
sudo nmcli connection up "Connexion filaire 1"
```

Verification:

```bash
ip a
```

Expected result:

```
enp0s8:
inet 192.168.56.10/24
```

---

# 🔐 Remote Administration with SSH

The server is accessible remotely from the Windows host.

Connection test:

```powershell
ssh moebyus@192.168.56.10
```

Successful login:

```
Welcome to Ubuntu 26.04 LTS
```

---

## SSH Security Hardening

SSH configuration was checked:

```bash
sudo sshd -T | grep -E "permitrootlogin|passwordauthentication"
```

Security configuration:

```
permitrootlogin no
passwordauthentication no
```

Security measures applied:

* Root login disabled
* Password authentication disabled
* SSH key authentication required

---

# 🧰 Administration Toolkit Installation

Administrative tools were installed:

```bash
sudo apt install -y \
git \
curl \
wget \
vim \
nano \
tmux \
htop \
ncdu \
tree \
net-tools \
bind9-dnsutils \
traceroute
```

Additional network analysis tools:

```bash
sudo apt install -y \
nmap \
tcpdump \
wireshark \
netcat-openbsd \
whois \
mtr-tiny
```

Installed utilities:

| Tool       | Purpose               |
| ---------- | --------------------- |
| htop       | Process monitoring    |
| tmux       | Terminal multiplexing |
| ncdu       | Disk usage analysis   |
| nmap       | Port scanning         |
| tcpdump    | Packet capture        |
| wireshark  | Network analysis      |
| netcat     | Network testing       |
| traceroute | Path analysis         |
| mtr        | Network diagnostics   |

---

# 📡 Network Diagnostic Script

A custom administration script was deployed:

Location:

```bash
/opt/admin-toolkit/scripts/network-check.sh
```

Verification:

```bash
ls -l /opt/admin-toolkit/scripts/
```

Output:

```
-rwxrwxr-x network-check.sh
```

The script generates network reports:

```bash
/opt/admin-toolkit/reports/network/
```

Example:

```
network-report-20260803-174754.txt
```

---

# 🔎 Service Discovery and Analysis

A local service scan was performed:

```bash
nmap -sV localhost
```

Detected services:

| Port | Service    | Status       |
| ---- | ---------- | ------------ |
| 22   | OpenSSH    | Open         |
| 80   | Nginx HTTP | Open         |
| 111  | RPCBind    | Open         |
| 631  | CUPS       | Open         |
| 2049 | NFS        | Open         |
| 3306 | MariaDB    | Open locally |

---

# 🌍 Web Server Configuration

Nginx status:

```bash
systemctl status nginx
```

Result:

```
Active: active (running)
```

Configuration validation:

```bash
sudo nginx -T
```

Result:

```
syntax is ok
test is successful
```

Virtual host:

```text
server_name homelab.local localhost;

root /var/www/homelab;
```

---

# 🗄️ Database Security Check

MariaDB was detected:

```bash
sudo ss -tulpn | grep 3306
```

Result:

```
127.0.0.1:3306 users:(("mariadbd"))
```

The database service only listens locally.

Security benefit:

* No external database exposure
* Reduced attack surface
* Local application access only

---

# 📂 NFS Verification

NFS exports were checked:

```bash
showmount -e localhost
```

Result:

```
Export list for localhost:
/srv/nfs/share 127.0.0.1
```

The share is restricted to localhost.

---

# 🖨️ CUPS Service Check

CUPS status:

```bash
systemctl status cups
```

Result:

```
Active: active (running)
```

Listening ports:

```bash
sudo ss -tulpn | grep 631
```

Result:

```
127.0.0.1:631
[::1]:631
```

The printing service is not exposed externally.

---

# 🛡️ Firewall Configuration

UFW firewall was enabled:

```bash
sudo ufw status verbose
```

Configuration:

```
Default: deny (incoming)
Default: allow (outgoing)
```

Allowed services:

| Port    | Service |
| ------- | ------- |
| 22/tcp  | SSH     |
| 80/tcp  | HTTP    |
| 443/tcp | HTTPS   |

Firewall rules:

```text
22/tcp ALLOW IN
80/tcp ALLOW IN
443/tcp ALLOW IN
```

---

# 🔥 IPTables Verification

Active firewall rules:

```bash
sudo iptables -L -n -v
```

Security policy:

```
INPUT policy DROP
OUTPUT policy ACCEPT
```

Incoming traffic is filtered through UFW rules.

---

# ✅ Lab Validation Checklist

| Test                            | Result |
| ------------------------------- | ------ |
| Hostname configured             | ✅      |
| Static management IP configured | ✅      |
| SSH remote access working       | ✅      |
| SSH hardened                    | ✅      |
| Admin tools installed           | ✅      |
| Network tools installed         | ✅      |
| Nmap service analysis completed | ✅      |
| Nginx verified                  | ✅      |
| MariaDB restricted locally      | ✅      |
| NFS checked                     | ✅      |
| CUPS restricted locally         | ✅      |
| Firewall enabled                | ✅      |

---

# 📚 Skills Acquired

This lab demonstrates practical Linux administration skills:

* Network interface configuration
* Host-only network management
* SSH hardening
* Service enumeration
* Firewall management
* Network troubleshooting
* Linux security auditing
* Server hardening methodology

---

# 🏁 Conclusion

The NOVATECH-ADMIN01 server is now configured as a hardened Linux administration node.

The system provides:

* Secure remote administration
* Controlled network exposure
* Diagnostic capabilities
* Service monitoring foundations
* Firewall protection

This environment represents a realistic foundation for a professional Linux administrator homelab.

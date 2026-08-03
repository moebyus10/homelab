# 🛡️ Lab 26 – Incident Response Linux

## 📋 Overview

This lab simulates an incident response investigation on a Linux server in a small enterprise environment.

The objective is to collect evidence, analyze the operating system, review authentication logs, inspect running processes, verify active network services, and determine whether the server shows signs of compromise.

Unlike previous labs focused on system administration, this lab follows a defensive cybersecurity methodology inspired by real-world blue team investigations.

---

## 🎯 Objectives

- Collect forensic information from a Linux server.
- Identify currently connected users.
- Review authentication history.
- Analyze SSH activity.
- Inspect running processes.
- Review active services.
- Identify listening network ports.
- Verify privileged accounts.
- Detect suspicious binaries.
- Produce an incident report.
- Recommend remediation actions.

---

# 🖥️ Environment

| Component | Value |
|-----------|-------|
| OS | Ubuntu 26.04 LTS |
| Hostname | NOVATECH-ADMIN01 |
| Kernel | Linux 7.0.0-28-generic |
| Virtualization | Oracle VirtualBox |
| User | moebyus |

---

# 🏢 Investigation Scenario

The system administrator receives an alert indicating possible suspicious activity on the Linux server.

No additional information is available.

The administrator must determine whether the server has been compromised by collecting and analyzing system evidence.

---

# 🔍 Step 1 — System Identification

Commands executed:

```bash
hostnamectl
date
uptime
who
w
users
```

Information collected:

- Hostname verification
- Operating system version
- Kernel version
- Current uptime
- Logged-in users
- Active sessions
- Current workload

### Findings

- Ubuntu 26.04 LTS
- Virtual machine hosted on VirtualBox
- System running normally
- Low CPU load
- Three active user sessions
- No abnormal activity observed

---

# 🔐 Step 2 — Authentication History

Commands:

```bash
last

journalctl -u ssh --since "today"

grep -Ei "Failed|Accepted|Invalid|authentication" /var/log/auth.log
```

The investigation focused on:

- Successful logins
- Failed authentication
- Invalid users
- SSH sessions
- Authentication anomalies

### Findings

Recent SSH connections were successfully authenticated using public key authentication.

Examples:

- SSH login from 10.0.2.2
- SSH login from 192.168.56.1

No failed login attempts were detected.

No brute-force activity was observed.

No unknown users attempted authentication.

---

# 👤 Step 3 — User Investigation

Commands:

```bash
getent group sudo

cut -d: -f1 /etc/passwd
```

### Privileged users

```
moebyus
alice
bob
```

### Findings

The server contains several administrative accounts.

No unexpected privileged account was discovered.

---

# 📂 Step 4 — Recently Modified Configuration Files

Command:

```bash
sudo find /etc -type f -mtime -2
```

Files reviewed included:

- sshd_config
- wg0.conf
- nginx configuration
- fail2ban configuration
- Docker repository
- Netplan configuration
- hosts
- passwd
- group
- fstab

### Findings

Recent modifications correspond to infrastructure changes performed during previous Homelab labs.

No suspicious configuration changes were identified.

---

# ⚙️ Step 5 — Running Processes

Commands:

```bash
ps aux --sort=-%cpu | head -20

ps aux --sort=-%mem | head -20

top -b -n1 | head -20
```

Important running processes:

- gnome-shell
- dockerd
- containerd
- fail2ban-server
- mariadbd
- nginx
- sshd
- VBoxService

### Findings

CPU usage remained very low.

Memory usage was normal.

No suspicious process names were detected.

No cryptocurrency miners.

No reverse shells.

No hidden processes.

---

# 🖥️ Step 6 — Service Analysis

Commands:

```bash
systemctl --failed

systemctl list-units --type=service --state=running
```

Important services:

- SSH
- Docker
- MariaDB
- Nginx
- Fail2ban
- Chrony
- NetworkManager
- rsyslog

### Findings

No failed services.

Critical services were running normally.

---

# 🌐 Step 7 — Network Analysis

Commands:

```bash
sudo ss -tulpn

sudo lsof -i -P -n
```

Listening services identified:

| Port | Service |
|-------|----------|
| 22 | SSH |
| 80 | Nginx |
| 111 | RPCBind |
| 2049 | NFS |
| 3306 | MariaDB (localhost only) |
| 53 | systemd-resolved |

### Findings

Open ports matched installed services.

MariaDB was correctly restricted to localhost.

SSH was expected.

Nginx was expected.

No suspicious listening port was identified.

---

# 🔥 Step 8 — Firewall Verification

Command:

```bash
sudo ufw status verbose
```

Configured rules:

- Allow SSH
- Allow HTTP
- Allow HTTPS

Default policy:

- Deny Incoming
- Allow Outgoing

### Findings

Firewall correctly configured.

Only required services were exposed.

---

# 🔐 Step 9 — SUID Binary Inspection

Command:

```bash
sudo find / -perm -4000 -type f
```

Inspection covered:

- sudo
- su
- passwd
- mount
- umount
- pkexec
- ssh-keysign

### Findings

Only expected SUID binaries were identified.

Additional SUID files belonged to:

- Snap packages
- VirtualBox Guest Additions
- Containerd snapshots

No suspicious custom SUID binary was discovered.

---

# 🐳 Step 10 — Docker Inspection

Command:

```bash
docker ps -a
```

### Findings

Docker was installed.

No running containers were detected.

---

# 🚫 Step 11 — Fail2ban Verification

Command:

```bash
sudo fail2ban-client status
```

### Findings

Fail2ban was active.

Active jail:

```
sshd
```

The SSH service was protected against brute-force attacks.

---

# 📑 Incident Report

## Timeline

| Time | Event |
|------|-------|
| System boot | Server started normally |
| SSH connections | Successful public key authentication |
| Investigation | Full forensic analysis performed |
| Result | No compromise detected |

---

## Evidence Collected

- System information
- Authentication logs
- SSH logs
- Logged users
- Running processes
- Running services
- Firewall configuration
- Listening ports
- Privileged users
- SUID binaries
- Docker status
- Fail2ban status

---

## Root Cause Analysis

No indicators of compromise were identified.

Observed activity corresponds to normal administration of the Homelab environment.

The configuration changes detected under `/etc` are consistent with previous infrastructure deployments.

---

## Security Assessment

| Check | Result |
|--------|--------|
| Unauthorized users | ✅ None |
| Failed SSH attempts | ✅ None |
| Unknown processes | ✅ None |
| Suspicious services | ✅ None |
| Unexpected listening ports | ✅ None |
| Firewall enabled | ✅ Yes |
| Fail2ban active | ✅ Yes |
| SSH secured with keys | ✅ Yes |

---

# 🛠️ Remediation Recommendations

Although no compromise was identified, the following best practices remain recommended:

- Continue using SSH public key authentication.
- Keep UFW enabled.
- Monitor authentication logs regularly.
- Update installed packages.
- Periodically review privileged accounts.
- Maintain Fail2ban protection.
- Audit exposed network services regularly.
- Implement centralized log monitoring for larger infrastructures.

---

# ✅ Conclusion

This lab demonstrated a complete Linux incident response workflow using native system administration tools.

The investigation covered authentication logs, user activity, system services, running processes, firewall configuration, network exposure, privileged accounts, and security controls.

After analyzing all collected evidence, no indicators of compromise were detected. The server remained in a healthy operational state, and all observed activity was consistent with legitimate administration tasks performed during previous Homelab exercises.

---

# 📚 Skills Acquired

- Linux Incident Response
- Log Analysis
- SSH Investigation
- User Enumeration
- Process Analysis
- Service Inspection
- Network Enumeration
- Firewall Verification
- Security Assessment
- Digital Forensics Fundamentals
- Blue Team Methodology
- Linux System Auditing

# 🔐 SSH Bastion Host

## 📌 Overview

This lab implements a secure **SSH Bastion Host** architecture.

A bastion host is a dedicated server used as a secure entry point to access internal systems. Instead of connecting directly to internal machines, administrators connect through the bastion using SSH keys and controlled forwarding.

The goal of this lab is to build a hardened administration gateway similar to an enterprise environment.

---

## 🎯 Objectives

* Deploy an SSH Bastion Host
* Configure SSH key-based authentication
* Use SSH ProxyJump for internal access
* Harden SSH server configuration
* Restrict SSH users
* Control SSH forwarding capabilities
* Monitor SSH authentication logs

---

## 🏗️ Architecture

```
Windows Host
192.168.56.1
      |
      | SSH ED25519
      |
      v
+----------------------+
| NOVATECH-ADMIN01     |
| SSH Bastion Host     |
| 192.168.56.10        |
| Ubuntu 26.04 LTS     |
+----------------------+
      |
      | SSH ProxyJump
      |
      v
+----------------------+
| NODE01               |
| Internal Linux Node  |
| 192.168.56.11        |
| Ubuntu 26.04 LTS     |
+----------------------+
```

---

## 🖥️ Environment

| Component      | Details          |
| -------------- | ---------------- |
| Host OS        | Windows 11       |
| Hypervisor     | VirtualBox 7.2   |
| Bastion OS     | Ubuntu 26.04 LTS |
| Internal Node  | Ubuntu 26.04 LTS |
| SSH Client     | OpenSSH          |
| Authentication | ED25519 SSH Keys |

---

## 🔑 SSH Key Authentication

An ED25519 SSH key pair was used for secure authentication.

Key generation:

```bash
ssh-keygen -t ed25519
```

The public key was deployed into:

```bash
~/.ssh/authorized_keys
```

Password authentication was disabled.

---

## ⚙️ SSH Hardening

The SSH daemon was hardened with the following configuration:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers moebyus
```

Security improvements:

* Root SSH login disabled
* Password authentication disabled
* Public key authentication enabled
* SSH access restricted to administrative users

---

## 🔒 SSH Forwarding Configuration

The bastion was configured to support ProxyJump while limiting unnecessary forwarding capabilities.

Configuration:

```text
AllowTcpForwarding yes
PermitOpen 192.168.56.11:22
X11Forwarding no
PermitTunnel no
```

This allows:

* Secure SSH jump access
* Controlled forwarding
* Disabled graphical forwarding
* Disabled SSH tunnels

---

## 🖥️ SSH Client Configuration

A Windows SSH configuration file was created:

Location:

```text
C:\Users\kylli\.ssh\config
```

Configuration:

```sshconfig
Host BASTION
    HostName 192.168.56.10
    User moebyus

Host NODE01
    HostName 192.168.56.11
    User moebyus
    ProxyJump BASTION
```

---

## 🚀 Connection Tests

### Connect to Bastion

Command:

```powershell
ssh BASTION
```

Result:

```text
moebyus@NOVATECH-ADMIN01
```

---

### Connect to Internal Node Through Bastion

Command:

```powershell
ssh NODE01
```

Connection flow:

```
Windows Host
      |
      |
      v
NOVATECH-ADMIN01
SSH Bastion
      |
      |
      v
NODE01
Internal Server
```

Result:

```text
moebyus@NODE01
```

---

## 📋 SSH Logs Verification

Authentication logs were checked using:

```bash
sudo journalctl _COMM=sshd-session | grep "Accepted"
```

Example:

```text
Accepted publickey for moebyus from 192.168.56.1
```

Validation:

* Successful SSH key authentication
* Correct administrator account
* Correct source machine

---

## 🧪 Security Validation

SSH configuration test:

```bash
sudo sshd -t
```

SSH service restart:

```bash
sudo systemctl restart ssh
```

Configuration verification:

```bash
sudo sshd -T | grep -E "permitrootlogin|passwordauthentication|pubkeyauthentication|allowusers|allowtcpforwarding|x11forwarding|permittunnel"
```

Expected result:

```text
permitrootlogin no
passwordauthentication no
pubkeyauthentication yes
allowusers moebyus
allowtcpforwarding yes
x11forwarding no
permittunnel no
```

---

## ✅ Lab Results

| Feature                          | Status |
| -------------------------------- | ------ |
| SSH Bastion deployed             | ✅      |
| ED25519 authentication           | ✅      |
| Password authentication disabled | ✅      |
| Root SSH disabled                | ✅      |
| User restriction enabled         | ✅      |
| ProxyJump configured             | ✅      |
| Internal node access protected   | ✅      |
| SSH logs verified                | ✅      |

---

## 📚 Skills Acquired

* SSH security hardening
* Bastion host architecture
* ProxyJump administration
* SSH key management
* Linux authentication auditing
* Secure remote administration practices

---

## 🏁 Conclusion

This lab demonstrates how enterprise environments protect internal servers using a hardened SSH Bastion Host.

Administrators no longer access internal systems directly. Instead, SSH administration passes through a controlled and monitored gateway.

This architecture improves security, access control, and traceability.

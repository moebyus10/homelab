# Secure Remote Administration with SSH

## 📌 Objective

The purpose of this lab is to install, configure and secure an OpenSSH Server on Ubuntu.

SSH (Secure Shell) is the standard protocol used by system administrators to securely manage Linux servers over a network.

At the end of this lab, remote administration will be possible using both password authentication and SSH key authentication.

---

## 🖥️ Lab Environment

| Component | Value |
|-----------|-------|
| Host OS | Windows 11 |
| Hypervisor | Oracle VirtualBox 7.2.12 |
| Guest OS | Ubuntu Server 24.04 LTS |
| Hostname | vega |
| Network | NAT |

---

## 📋 Scenario

A new Ubuntu server has been deployed inside the company's infrastructure.

Instead of managing the server locally with a keyboard and monitor, the system administrator must enable secure remote access using SSH.

The server will first be tested with password authentication before being secured using SSH key authentication.

---

## 🔍 SSH Service Verification

The first step consisted of checking whether the OpenSSH service was already installed and running.

```bash
systemctl status ssh
```

This command displays:

- the service status;
- whether the service is enabled;
- the current process information.

![SSH service status](../../screenshots/lab04/01-ssh-service-status.png)

The server will first be tested with password authentication before being secured using SSH key authentication.

---

## ▶️ Starting the SSH Service

The SSH service was started manually using `systemctl`.

```bash
sudo systemctl start ssh
```

The service status was verified afterwards.

The output confirmed that:

- the SSH daemon was running;
- the server was listening on TCP port **22**;
- remote SSH connections were now possible.

![SSH service running](../../screenshots/lab04/02-ssh-service-running.png)

---

## 🔄 Enabling the SSH Service at Boot

To ensure remote administration remains available after a system reboot, the SSH service was configured to start automatically.

```bash
sudo systemctl enable ssh
```

The service status confirmed that SSH was now enabled at boot.

![SSH enabled](../../screenshots/lab04/03-ssh-enabled.png)

---

## 🌐 Verifying the Listening Port

To confirm that the SSH server was accepting incoming connections, the listening ports were inspected.

```bash
sudo ss -tlnp | grep :22
```

The output confirmed that:

- the SSH daemon was listening on TCP port **22**;
- the service was accepting connections on both IPv4 and IPv6;
- the server was ready for remote administration.

![SSH listening on port 22](../../screenshots/lab04/04-ssh-port-22-listening.png)

---

## 🌐 Configuring Port Forwarding

The virtual machine was configured in **NAT** mode.

To allow SSH connections from the Windows host, a VirtualBox port forwarding rule was created.

| Host Port | Guest Port | Protocol |
|-----------|-----------|----------|
| 2222 | 22 | TCP |

This configuration forwards incoming connections on port **2222** of the host machine to the SSH service running on port **22** inside the Ubuntu virtual machine.

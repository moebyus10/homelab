# Secure Remote Administration with SSH

## 📌 Objective

The purpose of this lab is to install, configure and secure an OpenSSH Server on an Ubuntu system.

SSH (Secure Shell) is the standard protocol used by system administrators to securely manage Linux servers over a network.

At the end of this lab, remote administration will be possible using password authentication and SSH key authentication.

---

## 🖥️ Lab Environment

| Component | Value |
|-----------|-------|
| Host OS | Windows 11 |
| Hypervisor | Oracle VirtualBox 7.2.12 |
| Initial Guest OS | Ubuntu 24.04 LTS |
| Final Guest OS | Ubuntu 26.04 LTS |
| User account | moebyus |
| Hostname | Vega |
| Network | NAT |

---

## ⚠️ Environment Change During the Lab

The lab was initially performed using an Ubuntu 24.04 LTS virtual machine.

During the SSH configuration and testing phase, connection issues persisted despite correct SSH service configuration and network settings.

After troubleshooting, the decision was made to replace the virtual machine with a fresh Ubuntu 26.04 LTS installation in order to continue the exercise on a clean and stable environment.

The previous virtual machine was removed, and all remaining SSH configuration steps were reproduced on the new Ubuntu 26.04 LTS system.

---

## 📋 Scenario

A new Ubuntu system has been deployed inside a test infrastructure.

Instead of managing the machine locally with a keyboard and monitor, the system administrator must enable secure remote access using SSH.

The server will first be tested with password authentication before being secured using SSH key authentication.

---

# 🔍 SSH Service Verification

The first step consisted of checking whether the OpenSSH service was installed and running.

```bash
systemctl status ssh

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

![SSH service running](../../screenshots/lab-04/02-ssh-service-running.png)

---

## 🔄 Enabling the SSH Service at Boot

To ensure remote administration remains available after a system reboot, the SSH service was configured to start automatically.

```bash
sudo systemctl enable ssh
```

The service status confirmed that SSH was now enabled at boot.

![SSH enabled](../../screenshots/lab-04/03-ssh-enabled.png)

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

![SSH listening on port 22](../../screenshots/lab-04/04-ssh-port-22-listening.png)

---

## 🌐 Configuring Port Forwarding

The virtual machine was configured in **NAT** mode.

To allow SSH connections from the Windows host, a VirtualBox port forwarding rule was created.

| Host Port | Guest Port | Protocol |
|-----------|-----------|----------|
| 2222 | 22 | TCP |

This configuration forwards incoming connections on port **2222** of the host machine to the SSH service running on port **22** inside the Ubuntu virtual machine.

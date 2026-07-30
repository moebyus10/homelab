# ⚙️ Linux Service Management with systemd

## 📌 Objective

The purpose of this lab is to learn how to manage Linux services using **systemd** on an Ubuntu Linux system.

System services are essential components of a Linux server. They allow administrators to control background processes, configure automatic startup, monitor service status and troubleshoot system operations.

At the end of this lab, service management operations will be performed using `systemctl` and service activity will be monitored using `journalctl`.

---

## 🖥️ Lab Environment

| Component | Value |
|-----------|-------|
| Host OS | Windows 11 |
| Hypervisor | Oracle VirtualBox 7.2.12 |
| Guest OS | Ubuntu 26.04 LTS |
| User account | moebyus |
| Hostname | Vega |
| Network | NAT |
| Service manager | systemd |

---

## 📋 Scenario

A Linux server administrator must maintain and monitor system services running on an Ubuntu server.

The administrator needs to verify running services, control service states, configure automatic startup and analyze service logs in order to troubleshoot potential issues.

The objective is to understand how systemd manages services and how administrators interact with services during daily system administration tasks.

---

# 🔍 Initial Service Verification

Before modifying any service configuration, the currently running services were inspected.

The following command was executed:

```bash
systemctl --type=service --state=running
```

This command displays all services currently active on the system.

The output confirmed that several important services were running, including:

- SSH remote administration service;
- Network management service;
- System logging service;
- Scheduled task management service.

This initial verification provides an overview of the current system state before performing administrative operations.

---

# 🔍 Checking SSH Service Status

The SSH service was inspected to verify its current state and configuration.

The following command was executed:

```bash
systemctl status ssh
```

The output confirmed that:

- the SSH service was correctly loaded by systemd;
- the service was currently running;
- the SSH daemon was listening on TCP port **22**;
- remote administration remained available through SSH key authentication.

The logs also confirmed a successful public key authentication session for the user account.

---

## 🔄 Verifying Service Startup Configuration

To determine whether the SSH service was configured to start automatically during system boot, the following command was executed:

```bash
systemctl is-enabled ssh
```

The command returned:

```text
enabled
```

This confirms that SSH is configured to start automatically whenever the system boots.

---

# ▶️ Managing a Service State

To demonstrate service management operations, the `cron` service was selected.

The cron daemon is responsible for executing scheduled tasks on Linux systems.

Before modifying its state, the service status was checked:

```bash
systemctl status cron
```

The output confirmed that:

- the service was loaded correctly;
- the service was active and running;
- scheduled tasks were being executed successfully.

---

## ⏹️ Stopping and Restarting a Service

The `cron` service was temporarily stopped to demonstrate service control using systemd.

The following command was executed:

```bash
sudo systemctl stop cron
```

The service state was then verified.

After confirming that the service had stopped, it was started again:

```bash
sudo systemctl start cron
```

The service status was checked once more:

```bash
systemctl status cron
```

The output confirmed that the service returned to an active state.

This demonstrates how administrators can safely control running services during maintenance operations.

---

# 🔄 Restarting a Service

After testing the stop and start operations, the service restart procedure was verified.

The following command was executed:

```bash
sudo systemctl restart cron
```

The service status was checked afterwards:

```bash
systemctl status cron
```

The output confirmed that the service was successfully restarted and returned to the running state.

Restarting services is commonly used by system administrators after modifying configuration files or applying system changes.

---

# 📋 Reviewing Service Logs

To analyze service activity and troubleshooting information, the system journal was consulted.

The following command was executed:

```bash
sudo journalctl -u cron -n 20
```

The logs displayed:

- service start and stop events;
- successful service restarts;
- cron task execution history;
- systemd service management messages.

The following entries confirmed that the service was correctly managed by systemd:

```text
Started cron.service
Stopped cron.service
Deactivated successfully
```

---

# 🔎 Checking Service Activity and Startup Status

Two different checks were performed to distinguish between the current service state and the startup configuration.

The following commands were executed:

```bash
systemctl is-enabled cron
```

and:

```bash
systemctl is-active cron
```

The results confirmed:

```text
enabled
active
```

This means that:

- the cron service is currently running;
- the cron service is configured to start automatically during system boot.

These two states are independent and are important for daily Linux administration.

---

# ⚙️ Managing Automatic Service Startup

To demonstrate service startup management, the automatic launch of the cron service was temporarily disabled.

The following command was executed:

```bash
sudo systemctl disable cron
```

The configuration was verified:

```bash
systemctl is-enabled cron
```

The output confirmed:

```text
disabled
```

The service was then re-enabled to restore the original system configuration:

```bash
sudo systemctl enable cron
```

The command recreated the systemd startup link and restored automatic service startup.

---

# 📋 Final Service Management Summary

The systemd service management process was successfully completed.

| Service Management Task | Status |
|------------------------|--------|
| Running services inspected | ✅ |
| SSH service status verified | ✅ |
| SSH automatic startup verified | ✅ |
| Service status checked with systemctl | ✅ |
| Service stop operation tested | ✅ |
| Service start operation tested | ✅ |
| Service restart operation tested | ✅ |
| Service logs reviewed with journalctl | ✅ |
| Service activity and startup status compared | ✅ |
| Automatic service startup management tested | ✅ |
| Original service configuration restored | ✅ |

---

# 📝 Conclusion

This lab demonstrated how to manage Linux services using **systemd** on an Ubuntu server.

The main service administration operations were performed using:

- `systemctl` to inspect, start, stop, restart and configure services;
- `journalctl` to analyze service activity and troubleshooting information.

The practical exercises showed the difference between a service being currently active and a service being enabled at system startup.

The Ubuntu server is now configured and monitored using standard Linux service management practices, providing a solid foundation for future administration and troubleshooting tasks.

# 🐧 Process Management & System Analysis

## 🎯 Objective

The objective of this lab was to understand how Linux manages running processes and system services.

The lab focused on:

- Process inspection and analysis
- Understanding PID hierarchy
- Monitoring system activity
- Managing services with systemd
- Reading and analyzing system logs

This lab introduces essential skills for Linux system administration and server troubleshooting.

---

# 1. Process Inspection

## Listing running processes

The following command was used to display all running processes:

```bash
ps aux
```

This command provides information about:

- Process owner
- Process ID (PID)
- CPU usage
- Memory usage
- Running command

Example:

```text
USER       PID  %CPU  %MEM   COMMAND

root      1581   0.0   0.1   sshd
```

The PID identifies each running process uniquely.

---

# 2. Understanding PID 1 and systemd

Linux starts the first user-space process after the kernel boot.

The PID 1 process was checked using:

```bash
ps -p 1
```

Result:

```text
PID TTY          TIME CMD
1   ?            00:00:05 systemd
```

The system uses:

```text
systemd
```

as its init system.

systemd is responsible for:

- Starting system services
- Managing background processes
- Handling service dependencies
- Controlling system units

---

# 3. Process Hierarchy

The process tree was displayed with:

```bash
pstree -p
```

Example:

```text
systemd(1)
 ├── sshd
 │    └── sshd-session
 │         └── bash
 │              └── pstree
```

This shows the relationship between parent and child processes.

The current SSH session follows this hierarchy:

```text
systemd
    ↓
sshd
    ↓
SSH session
    ↓
bash
    ↓
commands executed by user
```

---

# 4. Real-Time Process Monitoring

## Using top

The system was monitored using:

```bash
top
```

This tool provides real-time information about:

- CPU usage
- Memory usage
- Running processes
- System load

Useful shortcuts:

| Key | Action |
|-----|--------|
| P | Sort by CPU usage |
| M | Sort by memory usage |
| k | Kill a process |
| q | Quit |

---

## Using htop

For a more user-friendly interface:

```bash
sudo apt install htop
```

Then:

```bash
htop
```

htop provides:

- Interactive process management
- Resource visualization
- Process filtering
- Easier system monitoring

---

# 5. Service Management with systemd

Linux services are managed using systemd.

List active services:

```bash
systemctl list-units --type=service
```

Example services:

```text
ssh.service
ufw.service
NetworkManager.service
systemd-journald.service
```

---

## Checking SSH service status

The SSH service was verified using:

```bash
systemctl status ssh
```

Example:

```text
Active: active (running)

Main PID: xxxx (sshd)
```

This confirms that the SSH daemon is running correctly.

---

# 6. Service and Process Relationship

A system service launches and manages one or several processes.

Example:

```text
ssh.service

        |
        ↓

sshd process

        |
        ↓

SSH connections
```

The main process ID was displayed with:

```bash
systemctl show ssh --property=MainPID
```

---

# 7. System Logs Analysis

Linux uses journald to store system logs.

## Viewing SSH logs

Command used:

```bash
journalctl -u ssh
```

This displays:

- Service startup events
- SSH connections
- Authentication events
- Errors

---

## Displaying recent logs

```bash
journalctl -u ssh -n 20
```

Shows the latest 20 SSH log entries.

---

## Monitoring logs in real time

```bash
sudo journalctl -u ssh -f
```

This allows real-time monitoring of SSH activity.

Example:

```text
Accepted publickey for moebyus
```

---

## Checking system errors

Command used:

```bash
sudo journalctl -p err -b
```

Options:

- `-p err` → display only errors
- `-b` → current boot session

Result:

```text
-- No entries --
```

No system errors were detected.

---

# 8. System Analysis Commands Summary

| Command | Purpose |
|---------|---------|
| ps aux | List running processes |
| ps -p PID | Display process information |
| pstree -p | Display process hierarchy |
| top | Real-time monitoring |
| htop | Interactive monitoring |
| systemctl status | Check service status |
| systemctl list-units | List services |
| journalctl | Analyze system logs |
| pidof | Find process PID |

---

# 9. Lab Validation

The following checks were successfully completed:

✅ Process listing with ps  
✅ PID 1 identification  
✅ systemd analysis  
✅ Process tree visualization  
✅ Real-time monitoring with top/htop  
✅ Service management with systemctl  
✅ SSH service verification  
✅ System log analysis with journalctl  

---

# Conclusion

This lab introduced the fundamentals of Linux process and service management.

The system was analyzed through different administration tools:

- Process inspection
- Resource monitoring
- Service supervision
- Log analysis

Understanding the relationship between processes, services and system logs is essential for Linux administration and troubleshooting.

These skills provide the foundation for managing production servers, diagnosing issues and maintaining system availability.

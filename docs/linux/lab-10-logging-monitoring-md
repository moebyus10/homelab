# 🧪 Logging & Monitoring

## 🎯 Objective

The objective of this lab is to learn how to analyze, monitor and troubleshoot a Linux system using logs and system monitoring tools.

This lab covers:

- Linux log management
- systemd journal analysis
- SSH activity monitoring
- Error investigation
- Process monitoring
- Memory and disk analysis

The goal is to develop basic troubleshooting skills required for Linux system administration.

---

# 🖥️ Environment

| Component | Configuration |
|-----------|---------------|
| Host OS | Windows 11 |
| Hypervisor | VirtualBox 7.2 |
| Guest OS | Ubuntu Linux |
| Network | NAT |
| User | moebyus |

---

# 📋 Prerequisites

Before starting this lab:

- Ubuntu virtual machine running
- SSH access configured
- User with sudo privileges

---

# 1. Exploring Linux Logs

Linux stores system logs mainly inside:

    /var/log

Directory inspection:

    ls -lah /var/log

Important log files:

| File | Description |
|------|-------------|
| `/var/log/auth.log` | Authentication, SSH and sudo activity |
| `/var/log/syslog` | General system events |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/dpkg.log` | Package management logs |
| `/var/log/journal/` | systemd journal storage |

---

# 2. Authentication Log Analysis

Authentication logs were inspected with:

    sudo tail -50 /var/log/auth.log

Observed events:

- SSH session creation
- User authentication
- sudo commands
- Automated cron tasks

Example:

    sshd-session: session opened for user moebyus

These logs are useful for investigating:

- successful connections
- failed login attempts
- administrative actions

---

# 3. systemd Journal Analysis

Ubuntu uses systemd-journald to centralize logs.

Display recent system events:

    sudo journalctl -n 50

The journal contains:

- system service events
- kernel messages
- authentication events
- scheduled tasks

---

## Filtering Errors

Display only error messages:

    sudo journalctl -p err

This allows administrators to quickly identify:

- service failures
- system problems
- configuration issues

---

## Errors Since Last Boot

Command:

    sudo journalctl -p err -b

Result:

    -- No entries --

No error events were detected during the current boot session.

---

# 4. SSH Service Monitoring

SSH logs were analyzed using:

    sudo journalctl -u ssh

Recent SSH events:

    sudo journalctl -u ssh -n 20

Observed events:

- SSH service startup
- Listening on port 22
- Successful user authentication

Example:

    Accepted publickey for moebyus from 10.0.2.2

This confirms that SSH key authentication is working correctly.

---

# 5. SSH Authentication Filtering

Successful SSH connections were filtered with:

    sudo journalctl -u ssh | grep Accepted

The logs showed:

- password authentication during initial tests
- ED25519 public key authentication after SSH hardening

Example:

    Accepted publickey for moebyus from 10.0.2.2

This confirms the SSH configuration from Lab 04.

---

# 6. Process Monitoring

## top

Real-time process monitoring:

    top

Information displayed:

- CPU usage
- memory usage
- running processes
- process identifiers

Important columns:

| Column | Description |
|-|-|
| PID | Process identifier |
| USER | Process owner |
| %CPU | CPU usage |
| %MEM | Memory usage |
| COMMAND | Program name |

---

## htop

Installation:

    sudo apt install htop

Execution:

    htop

Advantages:

- interactive interface
- easier process navigation
- process tree visualization
- improved readability compared to top

---

# 7. Memory Monitoring

Command:

    free -h

Result:

    Mem:       7.3Gi total
               880Mi used
               6.4Gi available

    Swap:      4.0Gi
               0B used

Analysis:

- Low RAM usage
- No swap usage
- No memory pressure detected

---

# 8. Disk Monitoring

Filesystem usage:

    df -h

System partition:

    /dev/sda2
    Size: 40G
    Used: 11G
    Available: 27G
    Usage: 29%

The system filesystem has enough available space.

---

# 9. Directory Size Analysis

Log directory size:

    sudo du -sh /var/log

Result:

    79M /var/log

Global /var analysis:

    sudo du -sh /var/*

Main directories:

| Directory | Size | Description |
|-|-|-|
| `/var/lib` | 1.9G | Persistent service data |
| `/var/cache` | 179M | Application cache |
| `/var/log` | 79M | System logs |

The du command helps identify disk usage problems.

---

# 10. Final System Health Check

## Failed Services

Command:

    systemctl --failed

Result:

    0 loaded units listed.

No failed systemd services were detected.

---

## System Load

Command:

    uptime

Result:

    up 48 min, 2 users, load average: 0.18, 0.06, 0.02

The system load remains low.

---

# ✅ Lab Validation

The following objectives were completed:

✔ Explore Linux log files  
✔ Analyze authentication logs  
✔ Use journalctl  
✔ Filter system errors  
✔ Monitor SSH activity  
✔ Monitor processes with top and htop  
✔ Check RAM usage  
✔ Check disk usage  
✔ Analyze directory sizes  
✔ Perform system health checks  

---

# 🧠 Skills Acquired

- Linux log management
- systemd troubleshooting
- SSH auditing
- Resource monitoring
- Disk analysis
- Basic Linux troubleshooting methodology

---

# 📝 Conclusion

During this lab, the Linux system monitoring and logging mechanisms were explored.

The main objectives were achieved:

- Understanding the Linux logging structure
- Using `journalctl` to analyze system and service events
- Monitoring SSH authentication activity
- Identifying system errors and checking service health
- Monitoring system resources such as CPU, memory and disk usage
- Using diagnostic tools commonly used by system administrators

This lab provided the foundations required for Linux troubleshooting and server maintenance.

The ability to analyze logs and monitor system resources is essential for maintaining reliable and secure Linux environments.

---


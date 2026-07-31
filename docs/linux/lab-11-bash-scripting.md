# 🐧 Lab 11 — Bash Scripting

## 🎯 Objective

The objective of this lab is to learn Bash scripting fundamentals and create a Linux administration toolkit.

The script developed during this lab automates system checks, performs health monitoring, generates reports, and provides an interactive administration interface.

---

# 📌 Lab Overview

During this lab, a Bash script named:

```bash
system_dashboard.sh
```

was created.

The goal was to transform basic Linux commands into an automated administration tool capable of:

- Collecting system information
- Checking important services
- Monitoring system health
- Generating reports
- Providing an interactive menu
- Supporting command-line arguments

---

# 🛠️ Environment

| Component | Value |
|-----------|-------|
| Operating System | Ubuntu |
| Hostname | Vega |
| User | moebyus |
| Shell | Bash |
| Script Location | `/home/moebyus/scripts` |

---

# 📂 Project Structure

The project directory:

```text
scripts/
│
├── system_dashboard.sh
│
├── system_dashboard_v1.2.sh
├── system_dashboard_v1.3.sh
├── system_dashboard_v1.4.sh
├── system_dashboard_v1.5.sh
│
└── reports/
    └── system_report_YYYYMMDD_HHMMSS.txt
```

Version backups were created during development to keep track of improvements.

---

# 📝 Script Development

## Version 1.0 — System Information

The first version collected basic system information:

- Current user
- Hostname
- Kernel version
- System uptime
- Memory usage
- Disk usage

Example:

```bash
whoami
hostname
uname -r
uptime
free -h
df -h
```

---

# Version 1.1 — Health Checks

The script was improved with automated system checks.

Implemented checks:

## SSH Service

Verification using:

```bash
systemctl is-active ssh
```

Result:

```text
✔ SSH Service Active
```

---

## Firewall Status

Verification using:

```bash
ufw status
```

Result:

```text
✔ Firewall Active
```

---

## Network Connectivity

Internet connectivity test:

```bash
ping -c1 8.8.8.8
```

Result:

```text
✔ Internet Connected
```

---

## Disk Usage

Disk monitoring:

```bash
df /
```

Example:

```text
✔ Disk Usage 29%
```

---

## Memory Usage

Memory monitoring:

```bash
free
```

Example:

```text
✔ Memory Usage 11%
```

---

# Version 1.2 — Health Score

A global system health score was introduced.

The script calculates:

```text
Health Score = Successful Checks / Total Checks
```

Example:

```text
Health Score 5 / 5

✔ Overall Status HEALTHY
```

System states:

| Score | Status |
|-------|--------|
| All checks passed | HEALTHY |
| Partial checks passed | WARNING |
| Multiple failures | CRITICAL |

---

# Version 1.3 — Report Generation

The script was extended to generate automatic reports.

Reports are stored in:

```text
reports/
```

Example:

```text
system_report_20260801_001549.txt
```

Generated information:

- Date
- Hostname
- Kernel
- Uptime
- Health checks
- Health score
- Overall status

Example:

```text
=================================================
        Linux Homelab Administration Toolkit
=================================================

Date:
sam. 01 août 2026 00:15:49 CEST

Hostname:
Vega

Kernel:
7.0.0-28-generic

Health Score:
5 / 5

Status: HEALTHY
```

---

# Version 1.4 — Interactive Menu

An interactive administration menu was added.

Launch:

```bash
./system_dashboard.sh
```

Menu:

```text
=================================================
        Linux Homelab Administration Toolkit
=================================================

1) System Dashboard
2) Generate Report
3) Check Services
4) Network Test
5) Exit
```

Implemented Bash concepts:

- `while` loops
- `case` statements
- user input with `read`

---

# Version 1.5 — Command-Line Arguments

The script now supports command-line options.

Examples:

## Display Dashboard

```bash
./system_dashboard.sh --dashboard
```

---

## Generate Report

```bash
./system_dashboard.sh --report
```

---

## Check Services

```bash
./system_dashboard.sh --services
```

---

## Network Test

```bash
./system_dashboard.sh --network
```

---

## Help

```bash
./system_dashboard.sh --help
```

---

# 📊 Final Dashboard Example

```text
=========================================================
        Linux Homelab Administration Toolkit
                     Version 1.5
=========================================================

System Information
---------------------------------------------------------
User                   moebyus
Hostname               Vega
Kernel                 7.0.0-28-generic
Uptime                 up 5 hours, 53 minutes


Health Checks
---------------------------------------------------------
✔ SSH Service          Active
✔ Firewall             Active
✔ Internet             Connected
✔ Disk Usage           29%
✔ Memory Usage         11%


---------------------------------------------------------

Health Score           5 / 5

✔ Overall Status       HEALTHY
```

---

# 🧠 Skills Learned

## Bash

- Variables
- Functions
- Conditions
- Loops
- Case statements
- User input
- Arguments handling

## Linux Administration

- Service monitoring
- Firewall verification
- System information gathering
- Resource monitoring
- Report generation

## Automation

- Creating reusable scripts
- Automating repetitive tasks
- Building administration tools

---

# ✅ Lab Conclusion

This lab introduced Bash scripting as a tool for Linux system administration.

The final result is a lightweight administration toolkit capable of monitoring system health, generating reports, and providing automated diagnostics.

This project will serve as a foundation for future labs by adding new modules related to:

- Process management
- Networking
- Storage
- Backup
- Automation

---

# ✅ Lab Conclusion

This lab introduced Bash scripting as a practical tool for Linux system administration.

Starting from basic command execution, the project evolved into a complete administration toolkit capable of collecting system information, performing automated health checks, generating reports, and providing both interactive and command-line interfaces.

Throughout this lab, several key Linux administration concepts were applied:

* Automating repetitive system checks
* Monitoring services and resources
* Managing Bash variables, functions, conditions, loops, and arguments
* Creating structured reports for system diagnostics
* Designing a reusable administration tool

The final version of the script (**Linux Homelab Administration Toolkit v1.5**) provides a solid foundation that can be extended with additional modules in future labs.

Future improvements will include integrating process monitoring, network diagnostics, storage management, backups, and automation features to progressively build a complete Linux administration toolkit.

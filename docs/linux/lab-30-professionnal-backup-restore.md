# 💾 Professional Backup & Restore

## 📌 Overview

This lab focuses on implementing a professional backup and restore solution on a Linux server.

The objective is to move from manual backups to an automated backup infrastructure using:

- `rsync` for data synchronization
- Bash scripting for automation
- systemd services and timers for scheduling
- backup retention management
- restore testing procedures

This lab simulates a real enterprise backup workflow where critical data must be protected, recoverable, and automatically maintained.

---

# 🎯 Objectives

By completing this lab, the following skills were practiced:

- Designing a backup strategy
- Synchronizing critical data with `rsync`
- Creating automated backup scripts
- Managing systemd services
- Scheduling backups with systemd timers
- Implementing backup retention policies
- Testing restore procedures
- Managing operational backup logs

---

# 🖥️ Environment

| Component | Configuration |
|-----------|--------------|
| OS | Ubuntu 26.04 LTS |
| Hostname | NOVATECH-ADMIN01 |
| Backup Tool | rsync |
| Automation | Bash + systemd timer |
| Storage | Local backup repository |
| Data Location | `/data/critical` |
| Backup Location | `/backup/daily` |

---

# 📂 Backup Architecture

The backup architecture:

```
/data/critical
        |
        |
        v
/usr/local/bin/backup-critical.sh
        |
        |
        v
/backup/daily/YYYY-MM-DD
```

Critical files are copied daily into a dated backup directory.

Example:

```
/backup
└── daily
    └── 2026-08-04
        ├── configuration.conf
        ├── database.sql
        ├── users.txt
        └── test_backup.txt
```

---

# 🗄️ Preparing Critical Data

A test dataset was created:

```bash
sudo mkdir -p /data/critical
```

Example files:

```
/data/critical
├── configuration.conf
├── database.sql
└── users.txt
```

Verification:

```bash
tree /data
```

Output:

```
/data
└── critical
    ├── configuration.conf
    ├── database.sql
    └── users.txt
```

---

# 🔄 Manual Backup Test with rsync

Initial backup:

```bash
sudo rsync -avh /data/critical/ /backup/daily/critical/
```

Result:

```
configuration.conf
database.sql
users.txt
```

Verification:

```bash
tree /backup
```

---

# 🔁 Restore Test

A file was intentionally deleted:

```bash
sudo rm /data/critical/configuration.conf
```

Verification:

```bash
ls /data/critical
```

Restore operation:

```bash
sudo rsync -avh /backup/daily/critical/ /data/critical/
```

Result:

```
configuration.conf restored
```

Verification:

```bash
cat /data/critical/configuration.conf
```

Output:

```
Server configuration backup
```

---

# 📝 Automated Backup Script

Backup script:

```
/usr/local/bin/backup-critical.sh
```

Permissions:

```bash
sudo chmod +x /usr/local/bin/backup-critical.sh
```

The script performs:

- creation of daily backup directories
- rsync synchronization
- backup status logging
- success/failure reporting
- retention cleanup

---

# 📜 Backup Script

```bash
#!/bin/bash

# NOVATECH Backup Script
# Lab 31 - Professional Backup & Restore

SOURCE="/data/critical"
DEST="/backup/daily/$(date +%F)"
LOG="/var/log/backup.log"

echo "===== Backup started $(date) =====" >> "$LOG"

mkdir -p "$DEST"

rsync -avh "$SOURCE/" "$DEST/" >> "$LOG" 2>&1

if [ $? -eq 0 ]; then
    echo "Backup completed successfully" >> "$LOG"
else
    echo "Backup failed" >> "$LOG"
fi

echo "===== Backup finished $(date) =====" >> "$LOG"
echo "" >> "$LOG"
```

---

# 🧪 Automated Backup Test

Manual execution:

```bash
sudo /usr/local/bin/backup-critical.sh
```

Backup created:

```bash
tree /backup/daily
```

Example:

```
/backup/daily
└── 2026-08-04
    ├── configuration.conf
    ├── database.sql
    ├── test_backup.txt
    └── users.txt
```

---

# 📊 Backup Logs

Logs are stored in:

```
/var/log/backup.log
```

Example:

```bash
sudo cat /var/log/backup.log
```

Output:

```
===== Backup started =====

sending incremental file list

configuration.conf
database.sql
users.txt

Backup completed successfully

===== Backup finished =====
```

---

# ⏱️ Systemd Automation

Two systemd units were created:

```
/etc/systemd/system/backup-critical.service
/etc/systemd/system/backup-critical.timer
```

---

## Backup Service

The service executes the backup script.

Example:

```ini
[Unit]
Description=NOVATECH Critical Data Backup

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup-critical.sh
```

---

## Backup Timer

The timer schedules automatic execution.

Example:

```ini
[Unit]
Description=Daily Backup Timer

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

---

# 🚀 Enable Automatic Backups

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Enable timer:

```bash
sudo systemctl enable --now backup-critical.timer
```

Check timer:

```bash
systemctl list-timers
```

---

# ▶️ Manual Execution Test

Run backup service:

```bash
sudo systemctl start backup-critical.service
```

Check logs:

```bash
sudo tail -30 /var/log/backup.log
```

Result:

```
Backup completed successfully
Old backups cleaned
```

---

# 🧹 Backup Retention

A retention policy was implemented.

Old backups are automatically removed to avoid unlimited storage growth.

Example:

```
Daily backups
        |
        |
        v
Retention cleanup
        |
        |
        v
Remove expired backups
```

---

# 🔐 Backup Strategy Summary

Implemented solution:

| Feature | Status |
|-|-|
| Manual backup | ✅ |
| rsync synchronization | ✅ |
| Restore testing | ✅ |
| Backup script | ✅ |
| Logging | ✅ |
| systemd service | ✅ |
| systemd timer | ✅ |
| Retention policy | ✅ |

---

# 🧠 Skills Learned

During this lab:

- Linux backup administration
- Data recovery procedures
- rsync usage
- Bash automation
- systemd scheduling
- Operational monitoring
- Backup lifecycle management

---

# 🏁 Conclusion

The NOVATECH backup infrastructure is now automated.

The server can:

- protect critical data
- create scheduled backups
- maintain backup history
- restore deleted files
- automatically clean obsolete backups

This configuration represents a simplified enterprise backup workflow used by system administrators.

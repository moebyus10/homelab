# 🕒 Cron & Scheduled Tasks

## 🎯 Objective

The objective of this lab is to understand how Linux task scheduling works using **cron**.

During this lab, I learned how to:

- Check and manage the cron service
- Create scheduled tasks for a user
- Automate the execution of Bash scripts
- Configure system-wide scheduled tasks
- Verify cron execution through system logs

---

# 🖥️ Environment

- **Operating System:** Ubuntu Server
- **Hostname:** Vega
- **User:** moebyus
- **Service:** cron
- **Shell:** Bash

---

# 🔍 Checking the Cron Service

First, I verified that the cron service was running.

Command executed:

    systemctl status cron

Verification:

    systemctl is-active cron

Result:

    active

The cron service was running correctly.

---

# 👤 User Cron Jobs

## Checking Existing Cron Tasks

I checked the current user's scheduled tasks:

    crontab -l

No previous user cron jobs were configured.

---

## Creating a Test Script

A dedicated directory was created:

    mkdir ~/cron-lab

A Bash script was created:

    nano ~/cron-lab/backup-test.sh

Script content:

    #!/bin/bash

    echo "Cron executed at $(date)" >> ~/cron-lab/log.txt

The script was made executable:

    chmod +x ~/cron-lab/backup-test.sh

Manual execution test:

    ~/cron-lab/backup-test.sh

Verification:

    cat ~/cron-lab/log.txt

Example output:

    Cron executed at sam. 01 août 2026 01:39:58 CEST
    Cron executed at sam. 01 août 2026 01:42:37 CEST
    Cron executed at sam. 01 août 2026 01:45:01 CEST

---

# ⏰ Scheduling a User Cron Task

The user crontab was edited:

    crontab -e

Scheduled task added:

    */5 * * * * /home/moebyus/cron-lab/backup-test.sh

This configuration executes the script every 5 minutes.

Verification:

    crontab -l

Output:

    */5 * * * * /home/moebyus/cron-lab/backup-test.sh

---

# 📋 Monitoring Cron Execution

Cron execution was verified through system logs:

    sudo journalctl -u cron --since "15 minutes ago"

Output:

    août 01 01:45:01 Vega CRON[3403]: (moebyus) CMD (/home/moebyus/cron-lab/backup-test.sh)

This confirmed that the scheduled task was executed automatically by Cron.

---

# 🖥️ System Cron Configuration

Linux also provides system-wide scheduled tasks.

A system cron job was created inside:

    /etc/cron.d/

Creation:

    sudo nano /etc/cron.d/system-test

Configuration:

    */10 * * * * root echo "System cron executed at $(date)" >> /var/log/system-cron-test.log

This task:

- Runs every 10 minutes
- Executes as root
- Writes execution logs into `/var/log/system-cron-test.log`

---

# 📄 System Cron Verification

After execution:

    sudo cat /var/log/system-cron-test.log

Output:

    System cron executed at sam. 01 août 2026 02:00:01 CEST

The system cron task executed successfully.

---

# 🧹 Cleanup

Temporary files and scheduled tasks were removed.

Remove user cron:

    crontab -r

Remove system cron:

    sudo rm /etc/cron.d/system-test

Remove log file:

    sudo rm /var/log/system-cron-test.log

Remove test directory:

    rm -rf ~/cron-lab

---

# ✅ Results

The lab was successfully completed.

I learned how to:

- Configure user cron jobs
- Create automated Bash scripts
- Schedule recurring commands
- Configure system-wide cron tasks
- Monitor scheduled tasks using system logs

---

# 📚 Skills Learned

- Linux task scheduling
- Cron syntax
- Bash scripting
- Automation
- System administration
- Log monitoring

---

# 🏁 Conclusion

This lab allowed me to understand how Linux automation works through **Cron**.

I learned how to create scheduled tasks for both users and the system, automate Bash scripts, and monitor their execution using system logs.

These skills are essential for system administration because scheduled tasks are widely used for:

- Automated backups
- System maintenance
- Log rotation
- Monitoring tasks
- Routine administrative operations

Cron is a fundamental Linux tool that helps administrators save time and improve system reliability by automating repetitive tasks.

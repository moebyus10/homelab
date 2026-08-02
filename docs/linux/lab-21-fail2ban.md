# 🔥 Fail2Ban

## 🎯 Objective

The goal of this lab is to install and configure Fail2Ban to protect the SSH service against suspicious authentication attempts and brute-force attacks.

During this lab, the following concepts were covered:

- Installing and managing Fail2Ban
- Configuring SSH protection
- Monitoring authentication attempts
- Managing ban policies
- Verifying security events through logs
- Integrating Fail2Ban with SSH hardening

---

# 📦 Installing Fail2Ban

Fail2Ban was installed using APT:

```bash
sudo apt install fail2ban

The service status was checked:

sudo systemctl status fail2ban

Result:

Active: active (running)

Fail2Ban is installed and running correctly.

🔎 Checking Fail2Ban Status

The global Fail2Ban status was verified:

sudo fail2ban-client status

Result:

Status
|- Number of jail:      1
`- Jail list:   sshd

The SSH jail is enabled and active.

🔐 Checking SSH Protection Jail

The SSH jail status was checked:

sudo fail2ban-client status sshd

Initial result:

Currently failed: 0
Total failed:     0
Currently banned: 0
Total banned:     0

Fail2Ban is monitoring SSH authentication events.

⚙️ Creating Custom SSH Protection Rules

A custom Fail2Ban configuration was created:

sudo nano /etc/fail2ban/jail.d/sshd.local

Configuration:

[sshd]
enabled = true
port = ssh
backend = systemd
maxretry = 3
findtime = 10m
bantime = 1h

Configuration details:

Parameter	Value	Description
maxretry	3	Number of failed attempts before ban
findtime	600 seconds	Detection time window
bantime	3600 seconds	Ban duration
backend	systemd	Uses system journal
🔄 Applying Fail2Ban Configuration

Fail2Ban was restarted:

sudo systemctl restart fail2ban

The configuration was verified:

sudo fail2ban-client get sshd bantime
sudo fail2ban-client get sshd maxretry
sudo fail2ban-client get sshd findtime

Results:

bantime = 3600
maxretry = 3
findtime = 600

The custom security policy is correctly applied.

📋 Monitoring Fail2Ban Logs

Fail2Ban logs were checked:

sudo tail -f /var/log/fail2ban.log

Logs confirmed:

INFO    [sshd] maxRetry: 3
INFO    findtime: 600
INFO    banTime: 3600
INFO    Jail 'sshd' started

The SSH protection jail started successfully.

🧪 SSH Detection Test

A connection attempt using an invalid SSH user was performed:

ssh testuser@localhost -p 2222

Result:

Permission denied (publickey).

The SSH journal was checked:

sudo journalctl -u ssh --since "5 minutes ago"

Detected event:

Invalid user testuser from 10.0.2.2
Connection reset by invalid user testuser

Fail2Ban detected the suspicious activity:

sudo fail2ban-client status sshd

Result:

Currently failed: 1
Total failed:     1
Currently banned: 0
Total banned:     0

The attempt was detected but no ban occurred because the configured threshold is:

maxretry = 3
🔒 SSH Security Context

SSH was previously hardened.

Current SSH configuration:

PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes

This prevents password brute-force attacks and enforces SSH key authentication.

Fail2Ban provides an additional security layer by monitoring suspicious SSH activity.

✅ Lab Validation

The following objectives were successfully completed:

✔ Fail2Ban installed and running
✔ SSH jail enabled
✔ Custom SSH protection configured
✔ Systemd journal monitoring enabled
✔ Invalid SSH attempt detection verified
✔ Ban policy configured
✔ SSH hardening maintained

🧠 Skills Practiced
Linux security administration
Intrusion prevention systems
SSH protection
Log monitoring
Fail2Ban configuration
Authentication security
Server hardening
🏁 Conclusion

This lab demonstrated how to protect a Linux server against SSH attacks using Fail2Ban.

Combined with SSH hardening measures such as disabling password authentication and blocking root login, Fail2Ban provides an additional defensive layer by detecting suspicious connection attempts.

The next step will be deploying Docker containers and learning container management.

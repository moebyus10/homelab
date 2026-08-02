# 🔥 SSH Hardening & Security

## 🎯 Objective

The goal of this lab is to improve SSH server security by applying hardening techniques while keeping remote administration functional.

During this lab, the following concepts were covered:

- SSH security auditing
- OpenSSH configuration hardening
- Disabling insecure authentication methods
- Restricting privileged access
- Reducing brute-force attack possibilities
- Validating SSH configuration changes
- Monitoring SSH authentication logs

---

# 🔎 SSH Security Audit

The current SSH configuration was checked:

```bash
sudo sshd -T
```

Security parameters were reviewed:

```bash
sudo sshd -T | grep permitrootlogin
sudo sshd -T | grep passwordauthentication
sudo sshd -T | grep pubkeyauthentication
sudo sshd -T | grep x11forwarding
sudo sshd -T | grep maxauthtries
```

Initial configuration:

```
permitrootlogin prohibit-password
passwordauthentication no
pubkeyauthentication yes
x11forwarding yes
maxauthtries 6
```

The SSH service was already using key authentication, but additional hardening was applied.

---

# 💾 SSH Configuration Backup

Before modifying SSH settings, a backup was created:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

Backup file:

```
/etc/ssh/sshd_config.backup
```

---

# 🔐 SSH Hardening Configuration

The SSH configuration file was edited:

```bash
sudo nano /etc/ssh/sshd_config
```

The following security settings were applied:

```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
X11Forwarding no
```

Security improvements:

- Root SSH login disabled
- Password authentication disabled
- Public key authentication enforced
- Authentication attempts reduced
- X11 forwarding disabled

---

# 🧹 SSH Configuration Cleanup

Multiple X11 forwarding directives were detected:

```bash
sudo grep -n "X11Forwarding" /etc/ssh/sshd_config
```

The configuration was cleaned to keep the secure setting:

```
X11Forwarding no
```

An unused configuration file was also identified:

```
/etc/ssh/sshd_configq
```

This file was removed because it was not used by SSH.

---

# ✅ SSH Configuration Validation

The SSH configuration syntax was tested:

```bash
sudo sshd -t
```

No errors were returned.

The SSH service was reloaded:

```bash
sudo systemctl reload ssh
```

Final configuration check:

```bash
sudo sshd -T | grep -E "permitrootlogin|passwordauthentication|pubkeyauthentication|x11forwarding|maxauthtries"
```

Final result:

```
maxauthtries 3
permitrootlogin no
pubkeyauthentication yes
passwordauthentication no
x11forwarding no
```

The SSH server is now hardened.

---

# 🧪 SSH Connectivity Test

A local SSH connection was tested:

```bash
ssh localhost
```

The connection succeeded after accepting the server fingerprint.

This confirmed that SSH hardening did not break administrative access.

---

# 📋 SSH Authentication Logs

SSH authentication logs were reviewed:

```bash
sudo journalctl -u ssh --since "10 minutes ago"
```

Successful authentication:

```
Accepted publickey for moebyus from 10.0.2.2
```

The logs confirmed:

- SSH key authentication works
- User sessions are correctly recorded
- Remote administration remains available

---

# ⚙️ SSH Service Status

The SSH service was checked:

```bash
sudo systemctl status ssh
```

Result:

```
Active: active (running)
```

The SSH daemon remains operational after hardening.

---

# ✅ Lab Validation

The following objectives were successfully completed:

✔ SSH configuration audited  
✔ SSH configuration backed up  
✔ Root login disabled  
✔ Password authentication disabled  
✔ Public key authentication validated  
✔ Maximum authentication attempts reduced  
✔ X11 forwarding disabled  
✔ SSH syntax validated  
✔ SSH service reloaded successfully  
✔ SSH connection tested after hardening  
✔ SSH authentication logs reviewed  

---

# 🧠 Skills Practiced

- OpenSSH administration
- Linux security hardening
- Secure authentication management
- SSH configuration troubleshooting
- System service management
- Security log analysis

---

# 🏁 Conclusion

This lab demonstrated how to harden an existing SSH service while maintaining secure remote administration.

The final SSH configuration reduces attack surface by disabling unnecessary access methods, limiting authentication attempts and enforcing secure key-based authentication.

The SSH service remains fully operational with an improved security posture.

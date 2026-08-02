# 🔥Firewall & Web Service Exposure

## 🎯 Objective

The goal of this lab is to understand Linux firewall management using UFW and expose a web service securely.

During this lab, the following concepts were covered:

- Configuring and managing UFW firewall rules
- Allowing and removing network services
- Installing and managing a web server
- Checking listening ports
- Testing service accessibility locally and remotely

---

# 🛡️ Firewall Configuration with UFW

## Checking Firewall Status

The firewall status was checked with:

```bash
sudo ufw status verbose
```

Initial configuration:

```
Status: active

Default:
deny (incoming)
allow (outgoing)
```

Allowed services:

- SSH (port 22)
- HTTPS (port 443)

---

# 🔎 Listing Firewall Rules

Rules were displayed with:

```bash
sudo ufw status numbered
```

Current rules:

```
[1] OpenSSH                    ALLOW IN    Anywhere
[2] 443/tcp                    ALLOW IN    Anywhere
[3] OpenSSH (v6)               ALLOW IN    Anywhere (v6)
[4] 443/tcp (v6)               ALLOW IN    Anywhere (v6)
```

---

# ➕ Adding and Removing Firewall Rules

A temporary rule was created to allow port 8080:

```bash
sudo ufw allow 8080/tcp
```

Verification:

```bash
sudo ufw status numbered
```

The rule was then removed:

```bash
sudo ufw delete <rule_number>
```

IPv4 and IPv6 rules were successfully removed.

---

# 🌐 Installing Nginx Web Server

Nginx was installed:

```bash
sudo apt install nginx
```

The service status was checked:

```bash
systemctl status nginx
```

Result:

```
Active: active (running)
```

The service is enabled and running correctly.

---

# 🔌 Checking Listening Ports

Listening services were checked:

```bash
sudo ss -tulnp | grep nginx
```

Result:

```
tcp LISTEN 0.0.0.0:80
tcp LISTEN [::]:80
```

Nginx is listening on HTTP port 80.

---

# 🧪 Testing Web Service Locally

The web server was tested locally:

```bash
curl localhost
```

Response:

```
Welcome to nginx!
```

The default Nginx page confirms that the web service is working.

---

# 🌍 Opening HTTP Access Through Firewall

HTTP access was allowed:

```bash
sudo ufw allow 80/tcp
```

Verification:

```bash
sudo ufw status numbered
```

Final firewall rules:

```
[1] OpenSSH                    ALLOW IN    Anywhere
[2] 443/tcp                    ALLOW IN    Anywhere
[3] 80/tcp                     ALLOW IN    Anywhere
[4] OpenSSH (v6)               ALLOW IN    Anywhere (v6)
[5] 443/tcp (v6)               ALLOW IN    Anywhere (v6)
[6] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
```

---

# 🖥️ Remote Access Test

The web server was tested from the Windows host:

```powershell
curl http://localhost
```

The Nginx welcome page was successfully displayed.

The service is therefore accessible from outside the VM through the configured network forwarding.

---

# 🔒 Firewall Blocking Test

A temporary service was exposed on port 8080 to verify firewall behavior.

Port 8080 was first blocked, then allowed through UFW.

The service became reachable only after the firewall rule was added.

This confirmed that UFW correctly filters incoming network traffic.

---

# ✅ Lab Validation

The following objectives were successfully completed:

✔ UFW firewall enabled and configured  
✔ Firewall rules created and removed  
✔ SSH access preserved  
✔ Nginx installed and running  
✔ HTTP service exposed through firewall  
✔ Network accessibility tested from host machine  

---

# 🧠 Skills Practiced

- Linux firewall administration
- Network service exposure
- TCP port management
- Web server deployment
- Service verification with systemd
- Remote connectivity testing

---

# 🏁 Conclusion

This lab demonstrated how to securely expose a Linux service while controlling network access through a firewall.

The next step will be web server administration, including Nginx configuration, virtual hosts, and website deployment.

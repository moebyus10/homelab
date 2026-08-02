# 🌐 Nginx Web Server Administration

## 🎯 Objective

The goal of this lab is to understand web service deployment and secure exposure using Nginx and UFW.

During this lab, the following concepts were covered:

- Configuring and managing UFW firewall rules
- Installing and managing a Nginx web server
- Exposing HTTP services securely
- Understanding Nginx configuration structure
- Creating and managing Server Blocks
- Deploying a custom website
- Monitoring web server logs
- Testing service accessibility locally and remotely

---

# 🛡️ Firewall Configuration with UFW

## Checking Firewall Status

The firewall status was checked with:

    sudo ufw status verbose

Initial configuration:

    Status: active

    Default:
    deny (incoming)
    allow (outgoing)

Allowed services:

- SSH (port 22)
- HTTPS (port 443)

---

# 🔎 Listing Firewall Rules

Rules were displayed with:

    sudo ufw status numbered

Initial rules:

    [1] OpenSSH                    ALLOW IN    Anywhere
    [2] 443/tcp                    ALLOW IN    Anywhere
    [3] OpenSSH (v6)               ALLOW IN    Anywhere (v6)
    [4] 443/tcp (v6)               ALLOW IN    Anywhere (v6)

---

# ➕ Adding and Removing Firewall Rules

A temporary rule was created to test firewall behavior:

    sudo ufw allow 8080/tcp

The rule was verified:

    sudo ufw status numbered

The temporary rule was then removed:

    sudo ufw delete <rule_number>

IPv4 and IPv6 rules were successfully removed.

This confirmed that UFW correctly manages incoming network access.

---

# 🌐 Installing Nginx Web Server

Nginx was installed:

    sudo apt install nginx

The service status was checked:

    systemctl status nginx

Result:

    Active: active (running)

The Nginx service is enabled and running correctly.

---

# 🔌 Checking Listening Ports

Listening services were checked:

    sudo ss -tulnp | grep nginx

Result:

    tcp LISTEN 0.0.0.0:80
    tcp LISTEN [::]:80

Nginx is listening correctly on HTTP port 80.

---

# 🧪 Testing Default Web Service

The default web server was tested locally:

    curl localhost

Response:

    Welcome to nginx!

The default Nginx page confirmed that the web service was working.

---

# 🌍 Opening HTTP Access Through Firewall

HTTP access was allowed:

    sudo ufw allow 80/tcp

Verification:

    sudo ufw status numbered

Final firewall rules:

    [1] OpenSSH                    ALLOW IN    Anywhere
    [2] 443/tcp                    ALLOW IN    Anywhere
    [3] 80/tcp                     ALLOW IN    Anywhere
    [4] OpenSSH (v6)               ALLOW IN    Anywhere (v6)
    [5] 443/tcp (v6)               ALLOW IN    Anywhere (v6)
    [6] 80/tcp (v6)                ALLOW IN    Anywhere (v6)

HTTP traffic is now allowed through the firewall.

---

# 🖥️ Remote Access Test

The web server was tested from the Windows host:

    curl http://localhost

The Nginx welcome page was successfully displayed.

The service is accessible from outside the VM through the configured network forwarding.

---

# 📁 Creating a Custom Website

A dedicated directory was created:

    /var/www/homelab

A custom webpage was created:

    /var/www/homelab/index.html

Website content:

    Linux Homelab

    Welcome to my Nginx web server!

    This website is hosted on Ubuntu 26.04.

The website is now independent from the default Nginx page.

---

# ⚙️ Creating a Nginx Server Block

A custom website configuration was created:

    /etc/nginx/sites-available/homelab

The Server Block defines:

- Website hostname
- Document root
- Index page
- Access logs
- Error logs

Configuration summary:

    server_name homelab.local;

    root /var/www/homelab;

    index index.html;

Custom logs:

    /var/log/nginx/homelab_access.log
    /var/log/nginx/homelab_error.log

---

# 🔗 Enabling the Website

The website configuration was enabled:

    sudo ln -s /etc/nginx/sites-available/homelab /etc/nginx/sites-enabled/

Verification:

    ls -l /etc/nginx/sites-enabled

Result:

    default -> /etc/nginx/sites-available/default
    homelab -> /etc/nginx/sites-available/homelab

The custom website is now enabled.

---

# ✅ Testing Nginx Configuration

Before applying changes, the configuration was tested:

    sudo nginx -t

Result:

    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: the configuration file /etc/nginx/nginx.conf test is successful

The configuration syntax is valid.

---

# 🔄 Reloading Nginx

The service was reloaded:

    sudo systemctl reload nginx

Service verification:

    sudo systemctl status nginx

Result:

    Active: active (running)

Nginx successfully loaded the new configuration.

---

# 🖥️ Local Domain Configuration

A local hostname was configured:

    /etc/hosts

Entry added:

    127.0.0.1 homelab.local

Testing:

    ping -c 2 homelab.local

Result:

    PING homelab.local (127.0.0.1)

The hostname correctly resolves to the local web server.

---

# 🧪 Testing Custom Website

The custom website was tested:

    curl http://homelab.local

Response:

    <h1>Linux Homelab</h1>

    <p>Welcome to my Nginx web server!</p>

    <p>This website is hosted on Ubuntu 26.04.</p>

The custom website is successfully served by Nginx.

---

# 📊 Log Monitoring

Access logs were checked:

    sudo tail -f /var/log/nginx/homelab_access.log

Example:

    127.0.0.1 - - [02/Aug/2026:17:20:27 +0200] "GET / HTTP/1.1" 200 244 "-" "curl/8.18.0"

The HTTP status code:

    200

confirms successful page delivery.

The error log was checked:

    sudo tail -n 20 /var/log/nginx/homelab_error.log

No errors were reported.

---

# 🔐 Permissions Verification

Website permissions were checked:

    ls -ld /var/www/homelab

Result:

    drwxr-xr-x root root /var/www/homelab

Files:

    ls -l /var/www/homelab

Result:

    -rw-r--r-- root root index.html

The website files are owned by root and only readable by the web server.

---

# 🔒 Firewall Blocking Test

A temporary service was exposed on port 8080 to verify firewall behavior.

Port 8080 was blocked initially, then allowed through UFW.

The service became reachable only after adding the firewall rule.

This confirmed that UFW correctly filters incoming network traffic.

---

# 🔎 Final Service Verification

Nginx listening ports were verified:

    sudo ss -tulnp | grep nginx

Result:

    tcp LISTEN 0.0.0.0:80
    tcp LISTEN [::]:80

The web service is correctly exposed.

---

# ✅ Lab Validation

The following objectives were successfully completed:

✔ UFW firewall enabled and configured  
✔ Firewall rules created and removed  
✔ SSH access preserved  
✔ Nginx installed and running  
✔ HTTP service exposed securely  
✔ Custom website deployed  
✔ Nginx Server Block configured  
✔ Local hostname configured  
✔ Logs monitored successfully  
✔ Network accessibility tested  

---

# 🧠 Skills Practiced

- Linux firewall administration
- Web server deployment
- Nginx configuration management
- Virtual host administration
- HTTP service exposure
- Network troubleshooting
- Log analysis
- systemd service management
- Linux file permissions

---

# 🏁 Conclusion

This lab demonstrated how to securely deploy and manage a Nginx web server on Ubuntu.

The service was first exposed through UFW firewall rules, then customized using a dedicated Server Block and a personal website.

The deployment was validated through HTTP requests, network inspection, permission checks and log analysis.

The next step will be database administration with MariaDB, including installation, user management and database operations.

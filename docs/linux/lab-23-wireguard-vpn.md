# 🔐 WireGuard VPN

## 🎯 Objective

The goal of this lab was to install and configure WireGuard on Ubuntu 26.04 in order to understand the fundamentals of modern VPN technology.

During this lab, the following concepts were covered:

- Installing WireGuard
- Generating cryptographic key pairs
- Protecting sensitive key files
- Configuring a WireGuard interface
- Managing the VPN service with systemd
- Verifying interface configuration
- Understanding WireGuard architecture

---

## 📦 Installing WireGuard

WireGuard was installed using the Ubuntu repositories.

```bash
sudo apt update
sudo apt install -y wireguard
```

The installation was verified:

```bash
wg --version
```

**Result:**

```text
wireguard-tools v1.0.20250521
```

The WireGuard service template was also verified:

```bash
sudo systemctl status wg-quick@wg0
```

Initially, the service was inactive because no configuration file had been created.

---

## 🔑 Generating Cryptographic Keys

The WireGuard configuration directory was secured:

```bash
sudo mkdir -p /etc/wireguard
sudo chmod 700 /etc/wireguard
```

A private key was generated:

```bash
wg genkey | sudo tee /etc/wireguard/private.key > /dev/null
```

Permissions were restricted:

```bash
sudo chmod 600 /etc/wireguard/private.key
```

The corresponding public key was generated:

```bash
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key > /dev/null
```

The public key is intended to be shared with remote peers, while the private key must remain secret.

The generated files were verified:

```bash
sudo ls -l /etc/wireguard
```

**Result:**

```text
-rw------- private.key
-rw-r--r-- public.key
```

---

## ⚙️ Creating the WireGuard Interface

A WireGuard configuration file was created:

```bash
sudo nano /etc/wireguard/wg0.conf
```

Configuration:

```ini
[Interface]
Address = 10.10.10.1/24
ListenPort = 51820
PrivateKey = <private_key>

SaveConfig = true
```

Permissions were secured:

```bash
sudo chmod 600 /etc/wireguard/wg0.conf
```

---

## 🚨 Troubleshooting Configuration

The first startup attempt failed because the configuration file still contained the placeholder:

```text
PrivateKey = <PRIVATE_KEY>
```

WireGuard returned the following error:

```text
Key is not the correct length or format
Configuration parsing error
```

After replacing the placeholder with the actual private key stored in `/etc/wireguard/private.key`, the service started successfully.

This demonstrated the importance of correct key formatting when configuring WireGuard.

---

## ▶️ Starting the VPN Interface

The WireGuard interface was started:

```bash
sudo systemctl restart wg-quick@wg0
```

Service status:

```bash
sudo systemctl status wg-quick@wg0
```

**Result:**

```text
Active: active (exited)
```

The VPN interface was successfully created.

---

## 🔍 Verifying the Configuration

WireGuard status:

```bash
sudo wg
```

**Result:**

```text
interface: wg0
public key: (generated)
private key: (hidden)
listening port: 51820
```

The network interface was verified:

```bash
ip addr show wg0
```

**Result:**

```text
wg0
inet 10.10.10.1/24
state UNKNOWN
```

The interface is operational and listening for WireGuard peers.

---

## 🌐 Understanding WireGuard Architecture

This lab was completed using a single virtual machine.

As a result, only the local WireGuard interface was configured.

A complete VPN tunnel requires at least two peers:

- Client ↔ Server
- VM ↔ VM
- Site ↔ Site

Although no remote peer was configured, the lab demonstrated all required steps to prepare a functional WireGuard endpoint.

---

## ✅ Lab Validation

The following objectives were successfully completed:

- [x] WireGuard installed
- [x] Cryptographic keys generated
- [x] Configuration directory secured
- [x] Private key protected
- [x] WireGuard interface configured
- [x] VPN service started
- [x] Network interface verified
- [x] Listening port configured
- [x] Basic WireGuard administration completed

---

## 🧠 Skills Practiced

- VPN fundamentals
- WireGuard administration
- Linux networking
- Public/private key cryptography
- Service management with systemd
- Network interface configuration
- Secure file permissions
- Troubleshooting configuration errors

---

## 🏁 Conclusion

This lab introduced the deployment and administration of WireGuard on Ubuntu 26.04.

A complete WireGuard endpoint was configured by generating a cryptographic key pair, creating a VPN interface, securing sensitive files, and managing the service with systemd.

Although the homelab contains only one virtual machine, the environment is fully prepared for future VPN deployments involving additional clients or servers. The knowledge gained in this lab provides a solid foundation for implementing secure site-to-site or remote-access VPN solutions.

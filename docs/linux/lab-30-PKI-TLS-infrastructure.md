# 🔐 PKI & TLS Infrastructure

## 📌 Overview

This lab focuses on implementing a complete Public Key Infrastructure (PKI) environment and deploying a trusted HTTPS service using certificates generated internally.

The objective is to understand how enterprises manage digital certificates, Certificate Authorities (CA), TLS encryption, and secure web services.

During this lab, a private Root Certificate Authority was created, used to sign a server certificate, and deployed on an Nginx HTTPS server.

---

# 🎯 Objectives

- Understand PKI concepts and certificate chains
- Create a private Root Certificate Authority
- Generate server certificates
- Sign certificates using an internal CA
- Configure HTTPS with Nginx
- Enable TLS encryption
- Validate certificate trust and TLS negotiation
- Import an internal CA into Windows trust store

---

# 🏗️ Lab Architecture

```
                    +----------------------+
                    |  NOVATECH Root CA    |
                    |  Private Certificate |
                    |      Authority       |
                    +----------+-----------+
                               |
                               |
                     Signs server certificate
                               |
                               |
                    +----------v-----------+
                    |   NOVATECH-ADMIN01   |
                    |      Nginx HTTPS     |
                    |     TLS Certificate  |
                    +----------+-----------+
                               |
                               |
                     HTTPS connection
                               |
              +----------------+----------------+
              |                                 |
        Ubuntu Client                  Windows Client
        curl/OpenSSL                  Browser/curl
```

---

# 🔑 Step 1 — Creating the Root CA

A private Root Certificate Authority was created using OpenSSL.

The Root CA contains:

- RSA 4096-bit private key
- Self-signed certificate
- Internal organization information

Example identity:

```
C=FR
ST=Bretagne
L=Rennes
O=NOVATECH
OU=Infrastructure
CN=NOVATECH Root CA
```

The Root CA is responsible for signing internal certificates.

---

# 📜 Step 2 — Generating the Server Certificate

A server private key and certificate request were generated.

Server identity:

```
CN=NOVATECH-ADMIN01
```

Certificate properties:

```
Algorithm:
RSA 4096 bits

Signature:
sha256WithRSAEncryption

Issuer:
NOVATECH Root CA

Validity:
August 2026 - November 2028
```

---

# ✅ Step 3 — Certificate Verification

The generated certificate was verified against the Root CA:

```bash
openssl verify \
-CAfile certs/rootCA.crt \
certs/server.crt
```

Result:

```
certs/server.crt: OK
```

The certificate chain was successfully validated.

---

# 🌐 Step 4 — Nginx HTTPS Configuration

The server certificate was deployed on Nginx.

Certificate location:

```
/etc/nginx/ssl/server.crt
```

Private key:

```
/etc/nginx/ssl/server.key
```

HTTPS virtual host:

```
/etc/nginx/sites-available/novatech-https
```

Configuration:

```nginx
server {

    listen 443 ssl;

    server_name NOVATECH-ADMIN01 localhost;

    ssl_certificate /etc/nginx/ssl/server.crt;
    ssl_certificate_key /etc/nginx/ssl/server.key;

    ssl_protocols TLSv1.2 TLSv1.3;

    root /var/www/html;

    index index.html;

}
```

---

# 🔒 Step 5 — TLS Validation

Nginx HTTPS service verification:

```bash
sudo systemctl status nginx
```

Result:

```
Active: active (running)
```

Listening HTTPS port:

```bash
ss -tlnp | grep 443
```

Result:

```
LISTEN 0 511 0.0.0.0:443
```

---

# 🧪 Step 6 — Testing HTTPS with curl

Local Linux test:

```bash
curl https://NOVATECH-ADMIN01
```

Result:

```html
<h1>🔐 NOVATECH PKI & TLS Lab</h1>

<p>
HTTPS is successfully configured using a certificate signed by the NOVATECH Root CA.
</p>

<p>
Server: NOVATECH-ADMIN01
</p>
```

---

# 🔍 Step 7 — Testing TLS with OpenSSL

TLS handshake verification:

```bash
openssl s_client -connect NOVATECH-ADMIN01:443
```

Successful result:

```
Verification: OK

Verify return code: 0 (ok)

Protocol:
TLSv1.3

Cipher:
TLS_AES_256_GCM_SHA384
```

The server successfully negotiated a secure TLS 1.3 connection.

---

# 🖥️ Step 8 — Windows Trust Configuration

The internal Root CA certificate was imported into the Windows certificate store.

Certificate store:

```
Trusted Root Certification Authorities
```

After importing the Root CA, Windows trusted certificates signed by:

```
NOVATECH Root CA
```

---

# 🧪 Step 9 — Windows HTTPS Test

Test from Windows:

```powershell
curl.exe --ssl-no-revoke https://NOVATECH-ADMIN01
```

Successful response:

```html
<h1>🔐 NOVATECH PKI & TLS Lab</h1>

<p>
HTTPS is successfully configured using a certificate signed by the NOVATECH Root CA.
</p>

<p>
Server: NOVATECH-ADMIN01
</p>
```

---

# ⚠️ Note About Certificate Revocation

Windows attempted to verify certificate revocation status:

```
CRYPT_E_NO_REVOCATION_CHECK
```

This is expected because the lab PKI does not currently provide:

- Certificate Revocation Lists (CRL)
- OCSP responder

Enterprise PKI infrastructures usually include these components.

---

# 📚 Skills Learned

## Linux Administration

- Nginx HTTPS deployment
- TLS configuration
- Certificate management
- OpenSSL administration

## Cybersecurity

- Public Key Infrastructure (PKI)
- Certificate chains
- Trust relationships
- TLS encryption
- Internal Certificate Authorities

## Enterprise Concepts

- Private CA management
- Internal certificates
- Secure service deployment
- Windows certificate trust management

---

# 🏁 Lab Conclusion

This lab successfully implemented a complete internal PKI environment.

A private NOVATECH Root CA was created, used to sign a server certificate, and deployed on an HTTPS-enabled Nginx server.

The final architecture reproduces a simplified enterprise certificate management workflow:

```
Root CA
   |
   |
Server Certificate
   |
   |
Nginx HTTPS Service
   |
   |
Trusted Client Connection
```

The NOVATECH infrastructure now provides trusted TLS encryption using internally managed certificates.


# Part 3: Use SSL Analysis Tools in Kali Linux 

This lab introduces several SSL/TLS tools that come preinstalled (or are easily installable) in Kali Linux. These tools are widely used by **penetration testers, security analysts, SOC analysts, blue teams, and network administrators** to assess SSL/TLS configurations, analyze encrypted traffic, and troubleshoot secure communications.

> **Note:** Only scan or intercept traffic on systems you own or have explicit permission to test. Some of the tools below can be used for man-in-the-middle (MitM) testing and should only be used in authorized lab environments.

---

# What is SSL/TLS?

When you visit:

```text
https://google.com
```

your browser establishes an encrypted TLS connection.

Example:

```
Browser
    │
    │ TLS Handshake
    ▼
Google Server

Encrypted Communication
```

The tools in this lab help analyze this secure connection in different ways.

---

# Overview of the SSL Tools

| Tool     | Purpose                                         | Category               |
| -------- | ----------------------------------------------- | ---------------------- |
| sslscan  | Analyze supported SSL/TLS protocols and ciphers | Reconnaissance         |
| ssldump  | Decode SSL/TLS traffic (when possible)          | Traffic Analysis       |
| sslh     | Share one TCP port among multiple services      | Utility                |
| sslsplit | Intercept SSL/TLS traffic in a lab (MitM)       | Testing / Exploitation |
| sslyze   | Comprehensive SSL/TLS configuration scanner     | Reconnaissance         |

---

# Step 1: Open Kali Linux

Login:

```
Username: kali
Password: kali
```

Open Terminal.

---

# Step 2: Check Whether the Tools are Installed

Run:

```bash
which sslscan
which ssldump
which sslh
which sslsplit
which sslyze
```

Example:

```text
/usr/bin/sslscan
/usr/bin/ssldump
/usr/bin/sslh
/usr/bin/sslsplit
/usr/bin/sslyze
```

If a tool is missing:

```bash
sudo apt update
sudo apt install sslscan ssldump sslh sslsplit sslyze
```

---

# Tool 1 — sslscan

## What is sslscan?

**sslscan** checks how securely a web server is configured.

It discovers:

* Supported TLS versions
* Supported cipher suites
* Weak encryption
* Certificate details
* Protocol vulnerabilities

It **does not exploit** the server. It simply gathers information.

Category:

> Reconnaissance

---

## Basic Syntax

```bash
sslscan hostname
```

Example

```bash
sslscan google.com
```

---

## What Happens?

```
Your PC
      │
      │
      │  SSL Handshake
      ▼
Google Server
```

sslscan attempts multiple TLS handshakes using different protocol versions and cipher suites to see what the server accepts.

---

## Example Output

```text
SSL/TLS Protocols

TLSv1.2 Enabled

TLSv1.3 Enabled

SSLv3 Disabled

TLSv1 Disabled
```

---

Cipher list

```text
TLS_AES_256_GCM_SHA384

TLS_CHACHA20_POLY1305_SHA256
```

---

Certificate

```text
Issuer

Google Trust Services

Expires

January 2027
```

---

## Useful Commands

### Scan HTTPS port

```bash
sslscan example.com
```

---

### Specify port

```bash
sslscan example.com:8443
```

---

### Quiet output

```bash
sslscan --no-colour example.com
```

---

## Practical Use

An ethical hacker can determine whether:

* TLS 1.0 is enabled
* Weak ciphers are allowed
* The certificate is expired
* SSLv2 or SSLv3 is still supported

---

# Tool 2 — ssldump

## What is ssldump?

ssldump analyzes SSL/TLS traffic.

It is similar to tcpdump but understands SSL/TLS handshakes and protocol messages.

Category:

Traffic analysis (the lab labels it under exploitation because it is often used in offensive testing, but it is also valuable for troubleshooting).

---

## Basic Syntax

```bash
sudo ssldump
```

---

## Capture from Interface

```bash
sudo ssldump -i eth0
```

or

```bash
sudo ssldump -i wlan0
```

---

## Observe HTTPS Connections

Example

Open

```
https://example.com
```

Terminal displays

```text
Client Hello

Server Hello

Certificate

Finished
```

---

## What Can You Learn?

* TLS Version
* Cipher Negotiation
* Certificate Exchange
* Handshake Timing

Without the necessary private keys, modern TLS application data remains encrypted.

---

# Tool 3 — sslh

## What is sslh?

Normally

```
Port 80 → HTTP

Port 443 → HTTPS
```

But what if you want

* HTTPS
* SSH
* OpenVPN

all on the same port?

sslh solves this problem.

Category

Utility

---

## Example

Without sslh

```
22 SSH

443 HTTPS
```

With sslh

```
443

↓

HTTPS

SSH

OpenVPN
```

---

## Install

```bash
sudo apt install sslh
```

---

## Example Configuration

```text
Incoming Port

443

↓

If HTTPS

↓

Apache

If SSH

↓

OpenSSH
```

---

## Practical Use

Useful when:

* Firewalls only allow port 443
* Multiple services must share one external port

---

# Tool 4 — sslsplit

## What is sslsplit?

sslsplit performs SSL/TLS interception in a controlled environment.

It works by terminating the client's TLS connection and creating a separate TLS connection to the destination server, allowing inspection of traffic **only when clients trust the interception certificate**.

Category

MitM testing / Exploitation (authorized environments only)

---

## How it Works

```
Victim

↓

TLS

↓

sslsplit

↓

TLS

↓

Website
```

---

## Example

Run

```bash
sudo sslsplit
```

with an appropriate lab configuration, certificate, and network redirection.

The client connects to sslsplit instead of directly to the server.

sslsplit:

* decrypts
* inspects
* re-encrypts

before forwarding the traffic.

---

## Practical Uses

Security professionals use it to:

* Test corporate proxies
* Inspect TLS in a lab
* Validate security appliances
* Debug applications

---

# Tool 5 — sslyze

## What is sslyze?

sslyze is one of the most comprehensive SSL/TLS scanners available.

It checks

* Protocol support
* Cipher suites
* Certificate chain
* Renegotiation
* Compression
* Session resumption
* OCSP stapling
* Security configuration

Category

Reconnaissance

---

## Basic Syntax

```bash
sslyze hostname
```

Example

```bash
sslyze google.com
```

---

## Example Output

```
TLS 1.2

Supported

TLS 1.3

Supported

TLS 1.0

Disabled
```

---

Certificate

```
Issuer

Google Trust Services
```

---

Cipher

```
AES256-GCM

CHACHA20
```

---

## Scan Specific Port

```bash
sslyze example.com:443
```

---

## Practical Use

Security teams use sslyze to:

* Audit web servers
* Detect weak TLS settings
* Verify certificate chains
* Check compliance with security standards

---

# Comparison of All Tools

| Tool     | Main Purpose                               | Typical User           |
| -------- | ------------------------------------------ | ---------------------- |
| sslscan  | Check TLS protocols, ciphers, certificates | Pentester, SOC Analyst |
| ssldump  | Observe TLS handshakes and metadata        | Network Analyst        |
| sslh     | Share one port among multiple services     | System Administrator   |
| sslsplit | Controlled TLS interception in a lab       | Penetration Tester     |
| sslyze   | Comprehensive TLS security audit           | Security Engineer      |


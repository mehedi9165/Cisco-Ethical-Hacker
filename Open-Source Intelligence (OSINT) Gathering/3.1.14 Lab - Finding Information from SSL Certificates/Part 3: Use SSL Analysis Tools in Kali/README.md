# Lab: Part 3 – Use SSL Analysis Tools in Kali Linux (Step-by-Step Guide)

This lab introduces the **SSL/TLS security tools** that come with Kali Linux. These tools are commonly used by **penetration testers**, **security researchers**, and **network administrators** to assess the security of encrypted connections.

> **Important:** Use these tools only on systems you own or have explicit permission to test.

---

# Lab Objective

After completing this lab, you will be able to:

* Identify SSL-related tools available in Kali Linux
* Understand the purpose of each tool
* Distinguish between reconnaissance, exploitation, and utility tools
* Run basic commands to explore each tool

---

# Step 1: Start Kali Linux

1. Open **VMware** or **VirtualBox**.
2. Start your Kali Linux virtual machine.
3. Log in with:

```
Username: kali
Password: kali
```

You should see the Kali desktop.

---

# Step 2: Open the Terminal

Click the **Terminal** icon or press:

```
Ctrl + Alt + T
```

You should see:

```bash
kali@kali:~$
```

---

# Step 3: Search for SSL Tools

There are two easy ways to find SSL-related tools.

### Method 1: Kali Menu

1. Click the Kali (dragon) icon.
2. Type:

```
ssl
```

You should see tools such as:

```
sslscan
sslyze
ssldump
sslsplit
sslh
```

---

### Method 2: Terminal

Search installed packages:

```bash
apt list --installed | grep ssl
```

or check whether a specific tool is installed:

```bash
which sslscan
which sslyze
which ssldump
```

Example:

```text
/usr/bin/sslscan
/usr/bin/sslyze
```

---

# Tool 1 — sslscan

## What is sslscan?

`sslscan` is a reconnaissance tool that checks the SSL/TLS configuration of a remote server.

It reports:

* Supported TLS versions
* Supported cipher suites
* Weak ciphers
* Certificate details
* Security configuration

---

## Basic Syntax

```bash
sslscan example.com
```

or

```bash
sslscan example.com:443
```

---

## Example

```bash
sslscan google.com
```

Example output:

```text
SSL/TLS Protocols

TLSv1.2 Enabled

TLSv1.3 Enabled

TLSv1 Disabled

SSLv3 Disabled
```

It also lists cipher suites such as:

```text
TLS_AES_256_GCM_SHA384

TLS_CHACHA20_POLY1305_SHA256
```

---

## Why use it?

It helps determine whether a server:

* Uses outdated SSL versions
* Supports weak encryption
* Is configured securely

---

## Category

✅ **Reconnaissance**

---

# Tool 2 — ssldump

## What is ssldump?

`ssldump` captures and decodes SSL/TLS traffic.

It is similar to Wireshark but focuses on SSL/TLS sessions.

It can display:

* SSL handshakes
* Certificates
* Encrypted session details (when keys are available)

---

## Basic Syntax

```bash
sudo ssldump -i eth0
```

Monitor a specific host:

```bash
sudo ssldump host example.com
```

---

## Example Output

```text
SSL Handshake

Client Hello

Server Hello

Certificate

Finished
```

---

## Why use it?

Useful for:

* Troubleshooting SSL
* Inspecting handshakes
* Security research

---

## Category

✅ **Exploitation / Analysis**

(It analyzes SSL traffic and may be used during authorized security assessments.)

---

# Tool 3 — sslh

## What is sslh?

`sslh` is a protocol multiplexer.

It allows multiple services to share **port 443**.

Example:

```
Port 443

↓

HTTPS

SSH

OpenVPN
```

The tool automatically detects which protocol is being used.

---

## Example

Instead of:

```
443 → HTTPS

22 → SSH
```

You can configure:

```
443 → HTTPS + SSH
```

---

## Why use it?

Useful when:

* Firewalls only allow port 443
* Hosting multiple secure services on one port

---

## Category

✅ **Utility**

---

# Tool 4 — sslsplit

## What is sslsplit?

`sslsplit` performs SSL/TLS interception for **authorized** testing.

It acts as a **man-in-the-middle (MITM) proxy**.

Example:

```
Client

↓

sslsplit

↓

Website
```

It decrypts traffic for inspection during controlled security testing.

---

## Example

```bash
sslsplit -D
```

Starts in debug mode (requires additional configuration).

---

## Why use it?

Used by:

* Security researchers
* Penetration testers
* Malware analysts

to inspect encrypted traffic in environments where they have permission.

---

## Category

✅ **Exploitation**

---

# Tool 5 — sslyze

## What is sslyze?

`sslyze` is an advanced SSL/TLS configuration analyzer.

It checks:

* TLS versions
* Cipher suites
* Certificate validity
* Heartbleed exposure
* Compression
* Renegotiation
* OCSP
* HSTS

---

## Basic Syntax

```bash
sslyze google.com
```

---

## Example

```bash
sslyze example.com
```

Possible output:

```text
TLS 1.2

Supported

TLS 1.3

Supported

Heartbleed

Not Vulnerable

Certificate

Valid
```

---

## Why use it?

Useful for auditing the security of HTTPS services and identifying misconfigurations.

---

## Category

✅ **Reconnaissance**

---

# Comparison Table

| Tool     | Purpose                                                           | Category                |
| -------- | ----------------------------------------------------------------- | ----------------------- |
| sslscan  | Queries SSL services to determine supported protocols and ciphers | Reconnaissance          |
| ssldump  | Analyzes and decodes SSL/TLS traffic                              | Exploitation / Analysis |
| sslh     | Allows multiple services to share TCP port 443                    | Utility                 |
| sslsplit | Intercepts SSL/TLS connections for authorized MITM testing        | Exploitation            |
| sslyze   | Performs detailed SSL/TLS configuration analysis                  | Reconnaissance          |

---

# Understanding the Categories

### Reconnaissance

Collects information without attempting to compromise the target.

Examples:

* sslscan
* sslyze

---

### Exploitation

Used during authorized penetration tests to analyze or intercept communications.

Examples:

* ssldump
* sslsplit

---

### Utility

Provides supporting functionality.

Example:

* sslh

---

# Real-World Example

Suppose a company hosts:

```
https://company.com
```

A security tester may:

**Step 1**

Run:

```bash
sslscan company.com
```

to identify supported protocols and ciphers.

**Step 2**

Run:

```bash
sslyze company.com
```

to perform a deeper configuration assessment.

**Step 3**

If investigating SSL traffic in a controlled lab, use:

```bash
ssldump
```

to inspect the TLS handshake.

**Step 4**

In a lab environment requiring SSL interception, configure:

```bash
sslsplit
```

with the necessary certificates and routing.

---

# Final Answers (Lab Table)

| Tool         | Description                                                             | Recon, Exploitation, or Utility |
| ------------ | ----------------------------------------------------------------------- | ------------------------------- |
| **sslscan**  | Queries SSL services to determine supported protocols and cipher suites | **Reconnaissance**              |
| **ssldump**  | Analyzes and decodes SSL/TLS traffic                                    | **Exploitation**                |
| **sslh**     | Allows multiple services to run on TCP port 443                         | **Utility**                     |
| **sslsplit** | Enables authorized SSL/TLS interception (MITM) for testing              | **Exploitation**                |
| **sslyze**   | Performs comprehensive SSL/TLS configuration analysis of servers        | **Reconnaissance**              |

---


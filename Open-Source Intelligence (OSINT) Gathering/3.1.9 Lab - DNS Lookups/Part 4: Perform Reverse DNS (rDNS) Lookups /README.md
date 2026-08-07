# Part 4: Perform Reverse DNS (rDNS) Lookups 

This lab teaches you how to perform **Reverse DNS (rDNS) lookups**, which are commonly used during the **information gathering (OSINT)** phase of penetration testing.

Unlike normal DNS lookups (Hostname → IP), **Reverse DNS** converts an **IP address → Hostname**.

---

# Lab Objective

By the end of this lab you will learn:

* What Reverse DNS is
* How PTR records work
* How to perform rDNS using:

  * Dig
  * Host
  * Nslookup
* How attackers and defenders use Reverse DNS

---

# What is Reverse DNS?

Normally DNS works like this:

```
Hostname
    │
    ▼
cisco.com
    │
    ▼
72.163.4.185
```

Reverse DNS does the opposite.

```
72.163.4.185
      │
      ▼
cisco.com
```

Instead of asking

> "What is the IP of this hostname?"

You ask

> "What hostname belongs to this IP?"

---

# What record does Reverse DNS use?

Reverse DNS uses a special DNS record called

## PTR (Pointer Record)

Example

```
72.163.5.201

↓

PTR Record

↓

ns1.cisco.com
```

---

# Why do penetration testers perform Reverse DNS?

Suppose you discover this IP during reconnaissance.

```
72.163.5.201
```

Without Reverse DNS

You only know

```
IP Address
```

After Reverse DNS

```
72.163.5.201

↓

ns1.cisco.com
```

Now you know

* It's a DNS server
* It's Cisco's DNS
* It belongs to Cisco

This reveals valuable information without scanning the system.

---

# Step 1 — Open Kali Linux

Login

```
Username : kali
Password : kali
```

Open Terminal

```
┌──(kali㉿kali)-[~]
└─$
```

---

# Step 2 — Perform Reverse DNS using Dig

The syntax is

```bash
dig -x IP_Address
```

The **-x** option tells Dig

> Perform a Reverse DNS lookup.

---

## Example

Type

```bash
dig -x 72.163.5.201
```

Example output

```text
; <<>> DiG 9.20 <<>> -x 72.163.5.201

;; QUESTION SECTION:

;201.5.163.72.in-addr.arpa.

IN PTR

;; ANSWER SECTION:

201.5.163.72.in-addr.arpa.

PTR ns1.cisco.com.
```

---

# Understanding the Output

## QUESTION SECTION

```
201.5.163.72.in-addr.arpa
```

You might wonder

Why is the IP backwards?

Because Reverse DNS uses the

```
in-addr.arpa
```

domain.

Example

Original IP

```
72.163.5.201
```

becomes

```
201.5.163.72.in-addr.arpa
```

---

## ANSWER SECTION

```
PTR ns1.cisco.com
```

Meaning

```
72.163.5.201

belongs to

ns1.cisco.com
```

---

# Question 1

**What type of record is returned?**

Answer

> **PTR (Pointer Record)**

---

# Step 3 — Query another IP in the subnet

Now query

```bash
dig -x 72.163.1.1
```

Example output

```text
;; ANSWER SECTION

1.1.163.72.in-addr.arpa

PTR hsrp-72-163-1-1.cisco.com
```

---

# Understanding the hostname

```
hsrp-72-163-1-1
```

HSRP means

```
Hot Standby Router Protocol
```

It is Cisco's gateway redundancy protocol.

Usually

```
hsrp

↓

Gateway Router
```

---

# Question 2

**What type of device is probably assigned this IP?**

Answer

> It is probably the default gateway running Cisco HSRP.

---

# Why is this useful?

Imagine you discover

```
72.163.1.1
```

Without Reverse DNS

```
Unknown device
```

After Reverse DNS

```
hsrp-72-163-1-1.cisco.com
```

Now you know

* Gateway
* Router
* Cisco device

Very useful during reconnaissance.

---

# Step 4 — Perform Reverse DNS using Host

Linux has another DNS utility called

```
host
```

Its syntax is much simpler.

```
host IP
```

---

## Example

Type

```bash
host 72.163.10.1
```

Example output

```text
1.10.163.72.in-addr.arpa

domain name pointer

hsrp-72-163-10-1.cisco.com.
```

---

# Understanding the output

```
domain name pointer
```

means

```
PTR Record
```

The hostname is

```
hsrp-72-163-10-1.cisco.com
```

---

# Step 5 — Use Host for Forward Lookup

Host can also do

Hostname → IP

Type

```bash
host hsrp-72-163-10-1.cisco.com
```

Example

```text
hsrp-72-163-10-1.cisco.com

has address

72.163.10.1
```

Now you've verified

```
Hostname

↓

IP
```

---

# Comparing Host and Dig

### Dig

Shows

* Question
* Answer
* Authority
* Additional
* Query time
* DNS server
* Flags

---

Example

```
PTR Record

DNS Server

TTL

Flags

Authority

Statistics
```

Very detailed.

---

### Host

Shows

```
Only hostname
```

Example

```
72.163.10.1

↓

hsrp-72-163-10-1.cisco.com
```

Much simpler.

---

# Question 3

**How does Host differ from Dig or Nslookup?**

Answer

> Host provides only the hostname (or IP address) without showing DNS server information, query statistics, or other detailed DNS sections.

---

# Step 6 — Use Nslookup for Reverse DNS

Syntax

```bash
nslookup IP_Address
```

Example

```bash
nslookup 72.163.5.201
```

Example output

```text
Server:

192.168.1.1

Address:

192.168.1.1#53

Non-authoritative answer:

201.5.163.72.in-addr.arpa

name = ns1.cisco.com
```

---

# Understanding the Output

Server

```
192.168.1.1
```

Your DNS server.

---

Answer

```
ns1.cisco.com
```

Hostname assigned to the IP.

---

# Step 7 — Interactive Mode

Start

```bash
nslookup
```

Prompt changes

```
>
```

Type

```
72.163.5.201
```

Output

```
name = ns1.cisco.com
```

Exit

```
exit
```

---

# Comparison of Three Tools

| Feature           | Dig      | Host | Nslookup |
| ----------------- | -------- | ---- | -------- |
| Reverse lookup    | ✔        | ✔    | ✔        |
| Detailed output   | ✔        | ✘    | Medium   |
| Query time        | ✔        | ✘    | ✘        |
| DNS server info   | ✔        | ✘    | ✔        |
| Authority section | ✔        | ✘    | ✘        |
| Beginner friendly | Moderate | Easy | Easy     |

---

# Typical Workflow in a Penetration Test

Suppose you find an IP:

```
72.163.5.201
```

### Step 1

Reverse lookup

```bash
dig -x 72.163.5.201
```

↓

```
ns1.cisco.com
```

---

### Step 2

Verify with Host

```bash
host 72.163.5.201
```

↓

```
ns1.cisco.com
```

---

### Step 3

Verify with Nslookup

```bash
nslookup 72.163.5.201
```

↓

```
ns1.cisco.com
```

Now you know

* Device name
* Company
* Possible role (DNS server)

All without sending intrusive probes.

---

# Commands Practiced in This Lab

```bash
# Reverse DNS using Dig
dig -x 72.163.5.201

# Reverse DNS for another IP
dig -x 72.163.1.1

# Reverse DNS using Host
host 72.163.10.1

# Forward lookup using Host
host hsrp-72-163-10-1.cisco.com

# Reverse DNS using Nslookup
nslookup 72.163.5.201

# Interactive Nslookup
nslookup
72.163.5.201
exit
```

---

# Final Answers for the Lab

### **Q1. What type of record is returned with the host name?**

**Answer:** A **PTR (Pointer)** record is returned.

---

### **Q2. What type of device do you think is assigned the `72.163.1.1` address?**

**Answer:** Because the hostname is `hsrp-72-163-1-1.cisco.com`, it is most likely the **default gateway or a router participating in Cisco's Hot Standby Router Protocol (HSRP)**.

---

### **Q3. How does the output of the `host` command differ from `dig` or `nslookup`?**

**Answer:** The `host` command provides a concise result, typically showing only the hostname or IP address mapping. In contrast, `dig` and `nslookup` include additional details such as the DNS server used, query metadata, TTL values, and other DNS record information.



# Lab Objective

By the end of this lab you will learn how to use **nslookup** to:

* Resolve domain names to IP addresses
* Find DNS servers
* Find Mail (MX) servers
* Query different DNS record types
* Use another DNS server (Google DNS)
* Collect DNS information for passive reconnaissance

---

# What is nslookup?

**nslookup** stands for

> **Name Server Lookup**

It is a DNS query tool.

Instead of opening a browser, you ask the DNS server directly.

For example

```text
www.google.com

↓

DNS Server

↓

142.250.xx.xx
```

---

# What is DNS?

DNS stands for

> Domain Name System

Humans remember

```
google.com
```

Computers understand

```
142.250.183.206
```

DNS translates one into the other.

---

# Step 1 — Login to Kali Linux

Login

```
Username

kali

Password

kali
```

After login you'll see the Kali desktop.

---

# Step 2 — Open Terminal

Click

```
Applications

↓

Terminal
```

or

Press

```
Ctrl + Alt + T
```

The terminal opens.

Example

```text
┌──(kali㉿kali)-[~]
└─$
```

---

# Step 3 — Read the Manual

Type

```bash
man nslookup
```

Example

```text
NSLOOKUP(1)

NAME

nslookup

query Internet name servers
```

Navigate

| Key   | Function      |
| ----- | ------------- |
| Space | Next page     |
| b     | Previous page |
| q     | Quit          |

Quit

```
q
```

---

# Lab Question 1

### Which keyword queries Mail Server (MX) records?

Answer

```text
set type=mx
```

or

```text
set querytype=mx
```

Both work.

---

# Step 4 — Start Interactive Mode

Type

```bash
nslookup
```

Output

```text
┌──(kali㉿kali)-[~]

└─$ nslookup

>
```

Notice

The prompt changes from

```
$
```

to

```
>
```

You are now inside nslookup.

---

# Step 5 — Resolve a Domain

Type

```text
cisco.com
```

Example

```text
> cisco.com

Server:

192.168.1.1

Address:

192.168.1.1#53

Non-authoritative answer:

Name:

cisco.com

Address:

72.163.4.185

Name:

cisco.com

Address:

2001:420:1101:1::185
```

---

## What happened?

DNS returned

IPv4

```
72.163.4.185
```

IPv6

```
2001:420:1101:1::185
```

---

# Understanding the Output

```text
Server:

192.168.1.1
```

This is YOUR DNS server.

Usually your router.

---

```text
Non-authoritative answer
```

Means

Your DNS server cached the result.

It is NOT Cisco's own DNS server.

---

```text
Address:

72.163.4.185
```

This is the IPv4 address.

---

```text
Address:

2001:420:1101:1::185
```

IPv6 address.

---

# Step 6 — Query Name Servers

Inside nslookup

Type

```text
set type=ns
```

Now type

```text
cisco.com
```

Example

```text
> set type=ns

> cisco.com

cisco.com

nameserver = ns1.cisco.com

cisco.com

nameserver = ns2.cisco.com

cisco.com

nameserver = ns3.cisco.com
```

---

What is an NS record?

It tells us

> Which DNS servers are authoritative for this domain.

---

# Lab Question 2

### What are the IPv4 and IPv6 addresses of ns1?

Type

```text
ns1.cisco.com
```

Example

```text
Name:

ns1.cisco.com

Address:

72.163.5.201

Address:

2001:420:1101:6::a
```

Answer

IPv4

```
72.163.5.201
```

IPv6

```
2001:420:1101:6::a
```

---

# Exit Interactive Mode

Type

```text
exit
```

You return to

```text
$
```

---

# Step 7 — Use Another DNS Server

Instead of your router

```
192.168.1.1
```

Use

Google DNS

```
8.8.8.8
```

Command

```bash
nslookup netacad.com 8.8.8.8
```

Example

```text
Server:

8.8.8.8

Address:

8.8.8.8#53

Name:

netacad.com

Address:

13.xxx.xxx.xxx
```

---

Why use Google DNS?

Sometimes

Local DNS

↓

returns private IPs

or

↓

fails

Google DNS usually provides public Internet records.

---

# Step 8 — Interactive Mode with Google DNS

Start

```bash
nslookup
```

Set Google DNS

```text
server 8.8.8.8
```

Output

```text
Default server:

8.8.8.8

Address:

8.8.8.8#53
```

---

# Step 9 — Request ALL Records

Type

```text
set type=any
```

Then

```text
netacad.com
```

Example

```text
> set type=any

> netacad.com
```

Output

```text
Name:

netacad.com

Address:

13.225.142.127

Address:

13.225.142.73

Address:

13.225.142.9

Address:

13.225.142.7
```

---

Then

```text
Nameserver

ns-1130.awsdns-13.org

ns-1652.awsdns-14.co.uk

ns-489.awsdns-61.com
```

---

Then

```text
Mail exchanger

10 inbound-smtp.us-east-1.amazonaws.com
```

---

Then

```text
Text

facebook-domain-verification

google-site-verification
```

---

# What Does "ANY" Return?

Usually

| Record | Purpose            |
| ------ | ------------------ |
| A      | IPv4               |
| AAAA   | IPv6               |
| NS     | Name Servers       |
| MX     | Mail Servers       |
| TXT    | Text Records       |
| SOA    | Start of Authority |

Some DNS servers no longer fully support `ANY`, so you may need to query individual record types.

---

# Understanding DNS Records

## A Record

Maps

```
Domain

↓

IPv4
```

Example

```
google.com

↓

142.250.183.206
```

---

## AAAA Record

Maps

```
Domain

↓

IPv6
```

---

## NS Record

Shows

Authoritative DNS Servers

Example

```
ns1.google.com
```

---

## MX Record

Shows

Mail Server

Example

```
gmail-smtp-in.l.google.com
```

---

## TXT Record

Contains

Verification

SPF

DKIM

Cloudflare

Google

Microsoft

Facebook

---

## SOA Record

Contains

Primary DNS server

Admin email

Serial number

Refresh timer

Retry timer

Expire timer

---

# Lab Question 3

### What records are returned by ANY?

Answer

```
A

AAAA

NS

MX

TXT

SOA

(and any other record types the DNS server permits)
```

The official lab answer is:

> **All permitted record types, including A, AAAA, NS, MX, and TXT.**

---

# Useful nslookup Commands

| Command          | Purpose                                    |
| ---------------- | ------------------------------------------ |
| `nslookup`       | Interactive mode                           |
| `exit`           | Quit                                       |
| `set type=mx`    | Mail servers                               |
| `set type=ns`    | Name servers                               |
| `set type=txt`   | TXT records                                |
| `set type=soa`   | SOA record                                 |
| `set type=any`   | Query multiple record types (if supported) |
| `server 8.8.8.8` | Change DNS server                          |
| `cisco.com`      | Query domain                               |

---

# Complete Workflow

```text
Open Terminal
       │
       ▼
Run nslookup
       │
       ▼
Query cisco.com
       │
       ▼
Receive IPv4 & IPv6
       │
       ▼
set type=ns
       │
       ▼
Find Name Servers
       │
       ▼
Lookup ns1.cisco.com
       │
       ▼
Get IPv4 & IPv6
       │
       ▼
exit
       │
       ▼
nslookup netacad.com 8.8.8.8
       │
       ▼
Use Google DNS
       │
       ▼
set type=any
       │
       ▼
View A, AAAA, NS, MX, TXT, SOA
```

---

# Final Answers (Lab)

| Question                                         | Answer                                                                             |
| ------------------------------------------------ | ---------------------------------------------------------------------------------- |
| Which keyword queries Mail Server records?       | `set type=mx` or `set querytype=mx`                                                |
| IPv4 address of `ns1.cisco.com` (course example) | `72.163.5.201`                                                                     |
| IPv6 address of `ns1.cisco.com` (course example) | `2001:420:1101:6::a`                                                               |
| What does `set type=any` display?                | All permitted record types, including **A, AAAA, NS, MX, TXT**, and often **SOA**. |


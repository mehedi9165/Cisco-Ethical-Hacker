
# Lab Objective

In this lab you will learn:

* What **dig** is
* How it differs from **nslookup**
* How to query different DNS record types
* How to use another DNS server (Google DNS)
* How to interpret the output

---

# What is Dig?

**Dig (Domain Information Groper)** is one of the most popular DNS lookup tools used by:

* Penetration Testers
* Network Engineers
* SOC Analysts
* Blue Team
* System Administrators

Unlike **nslookup**, Dig provides much more detailed DNS information.

---

# Step 1 — Open Kali Linux

Login

```
Username: kali
Password: kali
```

Open Terminal

You should see

```bash
kali@kali:~$
```

or

```bash
┌──(kali㉿kali)-[~]
└─$
```

---

# Step 2 — Check whether Dig is installed

Type

```bash
dig -v
```

Example

```
DiG 9.20.5
```

If installed, continue.

If not,

```bash
sudo apt update
sudo apt install dnsutils
```

---

# Step 3 — Query the A Record

Type

```bash
dig cisco.com
```

Example output

```text
; <<>> DiG 9.20.5 <<>> cisco.com
;;

;; QUESTION SECTION:
;cisco.com.      IN   A

;; ANSWER SECTION:
cisco.com. 300 IN A 72.163.4.185

;; Query time: 35 msec
;; SERVER: 192.168.1.1
;; WHEN: Tue Aug 11
```

---

## Understanding the Output

### QUESTION SECTION

```
;cisco.com IN A
```

Meaning

> Dig is asking for the IPv4 address.

---

### ANSWER SECTION

```
72.163.4.185
```

This is Cisco's IPv4 address.

---

### Query Time

```
35 msec
```

How long DNS took to answer.

---

### SERVER

```
192.168.1.1
```

DNS server used.

Usually your router.

---

### WHEN

Time of lookup.

---

# Question 1

**What is the difference between Dig and Nslookup?**

Answer

> Dig queries only the **A record** by default, whereas **Nslookup queries both A and AAAA records**.

---

# Step 4 — Query IPv6 Address

Dig only asked for IPv4.

Now ask specifically for IPv6.

Type

```bash
dig cisco.com AAAA
```

Example

```text
;; ANSWER SECTION

cisco.com 300 IN AAAA 2001:420:1101:1::185
```

Now you have

```
IPv6 Address
```

---

# Common DNS Record Types

| Record | Purpose        |
| ------ | -------------- |
| A      | IPv4           |
| AAAA   | IPv6           |
| MX     | Mail server    |
| NS     | DNS server     |
| TXT    | Verification   |
| SOA    | Authority      |
| CNAME  | Alias          |
| PTR    | Reverse lookup |

---

# Step 5 — Query Name Servers

Now we want DNS servers.

Command

```bash
dig cisco.com NS
```

Example

```text
;; ANSWER SECTION

cisco.com. IN NS ns1.cisco.com.
cisco.com. IN NS ns2.cisco.com.
cisco.com. IN NS ns3.cisco.com.
```

Meaning

Cisco uses

```
ns1.cisco.com
ns2.cisco.com
ns3.cisco.com
```

---

# Step 6 — Use Google's DNS Server

Instead of your ISP DNS,

use

```
8.8.8.8
```

Command

```bash
dig @8.8.8.8 cisco.com NS
```

Notice

The syntax is

```bash
dig @DNS_Server Domain RecordType
```

Example

```bash
dig @8.8.8.8 cisco.com NS
```

Output

```text
;; ANSWER SECTION

cisco.com. IN NS ns1.cisco.com.
cisco.com. IN NS ns2.cisco.com.
cisco.com. IN NS ns3.cisco.com.
```

---

## Why use another DNS server?

Sometimes

Local DNS

↓

returns

```
Internal IP
```

Instead,

Google DNS returns

```
Public IP
```

Useful during reconnaissance.

---

# Step 7 — Query ANY Records

Now ask for everything.

Command

```bash
dig netacad.com ANY
```

Depending on the DNS server, you may receive

```
A
AAAA
MX
TXT
NS
SOA
```

Some DNS servers block `ANY` queries, so if you receive a limited response or a refusal, that's normal.

Example output

```text
;; ANSWER SECTION

netacad.com.

A
13.225.xxx.xxx

NS
ns-1130.awsdns-13.org.

MX
inbound-smtp.us-east-1.amazonaws.com.

TXT
google-site-verification=...

TXT
facebook-domain-verification=...
```

---

# Understanding Each Record

## A

```
13.225.142.127
```

IPv4 address.

---

## MX

```
inbound-smtp.us-east-1.amazonaws.com
```

Mail server.

---

## NS

```
ns-1130.awsdns-13.org
```

DNS server.

---

## TXT

Example

```
google-site-verification
```

Used for Google Search Console verification.

---

## TXT

```
facebook-domain-verification
```

Used by Facebook.

---

# Step 8 — Compare Dig and Nslookup

Suppose you ran

Nslookup

```bash
nslookup
```

Then

```
server 8.8.8.8

set type=any

netacad.com
```

Output

```
A

NS

TXT

MX
```

Now compare with

```bash
dig netacad.com ANY
```

Dig groups records neatly into sections, making it easier to identify each record type.

---

# Lab Question

**Which output is easier to read?**

**Answer:**

> The **dig** output is easier to read because it organizes DNS records into clear sections and groups them by record type.

---

# Useful Dig Commands

## IPv4

```bash
dig google.com
```

---

## IPv6

```bash
dig google.com AAAA
```

---

## Mail Servers

```bash
dig google.com MX
```

---

## Name Servers

```bash
dig google.com NS
```

---

## TXT Records

```bash
dig google.com TXT
```

---

## SOA Record

```bash
dig google.com SOA
```

---

## Reverse Lookup

```bash
dig -x 8.8.8.8
```

---

## Use Google DNS

```bash
dig @8.8.8.8 google.com
```

---

## Use Cloudflare DNS

```bash
dig @1.1.1.1 google.com
```

---

## Short Output Only

```bash
dig +short google.com
```

Example

```
142.250.183.206
```

---

# Comparison: `nslookup` vs `dig`

| Feature                    | nslookup    | dig         |
| -------------------------- | ----------- | ----------- |
| Default query              | A + AAAA    | A only      |
| Detailed output            | Basic       | Extensive   |
| DNS troubleshooting        | Limited     | Excellent   |
| Shows query time           | Limited     | Yes         |
| Displays flags and headers | No          | Yes         |
| Preferred by professionals | Less common | Widely used |

---

# Commands Practiced in This Lab

```bash
dig -v
dig cisco.com
dig cisco.com AAAA
dig cisco.com NS
dig @8.8.8.8 cisco.com NS
dig netacad.com ANY
dig google.com MX
dig google.com TXT
dig google.com SOA
dig -x 8.8.8.8
dig +short google.com
```

---

## Final Answers for the Lab

**Q1. What is the difference between the default record types queried by Dig and Nslookup?**

**Answer:** Dig queries only the **A** record by default, while **nslookup** queries both **A** and **AAAA** records.

**Q2. Which output is easier to read to obtain the values contained in the various record types?**

**Answer:** The **dig** output is easier to read because it presents DNS records in a structured, grouped format with clearly labeled sections (QUESTION, ANSWER, AUTHORITY, ADDITIONAL), making each record type easier to identify.

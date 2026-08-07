

# Lab Objective

Learn how to use the **whois** command to gather public information about:

* Domain ownership
* Registrar information
* Registration dates
* Name servers
* Contact information
* IP address ownership
* IP address ranges

Unlike **nslookup**, which queries **DNS records**, **whois** queries the **domain registration database (WHOIS/RDAP)**.

---

# What is Whois?

Think of Whois like a vehicle registration database.

For example,

Car Plate:

```
Dhaka Metro Ga-12-3456
```

Vehicle Database tells you

* Owner
* Registration date
* Address
* Registration authority

Similarly,

```
cisco.com
```

Whois tells you

* Who registered it
* Which company owns it
* Registrar
* Registration dates
* Name servers
* Contact information

It **does not hack anything**.

It simply asks the public registration database.

---

# Step 1 — Open Kali Linux

Login

```
Username:
kali

Password:
kali
```

Open Terminal

You should see

```
┌──(kali㉿kali)-[~]
└─$
```

---

# Step 2 — Check whether Whois is installed

Type

```bash
whois
```

If installed

You will see

```
Usage:

whois [OPTION] OBJECT
```

If not installed

```bash
sudo apt update

sudo apt install whois
```

Verify

```bash
whois --version
```

---

# Step 3 — Whois Cisco Domain

Type

```bash
whois cisco.com
```

Wait a few seconds.

You'll receive many lines.

Example

```
Domain Name: CISCO.COM

Registrar:
MarkMonitor Inc.

Creation Date:
1987-05-14

Registry Expiry Date:
2027-05-15

Name Server:
NS1.CISCO.COM

Name Server:
NS2.CISCO.COM

Registrant Organization:
Cisco Systems, Inc.
```

---

# Understanding Every Section

---

## Domain Name

```
Domain Name:
CISCO.COM
```

Meaning

The registered domain.

---

## Registrar

```
Registrar:
MarkMonitor Inc.
```

Meaning

The company where Cisco purchased the domain.

Examples

* GoDaddy
* Namecheap
* Cloudflare
* MarkMonitor

---

## Creation Date

```
Creation Date:
1987-05-14
```

Meaning

Cisco registered this domain in 1987.

---

## Expiry Date

```
Registry Expiry Date:
2027-05-15
```

Meaning

The registration expires on that date.

---

## Name Servers

Example

```
NS1.CISCO.COM

NS2.CISCO.COM

NS3.CISCO.COM
```

These answer DNS queries.

---

## Registrant

Example

```
Registrant Organization

Cisco Systems Inc.
```

Shows the company owner.

Sometimes hidden using privacy protection.

---

# Step 4 — Whois Netacad

Now type

```bash
whois netacad.com
```

Example

```
Registrant:

Cisco Systems Inc.

Registrar:

MarkMonitor

Name Servers

AWS DNS Servers
```

---

## Compare

Cisco.com

Owned by Cisco

↓

Netacad.com

Owned by Cisco

↓

Hosted on AWS

Therefore,

**Answer**

> Both domains belong to Cisco. NetAcad is one of Cisco's services and is hosted on cloud infrastructure.

This matches the lab answer.

---

# Step 5 — Find DNS Server IP Address

Now we need Cisco's DNS server IP.

Open nslookup

```bash
nslookup
```

Type

```
set type=ns

cisco.com
```

Output

```
ns1.cisco.com

ns2.cisco.com

ns3.cisco.com
```

Now resolve one.

```
ns1.cisco.com
```

Example

```
72.163.5.201
```

Exit

```
exit
```

---

# Step 6 — Whois an IP Address

Now query

```bash
whois 72.163.5.201
```

Example output

```
NetRange:

72.163.0.0 - 72.163.255.255

CIDR:

72.163.0.0/16

Organization:

Cisco Systems Inc.

OriginAS:

AS109
```

---

# Understanding the Output

---

## NetRange

```
72.163.0.0

↓

72.163.255.255
```

Meaning

Cisco owns every address between these two.

---

## CIDR

```
72.163.0.0/16
```

CIDR notation for the same network.

---

## Organization

```
Cisco Systems Inc.
```

Owner.

---

## ASN

```
AS109
```

Cisco's Autonomous System Number.

Large companies announce their IP blocks using AS numbers.

---

# Step 7 — Answer the Question

Question

What is the IPv4 range?

Answer

```
72.163.0.0 – 72.163.255.255

or

72.163.0.0/16
```

---

# Why Is This Useful?

Imagine you're performing an **authorized** penetration test for Cisco.

You discover

```
72.163.5.201
```

Running Whois reveals

```
Cisco owns

72.163.0.0/16
```

This tells you the organization's public IP allocation, which helps define the scope of infrastructure to inventory or assess **with permission**. It does **not** mean every IP should be scanned without authorization.

---

# Example Using Another Company

Run

```bash
whois microsoft.com
```

You may see

```
Registrant

Microsoft Corporation

Registrar

MarkMonitor

Creation Date

1991

Name Servers

ns1-205.azure-dns.com
```

---

Run

```bash
nslookup microsoft.com
```

Suppose

```
20.70.246.20
```

Then

```bash
whois 20.70.246.20
```

Example

```
Organization

Microsoft Corporation

NetRange

20.64.0.0/10
```

---

# Difference Between Whois and Nslookup

| Feature                  | nslookup | whois |
| ------------------------ | -------- | ----- |
| Uses DNS                 | ✅        | ❌     |
| Resolves domain to IP    | ✅        | ❌     |
| Shows MX records         | ✅        | ❌     |
| Shows NS records         | ✅        | ❌     |
| Shows TXT records        | ✅        | ❌     |
| Shows owner              | ❌        | ✅     |
| Shows registrar          | ❌        | ✅     |
| Shows registration dates | ❌        | ✅     |
| Shows IP allocation      | ❌        | ✅     |

---

# Commands Used in This Lab

```bash
# Query domain registration
whois cisco.com

# Query another domain
whois netacad.com

# Resolve DNS records
nslookup

# Find Cisco name servers
set type=ns
cisco.com

# Resolve ns1
ns1.cisco.com

# Exit nslookup
exit

# Query IP registration
whois 72.163.5.201
```

---

# Final Answers

**Q1. What conclusion can you make about `cisco.com` and `netacad.com`?**

**Answer:**

> Both domains are owned by Cisco. `netacad.com` is Cisco's Networking Academy domain and is hosted on cloud infrastructure.

**Q2. What is the IPv4 address range allocated to Cisco?**

**Answer:**

> `72.163.0.0 – 72.163.255.255` (CIDR: `72.163.0.0/16`)


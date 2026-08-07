# Lab: Access Detailed Certificate Information Online (Certificate Transparency Logs)



# What is Certificate Transparency (CT)?

Every time a trusted Certificate Authority (CA) issues an SSL/TLS certificate, the certificate is recorded in a **public Certificate Transparency log**.

Think of it like a **public registry**.

Example:

```
Website requests SSL certificate
           │
           ▼
Certificate Authority (Let's Encrypt, DigiCert, etc.)
           │
           ▼
Certificate issued
           │
           ▼
Automatically recorded in CT Logs
           │
           ▼
Anyone can search it
```

Because these logs are public, security researchers and ethical hackers can discover:

* Domains
* Subdomains
* Certificate history
* Certificate issuer
* Expiration dates
* Newly created services

without actively scanning the target.

---

# Why is CT Useful in OSINT?

Suppose a company owns

```
company.com
```

You only know this domain.

Searching CT logs might reveal

```
vpn.company.com

mail.company.com

dev.company.com

test.company.com

staging.company.com

api.company.com

portal.company.com

admin.company.com
```

Some of these subdomains may never appear on Google but are still listed in CT logs because certificates were issued for them.

---

# What is crt.sh?

**crt.sh** is a free website that searches Certificate Transparency logs.

It collects certificates from multiple public CT log servers.

Website:

```
https://crt.sh 
```
or 
```
https://crt.sh/?trk=article-ssr-frontend-pulse_little-text-block
```
or

```
[https://crt.sh/?trk=article-ssr-frontend-pulse_little-text-block](https://www.linkedin.com/pulse/crtsh-how-discover-ssltls-certificates-subdomains-using-alex-krasov-vuj5c)
```

No login is required.

---

# Step 1: Open crt.sh

Open Firefox (or any browser).

Go to

```
https://crt.sh
```

You will see something similar to:

```
---------------------------------------
Certificate Search

Identity:

_________________________

        [ Search ]
---------------------------------------
```

---

# Step 2: Search the Skills for All Domain

The lab refers to **Skills for All**.

Historically, Cisco Skills for All has been associated with:

```
netacad.com
```

Type

```
netacad.com
```

Click

```
Search
```

---

# Step 3: Observe the Results

After a few seconds, you will see a large table.

Example

| ID        | Logged At | Common Name       | Matching Identities |
| --------- | --------- | ----------------- | ------------------- |
| 123456789 | 2026      | netacad.com       | netacad.com         |
| 123456790 | 2026      | *.netacad.com     | *.netacad.com       |
| 123456791 | 2025      | dev.netacad.com   | dev.netacad.com     |
| 123456792 | 2025      | stage.netacad.com | stage.netacad.com   |

There may be hundreds of certificates.

---

# What does each column mean?

### ID

Unique crt.sh certificate identifier.

Example

```
214598734
```

Clicking it opens detailed certificate information.

---

### Logged At

When the certificate appeared in the CT log.

Example

```
2025-04-10
```

---

### Common Name (CN)

The main domain the certificate protects.

Example

```
netacad.com
```

---

### Matching Identities

Lists every domain covered by the certificate.

Example

```
netacad.com

www.netacad.com

api.netacad.com
```

---

# Step 4: Click a Certificate ID

Click one of the IDs.

You will see detailed certificate information.

Example

```
Certificate:

Issued To

*.netacad.com

Issuer

DigiCert

Valid From

2026

Valid Until

2027

SHA256 Fingerprint

Public Key

SAN Entries
```

---

# Step 5: Look for Subdomains

Scroll down until you find

```
X509v3 Subject Alternative Name
```

Example

```
DNS:netacad.com

DNS:www.netacad.com

DNS:stage.netacad.com

DNS:dev.netacad.com

DNS:api.netacad.com
```

These are all subdomains covered by the certificate.

---

# What are Subdomains?

If

```
example.com
```

is the main domain,

then

```
mail.example.com

vpn.example.com

api.example.com

dev.example.com
```

are subdomains.

---

# Example

Suppose you search

```
example.com
```

CT logs reveal

```
vpn.example.com

mail.example.com

portal.example.com

stage.example.com

dev.example.com

admin.example.com
```

This tells an ethical hacker that the organization has multiple services.

---

# Step 6: Identify Development Servers

The lab asks:

> Note the names of the subdomains.

You may see names like

```
dev

stage

staging

uat

test
```

These names have common meanings.

| Subdomain | Purpose                 |
| --------- | ----------------------- |
| dev       | Development server      |
| stage     | Staging environment     |
| test      | Testing                 |
| qa        | Quality Assurance       |
| uat       | User Acceptance Testing |
| beta      | Beta version            |

These environments are typically intended for internal development and testing rather than public users.

---

# Lab Question 1

**Who do you think these subdomains are intended to be used by?**

### Answer

These subdomains are likely intended for **developers, testers, quality assurance (QA) teams, and system administrators** who build, test, and maintain the Skills for All platform before changes are released to the public.

---

# Step 7: Search the Related Domain

The lab asks:

> What other domain is associated with Skills for All?

Search

```
socialgoodplatform.com
```

in crt.sh.

---

You may now see

```
portal.socialgoodplatform.com

login.socialgoodplatform.com

api.socialgoodplatform.com

cdn.socialgoodplatform.com

stage.socialgoodplatform.com

dev.socialgoodplatform.com
```

Many more certificates may appear.

---

# Why does this happen?

Skills for All is part of a larger platform.

The SSL certificates reveal infrastructure belonging to

```
socialgoodplatform.com
```

---

# Lab Question 2

**What other domain is associated with Skills for All?**

### Answer

```
socialgoodplatform.com
```

---

# Step 8: Compare the Number of Subdomains

Notice

```
netacad.com
```

might have

```
20

30

40
```

subdomains.

But

```
socialgoodplatform.com
```

may have significantly more.

Example

```
api.socialgoodplatform.com

cdn.socialgoodplatform.com

portal.socialgoodplatform.com

mail.socialgoodplatform.com

stage.socialgoodplatform.com

dev.socialgoodplatform.com

files.socialgoodplatform.com

login.socialgoodplatform.com

auth.socialgoodplatform.com
```

---

# Why is this Important?

More subdomains generally indicate a larger infrastructure.

Each service represents another internet-facing system that must be secured.

---

# Attack Surface

Attack surface means

> Every place that could potentially be attacked.

Example

Small company

```
company.com
```

Attack surface

```
Website only
```

Large company

```
company.com

vpn.company.com

mail.company.com

api.company.com

portal.company.com

cdn.company.com

git.company.com

stage.company.com
```

Much larger attack surface.

---

# Lab Question 3

**What general observation can you make about the domains revealed?**

### Answer

The CT logs reveal many subdomains under **socialgoodplatform.com**, indicating that the platform supports multiple services and environments. This suggests a relatively large and complex infrastructure.

**What does this imply about the network?**

It implies that the organization has a **larger attack surface**, meaning there are more internet-facing systems and services that require proper security management. The presence of development and staging subdomains also indicates separate environments used during the software development lifecycle.

---

# Additional Examples

## Example 1

Search

```
microsoft.com
```

Possible findings

```
login.microsoft.com

api.microsoft.com

portal.microsoft.com

support.microsoft.com
```

---

## Example 2

Search

```
tesla.com
```

Possible findings

```
shop.tesla.com

auth.tesla.com

developer.tesla.com
```

---

## Example 3

Search

```
openai.com
```

Possible findings

```
chat.openai.com

platform.openai.com

auth.openai.com
```

The exact results will change over time as certificates are issued or renewed.

---

# Why Do Ethical Hackers Use CT Logs?

CT logs help them:

* Discover hidden subdomains
* Identify development and staging environments
* Understand an organization's infrastructure
* Track certificate issuance history
* Monitor for unauthorized or unexpected certificates

Since CT logs are public, this is considered **passive reconnaissance** and does not involve interacting with the target's systems.

---

# Complete Lab Answers

### Q1. Who are the `dev` and `stage` subdomains intended for?

**Answer:** They are typically intended for **developers, QA engineers, testers, and administrators** to develop, test, and validate applications before production deployment.

---

### Q2. What other domain is associated with Skills for All?

**Answer:** `socialgoodplatform.com`

---

### Q3. What observation can you make about the domains revealed?

**Answer:** The search reveals many subdomains under `socialgoodplatform.com`, showing that the organization operates multiple services and environments.

---

### Q4. What does this imply about the network?

**Answer:** It implies the organization has a **large and diverse network infrastructure** with numerous internet-facing services. This creates a broader attack surface that requires careful security monitoring and management.

---

## Key Takeaways

* **Certificate Transparency (CT)** logs publicly record all trusted SSL/TLS certificates.
* **crt.sh** is a free tool for searching CT logs.
* CT logs can reveal **domains, subdomains, certificate history, issuers, and validity periods**.
* Discovering subdomains helps analysts understand an organization's infrastructure and potential exposure **without actively scanning the target**. This makes CT logs a valuable source of passive OSINT.

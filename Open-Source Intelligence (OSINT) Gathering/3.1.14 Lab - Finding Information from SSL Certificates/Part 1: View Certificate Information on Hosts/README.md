## Part 1: View Certificate Information on Hosts 

This lab teaches you how to inspect **SSL/TLS certificates** on websites and in your operating system. This is an important **OSINT (Open-Source Intelligence)** and **defensive security** skill because certificates reveal:

* Domain owner
* Certificate Authority (CA)
* Encryption algorithm
* Validity period
* Organization details
* Public key information

Ethical hackers use certificate information during **passive reconnaissance**.

---

# What is an SSL Certificate?

When you visit a website using HTTPS, your browser receives a digital certificate.

Example:

```
https://www.google.com
```

The lock icon (🔒) means:

* Connection is encrypted
* Identity of the website is verified
* Data cannot easily be intercepted

---

# Lab Overview

You will learn

✅ View website certificates

✅ View locally stored certificates

✅ Understand Certificate Authorities

✅ Find certificate expiration date

✅ Identify encryption algorithms

✅ Research SSL certificate pricing

---

# Step 1: Open a Website

Open Firefox or Chrome.

Visit

```
https://netacad.com
```

You should see

```
🔒 https://netacad.com
```

Example:

```
 --------------------------------------
| 🔒 https://netacad.com              |
---------------------------------------
```

---

# Step 2: Click the Lock Icon

Click

```
🔒
```

Most browsers display

```
Connection is Secure

Certificate

Cookies

Permissions
```

Click

```
Certificate
```

or

```
Connection is Secure
↓

Certificate is valid
↓

View Certificate
```

---

## Example

You may see

```
Issued To

socialgoodplatform.com
```

or another domain if the site has changed since the lab was written.

---

# Step 3: Open Certificate Details

The certificate window contains several tabs.

Example

```
General

Details

Certification Path
```

Click

```
Details
```

---

You will now see information similar to

```
Version

Serial Number

Issuer

Subject

Public Key

Signature Algorithm

Validity

Thumbprint
```

---

# Step 4: Find the Domain Name

Look for

```
Subject

CN=
```

Example

```
CN=socialgoodplatform.com
```

Answer

> The certificate was issued to **socialgoodplatform.com** (or the current domain displayed).

---

# Step 5: Find the Certificate Authority (Issuer)

Look for

```
Issuer
```

Example

```
Issuer

IdenTrust
```

or

```
Let's Encrypt
```

or

```
DigiCert
```

or

```
GlobalSign
```

Answer

```
Issued by IdenTrust
```

---

# Step 6: Find Expiration Date

Look for

```
Validity

Not Before

Not After
```

Example

```
Not Before

July 10 2026

Not After

January 20 2027
```

Answer

```
Certificate expires on January 20, 2027.
```

The exact date depends on when you perform the lab.

---

# Step 7: Find Signature Algorithm

Look for

```
Signature Algorithm
```

Example

```
SHA256 with RSA Encryption
```

or

```
ECDSA with SHA384
```

Answer

```
SHA-256 with RSA Encryption
```

---

# Step 8: Find Public Key Size

Example

```
RSA

2048 bits
```

or

```
ECDSA

384 bits
```

Larger keys generally provide stronger cryptographic security, though algorithm choice also matters.

---

# Example Certificate Summary

| Field               | Example                |
| ------------------- | ---------------------- |
| Subject             | socialgoodplatform.com |
| Issuer              | IdenTrust              |
| Signature Algorithm | SHA256 with RSA        |
| Public Key          | RSA 2048               |
| Expires             | January 20, 2027       |

---

# Step 9: View Certificates Stored in Kali Linux

Open Terminal.

Navigate to Mozilla certificate directory.

```bash
cd /usr/share/ca-certificates/mozilla
```

List certificates

```bash
ls
```

Example

```
Amazon_Root_CA_1.crt

GlobalSign_Root_CA.crt

DigiCert_Global_Root_CA.crt

ISRG_Root_X1.crt
```

---

## Search for Root Certificates

```bash
ls | grep Root
```

or

```bash
ls | grep root
```

Example

```
GlobalSign_Root_CA.crt

DigiCert_Global_Root_CA.crt

Entrust_Root_CA.crt
```

---

# Step 10: Open a Certificate

Double-click

```
GlobalSign_Root_CA.crt
```

or use

```bash
openssl x509 -in GlobalSign_Root_CA.crt -text -noout
```

You'll see details like

```
Issuer

Subject

Validity

Public Key

Extensions
```

---

# Three Common Certificate Authorities

Typical trusted root CAs you may find include:

* **GlobalSign**
* **DigiCert**
* **GoDaddy**

Other common CAs include:

* Let's Encrypt (ISRG Root X1)
* Sectigo
* Entrust
* Amazon Trust Services
* Microsoft

---

# Research the Certificate Authorities

### 1. GlobalSign

* Founded: 1996
* Headquarters: Belgium
* Provides SSL/TLS, Code Signing, S/MIME, PKI

---

### 2. DigiCert

* Founded: 2003
* Headquarters: USA
* One of the world's largest certificate providers
* Acquired Symantec's certificate business

---

### 3. GoDaddy

* Domain registrar
* Also sells SSL certificates
* Popular among small businesses

---

# How to Find SSL Certificate Prices

Visit the official vendor websites.

## GlobalSign

[GlobalSign SSL Certificates](https://shop.globalsign.com/en/ssl?utm_source=chatgpt.com)

Browse to:

```
SSL Certificates
```

Choose:

```
Domain Validation (DV)
```

You'll find the current pricing and plans. ([GlobalSign][1])

---

## DigiCert

[DigiCert TLS/SSL Certificates](https://www.digicert.com/tls-ssl/basic-tls-ssl-certificates?utm_source=chatgpt.com)

The site lists current subscription pricing for standard, wildcard, and business SSL certificates. ([digicert.com][2])

---

## GoDaddy

[GoDaddy Domain Validation SSL](https://www.godaddy.com/en/web-security/domain-validation-ssl-certificate?utm_source=chatgpt.com)

The product page shows current pricing, promotions, and included features for DV SSL certificates. ([GoDaddy][3])

---

# Example Price Comparison

| Certificate Authority | Product               | Typical Current Pricing*                                                      |
| --------------------- | --------------------- | ----------------------------------------------------------------------------- |
| GlobalSign            | Domain Validated (DV) | See official pricing page                                                     |
| DigiCert              | Basic TLS/SSL         | Starts around **US$24–26/month** on annual subscription plans                 |
| GoDaddy               | DV SSL                | Promotional pricing often starts around **US$5.99/month** on multi-year terms |

*Prices change frequently and promotions vary, so always verify on the vendor's official website. ([GlobalSign][1])

---

# Why Is This Useful During OSINT?

An ethical hacker can learn:

* Website owner
* Certificate Authority
* Encryption algorithm
* Validity period
* Public key type
* Whether the certificate is expired
* Organization information (for OV/EV certificates)

This information helps build a profile of the target's infrastructure without interacting with it in an intrusive way.

---

# Example Lab Answers

**Q1. What domain was the certificate issued to?**

**Answer:** The domain shown in the certificate's **Subject/Common Name (CN)** field (for example, `socialgoodplatform.com` if following the original lab, or whatever the site currently uses).

**Q2. Who issued the certificate?**

**Answer:** The Certificate Authority listed in the **Issuer** field (for example, IdenTrust, DigiCert, Let's Encrypt, or another CA).

**Q3. When will it expire?**

**Answer:** The date shown in the **Not After** field.

**Q4. What is the signature algorithm?**

**Answer:** The value shown in the **Signature Algorithm** field (for example, **SHA-256 with RSA Encryption**).

**Q5. Name three common Certificate Authorities.**

**Answer:**

* GlobalSign
* DigiCert
* GoDaddy

**Q6. How do you find the SSL certificate price?**

**Answer:** Visit the vendor's official website (such as GlobalSign, DigiCert, or GoDaddy), navigate to its SSL/TLS certificate section, choose the desired certificate type (DV, OV, or EV), and review the published pricing and plan details.

[1]: https://shop.globalsign.com/en/ssl?utm_source=chatgpt.com "SSL / TLS CERTIFICATES"
[2]: https://www.digicert.com/tls-ssl/basic-tls-ssl-certificates?utm_source=chatgpt.com "Buy Basic TLS/SSL Certificates | DigiCert"
[3]: https://www.godaddy.com/en/web-security/domain-validation-ssl-certificate?utm_source=chatgpt.com "Domain Validation SSL Certificate | Buy A DV SSL Cert Today - GoDaddy IE"

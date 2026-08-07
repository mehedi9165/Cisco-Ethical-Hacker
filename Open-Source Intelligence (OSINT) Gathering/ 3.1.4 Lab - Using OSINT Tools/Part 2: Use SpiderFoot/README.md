# Part 1 (Step 1: Start and run SpiderFoot + Step 2: Explore SpiderFoot)
# Lab Objective

In this lab you will learn:

* Install and start SpiderFoot
* Explore SpiderFoot modules
* Search modules from the terminal
* Understand API requirements
* Complete the lab table
* Run a Footprint scan
* Analyze the results

---

# What is SpiderFoot?

SpiderFoot is an automated **OSINT (Open Source Intelligence)** tool.

Instead of manually searching Google, GitHub, Shodan, DNS records, breach databases, social media, etc., SpiderFoot queries many public sources and combines the findings into one report.

Latest versions include hundreds of modules (the exact number changes over time).

It accepts targets such as:

* Domain
* IP address
* ASN
* Email
* Phone number
* Person's name

---

# Step 1 — Start SpiderFoot

Open Kali Terminal.

Run

```bash
spiderfoot -l 127.0.0.1:5001
```

Example

```
SpiderFoot 4.x

Starting web server...

Listening on:

http://127.0.0.1:5001
```

Do **NOT** close this terminal.

---

# Step 2 — Open the GUI

Open Firefox.

Visit

```
http://127.0.0.1:5001
```

You should see

```
SpiderFoot

Scans

New Scan

Settings

Logs
```

If this is your first time,

Scans page is empty.

---

# Step 3 — Explore Settings

Click

```
Settings
```

Left side contains

```
SpiderFoot

General

Modules

Account Finder

AbuseIPDB

AlienVault

Archive.org

Bing

BuiltWith

Censys

DNS

EmailCrawlr

GitHub

Hunter

Leak Lookup

Shodan

VirusTotal

...
```

Every entry is a SpiderFoot module.

---

# Step 4 — Understand Module Names

Open one module.

Example

```
Account Finder
```

You will see

```
Module

sfp_accounts
```

Every SpiderFoot module begins with

```
sfp_
```

Examples

```
sfp_accounts

sfp_emailcrawlr

sfp_crossref

sfp_dns

sfp_ipapicom

sfp_shodan

sfp_hunter
```

---

# Step 5 — Which Modules Need APIs?

Some modules show

🔒 Lock icon

Example

```
Hunter.io
```

Click it.

You may see

```
API Key

Required
```

Click

```
?
```

SpiderFoot tells you

* Website
* Registration process
* Free plan available?
* Paid?

---

# Step 6 — Explore Modules from Terminal

SpiderFoot also works in terminal.

Show help

```bash
spiderfoot -h
```

Show modules

```bash
spiderfoot -M
```

Output

```
sfp_accounts

sfp_abuseipdb

sfp_archive

sfp_email

sfp_dns

sfp_emailcrawlr

sfp_hunter

sfp_intellx

...
```

---

# Step 7 — Search Modules

Find email modules

```bash
spiderfoot -M | grep email
```

Example

```
sfp_email

sfp_emailcrawlr

sfp_emailformat

sfp_emailrep
```

Find DNS modules

```bash
spiderfoot -M | grep dns
```

Output

```
sfp_dns

sfp_dnscommonsrv

sfp_dnsresolve
```

Find GitHub

```bash
spiderfoot -M | grep github
```

Output

```
sfp_github
```

Find breach modules

```bash
spiderfoot -M | grep leak
```

or

```bash
spiderfoot -M | grep breach
```

---

# Step 8 — Complete the Table

Your lab asks you to identify one module for each information type.

Below is an example. Your exact module names may vary slightly depending on the SpiderFoot version.

| Information Type                            | Scanner / Module                                                 | API Required              | Comments                                             |
| ------------------------------------------- | ---------------------------------------------------------------- | ------------------------- | ---------------------------------------------------- |
| Possible accounts associated with a domain  | Account Finder (`sfp_accounts`)                                  | No                        | Searches many public sites for related accounts      |
| Links associated with the target            | Cross-Referencer (`sfp_crossref`)                                | No                        | Correlates relationships between discovered entities |
| Email addresses associated with the target  | EmailCrawlr (`sfp_emailcrawlr`)                                  | Yes (Free plan available) | Searches public email references                     |
| Domains and URLs associated with the target | grep.app (`sfp_grep_app`)                                        | Yes (Free plan available) | Finds references in public code repositories         |
| Geolocation information                     | ipapi.com (`sfp_ipapicom`)                                       | Yes (Free plan available) | Provides IP geolocation data                         |
| Data breach information                     | Leak-Lookup or another breach-related module (varies by version) | Usually Yes               | Checks public breach data where supported            |

> Different SpiderFoot releases enable, rename, or retire modules, so compare these with the modules shown in **Settings** on your installation.

---

# Example Module Page

Suppose you open

```
EmailCrawlr
```

You might see

```
Scanner

EmailCrawlr

Module

sfp_emailcrawlr

Purpose

Searches public sources for email addresses

API Required

Yes

Free Plan

Yes
```

---

# Example: grep.app

```
Scanner

grep.app

Module

sfp_grep_app

Purpose

Search public code

API

Required
```

Useful for finding

```
URLs

API endpoints

Domain names

Configuration files
```

---

# Example: ipapi.com

```
Module

sfp_ipapicom
```

Returns

```
Country

City

Latitude

Longitude

ISP
```

---

# Example: Cross Referencer

```
Module

sfp_crossref
```

Purpose

```
Connects discovered entities together

Example

Email

↓

GitHub

↓

Domain

↓

DNS
```

---

# Why is grep useful?

Suppose there are 250 modules.

Instead of scrolling,

search

```bash
grep dns

grep github

grep leak

grep email
```

Much faster.

---

# How to Verify the Answers

For each row in the table:

1. Open **Settings**.
2. Select the module.
3. Read:

   * Module name (`sfp_...`)
   * Description
   * API requirements
4. Record the information in your table.

This is exactly what the lab expects you to do.

---

Absolutely. This is one of the most important SpiderFoot labs because it teaches you how to perform **automated passive reconnaissance (OSINT)**. I'll explain **every click, every option, and what you should expect to see**. I will **not fabricate live scan results**, but I'll show you realistic example outputs and explain how to interpret them.

---








# Part 2: Run a SpiderFoot Scan

## What are we doing?

We will use SpiderFoot to collect **publicly available information** about a domain.

Example target:

```
h4cker.org
```

SpiderFoot will automatically search many public OSINT sources for information such as:

* DNS records
* IP addresses
* Email addresses
* Subdomains
* Technologies
* WHOIS information
* SSL certificates
* Data breaches (if supported)
* Public code repositories
* Social media references
* URLs

---

# Step 1: Start SpiderFoot

Open Terminal.

Run:

```bash
spiderfoot -l 127.0.0.1:5001
```

Expected output:

```text
SpiderFoot 4.x

Listening on:

http://127.0.0.1:5001
```

Leave this terminal running.

---

# Step 2: Open the GUI

Open Firefox.

Go to

```
http://127.0.0.1:5001
```

You will see

```
SpiderFoot

Scans

New Scan

Settings

Logs
```

---

# Step 3: Create a New Scan

Click

```
New Scan
```

You'll see fields similar to:

```
Scan Name

Target

Scan by:

○ Use Case

○ Data Type

○ Modules
```

---

## Fill in the Scan

### Scan Name

This is just a label.

Example:

```
H4cker Footprint Scan
```

---

### Target

Type

```
h4cker.org
```

You can also scan:

```
example.com
```

or an IP address you are authorized to assess.

---

# Step 4: Select Scan Type

SpiderFoot provides four scan profiles.

---

## 1. All

```
Everything
```

Searches every enabled module.

Advantages

* Most comprehensive

Disadvantages

* Slow
* Some modules may actively interact with the target
* Use only when you have authorization

---

## 2. Footprint ✅ (Recommended)

Choose

```
Footprint
```

This profile searches for:

* Domain information
* DNS
* IPs
* Emails
* WHOIS
* SSL certificates
* Public code
* Public identities

This is exactly what the lab requests.

---

## 3. Investigate

Designed for investigating suspicious infrastructure using public threat-intelligence sources.

---

## 4. Passive

Safest choice.

Uses only passive OSINT.

No direct interaction with the target.

---

# Step 5: Run the Scan

Click

```
Run Scan Now
```

SpiderFoot begins collecting information.

---

# Step 6: Watch the Progress

The dashboard displays:

```
██████████
```

Bar graphs appear.

Example:

```
Domains

████████

Emails

███

IPs

██████

URLs

█████

Technology

██
```

Hovering over a bar shows counts.

Example

```
Email Addresses

12 Found
```

---

# Step 7: Wait

SpiderFoot scans continuously.

Small scans:

```
5–15 minutes
```

Typical footprint scans:

```
30–60 minutes
```

Large domains:

```
Several hours
```

You can browse results while the scan is running.

---

# Step 8: Open the Scans Page

Click

```
Scans
```

Example

| Name             | Status  | Started    |
| ---------------- | ------- | ---------- |
| H4cker Footprint | Running | 15 min ago |

---

# Step 9: Stop the Scan

Click the

■

button.

SpiderFoot finalizes the results.

Some modules only produce complete output after the scan ends.

---

# Step 10: Browse Results

Click the scan name.

SpiderFoot opens

```
Browse
```

Example table:

| Data Type  | Data Element                                       | Source Module   |
| ---------- | -------------------------------------------------- | --------------- |
| Domain     | h4cker.org                                         | sfp_dns         |
| IP         | 203.0.113.5                                        | sfp_dnsresolve  |
| Email      | [admin@example.org](mailto:admin@example.org)      | sfp_emailcrawlr |
| URL        | [https://www.example.org](https://www.example.org) | sfp_crossref    |
| Technology | Apache                                             | sfp_builtwith   |

Each row tells you:

* What was found
* Which module found it

---

# Example Results

### Domain

```
h4cker.org
```

Module

```
sfp_dns
```

---

### IP Address

```
203.0.113.5
```

Module

```
sfp_dnsresolve
```

---

### Nameserver

```
ns1.example.net
```

---

### MX Record

```
mail.example.org
```

---

### Email

```
admin@example.org
```

Module

```
sfp_emailcrawlr
```

---

### Technology

```
Apache
```

Module

```
sfp_builtwith
```

---

### WHOIS

```
Registrar

Cloudflare
```

---

### SSL

```
Let's Encrypt
```

---

# Step 11: Register API Keys (Optional)

Some modules require API keys to access additional public data.

Go to:

```
Settings
```

Search for each module.

---

## BuiltWith

Module

```
sfp_builtwith
```

Purpose

Detects web technologies.

Example findings

```
Apache

Nginx

WordPress

PHP

Cloudflare
```

---

## Hunter.io

Module

```
sfp_hunter
```

Purpose

Finds public email addresses associated with a domain.

Example

```
admin@example.org

support@example.org
```

Free tier available with limited requests.

---

## Onion.link

Purpose

Searches public information related to Tor onion services.

---

## IntelligenceX

Module

```
sfp_intellx
```

Purpose

Searches a wide variety of publicly indexed data, such as archived pages, public documents, and other OSINT sources.

---

# API Table

| Module        | Information Type        | API Key                                     |
| ------------- | ----------------------- | ------------------------------------------- |
| BuiltWith     | Detect web technologies | Obtain from BuiltWith (varies by plan)      |
| Hunter.io     | Email address search    | Hunter.io API key (free tier available)     |
| Onion.link    | Tor-related information | If required by the service                  |
| IntelligenceX | Broad OSINT search      | IntelligenceX API key (availability varies) |

---

# Step 12: Add API Keys

Open the module.

Paste the API key.

Click

```
Save
```

Repeat for each module.

---

# Step 13: Run Only API Modules

Click

```
New Scan
```

Choose

```
By Module
```

Uncheck everything.

Check only

```
BuiltWith

Hunter.io

IntelligenceX

Onion.link
```

Target

```
h4cker.org
```

Click

```
Run Scan
```

This limits the scan to those configured modules.

---

# Step 14: Analyze Results

When complete

Open

```
Browse
```

Look at

```
Source Module
```

Example

| Data Type     | Source Module |
| ------------- | ------------- |
| Technology    | sfp_builtwith |
| Email         | sfp_hunter    |
| Leak Site URL | sfp_intellx   |

---

# Lab Question 1

**Which module contributed to the "Leak Site URL" table?**

**Answer:**

```
sfp_intellx
```

---

# Lab Question 2

**What do you see after opening the entries?**

The linked pages typically show the original publicly available source that SpiderFoot referenced. Depending on the target and the data source, this might be an archived webpage, a public document, or another indexed resource.

**Lab answer:**

> The original leaked information.

---

# Overall Workflow

```text
Start SpiderFoot
        │
        ▼
Open http://127.0.0.1:5001
        │
        ▼
New Scan
        │
        ▼
Enter Scan Name
        │
        ▼
Target: h4cker.org
        │
        ▼
Select Footprint
        │
        ▼
Run Scan
        │
        ▼
Wait 30–60 minutes
        │
        ▼
Review Scans
        │
        ▼
Stop Scan (optional)
        │
        ▼
Browse Results
        │
        ▼
Review Source Modules
        │
        ▼
(Optional) Configure API Keys
        │
        ▼
Run Module-Specific Scan
        │
        ▼
Analyze Findings
```

### Tips for your lab report

* Include screenshots of the **New Scan** page, the **running progress graph**, the **Browse** tab, and the **Settings** page showing the configured modules (without exposing your API keys).
* Mention that **Footprint** or **Passive** scans are appropriate for OSINT because they rely primarily on publicly available information and are suitable when you do not have authorization for active testing.
* If you compare multiple scan types, clearly state why you chose **Footprint** for this exercise and what additional information API-enabled modules provided.

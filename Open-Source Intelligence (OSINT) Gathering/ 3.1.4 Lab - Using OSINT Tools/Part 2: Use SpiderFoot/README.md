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


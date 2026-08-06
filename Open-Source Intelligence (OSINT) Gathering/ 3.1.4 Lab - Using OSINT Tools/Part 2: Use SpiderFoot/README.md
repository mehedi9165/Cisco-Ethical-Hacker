This lab teaches you how to use **SpiderFoot**, one of the most powerful **OSINT automation tools**. It automates information gathering from **200+ modules** and **1000+ public data sources**. Security professionals use it during the **Reconnaissance (Recon)** phase of a penetration test to collect publicly available information about a target.

> **Important:** Always scan only domains, IPs, or organizations that you own or have permission to assess. When in doubt, use the **Passive** scan type, which is designed to avoid interacting directly with the target.

---

# Lab Objective

After completing this lab, you will be able to:

* Start SpiderFoot
* Understand SpiderFoot modules
* Use the Settings page
* Search modules from the terminal
* Create a new scan
* Run a Footprint or Passive scan
* Analyze scan results
* Configure API keys (optional)
* Understand which modules provide different types of OSINT information

---

# What is SpiderFoot?

SpiderFoot is an **OSINT automation framework**.

Instead of manually visiting many websites, SpiderFoot queries many public data sources and combines the results into one report.

It can search for:

* Domains
* Email addresses
* IP addresses
* Phone numbers
* Names
* Usernames
* Subnets
* Autonomous System Numbers (ASN)

---

# Step 1: Start SpiderFoot

Open Kali Linux.

Open Terminal.

Run:

```bash
spiderfoot -l 127.0.0.1:5001
```

Example output:

```text
SpiderFoot starting...

Listening on:

http://127.0.0.1:5001
```

Do **NOT** close this terminal.

Minimize it.

---

# Step 2: Open the GUI

Open Firefox.

Visit:

```text
http://127.0.0.1:5001
```

You will see the SpiderFoot dashboard.

If this is your first time, the **Scans** page will be empty.

Menu:

```text
Scans

New Scan

Browse

Settings

Logs
```

---

# Step 3: Explore Settings

Click

```text
Settings
```

There are many modules (the exact number depends on your SpiderFoot version).

The first items configure SpiderFoot itself.

Below them are the OSINT modules.

Each module contains:

* Scanner name
* Module name
* Description
* API settings
* Documentation

Example:

```text
Account Finder

Module:

sfp_accounts
```

---

# Step 4: Understand Module Names

Every SpiderFoot module starts with:

```text
sfp_
```

Example:

| Module          | Purpose                       |
| --------------- | ----------------------------- |
| sfp_accounts    | Username/account search       |
| sfp_emailcrawlr | Email search                  |
| sfp_crossref    | Link relationships            |
| sfp_ipapicom    | IP geolocation                |
| sfp_grep_app    | Public code/domain references |
| sfp_intellx     | IntelligenceX integration     |

---

# Step 5: Find Modules Using the Terminal

SpiderFoot can list modules from the command line.

Run:

```bash
spiderfoot -M
```

This displays all available modules.

To search for a keyword, use:

```bash
spiderfoot -M | grep email
```

Example output:

```text
sfp_emailcrawlr

sfp_emailformat

sfp_emailrep
```

Search for IP modules:

```bash
spiderfoot -M | grep ip
```

Search for DNS:

```bash
spiderfoot -M | grep dns
```

This helps you quickly identify relevant modules.

---

# Step 6: Complete the Lab Table

Below are example answers based on common SpiderFoot modules.

| Information Type             | Scanner / Module                                              | API Required?             | Comments                                           |
| ---------------------------- | ------------------------------------------------------------- | ------------------------- | -------------------------------------------------- |
| Possible accounts            | Account Finder (`sfp_accounts`)                               | No                        | Finds public accounts on many websites             |
| Links associated with target | Cross-Referencer (`sfp_crossref`)                             | No                        | Finds relationships between discovered entities    |
| Email addresses              | EmailCrawlr (`sfp_emailcrawlr`)                               | Yes (Free plan available) | Searches public email references                   |
| Domains & URLs               | grep.app (`sfp_grep_app`)                                     | Yes (Free plan available) | Finds public references to domains and URLs        |
| Geolocation                  | ipapi.com (`sfp_ipapicom`)                                    | Yes (Free plan available) | Provides IP geolocation information                |
| Data breach information      | Leak-Lookup / Intelligence-related module (varies by version) | Yes                       | Searches public breach information where supported |

> Module names and API requirements may vary slightly depending on the SpiderFoot version you are using.

---

# Step 7: Create a New Scan

Click

```text
New Scan
```

You will see:

```text
Scan Name

Target

Scan Type
```

Example:

**Scan Name**

```text
My Domain Scan
```

Target:

```text
h4cker.org
```

---

# Step 8: Choose the Scan Type

SpiderFoot offers several use cases:

### 1. All

Searches almost everything.

Very thorough.

Can take several hours.

---

### 2. Footprint

Recommended for learning.

Looks for:

* Domains
* Emails
* IPs
* DNS
* Web information
* Public identities

---

### 3. Investigate

Focuses on indicators of potentially malicious infrastructure using public threat intelligence sources.

---

### 4. Passive

Safest option.

Only collects publicly available information without directly interacting with the target.

For coursework and demonstrations, **Passive** is generally the safest choice unless you have explicit permission for broader testing.

---

# Step 9: Start the Scan

Click

```text
Run Scan Now
```

SpiderFoot starts scanning.

Example status:

```text
Running...

10%

25%

50%

80%

100%
```

Results begin appearing as modules complete.

---

# Step 10: Monitor the Graph

SpiderFoot displays a graph.

Example:

```text
Email

██████

IP Address

████

Domains

███████

URLs

█████
```

Hover over a bar.

Example:

```text
Email Addresses

15 Found
```

---

# Step 11: Stop the Scan

Go to:

```text
Scans
```

Click the **Stop** button (black square) if you want to end the scan.

Some reports become available only after the scan is completed or stopped.

---

# Step 12: Browse Results

Click the scan name.

Open:

```text
Browse
```

You will see a table.

Example:

| Data Type | Data                                          | Source Module   |
| --------- | --------------------------------------------- | --------------- |
| Email     | [admin@example.com](mailto:admin@example.com) | sfp_emailcrawlr |
| Domain    | example.com                                   | sfp_dns         |
| IP        | 192.0.2.1                                     | sfp_ipapicom    |
| URL       | [www.example.com](http://www.example.com)     | sfp_crossref    |

Each row shows which module discovered the information.

---

# Step 13: Configure API Keys (Optional)

Some modules require API keys.

Go to:

```text
Settings
```

Find a module.

Click:

```text
?
```

SpiderFoot explains:

* Where to register
* How to obtain the key
* Whether a free plan exists

Example modules:

| Module        | Information                            |
| ------------- | -------------------------------------- |
| BuiltWith     | Technologies used by websites          |
| Hunter.io     | Public email addresses                 |
| Onion.link    | Public Tor-related information         |
| IntelligenceX | Broad public OSINT search capabilities |

---

# Step 14: Enter API Keys

After registering (where applicable):

Paste the API key into the module settings.

Click

```text
Save
```

Repeat for other modules.

---

# Step 15: Scan Using Only API Modules

Go to:

```text
New Scan
```

Choose:

```text
By Module
```

Select only the modules for which you've configured API keys.

Target:

```text
h4cker.org
```

Click:

```text
Run Scan
```

This focuses the scan on those specific data sources.

---

# Step 16: Analyze API Results

When finished:

Open

```text
Browse
```

Look at the **Source Module** column.

Example:

| Data          | Source Module   |
| ------------- | --------------- |
| Leak Site URL | `sfp_intellx`   |
| Email         | `sfp_hunter`    |
| Technology    | `sfp_builtwith` |

---

# Lab Question 1

**Which module contributed to the Leak Site URL table?**

**Answer:**

> `sfp_intellx`

---

# Lab Question 2

**What do you see when opening several entries?**

**Answer:**

> Publicly available information referenced by the module, such as archived or indexed content from the linked source. The exact results depend on the target and the data available through the configured service.

---

# Overall Workflow

```text
Start SpiderFoot
        │
        ▼
Open GUI
        │
        ▼
Explore Settings
        │
        ▼
Review Modules
        │
        ▼
(Optional) Configure API Keys
        │
        ▼
New Scan
        │
        ▼
Choose Passive or Footprint
        │
        ▼
Run Scan
        │
        ▼
Wait for Results
        │
        ▼
Browse Findings
        │
        ▼
Review Source Modules
        │
        ▼
Create Report
```


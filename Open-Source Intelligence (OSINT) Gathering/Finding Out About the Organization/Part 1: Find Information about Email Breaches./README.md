# Step 1: Investigate your email status.

---

# Objective

Determine whether an email address has been exposed in any known data breach.

You will answer:

> **Have your email addresses been part of a breach? If so, in which breach(es) were they disclosed?**

---

# Step 1: Choose an Email Address

Use one of the following:

* Your own email address
* A test email address (if your instructor provided one)
* A company domain (example: `example.com`)

Example:

```
johnsmith@gmail.com
```

or

```
company.com
```

---

# Step 2: Visit Have I Been Pwned

Open your browser.

Go to:

**[https://haveibeenpwned.com](https://haveibeenpwned.com)**

or
f-secure.com
hacknotice.com
breachdirectory.com
keepersecurity.com



You will see a search box.

Example:

```
Email address:
_____________________
```

Enter your email.

Example:

```
johnsmith@gmail.com
```

Click **pwned?**

---

# Step 3: Read the Result

There are two possible results.

---

## Case 1 — Good News

If you see something similar to

```
Good news — no pwnage found!
```

That means

* Your email has not been found in the public breach database.

Your answer would be

> My email was not found in any known public data breach.

---

## Case 2 — Breached

If you see

```
Oh no — pwned!
```

The site will list breaches.

Example

| Breach   | Year |
| -------- | ---- |
| Adobe    | 2013 |
| LinkedIn | 2012 |
| Dropbox  | 2016 |

It also explains what information was leaked.

Example

```
Email address

Password

Username

Phone number
```

---

# Step 4: Read Each Breach

Click each breach.

For example

```
Adobe
```

It may explain

```
In October 2013 Adobe suffered a breach affecting
153 million users.
```

Look for

* Date
* Number of users affected
* What information leaked

Example

```
Leaked Data

Email

Password

Username

Hint

Password hash
```

Write these down.

---

# Step 5: Try Other Websites

Repeat the search using other breach-checking services mentioned in your lab, such as:

* Have I Been Pwned
* F-Secure breach checker
* HackNotice
* BreachDirectory
* Keeper Security breach checker

Different services may show different results because they rely on different datasets.

---

# Step 6: Search a Company Domain

Instead of an email

Search

```
company.com
```

Some services allow domain searches.

Example

```
company.com
```

The service may report

```
23 breached accounts found
```

or

```
No breach found
```

For a penetration test, this helps identify whether employees' work email addresses have appeared in known breaches. You should only search domains you own or have permission to assess.

---

# Step 7: Record Your Findings

Example report

```
Email Tested:
johnsmith@gmail.com

Result:
Found in 3 breaches.

Breaches:
1. Adobe (2013)
2. LinkedIn (2012)
3. Dropbox (2016)

Exposed Information:
• Email address
• Password hash
• Username
```

---

# Step 8: Answer the Lab Question

If no breach

```
The tested email address was not found in any publicly known data breaches.
```

If breaches were found

```
The tested email address was found in multiple breaches, including Adobe (2013), LinkedIn (2012), and Dropbox (2016). The exposed information included the email address and password hashes.
```

---

# Why This Matters in Penetration Testing

Finding breached email addresses can help assess an organization's exposure because attackers may attempt to reuse credentials or target known accounts. During an authorized penetration test, this information can support risk assessment and recommendations such as:

* Enforcing password changes after known breaches.
* Requiring multi-factor authentication (MFA).
* Monitoring for credential reuse.
* Providing employee security awareness training.

---

# Example Answer for the Lab

**If no breach:**

```
The email address tested was not found in any publicly known data breaches according to the checked breach databases.
```

**If breaches were found:**

```
The email address was found in multiple publicly known breaches, including Adobe (2013), LinkedIn (2012), and Dropbox (2016). The exposed information included the email address, username, and password hash. These findings indicate that the account should use a unique password and MFA if not already enabled.
```

> **Important:** Only search email addresses or domains that you own or have explicit permission to assess. Unauthorized collection or use of breach data may violate privacy policies, laws, or ethical guidelines.






# Step 2: Use a tool to find email addresses for a domain.

This lab teaches you how to use **EmailHarvester** to gather **publicly available email addresses** associated with a domain as part of **OSINT (Open-Source Intelligence)**. The intent is reconnaissance on public information, not unauthorized access. Below is a detailed walkthrough.

---

# Objective

Use **EmailHarvester** to:

* Find publicly available email addresses for a domain.
* Learn the purpose of the `-d` option.
* Save the results to a file.
* Inspect the generated files.
* Optionally check whether discovered email addresses have appeared in public breach databases.

---

# Step 1: Open Kali Linux

Start your Kali Linux virtual machine.

Open a Terminal.

You should see something similar to:

```bash
kali@kali:~$
```

---

# Step 2: Run EmailHarvester

Type:

```bash
emailharvester
```

Since the tool may not be installed, Kali might display a message like:

```text
The program 'emailharvester' is currently not installed.
Would you like to install it? (Y/n)
```

Type:

```text
y
```

Press **Enter**.

If prompted, enter the password for the **kali** user.

The installation process will begin.

Example:

```text
Installing EmailHarvester...
Downloading packages...
Installation completed.
```

---

# Step 3: View the Help Menu

Type:

```bash
emailharvester -h
```

or

```bash
emailharvester --help
```

You will see the available options.

Example:

```text
Usage:
 emailharvester [OPTIONS]

Options:

-d DOMAIN
-s FILE
-h
-v
```

---

# Step 4: Understand the `-d` Option

The lab asks:

> **What does the -d option do?**

**Answer:**

> The `-d` option specifies the target **domain** that EmailHarvester will search for publicly available email addresses.

Example:

```bash
emailharvester -d example.com
```

This tells EmailHarvester to search public sources for email addresses associated with `example.com`.

---

# Step 5: Scan a Domain

The lab suggests domains such as:

```text
h4cker.org
hackxor.net
scanme.nmap.org
```

Example command:

```bash
emailharvester -d h4cker.org
```

The tool searches public sources such as search engines or publicly indexed web pages (depending on its capabilities and configuration) for email addresses associated with that domain.

Example output:

```text
Searching...
Searching completed...

Emails Found:

admin@h4cker.org

support@h4cker.org

info@h4cker.org

security@h4cker.org
```

*(The actual results will vary depending on the domain and current public information.)*

---

# Step 6: Save Results to a File

Instead of only displaying results on the screen, use the `-s` option to save them.

Example:

```bash
emailharvester -d h4cker.org -s results
```

This creates output files.

The tool typically generates:

```text
results.txt

results.xml
```

If no path is specified, they are commonly stored in the default output directory used by the tool (as described in your lab instructions).

---

# Step 7: Locate the Files

Navigate to the output directory mentioned in your lab.

For example:

```bash
cd /usr/share/emailharvester
```

List the files:

```bash
ls
```

Example:

```text
results.txt

results.xml
```

---

# Step 8: View the Text File

Open it with:

```bash
cat results.txt
```

or

```bash
less results.txt
```

Example contents:

```text
admin@h4cker.org

support@h4cker.org

sales@h4cker.org
```

---

# Step 9: View the XML File

Display it:

```bash
cat results.xml
```

Example:

```xml
<emails>
    <email>admin@h4cker.org</email>
    <email>support@h4cker.org</email>
    <email>sales@h4cker.org</email>
</emails>
```

XML output is useful because other security tools can often import structured XML data.

---

# Step 10: Check Whether an Email Appears in a Public Breach

If you find a public email address, you can check whether it has appeared in known public breach databases.

For example, if EmailHarvester finds:

```text
admin@h4cker.org
```

You can search that email on a public breach-checking service (such as Have I Been Pwned) to see whether it appears in any **publicly disclosed** breaches.

Possible result:

```text
Found in:

LinkedIn

Dropbox

Adobe
```

Or:

```text
No breach found
```

This information can help assess whether the account has been exposed in publicly known incidents. It does **not** reveal current passwords or authorize any further access.

---

# Step 11: Why Save the Results?

Many security tools can accept a text file containing email addresses as input.

For example:

```text
results.txt

↓

Email verification

↓

Awareness planning

↓

OSINT reporting
```

This makes it easier to organize and reuse the information gathered during an authorized assessment.

---

# Sample Lab Answers

### Question 1

**What does the `-d` option do?**

**Answer:**

> The `-d` option specifies the target domain that EmailHarvester will scan for publicly available email addresses.

---

### Example Observation

```text
Command Used:

emailharvester -d h4cker.org

Result:

Found 6 email addresses.

Saved Output:

results.txt

results.xml
```

---

### Example Report

```text
Target Domain:
h4cker.org

Emails Discovered:
admin@h4cker.org
info@h4cker.org
support@h4cker.org

Output Files:
results.txt
results.xml

Observation:
Some discovered email addresses may also appear in publicly known breach databases. This can indicate prior exposure and should be considered during an authorized security assessment.
```

### Notes

* Results depend on what information is **publicly available** at the time of the search.
* Some domains may return no email addresses.
* Always perform OSINT only on domains you own or have permission to assess, and follow your course's rules and applicable laws.



# Step 3: Use Spiderfoot to research email addresses.
This lab introduces **SpiderFoot**, an OSINT automation tool that collects information from many public sources. In this exercise, you'll configure SpiderFoot, explore modules, and run a scan focused on an email address using only appropriate OSINT modules. The purpose is to gather publicly available information for security assessment or learning.

---

# Objective

Learn how to:

* Start SpiderFoot
* Open the web interface
* Understand modules
* Identify modules useful for email OSINT
* Run an email scan
* Interpret the results

---

# Step 1: Open Kali Linux

Boot your Kali Linux VM.

Open a Terminal.

---

# Step 2: Start SpiderFoot

Run:

```bash
spiderfoot -l 127.0.0.1:5001
```

Example output:

```text
Starting SpiderFoot...

Listening on:

http://127.0.0.1:5001
```

Do **not** close this terminal.

Just minimize it.

Why?

Because this terminal is running the SpiderFoot web server. If you close it, SpiderFoot stops.

---

# Step 3: Open SpiderFoot in Your Browser

Open Firefox (or another browser in Kali).

Go to:

```text
http://127.0.0.1:5001
```

You should see the SpiderFoot home page.

Typical menu:

```
Dashboard

New Scan

Scans

Settings

Logs
```

---

# Step 4: Open Settings

Click

```
Settings
```

This page contains all SpiderFoot modules.

There are usually many modules (the exact number varies by version).

Each module has:

* Module name
* Description
* Required API key (if any)
* Status

Example:

| Module        | Description                | API Required |
| ------------- | -------------------------- | ------------ |
| Bing          | Search Microsoft Bing      | Yes          |
| DuckDuckGo    | Search DuckDuckGo          | No           |
| Archive.org   | Search archived pages      | No           |
| Leak-Lookup   | Search public breach data  | Yes          |
| EmailCrawlr   | Search for email addresses | No           |
| AccountFinder | Search for usernames       | No           |

---

# Step 5: Read Module Descriptions

Click a module.

Example:

```
EmailCrawlr
```

Description:

```
Searches public websites
for email addresses.
```

Next:

```
Leak-Lookup
```

Description:

```
Checks whether an email
appears in publicly known
breach datasets.
```

Repeat for other modules.

---

# Step 6: API Keys (Optional)

Some modules require an API key.

Example:

```
Bing API

Dehashed API

Leak-Lookup API
```

Without an API key:

```
Module Disabled
```

If you create a free account where available, you can enter the API key into the module's configuration.

For this lab, API keys are optional unless your instructor requires them.

---

# Step 7: Find Modules Useful for Email Investigation

The lab asks you to identify modules useful for researching an email address.

Examples include:

### 1. EmailCrawlr

Purpose:

Finds publicly available email addresses.

Useful because:

```
Finds email references
on websites.
```

---

### 2. AccountFinder

Purpose:

Looks for usernames associated with an email or naming pattern on public services.

Useful because:

```
May identify public accounts
that use the same identifier.
```

---

### 3. Leak-Lookup

Purpose:

Checks whether an email appears in known public breach datasets (API-dependent).

Useful because:

```
Shows previous
data breach exposure.
```

---

### 4. Archive.org

Purpose:

Searches archived websites.

Useful because:

```
Finds older versions
of websites.
```

---

### 5. CommonCrawl

Purpose:

Searches publicly indexed web data.

Useful because:

```
Finds pages mentioning
the email.
```

---

### 6. DuckDuckGo

Purpose:

Searches public web results.

Useful because:

```
Looks for public references
to the email.
```

---

### 7. Bing

Purpose:

Searches Microsoft's search engine.

Useful because:

```
Searches indexed pages.
```

---

### 8. Ahmia

Purpose:

Searches public Tor-hidden service indexes.

Use only if it is relevant to your assignment and within your institution's rules.

---

# Step 8: Start a New Scan

Click:

```
New Scan
```

You will see several scan types.

Select:

```
By Module
```

This lets you choose exactly which modules will run.

---

# Step 9: Enter Scan Information

Example:

**Scan Name**

```
Email Investigation
```

**Target**

Example:

```
john@example.com
```

(Use an email address you own, one provided by your instructor, or another address you have permission to assess.)

---

# Step 10: Select Modules

Check only the modules you want.

Example:

```
✔ EmailCrawlr

✔ DuckDuckGo

✔ CommonCrawl

✔ Archive.org

✔ AccountFinder
```

If configured with API keys, you might also include:

```
✔ Leak-Lookup

✔ Bing
```

---

# Step 11: Run the Scan

Scroll to the bottom.

Click:

```
Run Scan Now
```

SpiderFoot begins collecting information.

Example status:

```
Running...

Completed:

10%

35%

80%

100%
```

Scan time depends on the selected modules and the target.

---

# Step 12: View Results

When complete, SpiderFoot organizes the findings into categories.

Possible categories include:

```
Email Addresses

Web Pages

Social Profiles

Leaks

DNS

Domain Information

Search Engine Results
```

For an email-focused scan, you might see:

```
Target:

john@example.com

Email Found:

Yes

Referenced On:

3 websites

Possible Public Profiles:

2

Breach Exposure:

None reported
```

Or, if using a breach-checking module with a valid API:

```
Known Breaches:

Adobe

LinkedIn
```

The exact results depend on the target and available public information.

---

# Step 13: Examine Relationships

Click an item to see where the information came from.

For example:

```
Email

↓

Web page

↓

Organization

↓

Related domain
```

SpiderFoot can also display relationship graphs in some versions, helping visualize connections between discovered entities.

---

# Step 14: Document Your Findings

Example report:

```
Scan Name:
Email Investigation

Target:
john@example.com

Modules Used:
EmailCrawlr
AccountFinder
Archive.org
DuckDuckGo
CommonCrawl

Results:
• Email referenced on 4 public webpages.
• No publicly reported breach exposure from the configured modules.
• One public profile associated with the same identifier.
```

---

# Example Lab Answer

**Modules useful for email scans:**

* EmailCrawlr — Finds publicly available email addresses.
* AccountFinder — Looks for related public accounts.
* Leak-Lookup — Checks whether an email appears in publicly known breach datasets (API required).
* Archive.org — Searches archived web pages.
* CommonCrawl — Searches indexed web content.
* DuckDuckGo — Searches public web references.
* Bing — Searches indexed pages (API may be required).

---

## Workflow Summary

```text
Start SpiderFoot
        │
        ▼
Open http://127.0.0.1:5001
        │
        ▼
Settings
        │
        ▼
Review Email Modules
        │
        ▼
New Scan
        │
        ▼
By Module
        │
        ▼
Enter Scan Name
        │
        ▼
Enter Email Target
        │
        ▼
Select Modules
        │
        ▼
Run Scan
        │
        ▼
Review Results
        │
        ▼
Document Findings
```

### Best Practices

* Only scan email addresses or domains that you own or are authorized to assess.
* Remember that SpiderFoot aggregates **publicly available** information; it does not verify that every finding is current or accurate.
* Treat breach-related findings as indicators for further investigation rather than proof of account compromise.

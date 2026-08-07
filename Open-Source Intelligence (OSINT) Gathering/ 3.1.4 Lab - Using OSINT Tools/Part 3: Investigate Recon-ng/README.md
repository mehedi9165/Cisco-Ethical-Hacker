

# Part 3: Investigate Recon-ng

## What is Recon-ng?

Recon-ng is an OSINT framework written in Python.

It automates reconnaissance tasks such as:

* Domain enumeration
* Email discovery
* Subdomain discovery
* WHOIS lookup
* DNS lookup
* Company profiling
* Social media searches
* Data collection from APIs

Unlike SpiderFoot, Recon-ng stores all results in an SQLite database inside a **workspace**.

---

# Architecture of Recon-ng

```
                 Recon-ng
                     │
      ┌──────────────┼──────────────┐
      │              │              │
 Workspaces      Modules        Database
      │              │              │
      │              │              │
Customer A     DNS Module     Hosts Table
Customer B     Bing Module    Domains Table
Customer C     WHOIS Module   Contacts Table
```

Each workspace keeps its own database.

---

# Step 1: Start Recon-ng

Open Kali Terminal.

Run:

```bash
recon-ng
```

Example:

```text
Recon-ng v5.x

[recon-ng][default] >
```

The prompt changes because you are now inside the Recon-ng framework.

---

# Step 2: View Help

Type:

```bash
help
```

Example output:

```text
Commands

back
dashboard
db
exit
help
marketplace
modules
options
show
workspaces
```

---

# Step 3: Investigate Workspaces

Recon-ng stores investigations inside **workspaces**.

Think of workspaces as separate folders/projects.

Example:

```
Workspace 1

Company A

Workspace 2

Company B

Workspace 3

University Lab
```

---

## View Workspace Help

Type

```bash
workspaces help
```

Output shows:

```text
create

list

remove

select
```

---

# Lab Question 1

### How can you display available workspaces?

Answer:

```bash
workspaces list
```

---

Example

```bash
[recon-ng][default] > workspaces list
```

Output

```text
default

companyA

companyB
```

---

# Lab Question 2

### How can you remove a workspace?

Answer

```bash
workspaces remove test
```

General syntax

```bash
workspaces remove <workspace_name>
```

Example

```bash
workspaces remove companyA
```

---

# Step 4: Create a Workspace

Create a new workspace.

Example

```bash
workspaces create test
```

Output

```text
Workspace created.
```

Prompt changes

```text
[recon-ng][test] >
```

Now all data goes into the **test** workspace.

---

# Step 5: View Commands Inside Workspace

Type

```bash
help
```

Commands include

```
dashboard

modules

options

run

show

back

exit
```

---

# Lab Question 3

### What command exits the workspace/module and returns to the previous Recon-ng prompt?

Answer

```bash
back
```

---

# Step 6: Investigate Modules

Type

```bash
modules search
```

If this is a fresh installation, you may see:

```text
No modules installed.
```

---

# Lab Question 4

### How many modules are available?

Answer

```
No modules are installed.
```

---

# Step 7: Explore the Marketplace

Recon-ng downloads modules from its online marketplace.

Search:

```bash
marketplace search
```

You will see categories such as:

```
recon/

reporting/

import/

discovery/
```

Example module

```
recon/domains-hosts/bing_domain_web
```

---

# Step 8: Search for Specific Modules

Example:

```bash
marketplace search bing
```

Example output

```
recon/domains-hosts/bing_domain_web
```

Search

```bash
marketplace search shodan
```

Output may include Shodan-related modules.

---

# Lab Question 5

### What are the requirements for Shodan modules?

Answer

> They have **dependencies (D)** and require **API keys (K)**. Dependencies are Python packages needed for the module, and the API key is required to query the Shodan service.

---

# Step 9: View Module Information

Type

```bash
marketplace info recon/domains-hosts/bing_domain_web
```

Example output

```
Name

Version

Author

Description

Options

Dependencies

API Requirements
```

---

# Step 10: Install a Module

Install Bing module

```bash
marketplace install recon/domains-hosts/bing_domain_web
```

Output

```
Installing...

Done
```

Verify

```bash
modules search
```

Output

```
bing_domain_web
```

---

# Install Hackertarget Module

Search

```bash
marketplace search hackertarget
```

Install

```bash
marketplace install recon/domains-hosts/hackertarget
```

---

# Step 11: Load a Module

Create a new workspace

```bash
workspaces create lab
```

Load Hackertarget

```bash
modules load recon/domains-hosts/hackertarget
```

Prompt changes

```
[recon-ng][lab][hackertarget] >
```

---

# Step 12: View Module Information

Type

```bash
info
```

Output

```
Name

Version

Author

Description

Options
```

---

# Lab Question 6

### What information is available for this module?

Answer

> The module name, version, author, description, and configurable options.

---

# Lab Question 7

### What is the only option?

Answer

```
SOURCE
```

---

# Step 13: Set the Target

Instead of passing arguments every time, Recon-ng stores them as options.

Type

```bash
options set SOURCE hackxor.net
```

Output

```
SOURCE => hackxor.net
```

Verify

```bash
info
```

You will see

```
SOURCE

hackxor.net
```

---

# Step 14: Run the Module

Type

```bash
run
```

The module queries its configured public source and stores the results in the workspace database.

---

# Step 15: View Dashboard

Type

```bash
dashboard
```

Example

```
Hosts

9

Domains

1

Contacts

0

Companies

0
```

---

# Lab Question 8

### What is the Recon-ng label for the discovered subdomains?

Answer

```
Hosts
```

---

### How many were discovered?

The course answer is:

```
9 Hosts
```

**Note:** The actual number may differ today because DNS records change over time.

---

# Step 16: Show Hosts

Type

```bash
show hosts
```

Example output

```
mail.hackxor.net

vpn.hackxor.net

ftp.hackxor.net

www.hackxor.net

blog.hackxor.net

api.hackxor.net
```

---

# Step 17: Load the Bing Module

Return

```bash
back
```

Load

```bash
modules load recon/domains-hosts/bing_domain_web
```

Set target

```bash
options set SOURCE hackxor.net
```

Run

```bash
run
```

View results

```bash
show hosts
```

---

# Lab Question 9

### How many subdomains did the Bing module find?

**Course answer:**

> At the time of writing, both the Bing and Hackertarget modules found **6 subdomains**.

**Important:** This number may be different today because search engine indexes and DNS records change.

---

# Step 18: Use the Web Interface

Open another terminal.

Run

```bash
recon-web
```

Example output

```
Listening on:

http://127.0.0.1:5000
```

Open your browser.

Visit

```
http://127.0.0.1:5000
```

The web interface displays the current workspace.

You can:

* Switch between workspaces.
* View tables (Hosts, Domains, Contacts, Companies).
* Export data for reports.
* Browse findings more easily than in the terminal.

---

# Workflow Summary

```text
Start Recon-ng
        │
        ▼
Create Workspace
        │
        ▼
Search Marketplace
        │
        ▼
Install Modules
        │
        ▼
Load Module
        │
        ▼
Set SOURCE Option
        │
        ▼
Run Module
        │
        ▼
Results Saved to Database
        │
        ▼
Dashboard Summary
        │
        ▼
Show Hosts / Domains
        │
        ▼
(Optional) View in recon-web
```

---

# Final Answers (Lab)

| Question                                                            | Answer                                                  |
| ------------------------------------------------------------------- | ------------------------------------------------------- |
| How do you display available workspaces?                            | `workspaces list`                                       |
| How do you remove a workspace?                                      | `workspaces remove <workspace_name>`                    |
| What command returns to the previous prompt?                        | `back`                                                  |
| How many modules are available initially?                           | No modules are installed.                               |
| What are the requirements for Shodan modules?                       | They have dependencies (D) and require API keys (K).    |
| Which Bing module requires no dependencies or API keys?             | `recon/domains-hosts/bing_domain_web`                   |
| What information does `info` show?                                  | Module name, version, author, description, and options. |
| What is the only option in the Hackertarget module?                 | `SOURCE`                                                |
| What label does Recon-ng use for discovered subdomains?             | `Hosts`                                                 |
| How many hosts were discovered in the course example?               | 9 (actual results may vary).                            |
| How many subdomains did the Bing module find in the course example? | 6 (actual results may vary).                            |


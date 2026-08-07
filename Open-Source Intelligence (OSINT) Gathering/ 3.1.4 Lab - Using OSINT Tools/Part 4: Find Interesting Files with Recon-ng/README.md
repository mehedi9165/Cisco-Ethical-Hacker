
# Part 4: Find Interesting Files with Recon-ng

## Objective

Learn how to:

* Install a Recon-ng module
* Load the module
* Configure it
* Run it
* Analyze the output
* Locate the generated CSV report

---

# What Does This Module Do?

Module:

```text
discovery/info_disclosure/interesting_files
```

Purpose:

It searches **public search engine indexes** for files that may reveal useful information, such as:

* PDF
* DOC
* DOCX
* XLS
* XLSX
* PPT
* PPTX
* TXT
* CSV
* LOG
* BAK

These files sometimes contain:

* Employee names
* Email addresses
* Internal server names
* Software versions
* Phone numbers
* Metadata
* Network diagrams
* Project information

During a penetration test, these files can provide valuable OSINT without directly interacting with the target's infrastructure.

---

# Step 1: Start Recon-ng

Open a terminal.

Run:

```bash
recon-ng
```

Example:

```text
Recon-ng v5.x

[recon-ng][default] >
```

---

# Step 2: Search the Marketplace

Search for the module:

```bash
marketplace search interesting
```

Example output:

```text
+-------------------------------------------------------------+
| Path                                                        |
+-------------------------------------------------------------+
| discovery/info_disclosure/interesting_files                 |
+-------------------------------------------------------------+
```

---

## Lab Question

### Which module did you find?

Answer:

```text
discovery/info_disclosure/interesting_files
```

✔ This is the official lab answer.

---

# Step 3: Install the Module

Install it:

```bash
marketplace install discovery/info_disclosure/interesting_files
```

Example output:

```text
[*] Module installed successfully.
```

---

Verify installation:

```bash
modules search interesting
```

Output:

```text
discovery/info_disclosure/interesting_files
```

---

# Step 4: Load the Module

Load it:

```bash
modules load discovery/info_disclosure/interesting_files
```

Prompt changes:

```text
[recon-ng][default][interesting_files] >
```

---

# Step 5: View Module Information

Type:

```bash
info
```

Example:

```text
Name:
Interesting Files

Author:
Recon-ng Team

Description:
Searches public indexes for interesting files.

Options:
SOURCE
```

Notice:

The only required option is:

```text
SOURCE
```

---

# Step 6: Configure the Target

Suppose the lab uses:

```text
hackxor.net
```

Configure it:

```bash
options set SOURCE hackxor.net
```

Output:

```text
SOURCE => hackxor.net
```

Verify:

```bash
info
```

Expected:

```text
SOURCE

hackxor.net
```

---

# Step 7: Run the Module

Execute:

```bash
run
```

The module searches for indexed files associated with the target domain.

---

## What Happens Internally?

Conceptually, the module looks for public files associated with the domain, such as:

```text
site:hackxor.net filetype:pdf
site:hackxor.net filetype:doc
site:hackxor.net filetype:xls
site:hackxor.net filetype:ppt
```

It gathers the matching URLs and saves them for later analysis.

---

# Example Output

The exact output depends on the target and current search engine indexes.

A typical result might resemble:

```text
Found:

https://hackxor.net/docs/security.pdf

https://hackxor.net/files/employees.xlsx

https://hackxor.net/manuals/router_config.pdf

https://hackxor.net/reports/network.pdf
```

**These are illustrative examples, not actual findings.**

---

# Step 8: CSV File Creation

When the module finishes, it creates a CSV report.

Typical location:

```text
~/.recon-ng/data/
```

or

```text
recon-ng/data/
```

depending on how Recon-ng is installed.

---

Example:

```text
interesting_files.csv
```

---

# Step 9: View the CSV

Open it with:

```bash
cat ~/.recon-ng/data/interesting_files.csv
```

or

```bash
less ~/.recon-ng/data/interesting_files.csv
```

or

```bash
xdg-open ~/.recon-ng/data/interesting_files.csv
```

---

Example CSV

```text
URL,TYPE

https://hackxor.net/files/report.pdf,PDF

https://hackxor.net/docs/manual.docx,DOCX

https://hackxor.net/backups/network.xlsx,XLSX
```

---

# Step 10: Why Are These Files Valuable?

Suppose you find:

```text
employees.pdf
```

Opening it might reveal:

```text
CEO

John Smith

IT Manager

Alice Brown

Network Engineer

David Lee
```

Now you know:

* Employee names
* Job roles
* Departments

This information can support authorized security assessments and awareness exercises.

---

Another example:

```text
network_diagram.pdf
```

It might contain:

```text
Firewall

VPN

DMZ

Mail Server

Internal IP ranges
```

Again, this is **only an illustrative example** of the kinds of information such files can contain.

---

# Step 11: Experiment with Other Domains

The lab suggests trying other domains that you are authorized to assess.

Examples include:

```text
h4cker.org
```

or

```text
scanme.nmap.org
```

Always ensure you are following the scope and rules of your course or engagement.

---

# Useful Recon-ng Commands

Search modules:

```bash
marketplace search interesting
```

Install:

```bash
marketplace install discovery/info_disclosure/interesting_files
```

Load:

```bash
modules load discovery/info_disclosure/interesting_files
```

Display module info:

```bash
info
```

Set the target:

```bash
options set SOURCE hackxor.net
```

Run:

```bash
run
```

Return:

```bash
back
```

Show collected data:

```bash
show hosts
```

View dashboard:

```bash
dashboard
```

---

# Workflow Summary

```text
Open Recon-ng
        │
        ▼
Search Marketplace
        │
        ▼
Install Module
        │
        ▼
Load Module
        │
        ▼
Set SOURCE
        │
        ▼
Run Module
        │
        ▼
Search Public Files
        │
        ▼
Generate CSV Report
        │
        ▼
Review File URLs
        │
        ▼
Analyze Metadata & Contents
```

---

# Official Lab Answers

| Question                              | Answer                                                                                                                          |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Which module finds interesting files? | `discovery/info_disclosure/interesting_files`                                                                                   |
| How do you install it?                | `marketplace install discovery/info_disclosure/interesting_files`                                                               |
| How do you load it?                   | `modules load discovery/info_disclosure/interesting_files`                                                                      |
| Required option                       | `SOURCE`                                                                                                                        |
| Example target                        | `hackxor.net`                                                                                                                   |
| Command to set the target             | `options set SOURCE hackxor.net`                                                                                                |
| Command to execute the module         | `run`                                                                                                                           |
| Where is the report stored?           | A CSV file in Recon-ng's `data` directory (for example, `~/.recon-ng/data/` or `recon-ng/data/`, depending on the installation) |
| What does the report contain?         | URLs of publicly indexed files (such as PDF, DOCX, XLSX, PPTX) that may be useful during authorized reconnaissance.             |


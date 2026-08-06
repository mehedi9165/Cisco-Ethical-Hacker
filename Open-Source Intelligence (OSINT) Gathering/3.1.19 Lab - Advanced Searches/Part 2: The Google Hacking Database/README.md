This lab introduces the **Google Hacking Database (GHDB)**, a curated collection of **Google search queries (Google dorks)** created by the security community. The purpose of the lab is to understand how public information can be discovered using advanced search operators and why organizations should be careful about what they expose online.

> **Important:** Use GHDB only for learning and authorized security work. The examples below focus on understanding search results and publicly available information, not exploiting systems or accessing anything without permission.

---

# Lab Objective

After completing this lab, you will be able to:

* Understand what the Google Hacking Database (GHDB) is.
* Browse and search the GHDB.
* Understand the information provided for each dork.
* Understand what types of public information different dorks can reveal.
* Explain why publicly exposed information may create security risks.

---

# What is GHDB?

The **Google Hacking Database (GHDB)** is a searchable database of Google search queries (called **dorks**) that help locate specific types of publicly indexed information.

These searches are useful for:

* Security professionals
* Penetration testers
* OSINT investigators
* Security auditors

They are also useful to help organizations discover information they may have unintentionally exposed to search engines.

---

# Step 1: Open the Google Hacking Database

Open your browser.

Go to Google.

Search for:

```text
GHDB
```

Open the **Google Hacking Database** website (maintained by Exploit Database).

The home page contains:

```
Google Hacking Database

Search

Categories

Authors

Filters
```

---

# Step 2: Explore Filters

Click

```
Filters
```

You will see options such as:

* Categories
* Author
* Quick Search

Example categories may include:

* Footholds
* Files Containing Passwords
* Login Portals
* Vulnerable Files
* Sensitive Directories
* Network or Vulnerability Data

Each category groups dorks by purpose.

---

# Step 3: Browse Categories

Select different categories and observe the listed dorks.

For each dork, GHDB typically shows:

* GHDB ID
* Author
* Publication date
* Short description
* A Google search link

---

## Lab Question

**What information is provided about the Dorks?**

**Answer:**

> Each entry typically includes the GHDB ID number, the author, the publication date, a brief description of the dork's purpose, and a link that opens the corresponding Google search.

---

# Step 4: Use Quick Search

Use the **Quick Search** box to search for a keyword.

For example:

```text
tsweb
```

The results display dorks related to that keyword.

Click one of the results to view its description.

---

# Step 5: Understand the `allinurl:tsweb/default.htm` Dork

One example in your lab is:

```text
allinurl:tsweb/default.htm
```

This search is designed to locate pages whose URL contains `tsweb/default.htm`.

Historically, this path was associated with **Microsoft Terminal Services Web Connection** pages.

The purpose of the lab is to recognize what type of page the search identifies—not to interact with or attempt to access those systems.

---

## Lab Question

**What does this Dork return?**

**Answer:**

> It returns publicly indexed pages related to Microsoft Terminal Services Web Connection (Remote Desktop web connection) login pages.

---

# Step 6: Understanding the Screenshot

The screenshot you uploaded shows a **Windows 2000 Terminal Services Web Connection** login page.

It contains fields such as:

* Server
* User name
* Domain
* Connect button

It also clearly identifies:

```
Microsoft Windows 2000
Terminal Services Web Connection
```

This demonstrates why public information can matter.

From this page alone, someone can infer that:

* The service is Microsoft Terminal Services.
* The interface is associated with Windows 2000.
* The page is a remote desktop web connection.

A security professional reviewing such exposure might recommend updating or replacing legacy systems if they are still in use.

---

# Why Is This Important?

Suppose a company publicly exposes:

```
Windows 2000 Terminal Services
```

A security assessor may conclude:

* The software appears to be very old.
* Legacy software often lacks vendor support.
* The organization should verify whether the system is still needed and ensure it is properly secured or retired.

This is an example of how publicly visible information can inform a security review.

---

# Step 7: Combine Categories with Search

The lab asks you to choose the category:

```
Files Containing Passwords
```

Then search:

```
db_pass
```

This shows examples of search queries intended to locate publicly indexed files that may contain database password references.

The educational goal is to understand that sensitive files should **not** be publicly indexed and that organizations should regularly audit their public exposure.

---

# Step 8: Read the Description

Click a dork.

Example information:

```
GHDB ID:
1234

Author:
John Smith

Published:
2018

Description:
Searches for publicly indexed files matching a particular pattern.
```

Notice that GHDB explains:

* what the search looks for,
* why it may be interesting from a security perspective.

---

# Step 9: Why Security Professionals Use GHDB

Organizations use GHDB to:

* Find unintentionally exposed documents.
* Locate publicly indexed login pages.
* Review public configuration files.
* Identify legacy web applications.
* Verify what search engines can see about their infrastructure.

This helps them reduce unnecessary public exposure.

---

# Example Workflow

```
Open Google
      │
      ▼
Search GHDB
      │
      ▼
Open Google Hacking Database
      │
      ▼
Browse Categories
      │
      ▼
Read Dork Descriptions
      │
      ▼
Use Quick Search
      │
      ▼
Review Public Search Results
      │
      ▼
Identify Publicly Exposed Information
      │
      ▼
Recommend Security Improvements
```

---

# Lab Questions and Answers

### Question 1

**What information is provided about the Dorks?**

**Answer:**

> The GHDB ID number, author, publication date, a brief description of the dork, and a link to perform the corresponding Google search.

---

### Question 2

**What does the `allinurl:tsweb/default.htm` dork return?**

**Answer:**

> It returns publicly indexed Microsoft Terminal Services (Remote Desktop Web Connection) login pages.

---

# Best Practices

From a defensive perspective, organizations should:

* Regularly review what search engines index about their public websites.
* Remove or restrict indexing of pages that are not intended for public discovery.
* Keep systems updated and retire unsupported software.
* Review publicly accessible documents for unnecessary metadata or sensitive information before publishing.


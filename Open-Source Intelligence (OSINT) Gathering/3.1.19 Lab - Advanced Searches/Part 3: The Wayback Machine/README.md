# Lab: The Wayback Machine (Internet Archive)

## Objective

This lab teaches you how to use the **Wayback Machine** to perform **passive reconnaissance (OSINT)** by exploring archived versions of websites. The goal is to understand what information has been publicly available over time and why organizations should review their historical online exposure.

> **Important:** Use the Wayback Machine only to view publicly archived pages. Do not attempt to access systems, hidden resources, or anything you are not authorized to investigate.

---

# What is the Wayback Machine?

The **Wayback Machine** is an Internet archive maintained by the Internet Archive.

Website:

```text
https://web.archive.org
```

It periodically saves copies (snapshots) of public websites.

For example:

```text
www.example.com
```

may have snapshots from:

* 2018
* 2019
* 2020
* 2021
* 2022
* 2023
* 2024

This lets you see what the website looked like in the past.

---

# Why is it useful?

Organizations change their websites over time.

Old websites may have contained:

* Old employee information
* Phone numbers
* Email addresses
* Old documents
* Software version information
* Company history
* Product names
* Public announcements
* Older navigation structures

Security professionals can use archived pages to understand an organization's historical public footprint and recommend improvements where necessary.

---

# Step 1: Open the Wayback Machine

Open Firefox or Chrome.

Go to:

```text
https://web.archive.org
```

You will see:

```text
Wayback Machine

Search URL
______________________
```

---

# Step 2: Search a Website

Enter a public website.

Example:

```text
example.com
```

Click

```text
Browse History
```

The Wayback Machine loads the archived history.

---

# Step 3: Explore the Calendar Tab

The Calendar tab is usually selected automatically.

You will see:

```
Years

2019

2020

2021

2022

2023

2024
```

Below that is a calendar.

Example:

```
July 2023

1

2

3

●

5

6

●

9

```

The dots indicate archived snapshots.

---

# Step 4: Open a Snapshot

Click a year.

Example:

```
2021
```

Click a highlighted date.

Example:

```
August 15
```

If multiple snapshots exist that day:

```
09:05

11:42

17:10
```

Click one.

The archived webpage opens.

Example:

```
Example Company

Products

Support

About

Careers
```

---

# Step 5: Browse the Old Website

Some archived pages remain partially navigable.

You might notice:

Old logo

↓

Old products

↓

Old contact page

↓

Old news

↓

Old employee list

↓

Old documentation

These are historical copies and may no longer reflect the current website.

---

# Lab Question

**How can it be advantageous for a hacker to collect information from an archived site?**

### Answer

> Archived websites can reveal historical public information such as previous contact details, employee names, organizational structure, product information, or documents that are no longer visible on the current website. This historical context can help security professionals understand an organization's past public exposure and can also inform social engineering awareness and defensive improvements.

---

# Step 6: Explore the Collections Tab

Click

```
Collections
```

You will see various archive collections.

Example:

```
Internet Archive

Archive-It

Other Collections
```

These indicate which archive captured the page.

Click one.

You can learn:

* Who maintains it
* What it archives
* When it collected data

---

# Step 7: Explore the Changes Tab

Click

```
Changes
```

This shows how much a page changed between captures.

Example colors:

```
Grey

↓

Little change

Blue

↓

Large change
```

Select two snapshots.

Example:

```
January 2021

↓

December 2023
```

Click

```
Compare
```

The differences are highlighted.

Example:

Old page:

```
Products

Support

About
```

New page:

```
Products

Support

Cloud Services

Careers
```

This makes it easy to identify how a website evolved over time.

---

# Step 8: Explore the Summary Tab

Click

```
Summary
```

Unlike the Calendar view (which focuses on one page), the Summary provides information about the **entire domain**.

It shows content types (MIME types), such as:

* Text
* Images
* JavaScript
* CSS
* PDFs

You can adjust:

```
Year Start

↓

2018

Year End

↓

2024
```

You can also filter by:

```
All

Text

Application

Image

Audio

Video
```

This helps you see what kinds of content were archived over time.

---

# Step 9: Explore the Site Map Tab

Click

```
Site Map
```

You'll see a graphical representation of the site's structure.

The center represents the homepage.

Each ring moving outward represents additional pages or sections.

For example:

```
Home
│
├── About
├── Products
├── Contact
└── Careers
```

Over the years, you may notice the structure becoming more or less complex.

---

# Step 10: Explore the URLs Tab

Click

```
URLs
```

This lists archived URLs for the domain.

Use the filter box to search for file types or paths.

Examples include:

```
.pdf
```

to find archived PDF documents,

```
.csv
```

to identify archived CSV files,

or

```
/api/
```

to locate archived URLs that include `/api/`.

The lab also suggests experimenting with filters such as:

* `.zip`
* `.backup`
* `.config`
* `/admin/`

The educational goal is to understand how archived URLs are organized and how organizations can review their historical public exposure.

---

# Example Walkthrough

Suppose you search:

```
example.com
```

You observe:

### Calendar

```
2019

2020

2021

2022

2023
```

### Snapshot

```
March 2021
```

The archived homepage includes:

```
About

Services

Support

Careers
```

### Summary

```
Images

PDFs

JavaScript

CSS
```

### URLs

You filter for:

```
.pdf
```

The archive lists several publicly archived PDF documents associated with the domain.

---

# Workflow Summary

```
Open web.archive.org
          │
          ▼
Search Website
          │
          ▼
Calendar
          │
          ▼
Open Old Snapshot
          │
          ▼
Browse Archived Pages
          │
          ▼
Collections
          │
          ▼
Changes
          │
          ▼
Summary
          │
          ▼
Site Map
          │
          ▼
URLs
          │
          ▼
Document Findings
```

---

# Lab Questions and Answers

## Question 1

**How can it be advantageous for a hacker to collect information from an archived site?**

**Answer:**

> Archived websites may contain historical public information that is no longer visible on the current site, such as employee names, contact information, organizational history, product information, or publicly shared documents. Reviewing this information can help organizations understand and reduce their historical public exposure, and it can provide useful context for security awareness and defensive planning.

---

## Key Learning Points

| Feature         | Purpose                                                |
| --------------- | ------------------------------------------------------ |
| **Calendar**    | View snapshots from different dates                    |
| **Collections** | See which archives captured the site                   |
| **Changes**     | Compare how pages changed over time                    |
| **Summary**     | View domain-wide content types and archive statistics  |
| **Site Map**    | Explore the historical structure of the website        |
| **URLs**        | Browse archived URLs and filter by extensions or paths |


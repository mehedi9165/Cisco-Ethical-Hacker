This lab teaches you how to use **Google Advanced Search Operators (Google Dorking)** for **passive reconnaissance (OSINT)**. The objective is to find **publicly indexed information** more efficiently—not to access systems or bypass security. The examples below use benign topics and publicly available content.

---

# Lab Objective

You will learn:

* What Google Dorking is
* How to use common Google search operators
* How each operator works
* How to combine operators
* How these searches support passive reconnaissance
* How to answer the lab questions

---

# What is Google Dorking?

Normally, people search Google like this:

```text
ethical hacker
```

This returns millions of results.

Google Dorking uses **search operators** to narrow the results.

For example:

```text
ethical hacker site:pearson.com
```

Instead of searching the whole Internet, Google only searches the **pearson.com** domain.

---

# Step 1: Open Google

Open your browser.

Visit:

```text
https://www.google.com
```

---

# Step 2: Normal Search

Type:

```text
ethical hacker
```

Press **Enter**.

You may see many different types of results, such as:

* Articles
* Courses
* Videos
* News
* Books
* Blogs

This demonstrates that a simple keyword search can return many unrelated results.

---

# Step 3: Use the `site:` Operator

The `site:` operator restricts results to a specific website.

Search:

```text
ethical hacker site:pearson.com
```

Google searches only within **pearson.com**.

Example results:

```text
Pearson Ethical Hacking Book

Ethical Hacking Certification

Cybersecurity Articles
```

### Lab Question

**What do all the results have in common?**

**Answer:**

> They all come from the `pearson.com` website and relate to ethical hacking.

---

# Step 4: Use the `filetype:` Operator

The `filetype:` operator limits results to a specific file format.

Search:

```text
ethical hacker site:pearson.com filetype:pdf
```

Now Google returns only PDF files.

Example:

```text
Ethical Hacking.pdf

Cybersecurity.pdf

Network Security.pdf
```

### Lab Question

**What file type is opened by each result?**

**Answer:**

> All the results are PDF files.

---

# Step 5: Use the `intitle:` Operator

This operator finds pages with a specific word in the page title.

Search:

```text
ethical hacker intitle:certification
```

Possible results:

```text
Ethical Hacker Certification

Top Ethical Hacking Certifications

Certification Guide
```

All returned pages contain the word **certification** in the title.

---

# Step 6: Use the `inurl:` Operator

This searches for a word within the page URL.

Search:

```text
ethical hacker inurl:free
```

Example URLs:

```text
example.com/free-course

website.com/free-training

learn.com/free-certification
```

All returned URLs contain the word **free**.

---

# Step 7: Use `allintext:`

This operator requires all search terms to appear in the page text.

Search:

```text
allintext:free ethical hacker practice test questions
```

Google returns pages where all of these words appear in the content.

You can also use quotation marks to search for an exact phrase:

```text
allintext:"ethical hacker practice test"
```

---

# Summary of Operators

| Operator     | Purpose              | Example                                |
| ------------ | -------------------- | -------------------------------------- |
| `site:`      | Search one website   | `site:pearson.com ethical hacker`      |
| `filetype:`  | Search one file type | `filetype:pdf cybersecurity`           |
| `intitle:`   | Search page titles   | `intitle:certification ethical hacker` |
| `inurl:`     | Search URLs          | `inurl:training cybersecurity`         |
| `allintext:` | Search page content  | `allintext:ethical hacker practice`    |

---

# Step 8: Use Google's Advanced Search Form

Instead of typing operators, Google also provides an Advanced Search page.

Search Google for:

```text
advanced search
```

Open the **Google Advanced Search** page.

You can specify:

* Words to include
* Exact phrases
* Language
* Region
* Last update
* Site or domain
* File type

It performs the same filtering as the operators above.

---

# Step 9: Passive Reconnaissance

Passive reconnaissance means collecting **publicly available** information without interacting with the target's systems.

Examples of benign searches include looking for:

* Public documentation
* Public presentations
* Public job postings
* Public press releases

This information can help defenders understand what an organization has intentionally made public.

---

# Step 10: Search for Public PDFs

Suppose you are researching a company's publicly available documentation.

Search:

```text
site:example.com filetype:pdf
```

Possible results:

```text
Annual Report

Product Brochure

User Manual

White Paper
```

These are documents intentionally indexed by search engines.

---

# Step 11: Search Public Employee-Related Documents

Search:

```text
site:example.com intext:employee filetype:pdf
```

Possible results:

```text
Employee Handbook

Benefits Guide

Training Manual
```

Review only information that is intentionally public. Do not attempt to access restricted documents.

---

# Step 12: Search LinkedIn

Search:

```text
site:linkedin.com "Example Company"
```

Possible results:

```text
Software Engineer

HR Manager

Cybersecurity Analyst

Sales Manager
```

This can show publicly available professional profiles.

---

# Lab Question

**What type of information could a hacker gain from this type of dork?**

A suitable answer is:

> Public search results may reveal employee names, job titles, department information, publicly shared professional profiles, company technologies mentioned in job postings, and other information intentionally published online. Such information can help defenders understand their public exposure and improve security awareness.

---

# Try Other Social Media

You can also search for public mentions on other platforms.

Examples:

```text
site:x.com "Example Company"
```

```text
site:facebook.com "Example Company"
```

```text
site:github.com "Example Company"
```

These searches return only publicly indexed pages.

---

# Workflow Summary

```text
Open Google
      │
      ▼
Normal Search
      │
      ▼
Use site:
      │
      ▼
Use filetype:
      │
      ▼
Use intitle:
      │
      ▼
Use inurl:
      │
      ▼
Use allintext:
      │
      ▼
Try Advanced Search Form
      │
      ▼
Review Public Results
      │
      ▼
Document Findings
```

# Example Lab Answers

### Question 1

**What do all the results have in common?**

**Answer:**

> They all contain information related to ethical hacking from the `pearson.com` website.

---

### Question 2

**What file type is opened by each result?**

**Answer:**

> All of the returned files are PDF documents.

---

### Question 3

**What type of information could a hacker gain from this type of dork?**

**Answer:**

> Publicly available information such as employee names, job titles, company technologies mentioned in job postings, publicly shared documents, and organizational information. Security teams can use the same techniques to identify and reduce unnecessary public exposure.

## Understanding the Table in Your Screenshot

The table summarizes the operators used in this lab:

| Operator     | What it Does                               | Example                                |
| ------------ | ------------------------------------------ | -------------------------------------- |
| `allintext:` | All keywords must appear in the page text. | `allintext:network security course`    |
| `filetype:`  | Limits results to a specific file format.  | `filetype:pdf cybersecurity`           |
| `intitle:`   | Requires a word in the page title.         | `intitle:certification ethical hacker` |
| `inurl:`     | Requires a word in the URL.                | `inurl:training cybersecurity`         |
| `site:`      | Restricts results to one domain.           | `site:pearson.com ethical hacker`      |

These operators are widely used for legitimate research and OSINT because they help narrow search results to relevant, publicly available information. Always use them responsibly and avoid attempting to access content that is not intended to be public.

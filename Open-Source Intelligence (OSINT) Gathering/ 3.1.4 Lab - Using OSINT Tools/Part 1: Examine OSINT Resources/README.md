This lab is about OSINT (Open-Source Intelligence). The goal is to learn how investigators and penetration testers gather publicly available information about people, usernames, companies, and online accounts. You are not hacking anything—you are searching information that is already publicly available.

I’ll explain every step in detail with examples, exactly like you would perform it in a lab.

### What is OSINT?

OSINT = Open-Source Intelligence

It means collecting information from public sources such as:

* Websites

* Social media

* GitHub

* Forums

* Public documents

* Search engines

* News articles

Penetration testers use OSINT during the Reconnaissance phase before any security testing begins.

### Step 1: Open the OSINT Framework

Go to:

[https://osintframework.com/](https://osintframework.com/)

You will see a page that looks like a large tree diagram.

![Open Source Intelligence: What Is OSINT & How Does It Work?](https://images.openai.com/static-rsc-4/cX07_BIa31Ta_wgBV1WxohvL17EjBXHgEuQjRNz-RXRjnvfQpEoi0_WbqgN8xJscFL4ACESqA5dHrQ6JomdTZ_09UFgPlsWCUtFJ6zERFgSLbJrvNZctWzCy5HS-lu7KbebWudyeLwzVaipuXxx9Wu9gPG4gc3TvW7KKCXC93F4UHKn8oU_rpahz_YdhyrPP?purpose=fullsize)

The categories are arranged vertically.

Examples:

* Username

* Email Address

* Domain Name

* IP Address

* Social Networks

* Search Engines

* Public Records

* Images

* Documents

Think of it as a map of OSINT tools.

### Step 2: Explore the Username Category

Click:

```
Username
```

Two subcategories appear:

```
Username
│
├── Username Search Engines
└── Username Checkers
```

Click each one.

You will see many tools.

One of them is:

```
WhatsMyName (T)
```

The (T) usually indicates a tool-related resource in the framework.

### Step 3: Open WhatsMyName

Click:

```
WhatsMyName (T)
```

You are taken to a GitHub repository.

Scroll down to the README.md section.

You will see links to implementations of WhatsMyName.

Click:

[https://whatsmyname.app/](https://whatsmyname.app/)

This opens the free web version.

![How recruiters find your profile online](https://images.openai.com/static-rsc-4/3WoVGQnN1yOJAqdHQPcqw8tyNyaX1vZfVbcW22dm_-g24Bwtkeh-XFTyggGUmRYJblJY6brkvlpRna8oCaWvjaWiaLS0T0F2KMdKss7xMK6E3pieEj95dxmTanBqb4xE1FXplQQ0Kyk9Iuiwz74I9kOLyGINFlaFRs3Mzn5fw_iKwf0vRm2zpuTlE1WmDN0E?purpose=fullsize)

### Step 4: Search for Usernames

In the search box, enter one or more usernames.

Example:

```
johnsmith
rakib123
cyberstudent
```

Each username should be on a separate line.

Example:

```
johnsmith
rakib123
cyberstudent
```

Now click the green magnifying glass.

The tool checks many websites such as:

* GitHub

* Reddit

* Instagram

* TikTok

* Pinterest

* Steam

* Medium

* Other public platforms

### Step 5: Understand the Results

Example results:

| Website   | Status    |
| --------- | --------- |
| GitHub    | Found     |
| Reddit    | Found     |
| Instagram | Not Found |
| Medium    | Found     |
| TikTok    | Not Found |

If a username is found, you can click the profile link.

For example:

```
https://github.com/johnsmith
```

This might show:

* Public repositories

* Bio

* Location

* Technologies used

### Why is this useful?

Suppose an employee uses the same username everywhere.

Example:

```
Company Email:
john.smith@company.com

GitHub Username:
johnsmith

Reddit Username:
johnsmith

Gaming Username:
johnsmith
```

An attacker could connect these accounts together and learn:

* Interests

* Hobbies

* Technical skills

* Publicly shared code

* Personal information

This helps build a social engineering profile.

### Lab Question

### What is the value of doing username searches and account enumeration?

A good answer:

Username searching helps identify public accounts associated with an individual. These accounts may reveal personal interests, professional information, technologies used, and other details that could assist in OSINT investigations or social engineering awareness. Organizations can use the same process to understand and reduce unnecessary public exposure.

### Step 6: Export the Results

WhatsMyName allows you to export:

* CSV

* PDF

Example CSV:

| Website | Username  |
| ------- | --------- |
| GitHub  | johnsmith |
| Reddit  | johnsmith |
| Medium  | johnsmith |

This is useful for reporting in a penetration test.

### Step 7: Open SMART (Start Me Aggregated Resource Tool)

Go to:

[https://smart.myosint.training/](https://smart.myosint.training/)

![Discover start.me OSINT USA: A Treasure Trove of Free Tools | Amish Patel posted on the topic | LinkedIn](https://images.openai.com/static-rsc-4/8L2O6ApenSsJaH101qUZcXZ54bbLGaiErC9rwM7vdBueSgBcEarM8VlxZxn4s0umQ9ppSlEWgy7UT1xsC57p7VmM7TlvWlrnSOALQam2SowGL6CEOWQeuWiBzzWsdvCsDCTbF47SEmR98wubpmE5PcPgC2KZweRnx1B_9WWrWoQ3xTxwR-RJ1-lefIdEVhl0?purpose=fullsize)

This site searches OSINT-related bookmarks shared by researchers.

Think of it as a search engine for OSINT tools.

### Step 8: Search for “usernames”

Type:

```
usernames
```

Press Search.

You may see results such as:

* Username Search

* Social Media Username Finder

* Account Enumeration Tools

* Reverse Username Lookup

These are links collected from the OSINT community.

### Step 9: Open Some Resources

Click a few links.

For example:

```
Namechk
```

or

```
Sherlock
```

Read what they do.

Some tools check username availability across hundreds of sites.

Important: Since these are community-shared links, only visit reputable resources and avoid downloading unknown software.

### Step 10: Search Other Categories

Try searching:

```
email
```

Results may include:

* Email verification tools

* Breach search tools

* Email OSINT resources

Try:

```
domain
```

Results may include:

* WHOIS tools

* DNS lookup tools

* Subdomain discovery resources

Try:

```
social media
```

Results may include:

* Facebook OSINT

* LinkedIn search techniques

* Twitter/X analysis tools

### Example: Investigating a Company

Suppose the target company is:

```
example.com
```

### Step A – Find email patterns

Use:

* Hunter.io

* Email-format tools

You discover:

```
firstname.lastname@example.com
```

### Step B – Enumerate usernames

From public GitHub commits you find:

```
jdoe
```

### Step C – Search with WhatsMyName

Search:

```
jdoe
```

Results:

| Site   | Found |
| ------ | ----- |
| GitHub | Yes   |
| Reddit | Yes   |
| Medium | Yes   |

### Step D – Review public information

GitHub shows:

* Python projects

* Docker

* Kubernetes

This suggests the employee works with cloud technologies.

Again, this is public information only.

### How SpiderFoot Fits In

In a real penetration test:

```
Company Domain
        │
        ▼
SpiderFoot
        │
        ▼
Email Addresses
        │
        ▼
Usernames
        │
        ▼
WhatsMyName
        │
        ▼
Public Accounts
        │
        ▼
OSINT Report
```

SpiderFoot automates the collection of emails and usernames, while WhatsMyName helps investigate where those usernames appear publicly.

### Workflow Summary

```
Open OSINT Framework
        │
        ▼
Click Username
        │
        ▼
Open WhatsMyName
        │
        ▼
Enter usernames
        │
        ▼
Review public accounts
        │
        ▼
Export CSV/PDF
        │
        ▼
Open SMART
        │
        ▼
Search for additional OSINT tools
        │
        ▼
Document findings
```

### Final Answers for the Lab

### Question 1

What is the value of doing username searches and account enumeration?

Answer:

Username searches help identify publicly available accounts associated with an individual across multiple websites. These accounts may reveal professional information, interests, technologies used, and other details that can support OSINT investigations, security assessments, or social engineering awareness. Organizations can use this information to understand and reduce unnecessary public exposure.

### Key Learning Points

Framework

OSINT Framework

A categorized map of OSINT resources and investigation areas.

Username

WhatsMyName

Checks whether a username appears on many public websites and services.

Discovery

SMART

Finds additional OSINT tools and bookmarked resources shared by researchers.

Recon

Passive reconnaissance

Collects publicly available information without interacting with the target's systems.

For your MSc cybersecurity studies and future PNPT/OSCP preparation, these username-enumeration and OSINT techniques are foundational skills because they teach you how attackers gather information before any technical attack begins.

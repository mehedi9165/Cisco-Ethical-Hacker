This lab is about **Open-Source Intelligence (OSINT)**. The goal is to determine whether an email address has appeared in any **publicly disclosed data breaches**. You are **not** exploiting anything or accessing unauthorized data—you are only searching publicly available breach databases.

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

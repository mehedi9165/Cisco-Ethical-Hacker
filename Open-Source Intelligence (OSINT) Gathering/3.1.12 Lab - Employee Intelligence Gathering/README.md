# Lab: Gather Information Through Social Media (OSINT)

## Objective

In this lab, you will learn how attackers perform **Open-Source Intelligence (OSINT)** using publicly available information on social media. The goal is to understand what information is visible to strangers and how it could be abused, **without attempting to access private accounts or perform any unauthorized actions**.

> **Purpose:** Learn how to protect your own digital footprint by viewing it from an attacker's perspective.

---

# Part 1: Gather Information Through Social Media

## Step 1: Conduct the Investigation

### Scenario

Suppose your name is:

```
John Smith
```

You also use these usernames:

```
johnsmith
john.smith
jsmith1998
```

Instead of using your normal browser, open an **Incognito/Private Window**.

### Why use Incognito Mode?

Incognito mode helps because:

* You are not logged into your accounts.
* Search results are less personalized.
* You see what a stranger is more likely to see.

---

## Step 2: Open an Incognito Window

### Google Chrome

Open Chrome

Press

```
Ctrl + Shift + N
```

or

Click

```
⋮
New Incognito Window
```

---

### Firefox

Press

```
Ctrl + Shift + P
```

---

### Edge

Press

```
Ctrl + Shift + N
```

---

## Step 3: Search Your Name

Search several variations.

Example:

```
John Smith
```

Then

```
John A Smith
```

Then

```
John Smith New York
```

Then

```
johnsmith
```

Then

```
"John Smith"
```

(Quotation marks search the exact phrase.)

---

## Step 4: Search Different Search Engines

Use more than one search engine.

Examples

Google

```
John Smith
```

Bing

```
John Smith
```

DuckDuckGo

```
John Smith
```

Yahoo

```
John Smith
```

Different search engines may index different websites.

---

## Step 5: Search Your Usernames

Many people reuse usernames.

Search:

```
johnsmith
```

```
"johnsmith"
```

```
jsmith1998
```

You may discover:

* GitHub profile
* Reddit posts
* Twitter/X account
* Instagram
* LinkedIn
* Gaming profiles
* Forums

---

## Step 6: Search Your Email (if appropriate)

Example

```
"john@gmail.com"
```

Sometimes public websites expose email addresses.

Possible findings

* Old forum posts
* Public resumes
* Contact pages

---

## Step 7: Check Every Social Media Platform

Visit each platform.

Examples

* Facebook
* LinkedIn
* Instagram
* X (Twitter)
* TikTok
* GitHub
* Reddit
* Pinterest
* Discord (public profiles)
* YouTube

---

## Step 8: View Your Profile as a Stranger

If possible:

Create another test account (following the platform's terms of service), or use the platform's "View As Public" feature if available.

Now ask:

> What can someone who doesn't know me see?

---

# Things to Check

## 1. Profile Information

Example

```
Name:
John Smith

Birthday:
May 12

Lives:
Chicago

Works:
ABC Company

School:
XYZ University

Phone:
Visible

Email:
Visible
```

Ask:

Is all of this really necessary?

---

## 2. Profile Picture

Can someone identify

* workplace?
* military uniform?
* company badge?
* vehicle plate?
* children?
* pets?

---

## 3. Cover Photo

Sometimes people post

* vacation
* home
* expensive car
* office

These reveal useful information.

---

## 4. Posts

Look for

```
Today is my birthday!
```

```
Started working at XYZ Bank.
```

```
Vacation starts tomorrow!
```

```
Finally bought a BMW.
```

Attackers love this information.

---

## 5. Status Updates

Example

```
Going to Dubai for 10 days!
```

Potential risk:

* Indicates the home may be unoccupied.
* Can be used in targeted scams.

---

## 6. Employment Information

Example

```
Cybersecurity Engineer
ABC Bank
```

Possible attacker use:

* Craft convincing phishing emails.
* Impersonate company IT staff.

---

## 7. Education

Example

```
Graduated from Harvard
```

Security question risk:

```
Which university did you attend?
```

---

## 8. Family Members

Example

```
My wife Sarah
```

```
Happy Birthday Mom!
```

```
Love you Dad!
```

Many security questions involve relatives' names.

---

## 9. Pets

Example

```
Happy birthday Bella!
```

Many people use pet names as passwords or password recovery answers.

---

## 10. Hobbies

Example

```
Photography
Gaming
Fishing
Football
```

Attackers may send hobby-related phishing messages.

Example:

```
Free FIFA World Cup Tickets
```

---

## 11. Location Check-ins

Example

```
Checked in at New York Airport
```

```
Checked in at Hilton Hotel
```

This reveals travel patterns.

---

## 12. Friends List

Can everyone see

* coworkers?
* family?
* military colleagues?

These relationships can aid social engineering.

---

## 13. Tagged Photos

Sometimes friends reveal information you never intended to share.

Example

```
Office party
```

```
Company meeting
```

```
Family gathering
```

---

## 14. Comments

Example

```
Can't wait to start my new job Monday!
```

Attackers now know:

* employer
* start date

---

## 15. Groups

Public groups reveal interests.

Example

```
Cisco Engineers
```

```
Kubernetes Bangladesh
```

```
Crypto Investors
```

This helps attackers tailor scams.

---

# Example Investigation

Suppose you searched yourself.

You found

| Information     | Visible |
| --------------- | ------- |
| Full Name       | ✅       |
| Birthday        | ✅       |
| Employer        | ✅       |
| LinkedIn        | ✅       |
| Instagram       | ✅       |
| GitHub          | ✅       |
| Public Email    | ❌       |
| Phone           | ❌       |
| Family Photos   | ✅       |
| Vacation Photos | ✅       |
| Home City       | ✅       |

---

# Question 1

## What type of information about you was revealed?

### Sample Answer

> My social media investigation revealed my full name, profile photos, employment information, education history, hometown, hobbies, family members, vacation photos, public comments, and connections with friends and coworkers. This information could be used by attackers to create convincing phishing emails, impersonate trusted contacts, answer security questions, or perform social engineering attacks.

---

# Part 2: Reflect on Your Findings

## Question 1(a)

### How could a cybercriminal use workplace information?

Example

Suppose your profile says

```
Microsoft Employee
```

An attacker sends

```
Subject:
Microsoft Password Reset

Dear Employee,

Click here immediately...
```

Because the email mentions your employer, you are more likely to trust it.

### Answer

> Workplace information can be used to launch targeted phishing (spear phishing) or business email compromise attacks by impersonating coworkers, IT staff, or company services.

---

## Question 1(b)

### How could hobbies be abused?

Suppose you love football.

The attacker sends

```
Free FIFA Tickets
```

or

```
Exclusive Manchester United Jersey
```

Victims are more likely to click.

### Answer

> A cybercriminal can use hobbies and interests to create personalized phishing messages, fake promotions, or malicious websites that appear relevant and trustworthy.

---

## Question 1(c)

### How can public conversations be abused?

Example

```
Friend:
Happy Birthday!

You:
Thanks!

Friend:
See you at our old school reunion.
```

Now attackers know

* birthday
* school
* close friends

These details can support identity theft or social engineering.

### Answer

> Public conversations can reveal relationships, birthdays, travel plans, and other personal details that attackers may use for impersonation, password guessing, or convincing social engineering attacks.

---

# Question 2

## Social Media Best Practices

### Sample Answer

> To protect my identity, I would:
>
> * Set all social media accounts to private where appropriate.
> * Avoid posting sensitive personal information such as my address, phone number, or full birth date.
> * Disable location sharing and avoid real-time check-ins.
> * Review tagged photos and remove unwanted tags.
> * Accept friend requests only from people I know.
> * Use strong, unique passwords and enable multi-factor authentication (MFA).
> * Regularly review privacy settings and remove old public posts.
> * Think carefully before sharing information about work, family, travel, or finances.

---

# Real-World Example

Imagine an attacker learns the following from public profiles:

* Name: John Smith
* Employer: ABC Bank
* Pet: Bella
* Birthday: May 12
* Favorite football club: Liverpool
* Traveling next week

The attacker could send:

> "Hello John, as an ABC Bank employee and Liverpool supporter, you've won VIP match tickets. Click here to claim them."

Because the message uses publicly available personal details, it appears more convincing—even though it is a phishing attempt.

---

# Final Lab Answers

**1. What type of information about you was revealed?**

> My social media profiles revealed personal information such as my full name, profile photos, employment history, education, hometown, hobbies, family relationships, travel activities, and public interactions. This information could be useful for phishing, impersonation, or social engineering attacks.

**2(a). Workplace information**

> Workplace information can be used to create targeted phishing emails, impersonate colleagues or IT staff, and gather intelligence about the organization.

**2(b). Hobbies and interests**

> Hobbies and interests can be used to craft convincing scams, fake promotions, or malicious links that match the victim's preferences.

**2(c). Public conversations**

> Public conversations can reveal relationships, personal details, travel plans, and information that may help attackers guess passwords or answer security questions.

**3. Social media best practices**

> Keep accounts private, limit personal information shared online, avoid real-time location sharing, regularly review privacy settings, enable MFA, use strong unique passwords, and think carefully before posting personal or work-related information.

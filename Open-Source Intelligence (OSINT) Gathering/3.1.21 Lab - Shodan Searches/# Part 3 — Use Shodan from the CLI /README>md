# Part 3 — Use Shodan from the CLI

This part moves from the **Shodan web interface** to the **Shodan command-line interface (CLI)** in Kali Linux.

You'll learn how to:

1. Check that Shodan CLI is installed
2. Initialize it with your API key
3. Display available commands
4. Perform a search
5. Check your Shodan credits
6. Find your public IP
7. Generate search statistics

> **Safety:** Shodan searches are passive database queries. Don't use returned IP addresses to log in, exploit, or otherwise interact with systems you don't own or have permission to test.

---

# Part 1 — Prepare Kali

Start your Kali VM.

Open:

```text
Applications
   ↓
Terminal
```

You should see something like:

```text
┌──(kali㉿kali)-[~]
└─$
```

You don't need to be `root` for normal Shodan CLI use.

---

# Step 1 — Check Shodan CLI

Run:

```bash
shodan -h
```

If it is installed, you'll see something similar to:

```text
Usage: shodan [OPTIONS] COMMAND [ARGS]...

Options:
  -h, --help
  --version

Commands:
  alert
  convert
  count
  data
  domain
  download
  honeypots
  info
  init
  myip
  parse
  search
  stats
```

The exact command list can vary with the installed Shodan CLI version.

---

# Step 2 — Get Your API Key

Log into:

[Shodan](https://www.shodan.io/)

Then go to:

```text
Account
   ↓
Overview
```

You'll find your API key.

It will look approximately like:

```text
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

### Important

**Don't send your API key to me or put it in a screenshot/GitHub repository.**

Treat it like a password.

---

# Step 3 — Initialize Shodan

In Kali:

```bash
shodan init YOUR_API_KEY
```

For example:

```bash
shodan init abcdef1234567890xxxxxxxx
```

Don't use that example literally.

If successful, you should see:

```text
Successfully initialized
```

This stores your API key in your local Shodan CLI configuration.

---

# Step 4 — Verify the Configuration

Run:

```bash
shodan info
```

You should get account information such as:

```text
Query credits available: ...
Scan credits available: ...
```

The exact numbers depend on your account.

---

# Step 5 — Display the Help Menu

Run:

```bash
shodan -h
```

This is useful because Shodan CLI functionality can change between versions.

You can also obtain help for an individual command.

For example:

```bash
shodan search --help
```

or:

```bash
shodan stats --help
```

---

# Step 6 — Perform the Lab Search

Your course asks you to reproduce the previous web search:

```bash
shodan search webcam
```

Conceptually:

```text
┌──(kali㉿kali)-[~]
└─$ shodan search webcam
```

Shodan queries its indexed database and returns matching results.

The output can look roughly like:

```text
203.0.113.10:80     Example Web Server
198.51.100.20:8080  HTTP Server
...
```

The exact results will change over time.

---

# What Does the Output Mean?

A result may contain:

```text
IP address : Port : Description
```

For example:

```text
203.0.113.10:80
```

means:

```text
IP address = 203.0.113.10
Port       = 80
```

The description may contain information extracted from the service banner.

Remember:

> A search result is information from Shodan's database. It isn't permission to access the corresponding system.

---

# Step 7 — Why Is the CLI Output Unformatted?

The web interface presents information visually:

```text
Country
Organization
Port
Product
Operating System
```

The CLI often gives a compact text representation.

That's useful because command-line output can be:

* redirected to files
* processed with Linux tools
* incorporated into scripts
* used for automated reporting

For example:

```bash
shodan search webcam > webcam-results.txt
```

This saves the search output to:

```text
webcam-results.txt
```

For an authorized lab, you can inspect it with:

```bash
cat webcam-results.txt
```

or:

```bash
less webcam-results.txt
```

---

# Step 8 — Check Your Query Credits

Run:

```bash
shodan info
```

Example:

```text
Query credits available: 0
Scan credits available: 0
```

Your course shows zero credits, but **your current account may show something different**.

### What are query credits?

They relate to API/search operations that consume Shodan query resources.

### What are scan credits?

They relate to Shodan's scanning functionality.

Your free account's capabilities can differ from paid plans, and Shodan changes its account/API policies from time to time.

So don't assume the numbers in the course material are the same as yours.

---

# Step 9 — Find Your Public IP

Run:

```bash
shodan myip
```

You may get:

```text
203.0.113.45
```

This represents the public-facing IP address Shodan identifies for your current Internet connection.

---

# Private IP vs Public IP

This is important.

Your Kali VM may have something like:

```text
192.168.1.25
```

or:

```text
10.0.2.15
```

Those are **private addresses**.

Your Internet connection may appear externally as:

```text
203.x.x.x
```

That's your **public IP**.

Conceptually:

```text
Kali VM
192.168.1.25
      │
      ▼
Router/NAT
      │
      ▼
Internet
      │
      ▼
Public IP
203.x.x.x
```

So:

```bash
shodan myip
```

is useful for seeing the Internet-facing address associated with your current connection.

---

# Step 10 — Run `shodan stats`

The course asks you to run:

```bash
shodan stats webcam
```

This produces summary information rather than displaying every individual result.

Conceptually, you might see categories such as:

```text
Top Countries

United States       ...
Germany              ...
United Kingdom       ...

Top Ports

80                  ...
443                 ...
8080                ...
```

The exact numbers are dynamic.

---

# Why Are Statistics Useful?

Suppose you search:

```bash
shodan stats apache
```

Instead of examining thousands of individual hosts, you can ask:

> "What countries, ports, organizations, or other categories dominate the results?"

This gives you a high-level understanding of the Internet exposure represented in Shodan's dataset.

---

# Step 11 — Understanding `stats` vs `search`

### `search`

```bash
shodan search apache
```

Purpose:

> Return matching results.

Conceptually:

```text
IP:Port
Banner
Product
...
```

### `stats`

```bash
shodan stats apache
```

Purpose:

> Return aggregate statistics about matching results.

Think:

```text
search → individual records

stats → summary
```

---

# Step 12 — Useful CLI Commands for Your Lab

Here's a compact reference:

| Command                | Purpose                         |
| ---------------------- | ------------------------------- |
| `shodan -h`            | Display CLI help                |
| `shodan init API_KEY`  | Configure your API key          |
| `shodan info`          | Display account/API information |
| `shodan search webcam` | Search Shodan's indexed data    |
| `shodan myip`          | Show your public IP             |
| `shodan stats webcam`  | Show aggregate statistics       |
| `shodan search --help` | Help for search                 |
| `shodan stats --help`  | Help for statistics             |

---

# Step 13 — A Complete Demonstration

You can perform the lab in this order:

### ① Open Terminal

```bash
```

### ② Check Shodan

```bash
shodan -h
```

### ③ Initialize

```bash
shodan init YOUR_API_KEY
```

Expected:

```text
Successfully initialized
```

### ④ Check account

```bash
shodan info
```

### ⑤ Search

```bash
shodan search webcam
```

### ⑥ Save results if required for your lab report

```bash
shodan search webcam > webcam-results.txt
```

### ⑦ Check public IP

```bash
shodan myip
```

### ⑧ Get statistics

```bash
shodan stats webcam
```

### ⑨ Read help whenever needed

```bash
shodan search --help
```

---

# Step 14 — What the Lab Is Actually Teaching You

The important concept isn't simply memorizing:

```bash
shodan search webcam
```

You're learning how a penetration tester can combine **passive information sources**.

For example:

```text
                Passive Reconnaissance

DNS
 │
 ├── nslookup
 ├── dig
 └── host
        │
        ▼
Domain/IP information

WHOIS
        │
        ▼
Registration / allocation information

Certificate Transparency
        │
        ▼
Certificates / subdomains

Shodan
        │
        ▼
Internet-exposed services / banners
```

This is why Shodan is particularly useful in reconnaissance: it gives you information that Shodan has already collected rather than requiring you to actively probe arbitrary Internet hosts.

---

# Lab Questions — Ready-to-Write Answers

### Q1. What command initializes Shodan?

```text
shodan init <API_KEY>
```

A successful initialization normally returns:

```text
Successfully initialized
```

---

### Q2. What command displays available Shodan CLI commands?

```text
shodan -h
```

---

### Q3. How do you search for webcams?

```text
shodan search webcam
```

---

### Q4. How do you check available credits?

```text
shodan info
```

---

### Q5. How do you find your public IP?

```text
shodan myip
```

---

### Q6. How do you obtain statistics for the webcam search?

```text
shodan stats webcam
```

---

## One important update to the course material

Your course says:

> "Searching with filters is not available with a free API key."

Don't treat that as a timeless rule. **Shodan's account/API capabilities and pricing can change**, and CLI behavior also depends on the version you're using.

If a command from the course produces an error, first run:

```bash
shodan --version
```

and:

```bash
shodan search --help
```

Then we can match the instructions to the **current Shodan CLI installed in your Kali VM** rather than blindly following an older course version.

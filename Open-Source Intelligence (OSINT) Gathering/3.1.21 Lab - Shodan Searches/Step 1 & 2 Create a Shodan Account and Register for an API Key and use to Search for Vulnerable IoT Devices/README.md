Yes. This lab is mainly teaching **passive reconnaissance with Shodan**: understanding what information Shodan has already indexed about Internet-connected services. Shodan describes its fundamental data unit as a **banner**, which contains metadata collected from a service running on a device. ([Shodan Help Center][1])

Because some of the lab's example searches concern potentially exposed webcams and anonymous FTP servers, I'll show you how to perform the **safe, passive parts** and how to document the results, but I won't provide instructions for logging into or interacting with devices you don't own.

# Part 1 — Create a Shodan Account and API Key

## Step 1 — Open Shodan

In Kali, open Firefox and go to:

[Shodan](https://www.shodan.io/?utm_source=chatgpt.com)

Click:

```text
Login
   ↓
Register
```

Create your account and verify your email.

---

## Step 2 — Log in

After verification:

```text
Firefox
   ↓
Shodan
   ↓
Login
```

You'll arrive at the Shodan search interface.

---

# Step 3 — Understand the "Banner"

Your first lab question is:

> According to Shodan, what is the fundamental unit of data it gathers?

### Answer

**Banner**

Shodan explains that a banner is information collected from a service running on an Internet-connected device. It can contain information such as the service response, IP address, port, organization, location, software, and other metadata. ([Shodan Help Center][1])

A simplified banner looks conceptually like:

```text
IP:          203.0.113.10
Port:        443
Organization: Example Organization
Country:     US
Product:     nginx
Version:     ...
```

The exact fields depend on the service.

---

# Part 2 — Understand Shodan Searches

## Step 4 — Perform a harmless search

In Shodan's search box, you can search for something general such as:

```text
Apache
```

or:

```text
nginx
```

These searches query Shodan's existing database rather than asking you to connect manually to the returned systems.

Shodan's current documentation explains that the basic search syntax is:

```text
filter:value
```

and filters can be combined. ([Shodan Help Center][1])

---

# Step 5 — Understand Search Filters

For example:

```text
apache country:US
```

means approximately:

```text
Search term: Apache
        +
Country: United States
```

Shodan's official API documentation uses this same type of query as an example. ([Shodan Developer][2])

Some useful filters for **authorized research** include:

```text
country:US
city:"Dhaka"
port:443
product:Apache
version:...
org:"Example Organization"
```

The important syntax rule is:

```text
filter:value
```

There is **no space** between the filter name and its value.

For values containing spaces:

```text
city:"New York"
```

rather than:

```text
city:New York
```

Shodan documents this syntax in its Search Query Fundamentals guide. ([Shodan Help Center][1])

---

# Step 6 — Understanding a Shodan Result

Clicking a result can show information such as:

### General information

Depending on the result, you may see:

```text
IP address
Hostnames
Domains
Country
City
Organization
ISP
ASN
```

This corresponds closely to the information Shodan stores in its banner data. ([Shodan Help Center][1])

---

## Example

Imagine Shodan displays:

```text
IP:          203.0.113.25
Hostname:    example.example
Country:     US
City:        Example City
Organization: Example ISP
ASN:         AS64500
```

You should interpret this as **reconnaissance information**, not as permission to interact with the host.

---

# Step 7 — Understanding Ports

A result may also show:

```text
PORT
22
80
443
```

For example:

```text
22/tcp    SSH
80/tcp    HTTP
443/tcp   HTTPS
```

The port tells you that Shodan observed a service associated with that port.

It does **not** automatically mean that the service is currently available or vulnerable.

Shodan's data is based on what it has collected, and results can change over time. ([Shodan Help Center][3])

---

# Step 8 — Understanding the Banner

Suppose a web service returns:

```text
HTTP/1.1 200 OK
Server: nginx
Content-Type: text/html
```

Shodan can store information like this in the banner.

The official documentation gives HTTP-banner information as an example and explains that the `data` field contains the main response from the service. ([Shodan Help Center][1])

So you might record:

| Field        | Example      |
| ------------ | ------------ |
| IP           | 203.0.113.25 |
| Port         | 443          |
| Product      | nginx        |
| Protocol     | HTTPS        |
| Country      | US           |
| Organization | Example ISP  |

---

# Part 3 — The Webcam Exercise

Your course asks you to search:

```text
webcam
```

This is intended to demonstrate how Shodan can identify services whose banners contain particular terms.

However, there is an important distinction:

> A Shodan result containing `webcam` does **not necessarily mean that the device is actually a webcam**.

The search is based on indexed banner information. Shodan itself notes that the type of indexed device can vary considerably. ([Shodan Help Center][3])

### Safe way to complete the exercise

You can examine:

* country statistics
* organization statistics
* product statistics
* operating-system statistics
* port information
* banner metadata

Do **not** attempt to log into the returned cameras or guess passwords.

---

# Lab Question — Top Country

The course's original answer says:

> **USA**

But this is a **time-sensitive result**.

Shodan's indexed data changes continuously, so you should record whatever country is actually shown at the time you perform the lab.

For example:

```text
Top Countries

United States
...
```

If your current results differ, use your current result rather than the old course answer.

---

# Part 4 — General Information

The lab asks:

> What information is contained in the General Information section?

### Expected answer

```text
Hostnames
Domains
Country
City
Organization
ISP
ASN
```

You can write:

> The General Information section can contain the hostnames, domains, country, city, organization, ISP, and ASN associated with the indexed host.

---

# Part 5 — Open Ports

The lab asks:

> What ports are open?

This answer **varies by result**.

Don't copy:

```text
8081, 8088, 80
```

unless those are actually displayed in your selected result.

Instead, record what Shodan currently shows.

For example:

```text
Ports:

80/tcp
443/tcp
```

Your report can say:

> The selected Shodan result showed ports 80/tcp and 443/tcp.

---

# Part 6 — What Information Can a Port Result Contain?

Depending on the service, Shodan can show information such as:

```text
Port
Protocol
Product
Version
HTTP headers
Webpage title
TLS/SSL information
Certificates
Service banner
```

The exact fields depend on the service.

Shodan's banner structure contains many properties, including nested HTTP and SSL information. ([Shodan Help Center][1])

---

# Part 7 — Understand the `cloud` Tag

Some Shodan results can be associated with cloud infrastructure.

You may encounter information such as:

```text
Cloud Provider
Cloud Region
Cloud Service
```

For example:

```text
Cloud Provider: Amazon Web Services
Cloud Region: ...
Cloud Service: ...
```

This is useful for understanding infrastructure at a high level.

---

# Part 8 — Understand the `honeypot` Tag

You may also encounter Shodan results identified as:

```text
honeypot
```

A honeypot is a deliberately exposed system designed to attract or study malicious activity.

Therefore:

```text
honeypot
```

does **not** mean:

> "This is an easy target."

It means the system may specifically exist for security monitoring/research.

---

# Part 9 — Apache Search Exercise

Your course gives:

```text
Apache port:80 city:"your-city"
```

The idea is:

```text
Apache
   +
port:80
   +
city:"your-city"
```

For example, if you were studying a **lab environment you control**, you could replace the city with an appropriate location.

Shodan's official documentation confirms that filters can be combined to narrow searches. ([Shodan Help Center][1])

### What you're learning

You're not learning how to attack Apache.

You're learning how to answer:

> "What Internet-facing services matching a particular software/service description has Shodan already indexed?"

---

# Part 10 — API Key

The lab also asks you to register for an API key.

Shodan's current documentation says that an API key is obtained by creating a Shodan account. ([Shodan Developer][4])

After logging into Shodan, find your account/API settings and copy the API key.

**Do not post the key in screenshots, GitHub, assignments, or chat.**

Treat it like a credential.

---

# Step 11 — Check Whether the Shodan CLI Is Installed

In Kali:

```bash
shodan -h
```

If installed, you'll see the command help.

You can check the version:

```bash
shodan version
```

Shodan's official CLI documentation recommends `shodan version` for checking the installed version. ([Shodan Help Center][5])

---

# Step 12 — Configure Your API Key

The CLI can be initialized with your own API key:

```bash
shodan init YOUR_API_KEY
```

For example:

```bash
shodan init xxxxxxxxxxxxxxxxxxxxx
```

Don't use the example key literally.

Then:

```bash
shodan info
```

can be used to inspect account/API information available to the CLI.

---

# Step 13 — Safe Statistics Example

One useful feature is obtaining **statistics rather than individual hosts**.

For example:

```bash
shodan stats --facets country,port apache
```

This produces a distribution of results rather than requiring you to interact with individual systems.

Shodan calls these **facets**; they provide summary statistics such as the distribution by country or port. ([Shodan Help Center][6])

Conceptually:

```text
Apache results

Country
US       xxxx
DE       xxxx
GB       xxxx

Port
80       xxxx
443      xxxx
8080     xxxx
```

This is particularly useful for learning reconnaissance without focusing on individual devices.

---

# Part 14 — How to Write Your Lab Answers

You can structure your submission like this:

### Question 1

**According to Shodan, what is the fundamental unit of data it gathers?**

> The fundamental unit of data gathered by Shodan is the **banner**. A banner contains metadata and information collected from a service running on an Internet-connected device. ([Shodan Help Center][1])

### Question 2

**What is the top country for webcam results?**

> The course's historical answer is **United States (USA)**. However, Shodan's results change over time, so the current top country should be recorded from the search results during the lab.

### Question 3

**What information is contained in General Information?**

> Hostnames, domains, country, city, organization, ISP, and ASN.

### Question 4

**What ports are open?**

> Answers vary. Record the ports displayed for the specific Shodan result you selected.

### Question 5

**What information is available for the ports?**

> Depending on the service, Shodan can provide service banners, HTTP information, product/version information, TLS/SSL certificate information, and other service metadata.

### Question 6

**What additional information can a cloud result contain?**

> Cloud Provider, Cloud Region, and Cloud Service.

---

# The Main Concept to Remember

Your previous DNS/OSINT lessons fit together nicely:

```text
DNS
 ↓
Domain → IP

Whois
 ↓
Domain/IP registration information

Certificate Transparency
 ↓
Domain → certificates → subdomains

Shodan
 ↓
IP → indexed services → banners → ports → software metadata
```

So Shodan is essentially another **passive reconnaissance source**.

It doesn't replace `nslookup`, `dig`, Whois, or CT logs—it complements them.

[Shodan Search Query Fundamentals](https://help.shodan.io/the-basics/search-query-fundamentals?utm_source=chatgpt.com)

[Shodan API Documentation](https://developer.shodan.io/api?utm_source=chatgpt.com)

[1]: https://help.shodan.io/the-basics/search-query-fundamentals?utm_source=chatgpt.com "Search Query Fundamentals - Shodan Help Center"
[2]: https://developer.shodan.io/api?utm_source=chatgpt.com "Shodan Developer"
[3]: https://help.shodan.io/the-basics/what-is-shodan?utm_source=chatgpt.com "What is Shodan? - Shodan Help Center"
[4]: https://developer.shodan.io/api/requirements?utm_source=chatgpt.com "Shodan Developer"
[5]: https://help.shodan.io/command-line-interface/1-getting-started?utm_source=chatgpt.com "Getting Started - Shodan Help Center"
[6]: https://help.shodan.io/command-line-interface/3-stats?utm_source=chatgpt.com "Generating Statistics - Shodan Help Center"

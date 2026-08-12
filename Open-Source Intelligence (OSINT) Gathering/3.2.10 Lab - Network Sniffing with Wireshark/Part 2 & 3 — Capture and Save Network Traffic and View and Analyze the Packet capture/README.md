
# Part 2 — Capture and Save Network Traffic

## Step 1 — Find the correct interface

Open a Kali terminal:

```bash
ifconfig
```

You may see something like:

```text
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>
        inet 192.168.1.10
        netmask 255.255.255.0
        ether 08:00:27:AA:BB:CC

lo: ...
```

The interface you want for the first capture is normally:

```text
eth0
```

But **use whatever interface your lab identifies as the Ethernet adapter**.

---

# Step 2 — Start tcpdump

Run:

```bash
sudo tcpdump -i eth0 -s 0 -w packetdump.pcap
```

If your interface is different, replace `eth0`.

For example:

```bash
sudo tcpdump -i ens33 -s 0 -w packetdump.pcap
```

You'll be asked for your Kali password:

```text
[sudo] password for kali:
```

Enter:

```text
kali
```

Nothing will appear while typing the password. That's normal.

---

# Understand the command

The command:

```bash
sudo tcpdump -i eth0 -s 0 -w packetdump.pcap
```

has several parts.

### `sudo`

```text
sudo
```

Runs tcpdump with elevated privileges.

---

### `tcpdump`

```text
tcpdump
```

A command-line packet capture tool.

---

### `-i eth0`

```text
-i eth0
```

means:

> Capture traffic from interface `eth0`.

---

### `-s 0`

```text
-s 0
```

means capture the full packet rather than restricting the snapshot length.

For this lab, this is important because you want enough packet contents available for later Wireshark analysis.

---

### `-w packetdump.pcap`

```text
-w packetdump.pcap
```

means:

> Write the captured packets into `packetdump.pcap`.

Instead of displaying packet details on the screen, tcpdump writes them to the PCAP file.

---

# What is happening now?

Think of tcpdump as a recorder:

```text
                 Internet
                    │
                    ▼
              ┌───────────┐
              │   eth0    │
              └─────┬─────┘
                    │
                    ▼
               tcpdump
                    │
                    ▼
          packetdump.pcap
```

**Do not press `CTRL+C` yet.**

tcpdump is currently waiting for traffic.

---

# Step 3 — Generate traffic

Now open Firefox while tcpdump continues running.

## First website

Navigate to:

```text
google.com
```

Don't log in or search.

This generates traffic such as:

```text
DNS query
DNS response
TCP/TLS traffic
HTTP/HTTPS-related traffic
```

---

# Step 4 — Open netacad.com

Open another browser tab and go to:

```text
netacad.com
```

The lab asks you to log in to Skills for All.

Use your **own authorized course account**.

Don't worry if the current website redirects through several domains. That's normal for modern web applications.

The browser will generate many packets.

For example:

```text
Browser
   │
   ├── DNS query
   ├── DNS response
   ├── TCP connection
   ├── TLS connection
   ├── HTTP/HTTPS requests
   └── responses
```

---

# Step 5 — Stop tcpdump

Return to the terminal running tcpdump.

Press:

```text
CTRL+C
```

tcpdump should stop.

You may see something similar to:

```text
^C
1234 packets captured
...
```

The exact numbers will vary.

---

# Step 6 — Check the PCAP file

Run:

```bash
ls packetdump.pcap
```

Expected:

```text
packetdump.pcap
```

You can also run:

```bash
ls -lh packetdump.pcap
```

Example:

```text
-rw-r--r-- 1 kali kali 2.4M Aug 12 12:45 packetdump.pcap
```

This tells you the capture file exists and its size.

---

# Part 3 — Analyze the PCAP with Wireshark

Now we're moving from:

```text
tcpdump = capture
```

to:

```text
Wireshark = analyze
```

---

# Step 1 — Start Wireshark

In the terminal:

```bash
wireshark
```

The graphical Wireshark application should open.

You can also launch it through Kali's application menu.

---

# Step 2 — Open packetdump.pcap

In Wireshark:

```text
File
  ↓
Open
  ↓
packetdump.pcap
  ↓
Open
```

You should see three major areas:

```text
┌──────────────────────────────────────────────┐
│ Packet List                                  │
├──────────────────────────────────────────────┤
│ Packet Details                               │
├──────────────────────────────────────────────┤
│ Packet Bytes                                 │
└──────────────────────────────────────────────┘
```

### Top section

Shows packets:

```text
No.
Time
Source
Destination
Protocol
Length
Info
```

### Middle section

Shows protocol layers.

### Bottom section

Shows raw packet bytes.

---

# Part 3 — Step 2: Analyze DNS Traffic

## Step 1 — Apply the DNS filter

At the top of Wireshark, find:

```text
Display Filter
```

Enter:

```text
dns
```

Then press:

```text
Enter
```

Now Wireshark should display primarily DNS packets.

---

# What is DNS traffic?

Suppose you type:

```text
www.example.com
```

The computer first needs to discover its IP address.

It sends something like:

```text
Kali
  │
  │ DNS Query:
  │ www.example.com?
  ▼
DNS Server
  │
  │ DNS Response:
  │ 93.x.x.x
  ▼
Kali
```

Therefore, DNS packets can reveal:

```text
Domain name
        ↓
IP address
```

This is why DNS traffic is useful during network analysis.

---

# Step 2 — What other DNS domains might you see?

The lab says that you may see domains other than the main website.

For example, websites often load:

```text
Google Analytics
CDNs
Social-media services
Authentication services
Cisco services
Advertising services
Fonts
Images
JavaScript libraries
```

So you might see DNS queries for several domains.

### Lab answer

> The DNS queries can include social-media sites, Google Analytics-related domains, other Cisco domains, and domains associated with resources loaded by the Google and Skills for All pages.

The **exact domains depend on the current websites and your capture**, so don't copy someone else's domain list as if it were your own result.

---

# Step 3 — Search for netacad.com

Click Wireshark's search/find function.

Search for:

```text
netacad
```

The lab instructs you to use:

```text
String
```

as the search type.

Find a DNS Standard Query associated with:

```text
netacad.com
```

Click that packet.

---

# Step 4 — Examine Ethernet II

In the packet details pane, expand:

```text
Ethernet II
```

You should see something conceptually like:

```text
Ethernet II
    Destination: XX:XX:XX:XX:XX:XX
    Source: YY:YY:YY:YY:YY:YY
    Type: IPv4
```

The important fields are:

```text
Source MAC
Destination MAC
```

---

# Why is the destination MAC the gateway's MAC?

Suppose:

```text
Kali:
192.168.1.10
```

and:

```text
DNS server:
8.8.8.8
```

They're not on the same Layer-2 network.

So Kali sends the Ethernet frame to its **default gateway**.

Conceptually:

```text
Ethernet frame

Source MAC:
Kali's MAC

Destination MAC:
Gateway's MAC
```

But inside that Ethernet frame:

```text
IP packet

Source IP:
Kali

Destination IP:
DNS server
```

This is a very important distinction:

```text
Layer 2:
Kali MAC → Gateway MAC

Layer 3:
Kali IP → DNS Server IP
```

---

# Lab Question

> Does the source MAC address match the address recorded in Part 1?

### Answer:

**Yes.**

The source MAC address in the DNS query should correspond to the MAC address of the Kali interface that generated the packet.

If your answer is different, first make sure you selected the correct interface in Part 1.

---

# Step 5 — Examine the DNS Query

Expand:

```text
Domain Name System (query)
```

You should see information such as:

```text
Transaction ID
Flags
Questions
Queries
    Name: netacad.com
    Type: A
    Class: IN
```

The important idea is:

```text
Query:
"What IP address belongs to netacad.com?"
```

---

# Step 6 — Find the DNS Response

The DNS query normally contains information pointing to its corresponding response.

Click the response packet.

You should see:

```text
Domain Name System (response)
```

Expand:

```text
Answers
```

You may find several records.

For example, the lab gives example addresses such as:

```text
13.33.21.125
13.33.21.5
13.33.21.122
13.33.21.30
```

But **your current capture may show different addresses** because DNS results can change over time and may depend on CDN/location.

### Lab answer

> The IP addresses associated with `netacad.com` are the IPv4 addresses listed under the DNS response's Answer section. At the time of the original lab, examples included `13.33.21.125`, `13.33.21.5`, `13.33.21.122`, and `13.33.21.30`.

---

# Very Important Wireshark Skill

When analyzing DNS, remember:

```text
DNS Query
   ↓
Question:
netacad.com
   ↓
DNS Response
   ↓
Answer:
IP address(es)
```

So if the instructor asks:

> "What IP addresses does this domain resolve to?"

Look at:

```text
DNS Response
   ↓
Answers
   ↓
A records
```

---

# Part 3 — Step 3: Analyze the HTTP Session

This is an especially important section because you will see why **unencrypted HTTP is dangerous**.

The lab uses:

```text
DVWA
```

inside the authorized lab environment.

---

# Step 1 — Find the 10.6.6.0/24 Interface

Open a terminal:

```bash
ifconfig
```

Look for an interface with an address in:

```text
10.6.6.0/24
```

The lab gives an example:

```text
Interface:
br-ee5c01518209
```

with:

```text
IP:
10.6.6.1
```

Your environment may use a different interface name.

For example:

```text
br-xxxxxxxxxxxx
```

This is normal because Docker bridge interface names can differ between systems.

---

# Step 2 — Understand the Docker Network

The architecture is approximately:

```text
Kali VM
10.6.6.1
     │
     │ Docker bridge
     ▼
10.6.6.13
     │
     ▼
DVWA
```

So:

```text
Kali:
10.6.6.1
```

and:

```text
DVWA:
10.6.6.13
```

are communicating on the lab's internal network.

---

# Step 3 — Start Wireshark

Run:

```bash
wireshark
```

When Wireshark opens, locate the interface corresponding to:

```text
10.6.6.0/24
```

For example:

```text
br-ee5c01518209
```

Double-click that interface.

Wireshark now begins capturing traffic on the internal lab network.

---

# Step 4 — Open DVWA

In Firefox, enter:

```text
http://10.6.6.13
```

You should see the DVWA login page.

The lab provides:

```text
Username: admin
Password: password
```

Use these **only for the supplied DVWA lab**.

---

# Step 5 — Log in

Enter:

```text
Username:
admin
```

and:

```text
Password:
password
```

Then log in.

Once the DVWA main page appears, click:

```text
Instructions
```

This generates additional HTTP traffic.

Then close the browser.

---

# Step 6 — Stop Wireshark

Return to Wireshark.

Click:

```text
Red Square
```

This stops packet capture.

---

# Step 7 — Search for POST

Use Wireshark's search function.

Search for:

```text
POST
```

Why `POST`?

Because HTML forms frequently send submitted information using an HTTP POST request.

Conceptually:

```text
Browser
   │
   │ POST /login.php
   │
   │ username=admin
   │ password=password
   ▼
DVWA Server
```

Because this lab uses **HTTP rather than HTTPS**, the HTTP application data can be visible in the capture.

---

# Step 8 — Open the POST Packet

Double-click the first relevant POST packet.

In the packet details, find:

```text
Hypertext Transfer Protocol
```

or the relevant HTTP/form-data section.

The lab specifically asks you to expand:

```text
HTML Form URL Encoded
```

You may see information conceptually like:

```text
username=admin
password=password
user_token=...
```

The exact token will vary.

---

# Lab Question

> What information is contained in this section?

### Answer:

> It contains the username and password submitted through the form, along with a `user_token` value.

This is the key security lesson:

```text
HTTP
 ↓
Application data can potentially be captured
 ↓
Credentials may be exposed
```

---

# Why HTTPS Is Different

With HTTPS:

```text
Browser
   │
   │ encrypted HTTP
   ▼
HTTPS Server
```

the network capture normally doesn't expose the login credentials as readable HTTP form fields.

With plain HTTP:

```text
Browser
   │
   │ username=admin
   │ password=password
   ▼
Server
```

the application data can be visible to someone who can legitimately capture the traffic.

---

# Step 9 — Investigate `302 Found`

Now use Wireshark search to find:

```text
302 Found
```

A `302 Found` is an HTTP redirect response.

For example:

```text
HTTP/1.1 302 Found
```

The server can include a header such as:

```text
Set-Cookie: PHPSESSID=...
```

---

# Step 10 — Find Set-Cookie

Expand:

```text
Hypertext Transfer Protocol
```

Look for:

```text
Set-Cookie
```

The lab expects the cookie name:

```text
PHPSESSID
```

So you may see something conceptually like:

```text
Set-Cookie: PHPSESSID=abc123...
```

The actual session ID will be different in your capture.

---

# What is PHPSESSID?

`PHPSESSID` is a PHP session cookie.

Think of it as a temporary identifier:

```text
Browser
    │
    │ PHPSESSID=ABC123
    ▼
Server
```

The server can use this value to associate subsequent requests with the user's session.

---

# Step 11 — Find the Next GET Request

After receiving the cookie, the browser sends another HTTP request.

Find the next:

```text
GET
```

packet.

Expand:

```text
Hypertext Transfer Protocol
```

Look for:

```text
Cookie:
```

You should find something conceptually like:

```text
Cookie: PHPSESSID=ABC123
```

---

# Step 12 — Compare the Session IDs

Compare:

### Server → Browser

```text
Set-Cookie:
PHPSESSID=ABC123
```

with:

### Browser → Server

```text
Cookie:
PHPSESSID=ABC123
```

They should match.

---

# Lab Question

> Does the PHPSESSID sent back to the server match the one sent by the server earlier?

### Answer:

**Yes.**

The browser sends the same `PHPSESSID` value back to the server in the subsequent GET request.

---

# Why Is This Important?

This demonstrates how session management works.

The basic flow is:

```text
1. Browser logs in
          │
          ▼
2. Server creates session
          │
          ▼
3. Server sends PHPSESSID
          │
          ▼
4. Browser stores cookie
          │
          ▼
5. Browser sends PHPSESSID
   with subsequent requests
          │
          ▼
6. Server recognizes the session
```

So:

```text
Set-Cookie
```

creates/assigns the session cookie, while:

```text
Cookie
```

sends that cookie back to the server.

---

# Complete Lab Flow

You can remember the entire exercise like this:

```text
PART 1
Prepare Kali
   │
   ├── pwd
   ├── ifconfig
   ├── ip route
   └── cat /etc/resolv.conf
   │
   ▼
PART 2
Capture
   │
   └── tcpdump
          │
          ▼
    packetdump.pcap
          │
          ▼
PART 3
Analyze
   │
   ├── DNS
   │    ├── Query
   │    ├── Response
   │    └── IP addresses
   │
   └── HTTP/DVWA
        ├── POST
        ├── username
        ├── password
        ├── Set-Cookie
        ├── PHPSESSID
        └── subsequent GET
```

---

# Important Answers to Write in the Lab

### DNS — Other websites

> Various additional domains may appear, including social-media services, Google Analytics-related domains, and other Cisco-related domains. The exact results depend on the current web pages and captured traffic.

### DNS — Source MAC

> **Yes.** The source MAC address should match the MAC address recorded for the Kali capture interface in Part 1.

### DNS — netacad.com IPs

> Use the IP addresses shown in the **DNS response → Answers** section of your own capture. The original lab gives examples such as `13.33.21.125`, `13.33.21.5`, `13.33.21.122`, and `13.33.21.30`, but current results can differ.

### 10.6.6.0/24 interface

> The original lab example uses `br-ee5c01518209`.

### Interface IP

> The original lab example uses `10.6.6.1`.

### HTTP POST

> The HTML form data contains the submitted username, password, and a `user_token`.

### Cookie

> The server sets a `PHPSESSID` session cookie.

### Subsequent GET

> **Yes.** The browser sends the same `PHPSESSID` value back to the server.

---

## One very important security lesson

This lab demonstrates **why HTTPS matters**.

With the DVWA HTTP example:

```text
HTTP
 │
 ├── POST
 │    ├── username
 │    └── password
 │
 └── Cookie
      └── PHPSESSID
```

the information can be visible in a packet capture.

With properly configured HTTPS:

```text
HTTPS
 │
 └── encrypted application data
       ↓
 credentials aren't normally readable
       as plain HTTP form fields
```


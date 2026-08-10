
# Part 1 — Investigate the Scapy Tool

## 1. What is Scapy?

**Scapy** is a Python-based packet manipulation and network analysis tool.

The easiest way to understand it is:

> Nmap is mainly used to **discover and scan**, while Scapy gives you much more direct control over **individual packets**.

Scapy can:

* Construct packets
* Send packets
* Capture packets
* Inspect packets
* Modify packet fields
* Decode protocol layers
* Save captured packets to `.pcap`
* Be used interactively
* Be used from Python programs

The lab's documentation describes Scapy as a Python program capable of sending, sniffing, dissecting, and forging network packets. 

### Simple mental model

Think of a network packet like an envelope:

```text
┌─────────────────────────────┐
│ Ethernet information        │
├─────────────────────────────┤
│ IP information              │
├─────────────────────────────┤
│ TCP/UDP/ICMP information    │
├─────────────────────────────┤
│ Application data            │
└─────────────────────────────┘
```

Scapy lets you look at and work with these individual layers.

---

# 2. Start Kali

Log into your Kali VM.

Open:

```text
Applications → Terminal
```

You should see something similar to:

```text
┌──(kali㉿kali)-[~]
└─$
```

---

# 3. Open Scapy Documentation

The lab asks you to open the Scapy documentation in Firefox:

[Scapy documentation](https://scapy.readthedocs.io/en/latest/introduction.html?utm_source=chatgpt.com)

The lab states that this customized Kali environment already includes Python and Scapy. 

---

# Question 1 — How does Scapy describe itself?

### Answer

Scapy is a **Python program that allows users to send, sniff, dissect, and forge network packets**, which enables construction of tools for probing, scanning, and attacking networks. 

For your answer sheet, you can write:

> **Scapy is a Python-based packet manipulation tool that can send, capture, dissect, and forge network packets.**

---

# 4. Start Scapy from the Terminal

The lab uses root privileges because some packet operations require them.

Run:

```bash
sudo su
```

You will be asked for the Kali password:

```text
[sudo] password for kali:
```

Enter:

```text
kali
```

Your prompt changes from:

```text
└─$
```

to:

```text
└─#
```

For example:

```text
┌──(root㉿kali)-[/home/kali]
└─#
```

This indicates that you're operating as root. 

---

# 5. Start Scapy

Now type:

```bash
scapy
```

You should see the Scapy banner and eventually:

```text
>>>
```

The `>>>` prompt means you are now inside Scapy's interactive Python environment. 

So the workflow is:

```text
Kali Terminal
     │
     ▼
sudo su
     │
     ▼
scapy
     │
     ▼
>>>
```

---

# 6. Explore Available Protocols

At the Scapy prompt:

```python
ls()
```

The lab explains that `ls()` displays the available packet formats and protocols. 

You'll get a very long list.

You may see protocols such as:

```text
ARP
Ether
IP
TCP
UDP
ICMP
DNS
DHCP
HTTP
...
```

The exact list depends on your Scapy version.

---

# Question — How many TFTP packet formats?

The supplied lab expects:

### **9**



---

# 7. Understand an IPv4 Packet

Before creating or examining packets, you need to understand the IPv4 header.

A simplified IPv4 packet looks like:

```text
┌──────────────────────────────┐
│ Version                      │
├──────────────────────────────┤
│ Header Length                │
├──────────────────────────────┤
│ DS / ToS                     │
├──────────────────────────────┤
│ Total Length                 │
├──────────────────────────────┤
│ Identification               │
├──────────────────────────────┤
│ Flags / Fragment Offset      │
├──────────────────────────────┤
│ TTL                          │
├──────────────────────────────┤
│ Protocol                     │
├──────────────────────────────┤
│ Header Checksum              │
├──────────────────────────────┤
│ Source IP                    │
├──────────────────────────────┤
│ Destination IP               │
└──────────────────────────────┘
```

The supplied lab identifies important fields including Version, DS, TTL, Protocol, Header Checksum, Source IP and Destination IP. 

---

# 8. What Does TTL Mean?

TTL means:

**Time To Live**

For example:

```text
TTL = 64
```

When a packet passes through a router, the TTL is reduced.

Example:

```text
Computer
TTL 64
  ↓
Router 1
TTL 63
  ↓
Router 2
TTL 62
  ↓
Router 3
TTL 61
```

If TTL reaches zero, the packet is discarded.

The lab explains that the router then sends an ICMP Time Exceeded message back to the source. 

---

# 9. What Does the Protocol Field Do?

The IPv4 `Protocol` field tells the receiving system what protocol is contained in the IP payload.

Common values include:

```text
ICMP = 1
TCP  = 6
UDP  = 17
```

So conceptually:

```text
IP
│
├── Protocol 1 → ICMP
├── Protocol 6 → TCP
└── Protocol 17 → UDP
```

The lab explicitly gives these examples. 

---

# 10. Examine the IPv4 Structure in Scapy

At:

```text
>>>
```

run:

```python
ls(IP)
```

You should see fields similar to:

```text
version
ihl
tos
len
id
flags
frag
ttl
proto
chksum
src
dst
options
```

The supplied lab shows these fields and their default values. 

---

# Understanding `ls(IP)`

For example:

```text
version = 4
```

means IPv4.

```text
ttl = 64
```

is the default TTL shown in the lab.

```text
src
```

is the source IP.

```text
dst
```

is the destination IP.

And:

```text
proto
```

identifies the next protocol.

---

# Question — Difference between the diagram and Scapy?

The supplied answer is:

> The DiffServ field is still identified as **TOS (Type of Service)** in Scapy, and Scapy also includes an **Options** field. 

So write:

**The main differences are that Scapy calls the DiffServ field TOS and includes an Options field.**

---

# 11. Which Field Determines the Source?

The lab asks:

> Which field would you change to create a packet that would generate a reply to a target machine rather than the machine that actually sent the packet?

The answer is:

### **Source IPv4 address (`src`)**



Why?

Suppose:

```text
Source → 10.6.6.1
Destination → 10.6.6.23
```

A response normally goes back toward:

```text
10.6.6.1
```

The source field identifies where the packet originated from at the IP layer.

**Important:** changing source addresses can create misleading/spoofed traffic, so only experiment with this inside an authorized lab.

---

# Part 2 — Use Scapy to Sniff Network Traffic

Now we move from:

```text
creating/inspecting packets
```

to:

```text
capturing packets
```

The lab compares Scapy's sniffing capability with tools such as tcpdump and tshark. 

---

# Step 1 — Capture Network Traffic

At the Scapy prompt:

```python
sniff()
```

Scapy starts capturing packets on the default interface.

You won't necessarily see packets immediately.

It is essentially waiting:

```text
Scapy
  │
  ├── TCP packet
  ├── UDP packet
  ├── ICMP packet
  ├── DNS packet
  └── ...
```

---

# 2. Generate Some Traffic

Open a **second terminal**.

Run:

```bash
ping -c 5 www.cisco.com
```

The lab specifically uses this command to generate traffic while Scapy is capturing. 

The `-c 5` means:

> Send 5 ICMP echo requests.

Conceptually:

```text
Kali
 │
 │ ICMP Echo Request
 ▼
www.cisco.com
 │
 │ ICMP Echo Reply
 ▼
Kali
```

---

# 3. Stop the Capture

Return to the Scapy terminal.

Press:

```text
CTRL+C
```

The lab shows output similar to:

```text
<Sniffed: TCP:75 UDP:42 ICMP:32 Other:2>
```

The numbers will vary depending on your environment and other network activity. 

---

# 4. Save the Captured Packets in a Variable

The lab uses:

```python
a=_
```

Here `_` represents the output of the previous operation in the interactive Python environment.

So:

```python
a=_
```

means approximately:

> Store the most recently returned packet capture in variable `a`.

Then:

```python
a.summary()
```

The lab uses this to display a summary of the captured packets. 

---

# What Does `a.summary()` Do?

Instead of displaying every field of every packet, it gives you a compact overview.

You may see something conceptually like:

```text
Ether / IP / TCP ...
Ether / IP / UDP ...
Ether / IP / ICMP ...
```

This is useful when you have hundreds or thousands of packets.

---

# Step 2 — Capture Traffic on a Specific Interface

Now the lab moves to the internal virtual network.

First determine which interface has:

```text
10.6.6.1
```

Open another terminal:

```bash
ifconfig
```

or, on modern Kali:

```bash
ip addr
```

The lab identifies the interface as:

```text
br-internal
```

for this environment. 

---

# 2. Capture on `br-internal`

Return to Scapy:

```python
sniff(iface="br-internal")
```

Now Scapy specifically listens on:

```text
br-internal
```

instead of using the default interface.

---

# 3. Generate Internal Web Traffic

Open Firefox and visit:

```text
http://10.6.6.23/
```

The lab says that the Gravemind home page should open. 

Now return to Scapy and press:

```text
CTRL+C
```

You may see something similar to:

```text
<Sniffed: TCP:112 UDP:0 ICMP:0 Other:2>
```

The actual numbers can differ.

---

# 4. Review the Capture

Again:

```python
a=_
```

then:

```python
a.summary()
```

Now you're examining traffic generated by communication with the internal web server.

---

# Step 3 — Filter Traffic

This is an important Scapy concept.

Instead of capturing everything:

```python
sniff()
```

you can tell Scapy:

> Capture only ICMP traffic.

The lab uses:

```python
sniff(iface="br-internal", filter="icmp", count=10)
```



There are three important parameters:

```text
iface
filter
count
```

---

## `iface`

```python
iface="br-internal"
```

Means:

> Listen on the `br-internal` interface.

---

## `filter`

```python
filter="icmp"
```

Means:

> Only capture ICMP traffic.

---

## `count`

```python
count=10
```

Means:

> Stop after capturing 10 matching packets.

So the complete command means:

```text
Capture
  ↓
br-internal
  ↓
only ICMP
  ↓
stop after 10 packets
```

---

# 4. Generate ICMP Traffic

Open another terminal:

```bash
ping -c 10 10.6.6.23
```

This generates ICMP traffic toward the internal target. 

Because your Scapy filter is:

```text
icmp
```

the capture records the ICMP packets.

---

# 5. View Numbered Packets

After the capture reaches 10 packets:

```python
a=_
```

Then:

```python
a.nsummary()
```

The `nsummary()` function displays packet summaries with line numbers.

The lab expects 10 lines because:

```text
count = 10
```



---

# Question — What Traffic Is Displayed?

The expected answer is:

### **Five ICMP Echo Requests and five ICMP Echo Replies.**



Why?

When you ping:

```text
10.6.6.1 → 10.6.6.23
```

you send:

```text
Echo Request
```

and the destination responds:

```text
Echo Reply
```

So:

```text
Request 1
Reply 1

Request 2
Reply 2

Request 3
Reply 3

Request 4
Reply 4

Request 5
Reply 5
```

= **10 packets**

---

# 6. Examine an Individual Packet

Suppose `nsummary()` shows:

```text
0
1
2
3
...
```

You can inspect packet number 2 with:

```python
a[2]
```

The lab specifically uses this method. 

Scapy will show the different protocol layers.

Conceptually, you might see:

```text
Ether
  ↓
IP
  ↓
ICMP
  ↓
Raw
```

---

# Understanding the Layers

For an ICMP packet, you can think of it as:

```text
Ethernet
   │
   └── IP
        │
        └── ICMP
```

### Ethernet

Contains Layer 2 information such as:

```text
Source MAC
Destination MAC
```

### IP

Contains Layer 3 information:

```text
Source IP
Destination IP
TTL
Protocol
```

### ICMP

Contains information related to:

```text
Echo Request
Echo Reply
```

---

# Question — Why Are There Two Sets of Source/Destination Addresses?

This is an important networking concept.

The answer is:

### First set

The first source/destination addresses are **MAC addresses** at the Ethernet/data-link layer.

Example:

```text
Source MAC:
00:11:22:33:44:55

Destination MAC:
AA:BB:CC:DD:EE:FF
```

### Second set

The second source/destination addresses are **IP addresses** at the network layer.

Example:

```text
Source IP:
10.6.6.1

Destination IP:
10.6.6.23
```

The lab gives exactly this distinction. 

So:

```text
Layer 2
MAC → MAC

Layer 3
IP → IP
```

This is one of the most important things to understand from this exercise.

---

# Step 4 — Save the Capture

Scapy can save captured traffic in PCAP format.

Run:

```python
wrpcap("capture1.pcap", a)
```

The lab uses this exact command. 

This creates:

```text
capture1.pcap
```

---

# What Is a PCAP File?

PCAP means **packet capture**.

It stores captured network packets so they can later be examined with tools such as:

* Wireshark
* tshark
* tcpdump
* Scapy

Think of it as:

```text
Live network traffic
        ↓
      Scapy
        ↓
   capture1.pcap
        ↓
    Wireshark
```

---

# 7. Confirm the PCAP File

Open another terminal:

```bash
ls
```

You should see:

```text
capture1.pcap
```

The lab states that the file is written to the default user directory and can then be opened in Wireshark. 

You can also use:

```bash
ls -lh capture1.pcap
```

to see its size.

---

# 8. Open the Capture in Wireshark

Start Wireshark:

```bash
wireshark capture1.pcap
```

or open Wireshark from the Kali applications menu and choose:

```text
File → Open → capture1.pcap
```

You should see packets displayed in Wireshark.

A typical structure is:

```text
Packet List
────────────────────────────
No.  Time  Source  Destination  Protocol
1    ...   ...     ...           ICMP
2    ...   ...     ...           ICMP
3    ...   ...     ...           ICMP
...
```

Click an ICMP packet and you'll see:

```text
Frame
Ethernet II
Internet Protocol Version 4
Internet Control Message Protocol
```

This directly corresponds to what you were seeing in Scapy.

---

# Complete Scapy Workflow

The whole lab can be remembered like this:

```text
                 SCAPY
                   │
        ┌──────────┴──────────┐
        │                     │
   Packet Structure       Packet Capture
        │                     │
        ▼                     ▼
      ls()                 sniff()
        │                     │
        ▼                     ▼
     ls(IP)              Generate traffic
        │                     │
        ▼                     ▼
 IP fields               a = _
                              │
                              ▼
                         a.summary()
                              │
                              ▼
                         a.nsummary()
                              │
                              ▼
                            a[2]
                              │
                              ▼
                   wrpcap("capture1.pcap",a)
                              │
                              ▼
                          Wireshark
```

---

# All Lab Answers

| Question                            | Answer                                                            |
| ----------------------------------- | ----------------------------------------------------------------- |
| What is Scapy?                      | Python tool for sending, sniffing, dissecting and forging packets |
| TFTP packet formats                 | **9**                                                             |
| Difference in Scapy's IP fields     | TOS is used for DiffServ and Scapy includes Options               |
| Field to change for source identity | **Source IPv4 (`src`)**                                           |
| What does `sniff()` do?             | Captures network packets                                          |
| What did the ICMP capture contain?  | **5 Echo Requests + 5 Echo Replies**                              |
| Why two source/destination pairs?   | MAC addresses at Layer 2 and IP addresses at Layer 3              |
| Output file                         | **capture1.pcap**                                                 |

The answers above follow the supplied lab's expected answers and terminology.   

## The most important commands to practice

```bash
# Start Scapy
sudo su
scapy
```

Then inside Scapy:

```python
ls()
ls(IP)

sniff()

a=_
a.summary()
a.nsummary()

sniff(iface="br-internal", filter="icmp", count=10)

a=_
a.nsummary()
a[2]

wrpcap("capture1.pcap", a)
```

And from another terminal:

```bash
ping -c 5 www.cisco.com
```

or, for the internal lab:

```bash
ping -c 10 10.6.6.23
```


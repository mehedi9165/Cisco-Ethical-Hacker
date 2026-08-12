

# Part 1 — Prepare the Host to Capture Network Traffic

## Overall picture

Before starting packet capture, think of your Kali machine like this:

```text
                         Internet
                            │
                            │
                    ┌───────▼────────┐
                    │ Default Gateway│
                    │ 192.168.1.1    │
                    └───────┬────────┘
                            │
                     Local Network
                            │
                 ┌──────────▼─────────┐
                 │     Kali Linux     │
                 │                    │
                 │ IP: 192.168.1.10   │
                 │ MAC: AA:BB:...     │
                 └────────────────────┘
                            │
                            │
                      DNS queries
                            │
                 ┌──────────▼─────────┐
                 │   DNS Server      │
                 │   192.168.1.1     │
                 └────────────────────┘
```

The addresses in this diagram are **examples**. Your Kali VM will probably have different values.

---

# Step 1 — Start Kali Linux

Start your Kali Linux VM.

The lab specifies:

```text
Username: kali
Password: kali
```

After logging in, you'll reach the Kali desktop.

---

# Step 2 — Open the Terminal

Click the terminal icon in the Kali menu bar.

You should see something similar to:

```text
┌──(kali㉿Kali)-[~]
└─$
```

The `~` means you're currently in your user's home directory.

For the `kali` user:

```text
~
```

usually means:

```text
/home/kali
```

---

# Step 3 — Find Your Current Directory

Run:

```bash
pwd
```

`pwd` means:

> **Print Working Directory**

Example:

```bash
┌──(kali㉿Kali)-[~]
└─$ pwd
/home/kali
```

So your current directory is:

```text
/home/kali
```

### Why does the lab ask for this?

Later, when you save captured packets, you need to know **where the capture file is being saved**.

For example:

```text
/home/kali/capture.pcap
```

means:

```text
/home
   └── kali
        └── capture.pcap
```

---

## Lab Question

> Record the directory location.

### Example answer:

```text
/home/kali
```

The lab notes that this is usually:

```text
/home/<username>
```

So if your username is `kali`:

```text
/home/kali
```

---

# Step 4 — Find Your Network Interface

Now run:

```bash
ifconfig
```

You may see output similar to:

```text
┌──(kali㉿Kali)-[~]
└─$ ifconfig
```

Example:

```text
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>
        inet 192.168.1.10
        netmask 255.255.255.0
        broadcast 192.168.1.255
        ether 08:00:27:AB:CD:EF
        ...
```

Your actual output will be different.

---

# Step 5 — Identify the Ethernet Interface

Look for an interface such as:

```text
eth0
```

or possibly:

```text
ens33
enp0s3
```

depending on your VM configuration.

The lab says the Ethernet interface is **usually `eth0`**.

---

# Step 6 — Find the IP Address

Inside the interface information, look for:

```text
inet
```

For example:

```text
inet 192.168.1.10
```

This is your IPv4 address.

So:

```text
Kali IP = 192.168.1.10
```

Another example might be:

```text
inet 10.6.6.1
```

Then:

```text
Kali IP = 10.6.6.1
```

---

# Step 7 — Find the MAC Address

Look for:

```text
ether
```

Example:

```text
ether 08:00:27:AB:CD:EF
```

This is the MAC address of the Ethernet interface.

Therefore:

```text
IP address:
192.168.1.10

MAC address:
08:00:27:AB:CD:EF
```

---

# What Is the Difference Between IP and MAC?

This is extremely important for packet analysis.

## MAC address

Used primarily at the **Data Link Layer (Layer 2)**.

Example:

```text
08:00:27:AB:CD:EF
```

## IP address

Used at the **Network Layer (Layer 3)**.

Example:

```text
192.168.1.10
```

Think of it like:

```text
MAC
 ↓
Physical/network interface identity on local network

IP
 ↓
Logical network address
```

---

# Example

Suppose your Kali machine has:

```text
IP:
10.6.6.1

MAC:
08:00:27:AB:CD:EF
```

and wants to communicate with:

```text
10.6.6.23
```

A packet might conceptually look like:

```text
Ethernet Frame
│
├── Source MAC
│   08:00:27:AB:CD:EF
│
├── Destination MAC
│   XX:XX:XX:XX:XX:XX
│
└── IP Packet
    │
    ├── Source IP
    │   10.6.6.1
    │
    └── Destination IP
        10.6.6.23
```

This distinction becomes very useful later when you inspect packets in **Wireshark or Scapy**.

---

# Modern Kali Alternative

If `ifconfig` isn't available, use:

```bash
ip addr
```

or:

```bash
ip -br addr
```

Example:

```text
eth0    UP    10.6.6.1/24
```

Then use:

```bash
ip link show eth0
```

to find the MAC address.

For example:

```text
link/ether 08:00:27:ab:cd:ef
```

---

# Step 8 — Record Your Information

Make a small table for yourself:

| Information  | Example             |
| ------------ | ------------------- |
| Interface    | `eth0`              |
| IPv4 address | `192.168.1.10`      |
| MAC address  | `08:00:27:AB:CD:EF` |
| Network      | `192.168.1.0/24`    |

**Use your actual output in the lab answer.**

---

# Step 9 — Find the Default Gateway

Now run:

```bash
ip route
```

Example:

```text
┌──(kali㉿Kali)-[~]
└─$ ip route
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10
```

The important line is:

```text
default via 192.168.1.1 dev eth0
```

Therefore:

```text
Default Gateway = 192.168.1.1
```

---

# What Does "Default Gateway" Mean?

The default gateway is the device Kali normally sends traffic to when the destination isn't on its local subnet.

For example:

```text
Kali:
192.168.1.10

Destination:
8.8.8.8
```

Kali determines that:

```text
8.8.8.8
```

is not part of:

```text
192.168.1.0/24
```

Therefore it sends the traffic toward:

```text
192.168.1.1
```

which is the default gateway.

Conceptually:

```text
Kali
192.168.1.10
      │
      │
      ▼
Gateway
192.168.1.1
      │
      ▼
   Internet
      │
      ▼
   8.8.8.8
```

---

# Step 10 — Understand the Lab's ARP Comment

Your lab says:

> The default gateway responds to ARP requests for destination IP addresses located off the source network.

Here's what that means.

Suppose Kali wants to communicate with:

```text
8.8.8.8
```

Kali doesn't need the MAC address of Google's DNS server directly.

Instead, it needs the MAC address of its **local gateway**.

So it effectively asks:

```text
Who has 192.168.1.1?
```

The gateway responds with its MAC address.

Then Kali sends the Ethernet frame to the gateway.

---

# Step 11 — Why Must the Gateway Be on the Same Subnet?

Suppose Kali has:

```text
IP:
192.168.1.10

Subnet:
255.255.255.0
```

That's:

```text
192.168.1.0/24
```

A valid gateway might be:

```text
192.168.1.1
```

because it belongs to:

```text
192.168.1.0/24
```

But:

```text
10.0.0.1
```

would normally not be a directly connected gateway for that interface.

That's why the lab says:

> The IP address of the default gateway must be on the same IP subnet as the Ethernet interface address.

---

# Step 12 — Record the Gateway

Suppose you ran:

```bash
ip route
```

and received:

```text
default via 192.168.1.1 dev eth0
```

Your answer is:

```text
Default Gateway: 192.168.1.1
```

Again, **use your actual value**, not this example.

---

# Step 13 — Find the DNS Server

Now run:

```bash
cat /etc/resolv.conf
```

Example:

```text
┌──(kali㉿Kali)-[~]
└─$ cat /etc/resolv.conf

nameserver 192.168.1.1
```

The important word is:

```text
nameserver
```

So:

```text
DNS Server = 192.168.1.1
```

---

# Another Example

You might see:

```text
nameserver 8.8.8.8
nameserver 8.8.4.4
```

Then your configured DNS servers are:

```text
Primary:
8.8.8.8

Secondary:
8.8.4.4
```

If the lab asks for **the configured default DNS server**, record the first relevant `nameserver` entry.

---

# Step 14 — What Does a DNS Server Do?

Suppose you type:

```text
www.google.com
```

Your computer needs an IP address.

It sends a DNS query:

```text
Kali
192.168.1.10
       │
       │ DNS Query:
       │ "What is www.google.com?"
       ▼
DNS Server
192.168.1.1
       │
       │ DNS Reply:
       │ "IP = ..."
       ▼
Kali
```

So the DNS server is involved in translating names into IP addresses.

---

# Step 15 — Why Does the Lab Say DNS Is the Destination and Source?

For a normal DNS query:

```text
Kali → DNS Server
```

the DNS server is the **destination**.

For the response:

```text
DNS Server → Kali
```

the DNS server is the **source**.

For example:

```text
DNS Query

Source:
192.168.1.10

Destination:
192.168.1.1
```

Then:

```text
DNS Reply

Source:
192.168.1.1

Destination:
192.168.1.10
```

This is exactly why identifying the DNS server is useful when you later examine packet captures.

---

# Complete Example

Suppose your Kali VM gives the following results.

### `pwd`

```bash
$ pwd
/home/kali
```

### `ifconfig`

```text
eth0:
    inet 10.6.6.1
    netmask 255.255.255.0
    ether 08:00:27:AA:BB:CC
```

### `ip route`

```text
default via 10.6.6.254 dev eth0
10.6.6.0/24 dev eth0 src 10.6.6.1
```

### `/etc/resolv.conf`

```text
nameserver 10.6.6.254
```

You would record:

| Item               | Result              |
| ------------------ | ------------------- |
| Current directory  | `/home/kali`        |
| Ethernet interface | `eth0`              |
| Kali IPv4          | `10.6.6.1`          |
| Kali MAC           | `08:00:27:AA:BB:CC` |
| Default gateway    | `10.6.6.254`        |
| DNS server         | `10.6.6.254`        |

**Those are example values only.**

---

# Complete Command Sequence

You can perform this entire section with:

```bash
pwd
```

then:

```bash
ifconfig
```

then:

```bash
ip route
```

then:

```bash
cat /etc/resolv.conf
```

If `ifconfig` isn't available:

```bash
ip addr
```

---

# What You Should Record Before Moving On

Your lab worksheet should eventually contain:

```text
Current directory:
________________________

Ethernet interface:
________________________

Kali IPv4 address:
________________________

Kali MAC address:
________________________

Default gateway:
________________________

Default DNS server:
________________________
```

The **IP address, MAC address, gateway, and DNS address are environment-dependent**, so there is no single correct value for those fields. The correct answer is whatever your Kali VM actually reports.

### The important relationship

```text
                 Kali
        ┌─────────────────────┐
        │ IP: 10.6.6.1        │
        │ MAC: AA:BB:CC:...   │
        └─────────┬───────────┘
                  │
          Local Ethernet
                  │
        ┌─────────▼───────────┐
        │ Default Gateway     │
        │ 10.6.6.254          │
        └─────────┬───────────┘
                  │
               Internet
```

And for DNS:

```text
Kali
  │
  │ DNS query
  ▼
DNS Server
  │
  │ DNS reply
  ▼
Kali
```


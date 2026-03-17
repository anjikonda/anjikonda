# 🌐 Chapter 02 — Networking Fundamentals

> **Level:** Beginner | **Read time:** 20 minutes
> Understanding networks is the foundation of all cybersecurity work.

---

## 📌 What You Will Learn

- What a network is and how the internet works
- IP addresses — your digital home address
- DNS — the internet's phone book
- How data travels — TCP vs UDP
- The OSI Model — 7 layers explained simply
- Ports — doors into a computer
- Why all this matters for hacking and defence

---

## 1️⃣ What is a Network?

![Computer network cables and connections](https://images.unsplash.com/photo-1558494949-ef010cbdcc31?w=800&q=80)

A network is simply **two or more computers connected together to share information**.

Your home has a small network right now:

```
Your phone   ──┐
Your laptop  ──┼──→ WiFi Router ──→ Internet ──→ Google, YouTube, GitHub
Your TV      ──┤
Your tablet  ──┘
```

The **internet** is nothing magical. It is just millions of these small home and office networks all connected to each other through cables, fibre, and satellites spread across the entire world.

**Types of networks:**

| Type | Full Name | What It Is |
|------|-----------|------------|
| **LAN** | Local Area Network | Your home or office network — small area |
| **WAN** | Wide Area Network | A network covering a large area — the internet is a WAN |
| **WiFi** | Wireless LAN | A LAN without cables — uses radio waves |
| **VPN** | Virtual Private Network | A private encrypted tunnel through the public internet |

---

## 2️⃣ IP Address — Your Digital Home Address

![IP address and internet routing concept](https://images.unsplash.com/photo-1544197150-b99a580bb7a8?w=800&q=80)

Every device on a network needs a unique address so that data knows where to go. This address is called an **IP address** (Internet Protocol address).

Think of it exactly like your home address:

```
Your real home address:
   Flat 4B, Some Street, Hyderabad, Telangana 500001

Your device's address on the internet:
   192.168.1.5   (inside your home network)
   103.21.58.199 (your public address on the internet)
```

---

### Private IP vs Public IP

| Type | Where It Works | Example | Who Sees It |
|------|---------------|---------|-------------|
| **Private IP** | Inside your home network only | 192.168.1.5 | Only devices in your house |
| **Public IP** | On the internet | 103.21.58.199 | The whole internet |

**Your router's job:**
Your WiFi router has one public IP address shared by the whole house. When your phone makes a request, the router tracks it and sends the reply back to the right device. This is called **NAT** (Network Address Translation).

```
Phone (192.168.1.2) ──→ Router (Public IP: 103.21.58.199) ──→ Google
                    ←───────────────────────────────────── ←──
```

---

### IPv4 vs IPv6

The internet is running out of IPv4 addresses — there are only about 4 billion of them and the world has more than 4 billion devices.

```
IPv4  →  192.168.1.1
         (4 groups of numbers, separated by dots)
         (about 4.3 billion possible addresses)

IPv6  →  2001:0db8:85a3:0000:0000:8a2e:0370:7334
         (8 groups of letters and numbers)
         (340 trillion trillion trillion possible addresses — basically unlimited)
```

---

### Special IP Addresses to Know

```
127.0.0.1        → Loopback / Localhost — means "this computer itself"
                   Used to test network software on your own machine

192.168.x.x      → Private home/office network range
10.x.x.x         → Private network range (larger organisations)
172.16.x.x       → Private network range

0.0.0.0          → Represents "all network interfaces"
255.255.255.255  → Broadcast — send to everyone on the network
```

> 🔒 **Security note:** When hackers "scan" a network, they are discovering what IP addresses exist and what devices are connected. This is the very first step of any attack — or any penetration test.

---

## 3️⃣ DNS — The Internet's Phone Book

![DNS server and domain name system concept](https://images.unsplash.com/photo-1551703599-6b3e8379aa8c?w=800&q=80)

You remember people's names, not their phone numbers. When you want to call someone, you look up their name in your contacts and it gives you the number.

**DNS does exactly this for websites.**

```
You type into browser:   google.com
DNS looks it up and says: "google.com is at IP address 142.250.80.46"
Your browser connects to: 142.250.80.46
Google loads on screen:   ✅
```

Without DNS, you would have to memorise `142.250.80.46` instead of `google.com`. That would be impossible for thousands of websites.

---

### How DNS Resolution Works Step by Step

```
Step 1: You type "google.com" in your browser

Step 2: Your computer checks its own memory (cache)
        "Do I already know the IP for google.com?"
        → If yes: use it directly (fast)
        → If no: continue to Step 3

Step 3: Ask your Router
        Router checks its cache
        → If yes: use it
        → If no: ask the ISP's DNS server

Step 4: Ask ISP's DNS Server (Resolver)
        → Checks its cache
        → If not found: ask the Root DNS Server

Step 5: Root DNS Server says:
        "I don't know google.com, but I know who handles .com"
        → Points to the .com Name Server

Step 6: .com Name Server says:
        "I don't have the exact address, but Google's name server does"
        → Points to Google's Name Server

Step 7: Google's Name Server says:
        "google.com = 142.250.80.46" ✅

Step 8: Your browser connects to 142.250.80.46 → Google loads
```

This entire process happens in **milliseconds** — you never notice it.

---

### DNS Attacks to Know

| Attack | What Happens |
|--------|-------------|
| **DNS Spoofing / Poisoning** | Hacker corrupts DNS data — when you type `yourbank.com`, DNS sends you to a fake copy of the site |
| **DNS Hijacking** | Hacker changes your DNS settings — all your lookups go through their server |
| **DNS Amplification** | Used in DDoS attacks — small DNS requests produce huge responses, flooding a target |

> 🔒 **Security note:** Always use encrypted DNS — **DNS over HTTPS (DoH)** or **DNS over TLS (DoT)**. Good free DNS providers: Cloudflare (1.1.1.1) and Google (8.8.8.8).

---

## 4️⃣ How Data Travels — TCP and UDP

![Data packets travelling through network](https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=800&q=80)

When you send anything over the internet — a message, a photo, a webpage — it does not travel as one whole piece. It gets broken into small pieces called **packets**.

```
You send the message: "Hello Anji, how are you?"

The network breaks it into packets:
  Packet 1: "Hello A"   → travels via one route → ─────────┐
  Packet 2: "nji, how"  → travels via another route → ─────┼──→ Reassembled:
  Packet 3: " are you?" → travels via another route → ─────┘    "Hello Anji, how are you?"
```

Each packet has a header containing:
- Where it came from (source IP)
- Where it is going (destination IP)
- What number packet it is
- The data itself

---

### TCP vs UDP — Two Ways to Send Packets

| Feature | TCP | UDP |
|---------|-----|-----|
| **Full name** | Transmission Control Protocol | User Datagram Protocol |
| **Reliability** | ✅ Guarantees delivery | ❌ No guarantee |
| **Speed** | Slower (checks everything) | Faster (just sends) |
| **Order** | Packets arrive in correct order | Packets can arrive in any order |
| **Error checking** | Yes — resends lost packets | No — lost packets stay lost |
| **Used for** | Websites, email, file downloads | Video calls, online gaming, live streams |

**Simple analogy:**
- **TCP** = Registered post. You get a confirmation that the letter was delivered. If it gets lost, it is resent.
- **UDP** = Regular post. You drop it in the box and hope for the best. No tracking, no guarantee.

**For live video calls**, UDP is better — if one packet arrives late, it is better to skip it than to pause the whole call waiting for it to be resent.

---

### The TCP 3-Way Handshake

Before TCP sends any real data, it sets up the connection with a 3-step process:

```
                    SYN
Your computer ──────────────────→ Server
              "Can we connect?"

                    SYN-ACK
Your computer ←────────────────── Server
              "Yes, I'm ready"

                    ACK
Your computer ──────────────────→ Server
              "Great, let's go"

              ════ Data flows now ════
```

- **SYN** = Synchronise — "I want to connect"
- **ACK** = Acknowledge — "I received your message"

> 🔒 **Security note:** A **SYN Flood attack** is a type of DDoS where an attacker sends thousands of SYN packets but never completes the handshake. The server keeps waiting for the ACK that never comes, using up all its resources.

---

## 5️⃣ The OSI Model — 7 Layers of a Network

![Network OSI model layers concept](https://images.unsplash.com/photo-1544197150-b99a580bb7a8?w=800&q=80)

The OSI Model (Open Systems Interconnection) is a way to understand how data travels from your computer to another computer. Think of it as 7 floors in a building — data travels down all 7 floors on the sender's side, and back up all 7 floors on the receiver's side.

```
SENDER (Your computer)           RECEIVER (Google's server)
──────────────────────           ──────────────────────────

┌─────────────────────┐          ┌─────────────────────┐
│  7. APPLICATION     │          │  7. APPLICATION     │
│  What the user sees │          │  Delivers to app    │
│  HTTP, HTTPS, DNS   │          │                     │
├─────────────────────┤          ├─────────────────────┤
│  6. PRESENTATION    │          │  6. PRESENTATION    │
│  Translates format  │          │  Translates format  │
│  Encrypts (SSL/TLS) │          │  Decrypts           │
├─────────────────────┤          ├─────────────────────┤
│  5. SESSION         │          │  5. SESSION         │
│  Opens/closes conn  │          │  Manages session    │
├─────────────────────┤          ├─────────────────────┤
│  4. TRANSPORT       │          │  4. TRANSPORT       │
│  Breaks into packets│          │  Reassembles packets│
│  TCP / UDP / Ports  │          │                     │
├─────────────────────┤          ├─────────────────────┤
│  3. NETWORK         │          │  3. NETWORK         │
│  Adds IP addresses  │          │  Reads IP addresses │
│  Routing            │          │                     │
├─────────────────────┤          ├─────────────────────┤
│  2. DATA LINK       │          │  2. DATA LINK       │
│  MAC addresses      │          │  MAC addresses      │
│  Switches           │          │                     │
├─────────────────────┤          ├─────────────────────┤
│  1. PHYSICAL        │          │  1. PHYSICAL        │
│  Cables, WiFi signal│          │  Cables, WiFi signal│
└─────────────────────┘          └─────────────────────┘
         │                                 ▲
         └──── Data travels as bits ───────┘
               through cables/WiFi
```

---

### Each Layer Simply Explained

**Layer 7 — Application**
This is what you actually see and use. When you open a browser and type a URL, that is Layer 7. Protocols here: HTTP (websites), HTTPS (secure websites), DNS (domain names), FTP (file transfer), SMTP (email).

**Layer 6 — Presentation**
Translates data into a format the application can read. Also handles encryption and decryption. When HTTPS encrypts your connection, that happens here.

**Layer 5 — Session**
Opens, manages, and closes connections between two devices. Handles login sessions — for example, keeping you logged into a website even as you browse between pages.

**Layer 4 — Transport**
Breaks data into packets and numbers them. Uses TCP or UDP. Manages ports (which application gets the data). This is where error checking and retransmission happens (TCP).

**Layer 3 — Network**
Handles IP addresses and routing — figuring out the best path for packets to travel from source to destination. Routers operate at this layer.

**Layer 2 — Data Link**
Handles MAC addresses (the hardware address of a network card). Switches operate here. Responsible for delivering data on the local network.

**Layer 1 — Physical**
The actual physical stuff — cables, fibre optic, radio waves for WiFi, electrical signals. Just raw bits (0s and 1s) moving from one place to another.

---

### Memory Trick to Remember the 7 Layers

```
Top to bottom (7 → 1):
All People Seem To Need Data Processing
A  P      S     T  N     D    P

Bottom to top (1 → 7):
Please Do Not Throw Sausage Pizza Away
P      D  N   T     S       P     A
```

---

### Why OSI Matters for Security

Every type of attack happens at a specific layer:

```
Layer 7 — Application:  SQL Injection, XSS, Phishing, HTTPS attacks
Layer 6 — Presentation: SSL stripping attacks
Layer 5 — Session:      Session hijacking
Layer 4 — Transport:    SYN flood, port scanning
Layer 3 — Network:      IP spoofing, routing attacks
Layer 2 — Data Link:    ARP spoofing, MAC flooding
Layer 1 — Physical:     Cable tapping, hardware tampering
```

When a security professional says "this is a Layer 3 attack" — they mean it targets IP addresses and routing.

---

## 6️⃣ Ports — The Doors Into a Computer

Think of a computer like a large building. The IP address tells you which building to go to. But the building has many different doors — each door is for a specific purpose. Those doors are called **ports**.

```
IP address = the building's street address
Port number = the specific door inside the building

192.168.1.5 : 80    → Building 192.168.1.5, Door 80 (website)
192.168.1.5 : 22    → Building 192.168.1.5, Door 22 (SSH login)
192.168.1.5 : 443   → Building 192.168.1.5, Door 443 (secure website)
```

There are **65,535 possible ports** on every computer.

---

### Most Important Ports to Know

```
Port 21    → FTP        File Transfer Protocol (transfers files)
Port 22    → SSH        Secure Shell (remote computer access — encrypted)
Port 23    → Telnet     Old remote access (NOT encrypted — dangerous!)
Port 25    → SMTP       Email sending
Port 53    → DNS        Domain Name System
Port 80    → HTTP       Normal websites (not encrypted)
Port 110   → POP3       Email receiving
Port 143   → IMAP       Email receiving (better version)
Port 443   → HTTPS      Secure websites (encrypted — always prefer this)
Port 3306  → MySQL      Database
Port 3389  → RDP        Remote Desktop Protocol (Windows remote access)
Port 8080  → HTTP Alt   Alternative web server port
```

---

### Open vs Closed vs Filtered Ports

When hackers or penetration testers scan a computer, they look at port states:

| State | Meaning |
|-------|---------|
| **Open** | A service is running and accepting connections — potential entry point |
| **Closed** | No service running, but the computer responded — computer is reachable |
| **Filtered** | No response — firewall is blocking the scan |

> 🔒 **Security rule:** Close every port that does not need to be open. Every open port is a potential door for an attacker.

---

### Scanning Ports with Nmap

**Nmap** is the most widely used tool for discovering open ports. Every ethical hacker uses it.

```bash
nmap 192.168.1.1              # Basic scan — which common ports are open?
nmap -sV 192.168.1.1          # What software is running on each open port?
nmap -A 192.168.1.1           # Full scan — OS, versions, services, scripts
nmap -p 1-65535 192.168.1.1   # Scan ALL 65,535 ports
nmap 192.168.1.0/24           # Scan every device on your home network

Example output:
PORT     STATE  SERVICE    VERSION
22/tcp   open   ssh        OpenSSH 8.2
80/tcp   open   http       Apache httpd 2.4.41
443/tcp  open   https      nginx 1.18.0
3306/tcp open   mysql      MySQL 8.0.26
```

---

## 7️⃣ Common Network Attacks

### 🔍 Port Scanning

**What:** Sending probes to every port to see which ones respond (are open).
**Why hackers do it:** Open ports reveal what services are running, giving hints about how to attack.
**Defence:** Use a firewall to close all unnecessary ports and hide others.

---

### 🎭 ARP Spoofing

**What:** ARP (Address Resolution Protocol) translates IP addresses into MAC addresses on a local network. An attacker sends fake ARP messages to associate their MAC address with another computer's IP.

**Result:** All traffic meant for a victim gets redirected to the attacker first — a Man-in-the-Middle attack on a local network.

---

### 🌊 SYN Flood

**What:** Send thousands of TCP SYN packets (connection requests) but never complete the handshake. The server keeps resources allocated waiting for connections that never finish. Eventually the server runs out of resources and crashes.

**This is a type of DoS (Denial of Service) attack.**

---

### ☠️ IP Spoofing

**What:** Sending packets with a fake (spoofed) source IP address. Used to hide the attacker's identity or to impersonate a trusted computer.

---

## 8️⃣ Networking Tools Every Security Person Uses

| Tool | What It Does |
|------|-------------|
| **Nmap** | Port scanning and network discovery — the most important tool |
| **Wireshark** | Captures and analyses all network traffic in real time |
| **Netcat** | The "Swiss Army knife" of networking — sends/receives raw data |
| **Traceroute** | Shows every hop a packet takes to reach a destination |
| **Dig / nslookup** | DNS lookup tools — find IP behind a domain name |
| **Ping** | Check if a host is reachable and measure response time |

---

## 9️⃣ Networking Quick Reference

```bash
# Check your own IP address
ip a                    (Linux)
ipconfig                (Windows)

# Check if a website is reachable
ping google.com

# Find the IP address of a domain
nslookup google.com
dig google.com

# Trace the path packets take to a destination
traceroute google.com   (Linux)
tracert google.com      (Windows)

# Check what ports are open on your own computer
netstat -tulnp          (Linux)
netstat -an             (Windows)

# Scan a network (use only on networks you own or have permission)
nmap 192.168.1.0/24
```

---

## ✅ Quick Revision — Chapter 02

```
Network        = Computers connected to share information
IP Address     = Unique address of a device on a network
Private IP     = Works only inside your home/office network
Public IP      = Your address on the internet (router's address)
DNS            = Translates domain names (google.com) to IP addresses
Packet         = A small piece of data travelling through a network
TCP            = Reliable, slower — checks packets arrived correctly
UDP            = Fast, unreliable — used for video calls and games
OSI Model      = 7 layers that explain how network communication works
Port           = A specific door/channel into a computer (0–65535)
Port 22        = SSH (secure remote access)
Port 80        = HTTP (websites)
Port 443       = HTTPS (secure websites — always prefer this)
Nmap           = Tool for scanning open ports on a network
Wireshark      = Tool for capturing and reading network traffic
```

---

## ➡️ Next Chapter

**[🐧 Chapter 03 — Linux for Hackers →]**

Learn the Linux command line — the operating system used by almost every hacker, server, and security tool in the world.

---

*📅 Written: March 2025 | 👤 Anji | Hyderabad, India*
*⭐ Star this repo if these notes helped you!*

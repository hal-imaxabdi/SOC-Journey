# Week 3 Notes — Networking, Firewalls, IDS, and IPS

This week was very technical. Networking is one of those topics where if you do not understand the foundation, everything else in cybersecurity becomes harder. We also got into firewalls properly, and did hands-on work in Wireshark and on the Windows Defender Firewall. A lot to cover, but it clicked once the analogies started making sense.

---

## Why Networking Matters for Cybersecurity

You cannot protect something you do not understand. Almost every attack — phishing, man-in-the-middle, DDoS, data exfiltration — happens over a network. If you want to work in a SOC, understanding how traffic flows is not optional. It is the foundation.

Networking shows up in:
- Identifying what type of attack is happening and where
- Analyzing logs and packet captures
- Understanding firewalls, routers, and switches
- Pen testing (reconnaissance)
- Cloud security
- Almost every certification you will take

---

## What is a Network?

A network is simply two or more devices connected to each other for the purpose of communication or sharing resources. Your phone connected to your laptop via hotspot is a network. Your home router connecting five devices is a network.

The internet is not a separate thing — it is just the largest network in existence, made up of millions of smaller networks all connected together. Nobody owns the internet, but organizations like the IETF (Internet Engineering Task Force) help maintain the rules that keep it running.

---

## Network Components

**End devices** — Where data originates and where it arrives. Your phone, laptop, PC. These are the sources and destinations.

**Intermediary devices** — The devices in between that move data around:
- **Switch** — Connects devices within a local network, uses MAC addresses
- **Router** — Directs traffic between networks, uses IP addresses
- **Firewall** — Controls what traffic is allowed in or out
- **Wireless Access Point** — Provides Wi-Fi connectivity

**Network media** — The medium through which data travels:
- **Cables (copper)** — Use electrical signals, example: USB, Ethernet
- **Fiber optic** — Use pulses of light, used for long-distance and internet backbone connections
- **Wireless** — Use electromagnetic waves and radio frequencies, example: Wi-Fi, Bluetooth

---

## Types of Networks

**By size:**
- Small/Home Office (SOHO) — A few devices in a home or small office
- Medium to Large — Organizations with multiple locations
- Worldwide — The internet

**By geography:**
- **LAN (Local Area Network)** — Covers a small area like a home or office building
- **WAN (Wide Area Network)** — Covers a large area, connecting multiple LANs together

The internet is essentially a massive collection of LANs and WANs all interconnected.

---

## Message Delivery Types

When a device sends data, there are three ways it can be delivered:

**Unicast** — One-to-one. You send a message to one specific person. Like a private DM on WhatsApp. The recipient is a specific address.

**Multicast** — One-to-many, with replies possible. Like a WhatsApp group. You send to multiple people and they can all respond. Everyone in the group sees everyone else's messages.

**Broadcast** — One-to-all, no reply expected back to the group. Like a WhatsApp broadcast list. You send to everyone on the list, but if they reply, they reply directly to you — others do not see their reply. Like a radio or TV station.

---

## Network Protocols

Protocols are the rules that define how communication happens across a network. Without protocols, devices from different manufacturers would not be able to talk to each other.

When you type a URL into your browser, protocols define exactly how that request is formatted, sent, received, and responded to.

Common protocols to know:
- **HTTP** (HyperText Transfer Protocol) — Port 80, how websites are accessed, unencrypted
- **HTTPS** (HTTP Secure) — Port 443, encrypted version of HTTP
- **SSH** — Port 22, secure remote access to systems
- **FTP** — Port 21, file transfers
- **SMTP** — Port 25, sending emails
- **DNS** — Translates domain names (google.com) into IP addresses that computers can use

There are 65,535 ports in total. Different services run on different ports. Knowing the common port numbers matters for log analysis and understanding network traffic.

---

## The OSI Model

The OSI (Open Systems Interconnection) model breaks down how network communication works into 7 layers. Each layer has a specific job.

Mnemonic from class — starting from the bottom: **Please Do Not Throw Sausage Pizza Away**

| Layer | Name | What it does |
|---|---|---|
| 1 | Physical | Transmits raw bits (0s and 1s) over cables or wireless |
| 2 | Data Link | Organizes bits into frames, uses MAC addresses, handles switches |
| 3 | Network | Handles logical addressing (IP addresses) and routing |
| 4 | Transport | Ensures reliable delivery, checks for errors |
| 5 | Session | Manages the connection between devices |
| 6 | Presentation | Formats/translates data so it can be understood |
| 7 | Application | Where user-facing protocols live (HTTP, DNS, SMTP) |

**Binary and how computers read data:** Computers do not understand English. Everything is converted to 0s and 1s. A 0 means no electrical pulse, a 1 means there is a pulse. Everything you type, every file you open, every website you visit — the computer is processing all of it as combinations of zeros and ones.

---

## The TCP/IP Model

The TCP/IP model is simpler — it combines the 7 OSI layers into 4 layers and is the model actually used on the internet today.

| Layer | Name | What it covers |
|---|---|---|
| 1 | Network Access | Physical connections and hardware |
| 2 | Internet | IP addressing and routing |
| 3 | Transport | Reliable delivery (TCP) or fast delivery (UDP) |
| 4 | Application | Everything the user interacts with (HTTP, DNS, etc.) |

---

## Encapsulation and De-encapsulation

**Encapsulation** — When you send a request (like loading google.com), your data travels *down* through the layers from Application to Physical. At each layer, additional information is added to the data (like a header saying where it is going and how to handle it).

**De-encapsulation** — When the data arrives at its destination, it travels *up* through the layers from Physical to Application. At each layer, the added information is stripped off until the original data is delivered to the application that needs it.

Think of it like packing a letter: you write the letter (application), put it in an envelope (transport), address it (network), hand it to the post office (physical). When it arrives, everything gets unwrapped in reverse until the person has just the letter.

---

## IP Addresses and MAC Addresses

**IP Address** — A logical address assigned to a device when it connects to a network. It can change. This is how devices find each other on the internet. Like a home address that can change if you move.

**MAC Address** — A physical address burned into the hardware of the device itself. It does not change. Every network card has a unique MAC address. Like a serial number stamped on the device.

To find your IP address:
- Windows: open Command Prompt and type `ipconfig`
- Linux/Mac: open Terminal and type `ifconfig`

The loopback address (127.0.0.1) is a special IP address that always points back to your own device. Also called localhost or home address.

---

## Wireshark — Seeing Network Traffic in Real Time

Wireshark is a packet capture tool. It lets you see every packet of data moving through your network in real time. This is a core tool for network analysis and incident response.

**What we did in class:**

1. Opened Wireshark and selected the Wi-Fi interface
2. Clicked the blue shark fin icon to start capturing traffic
3. Opened a browser and visited an HTTP site (the vulnerable login page at testasp.net)
4. Typed in a username and password on the HTTP site
5. Stopped the capture
6. Filtered by typing `http` in the display filter
7. Found the login request, right-clicked → Follow → HTTP Stream
8. Scrolled down and saw the username and password in plain text

**What this demonstrated:** HTTP is not encrypted. Every piece of data you send over HTTP — including usernames and passwords — is visible to anyone who can capture that traffic. This is a serious vulnerability.

**Then we filtered for HTTPS traffic (port 443):**
- Right-clicked a packet → Follow → HTTP Stream
- The content was completely unreadable — just cipher text
- This is because HTTPS uses TLS (Transport Layer Security) to encrypt all data

**The takeaway:** Never type sensitive information into a website that uses plain HTTP. Always check that the URL starts with HTTPS before entering any passwords or personal data.

---

## Firewalls

A firewall is a logical barrier that enforces a set of rules to control the flow of network traffic. It decides what is allowed in and what is blocked.

Think of it like a bouncer at a wedding — they check whether you are on the list, and either let you in or turn you away.

### Types of Firewalls

**Packet Filtering Firewall**
Operates at the router level. Checks each packet against a set of rules based on IP address and port number. Simple and fast but limited — it can be fooled if someone spoofs an IP address. It does not understand context, just the basic details of each packet.

**Proxy Firewall**
Sits between the user's device and the internet. The firewall receives the request on your behalf, checks it, then forwards it if it passes. The destination server never communicates directly with your device — only with the proxy. Like switching cars mid-journey to shake someone following you.

**Web Application Firewall (WAF)**
Specifically designed for HTTP and HTTPS traffic. Sits between a web application and the internet. Checks whether requests are legitimate or malicious. This is what projects like the WAF HomeLab in my portfolio use.

**Stateful Inspection Firewall**
Tracks the entire connection, not just individual packets. It knows who connected, when, and what they have been doing. Like a bouncer who remembers faces — if you left to use the bathroom and come back without your invitation, they still recognize you and let you in.

**Next Generation Firewall (NGFW)**
Combines traditional firewall functions with an IPS (Intrusion Prevention System). Not only does it block unauthorized traffic, it can actively prevent and respond to attacks. Like a bouncer who is also a police officer — they can arrest people, not just turn them away.

### DMZ — Demilitarized Zone

A DMZ is a neutral network zone that sits between the public internet and the private internal network. Public-facing services (like web servers) live here, so if they are compromised, the attacker still cannot reach the internal private network.

Think of it like a reception area in an office building. Visitors from outside can sit in the reception — they are inside the building, but they cannot access the private offices or boardrooms. The internal staff can come to reception and meet them there, but the visitor never enters the private areas.

Traffic from inside (private network) to the DMZ has little restriction. Traffic from outside (internet) to the private network is blocked entirely.

### Firewall Rules and Default Policies

Firewalls work by following rules. These rules define what is allowed and what is denied based on:
- Source and destination IP address
- Port number
- Protocol (TCP/UDP)
- Direction (inbound/outbound)

**Default deny policy:** If a packet does not match any rule that explicitly allows it, it is automatically blocked. This is the safest default — deny everything unless explicitly permitted.

### Firewall Deployment Types

- **Network-based** — Installed at the perimeter of the network, protects everything behind it
- **Host-based** — Installed on individual devices (like Windows Defender Firewall), protects that specific device
- **Cloud** — Deployed in cloud environments to protect cloud-based resources

---

## Windows Firewall Demo

We used `wf.msc` (Windows Defender Firewall with Advanced Security) to create an outbound rule that blocks HTTP traffic on port 80.

**Steps:**
1. Press Windows key + R, type `wf.msc`, press Enter
2. Click Outbound Rules on the left
3. Click New Rule under Actions
4. Select Port → Next
5. Select TCP, type `80` in Specific remote ports → Next
6. Select Block the connection → Next
7. Leave all profiles checked → Next
8. Name it BlockHTTP → Finish

**Result:** After enabling the rule, trying to visit an HTTP site returned "internet access is blocked." Disabling the rule allowed the site to load again.

---

## IDS — Intrusion Detection System

An IDS monitors network traffic and system activity for signs of malicious behavior. When it detects something suspicious, it raises an alarm so a human analyst can investigate.

Key point: an IDS detects and alerts. It does not automatically stop anything.

### Detection Methods

**Signature-based detection (Misuse detection)**
Looks for known attack patterns — patterns it has seen before. Like a bouncer who has memorized the tactics of known troublemakers. Very effective against known attacks, but completely blind to new ones (zero-day attacks).

Advantage: high accuracy for known threats
Disadvantage: cannot detect attacks it has never seen before

**Anomaly-based detection**
First defines what "normal" looks like for the network (a baseline). Then raises an alert whenever something deviates from that baseline. Like coming home and noticing your lights are off even though you always leave them on — something is wrong.

Advantage: can detect new, unknown attacks
Disadvantage: can produce false positives if normal activity changes

**Hybrid detection**
Combines both signature and anomaly detection. If either method flags something, an alert is raised.

### Types of IDS

- **Host-based IDS (HIDS)** — Installed on a single device, monitors that specific machine
- **Network-based IDS (NIDS)** — Monitors traffic across the entire network

---

## IPS — Intrusion Prevention System

An IPS does everything an IDS does, but goes one step further — it actively prevents attacks, not just detects them.

IDS: detects and alerts
IPS: detects, alerts, and blocks

Like a bouncer who is also a police officer. They do not just turn people away — they can arrest them so they cannot try again.

An IPS can use signature-based, anomaly-based, policy-based, and protocol analysis-based detection methods.

---

## Network Segmentation

Network segmentation is dividing a network into smaller, isolated sections. Each section can operate independently.

Why this matters: if one segment is compromised, the attacker cannot automatically reach other segments. It is like quarantine — you isolate the infected area to prevent it from spreading.

Benefits:
- **Improved security** — Contains breaches to a smaller area
- **Better performance** — Less traffic congestion within each segment
- **Simplified management** — Smaller networks are easier to monitor and manage

**Physical segmentation** — Separate hardware for each segment
**Logical segmentation** — Uses software (like VLANs) to separate traffic on the same physical hardware

---

## Defense in Depth

Defense in depth means using multiple layers of security rather than relying on one. If one layer fails, the others are still protecting the asset.

Like a house with a gate, CCTV, an alarm system, a deadbolt lock, and a biometric scanner. An attacker has to defeat every layer. Each layer makes it harder.

Common layers:
- Firewall
- IDS/IPS
- Antivirus
- Encryption
- Strong passwords and MFA
- Regular updates and patching
- User training

---

## What I Practiced This Week

- Captured live network traffic using Wireshark
- Observed HTTP credentials transmitted in plain text on a vulnerable login site
- Compared encrypted HTTPS traffic (unreadable cipher text) vs unencrypted HTTP
- Created a Windows Defender Firewall outbound rule using wf.msc to block port 80
- Verified the rule worked by attempting to access an HTTP site and seeing it blocked
- Disabled the rule and confirmed the site loaded again

---

## What I Want to Revisit

- iptables on Linux — the demo ran into network issues but the concept of blocking port 80 from the command line is important
- TLS handshake — I want to understand exactly how HTTPS sets up an encrypted connection
- VLAN configuration — how logical network segmentation is set up in practice

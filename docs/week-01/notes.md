# Week 1 Notes — Cybersecurity Foundations

These are my notes from the first week of the TechCrush cybersecurity bootcamp. Three classes, a lot of concepts. I'm writing this in my own words so I can actually come back and understand it, not just copy-paste definitions.

---

## What is Cybersecurity?

At its core, cybersecurity is just taking the idea of security — protecting things that matter — and applying it to the digital world. Systems, networks, data, accounts. Anything that lives online or on a device needs to be protected from people who want to access, steal, damage, or destroy it.

It matters for everyone. Individuals, companies, hospitals, governments. Nobody is excluded from needing it.

---

## The CIA Triad

This is the foundation of everything in cybersecurity. Every decision, every control, every attack maps back to one or more of these three things.

**Confidentiality** — Only the right people can see the data. If someone who is not supposed to read something gets access to it, confidentiality has been broken. We protect confidentiality using passwords and encryption.

**Integrity** — Data should not be changed without permission. Whether the data is sitting somewhere (at rest) or moving between systems (in transit), it should arrive exactly as it was sent. We protect integrity using hashing and digital signatures.

**Availability** — Systems and data should be accessible whenever they are needed. If a hospital cannot access patient records because of an attack, availability has been broken. We protect availability using backups, redundancy, and access controls.

There is also a fourth principle worth knowing:

**Non-repudiation** — Both the sender and the receiver of something cannot deny that the exchange happened. There must be evidence that I sent it, and evidence that you received it. Digital signatures help with this.

One thing that surprised me early on: not every attack hits all three pillars. The Equifax breach in 2017 hit Confidentiality only — attackers copied data but changed nothing and broke nothing. WannaCry ransomware hit Integrity and Availability but not Confidentiality. Understanding which pillar is being targeted tells you a lot about what the attacker actually wanted.

---

## Key Concepts You Need to Know

**Assets** — Anything that is important and needs to be protected. Your phone, your files, your database, your server. If it has value, it is an asset.

**Threats** — Anything that can cause harm to an asset. This includes attackers, malware, natural disasters, even careless employees. A threat is the potential danger, not the damage itself.

**Vulnerabilities** — Weaknesses in a system that a threat can use to get in. A broken window is a vulnerability. An unpatched software bug is a vulnerability. Vulnerabilities are the access points.

**Risk** — The possibility that a threat will actually exploit a vulnerability and cause damage. Risk = threat + vulnerability. The higher the chance of it happening and the worse the damage would be, the higher the risk.

**Exploit** — The specific method an attacker uses to take advantage of a vulnerability. The vulnerability is the weakness; the exploit is how they use it.

**Attack Vector** — The path or method an attacker uses to reach the system. Could be email, a web browser, a USB drive, a phone call.

**Countermeasure** — Any action, tool, or process put in place to reduce threats or fix vulnerabilities. Updating software, training staff, setting up a firewall — all countermeasures.

**Patching** — The specific process of fixing a known vulnerability in software. When a company releases a security update, that update is a patch.

---

## Encryption and Hashing

These two come up constantly, so it is worth understanding them properly.

**Encryption** is converting readable data (plain text) into unreadable data (cipher text) so that only someone with the right key can read it. Think of it as translating a message into a language only the intended recipient understands. Decryption is reversing that process.

**Hashing** is different. When you hash a file, you run it through a function that produces a fixed output — called a hash value. The important thing about hashing is that if anything in the file changes, even one character, the hash value changes completely. This is how we check if a file has been tampered with. If the hash you calculated matches the original, the file is intact.

---

## AAA — Authorization, Authentication, Accountability

These three things control who gets access and what they can do.

**Authorization** — Confirming that you are allowed into the system at all. Like swiping your ID card to enter a building. It grants you access.

**Authentication** — Determining what you are allowed to do once inside. Your ID card might let you into the building, but your role determines whether you can enter the server room. Authentication defines your permissions.

**Accountability** — Keeping a record of what people did with their access. Who logged in, when, what they accessed, what they changed. This is how investigations happen after something goes wrong.

---

## Types of Cybersecurity

Cybersecurity is not one thing. It has different areas:

- **Network Security** — Protecting the infrastructure that data travels through
- **Application Security (AppSec)** — Securing software and web applications
- **Information Security (InfoSec)** — Protecting data itself
- **Operational Security (OPSEC)** — Protecting processes and workflows
- **Disaster Recovery and Business Continuity** — Making sure operations can continue after an incident

---

## Malware — Types and How They Work

Malware means malicious software. Any software designed to cause harm, steal data, or compromise a system.

**Virus** — Attaches itself to a host file. The virus will not spread until you interact with that file (open it, run it). Once activated, it spreads and can corrupt other files. It needs that human action to trigger.

**Worm** — Does not need a host file or any user action. It replicates and spreads through a network on its own. More dangerous in that sense because you do not have to do anything wrong to get hit.

**Ransomware** — Encrypts your files so you cannot access them, then demands payment (usually in crypto) for the decryption key. The lesson I took from this: even if you pay, attackers often do not send the key. The better defense is having backups.

**Trojan Horse** — Disguised as legitimate software. It will do whatever it is supposed to do (word processing, a game, a tool) but also carry out malicious activity on the side. Named after the Greek myth — it gets accepted because it looks legitimate.

**Spyware** — Monitors your activity without you knowing. Can record your keystrokes (this specific type is called a keylogger), track what sites you visit, access your camera or microphone, and send that data to an attacker.

**RAT (Remote Access Trojan)** — A specific type of Trojan that gives the attacker remote control over your system. They can see your screen, access your files, and control your device from anywhere.

**Rootkit** — Goes deep into the operating system and manipulates it to create a backdoor. Even after you think you have removed it, it can find its way back in because it has changed how the system works at the core level.

---

## Phishing and Social Engineering

Social engineering is non-technical hacking. Instead of exploiting a software vulnerability, you exploit a human. You manipulate someone into giving information they would not normally give, or doing something they should not do.

Phishing is the most common form. You trick someone into clicking a link, giving up a password, or opening an attachment. The key is that the target does not realize what is happening.

Variants based on the channel used:
- **Phishing** — Done via email
- **Smishing** — Done via SMS / text message
- **Vishing** — Done via voice calls
- **Quishing** — Done via QR codes

How to spot a phishing email:
- The sender's address looks almost right but is slightly off (rnicrusoft instead of microsoft)
- Creates urgency — "urgent action needed" instead of just informing you
- Contains typos or grammatical errors that a real company would not have
- Links use HTTP instead of HTTPS
- The link destination does not match what it claims to be

**Spoofing** is related — making something appear to come from a trusted source when it does not. An email can be spoofed to look like it came from your bank. A phone number can be spoofed.

---

## DOS and DDoS Attacks

**DoS (Denial of Service)** — Flooding a system with so much traffic that it cannot handle legitimate requests. If a website can serve 1,000 users at once and you send 10,000 requests at it, real users get shut out.

**DDoS (Distributed Denial of Service)** — Same idea but on a massive scale, using botnets. A botnet is a network of compromised devices (also called zombies) that the attacker controls. Because the attack comes from thousands of different sources, it is much harder to block.

---

## Man-in-the-Middle (MitM) Attacks

The attacker positions themselves between two parties communicating. They can monitor what is being sent, intercept it, change it, or impersonate one of the parties. Works especially well on insecure networks. This is one reason you should not trust public Wi-Fi for anything sensitive.

---

## Zero-Day Exploits

A zero-day is a vulnerability that nobody knew about before — including the software vendor. Because it is unknown, there is no patch for it yet. Attackers who find these have a window where they can exploit the vulnerability and nobody can stop them. The name comes from the fact that defenders have had zero days to prepare.

---

## Brute Force and Dictionary Attacks

**Brute force** — Trying every possible password combination until one works. Slow but thorough.

**Dictionary attack** — Instead of trying every combination, use a list of known passwords. The rockyou.txt file, for example, contains over 14 million real passwords that have been leaked from data breaches. If your password is on that list, a dictionary attack will find it.

This is why password policies matter: long, complex passwords with symbols and numbers that are not based on anything personal to you are much harder to crack.

---

## Threat Actors

Not all attackers are the same. Understanding who they are changes how you think about defending against them.

- **Hacktivists** — Politically or ideologically motivated
- **Cybercriminals** — Motivated by money
- **Nation-state actors** — Sponsored by governments, usually going after infrastructure or intelligence
- **Malicious insiders** — Employees or people with legitimate access who abuse it, either out of greed, revenge, or by accident
- **Script kiddies** — People who do not really know how to hack but use tools and scripts built by others

---

## Types of Hackers

- **Black hat** — No permission. Hacks for personal gain or malicious reasons
- **White hat** — Has explicit permission. Hired to find vulnerabilities before criminals do (ethical hackers, penetration testers)
- **Grey hat** — Falls in between. May hack without permission but will disclose what they found rather than exploit it
- **Red hat** — Fights against black hat hackers, sometimes without following conventional rules
- **Green hat** — Beginners who are still learning

---

## Cybersecurity Career Paths

There are four major areas to specialize in:

**SOC — Security Operations Center (Blue Team)**
The defensive side. Monitors for threats, responds to incidents, protects organizations from attacks in real time. Tools: SIEM, EDR, IDS, IPS, SOAR. This is the path I am most interested in.

**VAPT — Vulnerability Assessment and Penetration Testing (Red Team)**
The offensive side. Ethically hacks systems to find weaknesses before criminals do. Tools: Metasploit, Nmap, OWASP ZAP, Nessus.

**Digital Forensics**
Investigates incidents after they happen. Looks for evidence in digital media, traces how an attack occurred. Tools: Autopsy, FTK, Cellebrite (for mobile).

**GRC — Governance, Risk, and Compliance**
Helps organizations manage risk, design security policies, and comply with regulations. Tools: ServiceNow, RSA Archer, RiskConnect.

---

## Password Hygiene

A strong password should have:
- Uppercase and lowercase letters
- Numbers
- Special characters / symbols
- Minimum 8 characters (longer is better)
- Nothing tied to your personal information (name, date of birth, pet's name)
- Be unique — do not reuse passwords across accounts

Useful tools:
- **Have I Been Pwned** — Check if your email or password has appeared in a data breach
- **Bitwarden password strength tester** — Check how long a password would take to crack
- **VirusTotal** — Scan links before clicking them to check if they are malicious

---

## Backups — The 3-Location Rule

You should always have backups in three places:
1. **On-site** — Within the organization or on a local drive
2. **Off-site** — In a completely different physical location
3. **Online/Cloud** — On a cloud storage service

The reason for three: if one backup is compromised, you still have two others. Ransomware attacks are a good example of why this matters.

---

## What I Want to Revisit

- Hashing in more depth — I get the concept but want to understand MD5 vs SHA-256
- How SIEM tools actually work under the hood
- The Google Phishing Quiz I took for the assignment — I want to write up how I approached each email

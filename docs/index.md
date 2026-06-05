# 🛡️ SOC Fundamentals

> A structured, beginner-friendly learning repository for aspiring **Blue Team / SOC Analysts** — built and maintained by a cybersecurity student on the path to a SOC internship.

---

## 👋 Who Is This For?

This repo is for **anyone starting from zero** in cybersecurity who wants to work in a **Security Operations Center (SOC)**. Whether you're a student, a career switcher, or just curious about how defenders think, this is your starting point.

If you can answer "yes" to any of these, you're in the right place:
- You've heard terms like SIEM, IDS, or threat hunting and want to actually understand them
- You want a structured, week-by-week roadmap to build SOC skills
- You want to document your learning publicly to build a portfolio

---

## 🧠 What Is a SOC?

A **Security Operations Center (SOC)** is a team of cybersecurity professionals who monitor, detect, analyze, and respond to security threats- 24/7. Think of them as the defenders watching over an organization's digital environment.

As a SOC Analyst (Tier 1), your day-to-day involves:
- Monitoring alerts from a **SIEM** (Security Information and Event Management) tool
- Triaging and investigating suspicious activity
- Escalating confirmed threats to senior analysts
- Documenting findings and writing incident reports

---

## 🗺️ Learning Roadmap

This repo follows a structured, progressive roadmap. Each week builds on the last.

```
Phase 1 — Foundation (Weeks 1–3)
├── Week 01: SOC Roles, Workflows & the CIA Triad
├── Week 02: Networking Fundamentals for Defenders
└── Week 03: Log Analysis & Windows/Linux Basics

Phase 2 — Core Tools (Weeks 4–6)
├── Week 04: Introduction to SIEM (Splunk / Wazuh)
├── Week 05: Intrusion Detection Systems (Snort / Suricata)
└── Week 06: Packet Analysis with Wireshark

Phase 3 — Threat Knowledge (Weeks 7–9)
├── Week 07: MITRE ATT&CK Framework
├── Week 08: Common Attack Types (Phishing, Ransomware, Lateral Movement)
└── Week 09: Indicators of Compromise (IOCs) vs TTPs

Phase 4 — Hands-On Practice (Weeks 10–12)
├── Week 10: TryHackMe SOC Level 1 Labs
├── Week 11: LetsDefend Alert Triage Scenarios
└── Week 12: Capstone — Full Incident Response Walkthrough
```

> Each week folder contains: notes, lab writeups, diagrams, and key takeaways.

---

## 🔧 Prerequisites

You don't need to be an expert, but having these basics will help:

| Topic | Why It Matters | Free Resource |
|---|---|---|
| Basic Networking (IP, DNS, HTTP) | Almost every alert involves network traffic | [Professor Messer's CompTIA Network+](https://www.professormesser.com/) |
| Command Line (Windows + Linux) | Log analysis and tools are CLI-heavy | [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/) |
| What an OS does (processes, files, users) | Understanding attacker behavior requires OS knowledge | [TryHackMe: Pre-Security Path](https://tryhackme.com/path/outline/presecurity) |


---

## 🖥️ Setting Up Your Home Lab

Hands-on practice is non-negotiable. Here's a minimal setup to follow along with the labs:

### Step 1 — Install a Hypervisor
- [VirtualBox](https://www.virtualbox.org/) (free) or VMware Workstation Player (free for personal use)

### Step 2 — Download These VMs
- **Ubuntu Server 22.04** — for running Wazuh (free SIEM)
- **Windows 10 Evaluation** — for Windows log analysis
- **Kali Linux** — for generating test traffic (attacker perspective helps defenders)

### Step 3 — Install Wazuh (Free SIEM)
```bash
# On your Ubuntu VM, run:
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```
> Full guide: [https://documentation.wazuh.com/current/quickstart.html](https://documentation.wazuh.com/current/quickstart.html)

### Step 4 — Create Accounts on These Platforms
| Platform | What You'll Use It For |
|---|---|
| [TryHackMe](https://tryhackme.com) | Guided SOC labs (start with SOC Level 1 path) |
| [LetsDefend.io](https://letsdefend.io) | Real-world SOC alert simulation |
| [Blue Team Labs Online](https://blueteamlabs.online) | Incident response challenges |
| [CyberDefenders](https://cyberdefenders.org) | DFIR and threat hunting labs |

---

## 📚 Core Concepts You'll Learn

Here's a glossary of terms you'll encounter throughout this repo:

| Term | Definition |
|---|---|
| **SIEM** | Security Information & Event Management — aggregates and analyzes logs from across an environment |
| **IDS/IPS** | Intrusion Detection/Prevention System — monitors traffic for malicious patterns |
| **IOC** | Indicator of Compromise — evidence that a system may have been breached (e.g., suspicious IP, hash) |
| **TTP** | Tactics, Techniques & Procedures — how an attacker operates |
| **MITRE ATT&CK** | A framework mapping real-world attacker behaviors to categories |
| **Triage** | The process of prioritizing and investigating alerts |
| **Alert Fatigue** | When analysts are overwhelmed by too many alerts, causing real threats to be missed |
| **DFIR** | Digital Forensics & Incident Response |
| **Threat Hunting** | Proactively searching for threats that evaded automated detection |
| **Kill Chain** | A model describing the stages of a cyberattack (Recon → Weaponize → Deliver → Exploit → Install → C2 → Act) |

---

## 🎓 Certifications Roadmap

These are the most recognized certs for blue teamers, in order of difficulty:

```
Beginner
├── CompTIA Security+       — Industry baseline, widely required for SOC roles
├── Google Cybersecurity Certificate — Good free/cheap intro (Coursera)
│
Intermediate
├── CompTIA CySA+           — Specifically for SOC/threat analysts
├── Blue Team Level 1 (BTL1) — Highly practical, very respected
│
Advanced
├── GCIH (GIAC)             — Incident handling
└── OSCP (Offensive)        — Understanding attacker TTPs hands-on
```

> **Start with Security+.** It's the door-opener for most entry-level SOC positions.

---

## 📁 Repository Structure

```
SOC-Fundamentals/
│
├── README.md               ← You are here
│
├── week-01/
│   ├── notes.md            ← Concept notes
│   ├── lab-writeup.md      ← Hands-on lab documentation
│   └── diagrams/           ← Visual summaries
│
├── week-02/
│   └── ...
│
├── case-studies/           ← Real-world incident breakdowns (coming soon)
│   ├── solarwinds.md
│   └── log4shell.md
│
└── resources.md            ← Curated list of tools, links, and references
```

---

## 📖 How to Use This Repo

1. **Read the weekly notes** to understand concepts
2. **Do the labs** on TryHackMe or LetsDefend (linked in each week folder)
3. **Write your own writeup** — don't just copy notes, explain it in your own words
4. **Commit regularly** — consistency is more impressive than perfection

> 💡 Tip: Treat each week like a mini sprint. 5–10 hours/week is enough to make real progress.

---

## 🔗 Essential Resources

### Free Learning
- [TryHackMe SOC Level 1 Path](https://tryhackme.com/path/outline/soclevel1) — best structured beginner path
- [Cybrary SOC Analyst](https://www.cybrary.it/path/soc-analyst) — free tier available
- [SANS Cyber Aces](https://www.cyberaces.org/) — free OS and networking fundamentals

### Reference
- [MITRE ATT&CK](https://attack.mitre.org/) — attacker TTP framework
- [MITRE D3FEND](https://d3fend.mitre.org/) — defensive countermeasures
- [VirusTotal](https://www.virustotal.com) — analyze suspicious files/IPs/URLs
- [Shodan](https://www.shodan.io/) — internet-connected device search engine
- [Cisco Talos Intelligence](https://talosintelligence.com/) — threat intelligence

### Communities
- [r/netsec](https://reddit.com/r/netsec) and [r/blueteamsec](https://reddit.com/r/blueteamsec)
- [BlueTeamVillage Discord](https://blueteamvillage.org/)
- [SANS Internet Storm Center](https://isc.sans.edu/) — daily threat briefings

---

## 🙋 About Me

- 🎓 Information Technology student -Cybersecurity Major
- 🎯 Goal: Land a SOC Analyst internship / entry-level position
- 🔵 Focus: Blue Team | SOC Operations | Incident Response
- 📍 Documenting everything publicly to hold myself accountable and help others

---

## ⭐ Contributing / Feedback

This is a personal learning repo, but if you spot errors, have suggestions, or want to share resources — open an issue or drop a comment. Learning is better together.

---

*Last updated: March 2026 | Follow the journey week by week →*

# Week 8 — SOC, Ethical Hacking & Vulnerability Assessment
---

## Class 1 — Security Operations Center (SOC) Deep Dive

### What is a SOC?

The Security Operations Center is the defensive core of an organisation's cybersecurity posture. Think of it as the 911 of cybersecurity — the team that is first on the ground when something goes wrong. It is not a single person; it is a structured team with defined roles, working around the clock.

Key things the SOC does:
- Monitors the entire network for unusual activity
- Detects threats in real time using tools like SIEM, EDR, IDS/IPS
- Investigates alerts and triages them by severity
- Responds to and contains incidents
- Documents everything that happens

### SOC Workflows

The SOC does not operate randomly. It follows structured workflows:

**1. Monitoring and Alerting**  
The team watches the network continuously. When something anomalous happens — an unusual login, an unexpected file access — an alert is raised.

**2. Triaging**  
Not every alert is equally important. Triaging means sorting through alerts and deciding which ones need immediate attention. This is where false positives become a real problem — your tools flagging normal activity as suspicious wastes analyst time.

**3. Investigation**  
For the alerts that matter, analysts dig in. They check firewall logs, DNS records, network traffic — whatever it takes to find the source and scope of the problem.

**4. Response and Containment**  
Fix the problem. Isolate affected systems. Stop the spread. This is where the Incident Response Playbook is used.

**5. Recovery and Remediation**  
Bring systems back to normal functioning. Restore from backups if needed.

**6. Documentation and Reporting**  
If it is not written down, it did not happen. Every incident, every action taken, every decision made needs to be documented. This is one of the hardest habits to build as a technical person, but it is non-negotiable in a SOC.

**7. Threat Hunting and Proactive Defence**  
The SOC does not only react. It actively looks for threats that have not triggered an alert yet. You go looking for the problem before the problem finds you.

---

### SOC Metrics — How Do You Know If You're Doing Well?

These are the key performance indicators used in a SOC:

| Metric | What It Measures |
|---|---|
| **MTTD** (Mean Time to Detect) | How long does it take to identify a threat? |
| **MTTR** (Mean Time to Respond) | How long does it take to contain and fix it? |
| **False Positive Rate** | How often is the system raising unnecessary alerts? |
| **Number of Incidents** | How many threats were detected, investigated, and resolved? |
| **Analyst Utilisation** | How efficiently is the team working? |

A high false positive rate is a problem — it means analysts spend time chasing shadows instead of real threats. A high false negative rate is even worse — it means real threats are slipping through undetected.

---

### False Positives, False Negatives, True Positives, True Negatives

This is a concept that comes up in every SOC job interview, and it also shows up in certification exams.

| | **System says: Threat** | **System says: No Threat** |
|---|---|---|
| **Actually a threat** | ✅ True Positive | ❌ False Negative |
| **Not actually a threat** | ❌ False Positive | ✅ True Negative |

- **False Positive:** Someone logs in using a VPN from a different location. The system flags it as suspicious. It is not malicious — it is just the VPN. The system cried wolf.
- **False Negative:** There is actual malware in the system, but the detection tools miss it entirely. The attacker is inside and nobody knows. This is the dangerous one.
- **True Positive:** A real attack is detected correctly. This is what you want.
- **True Negative:** Normal activity is correctly identified as normal. No alert is raised.

---

### Compliance Frameworks

As a SOC analyst, you need to understand the regulatory environment. Organisations are required to follow different standards depending on their industry and location.

| Framework | What It Covers |
|---|---|
| **PCI DSS** | Payment Card Industry Data Security Standard — anyone handling card payments must protect that data |
| **ISO 27001** | International standard for information security management systems |
| **GDPR** | General Data Protection Regulation — protects personal data of EU citizens |
| **HIPAA** | Health Insurance Portability and Accountability Act — protects patient health data in the US |
| **SOX** | Sarbanes-Oxley — financial reporting standards |


---

### 3-2-1 Backup Rule

This came up in the context of SOC recovery procedures and is worth memorising:

- **3** copies of your data
- **2** different storage media types (e.g. hard drive + another medium)
- **1** copy stored off-site

The three locations are: on-site, off-site, and cloud. All three serve a different purpose. If ransomware hits your on-site copy, your off-site and cloud copies survive.

---

## Class 2 — Ethical Hacking & Advanced Persistent Threats

### Types of Hackers (by permission level)

There are more hacker categories than these, but the three core categories are defined by the level of permission they have:

**White Hat Hacker (Ethical Hacker)**  
Hacks with explicit permission. An organisation signs a contract granting them access to find and fix vulnerabilities. Same technical skills as other hackers — different intentions and legal standing.

**Black Hat Hacker**  
Hacks without permission, for personal gain, ideology, or other motivations. The direct threat that the white hat hacker is trying to get ahead of.

**Grey Hat Hacker**  
No permission — but hacks first, then approaches the target. They find vulnerabilities and then tell the organisation: "pay me, employ me, or I'll release this." Neither fully ethical nor fully criminal. Unstable — can swing either way.

Other categories you should know:
- **Green Hat:** A beginner, someone just learning. Script kiddie territory.
- **Blue Hat:** Leans towards the defensive/SOC side.
- **Red Hat:** Actively targets black hat hackers.

> The skill set between white hat and black hat hackers is often identical. What separates them is permission and intention.

---

### Penetration Testing Types

When an ethical hacker is hired to test a system, there are three approaches:

**Black Box Testing**  
The penetration tester is given no information about the system. They come in completely blind, just like a real attacker would. Maximum realism.

**White Box Testing**  
The tester is given full details — network diagrams, configuration files, database structures, everything. Maximum efficiency, minimum wasted time.

**Grey Box Testing**  
Partial information is shared. The tester knows some things but not everything. A middle ground between realism and efficiency.

---

### Advanced Persistent Threats (APTs)

Most malware is loud and fast — ransomware locks your files within minutes, a virus corrupts data quickly, and you know immediately something is wrong.

APTs are the opposite. They are designed to be silent.

**What makes an APT different:**
- They stay in a network for months or even years without being detected
- They are not trying to destroy or lock anything — they want to steal sensitive data quietly
- They are well-funded (nation-states and large criminal organisations sponsor them)
- They have specific, targeted goals
- They use multiple entry points rather than relying on one vulnerability

Think of an APT as a spy, not a soldier. A soldier makes noise. A spy blends in.

**Techniques APTs use:**

- **Spear Phishing:** Targeted phishing. Instead of sending generic emails to everyone, attackers research the target and send emails about things the specific person cares about — their workplace, their interests, events they follow. Much harder to spot because it looks relevant.
- **Zero-Day Exploits:** Vulnerabilities that the software vendor does not yet know about. No patch exists yet. By the time a fix is available, the damage may already be done.
- **Watering Hole Attacks:** A popular website that the target is known to visit is compromised. Anyone who visits that site can have malicious code injected into their device.
- **Supply Chain Attacks:** Rather than attacking the target directly, the attacker compromises a supplier or third-party vendor that the target uses. The attack then spreads through the supply chain.

**How to reduce APT risk:**  
Network segmentation is the key defensive principle here. If the network is divided into segments, a threat that enters through one section cannot freely move to others. This limits lateral movement — the attacker's ability to spread through the network once they are inside.

---

## Class 3 — OWASP ZAP & Final Revision

### OWASP ZAP

**ZAP** stands for **Z Attack Proxy**. It is an open-source tool developed by OWASP (Open Web Application Security Project) used to find vulnerabilities in web applications.

**How it works — Spidering and Crawling**  
ZAP uses a method called spidering to scan web applications. Spidering works by crawling — moving across a web application the way a spider moves across a wall, quickly and systematically, probing every link, every input, every endpoint it finds. As it crawls, it maps the application and identifies potential weaknesses.

**Important: You must have permission before running any scan.** Running ZAP against a website you do not have permission to test is illegal, regardless of your intentions. Organisations that hire ethical hackers use signed contracts that explicitly grant permission. A verbal agreement is not enough — without written permission, the organisation can deny they ever agreed to it.

**Practice targets (safe to scan without permission):**
- `testphp.vulnweb.com`
- `testfire.net`
- VulnHub / DVWA environments

**Reading ZAP results:**

ZAP categorises its findings by risk level:
- **High** — Serious vulnerabilities like Cross-Site Scripting (XSS)
- **Medium** — Moderate-risk issues that need addressing
- **Low** — Minor issues worth reviewing
- **Informational** — Not necessarily malicious, but worth knowing about

For each alert, ZAP provides:
- The type of vulnerability
- The CWE (Common Weakness Enumeration) reference
- A description of the issue
- A recommended solution

After scanning, you can generate a full report from the Reports tab. Scan multiple times — one scan is not always sufficient to catch everything.

**Other vulnerability testing tools:**  
- **Nessus** — another widely used vulnerability scanner
- **Metasploit** — used for exploitation (beyond scanning)
- **Nmap** — used for port scanning and network discovery

---

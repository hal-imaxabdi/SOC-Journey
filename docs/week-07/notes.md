# Week 7 Notes — Threat Intelligence and Threat Hunting

Three classes this week. The first was Tutor Jeremiah covering threat intelligence, OSINT,
and a live demo of Grabify (IP tracking via links). The second was a deep dive into threat
hunting with real log analysis scenarios. The third was a revision class that covered false
positives vs false negatives and tied everything together.

This was one of the most practical weeks in the entire bootcamp.

---

## Part 1 — Threat Intelligence

### What Is Threat Intelligence?

A threat is anything that can cause harm to an asset or resource.

**Threat intelligence** is when organizations do not wait for attacks to happen before they
react. Instead, they go out of their way to research the threats they are likely to face,
understand how those threats work, and prepare defenses in advance.

It is proactive research about potential attackers — their motives, their methods, and the
techniques they would use — so that organizations can either prevent attacks or respond
effectively when they happen.

In short: knowing your enemy before the enemy shows up.

---

### The Threat Intelligence Lifecycle

Threat intelligence follows a structured process with 6 stages.

**1. Planning and Direction**
Define the scope. What exactly are you trying to protect against? Phishing? Ransomware?
Insider threats? Without a clear scope, you end up collecting endless amounts of irrelevant
data. This stage sets specific goals for what intelligence you need to gather.

**2. Collection**
Gather raw data from both internal and external sources. Internal sources include your IDS,
SIEM, vulnerability assessments, and network logs. External sources include open internet
resources, forums, the dark web, and human intelligence. At this stage you collect
everything — even the noise.

**3. Processing**
Raw data is not yet useful. This stage organizes and filters the data so you are left with
what is actually relevant. Think of it like using Wireshark — you capture hundreds of
packets, but then you filter for only the ones that matter to your investigation.

**4. Analysis**
Now you work out the patterns. This is where TTPs come in.

**TTPs** stands for **Tactics, Techniques, and Procedures** — the methods attackers use
when launching attacks. Understanding TTPs means you understand *how* an attacker thinks
and operates, not just what they have done before. This stage also looks for **IOCs**
(Indicators of Compromise) — signs that a system or network has already been attacked.

**5. Dissemination**
The intelligence is communicated to the people who need it — team leaders, CISOs, incident
responders, and SOC analysts. Reports are produced with findings and recommendations, not
just raw data.

**6. Feedback**
The process is reviewed and improved. What worked? What gaps were there? The feedback
stage makes the next cycle of threat intelligence better than the last.

---

### Where Does Threat Intelligence Sit in the Organization?

Threat intelligence and threat hunting both fall under the **blue team**, specifically the
SOC (Security Operations Center). The SOC is made up of different roles: SOC Tier 1, 2,
and 3 analysts, threat intelligence analysts, threat hunters, forensic experts, and incident
responders. They all work together.

---

### Sources of Threat Intelligence

**Internal sources** — data generated within your own organization:
logs, IDS alerts, SIEM data, vulnerability scan results.

**OSINT (Open Source Intelligence)** — information gathered from publicly available
sources. Social media, LinkedIn, Google, news articles, forums, Reddit, the dark web —
anything that is publicly accessible. OSINT is a form of **passive reconnaissance**,
meaning you are gathering information without directly interacting with the target.

**HUMINT (Human Intelligence)** — information gathered from people. Security researchers,
informants, interviews. This is intelligence gathered through human interaction.

---

### Reconnaissance: Active vs Passive

Before an attacker launches an attack, they gather information about their target. This
is called **reconnaissance** (or recon), and it is the first stage of hacking.

**Active reconnaissance** — you are directly engaging with the target. The target is
sending information back to you. Setting up a CNC (Command and Control) server inside
a network to receive data from inside is an example. The target knows something is
happening, even if they cannot identify it yet.

**Passive reconnaissance** — you are gathering information without any direct interaction
with the target. You are using publicly available resources. OSINT is passive recon.

**CNC / C2 (Command and Control)** — a server set up by an attacker inside a target's
network that communicates back to the attacker. It allows the attacker to receive data
and send commands without appearing to be actively attacking.

---

### OSINT in Practice — Google Dorking

**Google Dorking** is a technique that uses advanced Google search operators to find
specific types of information that a regular search would not surface.

Examples demonstrated in class:

**Finding specific people:**
```
"Benedict Okoedo"
```
Putting a name in quotation marks forces Google to return only results where that exact
name appears together, rather than general results with each word separately.

**Finding specific file types:**
```
ethical hacking filetype:pdf
```
This returns only PDF results that contain "ethical hacking". You can swap `pdf` for
`docx`, `pptx`, etc. depending on what you need.

This is useful both for finding resources and for understanding how much information
about yourself or your organization is publicly findable.

**What you should do:** Search your own name using Google Dorking and see what comes up.
Jeremiah found his own CV/resume online from a past job application. If sensitive
information is indexed, you can report it to Google for removal.

---

### OSINT Tooling — OSINT Dojo

**OSINT Dojo** (osintdojo.com) is a free directory of OSINT tools organized by category.
Categories include: email OSINT, phone number OSINT, IP address OSINT, LinkedIn OSINT,
image tracking, location data, dark web OSINT, and more.

Most tools are free or have free tiers. Useful for understanding what information is
traceable from different starting points.

---

### What Happens When You Click a Link — Grabify Demo

This was one of the most important demonstrations of the entire bootcamp.

**Grabify** is a tool that creates a tracking link. When someone clicks the link, it
redirects them to the real website, but before doing so, it captures data about the
person who clicked.

What was collected from class members who clicked the link in the demo:
- IP address
- Location (city/country, may be approximate)
- ISP (MTN, Airtel, 9mobile, etc.)
- Device type (Android, iOS, Windows)
- OS version (e.g. Android 10, iOS 18.1)
- Browser (Chrome, Safari, etc.)
- Whether a VPN was being used (VPN users showed locations like Finland or US)

The link redirected to TechCrush's website, so everyone arrived at the correct destination
and most people had no idea their information was captured.

**Why this matters:**
- Attackers send links like this in phishing emails
- Clicking the link alone gives them your IP, device, and location
- With that information, they can craft more targeted attacks against your specific system
- If they also know your OS version, they can find vulnerabilities specific to that version

**How to defend against IP grabbers:**
Use a browser extension like **Anti-Grabify** (though Jeremiah noted it may no longer
work as reliably — find an alternative if needed). A VPN also masks your real IP and
location, though your device information is still captured.

**Key lesson:** Do not click links from unknown senders. Even if the link takes you
somewhere legitimate, the redirect may have already captured your information.

---

## Part 2 — Threat Hunting

### What Is Threat Hunting?

Threat intelligence looks **outwards** — researching external threats before they arrive.

Threat hunting looks **inwards** — actively searching for threats that are already inside
your network, hiding from your automated defenses.

Threat hunters assume that breaches have already happened, or that attackers are already
present, and they go looking for evidence of that. They do not wait for alerts. They go
looking before the alert fires.

---

### Types of Threat Hunting

**Hypothesis-driven hunting** — you develop a theory about what kind of threat might be
present, and then hunt for evidence of it. For example: "We believe a compromised account
may have been used for lateral movement. Let's look for signs of that."

**IOC-based hunting** — you look for specific Indicators of Compromise. Suspicious IP
addresses from unexpected locations, known malicious file hashes, unusual registry
changes, abnormal network traffic patterns.

**Anomaly detection** — you look for behavior that is not normal. Things that deviate
from the baseline of what users and systems typically do.

---

### Behavioral Analytics — What Counts as Suspicious?

Normal behavior has a baseline. Threat hunting finds deviations from that baseline.

Examples of suspicious behavior:

**Unusual login location:** An employee logs in from Abuja at 8:55 AM. Seven minutes later,
the same account logs in from Germany. That is physically impossible. This is either
credential theft or a VPN being used — and both warrant investigation.

**Login outside working hours:** An account logs in at 2 AM and begins moving large files
to an external destination. That organization does not operate at 2 AM. This is anomalous.

**Repeated access to sensitive files:** An IT employee who should not have access to
payroll data is repeatedly accessing payroll files. This violates confidentiality (the C
in CIA triad).

**High CPU/memory usage with no obvious cause:** If your calculator is the only open
application but your CPU is running at 100%, something else is consuming system resources.
This could indicate malware running in the background, or a CNC server sending data.

**Unknown executable files:** A file named `techcrush.pdf.exe` looks like a PDF but is
actually a runnable program. The `.exe` extension means it will execute as a script when
opened, not open in a PDF viewer.

---

### Log Analysis — The Core Skill of Threat Hunting

Reading logs is how threat hunters find evidence of what happened. Jeremiah walked through
several real log scenarios in class.

**How to access logs:**

On Linux:
```
cat /var/log/auth.log          Login and authentication logs
```

On Windows: use **Event Viewer** to access system, security, and application logs.

---

### Log Scenario Walkthroughs

**Scenario 1 — Brute Force Attack**

```
08:55 AM — John logs in successfully from Abuja (IP: 197.x.x.x)
09:02 AM — Login attempt from Germany (different IP) — FAILED
09:02 AM — Login attempt from Germany — FAILED (16 seconds later)
09:02 AM — Login attempt from Germany — FAILED
09:04 AM — Login attempt from Germany — SUCCESS
09:04 AM — Password changed
```

What happened: An attacker gained John's credentials and tried logging in from Germany.
Multiple failed attempts in rapid succession is a **brute force attack** — trying passwords
until one works. The 7-minute gap between Abuja and Germany is physically impossible,
confirming this is a different person. The password change immediately after a successful
login is the attacker locking John out of his own account.

Response: Disable John's account, reset the password, isolate his machine from the
network to prevent **lateral movement** (attacker moving to other parts of the network).

---

**Scenario 2 — Post-Login Attacker Activity**

After gaining access, the attacker opened:
- Command Prompt — to run system commands
- PowerShell — for more powerful scripting
- Ran `whoami` — to confirm which user account they are in
- Ran user management commands — to escalate privileges or create new accounts
- Ran `ipconfig` — to map the network they are now inside

This is post-exploitation behavior. The attacker is now inside and mapping the environment.

---

**Scenario 3 — Data Exfiltration**

```
Multiple connection attempts to external IP 45.77.12.90 on port 443
15MB of data transferred to that IP
```

Port 443 is HTTPS. The attacker connected to an external server (likely their own CNC
server) and transferred 15MB of files outbound. This is **data exfiltration** —
stealing data from the organization.

---

**Scenario 4 — Persistence Mechanism**

```
Scheduled task created: "UpdaterCheck"
Trigger: Hourly
Action: [run malicious script]
```

The attacker created a scheduled task disguised with an innocent-sounding name to ensure
their script keeps running even if discovered and killed. Every hour, the script executes
again. This is called establishing **persistence** — making sure the attacker's foothold
survives attempts to remove them.

---

**Scenario 5 — PowerShell with Hidden Execution**

```
powershell.exe -NoProfile -WindowStyle Hidden [command]
```

`-NoProfile` means no profile is created for this session (leaves less trace).
`-WindowStyle Hidden` means the PowerShell window does not appear on screen. The script
runs invisibly. This is a classic indicator of malicious PowerShell use.

---

**Scenario 6 — Normal Activity (for comparison)**

```
08:58 AM — Kate logs in from Lagos — SUCCESS
12:15 PM — Kate logs in from Lagos — SUCCESS
06:22 PM — Kate logs in from Lagos — SUCCESS
```

Same user, same location, reasonable time intervals, all successful. This is normal
behavior. No anomaly, no investigation needed.

---

### Lateral Movement

When an attacker gains access to one part of a network, they often try to move to other
parts of the network to gain more access or find more valuable data. This is called
**lateral movement**.

Network segmentation (covered in Week 3) is one defense against this — it limits how
far an attacker can move even after they have breached one segment.

---

## Part 3 — False Positives and False Negatives

These came up across all three classes and are critical for SOC analysts.

**False Positive** — The system raises an alert saying something is wrong, but it is not.
Example: Your antivirus flags Wireshark as malicious because it does not recognize the
application, even though Wireshark is a legitimate security tool.

False positives waste analyst time, but they are manageable. You investigate, confirm it
is safe, and move on.

**False Negative** — The system does not raise an alert, but something *is* wrong. The
threat passes through undetected. Example: A zero-day malware bypasses your antivirus
because the antivirus only knows about previously documented malware, not this new variant.

False negatives are far more dangerous than false positives. The threat is real and it
got through. This is why human analysts and threat hunting exist — to catch what automated
systems miss.

**Why it is better to have a false positive than a false negative:**
A false positive wastes time but causes no harm. A false negative means a real threat is
already inside, undetected, doing damage. As a SOC analyst, you would rather investigate
ten false alarms than miss one real attack.

**True Negative** — The system says nothing is wrong, and that is correct. No threat, no
alert. This is the normal, expected state.

---

### The Phishing Scenario from Class 3

A staff member at a fictional bank (Petocall Bank) received this WhatsApp message:

*"The Central Bank of Nigeria has issued an urgent circular regarding updated KYC
requirements for all banking staff. Please review and acknowledge receipt before close of
business today. Failure to comply may result in account suspension. [Link]"*

She forwarded it to IT support without clicking. Two colleagues had already clicked.

**What made it suspicious:**
- Unsolicited urgent message with a deadline
- Threat of consequences (account suspension) to create panic
- Link with unusual domain
- From an unknown number

**What the link did:**
Using a tracking link (like Grabify), the attacker collected IP addresses, locations,
device types, and ISPs from everyone who clicked. This is reconnaissance — gathering
information to plan the next stage of an attack.

**What defenders should do:**
- Check the link's redirect history using tools like **wheregoes.com**
- Look up the domain in threat intelligence feeds
- Block the IP addresses that the link points to
- Alert all staff not to click
- Investigate the two employees who did click — check their devices and accounts

---

### MITRE ATT&CK

Mentioned briefly in Class 3. **MITRE ATT&CK** (attack.mitre.org/attack-navigator) is a
publicly available knowledge base of attacker tactics, techniques, and procedures.

It documents real-world attack methods organized by category — initial access, execution,
persistence, privilege escalation, lateral movement, exfiltration, and more. Threat hunters
and SOC analysts use it as a reference to understand what attackers might do at each stage
and what to look for in logs.

---

## Quick Reference

| Term | What it means |
|---|---|
| Threat Intelligence | Proactive research into threats an organization may face |
| OSINT | Open Source Intelligence — info from publicly available sources |
| HUMINT | Human Intelligence — info gathered from people |
| Reconnaissance | First stage of hacking — gathering information about a target |
| Active recon | Directly engaging with the target to gather info |
| Passive recon | Gathering info without interacting with target (OSINT) |
| CNC / C2 | Command and Control server — attacker's server inside a network |
| TTP | Tactics, Techniques, and Procedures — how attackers operate |
| IOC | Indicator of Compromise — sign that a breach has occurred |
| Threat Hunting | Actively searching for threats already inside your network |
| Lateral movement | Attacker moving from one part of a network to another |
| Persistence | Attacker ensuring their foothold survives removal attempts |
| Data exfiltration | Attacker stealing and sending data to an external server |
| False positive | System flags a threat that is not actually a threat |
| False negative | System misses a real threat — far more dangerous |
| True negative | No threat exists and no alert fires — normal state |
| Google Dorking | Advanced search operators to find specific types of information |
| Grabify | IP tracking tool — captures device info when a link is clicked |
| MITRE ATT&CK | Database of real-world attacker tactics and techniques |

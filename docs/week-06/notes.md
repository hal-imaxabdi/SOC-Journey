# Week 6 Notes — Incident Response, Digital Forensics

Two big topics this week across three classes. The first class covered the full incident
response lifecycle. The second class went deep into forensic techniques. The third class
was a revision before the quiz, with a good walkthrough of the Linux permission calculations.

---

## Part 1 — Incident Response and Incident Management

### What Is the Difference?

These two terms sound similar but they describe different things.

**Incident Response** is what you do *immediately* when something goes wrong. It is reactive —
something has to happen first before you respond. Think of it like applying first aid when
someone gets hurt. You are not planning, you are acting.

**Incident Management** is everything that surrounds the response — the preparation, the
policies, the team, the tools, the lessons learned. It is both proactive (before an incident)
and reactive (during and after). It is long-term and continuous.

A simple way to keep them apart:
- Incident response = the action taken right now
- Incident management = the system that makes that action possible and improves over time

---

### The Incident Response Lifecycle

This is the framework for how a cybersecurity team handles a security incident from start
to finish. There are 6 phases.

---

**Phase 1 — Preparation**

This phase happens *before* any incident occurs. It is everything you do to be ready.

- Develop an **Incident Response Plan** — a step-by-step document that tells the team
  exactly what to do when something happens. Like a fire drill procedure. The drill exists
  so that when a real fire happens, people do not freeze.
- Establish an **Incident Response Team** — handpick the specific people responsible.
  Not everyone responds. There is a designated team: security analysts, network analysts,
  managers. This prevents chaos where everyone is giving different instructions.
- Conduct **risk assessments** — identify what vulnerabilities exist and what the
  organization stands to lose if they are exploited.
- Implement detection and response tools — SIEM, IDS, IPS, log management systems.
- Train and run drills — the team needs to practice before the real thing happens.

The important thing about preparation is that nobody tells you when the drill will happen.
If they did, it defeats the purpose. Same with real security incidents — attackers do not
announce themselves.

---

**Phase 2 — Detection**

This phase is about confirming that a real incident has actually occurred.

Not every alert is a real attack. A false positive is when the system throws an alarm but
there is no actual threat — like an employee logging in from Dubai while on holiday. The
system does not know it is them, so it flags it as suspicious. Detection is the process of
verifying whether an alert is real or not.

Detection methods include:
- Monitoring logs and system activity
- Analyzing network traffic and behavior
- Receiving alerts from security tools (IDS, SIEM)
- Reports from users — sometimes the first person to notice is a regular employee

Detection tools: IDS, SIEM platforms, log management tools, network monitoring tools,
EDR (Endpoint Detection and Response).

---

**Phase 3 — Containment**

Once an incident is confirmed, the goal is to stop it from spreading.

Think of it like tying a tourniquet when someone is bitten by a snake — you are not
removing the venom yet, you are stopping it from reaching the heart. Containment is about
limiting the damage, not fixing it yet.

Containment strategies:
- **Network segmentation** — isolate the affected part of the network so the infection
  cannot move to clean systems. Like cutting a bridge between two areas.
- **System isolation** — physically disconnect the infected machine from the network.
- **Account lockouts** — if an account has been compromised, lock it immediately so the
  attacker cannot keep using it. Same principle as calling your bank to block a stolen
  ATM card.
- **Traffic filtering and firewall rules** — block suspicious traffic at the network level.
- **Creating forensic snapshots** — capture the state of the system before anything is
  cleaned up. This evidence is needed later for investigation.

---

**Phase 4 — Eradication**

Now that the spread is contained, the goal is to remove the root cause entirely.

This is not the same as containment. Containment stops the spread. Eradication removes
the problem itself.

Eradication steps:
- Remove malware using antivirus and endpoint security tools
- Conduct a full system scan to find anything hiding
- Delete suspicious or malicious files
- Eliminate compromised accounts
- Apply security patches to fix the vulnerability that was exploited
- Restore compromised data from backups

A key point from class: you cannot just keep eradicating the same threat over and over
without understanding why it keeps coming back. If the same breach keeps happening, the
root vulnerability has not been fixed. That is a serious problem, and organizations can
face legal fines for it.

Also, antivirus cannot detect zero-day attacks — attacks that have never been seen before.
There is no signature to match against. This is why behavioral monitoring and anomaly
detection matter.

---

**Phase 5 — Recovery**

The system is clean. Now restore everything to normal so operations can continue.

Recovery goals:
- Restore system functionality
- Recover lost or corrupted data
- Verify that the system is now secure before bringing it back online
- Minimize downtime

Downtime is expensive. Organizations calculate expected revenue per hour. Every hour a
system is down is money lost. This is why preparation and practiced response procedures
matter — a team that does not know what to do wastes time, and time costs money.

---

**Phase 6 — Post-Incident Activity (Lessons Learned)**

Some materials list only 5 phases. The 6th is sometimes treated separately, but it is
important enough to include.

After everything is back to normal, the team documents what happened:
- How did the attacker get in?
- What slowed down the response?
- What worked well?
- How do we improve for next time?

This is what makes incident management continuous. You do not just recover and move on.
You learn, update the plan, and train again. The goal is that each time an incident
happens, the response is faster and more effective than the last time.

---

## Part 2 — Digital Forensics

Forensics is what happens after the immediate incident response is over. The attacker has
been contained and eradicated. Now the question is: who did this, how did they do it, and
can we prove it in a court of law?

### What Is Digital Forensics?

Digital forensics is the scientific process of identifying, preserving, analyzing, and
presenting digital evidence in a legally acceptable manner.

The key word is **legally**. Evidence that is mishandled — tampered with, broken chain of
custody, improperly collected — may be inadmissible in court. The entire point of forensics
is to collect evidence that holds up legally.

It is applied to computers, networks, mobile devices, and other electronic systems.

---

### Core Principles

**Preservation** — evidence must not be altered, corrupted, or destroyed during collection
or analysis. You do not touch the original if you can help it. You work from copies.

**Identification** — determining what constitutes evidence. When investigators arrive at a
crime scene, nothing is touched. Everything is a potential piece of evidence until
confirmed otherwise. Digital forensics works the same way — computers, phones, USB drives,
cloud accounts — all of it is preserved until its relevance is determined.

**Analysis** — interpreting the recovered data. Finding a fingerprint means nothing until
you match it to a person. Finding a file means nothing until you understand what it
contains and how it connects to the incident.

**Documentation and Chain of Custody** — every person who handles a piece of evidence must
be recorded. From the moment evidence is seized to the moment it is presented in court,
there is a documented record of who had it, when, and what was done with it. If this chain
breaks, the evidence can be challenged. Think of it like a library — every time the book
changes hands, it is logged.

---

### Types of Digital Forensics

**Disk Forensics**

Analyzing storage devices: HDDs, SSDs, USB drives, SD cards, memory cards.

The key technique is creating a **forensic image** — a bit-for-bit exact copy of the
storage device. This is done using a hardware tool called a **Write Blocker**, which
prevents any changes to the original device while the copy is being made. Once the copy
is made, investigators work only on the copy. The original is preserved.

Important fact: deleted files are not truly gone. When you delete a file, the system
marks that space as available for reuse, but the data remains until something new
overwrites it. This is how forensic analysts recover deleted files. Even emptying the
recycle bin does not fully erase data.

Tools: **Autopsy** (free, used for disk analysis and file recovery).

The process also includes analyzing internet history, email archives, system logs, and
reconstructing a timeline of activity.

---

**Memory Forensics**

Analyzing the RAM — the volatile (temporary) memory that is active while the system is
running.

RAM is important because it holds active processes, running applications, network
connections, and sometimes even encryption keys and passwords that are temporarily
stored there.

The critical constraint: **RAM is cleared when the computer is powered off.** Once the
machine shuts down, that evidence is gone forever. This is why investigators need to
capture a **memory dump** while the system is still running.

A memory dump is a snapshot of everything in RAM at that moment. Investigators then
analyze it for:
- Suspicious or unauthorized running processes
- Malware that is operating in memory
- Active network connections
- Evidence of ransomware in the process of encrypting files

In movies, when investigators say "do not touch the computer, do not turn it off" — this
is why. They are protecting volatile evidence.

Tools: **FTK Imager**, **Volatility**.

---

**Log Analysis and Correlation**

Everything that happens on a system leaves a log. Login attempts, file access, network
connections, application events — all of it is recorded.

Log analysis means examining these logs to find evidence of abnormal activity. Log
correlation means linking events from multiple log sources to piece together how an
attack happened.

Example: if an attacker is brute-forcing a login, you will see dozens of failed login
attempts in quick succession, all from the same IP address. If one of those attempts
succeeds, that is now a suspicious successful login. The logs show the full story.

This is similar to reconstructing a crime scene in an investigation — you piece together
different sources of information to build a timeline.

Types of logs:
- System logs — OS-level events
- Application logs — events within specific software
- Network logs — traffic, connections, firewall activity

Tools: **Splunk** (SIEM platform used for log collection, searching, and correlation).

---

**Network Forensics**

Analyzing captured network traffic to find evidence of an intrusion or data exfiltration.

This is essentially what you did with Wireshark. If an incident occurred while traffic
was being captured, you can go back through the capture and find which IP addresses were
communicating, what protocols were used, and whether anything suspicious was transmitted.

---

**Other Types of Forensics**

- **Database forensics** — who accessed the database, what actions were taken, when.
  Important because database breaches carry legal consequences for organizations.
- **Mobile forensics** — analyzing phones and tablets.
- **Cloud forensics** — evidence stored in cloud environments.
- **Email forensics** — analyzing email headers, attachments, and metadata.
- **Wireless forensics** — analyzing Wi-Fi traffic and connection logs.

---

### Evidence Collection and Preservation — Step by Step

1. **Document the scene** — take photos of the physical and digital environment before
   touching anything. This records the state of the evidence.

2. **Prioritize volatile evidence** — capture RAM (memory dump) first, before the machine
   is powered off, because that evidence will disappear.

3. **Create forensic images** — make bit-for-bit copies of storage devices using write
   blockers. Work only from the copies.

4. **Maintain chain of custody** — document every person who handles the evidence from
   seizure to court presentation.

5. **Securely store and transport evidence** — protect copies with strong encryption
   to prevent tampering during transfer.

---

### Hashing in Forensics

Hashing is used to verify that evidence has not been tampered with.

When a forensic image is created, a hash is calculated for it (using MD5 or SHA-256).
If anyone modifies the image — even by changing a single byte — the hash will be
completely different. Investigators can compare the hash at any point to confirm the
evidence is still in its original state.

This is the same principle Jeremiah demonstrated in an earlier class when verifying a
Wireshark download — compare the published hash to the one you calculate yourself.

---

### A Note on Ethics and Legality

This came up strongly in class. Everything in forensics is done legally and with
authorization. Using tools like Nmap, Wireshark, or Autopsy on systems you do not own
or do not have explicit permission to analyze is illegal.

Black hat activity is traceable. Forensic investigators find people. The same skills
being taught in this course are the skills used to find attackers.

---

## Quick Reference

| Term | What it means |
|---|---|
| Incident Response | Immediate reactive action taken when an incident occurs |
| Incident Management | The full system: preparation, response, and improvement |
| False positive | An alert that turns out not to be a real threat |
| Containment | Stopping the spread of an incident |
| Eradication | Removing the root cause |
| Recovery | Restoring systems to normal operation |
| Forensic image | Bit-for-bit copy of a storage device |
| Write blocker | Hardware that prevents changes to original media during imaging |
| Memory dump | Snapshot of RAM taken while the system is running |
| Volatile evidence | Evidence that disappears when the machine powers off (RAM) |
| Chain of custody | Record of who handled evidence and when |
| Log correlation | Linking events from multiple log sources to identify attack patterns |
| Zero-day | An attack that has never been seen before — no signature exists for it |
| Disk forensics | Analysis of storage devices (HDD, SSD, USB) |
| Memory forensics | Analysis of RAM |
| Network forensics | Analysis of captured network traffic |

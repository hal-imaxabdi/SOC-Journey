# Week 01 Lab Writeup – SOC Roles, Workflows & the CIA Triad

**Date:** 08-03-2026
**Lab Environment:** TryHackMe / Local Notes
**Time Spent:** 2hrs

---

## Lab Objective

Practice identifying CIA Triad violations in real attack scenarios, and simulate a basic T1 analyst alert triage workflow.

---

## Part 1 – CIA Triad Scenario Analysis

For each scenario below, identify which CIA pillar(s) are violated and explain why.

### Scenario A: WannaCry Ransomware (2017)
**What happened:** WannaCry encrypted files on hundreds of thousands of systems globally. Users could not access their data unless a ransom was paid.

**CIA Violation(s):**
- [ ] Confidentiality
- [x] Integrity ← files were altered/encrypted
- [x] Availability ← systems became inaccessible

**My Analysis:**
> Intergrity was violated reason being was that the ransomware encrypted the files with original content, modifying it without the woner's permission. 
>Hospitals like the UK's NHS could not access patient records  causing surgeries to be cancelled. This affected the Availability in the CIA triad. Data became unreachable.
> What i noticed here is that not all  pillars are affected whenever there is an incident, rather it depends on the attack itself.

---

### Scenario B: Equifax Data Breach (2017)
**What happened:** Attackers exploited an Apache Struts vulnerability and exfiltrated PII of ~147 million people.

**CIA Violation(s):**
- [x] Confidentiality ← PII was accessed by unauthorized parties
- [ ] Integrity
- [ ] Availability

**My Analysis:**
> Confidentiality was not upheld in this incident.Attackers got in through an unpatched web application vulnerability (CVE-2017-5638 in Apache Struts) and quietly copied out names, Social Security numbers, birth dates, and credit card info for 147 million Americans. The data was not  changed, and Equifax's systems stayed online,  people could still use their services. The damage was that private information got into the hands of people who were never supposed to see it.
Integrity was not violated because the attackers were in "read only" mode that is they copied data but did not modify the database records.
Availability was not violated because Equifax's website and services kept running during the breach.
What shocked me: the breach went undetected for 78 days. That's a SOC monitoring failure, better alerting on unusual data egress volumes could have caught this much earlier.
Key takeaway: Data theft = Confidentiality only. If the attacker is a silent reader who changes nothing and breaks nothing, that's the only pillar violated.

---

### Scenario C: SolarWinds Supply Chain Attack (2020)
**What happened:** Malicious code was injected into SolarWinds Orion software updates, giving attackers backdoor access to thousands of organizations.

**CIA Violation(s):**
- [x] Confidentiality ← unauthorized access to networks
- [x] Integrity ← software supply chain was tampered with
- [ ] Availability

**My Analysis:**
> This is the most sophisticated of the three and it hits two pillars.
Integrity was violated at the source. Attackers (believed to be Russian SVR) compromised SolarWinds' own build pipeline and injected malicious code (the SUNBURST backdoor) into a legitimate software update. Every organization that downloaded the update received tampered software they believed was clean. The software lost its integrity, and there was no way for end users to know.
Confidentiality was violated for the thousands of organizations that installed the update; including US government agencies like Treasury and Homeland Security. Attackers had silent backdoor access for months, reading emails and internal documents they had zero authorization to see.
Availability was NOT violated. Systems kept running, people could still log in and work. The attack was specifically designed to be invisible.
Why this is scary: traditional antivirus would not catch this because the malicious code was digitally signed and delivered as a legitimate update from a trusted vendor. This is why supply chain security matters.
Key takeaway: Supply chain attacks = Integrity (tampered source) + Confidentiality (unauthorized access). The most dangerous attacks are the ones you don't notice for months.

---


## Part 2 – T1 Analyst Triage Simulation

**Simulated Alert:**
```
ALERT ID: ALT-0042
Timestamp: 2024-01-15 03:17:42 UTC
Source IP: 192.168.1.105 (internal workstation)
Destination IP: 185.220.101.45 (external, TOR exit node)
Port: 443 (HTTPS)
Rule triggered: OUTBOUND_TOR_CONNECTION
User: jsmith@company.com
```

### Step 1: Initial Assessment

**Questions I asked myself as a T1 analyst:**

- **Is 185.220.101.45 a known bad IP?**
  Yes, checked on VirusTotal and AbuseIPDB. Confirmed TOR exit node. TOR traffic is a red flag because it's used to anonymize connections and bypass corporate controls.

- **Is jsmith authorized to use TOR?**
  Checked the acceptable use policy, no employees are authorized to route traffic through TOR on company devices. This is an immediate policy violation regardless of intent.

- **Why does 3:17 AM matter?**
  jsmith is a mid-level marketing employee. There's no business reason for a marketer to be on their work laptop at 3 AM connecting to a TOR node. Off-hours activity from unexpected users is a classic indicator of a compromised account or insider threat.

- **Could this be a false positive?**
  Possibly, maybe jsmith left a VPN or privacy browser extension running. But because of the destination (TOR exit node) and the time (3 AM), I can't dismiss this. When in doubt, escalate- it's always better to send a false positive to T2 than to miss a real incident.

- **What else would I want to know?**
  - What other traffic did 192.168.1.105 generate in the last 24 hours?
  - Did jsmith's account have any failed logins recently?
  - Is 192.168.1.105 running any unusual processes? (T2 checks this via EDR)

---

### Step 2: Triage Decision

```
Severity:  HIGH
Action:    Escalate to T2
Reason:    Internal host connected to a known TOR exit node at 3:17 AM.
           User (jsmith) has no business justification for TOR usage.
           Possible C2 beacon or data exfiltration in progress.
           Cannot rule out compromised endpoint or insider threat.
```

---

### Step 3: Initial Documentation (ticket notes)

```
TICKET:   INC-0042
Created:  2024-01-15 03:22:00 UTC
Status:   ESCALATED → Tier 2
Analyst:  Hal Abdi
Priority: HIGH

SUMMARY:
User jsmith@company.com on workstation 192.168.1.105 made an outbound
HTTPS connection on port 443 to 185.220.101.45 (confirmed TOR exit node)
at 03:17 UTC. Activity is off-hours and unauthorized per company policy.

FINDINGS:
- Destination IP confirmed as TOR exit node via VirusTotal + AbuseIPDB
- 3 AM activity inconsistent with user role (Marketing dept)
- No TOR use exceptions on file for this user or device
- No prior alerts on this host in the past 30 days

POSSIBLE EXPLANATIONS (ranked by likelihood):
1. Compromised endpoint — malware beaconing out through TOR for C2
2. Compromised credentials — attacker operating remotely as jsmith
3. Insider threat — employee deliberately using TOR to exfiltrate data
4. False positive — privacy tool misconfigured (lowest probability)

RECOMMENDED ACTIONS FOR T2:
- Pull EDR telemetry for 192.168.1.105 (last 24 hrs)
- Check for other unusual outbound connections from this host
- Review jsmith's authentication logs for anomalies
- Consider network isolation of host pending investigation

NOTE: Did not contact user or manager — left that call to T2.
      Alert kept open — did not mark as false positive.
```

---

## Part 3 – TryHackMe Room Notes

**Room:** [SOC Level 1 Path on TryHackMe](https://tryhackme.com/path/outline/soclevel1)

- [x] **Junior Security Analyst Intro**
- [x] **Pyramid Of Pain**
- [ ] **Cyber Kill Chain** ← starting next session

**Junior Security Analyst Intro – My Notes:**
> The room broke down the SOC analyst day-to-day really clearly. The thing that hit me was how much of the job is documentation — it's not just "find the threat," it's writing clear, factual notes so the T2 analyst can pick up exactly where you left off without re-investigating everything. Also learned what SIEM actually does: it ingests logs from everywhere (firewalls, endpoints, cloud) and correlates them into alerts. Without a SIEM you'd be reading thousands of raw logs manually — impossible at scale.

**Pyramid of Pain – My Notes:**
> This framework by David Bianco ranks IOCs by how much pain it causes the attacker when you block them. Key insight: blocking a file hash is trivially easy for an attacker to bypass — they just recompile the malware and the hash changes. But if you detect their TTPs (Tactics, Techniques, Procedures — the actual *way* they operate), that's extremely hard to change because it's their core methodology. This changes how I think about detection rules — behavioral detections are much more valuable than hash-based signatures.

---

## What I Learned This Week

> **One thing that surprised me:**
> I assumed every cyberattack violates all three CIA pillars. Turns out most attacks are very targeted — ransomware ignores Confidentiality, data breaches ignore Availability. Understanding which pillar is targeted tells you a lot about what the attacker actually wanted.
>
> **One concept I want to dig deeper into:**
> The SolarWinds scenario made me want to learn more about supply chain security and how build pipeline integrity works. Going to look into the SLSA framework and how code signing verification is supposed to prevent this.
>
> **One thing I'd do differently:**
> On the triage exercise I almost talked myself into marking the TOR alert as a false positive. I need to internalize this rule: in a SOC, you escalate when uncertain. It is always better to send a false positive to T2 than to miss a real incident and have it become a breach.

---

## Resources Used

- [x] TryHackMe SOC Level 1 Path — 2 rooms completed
- [x] MITRE ATT&CK Matrix: https://attack.mitre.org — looked up T1048 (Exfiltration Over Alternative Protocol)
- [x] AbuseIPDB: https://www.abuseipdb.com — used to verify TOR exit node IP in triage exercise
- [x] VirusTotal: https://www.virustotal.com — IP reputation lookup
- [x] IBM: What is a SOC? https://www.ibm.com/topics/security-operations-center
- [ ] Professor Messer Security+ videos (CIA Triad) ← still to do

---

*Lab writeup for SOC-Fundamentals GitHub repo | Week 01 | Author: Hal Abdi*

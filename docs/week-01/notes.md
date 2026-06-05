# Week 01 – SOC Roles, Workflows & the CIA Triad

## 1. What Is a SOC?

A **Security Operations Center (SOC)** is a centralized team (and facility) that monitors, detects, investigates, and responds to cybersecurity threats in real time — 24/7/365.

Think of it as the nerve center of an organization's security posture.

---

## 2. The CIA Triad

The CIA Triad is the foundational model of information security. Every security control, policy, and incident maps back to one (or more) of these three pillars:

| Pillar | Definition | Example Attack | Example Control |
|---|---|---|---|
| **Confidentiality** | Only authorized users can access data | Data exfiltration, eavesdropping | Encryption, access controls, MFA |
| **Integrity** | Data is accurate and unaltered | Man-in-the-Middle, SQL injection | Hashing (SHA-256), digital signatures |
| **Availability** | Systems are accessible when needed | DDoS attack, ransomware | Redundancy, backups, failover systems |

### Real-World Mapping

- A **ransomware attack** primarily targets **Availability** (locks you out of your files).
- A **data breach** primarily targets **Confidentiality** (exposes private data).
- A **SQL injection** that modifies records targets **Integrity**.

---

## 3. SOC Tier Model (T1 → T3 + Management)

### Tier 1 – Alert Analyst
- **Role:** First responder. Monitors the SIEM dashboard, triages incoming alerts.
- **Daily tasks:** Review alerts, classify (true/false positive), escalate.
- **Tools:** SIEM (Splunk, IBM QRadar), ticketing systems.
- **Key skill:** Pattern recognition, not panicking during high-alert volume.

### Tier 2 – Incident Responder
- **Role:** Deep-dives into escalated tickets from T1.
- **Daily tasks:** Threat hunting, correlation, containment decisions.
- **Tools:** EDR (CrowdStrike, SentinelOne), packet analysis (Wireshark).
- **Key skill:** Forensics, log analysis, malware triage.

### Tier 3 – Threat Hunter / SME
- **Role:** Proactively hunts for threats not caught by automated tools.
- **Daily tasks:** Threat intelligence, custom detection rule writing, red team collaboration.
- **Tools:** MITRE ATT&CK, YARA rules, threat intel platforms (MISP).
- **Key skill:** Deep adversarial thinking, scripting (Python/KQL).

### SOC Manager
- **Role:** Oversees operations, reporting, hiring, metrics.
- **Daily tasks:** KPI reporting, SLA management, vendor relationships.

---

## 4. SOC Workflow – Alert Lifecycle

```
[Log Source] → [SIEM Ingestion] → [Alert Fired] → [T1 Triage]
                                                        |
                                              [False Positive?]
                                              /              \
                                           Close           Escalate to T2
                                                               |
                                                    [Confirmed Incident?]
                                                    /                   \
                                               Contain              Escalate to T3
                                                  |
                                            [Remediate]
                                                  |
                                            [Post-Incident Report]
```

### Key Terms
- **SIEM** – Security Information and Event Management (log aggregation + alerting)
- **IOC** – Indicator of Compromise (IP, hash, domain linked to threat)
- **TTPs** – Tactics, Techniques, and Procedures (how attackers operate)
- **SLA** – Service Level Agreement (e.g., T1 must triage within 15 minutes)
 
---

## 5. Key Frameworks to Know

| Framework | Purpose |
|---|---|
| **MITRE ATT&CK** | Catalog of adversary TTPs mapped to real-world groups |
| **NIST CSF** | Identify → Protect → Detect → Respond → Recover |
| **Kill Chain** | 7 stages of a cyberattack (Recon → Exploitation → Exfiltration) |

---

## 6. Study Checklist – Week 01

- [ ] Define the CIA Triad without notes
- [ ] Explain what each SOC tier does (elevator pitch style)
- [ ] Map 3 real attacks to CIA Triad pillars
- [ ] Draw the alert lifecycle from memory
- [ ] Research 1 real SOC incident from the past year (e.g., breach report)
- [ ] Watch: [John Strand – Free SOC Core Skills](https://www.youtube.com/watch?v=6X6S2U3E8gQ) (~1hr)
- [ ] Create your CIA Triad diagram in `/diagrams/`

---

## 7. Reflection Questions (add your answers!)

1. If a hospital's patient records are encrypted by ransomware, which CIA pillar(s) are violated?
2. What's the difference between a T1 and T2 analyst in terms of decision-making authority?
3. Why does a SOC need a Tier 3 if automated tools already catch most threats?

---

*Sources: NIST SP 800-61, MITRE ATT&CK, SANS SOC Survey 2023*

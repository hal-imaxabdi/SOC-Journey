# Week 01 Diagrams
 
## Diagram 1 – The CIA Triad

```
              ┌─────────────────────┐
              │                     │
              │   CONFIDENTIALITY   │
              │  Only authorized    │
              │   users see data    │
              │                     │
              └──────────┬──────────┘
                         │
              ┌──────────▼──────────┐
              │                     │
              │    CIA TRIAD        │
              │   (Core of all      │
              │  InfoSec decisions) │
              │                     │
              └──────────┬──────────┘
             /            \
┌───────────▼────────┐  ┌──▼──────────────────┐
│                    │  │                      │
│    INTEGRITY       │  │    AVAILABILITY      │
│  Data is accurate  │  │  Systems accessible  │
│  and unaltered     │  │  when needed         │
│                    │  │                      │
└────────────────────┘  └──────────────────────┘
```

## Diagram 2 – SOC Tier Structure

```
┌─────────────────────────────────────────────────────┐
│                    SOC MANAGER                       │
│          (Strategy, Reporting, Compliance)           │
└──────────────────────┬──────────────────────────────┘
                       │
       ┌───────────────┼───────────────┐
       ▼               ▼               ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│   TIER 1     │ │   TIER 2     │ │   TIER 3     │
│ Alert Analyst│ │  Incident    │ │   Threat     │
│              │ │  Responder   │ │   Hunter     │
│ • Monitor    │ │ • Investigate│ │ • Proactive  │
│ • Triage     │ │ • Contain    │ │   hunting    │
│ • Escalate   │ │ • Deep dive  │ │ • Rule dev   │
│              │ │              │ │ • Intel      │
│ Entry level  │ │ Mid level    │ │ Senior level │
└──────────────┘ └──────────────┘ └──────────────┘
```

## Diagram 3 – Alert Lifecycle (SOC Workflow)

```
  [Log Sources]                    [Outcome]
  ───────────                      ─────────
  Firewall  ─┐
  Endpoint  ─┤──► [SIEM] ──► [ALERT FIRES]
  Server    ─┘                     │
                                   ▼
                            [T1: TRIAGE]
                           /             \
                    [False Positive]   [Suspicious]
                          │                 │
                       [Close]        [Escalate T2]
                                           │
                                  [Confirmed Incident?]
                                  /                   \
                            [Contain]            [Escalate T3]
                                 │
                           [Remediate]
                                 │
                      [Post-Incident Report]
                                 │
                       [Lessons Learned]
```

## Diagram 4 – CIA Triad Attack Mapping

| Attack Type | C | I | A |
|---|:---:|:---:|:---:|
| Ransomware | | ✓ | ✓ |
| Data Breach / Exfiltration | ✓ | | |
| DDoS Attack | | | ✓ |
| SQL Injection (data theft) | ✓ | | |
| SQL Injection (data modify) | | ✓ | |
| MitM / Eavesdropping | ✓ | | |
| Supply Chain Attack | ✓ | ✓ | |

*C = Confidentiality, I = Integrity, A = Availability*

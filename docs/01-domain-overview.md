# Domain Overview — AML, KYC & Financial Crime

## 1. Executive Summary

Regulated banks must **know their customers** (KYC) and **monitor for suspicious activity** (AML). This portfolio piece covers a digital-first retail bank ("Helix Financial") launching EU-wide current accounts with integrated identity verification, sanctions screening, and transaction monitoring.

## 2. Regulatory Context

| Framework | Relevance |
|-----------|-----------|
| **EU AMLD4 / AMLD5** | CDD, beneficial ownership, PEP screening (Regulation (EU) 2024/1624 applies from July 2027) |
| **EU AMLD6** | Harmonized money laundering offences and penalties |
| **FATF 40 Recommendations** | Risk-based approach, STR/SAR filing |
| **GDPR** | Lawful basis for identity data processing |
| **eIDAS** | Qualified electronic signatures where applicable |

## 3. KYC Maturity Levels

```mermaid
flowchart LR
    A[Simplified DD] -->|Low risk retail| B[Standard CDD]
    B -->|Triggers| C[Enhanced DD]
    C --> D[Ongoing Monitoring]
    D --> E[Periodic Review]
```

### CDD Triggers for EDD

- PEP or RCA (relative/close associate)
- High-risk country residence or nationality
- Unusual onboarding patterns (velocity, device fingerprint)
- Adverse media hit above confidence threshold

## 4. AML Monitoring Scope

| Monitoring Type | Examples |
|-----------------|----------|
| **Real-time** | Sanctions list match, blocked country, structuring pattern |
| **Near real-time** | Velocity limits, mule account indicators |
| **Batch** | Peer group anomaly, dormant account reactivation |

## 5. Key Systems

| System | Function |
|--------|----------|
| **Onboarding Orchestrator** | Customer journey, document capture |
| **ID&V Provider** | Document + biometric verification |
| **Watchlist Service** | Sanctions, PEP, adverse media |
| **TM Engine** | Rule and scenario-based alerts |
| **Case Management** | Investigator workflow, SAR drafting |

## 6. Success Metrics

- Onboarding completion rate ≥ 78% (excluding intentional abandonment)
- Median KYC decision time < 3 minutes for low-risk retail
- Alert false-positive rate reduced 25% YoY without missing SAR SLA
- 100% of SAR filings within regulatory deadline (typically 24–72h from internal escalation)

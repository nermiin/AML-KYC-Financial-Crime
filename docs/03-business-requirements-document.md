# Business Requirements Document — Digital KYC & Sanctions Screening

| Field | Value |
|-------|-------|
| **Project** | Helix KYC Modernization |
| **Version** | 1.0 |
| **Status** | Approved |

---

## 1. Objectives

| ID | Objective |
|----|-----------|
| BO-01 | Reduce onboarding time for low-risk customers to < 3 minutes |
| BO-02 | Real-time sanctions/PEP screening at onboarding and daily |
| BO-03 | Structured audit trail for all KYC and alert decisions |

## 2. Customer Risk Rating Rules

### BR-KYC-01 — Identity Verification

| Check | Pass Criteria | Fail Action |
|-------|---------------|-------------|
| Document authenticity | Provider score ≥ 0.85 | Reject — `ID_DOC_FAIL` |
| Face match | Liveness pass + match ≥ 0.90 | Reject — `BIOMETRIC_FAIL` |
| Data consistency | Name/DOB match across doc and application | Refer — manual review |

### BR-KYC-02 — Sanctions & PEP

| Hit Type | Match Score | Action |
|----------|-------------|--------|
| Sanctions (OFAC/EU/UN) | ≥ 0.92 | **Block** — no account opening |
| PEP | ≥ 0.88 | **EDD required** before activation |
| Adverse media | ≥ 0.80 | **Refer** — compliance review |

### BR-KYC-03 — Geographic Risk

| Residence Country Risk Tier | Action |
|----------------------------|--------|
| Tier 1 (low) | Standard CDD |
| Tier 2 (medium) | Additional proof of address |
| Tier 3 (high) | EDD + senior compliance approval |

## 3. Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01 | Screen customer against sanctions/PEP at submit and daily batch | Must |
| FR-02 | Store verification artifacts with 5-year retention | Must |
| FR-03 | Assign risk rating (LOW/MEDIUM/HIGH) at onboarding | Must |
| FR-04 | Route HIGH risk to EDD workflow before account activation | Must |
| FR-05 | Provide investigator UI with alert context (90-day tx history) | Must |
| FR-06 | Mandatory disposition code on alert closure | Must |
| FR-07 | SAR case template with pre-filled customer data | Should |
| FR-08 | Onboarding status webhooks to CRM | Should |

## 4. Non-Functional Requirements

| ID | Requirement |
|----|-------------|
| NFR-01 | Screening API p95 < 800ms |
| NFR-02 | 99.95% availability for onboarding path |
| NFR-03 | RBAC — investigators cannot approve own EDD cases |
| NFR-04 | All PII encrypted at rest (AES-256) |

## 5. Integration Points

- ID&V vendor (REST, webhook callbacks)
- Watchlist provider (real-time + batch delta files)
- Core banking (account creation only after KYC PASS)
- Transaction monitoring (customer risk rating feed)

## 6. Reporting Requirements

| Report | Audience | Frequency |
|--------|----------|-----------|
| Onboarding funnel | Product | Daily |
| EDD queue aging | Compliance | Daily |
| SAR pipeline | MLRO | Weekly |
| False positive rate by rule | TM team | Monthly |

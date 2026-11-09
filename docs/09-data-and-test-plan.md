# Data Requirements and Test Plan: AML & KYC

## 1. KYC Profile Record

| Field | Type | Mandatory | Rule |
|-------|------|-----------|------|
| `customerId` | UUID | Yes | Core banking account created only after KYC PASS |
| `fullName`, `dateOfBirth`, `nationality` | Text, date, ISO 3166 | Yes | From verified document |
| `residenceCountry` | ISO 3166 | Yes | Drives geographic tier |
| `documentType`, `documentExpiry` | Enum, date | Yes | Expiry triggers refresh |
| `documentScore`, `faceMatchScore`, `livenessResult` | Number, number, Enum | Yes | BR-KYC-01 |
| `riskScore`, `riskRating` | Integer, Enum | Yes | BR-KYC-05 |
| `pepStatus` | Enum | Yes | NONE, TIER_1, TIER_2, FORMER |
| `eddStatus` | Enum | If needed | PENDING, APPROVED, REFUSED |
| `nextReviewDate` | Date | Yes | Derived from rating |
| `relationshipEndDate` | Date | On exit | Starts the retention period |

## 2. Screening Result Record

| Field | Rule |
|-------|------|
| `screeningId` | Unique per event |
| `listName`, `listVersion` | Proves what was screened against, and when |
| `matchedEntryId`, `matchScore` | Stored for each candidate |
| `decision` | CLEARED, CONFIRMED, PENDING |
| `decisionRationale`, `decidedBy` | Mandatory for manual decisions |

## 3. Alert Disposition Codes

| Code | Use | Visible outside financial crime group? |
|------|-----|----------------------------------------|
| `FALSE_POSITIVE` | Activity explained | No |
| `CUSTOMER_CONTACTED` | Routine outreach, approved first if a case is open | No |
| `SAR_FILED` | Filed with FIU | **Never** |
| `REFERRED_LAW_ENFORCEMENT` | External referral | **Never** |

## 4. Role Access

| Capability | Agent | L1 | L2 | MLRO | Audit |
|------------|-------|----|----|------|-------|
| View KYC status | Yes | Yes | Yes | Yes | Yes |
| View alerts | No | Yes | Yes | Yes | Read |
| Close alert | No | Yes | Yes | Yes | No |
| Escalate to MLRO | No | No | Yes | n/a | No |
| View SAR pipeline | No | No | Own cases | Yes | With approval |
| File SAR | No | No | No | Yes | No |
| Change thresholds | No | No | No | Approves | No |

## 5. SAR Narrative Minimum Content

Addresses PP-K05, inconsistent narrative quality.

| Section | Content |
|---------|---------|
| Who | Subject details, linked parties |
| What | Transactions, instruments, amounts, totals |
| When | Period, dates, pattern |
| Where | Branches, jurisdictions, counterparties |
| Why suspicious | Comparison against profile, indicators |
| How identified | Rule or referral, investigation steps |
| Action taken | Account status, any restriction |

## 6. Threshold Governance

| Step | Owner | Evidence |
|------|-------|----------|
| Propose change with data | TM analyst | Above and below the line sample |
| Replay against known SAR cases | Model owner | No missed cases |
| Approve | Compliance design authority | Minuted decision |
| Deploy with effective date | Engineering | Change record |
| Review after 90 days | Head of Financial Crime | Alert volume and quality |

## 7. UAT Plan

| TC | Covers | Input | Expected |
|----|--------|-------|----------|
| TC-01 | BR-KYC-01 | Document score 0.84 | Reject, `ID_DOC_FAIL` |
| TC-02 | BR-KYC-01 | Liveness failed | Reject, `BIOMETRIC_FAIL` |
| TC-03 | BR-KYC-02 | Sanctions 0.92 | Blocked, no core account |
| TC-04 | BR-KYC-02 | Sanctions 0.91 | Level 1 review |
| TC-05 | BR-KYC-02 | Sanctions 0.74 | Cleared, logged |
| TC-06 | BR-KYC-02 | PEP 0.88 | EDD before activation |
| TC-07 | BR-KYC-03 | Tier 3 residence | HIGH, senior approval |
| TC-08 | BR-KYC-04 | Rescreen confirms existing customer | Accounts frozen |
| TC-09 | BR-KYC-05 | Score 25, then 26 | LOW, then MEDIUM |
| TC-10 | BR-KYC-06 | Left office 8 months ago | Treated as PEP |
| TC-11 | BR-AML-01 | Agent views customer under SAR | No indicator displayed |
| TC-12 | BR-AML-01 | CRM webhook during SAR | No SAR or alert fields |
| TC-13 | FR-06 | Close alert with no code | Blocked |
| TC-14 | FR-09 | Deadline at T-48h and T-24h | Two MLRO alerts |
| TC-15 | NFR-03 | Self-approval of EDD | Blocked |
| TC-16 | NFR-01 | Screening under load | p95 below 800ms |

**Exit criteria:** Compliance sign-off on thresholds, DPO acknowledgment of retention, synthetic identities including test sanctions names all behave as expected, and an MLRO walkthrough confirms the information barrier.

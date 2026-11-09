# Worked Scenarios: AML & KYC

Fictional customers. Rules refer to the [To-Be Process Design](07-to-be-process-design.md).

## S-01 Low-Risk Straight-Through Onboarding

| Check | Result |
|-------|--------|
| Document score | 0.91, passes at 0.85 |
| Liveness and face match | Passed, 0.94 |
| Sanctions, PEP, adverse media | No hits |

| Risk factor | Value | Points |
|-------------|-------|--------|
| Geography | Germany, Tier 1 | 0 |
| Channel | Remote | 10 |
| Expected turnover | EUR 3,500 a month | 0 |
| Occupation | Salaried teacher | 0 |
| **Total** | | **10, LOW** |

**Result:** ACTIVE within the 3-minute target. Next periodic review in 5 years.

## S-02 Fuzzy Sanctions Match Cleared

| Step | Detail |
|------|--------|
| Screening | Applicant "Jon A. Example" against a listed "John Example", score 0.84 |
| Rule | In the 0.75 to 0.91 band, so Level 1 review before activation |
| Comparison | Date of birth differs by 19 years, nationality differs |
| Decision | False match, recorded with rationale and list version |
| Follow-up | Pair stored so the same customer and list entry do not alert again, unless either record changes |

Under the original BRD, this 0.84 score would have passed without anyone looking at it (F-02).

## S-03 PEP Onboarding

| Step | Detail |
|------|--------|
| Screening | PEP match 0.93, former deputy minister who left office 8 months ago |
| Status | Within the minimum 12-month period, tier 1 |
| Rating | Override to HIGH |
| EDD | Source of funds, source of wealth, purpose of account |
| Approval | Senior compliance sign-off, investigator cannot approve own case (NFR-03) |
| Result | Active with annual review and enhanced monitoring |

## S-04 Existing Customer Hit on Daily Rescreen

| Time | Event |
|------|-------|
| 06:10 | Delta file adds a new EU sanctions designation |
| 06:12 | Customer match, score 0.97 |
| 06:15 | Level 1 reviewer alerted, outbound payments held |
| 08:40 | True match confirmed |
| 08:45 | Accounts frozen, report started to competent authority |
| Customer view | Neutral message that the account is restricted, with a contact route |

## S-05 Structuring Alert Through to SAR

| Date | Cash deposit (EUR) | Branch |
|------|--------------------|--------|
| 2 June | 9,200 | A |
| 4 June | 9,400 | B |
| 6 June | 9,100 | C |
| 8 June | 9,600 | A |
| **Total** | **37,300** | |

Each deposit is below a fictional internal EUR 10,000 review threshold, while the 7-day total is well above it.

| Stage | Action | Disposition |
|-------|--------|-------------|
| TM rule | 4 deposits under threshold in 7 days, 3 branches | Alert raised |
| Level 1 | Customer profile: part-time salary of EUR 1,800 | Not explained |
| Level 2 | Funds leave by wire to an unrelated third party within 24 hours | Suspicion formed |
| MLRO | Reviews evidence, decides to report | `SAR_FILED` |
| Customer view | Nothing changes, no contact about the alert (BR-AML-01) | |

The question of source of funds SHALL NOT be asked in a way that reveals the investigation. Any outreach is approved by the MLRO first.

## S-06 Capacity and Tuning Case

| Driver | Value |
|--------|-------|
| Alerts a month | 6,000 |
| False-positive share | 94%, or 5,640 alerts |
| 25% reduction in false positives | 1,410 fewer alerts |
| Average handling time | 30 minutes |
| **Hours released a month** | **705** |

Any tuning change must also run against known SAR cases. A rule change that would have missed a filed SAR is rejected, whatever it saves.

## Edge Case Register

| ID | Situation | Expected Behavior | Rule |
|----|-----------|-------------------|------|
| E-01 | Screening provider unavailable | Onboarding held, never activated by default | FR-01 |
| E-02 | Sanctions score exactly 0.92 | Block | BR-KYC-02 |
| E-03 | Sanctions score exactly 0.75 | Level 1 review | BR-KYC-02 |
| E-04 | Customer moves to a Tier 3 country | Re-rate to HIGH, EDD triggered | BR-KYC-03 |
| E-05 | Customer exercises GDPR access during a SAR | Routed to DPO and MLRO, SAR data withheld where the law allows | BR-AML-01 |
| E-06 | Investigator owns both case and EDD approval | Approval blocked | NFR-03 |
| E-07 | Relationship ended 5 years ago, no legal hold | Records deleted | FR-02 |

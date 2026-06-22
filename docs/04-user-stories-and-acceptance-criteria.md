# User Stories & Acceptance Criteria — AML/KYC

### US-KYC-01 — Straight-through onboarding

**As a** retail customer  
**I want** to open an account in minutes with my passport and selfie  
**So that** I can use banking services without visiting a branch

```gherkin
  Scenario: Low-risk German resident passes KYC
    Given applicant is 25 years old residing in Germany
    And ID&V returns document score 0.91 and face match 0.94
    And sanctions and PEP screening returns no hits
    When onboarding is submitted
    Then account status becomes ACTIVE within 3 minutes
    And risk rating is LOW
```

---

### US-KYC-02 — Sanctions block

**As a** compliance officer  
**I want** confirmed sanctions matches to block onboarding immediately  
**So that** the bank does not onboard prohibited parties

```gherkin
  Scenario: Sanctions hit blocks account
    Given sanctions screening returns match score 0.95
    When onboarding is evaluated
    Then onboarding status is REJECTED
    And reason code is SANCTIONS_BLOCK
    And no core banking account is created
```

---

### US-KYC-03 — PEP enhanced due diligence

**As a** compliance analyst  
**I want** PEP matches routed to EDD before activation  
**So that** we collect source of funds and senior approval

- EDD checklist must be completed before status → ACTIVE.
- Senior compliance sign-off required for PEP tier 1 and 2.

---

### US-AML-01 — Alert disposition

**As a** financial crime analyst  
**I want** to close alerts with structured reason codes  
**So that** audit and model tuning have consistent data

| Disposition | When to Use |
|-------------|-------------|
| `FALSE_POSITIVE` | Legitimate customer activity explained |
| `CUSTOMER_CONTACTED` | Outreach completed, docs obtained |
| `SAR_FILED` | Escalated to MLRO and filed |
| `REFERRED_LAW_ENFORCEMENT` | External referral made |

---

### US-AML-02 — SAR deadline tracking

**As an** MLRO  
**I want** a dashboard of open SAR cases with regulatory deadlines  
**So that** filings are never late

- Dashboard shows days remaining per jurisdiction rule.
- Email alert at T-48h and T-24h before deadline.

## Definition of Done

- Compliance sign-off on screening thresholds
- UAT with synthetic identities (including sanction test names)
- Data retention policy acknowledged by DPO

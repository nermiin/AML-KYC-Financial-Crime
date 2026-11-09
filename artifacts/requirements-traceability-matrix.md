# Requirements Traceability Matrix — AML/KYC

| Business Objective | Business Rule | FR | User Story |
|--------------------|---------------|-----|------------|
| BO-01 Fast onboarding | BR-KYC-01 | FR-03 | US-KYC-01 |
| BO-02 Real-time screening | BR-KYC-02 | FR-01 | US-KYC-02 |
| BO-03 Audit trail | BR-KYC-02, BR-KYC-03 | FR-02, FR-06 | US-AML-01 |
| BO-02 | BR-KYC-03 | FR-04 | US-KYC-03 |
| BO-03 | — | FR-07 | US-AML-02 |

Coverage: all 8 baseline FRs, 6 user stories and 3 business objectives are traced once the Coverage Review below is included.

## Coverage Review

Requirements review found FR-05 and FR-08 untraced. They are closed below, together with the approved change requests.

| Objective | Rule or CR | FR | User Story | Test |
|-----------|------------|-----|------------|------|
| BO-03 | none | FR-05 | US-AML-01 | Investigator walkthrough |
| BO-01 | BR-AML-01 (CR-01) | FR-08 | US-KYC-04 | TC-12 |
| BO-03 Confidentiality | BR-AML-01 (CR-01) | FR-05 | US-AML-01 | TC-11 |
| BO-02 Complete screening | BR-KYC-02 amended (CR-02) | FR-01 | US-KYC-02 | TC-03 to TC-05 |
| BO-02 Ongoing control | BR-KYC-04 (CR-03) | FR-01 | US-KYC-02 | TC-08 |
| BO-03 Retention | CR-04 | FR-02 | US-AML-01 | Retention job test |
| BO-02 Defined rating | BR-KYC-05 (CR-05) | FR-03 | US-KYC-01 | TC-09 |
| BO-02 PEP handling | BR-KYC-06 (CR-06) | FR-04 | US-KYC-03 | TC-10 |
| BO-03 Filing deadlines | CR-07 | FR-09 | US-AML-02 | TC-14 |

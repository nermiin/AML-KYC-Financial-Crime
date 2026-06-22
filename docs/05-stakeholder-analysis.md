# Stakeholder Analysis — AML/KYC

## RACI — Onboarding Release

| Activity | Product | Compliance | FC Ops | Engineering | MLRO |
|----------|---------|------------|--------|-------------|------|
| Define risk rating rules | C | **R/A** | C | I | C |
| Approve screening thresholds | I | **R/A** | C | I | C |
| Build onboarding flows | **R** | C | I | **A** | I |
| Investigate alerts | I | C | **R/A** | I | C |
| File SAR | I | C | R | I | **A** |

## Key Stakeholders

- **MLRO** — Accountable for SAR quality and timeliness
- **Head of Financial Crime** — Operations capacity and tooling
- **DPO** — GDPR and data minimization
- **Vendor Management** — ID&V and watchlist SLAs

## Communication

| Forum | Cadence | Purpose |
|-------|---------|---------|
| Compliance design authority | Weekly | Rule and threshold changes |
| Go-live readiness | Once | Sign-off checklist |
| Post-incident review | Ad hoc | Screening misses or false negatives |

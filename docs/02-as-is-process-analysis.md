# As-Is Process Analysis — KYC Onboarding & Alert Investigation

## 1. Customer Onboarding (Current State)

```mermaid
sequenceDiagram
    participant C as Customer
    participant App as Mobile App
    participant Ops as Manual Review Queue
    participant WL as Watchlist (batch)

    C->>App: Submit ID + selfie
    App->>App: Auto OCR — no liveness check
    App->>Ops: All non-DE passports to manual queue
    Note over Ops: 48h SLA — often breached
    Ops->>WL: Weekly batch PEP screen
    WL-->>Ops: Hits reviewed next business day
    Ops->>C: Account opened or rejected (email only)
```

## 2. Alert Investigation (Current State)

```mermaid
flowchart TB
    A[TM rule fires] --> B[Alert in queue — FIFO]
    B --> C{Analyst capacity?}
    C -->|Backlog > 5 days| D[SLA breach]
    C -->|Available| E[Analyst opens case]
    E --> F[Copy data to spreadsheet]
    F --> G{Decision}
    G -->|Close| H[No structured rationale]
    G -->|Escalate| I[Email MLRO]
    I --> J[SAR drafted in Word template]
```

## 3. Pain Points

| ID | Issue | Business Impact |
|----|-------|-----------------|
| PP-K01 | PEP screening batched weekly | Exposure window up to 7 days |
| PP-K02 | No liveness detection | Synthetic identity fraud risk |
| PP-K03 | Alert investigation offline | No audit trail; regulator challenge |
| PP-K04 | Inconsistent EDD triggers | Some high-risk customers fast-tracked |
| PP-K05 | SAR narrative quality varies | Rework and filing delays |

## 4. To-Be Vision (Summary)

- Real-time sanctions/PEP at onboarding and daily refresh.
- Risk scoring routes customers to straight-through, enhanced, or reject paths.
- Integrated case management with mandatory disposition codes.
- MLRO dashboard for SAR pipeline and regulatory deadlines.

See [BRD](03-business-requirements-document.md) for detailed requirements.

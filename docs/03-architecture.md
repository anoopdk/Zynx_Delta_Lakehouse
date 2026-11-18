# Architecture

## Context diagram

```mermaid
flowchart LR
  SRC[CSV / source extracts] --> RAW[Raw / Bronze]
  RAW --> ENR[Enriched / Silver]
  ENR --> CUR[Curated / Gold]
  CUR --> BV[Business views]
  BV --> CONSO[Investor analytics consumers]
  CTRL[Control tables + orchestration] -.-> RAW
  CTRL -.-> ENR
  CTRL -.-> CUR
```

## Implementation flow

```mermaid
flowchart TB
  O1[Source to Raw orchestration] --> O2[Raw to Enrich]
  O2 --> O3[Enrich to Curated]
  O3 --> BV[Investor relation business views]
  UT[Unit tests] --> O1
  UT --> O2
```

## Design notes

- Prefer layered medallion / governed platform patterns where applicable.
- Keep configuration and secrets outside Git.
- Preserve auditability: documentation must match repository evidence.

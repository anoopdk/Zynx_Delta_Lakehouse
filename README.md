# Investor Relations Medallion Pipeline

**Delivered by:** [Zynx Technologies Ltd](https://www.zynxtechnologies.com/)  
**Portfolio host (pre-fork):** `anoopdk/medallion_project`  
**Domain client (sanitised):** Zynx x Financial Services Client  
**Engagement type:** Implementation Delivery  
**Portfolio version:** `1.0.0-portfolio`

> Inspection note: Client identity is domain-aliased for public portfolio use. This repository is structured for authenticity review of delivery artefacts, architecture, and engagement scope.

## Project overview

The Financial Services Client needed a governed medallion pipeline that curates investor and fund transaction history so investment teams can answer fund performance, peer comparison, MoM trend, and post-hoc adjustment questions without ad-hoc spreadsheet work.

### Solution delivered

Zynx delivered a Raw -> Enriched -> Curated (Bronze/Silver/Gold) lakehouse-style pipeline with orchestration notebooks, SQL DDL/control tables, business views, unit-test notebooks, and documented Databricks execution evidence.

## Engagement levels

| # | Level |
| --- | --- |
| 1 | Discovery & Requirements Confirmation |
| 2 | Solution Architecture |
| 3 | Implementation Delivery |
| 4 | Validation & Knowledge Transfer |

| Field | Value |
| --- | --- |
| Engagement posture | Delivery portfolio artefact prepared for audit / inspection |
| Confidentiality | Domain alias only; no real client legal name in public docs |
| Primary artefacts | Code/notebooks/docs already in-repo + Zynx standard pack under `docs/` and `decks/` |

## Architecture

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

### Implementation flow

```mermaid
flowchart TB
  O1[Source to Raw orchestration] --> O2[Raw to Enrich]
  O2 --> O3[Enrich to Curated]
  O3 --> BV[Investor relation business views]
  UT[Unit tests] --> O1
  UT --> O2
```

## Technology stack

`Databricks`, `Spark / notebooks`, `SQL DDL`, `Medallion architecture`, `Unit-test notebooks`

## Repository map

| Path | Purpose |
| --- | --- |
| `README.md` | Project overview + engagement levels |
| `docs/` | Problem, solution, architecture, implementation, runbook, audit factsheet |
| `decks/` | 3-slide delivery overview (Markdown + HTML) |
| `src/` / project subtree | Implementation assets (or placeholders where design-led) |
| `tests/` | Tests or placeholders |
| `scripts/` | Operational / helper scripts |
| `configs/` | Non-secret configuration templates |
| `.env.example` | Environment variable placeholders |

### Key existing delivery paths

- `docs/exercise write-up.md`
- `scripts/exercise/exercise_solutions.ipynb`
- `scripts/orchestration/`
- `unit_test/`

## How to inspect this repository

1. Read this README (overview + engagement).
2. Open `docs/audit/PROJECT-FACTSHEET.md`.
3. Review architecture in `docs/03-architecture.md`.
4. Walk implementation evidence listed in the factsheet.
5. Open `decks/overview.html` for the 3-slide executive summary.

## Related Zynx context

Zynx Technologies Ltd provides IT consulting across cloud, data analytics, and digital delivery. This repository is part of the public delivery portfolio hosted on `anoopdk` prior to organisation-level fork.

## License

See `LICENSE`.

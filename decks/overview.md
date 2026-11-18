# Investor Relations Medallion Pipeline

> Zynx Technologies | 3-slide delivery overview

---

## Slide 1: Context & problem

- Zynx x Financial Services Client
- Engagement: Implementation Delivery
- The Financial Services Client needed a governed medallion pipeline that curates investor and fund transaction history so investment teams can answer fund performance, peer comparison, MoM trend, and post-hoc adjustment questions without ad-hoc spreadsheet work.
- Engagement levels: Discovery & Requirements Confirmation -> Solution Architecture -> Implementation Delivery -> Validation & Knowledge Transfer

---

## Slide 2: Solution architecture & stack

- Zynx delivered a Raw -> Enriched -> Curated (Bronze/Silver/Gold) lakehouse-style pipeline with orchestration notebooks, SQL DDL/control tables, business views, unit-test notebooks, and documented Databricks execution evidence.
- Stack: Databricks, Spark / notebooks, SQL DDL, Medallion architecture, Unit-test notebooks
- Architecture documented in docs/03-architecture.md with Mermaid diagrams

---

## Slide 3: Implementation highlights & inspection

- Key evidence: docs/exercise write-up.md, scripts/exercise/exercise_solutions.ipynb, scripts/orchestration/, unit_test/
- Open docs/audit/PROJECT-FACTSHEET.md for the audit index
- Repository standardised to Zynx industrial portfolio structure
- Portfolio tag target: 1.0.0-portfolio

# Evidence Log — D-OPS-1 Monitoring Placement

**Version:** 1.0
**Date:** March 2026

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This log records analytical outputs produced during the development of
D-OPS-1 support infrastructure. All outputs are tagged using the project
O/D/I/A evidence standard.

No outputs constitute findings of fraud, waste, or abuse.
All outputs describe statistical patterns only.

---

## Evidence Standard

| Tag | Meaning |
|---|---|
| O | Observed — directly in the data |
| D | Derived — mathematically constructed from observed data |
| I | Inferred — evidence-based pattern interpretation |
| A | Assumed — declared gap-filling for decision support |

---

## Entries

| Evidence ID | Source | Notebook (Cell ID) | Result | Decision Relevance | Tag | Status |
|---|---|---|---|---|---|---|
| E-DOPS1-01 | df_work — merged from df_pathway (Tier 1) + df_tiered (Tier 2) | D-OPS-1_monitoring_placement.ipynb (DOPS1-03-PREP-01) | Working dataset: 61,416 providers — Tier 1: 22,400 | Tier 2: 39,016 | In D-POL-1 scope (M3): 3,182 | Out of scope (M3 neutral): 58,234 | Foundation dataset for all D-OPS-1 dimension scoring | D | Accepted |
| E-DOPS1-02 | df_work — M1, M2, M3 dimension scores | D-OPS-1_monitoring_placement.ipynb (DOPS1-05-SCORE-01) | M1 mean=0.2050 | M2 mean=0.4283 | M3 mean=0.4901 | Composite: min=0.3288 | max=0.9024 | mean=0.3545 | median=0.3400 | std=0.0520 | nulls=0 | Dimension scoring inputs and composite score for monitoring intensity assignment | D | Accepted |
| E-DOPS1-03 | DOPS1-07-DIAG-01 — Tier 1 pathway score distribution | D-OPS-1_monitoring_placement.ipynb (DOPS1-07-DIAG-01) | Natural breaks confirmed at 0.41 (L2) and 0.58 (L1) | OIG min=0.5751 | TPE min=0.4080 | EdLetter max=0.3970 | Clean gap confirmed between all three pathway populations | THRESHOLD_L1 and THRESHOLD_L2 calibrated (A → D) | Threshold calibration evidence for monitoring intensity assignment | D | Accepted |
| E-DOPS1-04 | monitoring_distribution_v1.png | D-OPS-1_monitoring_placement.ipynb (DOPS1-07-VIZ-01) | Two-panel distribution chart produced — full population and Tier 1 by pathway | Natural breaks visible at calibrated thresholds | TPE spillover into L1 confirmed and documented | Visual confirmation of threshold placement and population structure | D | Accepted |
| E-DOPS1-05 | df_work — monitoring_level column | D-OPS-1_monitoring_placement.ipynb (DOPS1-06-PATHWAY-01) | Level 1 Active Surveillance: 1,162 (1.9%) | $0.51B (7.1%) | Level 2 Periodic Review: 1,613 (2.6%) | $0.26B (3.7%) | Level 3 Passive Surveillance: 58,641 (95.5%) | $6.35B (89.2%) | No Education Letter provider crosses into L2 | 49 OIG providers assigned L2 — documented for Decision Owner awareness | All assertions passed | Monitoring intensity level assignments for full roster | D | Accepted |
| E-DOPS1-06 | monitoring_roster_summary_v1.png | D-OPS-1_monitoring_placement.ipynb (DOPS1-07-VIZ-02) | Two-panel roster composition chart produced — provider count and dollar exposure by monitoring intensity level | Summary visualization for recommendation memo | D | Accepted |
| E-DOPS1-07 | monitoring_scored_v1.parquet / monitoring_roster_v1.parquet | D-OPS-1_monitoring_placement.ipynb (DOPS1-08-OUTPUT-01) | Two parquet files written and round-trip verified | monitoring_scored_v1.parquet: 61,416 rows | 14 columns | monitoring_roster_v1.parquet: 61,416 rows | 7 columns | Persistent outputs for recommendation memo and D-OPS-2 consumption | D | Accepted |

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data
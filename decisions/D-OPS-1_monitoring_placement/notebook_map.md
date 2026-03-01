# Notebook Map — D-OPS-1 Monitoring Placement

**Version:** 1.0
**Date:** March 2026
**Notebook:** `notebooks/decision/D-OPS-1_monitoring_placement.ipynb`
**Decision:** D-OPS-1 — Monitoring Placement

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This map documents the cell-level architecture of the D-OPS-1 notebook.
It records each cell's ID, purpose, inputs, and outputs to support
reproducibility, audit, and future maintenance.

---

## Cell Inventory

| Cell ID | Type | Purpose | Inputs | Outputs |
|---|---|---|---|---|
| DOPS1-00-HEADER-00 | Markdown | Notebook title, decision context, monitoring intensity level definitions, what this notebook does and does not do, legal disclaimer | None | None |
| DOPS1-00-CONFIG-01 | Code | All imports, project paths, input file references, scope parameters, neutral score, dimension weights, M1 pathway scores, provisional then calibrated intensity thresholds, percentile ladder, print width, reproducibility seed, and IBM Carbon CVD palette. No hardcoded variables elsewhere in notebook. | None | All CONFIG constants — paths, MONITOR_TIERS, DPOL1_MAX_DIMS, NEUTRAL_SCORE, W_M1, W_M2, W_M3, M1_SCORES, THRESHOLD_L1, THRESHOLD_L2, PERCENTILE_LADDER, PRINT_WIDTH, RANDOM_STATE, CVD_PALETTE |
| DOPS1-01-LOAD-01 | Code | Load all four upstream dependency files. No transformations — load and confirm only. | `provider_tiered_v1.parquet`, `escalation_pathway_v1.parquet`, `escalation_scored_v1.parquet`, `code_scrutiny_flags_v1.parquet` | `df_tiered` (970,848 rows), `df_pathway` (22,400 rows), `df_scored` (22,400 rows), `df_flags` (3,182 rows) |
| DOPS1-02-VALIDATE-01 | Code | Six integrity checks — no duplicate NPIs in any input file, all df_pathway NPIs present in df_tiered, all df_flags NPIs present in df_pathway, pathway values match expected set, dimensions_flagged range [0, 3], Tier 2 provider count confirmation. anomaly_tier string label format confirmed via diagnostic. | `df_tiered`, `df_pathway`, `df_flags` | Assertion pass/fail output; Tier 2 count confirmed at 39,016 |
| DOPS1-03-PREP-00 | Markdown | Section header — data preparation. Documents anomaly_tier string label format discovery — values are Tier1_Review, Tier2_Monitor, Tier3_Watch, No_Flag. All tier references in notebook use string label form. Confirmed via DOPS1-02-VALIDATE-01 diagnostic. | None | None |
| DOPS1-03-PREP-01 | Code | Build df_work — D-OPS-1 working dataset. Combines Tier 1 from df_pathway and Tier 2 from df_tiered. Joins dimensions_flagged from df_flags. Out-of-scope providers flagged for M3 neutral assignment. | `df_pathway`, `df_tiered`, `df_flags` | `df_work` — 61,416 rows, 9 columns; Tier 1: 22,400 | Tier 2: 39,016 | M3 in scope: 3,182 | M3 neutral pending: 58,234 |
| DOPS1-05-SCORE-01 | Code | Dimension scoring — M1 pathway weight, M2 escalation score carry-forward, M3 code scrutiny intensity normalized to [0,1]. Composite monitoring placement score as weighted sum. Range assertions and percentile table for threshold calibration. | `df_work` | `m1_pathway_score`, `m2_escalation_score`, `m3_code_scrutiny`, `monitoring_score` columns in `df_work`; composite range [0.3288, 0.9024]; mean=0.3545; median=0.3400; std=0.0520 |
| DOPS1-07-DIAG-01 | Code | Tier 1 provider score distribution diagnostic. Isolates Tier 1 from Tier 2 mass. Identifies natural breaks at 0.41 and 0.58. Calibrates THRESHOLD_L1 and THRESHOLD_L2. Pathway-level score summary confirms clean population separation. | `df_work` | Printed diagnostic — natural breaks confirmed; thresholds calibrated (A → D); decision documented in DL-DOPS1-08 |
| DOPS1-07-VIZ-01 | Code | Two-panel composite monitoring score distribution. Left: full population (Tier 1 + Tier 2). Right: Tier 1 only colored by pathway. Calibrated thresholds shown. | `df_work` | `outputs/figures/monitoring_distribution_v1.png` (tag: D) |
| DOPS1-06-PATHWAY-01 | Code | Monitoring intensity level assignment — routes each provider to Level 1 Active Surveillance, Level 2 Periodic Review, or Level 3 Passive Surveillance based on calibrated thresholds. Level summary and cross-tab by pathway. Assertions. | `df_work` (monitoring_score) | `monitoring_level` column in `df_work`; Level 1: 1,162 (1.9%) | Level 2: 1,613 (2.6%) | Level 3: 58,641 (95.5%) |
| DOPS1-08-OUTPUT-01 | Code | Write two parquet output files. Round-trip verification for both. | `df_work` | `data/processed/monitoring_scored_v1.parquet` (61,416 rows, 14 cols) | `data/processed/monitoring_roster_v1.parquet` (61,416 rows, 7 cols) |
| DOPS1-07-VIZ-02 | Code | Two-panel monitoring roster summary bar chart — provider count and dollar exposure by monitoring intensity level. | `level_summary` | `outputs/figures/monitoring_roster_summary_v1.png` (tag: D) |
| DOPS1-09-EVIDENCE-01 | Code | Evidence log summary — E-DOPS1-01 through E-DOPS1-07, all O/D/I/A tagged. Mirrors entries in decisions/D-OPS-1_monitoring_placement/evidence_log.md. | Summary stats from prior cells | Printed evidence log entries |

---

## Input / Output Summary

### Inputs

| File | Source | Used In |
|---|---|---|
| `provider_tiered_v1.parquet` | D-PI-1 output | DOPS1-01-LOAD-01 |
| `escalation_pathway_v1.parquet` | D-PI-2 output | DOPS1-01-LOAD-01 |
| `escalation_scored_v1.parquet` | D-PI-2 output | DOPS1-01-LOAD-01 |
| `code_scrutiny_flags_v1.parquet` | D-POL-1 output | DOPS1-01-LOAD-01 |

### Outputs

| File | Location | Tag | Produced In |
|---|---|---|---|
| `monitoring_scored_v1.parquet` | `data/processed/` | D | DOPS1-08-OUTPUT-01 |
| `monitoring_roster_v1.parquet` | `data/processed/` | D | DOPS1-08-OUTPUT-01 |
| `monitoring_distribution_v1.png` | `outputs/figures/` | D | DOPS1-07-VIZ-01 |
| `monitoring_roster_summary_v1.png` | `outputs/figures/` | D | DOPS1-07-VIZ-02 |

---

## Cell Numbering Note

DOPS1-06-PATHWAY-01 (intensity assignment) appears after DOPS1-07-VIZ-01
(distribution visualization) in notebook execution order. Cell IDs reflect
logical module numbering established at design time; execution order in the
notebook differs. This is intentional — visualization and diagnostic calibrate
thresholds before intensity assignment executes.

DOPS1-07-DIAG-01 is retained in the notebook as a permanent analytical
record. It is not a temporary diagnostic — it provides the threshold
calibration evidence referenced in DL-DOPS1-08 and E-DOPS1-03.

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
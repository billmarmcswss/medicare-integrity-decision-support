# Notebook Map — D-POL-2 Specialty Pattern Shift

**Version:** 1.0
**Date:** February 2026
**Notebook:** `notebooks/decision/D-POL-2_specialty_shift.ipynb`
**Decision:** D-POL-2 — Specialty Pattern Shift

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This map documents the cell-level architecture of the D-POL-2 notebook.
It records each cell's ID, purpose, inputs, and outputs to support
reproducibility, audit, and future maintenance.

---

## Cell Inventory

| Cell ID | Type | Purpose | Inputs | Outputs |
|---|---|---|---|---|
| DPOL2-00-HEADER-00 | Markdown | Notebook title, decision context, two-track summary, upstream dependencies, legal disclaimer | None | None |
| DPOL2-00-CONFIG-01 | Code | All imports, paths, parameters, and IBM Carbon CVD palette. No hardcoded variables elsewhere in notebook. | None | All CONFIG constants — paths, TARGET_PATHWAYS, MIN_SPECIALTY_SIZE, TOP_N_DISPLAY, S1_CONCENTRATION_THRESHOLD, S2_PATHWAY_RATIO_THRESHOLD, S4_SHIFT_MAGNITUDE_THRESHOLD, S4_MIN_PROVIDER_COUNT, BASELINE_YEAR, NEUTRAL_SCORE, RANDOM_STATE, CVD_PALETTE |
| DPOL2-01-LOAD-00 | Markdown | Section header — data load | None | None |
| DPOL2-01-LOAD-01 | Code | Load all six upstream dependency files. No transformations — load and confirm only. | `code_scrutiny_flags_v1.parquet`, `top_codes_by_frequency_v1.parquet`, `top_codes_by_exposure_v1.parquet`, `escalation_pathway_v1.parquet`, `provider_tiered_v1.parquet`, `provider_rollup_v1.parquet` | `df_flags`, `df_freq`, `df_exposure`, `df_pathway`, `df_tiered`, `df_rollup` |
| DPOL2-02-VALIDATE-00 | Markdown | Section header — data validation | None | None |
| DPOL2-02-VALIDATE-01 | Code | Four integrity checks — no duplicate NPIs, pathway distribution matches D-PI-2 documented results, all flagged NPIs present in df_tiered, no null Provider Types in population denominator. NOTE: Pathway counts 655 and 2,527 are Data Year 2023 validated results — update if data year changes. | `df_flags`, `df_pathway`, `df_tiered` | Assertion pass/fail output; `provtype_col` identified |
| DPOL2-03-PREP-00 | Markdown | Section header — data preparation. Documents two-track architecture and population denominator sourcing. | None | None |
| DPOL2-03-PREP-01 | Code | Build Track 1 working dataset — join flagged providers to specialty assignments and full population denominators. NOTE: pathway and composite_score already present in df_flags from D-POL-1 — no pathway join required. | `df_flags`, `df_tiered` | `df_specialty` (3,182 rows) — provider x specialty working dataset |
| DPOL2-03-PREP-02 | Code | Build Track 2 baseline dataset — aggregate provider_rollup_v1 by Provider Type for Data Year 2023 baseline metrics. | `df_rollup` | `df_baseline` (104 rows) — specialty baseline metrics; `df_baseline_reliable` (101 rows) |
| DPOL2-04-SCORE-00 | Markdown | Section header — dimension scoring. Four dimensions table with track assignment. | None | None |
| DPOL2-04-SCORE-01 | Code | S1 — Specialty Anomaly Concentration (Track 1 — tag D). Flagged provider share by specialty vs S1_CONCENTRATION_THRESHOLD=0.0075. Neutral score for specialties below MIN_SPECIALTY_SIZE. | `df_specialty`, `specialty_population` | `specialty_flagged` dataframe; `s1_concentration_flag` column; 9 specialties flagged |
| DPOL2-04-SCORE-02 | Code | S2 — Pathway Stratification by Specialty (Track 1 — tag D). OIG share by specialty vs S2_PATHWAY_RATIO_THRESHOLD=0.35. Neutral score for specialties below MIN_SPECIALTY_SIZE. | `df_specialty` | `pathway_wide` dataframe; `s2_pathway_flag` column; 11 specialties flagged |
| DPOL2-04-SCORE-03 | Code | S3 — Specialty Norm Baseline (Track 2 — tag A — data-constrained). Data Year 2023 median metrics by Provider Type. | `df_baseline` | Printed baseline summary; reliability flags confirmed |
| DPOL2-04-SCORE-04 | Code | S4 — Shift Detection Threshold Design (Track 2 — tag A — data-constrained). detect_specialty_shift() function defined and ready. Awaiting Data Year 2024+. | None | `detect_specialty_shift()` function defined |
| DPOL2-05-AGGREGATE-00 | Markdown | Section header — aggregation. Two summary views described. | None | None |
| DPOL2-05-AGGREGATE-01 | Code | Track 1 specialty-level flag summary. Merges S1 and S2 results. dimensions_flagged uses S1 and S2 only. Three specialties with both dimensions firing. | `specialty_flagged`, `pathway_wide` | `specialty_summary` dataframe (74 rows) |
| DPOL2-05-AGGREGATE-02 | Code | Track 2 baseline summary. Top 20 specialties by median allowed per bene. Tag A — data-constrained. | `df_baseline_reliable` | Printed baseline table |
| DPOL2-06-VIZ-00 | Markdown | Section header — visualizations. Three visualizations described. | None | None |
| DPOL2-06-VIZ-01 | Code | Track 1 — Specialty anomaly concentration panel. Horizontal bar chart — reliable specialties ranked by flagged share. CVD-compliant. Tag D. | `specialty_flagged` | `outputs/figures/specialty_concentration_v1.png` |
| DPOL2-06-VIZ-02 | Code | Track 1 — OIG vs TPE specialty pathway distribution. Stacked bar chart — reliable specialties ranked by OIG share. CVD-compliant. Tag D. | `pathway_wide` | `outputs/figures/specialty_pathway_distribution_v1.png` |
| DPOL2-06-VIZ-03 | Code | Track 2 — Specialty norm baseline visualization. Three-panel grouped bar chart — top 20 specialties by median allowed per bene, services per bene, top-1 code share. CVD-compliant. Tag A. | `df_baseline_reliable` | `outputs/figures/specialty_baseline_v1.png` |
| DPOL2-07-OUTPUT-00 | Markdown | Section header — output | None | None |
| DPOL2-07-OUTPUT-01 | Code | Write three parquet output files. Round-trip verification for all three. | `specialty_summary`, `df_baseline`, `pathway_wide` | `specialty_pattern_flags_v1.parquet`, `specialty_baseline_v1.parquet`, `specialty_pathway_detail_v1.parquet` |
| DPOL2-08-EVIDENCE-00 | Markdown | Section header — evidence log | None | None |
| DPOL2-08-EVIDENCE-01 | Code | Evidence log summary — E-DPOL2-01 through E-DPOL2-08, all O/D/I/A tagged. Track 2 data constraint notice. Three dual-dimension specialties documented. | Summary stats from prior cells | Printed evidence log entries |

---

## Input / Output Summary

### Inputs

| File | Source | Used In |
|---|---|---|
| `code_scrutiny_flags_v1.parquet` | D-POL-1 output | DPOL2-01-LOAD-01 |
| `top_codes_by_frequency_v1.parquet` | D-POL-1 output | DPOL2-01-LOAD-01 |
| `top_codes_by_exposure_v1.parquet` | D-POL-1 output | DPOL2-01-LOAD-01 |
| `escalation_pathway_v1.parquet` | D-PI-2 output | DPOL2-01-LOAD-01 |
| `provider_tiered_v1.parquet` | D-PI-1 output | DPOL2-01-LOAD-01 |
| `provider_rollup_v1.parquet` | D-PI-1 output | DPOL2-01-LOAD-01 |

### Outputs

| File | Location | Tag | Produced In |
|---|---|---|---|
| `specialty_pattern_flags_v1.parquet` | `data/processed/` | D | DPOL2-07-OUTPUT-01 |
| `specialty_baseline_v1.parquet` | `data/processed/` | A | DPOL2-07-OUTPUT-01 |
| `specialty_pathway_detail_v1.parquet` | `data/processed/` | D | DPOL2-07-OUTPUT-01 |
| `specialty_concentration_v1.png` | `outputs/figures/` | D | DPOL2-06-VIZ-01 |
| `specialty_pathway_distribution_v1.png` | `outputs/figures/` | D | DPOL2-06-VIZ-02 |
| `specialty_baseline_v1.png` | `outputs/figures/` | A | DPOL2-06-VIZ-03 |

---

## Track 2 Data Constraint Notice

Cells DPOL2-04-SCORE-03, DPOL2-04-SCORE-04, DPOL2-05-AGGREGATE-02,
DPOL2-06-VIZ-03 tagged A. Temporal shift detection requires minimum
two data years. Data Year 2023 establishes baseline only. Cells resolve
to D when Data Year 2024+ incorporated. All parameters and detection
logic fully documented and operationally ready.

## Provisional Thresholds

| Parameter | Status |
|---|---|
| S1_CONCENTRATION_THRESHOLD = 0.0075 | Resolved — tag D |
| S2_PATHWAY_RATIO_THRESHOLD = 0.35 | Resolved — tag D |
| S4_SHIFT_MAGNITUDE_THRESHOLD = 0.20 | Provisional — data-constrained |

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
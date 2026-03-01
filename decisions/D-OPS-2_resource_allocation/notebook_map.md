# Notebook Map — D-OPS-2 Resource Allocation

**Version:** 1.0
**Date:** March 2026
**Notebook:** notebooks/decision/D-OPS-2_resource_allocation.ipynb
**Decision:** D-OPS-2 — Resource Allocation

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This map documents the cell-level architecture of the D-OPS-2 notebook.
It records each cell's ID, purpose, inputs, and outputs to support
reproducibility, audit, and future maintenance.

---

## Cell Inventory

| Cell ID | Type | Purpose | Inputs | Outputs |
|---|---|---|---|---|
| DOPS2-00-HEADER-00 | Markdown | Notebook title, decision context, four-denominator architecture summary, upstream dependencies, legal disclaimer | None | None |
| DOPS2-00-CONFIG-01 | Code | All imports, paths, parameters, and IBM Carbon CVD palette. No hardcoded variables elsewhere in notebook. | None | All CONFIG constants — paths, R1_SCORES, HOURS_L1, HOURS_L2, HOURS_L3, ANNUAL_PRODUCTIVE_HOURS, CASE_SLOT_RATE_L1, CASE_SLOT_RATE_L2, CASE_SLOT_RATE_L3, UPIC_TOTAL_CASE_CAPACITY, DIMENSION_WEIGHTS, NEUTRAL_SCORE, PRINT_WIDTH, CVD_PALETTE |
| DOPS2-01-LOAD-00 | Markdown | Section header — data load | None | None |
| DOPS2-01-LOAD-01 | Code | Load monitoring_roster_v1.parquet. Confirm shape, required columns, null checks, duplicate NPI check. No transformations — load and confirm only. | monitoring_roster_v1.parquet | df_roster (61,416 rows) — NPI, monitoring_level, pathway, allowed dollars, composite score |
| DOPS2-02-VALIDATE-00 | Markdown | Section header — data validation | None | None |
| DOPS2-02-VALIDATE-01 | Code | Four integrity checks — monitoring_level values match expected set (Level_1, Level_2, Level_3), pathway values match expected set, provider counts match D-OPS-1 documented results (L1=1,162, L2=1,613, L3=58,641), total allowed dollars within tolerance of D-OPS-1 documented totals. NOTE: Level and pathway counts are Data Year 2023 validated results — update if data year changes. | df_roster | Assertion pass/fail output |
| DOPS2-03-PREP-00 | Markdown | Section header — data preparation. Documents nine-cell structure design and L3-EdLetter/Tier2 combined cell rationale. | None | None |
| DOPS2-03-PREP-01 | Code | Build nine-cell aggregation — group df_roster by monitoring_level × pathway. Compute cell_count and cell_allowed for each cell. Assign cell_label. Confirm nine cells present. Flag L3-Tier2Only providers as combined into L3-EdLetter cell. | df_roster | df_cells (9 rows) — cell_label, monitoring_level, pathway, cell_count, cell_allowed |
| DOPS2-04-SCORE-00 | Markdown | Section header — dimension scoring. Three dimensions described with track assignment and evidence tags. | None | None |
| DOPS2-04-SCORE-01 | Code | R1 — Monitoring Intensity Weight (tag D). Assign R1 score by monitoring_level using R1_SCORES dict. Confirm all nine cells scored. No nulls. | df_cells | r1_score column in df_cells |
| DOPS2-04-SCORE-02 | Code | R2 — Provider Count Concentration (tag D). Normalize cell_count to [0,1] relative to max cell count. Confirm range [0,1]. No nulls. | df_cells | r2_score column in df_cells |
| DOPS2-04-SCORE-03 | Code | R3 — Dollar Exposure Concentration (tag D). Normalize cell_allowed to [0,1] relative to max cell allowed. Confirm range [0,1]. No nulls. | df_cells | r3_score column in df_cells |
| DOPS2-05-AGGREGATE-00 | Markdown | Section header — composite score and priority ranking. Weighted sum formula and weight rationale documented. | None | None |
| DOPS2-05-AGGREGATE-01 | Code | Composite allocation score — weighted sum of R1, R2, R3 using DIMENSION_WEIGHTS. Confirm score range, mean, median, no nulls. Print nine-cell priority ranking table sorted by composite score descending. | df_cells | allocation_score column in df_cells; printed priority ranking table |
| DOPS2-06-VIEWA-00 | Markdown | Section header — View A Review Hours. Assumption declaration and tag (A) noted. | None | None |
| DOPS2-06-VIEWA-01 | Code | View A — Review Hours. Compute cell_hours = cell_count × HOURS_BY_LEVEL. Compute hours_share as percentage of total modeled hours. Print View A allocation table ranked by allocation_score. | df_cells | cell_hours, hours_share columns in df_cells; printed View A table |
| DOPS2-06-VIEWB-00 | Markdown | Section header — View B FTE Capacity. Assumption declaration and tag (A) noted. | None | None |
| DOPS2-06-VIEWB-01 | Code | View B — FTE Capacity. Compute cell_fte = cell_hours / ANNUAL_PRODUCTIVE_HOURS. Compute fte_share as percentage of total modeled FTE. Print View B allocation table ranked by allocation_score. | df_cells | cell_fte, fte_share columns in df_cells; printed View B table |
| DOPS2-06-VIEWC-00 | Markdown | Section header — View C Case Slots. Assumption declaration and tag (A) noted. UPIC_TOTAL_CASE_CAPACITY ceiling documented. | None | None |
| DOPS2-06-VIEWC-01 | Code | View C — Case Slots. Compute cell_slots = cell_count × CASE_SLOT_RATE_BY_LEVEL. Compute slots_share as percentage of UPIC_TOTAL_CASE_CAPACITY. Flag if total modeled slots exceed capacity ceiling. Print View C allocation table ranked by allocation_score. | df_cells | cell_slots, slots_share columns in df_cells; printed View C table; capacity ceiling check |
| DOPS2-06-VIEWD-00 | Markdown | Section header — View D Abstract Capacity Units. Tag (D) noted. Portability rationale documented. | None | None |
| DOPS2-06-VIEWD-01 | Code | View D — Abstract Capacity Units. Compute acu = (allocation_score / sum_all_scores) × 100. Confirm ACU values sum to 100. Compute cumulative ACU by priority rank. Print View D allocation table. | df_cells | acu, cumulative_acu columns in df_cells; printed View D table |
| DOPS2-07-VIZ-00 | Markdown | Section header — visualizations. Three visualizations described. | None | None |
| DOPS2-07-VIZ-01 | Code | Cell priority ranking heatmap — nine cells arranged in level × pathway grid, color-scaled by composite allocation score. CVD-compliant. Tag D. | df_cells | outputs/figures/allocation_priority_v1.png |
| DOPS2-07-VIZ-02 | Code | Four-panel bar chart — resource share by cell for all four denominator views. Cells ranked by composite score within each panel. CVD-compliant. Tag A/D. | df_cells | outputs/figures/allocation_burden_v1.png |
| DOPS2-07-VIZ-03 | Code | Cumulative resource coverage chart — all four views overlaid. X-axis: priority rank (1–9). Y-axis: cumulative resource share (%). CVD-compliant. Tag A/D. | df_cells | outputs/figures/allocation_cumulative_v1.png |
| DOPS2-08-OUTPUT-00 | Markdown | Section header — output | None | None |
| DOPS2-08-OUTPUT-01 | Code | Write two parquet output files. Round-trip verification for both. | df_cells | data/processed/allocation_cells_v1.parquet (9 rows — cell scores and aggregations) | data/processed/allocation_views_v1.parquet (9 rows × four view columns) |
| DOPS2-09-SENS-00 | Markdown | Section header — sensitivity analysis. Two sensitivity tests described. | None | None |
| DOPS2-09-SENS-01 | Code | Sensitivity analysis — dimension weights. Test alternative R1/R2/R3 weight combinations. Confirm top-ranked cells stable across tested assumptions. Print rank comparison table. Tag D. | df_cells | Printed rank stability table |
| DOPS2-09-SENS-02 | Code | Sensitivity analysis — View A–C operational assumptions. Test alternative hours, FTE, and case slot parameters. Confirm cell rank order stable — denominator totals shift but relative priorities hold. Print sensitivity summary. Tag A. | df_cells | Printed sensitivity summary |
| DOPS2-10-EVIDENCE-00 | Markdown | Section header — evidence log | None | None |
| DOPS2-10-EVIDENCE-01 | Code | Evidence log summary — E-DOPS2-01 through E-DOPS2-16, all O/D/I/A tagged. Operational Assumption Registry printed. Mirrors entries in decisions/D-OPS-2_resource_allocation/evidence_log.md. | Summary stats from prior cells | Printed evidence log entries |

---

## Input / Output Summary

### Inputs

| File | Source | Used In |
|---|---|---|
| monitoring_roster_v1.parquet | D-OPS-1 output | DOPS2-01-LOAD-01 |

### Outputs

| File | Location | Tag | Produced In |
|---|---|---|---|
| allocation_cells_v1.parquet | data/processed/ | D | DOPS2-08-OUTPUT-01 |
| allocation_views_v1.parquet | data/processed/ | A/D | DOPS2-08-OUTPUT-01 |
| allocation_priority_v1.png | outputs/figures/ | D | DOPS2-07-VIZ-01 |
| allocation_burden_v1.png | outputs/figures/ | A/D | DOPS2-07-VIZ-02 |
| allocation_cumulative_v1.png | outputs/figures/ | A/D | DOPS2-07-VIZ-03 |

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
# Evidence Log — D-OPS-2 Resource Allocation

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
D-OPS-2 support infrastructure. All outputs are tagged using the project
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
| E-DOPS2-01 | monitoring_roster_v1.parquet — load and validation | D-OPS-2_resource_allocation.ipynb (DOPS2-01-LOAD-01) | 61,416 rows confirmed | Required columns present: NPI, monitoring_level, pathway, allowed dollars | No nulls in key columns | Zero duplicate NPIs | Foundation dataset for all D-OPS-2 dimension scoring | D | Pending |
| E-DOPS2-02 | monitoring_roster_v1.parquet — nine-cell structure | D-OPS-2_resource_allocation.ipynb (DOPS2-03-PREP-01) | Nine level × pathway cells constructed | Cell provider counts and allowed dollar aggregations confirmed | L3-EdLetter/Tier2 confirmed as combined cell — Tier2Only providers carry no pathway label | Cell structure input for all dimension scoring | D | Pending |
| E-DOPS2-03 | df_cells — R1 dimension scoring | D-OPS-2_resource_allocation.ipynb (DOPS2-04-SCORE-01) | R1 scores assigned by monitoring level: L1=1.00, L2=0.67, L3=0.33 | All nine cells scored | No nulls | R1 dimension input to composite allocation score | D | Pending |
| E-DOPS2-04 | df_cells — R2 dimension scoring | D-OPS-2_resource_allocation.ipynb (DOPS2-04-SCORE-02) | R2 scores derived: cell_count / max_cell_count | Maximum cell confirmed | All nine cells scored to [0,1] | No nulls | R2 dimension input to composite allocation score | D | Pending |
| E-DOPS2-05 | df_cells — R3 dimension scoring | D-OPS-2_resource_allocation.ipynb (DOPS2-04-SCORE-03) | R3 scores derived: cell_allowed / max_cell_allowed | Maximum cell confirmed | All nine cells scored to [0,1] | No nulls | R3 dimension input to composite allocation score | D | Pending |
| E-DOPS2-06 | df_cells — composite allocation score | D-OPS-2_resource_allocation.ipynb (DOPS2-05-AGGREGATE-01) | Composite score = R1×0.50 + R2×0.25 + R3×0.25 | Score range, mean, median confirmed | No nulls | Nine-cell priority ranking established | Composite allocation score driving all four denominator views | D | Pending |
| E-DOPS2-07 | df_cells — View A Review Hours | D-OPS-2_resource_allocation.ipynb (DOPS2-06-VIEWA-01) | Total estimated review hours per cell: cell_count × HOURS_BY_LEVEL | Hours share per cell as percentage of total modeled burden | Priority ranking by hours | View A allocation table | A | Pending |
| E-DOPS2-08 | df_cells — View B FTE Capacity | D-OPS-2_resource_allocation.ipynb (DOPS2-06-VIEWB-01) | FTE requirement per cell: cell_hours / ANNUAL_PRODUCTIVE_HOURS | FTE share per cell | Priority ranking by FTE | View B allocation table | A | Pending |
| E-DOPS2-09 | df_cells — View C Case Slots | D-OPS-2_resource_allocation.ipynb (DOPS2-06-VIEWC-01) | Case slots per cell: cell_count × CASE_SLOT_RATE_BY_LEVEL | Total modeled case slots vs UPIC_TOTAL_CASE_CAPACITY | Slots share per cell | Priority ranking by case slots | View C allocation table | A | Pending |
| E-DOPS2-10 | df_cells — View D Abstract Capacity Units | D-OPS-2_resource_allocation.ipynb (DOPS2-06-VIEWD-01) | ACU per cell: (cell_composite_score / sum_all_scores) × 100 | ACU shares sum to 100 confirmed | Cumulative ACU by priority rank | View D allocation table | D | Pending |
| E-DOPS2-11 | allocation_priority_v1.png | D-OPS-2_resource_allocation.ipynb (DOPS2-07-VIZ-01) | Cell priority ranking heatmap — nine cells by composite allocation score | Level × pathway grid with score-scaled color intensity | CVD-compliant | Visual confirmation of cell priority structure | D | Pending |
| E-DOPS2-12 | allocation_burden_v1.png | D-OPS-2_resource_allocation.ipynb (DOPS2-07-VIZ-02) | Four-panel bar chart — resource share by cell for all four denominator views | Cells ranked by composite score within each panel | CVD-compliant | Visual comparison of resource distribution across denominator views | A/D | Pending |
| E-DOPS2-13 | allocation_cumulative_v1.png | D-OPS-2_resource_allocation.ipynb (DOPS2-07-VIZ-03) | Cumulative resource coverage by priority rank — all four views overlaid | Shows how many cells capture what share of each resource type | CVD-compliant | Cumulative coverage evidence for Decision Notes | A/D | Pending |
| E-DOPS2-14 | allocation_cells_v1.parquet / allocation_views_v1.parquet | D-OPS-2_resource_allocation.ipynb (DOPS2-08-OUTPUT-01) | Two parquet files written and round-trip verified | allocation_cells_v1.parquet: nine rows — cell-level scores and aggregations | allocation_views_v1.parquet: nine rows × four view columns | Persistent outputs for recommendation memo and D-EXT-1 consumption | D/A | Pending |
| E-DOPS2-15 | Sensitivity analysis — dimension weights | D-OPS-2_resource_allocation.ipynb (DOPS2-09-SENS-01) | Priority ranking stability tested across alternative R1/R2/R3 weight assumptions | Top-ranked cells confirmed stable across tested weight combinations | Sensitivity range documented | Weight assumption robustness evidence for Decision Notes | D | Pending |
| E-DOPS2-16 | Sensitivity analysis — View A–C operational assumptions | D-OPS-2_resource_allocation.ipynb (DOPS2-09-SENS-02) | Priority ranking stability tested across alternative hours, FTE, and case slot assumptions | Cell rank order confirmed stable — denominator view totals shift but relative priorities hold | Assumption sensitivity documented for Decision Owner | A | Pending |

---

## Operational Assumption Registry

All View A–C parameters are asserted (A). Values below are CONFIG defaults.
Decision Owner should substitute operational actuals when available.

| Parameter | Default Value | View | Rationale |
|---|---|---|---|
| HOURS_L1 | 8.0 | A | Active surveillance — investigative engagement per provider |
| HOURS_L2 | 4.0 | A | Periodic review — structured review per provider |
| HOURS_L3 | 0.5 | A | Passive surveillance — tracking only |
| ANNUAL_PRODUCTIVE_HOURS | 1,800 | B | Standard federal workforce productive hours |
| CASE_SLOT_RATE_L1 | 1.00 | C | All L1 providers generate a formal case action |
| CASE_SLOT_RATE_L2 | 0.50 | C | Half of L2 providers generate a formal case per cycle |
| CASE_SLOT_RATE_L3 | 0.05 | C | Sustained anomaly pattern required to trigger formal action |
| UPIC_TOTAL_CASE_CAPACITY | 2,500 | C | Approximate UPIC operational throughput per review cycle |

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
# Evidence Log — D-PI-2 Escalation Threshold

**Version:** 1.0
**Date:** February 2026

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This log records analytical outputs produced during the development of
D-PI-2 support infrastructure. All outputs are tagged using the project
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
| E-DPI2-01 | df_work — merged from df_queue + interim parquet geo join | D-PI-2_escalation_threshold.ipynb (DPI2-03-PREP-00a) | Working dataset: 22,400 rows, 34 columns; zero geo nulls | Foundation dataset for all D-PI-2 dimension scoring | D | Accepted |
| E-DPI2-02 | df_work — D1 through D4 feature columns | D-PI-2_escalation_threshold.ipynb (DPI2-03-PREP-01) | D1 mean=0.432 | D2 mean=0.423 | D3 mean=0.208 | D4 mean=0.082; 801 D4 neutral assignments (subgroup < 10) | Dimension scoring inputs for composite score calculation | D | Accepted |
| E-DPI2-03 | df_work — composite_score column | D-PI-2_escalation_threshold.ipynb (DPI2-05-SCORE-01) | Composite scores: min=0.2052 | max=0.8496 | mean=0.3035 | median=0.2759 | std=0.0837; zero nulls; range fully within [0, 1] | Weighted composite risk score for pathway routing | D | Accepted |
| E-DPI2-04 | pathway_distribution_v1.png | D-PI-2_escalation_threshold.ipynb (DPI2-07-VIZ-01) | Composite score distribution histogram; natural breaks identified at 0.40 (TPE) and 0.50 (OIG); THRESHOLD_OIG and THRESHOLD_TPE calibrated (tag: A → D) | Threshold calibration evidence for pathway assignment | D | Accepted |
| E-DPI2-05 | df_work — pathway column | D-PI-2_escalation_threshold.ipynb (DPI2-06-PATHWAY-01) 
| OIG/DOJ Referral: 655 (2.9%) | TPE: 2,527 (11.3%) | Education Letter: 19,218 (85.8%) 
| Three-pathway escalation routing for D-PI-1 Tier 1 providers | D | Accepted |
| E-DPI2-06 | escalation_scored_v1.parquet / escalation_pathway_v1.parquet 
| D-PI-2_escalation_threshold.ipynb (DPI2-08-OUTPUT-01) 
| Two parquet files written and round-trip verified; 22,400 rows each; scored: 34 columns 
| pathway: 14 columns | Persistent outputs for recommendation memo and downstream modules | D | Accepted |

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
# Notebook Map — D-POL-1 Code Scrutiny

**Version:** 1.0
**Date:** February 2026
**Notebook:** `notebooks/decision/D-POL-1_code_scrutiny.ipynb`
**Decision:** D-POL-1 — Code Scrutiny

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This map documents the cell-level architecture of the D-POL-1 notebook.
It records each cell's ID, purpose, inputs, and outputs to support
reproducibility, audit, and future maintenance.

---

## Cell Inventory

| Cell ID | Type | Purpose | Inputs | Outputs |
|---|---|---|---|---|
| DPOL1-00-HEADER-00 | Markdown | Notebook title, decision context, four dimensions summary, what this notebook does NOT do | None | None |
| DPOL1-00-CONFIG-01 | Code | All imports, project paths, parameters, and IBM Carbon CVD palette. No hardcoded variables elsewhere in notebook. | None | All CONFIG constants — paths, TARGET_PATHWAYS, CORE_Z_THRESHOLD, MIN_PEER_SIZE, TOP_N_CODES, C1_CONCENTRATION_THRESHOLD, C4_NATIONAL_PERCENTILE, TOP_N_DISPLAY, CVD_PALETTE |
| DPOL1-01-LOAD-00 | Markdown | Section header — data load | None | None |
| DPOL1-01-LOAD-01 | Code | Load all upstream dependency files. No transformations — load and confirm only. | `escalation_pathway_v1.parquet`, `escalation_scored_v1.parquet`, `provider_signal_scores_v1.parquet`, `provider_tiered_v1.parquet`, `MUP_PHY_R25_P05_V20_D23_Prov_Svc.csv` | `df_pathway` (22,400 rows), `df_scored` (22,400 rows), `df_signals` (970,848 rows), `df_tiered` (970,848 rows), `df_services_raw` (9,660,647 rows) |
| DPOL1-02-VALIDATE-00 | Markdown | Section header — data validation | None | None |
| DPOL1-02-VALIDATE-01 | Code | Four integrity checks — no duplicate NPIs, pathway distribution matches D-PI-2 documented results, required columns present, no null NPIs or HCPCS codes. | `df_pathway`, `df_services_raw` | Assertion pass/fail output |
| DPOL1-03-PREP-00 | Markdown | Section header — data preparation. Documents Avg_ column derivation note. | None | None |
| DPOL1-03-PREP-01 | Code | Filter services file to 3,182 OIG + TPE providers. Join pathway assignment and composite score. | `df_services_raw`, `df_pathway` | `df_svc` (17,676 rows) — provider x HCPCS working dataset |
| DPOL1-03-PREP-02 | Code | Feature engineering — derived total allowed dollars (Avg x Tot_Benes), allowed per bene, provider totals, code share, Provider Type x HCPCS peer medians from full population. | `df_svc`, `df_services_raw`, `df_tiered` | `allowed_per_bene`, `total_allowed`, `provider_total_allowed`, `code_allowed_share`, `peer_median`, `peer_count` columns in `df_svc` |
| DPOL1-04-SCORE-00 | Markdown | Section header — dimension scoring. Four dimensions table. | None | None |
| DPOL1-04-SCORE-01 | Code | C1 — Code Concentration. Top-N code share vs. Provider Type peer median. | `df_svc`, `df_services_raw`, `df_tiered` | `c1_concentration_flag` column in `df_svc`; `topn_share` dataframe |
| DPOL1-04-SCORE-02 | Code | C2 — Peer Deviation by HCPCS. Allowed per bene vs. peer median Z-score >= CORE_Z_THRESHOLD. | `df_svc`, `df_services_raw` | `c2_zscore`, `c2_deviation_flag` columns in `df_svc` |
| DPOL1-04-SCORE-03 | Code | C3 — High-Concern Signal Overlap. C2 deviation flag firing on provider top-1 HCPCS code. | `df_svc` | `top1_hcpcs`, `c3_overlap_flag` columns in `df_svc` |
| DPOL1-04-SCORE-04 | Code | C4 — Specialty Norm Comparison. Allowed per bene above national p90 threshold by Provider Type x HCPCS. | `df_svc`, `df_services_raw` | `national_pct_threshold`, `c4_national_flag` columns in `df_svc` |
| DPOL1-05-AGGREGATE-00 | Markdown | Section header — aggregation. Two summary views described. | None | None |
| DPOL1-05-AGGREGATE-01 | Code | Provider-level flag summary. dimensions_flagged uses C2, C3, C4 only — C1 excluded per DPOL1-06-DIAG-01. | `df_svc` | `provider_flags` dataframe (3,182 rows) |
| DPOL1-05-AGGREGATE-02 | Code | Cross-provider HCPCS code aggregation. Codes ranked by provider flag frequency and dollar exposure. Primary policy output. | `df_svc` | `code_summary`, `top_by_frequency`, `top_by_exposure` dataframes |
| DPOL1-06-VIZ-00 | Markdown | Section header — visualizations | None | None |
| DPOL1-06-DIAG-01 | Code | C1 non-discriminating diagnostic. Documents OIG median=0.980, TPE median=1.000. C1 excluded from active dimensions. | `oig_share`, `tpe_share` | Printed diagnostic; decision documented in DL-DPOL1-06 |
| DPOL1-06-VIZ-01 | Code | Code concentration distribution by pathway. Side-by-side histogram OIG vs. TPE. | `topn_share`, `df_pathway` | `outputs/figures/code_concentration_v1.png` |
| DPOL1-06-VIZ-02 | Code | Top flagged codes dual panel — frequency (left) and dollar exposure (right). | `top_by_frequency`, `top_by_exposure` | `outputs/figures/top_flagged_codes_v1.png` |
| DPOL1-07-OUTPUT-00 | Markdown | Section header — output | None | None |
| DPOL1-07-OUTPUT-01 | Code | Write four parquet output files. Round-trip verification for all four. | `df_svc`, `provider_flags`, `code_summary` | `hcpcs_scored_v1.parquet`, `code_scrutiny_flags_v1.parquet`, `top_codes_by_frequency_v1.parquet`, `top_codes_by_exposure_v1.parquet` |
| DPOL1-08-EVIDENCE-00 | Markdown | Section header — evidence log | None | None |
| DPOL1-08-EVIDENCE-01 | Code | Evidence log summary — E-DPOL1-01 through E-DPOL1-10, all O/D/I/A tagged. | Summary stats from prior cells | Printed evidence log entries |

---

## Input / Output Summary

### Inputs

| File | Source | Used In |
|---|---|---|
| `escalation_pathway_v1.parquet` | D-PI-2 output | DPOL1-01-LOAD-01 |
| `escalation_scored_v1.parquet` | D-PI-2 output | DPOL1-01-LOAD-01 |
| `provider_signal_scores_v1.parquet` | D-PI-1 output | DPOL1-01-LOAD-01 |
| `provider_tiered_v1.parquet` | D-PI-1 output | DPOL1-01-LOAD-01 |
| `MUP_PHY_R25_P05_V20_D23_Prov_Svc.csv` | CMS PUF raw | DPOL1-01-LOAD-01 |

### Outputs

| File | Location | Tag | Produced In |
|---|---|---|---|
| `hcpcs_scored_v1.parquet` | `data/processed/` | D | DPOL1-07-OUTPUT-01 |
| `code_scrutiny_flags_v1.parquet` | `data/processed/` | D | DPOL1-07-OUTPUT-01 |
| `top_codes_by_frequency_v1.parquet` | `data/processed/` | D | DPOL1-07-OUTPUT-01 |
| `top_codes_by_exposure_v1.parquet` | `data/processed/` | D | DPOL1-07-OUTPUT-01 |
| `code_concentration_v1.png` | `outputs/figures/` | D | DPOL1-06-VIZ-01 |
| `top_flagged_codes_v1.png` | `outputs/figures/` | D | DPOL1-06-VIZ-02 |

---

## Active Dimensions Note

C1 (Code Concentration) was constructed and evaluated but determined
non-discriminating for this population. Top-3 code share median is
0.980 (OIG) and 1.000 (TPE), making the peer-relative threshold
effectively unreachable. C1 is retained in df_svc for future analysis
but excluded from dimensions_flagged. See DPOL1-06-DIAG-01 and
decision log entry DL-DPOL1-06.

Active dimensions: C2, C3, C4.

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
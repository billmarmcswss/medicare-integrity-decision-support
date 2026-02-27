# Evidence Log — D-POL-1 Code Scrutiny

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
D-POL-1 support infrastructure. All outputs are tagged using the project
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
| E-DPOL1-01 | df_svc — services file filtered to OIG + TPE NPIs | D-POL-1_code_scrutiny.ipynb (DPOL1-03-PREP-01) | 17,676 provider x HCPCS rows | 3,182 unique providers | 1,509 unique HCPCS codes | Foundation dataset for all D-POL-1 dimension scoring | D | Accepted |
| E-DPOL1-02 | df_svc — feature engineering (allowed_per_bene, peer_median, code_allowed_share) | D-POL-1_code_scrutiny.ipynb (DPOL1-03-PREP-02) | 17,676 rows | 567 null peer_median rows assigned neutral score | 0 null allowed_per_bene | Peer comparison inputs for C2 and C4 scoring | D | Accepted |
| E-DPOL1-03 | df_svc — C1 concentration flag | D-POL-1_code_scrutiny.ipynb (DPOL1-04-SCORE-01) | 615 of 3,182 providers flagged | C1 subsequently determined non-discriminating (DPOL1-06-DIAG-01) — OIG median top-3 share=0.980, TPE median=1.000 | C1 excluded from active dimensions — annotated for future review | A | Accepted — excluded from active dimensions |
| E-DPOL1-04 | df_svc — C2 peer deviation flag | D-POL-1_code_scrutiny.ipynb (DPOL1-04-SCORE-02) | 3,237 provider x HCPCS rows flagged | 878 unique providers | 567 neutral score rows (peer group below MIN_PEER_SIZE) | Code-level peer deviation scoring input | D | Accepted |
| E-DPOL1-05 | df_svc — C3 high-concern signal overlap flag | D-POL-1_code_scrutiny.ipynb (DPOL1-04-SCORE-03) | 577 provider x HCPCS rows flagged | 577 unique providers | Convergence of C2 deviation and top-1 code concentration | Signal convergence input for provider risk profile | D | Accepted |
| E-DPOL1-06 | df_svc — C4 specialty norm comparison flag | D-POL-1_code_scrutiny.ipynb (DPOL1-04-SCORE-04) | 5,054 provider x HCPCS rows flagged | 1,500 unique providers | 0 neutral score rows | National norm comparison input | D | Accepted |
| E-DPOL1-07 | provider_flags — revised dimensions_flagged (C2, C3, C4 only) | D-POL-1_code_scrutiny.ipynb (DPOL1-05-AGGREGATE-01 revised) | 3,182 providers | 0 dims: 1,645 | 1 dim: 673 | 2 dims: 310 | 3 dims: 554 | Provider-level flag summary for recommendation memo | D | Accepted |
| E-DPOL1-08 | code_summary — cross-provider HCPCS code aggregation | D-POL-1_code_scrutiny.ipynb (DPOL1-05-AGGREGATE-02) | 1,886 unique flagged HCPCS codes | 5,494 flagged provider x code rows | Top frequency: 99223 Hospitalist (100 providers) | Top exposure: 81519 Clinical Laboratory (~$85M) | Primary policy output — actionable code list for CPI | D | Accepted |
| E-DPOL1-09 | code_concentration_v1.png / top_flagged_codes_v1.png | D-POL-1_code_scrutiny.ipynb (DPOL1-06-VIZ-01 / DPOL1-06-VIZ-02) | Two visualizations produced — concentration distribution by pathway and top code dual panel (frequency + exposure) | Threshold calibration evidence and policy visualization | D | Accepted |
| E-DPOL1-10 | Four parquet output files | D-POL-1_code_scrutiny.ipynb (DPOL1-07-OUTPUT-01) | hcpcs_scored_v1.parquet (17,676 rows, 47 cols) | code_scrutiny_flags_v1.parquet (3,182 rows, 10 cols) | top_codes_by_frequency_v1.parquet (1,886 rows, 7 cols) | top_codes_by_exposure_v1.parquet (1,886 rows, 7 cols) — all round-trip verified | Persistent outputs for recommendation memo and downstream modules | D | Accepted |

---

## Governance Footnot
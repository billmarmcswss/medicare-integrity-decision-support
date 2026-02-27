# Notebook Map — D-PI-2 Escalation Threshold

**Version:** 1.0
**Date:** February 2026
**Notebook:** `notebooks/decision/D-PI-2_escalation_threshold.ipynb`
**Decision:** D-PI-2 — Escalation Threshold Assignment

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This map documents the cell-level architecture of the D-PI-2 notebook.
It records each cell's ID, purpose, inputs, and outputs to support
reproducibility, audit, and future maintenance.

One diagnostic cell (Cell 20) is present in the working notebook and
should be removed before any final archive or publication push.

---

## Cell Inventory

| Cell ID | Type | Purpose | Inputs | Outputs |
|---|---|---|---|---|
| DPI2-00-HEADER-00 | Markdown | Notebook title, decision context, legal disclaimer, pathway definitions | None | None |
| DPI2-00-CONFIG-01 | Code | All imports, project paths, parameters, weights, thresholds, and IBM Carbon palette. No hardcoded variables elsewhere in notebook. | None | `PROJECT_ROOT`, `PROCESSED_DIR`, `OUTPUTS_DIR`, `FIGURES_DIR`, `BRIEFS_DIR`, `DECISION_DIR`, all CONFIG constants |
| DPI2-01-LOAD-00 | Markdown | Section header — data load | None | None |
| DPI2-01-LOAD-01 | Code | Load all four D-PI-1 output parquet files. No transformations — load and confirm only. | `provider_review_queue_v1.parquet`, `provider_signal_scores_v1.parquet`, `provider_tiered_v1.parquet`, `provider_rollup_v1.parquet` | `df_queue`, `df_signals`, `df_tiered`, `df_rollup` |
| DPI2-02-VALIDATE-00 | Markdown | Section header — data validation | None | None |
| DPI2-02-VALIDATE-01 | Code | Data integrity checks — no duplicate NPIs, all queue NPIs in signals, null Z-score treatment (S6, S7 nulls → 0; tag: A). All checks must pass before scoring begins. | `df_queue`, `df_signals` | Assertion pass/fail output; null Z-scores zeroed in `df_queue` |
| DPI2-03-PREP-00 | Markdown | Section header — dataset preparation (x2) | None | None |
| DPI2-03-PREP-00a | Code | Build `df_work` — D-PI-2 working dataset. Joins `Rndrng_Prvdr_State_Abrvtn` and `Rndrng_Prvdr_RUCA` from interim parquet (not present in processed files). | `df_queue`, interim parquet (geo join) | `df_work` — 22,400 rows, 34 columns; zero geo nulls confirmed |
| DPI2-03-PREP-01 | Code | Feature engineering — D1 through D4 dimension scoring inputs. No composite scoring here — feature construction only. | `df_work` | D1 (signal breadth ratio), D2 (weighted high-concern signal score), D3 (peer-relative allowed per bene, capped at D3_CAP=5.0), D4 (signal persistence across State × RUCA subgroups; 801 neutral assignments for subgroups < MIN_SUBGROUP_SIZE=10) |
| DPI2-05-SCORE-00 | Markdown | Section header — composite scoring | None | None |
| DPI2-05-SCORE-01 | Code | Composite score calculation — weighted sum of D1 through D4. Weights: D1=0.25, D2=0.30, D3=0.25, D4=0.20. Range assertion and percentile table for threshold calibration. | `df_work` (D1–D4 columns) | `composite_score` column in `df_work`; range [0.2052, 0.8496]; mean=0.3035; median=0.2759; std=0.0837 |
| DPI2-07-VIZ-00 | Markdown | Section header — visualization | None | None |
| DPI2-07-VIZ-01 | Code | Composite score distribution histogram with percentile markers and provisional threshold markers. Natural breaks identified at 0.40 (TPE) and 0.50 (OIG). Calibrates THRESHOLD_OIG and THRESHOLD_TPE. | `df_work` (composite_score) | `outputs/figures/pathway_distribution_v1.png` (tag: D) |
| DPI2-06-PATHWAY-00 | Markdown | Section header — pathway assignment | None | None |
| DPI2-06-PATHWAY-01 | Code | Pathway assignment — routes each Tier 1 provider to OIG/DOJ Referral, TPE, or Provider Education Letter based on calibrated thresholds (THRESHOLD_OIG=0.50, THRESHOLD_TPE=0.40; tag: D). Pathway counts, score summary by pathway, and assertions. | `df_work` (composite_score) | `pathway` column in `df_work`; OIG: 655 (2.9%), TPE: 2,527 (11.3%), EdLetter: 19,218 (85.8%) |
| DPI2-08-OUTPUT-00 | Markdown | Section header — output | None | None |
| DPI2-08-OUTPUT-01 | Code | Write two parquet output files. Round-trip verification for both. | `df_work` | `data/processed/escalation_scored_v1.parquet` (34 cols, full audit trail); `data/processed/escalation_pathway_v1.parquet` (14 cols, lean operational file) |
| DPI2-09-EVIDENCE-00 | Markdown | Section header — evidence log | None | None |
| DPI2-09-EVIDENCE-01 | Code | Evidence log summary — O/D/I/A tagged outputs from this notebook. Mirrors entries in `decisions/D-PI-2_escalation_threshold/D-PI-2_escalation_log.md`. | `df_work` summary stats | Printed evidence log entries E-DPI2-01 through E-DPI2-06 |

---

## Diagnostic Cell — Remove Before Archive

| Cell | Status | Action Required |
|---|---|---|
| Cell 20 — `# DIAGNOSTIC — D3_CAP review` | Present in working notebook | Remove before final GitHub push or archive |

---

## Input / Output Summary

### Inputs (from D-PI-1 processed outputs)

| File | Used In |
|---|---|
| `data/processed/provider_review_queue_v1.parquet` | DPI2-01-LOAD-01 |
| `data/processed/provider_signal_scores_v1.parquet` | DPI2-01-LOAD-01 |
| `data/processed/provider_tiered_v1.parquet` | DPI2-01-LOAD-01 |
| `data/processed/provider_rollup_v1.parquet` | DPI2-01-LOAD-01 |
| Interim parquet (geo source — `Rndrng_Prvdr_State_Abrvtn`, `Rndrng_Prvdr_RUCA`) | DPI2-03-PREP-00a |

### Outputs

| File | Location | Tag | Produced In |
|---|---|---|---|
| `escalation_scored_v1.parquet` | `data/processed/` | D | DPI2-08-OUTPUT-01 |
| `escalation_pathway_v1.parquet` | `data/processed/` | D | DPI2-08-OUTPUT-01 |
| `pathway_distribution_v1.png` | `outputs/figures/` | D | DPI2-07-VIZ-01 |

---

## Cell Numbering Note

DPI2-06-PATHWAY-01 (pathway assignment) appears after DPI2-07-VIZ-01
(visualization) in notebook execution order. Cell IDs reflect logical
module numbering established at design time; execution order in the
notebook differs. This is intentional — visualization calibrates thresholds
before pathway assignment executes.

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
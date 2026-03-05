# Notebook Map — D-EXT-1 Safe Reporting

**Version:** 1.0
**Date:** March 2026
**Notebook:** notebooks/decision/D-EXT-1_safe_reporting.ipynb

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This map documents every cell in the D-EXT-1 notebook — its ID, type,
purpose, inputs, and outputs. It serves as the authoritative reference
for notebook structure and cell-level traceability.

---

## Cell Inventory

| Cell ID | Type | Section | Purpose |
|---|---|---|---|
| DEXT1-00-HEADER-00 | Markdown | Header | Module title, purpose, inputs, outputs, evidence standard |
| DEXT1-00-CONFIG-01 | Code | Config | All parameters — paths, filenames, allowlist, palette, random state |
| DEXT1-01-LOAD-01 | Code | Load | Load all four input parquet files, validate shapes and column names |
| DEXT1-02-VALIDATE-01 | Markdown | Validate | Column format discovery — Rule 14 documentation |
| DEXT1-03-PREP-01 | Code | Prep | Filter to external pathways, merge monitoring level, cross-tab validation |
| DEXT1-04-SAFE-01 | Code | Safe | Apply safe column allowlist, append disclaimer and evidence tag columns |
| DEXT1-04-SAFE-02 | Code | Safe | Build aggregate summary by pathway x monitoring level |
| DEXT1-05-TIP-01 | Markdown | Tip | Tip intake cross-reference infrastructure note and integration pattern |
| DEXT1-05-TIP-02 | Code | Tip | cross_reference_tip() function — validated on 3 synthetic test cases |
| DEXT1-06-VIZ-01 | Code | Viz | Two-panel chart — provider count and allowed dollars by pathway x level |
| DEXT1-07-OUTPUT-01 | Code | Output | Write and round-trip verify both output parquet files |
| DEXT1-08-EVIDENCE-01a | Code | Evidence | Evidence summary entries E-DEXT1-01 through E-DEXT1-04 |
| DEXT1-08-EVIDENCE-01b | Code | Evidence | Evidence summary entries E-DEXT1-05 through E-DEXT1-07 |

---

## Input / Output Table

| File | Direction | Cell | Description |
|---|---|---|---|
| escalation_pathway_v1.parquet | Input | DEXT1-01-LOAD-01 | D-PI-2 pathway assignments — 22,400 Tier 1 providers |
| escalation_scored_v1.parquet | Input | DEXT1-01-LOAD-01 | D-PI-2 composite scores — 22,400 Tier 1 providers |
| monitoring_roster_v1.parquet | Input | DEXT1-01-LOAD-01 | D-OPS-1 monitoring intensity levels — 61,416 providers |
| resource_allocation_v1.parquet | Input | DEXT1-01-LOAD-01 | D-OPS-2 resource allocation outputs — 61,416 providers |
| safe_reporting_providers_v1.parquet | Output | DEXT1-07-OUTPUT-01 | NPI-level safe output file — 3,182 providers | 9 columns |
| safe_reporting_summary_v1.parquet | Output | DEXT1-07-OUTPUT-01 | Aggregate summary by pathway x monitoring level — 5 rows | 10 columns |
| safe_reporting_summary_v1.png | Output | DEXT1-06-VIZ-01 | Two-panel summary chart — CVD-compliant |

---

## Key Findings

| Finding | Value | Tag |
|---|---|---|
| Total external reporting population | 3,182 providers | O |
| OIG/DOJ Referral providers | 655 | O |
| TPE providers | 2,527 | O |
| Total allowed dollars in scope | $798.55M | D |
| OIG/DOJ L1 Active — provider count | 606 (19.0%) | D |
| OIG/DOJ L1 Active — allowed dollars | $363.69M (45.5%) | D |
| TPE L2 Periodic — provider count | 1,564 (49.2%) | D |
| TPE L2 Periodic — allowed dollars | $255.40M (32.0%) | D |
| OIG providers in Level 2 — flagged | 49 | D |
| Tip cross-reference function | Validated — 3 test cases | A |

---

## Column Format Discoveries — Rule 14

| Assumed | Actual | Source | Documented In |
|---|---|---|---|
| `npi` | `Rndrng_NPI` | df_pathway, df_monitoring | DEXT1-02-VALIDATE-01 |
| `provider_type` | `Rndrng_Prvdr_Type` | df_pathway, df_monitoring | DEXT1-02-VALIDATE-01 |
| `state_cd` | `Rndrng_Prvdr_State_Abrvtn` | df_pathway | DEXT1-02-VALIDATE-01 |
| `tot_allowed_amt` | `allowed_dollars_sum` | df_pathway, df_monitoring | DEXT1-02-VALIDATE-01 |
| `"OIG Referral"` | `"OIG/DOJ Referral"` | df_pathway | DEXT1-02-VALIDATE-01 |
| `"TPE"` | `"Targeted Probe and Educate"` | df_pathway | DEXT1-02-VALIDATE-01 |

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, RY25/D23.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
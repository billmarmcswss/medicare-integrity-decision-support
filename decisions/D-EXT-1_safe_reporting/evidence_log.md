# Evidence Log — D-EXT-1 Safe Reporting

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
D-EXT-1 Safe Reporting. All outputs are tagged using the project
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

| Evidence ID | Source | Notebook Cell | Result | Decision Relevance | Tag | Status |
|---|---|---|---|---|---|---|
| E-DEXT1-01 | df_ext — filtered from df_pathway | DEXT1-03-PREP-01 | 3,182 providers in external pathways — OIG/DOJ Referral: 655 | TPE: 2,527 | Foundation dataset for all D-EXT-1 safe output construction | O | Accepted |
| E-DEXT1-02 | df_work — df_ext merged with monitoring_level from df_monitoring | DEXT1-03-PREP-01 | 3,182 rows | 15 columns | 0 unmatched — OIG L1: 606 | OIG L2: 49 | OIG L3: 0 | TPE L1: 556 | TPE L2: 1,564 | TPE L3: 407 | Pathway x monitoring level distribution for safe output file and summary | D | Accepted |
| E-DEXT1-03 | df_safe — safe column allowlist applied to df_work | DEXT1-04-SAFE-01 | 3,182 rows | 9 columns | 0 nulls — Columns: Rndrng_NPI, Rndrng_Prvdr_Type, Rndrng_Prvdr_State_Abrvtn, pathway, monitoring_level, allowed_dollars_sum, composite_score, disclaimer, evidence_tag | Safe provider-level output file for external reporting | D | Accepted |
| E-DEXT1-04 | df_summary — aggregate by pathway x monitoring level | DEXT1-04-SAFE-02 | 5 rows | 10 columns — Total providers: 3,182 | Total dollars: $798.55M | OIG/DOJ L1: 606 providers | $363.69M (45.5% of dollars) | TPE L2: 1,564 providers | $255.40M (32.0% of dollars) | Aggregate summary for external reporting and recommendation memo | D | Accepted |
| E-DEXT1-05 | cross_reference_tip() — validated on 3 synthetic test cases | DEXT1-05-TIP-02 | OIG match: NPI 1215003603 | Level 1 Active | score 0.6190 | $93,947,495.11 — TPE match: NPI 1407855240 | Level 2 Periodic | score 0.4082 | $69,171,750.22 — Unmatched: routed to D-PI-1 intake | Tip intake cross-reference infrastructure validated — no live tip data in RY25/D23 cycle | A | Accepted |
| E-DEXT1-06 | safe_reporting_summary_v1.png | DEXT1-06-VIZ-01 | Two-panel chart produced — provider count and allowed dollars by pathway x monitoring level | CVD-compliant IBM Carbon palette | Visual summary for recommendation memo and Decision Owner briefing | D | Accepted |
| E-DEXT1-07 | safe_reporting_providers_v1.parquet — safe_reporting_summary_v1.parquet | DEXT1-07-OUTPUT-01 | Both files written and round-trip verified — Providers file: 3,182 rows | 9 columns | Summary file: 5 rows | 10 columns | All assertions passed | Persistent outputs for recommendation memo and external coordination use | D | Accepted |

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, RY25/D23.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
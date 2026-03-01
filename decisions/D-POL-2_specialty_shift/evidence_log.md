# Evidence Log — D-POL-2 Specialty Pattern Shift

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
D-POL-2 support infrastructure. All outputs are tagged using the project
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
| E-DPOL2-01 | df_specialty — flagged providers with specialty assignments and full population denominators | D-POL-2_specialty_shift.ipynb (DPOL2-03-PREP-01) | 3,182 providers | 74 unique Provider Types | 0 null types | 0 null pathway assignments | Foundation dataset for all Track 1 dimension scoring | D | Accepted |
| E-DPOL2-02 | specialty_flagged — S1 Specialty Anomaly Concentration | D-POL-2_specialty_shift.ipynb (DPOL2-04-SCORE-01) | 74 specialties scored | 32 neutral (flagged count < 10) | 9 specialties flagged | Threshold 0.0075 derived from observed distribution | Top: Hospitalist (0.0136 flagged share) | Identifies specialties with disproportionate provider concentration among anomalous billing patterns | D | Accepted |
| E-DPOL2-03 | pathway_wide — S2 Pathway Stratification by Specialty | D-POL-2_specialty_shift.ipynb (DPOL2-04-SCORE-02) | 74 specialties scored | 32 neutral (total flagged < 10) | 11 specialties flagged | Threshold 0.35 derived from observed distribution | Top reliable specialty: Pathology (0.625 OIG share) | Identifies specialties with disproportionate OIG/DOJ r
# Decision Log — D-EXT-1 Safe Reporting

**Version:** 1.0
**Date:** March 2026

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This log records analytical and design decisions made during the development
of D-EXT-1 Safe Reporting. All decisions are traceable to notebook cells
and evidence entries.

No outputs constitute findings of fraud, waste, or abuse.
All outputs describe statistical patterns only.

---

## Decision Log Entries

| ID | Decision | Rationale | Cell | Tag |
|---|---|---|---|---|
| DL-DEXT1-01 | Scope limited to OIG/DOJ Referral (655) and TPE (2,527) providers only | Education Letter providers carry insufficient escalation signal for external reporting context; full monitoring roster (61,416) not appropriate for external output | DEXT1-03-PREP-01 | I |
| DL-DEXT1-02 | Safe column allowlist defined with 7 columns only | Limits external exposure to publicly available PUF fields and high-level routing signals; raw dimension scores (D1–D4) excluded to prevent misinterpretation of sub-score detail by external recipients | DEXT1-00-CONFIG-01 | I |
| DL-DEXT1-03 | Disclaimer appended as a column to every row in safe output file | Ensures downstream recipients cannot strip context from the data; disclaimer travels with the record regardless of how the file is sliced or filtered | DEXT1-04-SAFE-01 | I |
| DL-DEXT1-04 | Evidence tag appended as a column to every row | Maintains O/D/I/A tagging discipline at the record level for external recipients unfamiliar with project tagging conventions | DEXT1-04-SAFE-01 | I |
| DL-DEXT1-05 | Tip intake cross-reference implemented as placeholder infrastructure | No live tip data exists in RY25/D23 cycle; framework documents the integration pattern for operational use when tips are received; function validated on synthetic test cases | DEXT1-05-TIP-02 | A |
| DL-DEXT1-06 | Unmatched tip NPIs routed to D-PI-1 intake | Providers not in the external reporting population may still warrant program integrity review; D-PI-1 is the correct entry point for new provider signals | DEXT1-05-TIP-02 | I |
| DL-DEXT1-07 | Column name corrections applied before any filter logic | DEXT1-01-LOAD-01 diagnostic confirmed actual column names differ from CONFIG assumptions; corrections documented per Rule 14 before any downstream logic written | DEXT1-02-VALIDATE-01 | O |
| DL-DEXT1-08 | Pathway label corrections applied before any filter logic | Actual labels confirmed as 'OIG/DOJ Referral' and 'Targeted Probe and Educate' — not 'OIG Referral' and 'TPE' as initially assumed; corrected in CONFIG before filter logic | DEXT1-02-VALIDATE-01 | O |
| DL-DEXT1-09 | Evidence log cell split into two cells (01a and 01b) | Jupyter default output truncation prevented full display of all seven evidence entries in a single cell; split resolves display issue without affecting analytical content | DEXT1-08-EVIDENCE-01a/b | O |
| DL-DEXT1-10 | 49 OIG providers assigned to Level 2 — flagged for Decision Owner awareness | Expected profile places all OIG providers in Level 1; 49 scoring into Level 2 is consistent with D-OPS-1 finding and documented for Compliance Officer awareness — not suppressed | DEXT1-03-PREP-01 | I |

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, RY25/D23.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
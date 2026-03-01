# Decision Log — D-OPS-1 Monitoring Placement

**Version:** 1.0
**Date:** March 2026

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This log records analytical and architectural decisions made during the
development of D-OPS-1 support infrastructure. It captures what was decided,
why, what evidence was used, and whether any decision was later reversed.

This is not a log of provider-level findings.
It is a log of analytical design decisions.

---

## Entries

| ID | Date | Decision Made | Evidence Used | Confidence | Reversed? | Notes |
|---|---|---|---|---|---|---|
| DL-DOPS1-01 | 2026-03 | Monitoring roster scope set to Tier 1 + Tier 2 providers | Tier 2 providers have two signals flagged and warrant passive surveillance; operational continuity requires tracking beyond Tier 1 only | High | No | Tier 3 and No Flag providers excluded — anomaly signal insufficient for roster placement |
| DL-DOPS1-02 | 2026-03 | Three dimensions selected: M1 Escalation Pathway Weight, M2 Composite Escalation Score Carry-Forward, M3 Code-Level Scrutiny Intensity | Dimensions capture pathway severity, multi-dimensional risk from D-PI-2, and code-level specificity independently | High | No | No re-derivation of upstream signals — carry-forward design preserves analytical continuity |
| DL-DOPS1-03 | 2026-03 | Dimension weights set: M1=0.40, M2=0.35, M3=0.25 | M1 highest weight reflects pathway assignment as primary operational signal; M2 carry-forward captures full D-PI-2 composite; M3 adds code-level specificity | High | No | Weights sum to 1.0; asserted in CONFIG |
| DL-DOPS1-04 | 2026-03 | M1 pathway scores set: OIG=1.00, TPE=0.67, EdLetter=0.33, Tier2Only=0.10 | Ordinal scoring reflects CMS/UPIC enforcement severity hierarchy; Tier 2 score of 0.10 retains presence in composite without inflating above any escalated provider | High | No | Tag D; scores parameterized in M1_SCORES dict |
| DL-DOPS1-05 | 2026-03 | NEUTRAL_SCORE set to 0.50 for out-of-scope dimension assignments | Avoids zero-weighting providers lacking data for a dimension; 0.50 places them at mid-range without inflating or deflating composite score | High | No | Applied to Tier 2 M2 scores and all out-of-scope M3 assignments; tag A |
| DL-DOPS1-06 | 2026-03 | anomaly_tier column contains string labels not integers | DOPS1-02-VALIDATE-01 diagnostic confirmed values are Tier1_Review, Tier2_Monitor, Tier3_Watch, No_Flag | High | No | All tier references use string label form; documented in DOPS1-03-PREP-00 markdown cell; to be added to PROJECT_STANDARDS.md |
| DL-DOPS1-07 | 2026-03 | DPOL1_MAX_DIMS set to 3 | C1 excluded from active D-POL-1 dimensions per DL-DPOL1-06; three active dimensions are C2, C3, C4 | High | No | Inherited from D-POL-1 design; parameterized in CONFIG |
| DL-DOPS1-08 | 2026-03 | THRESHOLD_L1 set to 0.58, THRESHOLD_L2 set to 0.41 | DOPS1-07-DIAG-01: natural breaks confirmed — OIG min=0.5751, TPE min=0.4080, EdLetter max=0.3970; clean gaps between all three pathway populations | High | No | Tag D; replaces provisional values of 0.70 and 0.45; no Education Letter provider crosses L2 |
| DL-DOPS1-09 | 2026-03 | 49 OIG providers assigned Level 2 Periodic Review | Composite monitoring score below THRESHOLD_L1=0.58 despite OIG pathway assignment; reflects broader composite weighting rather than pathway alone; analytically defensible | High | No | Decision Owner awareness flagged in recommendation memo; not a misrouting |
| DL-DOPS1-10 | 2026-03 | 556 TPE providers assigned Level 1 Active Surveillance | Composite monitoring score at or above THRESHOLD_L1=0.58 on own merits; TPE spillover into L1 confirmed in DOPS1-07-VIZ-01 and DOPS1-07-DIAG-01 | High | No | Analytically justified — composite score driven by M1, M2, M3 independently of pathway label |
| DL-DOPS1-11 | 2026-03 | PERCENTILE_LADDER and PRINT_WIDTH added to CONFIG | Percentile reference points and display width are project parameters not inline literals; consistent with no-hardcoded-variables standard | High | No | PRINT_WIDTH=70; PERCENTILE_LADDER=[50,60,70,75,80,85,90,95,99] |

---

## Reversal Protocol

If any decision above is reversed in a future session:

1. Update the Reversed? field to Yes
2. Add a new entry documenting the replacement decision
3. Update the evidence log with new supporting evidence
4. Update notebook_map.md if notebook dependencies change
5. Re-run affected notebook cells in dependency order

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All decisions documented here reflect analytical design choices only.
No entries constitute findings of fraud, waste, or abuse.
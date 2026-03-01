# Decision Log — D-OPS-2 Resource Allocation

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
development of D-OPS-2 support infrastructure. It captures what was decided,
why, what evidence was used, and whether any decision was later reversed.

This is not a log of provider-level findings.
It is a log of analytical design decisions.

---

## Entries

| ID | Date | Decision Made | Evidence Used | Confidence | Reversed? | Notes |
|---|---|---|---|---|---|---|
| DL-DOPS2-01 | 2026-03 | Allocation target set to level × pathway simultaneously — nine-cell structure | D-OPS-1 roster contains both monitoring_level and pathway columns; pathway composition within levels is operationally meaningful (556 TPE-spillover in L1; 49 OIG in L2 per DL-DOPS1-09 and DL-DOPS1-10) | High | No | Single-dimension allocation by level alone would obscure pathway-driven operational distinctions within each level |
| DL-DOPS2-02 | 2026-03 | Four denominator views selected: Review Hours, FTE Capacity, Case Slots, Abstract Capacity Units | Each denominator serves a distinct Decision Owner operational context; presenting all four maximizes framework portability and explicitly surfaces assumption dependencies | High | No | Views A–C tagged (A) due to operational assumption requirements; View D tagged (D) — assumption-free |
| DL-DOPS2-03 | 2026-03 | Two-stage architecture adopted: Stage 1 denominator-agnostic core score; Stage 2 four view translations | Single core engine ensures all four views rank cells consistently; avoids conflicting priority signals across views | High | No | Core composite score is the authoritative cell ranking; denominator views translate ranking into operational units only |
| DL-DOPS2-04 | 2026-03 | Three dimensions selected: R1 Monitoring Intensity Weight, R2 Provider Count Concentration, R3 Dollar Exposure Concentration | R1 captures enforcement severity hierarchy; R2 captures operational volume burden; R3 captures financial stakes; three dimensions are independent and non-redundant | High | No | No carry-forward of D-OPS-1 composite score as a dimension — monitoring_level already encodes that signal via R1 |
| DL-DOPS2-05 | 2026-03 | Default dimension weights set: R1=0.50, R2=0.25, R3=0.25 | R1 weighted highest — monitoring intensity is the primary enforcement signal and the only fully data-derived dimension; R2 and R3 split equally as secondary volume and financial dimensions | High | No | Weights sum to 1.0; parameterized in CONFIG; sensitivity analysis required across alternative weight assumptions |
| DL-DOPS2-06 | 2026-03 | R1 scoring set: Level 1 = 1.00, Level 2 = 0.67, Level 3 = 0.33 | Ordinal scoring mirrors M1 pathway weight structure from D-OPS-1 (OIG=1.00, TPE=0.67, EdLetter=0.33) — maintains internal consistency across modules | High | No | Tag D; parameterized in R1_SCORES dict |
| DL-DOPS2-07 | 2026-03 | R2 normalization: cell_count / max_cell_count across all nine cells | Relative normalization preserves cell-to-cell comparison without distortion from absolute population size differences; consistent with normalization approach used in D-PI-2 and D-OPS-1 | High | No | Tag D |
| DL-DOPS2-08 | 2026-03 | R3 normalization: cell_allowed / max_cell_allowed across all nine cells | Same relative normalization rationale as R2; dollar exposure is independently meaningful from provider count | High | No | Tag D |
| DL-DOPS2-09 | 2026-03 | View A default hours assumptions: HOURS_L1=8.0, HOURS_L2=4.0, HOURS_L3=0.5 | No empirical hours data available in CMS PUF; assumptions reflect ordinal intensity hierarchy — L1 requires active investigative engagement, L2 periodic structured review, L3 passive tracking only | Medium | No | Tag A; Decision Owner should substitute operational actuals; parameterized in CONFIG |
| DL-DOPS2-10 | 2026-03 | View B default: ANNUAL_PRODUCTIVE_HOURS=1,800 | Standard federal workforce productive hours assumption (2,080 gross minus leave and overhead); widely used in government capacity modeling | Medium | No | Tag A; parameterized in CONFIG |
| DL-DOPS2-11 | 2026-03 | View C default case slot rates: L1=1.00, L2=0.50, L3=0.05; UPIC_TOTAL_CASE_CAPACITY=2,500 | L1 rate of 1.00 reflects every active surveillance provider generating a formal case action; L2 rate of 0.50 reflects TPE review cycle throughput constraints; L3 rate of 0.05 reflects passive surveillance — only sustained anomaly pattern triggers formal action | Medium | No | Tag A; UPIC_TOTAL_CASE_CAPACITY reflects approximate UPIC operational throughput — no public benchmark available; parameterized in CONFIG |
| DL-DOPS2-12 | 2026-03 | View D ACU formula: (cell_composite_score / sum_all_composite_scores) × 100 | Normalization to 100 produces interpretable percentage shares without operational assumptions; fully portable for external users | High | No | Tag D; recommended primary view for external or non-operational stakeholders |
| DL-DOPS2-13 | 2026-03 | Decision Notes in recommendation memo tagged (I) | Notes state recommended allocations based on analytical outputs — inference from data, not direct observation or derivation | High | No | Each of the four views receives one Decision Note; all four note the assumption dependency and recommend Decision Owner validation |
| DL-DOPS2-14 | 2026-03 | Single input file: monitoring_roster_v1.parquet only | All required fields — monitoring_level, pathway, allowed dollars, NPI — are present in D-OPS-1 output; no additional upstream joins required | High | No | Simplifies data load and eliminates join-related data quality risks |

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
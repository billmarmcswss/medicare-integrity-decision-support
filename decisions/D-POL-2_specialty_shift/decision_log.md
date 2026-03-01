# Decision Log — D-POL-2 Specialty Pattern Shift

**Version:** 1.0
**Date:** February 2026

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This log records analytical and architectural decisions made during the
development of D-POL-2 support infrastructure. It captures what was decided,
why, what evidence was used, and whether any decision was later reversed.

This is not a log of provider-level findings.
It is a log of analytical design decisions.

---

## Entries

| ID | Date | Decision Made | Evidence Used | Confidence | Reversed? | Notes |
|---|---|---|---|---|---|---|
| DL-DPOL2-01 | 2026-02 | Two analytical tracks selected — Track 1 Cross-Specialty Pattern Comparison and Track 2 Intra-Specialty Temporal Shift Framework | Track 1 fully executable from D-POL-1 outputs; Track 2 framework-complete but data-constrained at single year | High | No | Track 1 tag D; Track 2 tag A — resolves to D when Data Year 2024+ available |
| DL-DPOL2-02 | 2026-02 | Four dimensions selected: S1 Specialty Anomaly Concentration, S2 Pathway Stratification, S3 Specialty Norm Baseline, S4 Shift Detection Threshold Design | S1/S2 Track 1 tag D; S3/S4 Track 2 tag A — data-constrained | High | No | S3/S4 resolve to D when multi-year data available |
| DL-DPOL2-03 | 2026-02 | Input scope set to OIG + TPE combined (3,182 providers) | Inherits DL-DPOL1-01 scope decision; EdLetter providers excluded — specialty scrutiny not warranted at that pathway | High | No | Consistent with D-POL-1 input scope |
| DL-DPOL2-04 | 2026-02 | Specialty population denominator sourced from provider_tiered_v1.parquet full population | Full population denominators required for reliable concentration ratios; flagged subset would produce inflated shares | High | No | 970,848 providers used as denominator base |
| DL-DPOL2-05 | 2026-02 | S1_CONCENTRATION_THRESHOLD set to 0.0075 | Natural break identified in full specialty distribution diagnostic — separates top 9 specialties from broader distribution; specialties above threshold have roughly 1 in 130 or more providers flagged | High | No | Tag D — derived from observed distribution; replaces provisional value of 0.20 |
| DL-DPOL2-06 | 2026-02 | S2_PATHWAY_RATIO_THRESHOLD set to 0.35 | Natural break identified in reliable specialty OIG share distribution diagnostic — separates specialties where more than 1 in 3 flagged providers routes to OIG from broader distribution | High | No | Tag D — derived from observed distribution; replaces provisional value of 0.30 |
| DL-DPOL2-07 | 2026-02 | Track 2 temporal outputs tagged Assumed (A) throughout | Single data year available; temporal shift detection requires minimum two data years; Data Year 2023 establishes baseline only | High | No | Tag resolves to D when Data Year 2024+ incorporated; all parameters and detection logic fully documented |
| DL-DPOL2-08 | 2026-02 | Specialty baseline metrics selected: median_allowed_per_bene, median_services_per_bene, median_top1_share | Three metrics capture payment intensity, utilization frequency, and code concentration independently; consistent with D-PI-1 signal dimensions S1, S3, S5a | High | No | Tag A for Track 2; sourced from provider_rollup_v1.parquet |
| DL-DPOL2-09 | 2026-02 | S4_SHIFT_MAGNITUDE_THRESHOLD set provisionally at 0.20 | A 20% shift in specialty baseline metric between data years represents a meaningful pattern change warranting policy review; recalibrate when Data Year 2024+ available | Medium | Pending review | PROVISIONAL — tag A — data-constrained; cannot resolve until multi-year data available |
| DL-DPOL2-10 | 2026-02 | TOP_N_DISPLAY set to 20 | Consistent with D-POL-1 TOP_N_DISPLAY=20; provides actionable policy target list without overwhelming visualization or memo | Medium | No | Tag A; adjustable in CONFIG |

---

## Open Provisionals

| ID | Parameter | Status |
|---|---|---|
| DL-DPOL2-09 | S4_SHIFT_MAGNITUDE_THRESHOLD | Data-constrained — resolves when Data Year 2024+ available |

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
# Decision Recommendation Memo
## D-PI-1 — Provider Review Entry

**Date:** February 20, 2026
**Version:** 1.0
**Status:** Draft for Review

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B Public Use File,
> Reporting Year 2025, Data Year 2023 (RY25/D23).
> Readers should verify current organizational alignment independently.

---

## To

Center for Program Integrity (CPI)
Investigations Lead / Program Integrity Analytics Manager

## From

Medicare Program Integrity Decision Analytics
Decision Support System — Build 1, Phase 1

## Re

Provider Review Queue Recommendation — Medicare Part B, Data Year 2023

---

## Purpose

This memo presents a structured, evidence-based recommendation identifying
which providers should enter the program integrity review queue for the
current review cycle.

This recommendation is derived from peer-adjusted statistical analysis of
CMS Medicare Part B Provider and Service data. It does not constitute a
finding of fraud, waste, or abuse. All signals describe anomalous billing
patterns relative to peer providers.

---

## Decision Supported

**D-PI-1 — Provider Review Entry**

Which providers should enter the program integrity review queue based on
defensible, evidence-supported signals of anomalous utilization or billing
patterns relative to peer providers?

---

## Analytical Foundation

**Dataset:**
CMS Medicare Part B Public Use File — Provider and Service Level
`MUP_PHY_R25_P05_V20_D23_Prov_Svc.csv`
Reporting Year 2025 / Data Year 2023

**Providers evaluated:** 970,848
(After minimum volume filter: Tot_Benes >= 50)

**Signals computed:** 8 independent peer-adjusted signals covering:
- Utilization intensity (S3)
- Payment intensity (S1, S2)
- Pricing behavior (S4)
- Code concentration (S5a, S5b)
- Allowed vs payment gap (S6, S7)

**Peer grouping:** Provider Type (primary)
Median peer group size: 2,584 providers

---

## Parameters Applied (A — Assumed)

| Parameter | Value | Rationale |
|---|---|---|
| Z-Score Threshold | 2.0 | Standard statistical deviation boundary |
| Minimum Signals | 3 | Multi-signal agreement reduces false positives |
| Queue Size | 500 | Reflects operational review capacity |

These parameters are adjustable. See sensitivity analysis for
impact of alternative threshold combinations.

---

## Recommendation (I — Inferred)

**Initiate focused program integrity review for the top 500 providers
in the Tier 1 review queue.**

| Metric | Value |
|---|---|
| Providers recommended for review | 500 |
| Total allowed dollars represented | $1,770,070,918 |
| As percentage of all allowed dollars | 2.6% |
| As percentage of provider population | 0.05% |

Reviewing 0.05% of the evaluated provider population covers
2.6% of total Medicare Part B allowed dollars flagged by
three or more independent peer-adjusted anomaly signals.

---

## Tier Distribution Summary (D — Derived)

| Tier | Providers | Recommended Action |
|---|---|---|
| Tier 1 — Review | 22,400 | Focused medical review or audit |
| Tier 2 — Monitor | 39,016 | Active monitoring |
| Tier 3 — Watch | 68,721 | Passive surveillance |
| No Flag | 840,711 | No action warranted |
| **Total evaluated** | **970,848** | |

---

## Extreme Signal Diagnostic Summary (U5 Resolution)

Prior to finalizing this recommendation, providers with Z-scores
exceeding 8.0 standard deviations were reviewed to assess whether
extreme values reflected genuine patterns or data artifacts.

**Findings (I):**
- 20 providers identified with max Z-score >= 8.0
- All 20 had peer group sizes exceeding 14,000 providers
- Peer group size confirmed not the cause of inflation
- 17 of 20 flagged on only one signal — excluded from Tier 1
  by MIN_SIGNALS=3 requirement
- 3 of 20 flagged on 3+ signals — included in Tier 1 queue
- Dominant pattern: extreme services per beneficiary (S3)

**Conclusion (I):**
Extreme Z-scores reflect genuine statistical deviation within
large stable peer groups. The multi-signal agreement requirement
provides adequate protection against acting on single-signal
extremes. No data artifact or suppression issue identified.

---

## Top 5 Providers by Allowed Dollars (D — Derived)

| Rank | NPI | Provider Type | Allowed Dollars | Signals Flagged |
|---|---|---|---|---|
| 1 | 1427320399 | Independent Diagnostic Testing Facility | $132,129,700 | 5 |
| 2 | 1215003603 | Clinical Laboratory | $93,947,500 | 4 |
| 3 | 1013973866 | Clinical Laboratory | $91,941,960 | 3 |
| 4 | 1770833121 | Clinical Laboratory | $75,218,850 | 3 |
| 5 | 1407855240 | Ambulance Service Provider | $69,171,750 | 4 |

---

## Sensitivity Analysis Summary (D — Derived)

The recommendation is sensitive to parameter choices.
Key findings from sensitivity analysis:

- Loosening to Z=1.5, Min Signals=2, Queue=1,000 captures $3.922B
  but requires reviewing 1,000 providers
- Tightening to Z=3.0, Min Signals=4, Queue=500 drops to $0.367B
  with only 1,076 Tier 1 providers total
- Current parameters represent a defensible middle ground
  balancing detection with operational capacity

Full sensitivity table available in notebook D1-DPI1-SENSITIVITY-01.

---

## Uncertainties Remaining (U — Unresolved)

| ID | Uncertainty | Impact |
|---|---|---|
| U2 | Patient mix not risk-adjusted | May affect false positive rate |
| U4 | Thresholds not yet operationally calibrated | Queue size assumption |
| U5 | Resolved — see diagnostic summary above | None remaining |

---

## Recommended Next Actions

1. Review top 500 providers against operational capacity
2. Calibrate QUEUE_SIZE to actual review throughput per cycle
3. Conduct quarterly threshold recalibration review
4. Escalate providers with 5+ signals to priority investigation
5. Apply secondary geographic controls for regional pattern analysis

---

## Output Tagging Standard

| Tag | Meaning |
|---|---|
| O | Observed — directly in the data |
| D | Derived — mathematically constructed |
| I | Inferred — evidence-based interpretation |
| A | Assumed — declared analytical assumption |

---

## Governance Footnote

This recommendation reflects decision framing established February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
Provider NPIs included for analytical traceability only.
No personal or protected health information is contained herein.

# Decision Log — D-PI-1 Provider Review Entry

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
development of D-PI-1 support infrastructure. It captures what was decided,
why, what evidence was used, and whether any decision was later reversed.

This is not a log of provider-level findings.
It is a log of analytical design decisions.

---

## Log Format

| ID | Date | Decision Made | Evidence Used | Confidence | Reversed? | Notes |
|---|---|---|---|---|---|---|

---

## Entries

| ID | Date | Decision Made | Evidence Used | Confidence | Reversed? | Notes |
|---|---|---|---|---|---|---|
| DL-DPI1-01 | 2026-02 | Primary peer grouping set to Provider Type only | DPI1-01-DIAG-01 through DIAG-03: Type-only p10=48, median=2584 vs fragmented results at Type x State | High | No | State-level grouping caused over-fragmentation; RUCA retained as secondary diagnostic control |
| DL-DPI1-02 | 2026-02 | Minimum volume filter set to Tot_Benes >= 11 | CMS PUF suppression rules suppress records below this threshold prior to publication | High | No | Filter confirmed zero exclusions in current dataset; retained for future data releases |
| DL-DPI1-03 | 2026-02 | Z-score threshold set to 2.0 for signal flagging | Standard statistical threshold for anomaly detection; calibration review scheduled quarterly | Medium | Pending review | Tier thresholds subject to operational capacity calibration in decision notebook |
| DL-DPI1-04 | 2026-02 | Tier 1 defined as >= 3 signals flagged | Multi-signal agreement reduces false positive risk; single-signal flags insufficient for review prioritization | High | No | Tier 2 = 2 signals, Tier 3 = 1 signal |
| DL-DPI1-05 | 2026-02 | Tier 1 prioritization ranked by allowed dollars | Highest exposure providers represent greatest financial risk to Medicare Trust Fund | High | No | Payment dollars used as secondary sort where allowed dollars are equal |
| DL-DPI1-06 | 2026-02 | 8 signals selected for v1 signal engine | Covers utilization, payment intensity, pricing behavior, and coding pattern dimensions independently | High | No | S1-S7 plus S5a/S5b; additional signals deferred to v2 pending decision notebook calibration |

---

## Reversal Protocol

If any decision above is reversed in a future session:

1. Update the Reversed? field to Yes
2. Add a new entry documenting the replacement decision
3. Update the evidence log with new supporting evidence
4. Update notebook_map.md if notebook dependencies change
5. Re-run affected notebooks in dependency order

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All decisions documented here reflect analytical design choices only.
No entries constitute findings of fraud, waste, or abuse.
| DL-DPI1-07 | 2026-02 | CORE_Z_THRESHOLD fixed at 2.0 in core signal engine | Shared infrastructure must produce statistically standard baseline used by all decision notebooks A-E; parameterized control lives in decision notebooks only | High | No | Documented in DPI1-02-TIER-01 hardcoding notice; change requires re-run of all downstream decision notebooks |
| DL-DPI1-08 | 2026-02 | Tier boundary values (3,2,1) fixed in core signal engine | Core engine produces baseline tier structure only; operational threshold control (MIN_SIGNALS) lives exclusively in decision notebooks to prevent analytic bias leaking into shared infrastructure | High | No | Documented in DPI1-02-TIER-01 hardcoding notice; aligns with no-hardcoding standard by isolating fixed values with explicit rationale |
| DL-DPI1-09 | 2026-02 | EXTREME_Z_THRESHOLD fixed at 8.0 in D1-DPI1-DIAG-01 | Diagnostic boundary only; 4x standard flagging threshold (2.0); recalibrate if data composition changes materially | Medium | No | Named variable with hardcoding notice; not an operational threshold |
| DL-DPI1-10 | 2026-02 | MIN_BENES recalibrated from 11 to 50 | Diagnostic D1-DPI1-DIAG-01 revealed providers with Tot_Benes 11-25 producing mathematically extreme Z-scores due to unstable ratios at low volume; peer group size confirmed not the cause | High | No | Raises minimum beneficiary volume filter; requires rerun of DPI1-02-OUTLIER-01 through D1-DPI1-VIZ-01 |

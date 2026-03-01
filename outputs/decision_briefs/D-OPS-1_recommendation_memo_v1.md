# D-OPS-1 Monitoring Placement — Recommendation Memo

**To:** UPIC Operations Manager / CPI Monitoring Unit Lead
**From:** Medicare Integrity Decision Support Framework
**Date:** March 2026
**Re:** Monitoring Roster Placement — Data Year 2023

---

## Legal and Use Disclaimer

This memo describes statistical patterns in publicly available CMS Medicare
Part B billing data only. No finding in this memo constitutes evidence of
fraud, waste, or abuse. No output asserts provider intent or wrongdoing.
All outputs are tagged using the project O/D/I/A evidence standard.
Decision authority rests exclusively with the named Decision Owner.

---

## Decision Summary

A monitoring roster of 61,416 providers has been constructed from the
D-PI-1 Tier 1 and Tier 2 provider populations using a three-dimension
composite monitoring placement score. Providers have been assigned to
one of three monitoring intensity levels based on thresholds calibrated
from natural breaks in the composite score distribution.

---

## Roster Overview

| Monitoring Level | Providers | Share | Allowed Dollars | Share |
|---|---|---|---|---|
| Level 1 — Active Surveillance | 1,162 | 1.9% | $0.51B | 7.1% |
| Level 2 — Periodic Review | 1,613 | 2.6% | $0.26B | 3.7% |
| Level 3 — Passive Surveillance | 58,641 | 95.5% | $6.35B | 89.2% |
| **Total** | **61,416** | **100%** | **$7.12B** | **100%** |

---

## Scoring Methodology

The composite monitoring placement score is a weighted sum of three
independent analytical dimensions:

| Dimension | Weight | Source | Description |
|---|---|---|---|
| M1 — Escalation Pathway Weight | 0.40 | D-PI-2 pathway assignment | OIG=1.00 | TPE=0.67 | EdLetter=0.33 | Tier2=0.10 |
| M2 — Composite Escalation Score | 0.35 | D-PI-2 composite score | Carry-forward of full multi-dimensional risk score |
| M3 — Code Scrutiny Intensity | 0.25 | D-POL-1 dimensions flagged | Active dimensions flagged (C2, C3, C4) normalized to [0,1] |

Intensity thresholds were calibrated from natural breaks in the Tier 1
score distribution confirmed in diagnostic cell DOPS1-07-DIAG-01:

- **Level 1 threshold: 0.58** — OIG population minimum score = 0.5751
- **Level 2 threshold: 0.41** — TPE population minimum score = 0.4080
- Education Letter population maximum score = 0.3970 — no crossover into Level 2

All threshold values are tagged Derived (D) following calibration.

---

## Level 1 — Active Surveillance (1,162 providers | $0.51B)

Providers assigned to Level 1 carry composite monitoring scores at or
above 0.58, reflecting the highest combined weight of escalation pathway
severity, D-PI-2 composite risk, and code-level scrutiny intensity.

**Population composition:**
- 606 OIG/DOJ Referral providers — primary Level 1 population
- 556 TPE providers whose composite monitoring score places them at
  active surveillance intensity on own merits

**Decision Owner Note:** 556 TPE providers have been assigned Level 1
intensity despite their D-PI-2 TPE pathway assignment. This reflects
their composite monitoring score exceeding the L1 threshold independently
of pathway label. The Decision Owner should be aware that these providers
warrant active surveillance tracking alongside the OIG referral population.

Recommended action: Monthly billing pattern tracking. Anomaly recurrence
triggers escalation notice to UPIC Case Development.

---

## Level 2 — Periodic Review (1,613 providers | $0.26B)

Providers assigned to Level 2 carry composite monitoring scores between
0.41 and 0.58, reflecting mid-range combined risk across the three
monitoring dimensions.

**Population composition:**
- 1,564 TPE providers in active or recently completed TPE review cycle
- 49 OIG/DOJ Referral providers whose composite monitoring score
  falls below the Level 1 threshold

**Decision Owner Note:** 49 OIG providers have been assigned Level 2
intensity. Their D-PI-2 pathway assignment reflects signal type composition
weighting while their composite monitoring score reflects the broader
three-dimension framework. These providers should be reviewed individually
by the Decision Owner to confirm appropriate monitoring intensity given
their active OIG referral status.

Recommended action: Quarterly billing pattern review. Material change in
pattern triggers re-entry to escalation scoring.

---

## Level 3 — Passive Surveillance (58,641 providers | $6.35B)

Providers assigned to Level 3 carry composite monitoring scores below
0.41. This population is dominated by D-PI-1 Tier 2 providers who were
not routed through D-PI-2 or D-POL-1 and carry neutral scores on M2
and M3 dimensions.

**Population composition:**
- 39,016 D-PI-1 Tier 2 providers — two signals flagged, no D-PI-2 routing
- 19,218 Provider Education Letter providers — lowest D-PI-2 composite scores
- 407 TPE providers whose composite monitoring score places them at
  passive surveillance intensity

**Note on dollar concentration:** The $6.35B allowed dollar exposure at
Level 3 reflects the large Tier 2 population, not elevated individual
provider risk. Per-provider exposure at Level 3 is substantially lower
than at Levels 1 and 2.

Recommended action: Semi-annual billing pattern review. Sustained anomaly
pattern triggers promotion to Level 2.

---

## Analytical Limitations

| Limitation | Impact | Tag |
|---|---|---|
| No prior enforcement history | Cannot assess repeat vs. first-time flag status | A |
| No TPE review outcome data | Cannot confirm whether TPE resolved or escalated anomaly | A |
| No real-time billing data | Monitoring is retrospective to Data Year 2023 | A |
| Tier 2 neutral M2 and M3 scores | Composite scores for Tier 2 reflect assumed mid-range values | A |
| Education Letter neutral M3 scores | D-POL-1 code scrutiny not conducted for EdLetter population | A |

---

## Outputs Produced

| File | Description | Tag |
|---|---|---|
| `monitoring_scored_v1.parquet` | Full provider population with M1–M3 scores and composite monitoring score | D |
| `monitoring_roster_v1.parquet` | Lean operational roster with intensity level assignments | D |
| `monitoring_distribution_v1.png` | Composite score distribution by population and pathway | D |
| `monitoring_roster_summary_v1.png` | Roster composition by intensity level — provider count and dollar exposure | D |

---

## Governance Footnote

This memo reflects analytical outputs produced March 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
UPIC operational structure references sourced from CMS Program Integrity
Manual, Chapter 4, and OIG UPIC evaluation reports.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
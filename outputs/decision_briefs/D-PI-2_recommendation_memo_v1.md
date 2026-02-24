# D-PI-2 Escalation Threshold — Recommendation Memo
# Version: 1.0
# Date: February 2026
# Status: Final — pending Decision Owner review

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Decision authority patterns described remain analytically applicable regardless
> of subsequent organizational changes.
> Readers should verify current organizational alignment independently.

---

## Decision

Which Tier 1 providers from D-PI-1 should be escalated to OIG/DOJ referral,
targeted probe and educate, or provider education, based on a composite
assessment of signal breadth, signal type composition, peer-relative dollar
exposure, and signal persistence?

---

## Decision Owner

**Primary:**
Unified Program Integrity Contractors (UPIC) — Case Development

**Supporting:**
Center for Program Integrity (CPI) — Enforcement

**Secondary:**
Office of Inspector General (OIG) — Referral Recipient

**Decision Owner Role:**
Investigations Lead / UPIC Case Development Manager

---

## Analytical Approach

This recommendation is based on a four-dimension composite scoring model
applied to the 22,400 Tier 1 providers identified in D-PI-1.

| Dimension | Weight | Description |
|---|---|---|
| D1 — Signal Breadth | 0.25 | Proportion of 8 signals flagged |
| D2 — Signal Type Composition | 0.30 | Weighted presence of high-concern signals S4, S5a, S5b |
| D3 — Peer-Relative Dollar Exposure | 0.25 | Allowed per beneficiary vs. Provider Type peer median |
| D4 — Signal Persistence | 0.20 | Signal retention under State × RUCA subgroup re-evaluation |

Composite scores range [0, 1]. Higher scores indicate broader, more intense,
and more persistent anomalous billing patterns relative to peers.

Pathway thresholds were calibrated from the composite score distribution
using natural break analysis (DPI2-07-VIZ-01). Tag: D (Derived).

---

## Composite Score Distribution Summary

| Metric | Value |
|---|---|
| Providers scored | 22,400 |
| Minimum score | 0.2052 |
| Maximum score | 0.8496 |
| Mean score | 0.3035 |
| Median score | 0.2759 |
| Std deviation | 0.0837 |
| p90 | 0.4084 |
| p95 | 0.4682 |
| p99 | 0.5729 |

---

## Pathway Thresholds

| Threshold | Value | Basis |
|---|---|---|
| OIG/DOJ Referral | composite >= 0.50 | Natural break — isolated right tail above p99 |
| Targeted Probe and Educate | composite >= 0.40 and < 0.50 | Natural break — shoulder separating from main body at p90 |
| Provider Education Letter | composite < 0.40 | Main provider body |

---

## Pathway Assignment Results

| Pathway | Provider Count | Share | Composite Score Range | Median Score |
|---|---|---|---|---|
| OIG/DOJ Referral | 655 | 2.9% | 0.5002 – 0.8496 | 0.5421 |
| Targeted Probe and Educate | 2,527 | 11.3% | 0.4000 – 0.4999 | 0.4145 |
| Provider Education Letter | 19,218 | 85.8% | 0.2052 – 0.4000 | 0.2626 |
| **Total** | **22,400** | **100%** | | |

---

## Analytical Notes

**OIG/DOJ Referral (655 providers — 2.9%)**
Providers in this pathway exhibit composite scores ranging from 0.50 to 0.85,
with a median of 0.54. These providers demonstrate broad, intense, and
persistent anomalous billing patterns across multiple independent signal
dimensions relative to their peer groups. The isolated position of this
population in the right tail of the score distribution — well separated from
the main provider body — supports prioritization for formal referral
consideration.

**Targeted Probe and Educate (2,527 providers — 11.3%)**
Providers in this pathway score between 0.40 and 0.50, occupying the shoulder
of the distribution between the main body and the isolated right tail. Scores
are tightly clustered (median 0.41), suggesting a meaningfully distinct
population warranting structured investigation. The billing patterns driving
elevation in this group represent a priority area for further analysis under
D-POL-1 (Code Scrutiny) and D-POL-2 (Specialty Pattern Shift).

**Provider Education Letter (19,218 providers — 85.8%)**
The majority of Tier 1 providers fall in this pathway, with composite scores
between 0.21 and 0.40. These providers were flagged in D-PI-1 based on
multi-signal anomaly agreement, but their composite risk profile supports
administrative education rather than formal escalation at this time.

---

## Known Limitations and Assumptions

| Item | Description | Tag |
|---|---|---|
| D3_CAP | Set to 5.0 provisionally — peer-relative dollar exposure ceiling | A |
| D4 neutral assignments | 801 providers (3.6%) received neutral D4 score due to State × RUCA subgroup < 10 | A |
| No clinical data | Patient complexity not directly measurable from PUF data | A |
| No intent indicators | Composite score reflects statistical patterns only — no inference of provider intent | A |
| Lagged data | CMS PUF reflects Data Year 2023 — patterns may have evolved | A |

---

## Output Files

| File | Location | Description |
|---|---|---|
| escalation_scored_v1.parquet | data/processed/ | Full scored dataset — 22,400 rows, 34 columns |
| escalation_pathway_v1.parquet | data/processed/ | Lean operational file — 22,400 rows, 14 columns |
| pathway_distribution_v1.png | outputs/figures/ | Composite score distribution with threshold markers |

---

## Recommendation

This analytical output is provided to support the decision of the
Investigations Lead / UPIC Case Development Manager regarding escalation
pathway assignment for D-PI-1 Tier 1 providers.

No output in this memo constitutes a finding of fraud, waste, or abuse.
All outputs describe statistical patterns in publicly available billing data
relative to peer providers under defined grouping logic.

Pathway assignments reflect composite analytical signals only. Final
escalation decisions rest entirely with the named Decision Owner and
appropriate operational and legal review processes.

---

## Governance Footnote

This memo reflects analytical work completed February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
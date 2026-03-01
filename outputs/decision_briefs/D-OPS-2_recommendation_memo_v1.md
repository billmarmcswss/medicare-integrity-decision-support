# Decision Brief — D-OPS-2 Resource Allocation
## Medicare Integrity Decision Support Framework

**Prepared for:** CPI Director / Program Integrity Resource Manager
**Supporting:** Office of Enterprise Data and Analytics (OEDA)
**Date:** March 2026
**Status:** Analytical output — informs Decision Owner judgment
**Cadence:** Quarterly resource allocation review

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, Reporting Year 2025,
> Data Year 2023 (RY25/D23). Readers should verify current organizational
> alignment independently.

---

## Legal Disclaimer

All outputs in this brief describe statistical patterns in Medicare Part B
billing data. No output constitutes a finding of fraud, waste, or abuse.
No output asserts provider intent or wrongdoing. Resource estimates are
derived from declared operational assumptions (tag A) and are intended
to inform — not replace — CPI Director judgment.

---

## Decision

How should CPI and UPIC staff time and investigative resources be allocated
across the 61,416 providers on the D-OPS-1 monitoring roster, given their
monitoring intensity level assignments, dollar exposure profiles, and
Level × Pathway cell distributions?

---

## Decision Owner

**Primary:** CPI Director / Program Integrity Resource Manager
**Supporting:** Office of Enterprise Data and Analytics (OEDA)

No analytical output in this framework constitutes a resource allocation
decision. The CPI Director retains full operational authority over staffing
and resource deployment.

---

## Monitoring Roster — Input Summary (tag O, tag D)

The D-OPS-1 monitoring roster contains 61,416 providers assigned to three
monitoring intensity levels based on a composite score derived from escalation
pathway, composite escalation score carry-forward, and code-level scrutiny signal.

| Level | Label | Providers | Share | Allowed Dollars | Share |
|---|---|---|---|---|---|
| Level 1 | Active Surveillance | 1,162 | 1.9% | $0.51B | 7.1% |
| Level 2 | Periodic Review | 1,613 | 2.6% | $0.26B | 3.7% |
| Level 3 | Passive Surveillance | 58,641 | 95.5% | $6.35B | 89.2% |
| **Total** | | **61,416** | | **$7.12B** | |

---

## Workload Demand Estimates (tag A, tag D)

Resource estimates are based on declared operational assumptions.
No CMS staffing actuals are available in public data.
All assumption values are parameterized in CONFIG and subject to revision
when operational benchmarks become available.

### Review Hour Assumptions (tag A)

| Level | Hours per Provider per Quarter | Basis |
|---|---|---|
| Level 1 | 8.0 hours | Full case review cycle |
| Level 2 | 4.0 hours | Structured follow-through |
| Level 3 | 0.5 hours | Automated flag and log |

FTE capacity assumed at 480 hours per FTE per quarter (40 hrs/week × 12 weeks).

### FTE Demand by Monitoring Level — Base Scenario (tag D)

| Level | Providers | Total Hours | FTE Demand |
|---|---|---|---|
| Level 1 Active Surveillance | 1,162 | 9,296 | 19.4 FTEs |
| Level 2 Periodic Review | 1,613 | 6,452 | 13.4 FTEs |
| Level 3 Passive Surveillance | 58,641 | 29,321 | 61.1 FTEs |
| **Total** | **61,416** | **45,069** | **93.9 FTEs** |

---

## Sensitivity Analysis (tag A, tag D)

FTE demand is sensitive to review hour assumptions, particularly for
Level 3 where provider volume is highest. The following scenarios bound
the plausible resource range.

| Scenario | L1 Hours | L2 Hours | L3 Hours | Total FTE Demand |
|---|---|---|---|---|
| Low | 6.0 | 3.0 | 0.25 | 55.1 FTEs/quarter |
| Base | 8.0 | 4.0 | 0.50 | 93.9 FTEs/quarter |
| High | 12.0 | 6.0 | 1.00 | 171.4 FTEs/quarter |

The wide sensitivity range (55 to 171 FTEs) reflects the absence of
operational benchmarks for review time by intensity level. CPI is advised
to track actual review hours during the first monitoring cycle to
calibrate future quarters.

---

## Level × Pathway Cell Distribution (tag D)

Seven of twelve possible Level × Pathway cells are populated.
Five cells are structurally empty due to pathway routing logic.

| Level | Pathway | Providers | FTE Demand | Allowed Dollars |
|---|---|---|---|---|
| Level 1 | OIG/DOJ Referral | 606 | 10.1 | $0.36B |
| Level 1 | TPE | 556 | 9.3 | $0.14B |
| Level 2 | OIG/DOJ Referral | 49 | 0.4 | $0.01B |
| Level 2 | TPE | 1,564 | 13.0 | $0.26B |
| Level 3 | TPE | 407 | 0.4 | $0.03B |
| Level 3 | Education Letter | 19,218 | 20.0 | $3.49B |
| Level 3 | Tier 2 Only | 39,016 | 40.6 | $2.82B |

### Notable Cell Findings (tag I)

**556 TPE providers scored into Level 1 Active Surveillance.**
These providers were routed to the Targeted Probe and Educate pathway
by D-PI-2 but scored into the highest monitoring intensity level on their
own composite merits. This population (22.0% of all TPE providers)
warrants prioritized attention within the TPE process — their anomalous
billing pattern signal is stronger than pathway assignment alone suggests.

**49 OIG providers scored into Level 2 Periodic Review.**
These providers were routed to the OIG/DOJ Referral pathway but scored
below the Level 1 threshold on composite merits. The CPI Director and
OIG coordination lead should be aware that these 49 providers are
receiving Periodic Review rather than Active Surveillance monitoring
intensity. Decision Owner review of this population is advised before
the first monitoring cycle begins.

---

## Priority Allocation Guidance (tag I)

Based on the analytical outputs above, the following allocation
priorities are supported by the data.

**Priority 1 — Level 1 OIG Cell (606 providers, $0.36B)**
Highest composite monitoring score. Full case review cycle required.
Estimated 10.1 FTEs/quarter under base assumptions.

**Priority 2 — Level 1 TPE Cell (556 providers, $0.14B)**
TPE providers who scored into Active Surveillance on composite merits.
Should be tracked in parallel with OIG cell at equivalent review intensity.
Estimated 9.3 FTEs/quarter under base assumptions.

**Priority 3 — Level 2 TPE Cell (1,564 providers, $0.26B)**
Largest Level 2 cell by provider count and dollar exposure.
Structured follow-through required. Estimated 13.0 FTEs/quarter.

**Priority 4 — Level 3 Education Letter Cell (19,218 providers, $3.49B)**
Largest dollar exposure cell in the roster. Passive surveillance only,
but represents $3.49B in allowed dollars requiring automated monitoring.
Estimated 20.0 FTEs/quarter under base assumptions.

**Priority 5 — Level 3 Tier 2 Only Cell (39,016 providers, $2.82B)**
Largest provider count cell. Passive surveillance with automated
flag-and-log. Volume drives FTE demand (40.6 FTEs base) despite
low per-provider intensity. Automation investment is advisable
to manage this cell efficiently at scale.

---

## Key Uncertainties (tag A)

| ID | Uncertainty | Decision Impact |
|---|---|---|
| U1 | No CMS staffing actuals available | FTE estimates carry full assumption risk — base scenario may over- or under-state demand significantly |
| U2 | No prior enforcement history | Cannot distinguish repeat from first-time anomalous pattern providers — affects Level 1 prioritization within cells |
| U3 | Monitoring data is retrospective to Data Year 2023 | Billing patterns may have changed — roster reflects 2023 signal only |
| U4 | Level 3 automation capability unknown | If automated flag-and-log is unavailable, Level 3 FTE demand rises substantially |

---

## Recommended Actions for Decision Owner

1. **Benchmark review hours** during the first monitoring quarter.
   Track actual hours spent per provider by intensity level and use
   to recalibrate FTE demand estimates for Q2.

2. **Review the 49 OIG providers in Level 2.**
   Confirm that Periodic Review intensity is operationally acceptable
   for providers on the OIG referral pathway, or escalate selectively
   to Level 1 Active Surveillance.

3. **Prioritize automation investment for Level 3.**
   39,016 Tier 2 providers and 19,218 Education Letter providers
   at 0.5 hrs each represent 40.6 and 20.0 FTEs respectively
   under base assumptions. Automated monitoring tools would
   reduce this demand materially.

4. **Establish a quarterly roster refresh cadence.**
   The monitoring roster is based on Data Year 2023 billing patterns.
   Refresh when updated CMS Part B data becomes available.

---

## Outputs Produced

| File | Location | Tag |
|---|---|---|
| `resource_allocation_v1.parquet` | `data/processed/` | D |
| `resource_level_summary_v1.parquet` | `data/processed/` | D |
| `resource_workload_summary_v1.png` | `outputs/figures/` | D |
| `resource_exposure_by_cell_v1.png` | `outputs/figures/` | D |
| `resource_sensitivity_v1.png` | `outputs/figures/` | D |

---

## Governance Footnote

This brief reflects decision framing established March 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
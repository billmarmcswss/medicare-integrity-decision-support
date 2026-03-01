# Decision Charter — D-POL-2 Specialty Pattern Shift

**Version:** 1.0
**Date:** February 2026
**Status:** Active

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Decision authority patterns described remain analytically applicable regardless
> of subsequent organizational changes. Data references are based on CMS Medicare
> Part B Public Use File, Reporting Year 2025, Data Year 2023 (RY25/D23).
> Readers should verify current organizational alignment independently.

---

## Decision

Which Provider Type specialties exhibit disproportionate concentration of
anomalous HCPCS code billing patterns identified in D-POL-1, and do those
patterns reflect meaningful shifts from expected specialty norms that warrant
policy-level intervention — either through targeted specialty guidance,
focused medical review protocols, or escalation to enforcement pathways?

---

## Decision Owner

**Primary:**
Center for Program Integrity (CPI) — Policy Analysis
Responsible for identifying specialty-level billing pattern anomalies and
determining whether patterns warrant policy guidance, educational
intervention, or referral for further enforcement action.

**Supporting:**
Center for Medicare (CM) — Payment Policy
Responsible for payment policy context and Provider Type classification
standards. Informed of specialty-level patterns that may indicate systemic
billing practice issues requiring payment policy review or LCD/NCD guidance.

**Secondary:**
Medicare Administrative Contractors (MAC) — Medical Review
Consumes D-POL-2 specialty pattern findings to align TPE review protocols
with the specific Provider Type specialties identified as exhibiting
anomalous pattern concentration.

---

## Decision Actor Tag

`POL` — Policy

---

## Decision Owner Role

Medical Review Director / CPI Policy Analyst

---

## Cadence

- Operational: Semi-annual specialty pattern review aligned with D-POL-1
  quarterly code pattern review cycle
- Strategic: Annual review of specialty norm thresholds and pattern
  shift detection parameters

---

## Decision Context

### Why This Decision Exists

D-POL-1 identified 1,886 unique HCPCS codes driving anomalous billing
patterns among 3,182 OIG and TPE providers, with 554 providers flagged
across all three active analytical dimensions. The top frequency code
(99223 Hospitalist) appeared across 100 providers; the top exposure code
(81519 Clinical Laboratory) represented approximately $85M in flagged
allowed dollars.

D-POL-1 answered which codes are anomalous. D-POL-2 answers which
specialties are driving those anomalies — and whether the patterns
reflect a meaningful shift from expected specialty norms.

Without specialty-level pattern analysis, CPI and CM cannot:

- Determine whether anomalous code patterns are isolated to specific
  Provider Type specialties or broadly distributed across the provider
  population
- Identify whether certain specialties are disproportionately represented
  in OIG referral versus TPE pathways
- Distinguish between specialty-level patterns that reflect legitimate
  coding complexity and those that reflect systematic deviation from
  specialty norms warranting policy intervention
- Develop targeted specialty-level guidance, LCD/NCD review triggers,
  or focused medical review protocols aligned with actual risk concentration
- Establish a temporal baseline for detecting future specialty-level
  pattern shifts as multi-year data becomes available

### Two Analytical Tracks

D-POL-2 addresses the decision through two complementary analytical tracks:

**Track 1 — Cross-Specialty Pattern Comparison**
Examines which Provider Type specialties are disproportionately
represented among the anomalous code patterns identified in D-POL-1.
Directly answerable from Data Year 2023 outputs. Produces actionable
specialty-level risk profiles for CPI policy guidance.

**Track 2 — Intra-Specialty Temporal Shift Framework**
Establishes the analytical architecture for detecting billing pattern
shifts within specialties over time. Data Year 2023 provides the baseline.
The framework is designed to resolve from assumed (A) to derived (D) when
multi-year data becomes available. Documents the design, parameters, and
intended detection logic for future operational use.

Both tracks are presented together to ensure the Decision Owner has full
analytical context — cross-sectional findings from Track 1 and the
forward-looking detection framework from Track 2.

### What Happens If Wrong

**False positive specialty identification (flagging specialties that are
not anomalous):**
- CPI policy guidance targets specialties that do not reflect actual
  billing irregularities
- MAC review protocols misaligned with actual risk concentration
- Provider community trust in CMS oversight erodes for flagged specialties
- Downstream D-OPS-1 monitoring placement receives incorrect specialty inputs

**False negative specialty identification (missing anomalous specialties):**
- Specialty-level patterns escape policy scrutiny
- Systemic billing practice issues remain unaddressed through policy cycle
- OIG referral packages lack specialty context, reducing investigative utility
- Pattern shifts go undetected until next semi-annual review cycle

**Misattribution of anomalies to wrong specialty drivers:**
- Policy guidance targets wrong Provider Types
- MAC review protocols misaligned with actual anomaly concentration
- Temporal shift detection framework calibrated to wrong baseline

### What Happens If Delayed

- D-POL-1 code findings remain without specialty-level context
- CPI policy guidance cycle delayed by one semi-annual period minimum
- D-OPS-1 Monitoring Placement cannot begin without D-POL-2 specialty
  risk profiles
- Temporal baseline establishment delayed, compounding future detection gaps

---

## Primary Uncertainties

| ID | Uncertainty | Decision Impact |
|---|---|---|
| U1 | Are anomalous code patterns concentrated in a small number of specialties or broadly distributed? | Determines whether targeted or broad specialty guidance is warranted |
| U2 | Do OIG and TPE providers cluster in different specialties, or do their specialty profiles overlap significantly? | Affects whether pathway-stratified specialty analysis is required |
| U3 | Does specialty-level anomaly concentration persist when controlling for code-level peer group differences? | Affects reliability of cross-specialty comparison |
| U4 | Are high-frequency anomalous codes (e.g., 99223, 78815) driven by a small number of specialty outliers or broadly distributed within their specialty? | Determines whether code-specialty combinations or specialties alone drive policy targets |
| U5 | Is Data Year 2023 representative of typical specialty billing patterns, or does it reflect anomalous conditions specific to that year? | Affects confidence in temporal baseline establishment |
| U6 | Are specialty norm thresholds stable enough across semi-annual review cycles to support pattern shift detection? | Affects Track 2 framework reliability |

---

## Analytical Approach — Two Tracks, Four Dimensions

### Track 1 — Cross-Specialty Pattern Comparison

#### Dimension 1 — Specialty Anomaly Concentration (S1)
Measures the share of flagged providers within each Provider Type specialty
relative to the total provider population for that specialty. Identifies
specialties where anomalous patterns are disproportionately concentrated.

- Source: `code_scrutiny_flags_v1.parquet` joined to
  `provider_tiered_v1.parquet` for specialty population denominators
- Flag condition: Specialty flagged provider share exceeds defined
  concentration threshold (parameterized)
- Evidence tag: Derived (D)

#### Dimension 2 — Pathway Stratification by Specialty (S2)
For each Provider Type specialty, measures the distribution of OIG versus
TPE pathway assignments among flagged providers. Identifies specialties
with disproportionate OIG escalation relative to TPE assignment.

- Source: `code_scrutiny_flags_v1.parquet` joined to
  `escalation_pathway_v1.parquet`
- Flag condition: Specialty OIG share exceeds defined pathway ratio
  threshold (parameterized)
- Evidence tag: Derived (D)

### Track 2 — Intra-Specialty Temporal Shift Framework

#### Dimension 3 — Specialty Norm Baseline (S3)
Establishes Data Year 2023 as the reference baseline for each Provider
Type specialty across key billing metrics: allowed per beneficiary,
services per beneficiary, and top-code concentration share. Parameterized
for direct comparison against future data years when available.

- Source: `provider_rollup_v1.parquet` aggregated by Provider Type
- Output: Specialty-level baseline metrics table for future comparison
- Evidence tag: Assumed (A) — resolves to Derived (D) when multi-year
  data is available
- Note: Framework architecture complete; temporal detection requires
  Data Year 2024 or later input

#### Dimension 4 — Shift Detection Threshold Design (S4)
Documents the parameterized detection logic for identifying meaningful
specialty-level pattern shifts between data years. Defines shift magnitude
thresholds, minimum provider count requirements for reliable detection,
and flag conditions for escalation to policy review.

- Source: Framework design based on D-PI-1 Z-score methodology;
  applied at specialty aggregate level
- Flag condition: Specialty metric shift exceeds defined magnitude
  threshold between data years (parameterized)
- Evidence tag: Assumed (A) — resolves to Derived (D) when multi-year
  data is available
- Note: All parameters are fully documented and ready for operational
  use upon data availability

---

## Evidence Required

**Track 1:**
- 3,182 OIG + TPE provider specialty assignments from
  `code_scrutiny_flags_v1.parquet`
- Full provider population specialty counts from
  `provider_tiered_v1.parquet` for concentration denominators
- Pathway assignments from `escalation_pathway_v1.parquet`
- Top anomalous codes by specialty from
  `top_codes_by_frequency_v1.parquet`

**Track 2:**
- Specialty-level aggregate billing metrics from
  `provider_rollup_v1.parquet`
- Parameterized shift detection thresholds documented with rationale
- Framework design documentation for future multi-year application

---

## Data Dependencies

**Primary Input Datasets:**

| File | Source | Content |
|---|---|---|
| `code_scrutiny_flags_v1.parquet` | D-POL-1 output | Provider-level code scrutiny flags with pathway assignments |
| `top_codes_by_frequency_v1.parquet` | D-POL-1 output | HCPCS codes ranked by provider flag frequency |
| `top_codes_by_exposure_v1.parquet` | D-POL-1 output | HCPCS codes ranked by total allowed dollar exposure |
| `escalation_pathway_v1.parquet` | D-PI-2 output | Provider pathway assignments (OIG, TPE, EdLetter) |
| `provider_tiered_v1.parquet` | D-PI-1 output | Full provider population with specialty assignments |
| `provider_rollup_v1.parquet` | D-PI-1 output | Provider-level billing metrics for specialty aggregation |

**Reference Documentation:**
- Data Dictionary: `MUP_PHY_RY25_20250312_DD_PRV_SVC_508.pdf`
- Technical Specifications: `MUP_PHY_RY25_20250312_Technical_Specifications_508.pdf`
- Methodology: `MUP_PHY_RY25_20250312_Methodology_508.pdf`

**Known Data Gaps:**

| Gap | Impact | Tag |
|---|---|---|
| Single data year (2023 only) | Track 2 temporal shift detection cannot be executed; framework design complete but data-constrained | A |
| No patient complexity data | Specialty-level intensity differences may partly reflect case mix variation across Provider Types | A |
| No prior enforcement history | Cannot distinguish first-cycle anomalous specialties from persistent patterns | A |
| Provider Type classification may aggregate distinct sub-specialties | Specialty-level patterns may mask sub-specialty concentration | A |
| Lagged data (Data Year 2023) | Specialty patterns may have evolved since reporting period | A |

---

## Analytical Requirements

- Track 1 fully executable from existing D-POL-1 and D-PI-2 outputs
- Track 2 framework fully documented and parameterized; data-constrained
  flag applied to all temporal outputs
- All dimension thresholds fully parameterized with documented rationale
- Pathway-stratified specialty output: OIG and TPE specialty profiles
  produced jointly and separately
- Top anomalous specialties ranked by flagged provider concentration
  and by dollar exposure
- Specialty-level baseline metrics table produced for Track 2 future use
- Colorblind-accessible visualizations using IBM Carbon palette throughout

---

## Output Tagging Standard

| Tag | Meaning |
|---|---|
| O | Observed — directly in the data |
| D | Derived — mathematically constructed from observed data |
| I | Inferred — evidence-based pattern interpretation |
| A | Assumed — declared gap-filling for decision support |

**No output may assert provider intent, fraud, or wrongdoing.**
Language is restricted to: anomalous specialty-level patterns, statistical
deviation, peer-adjusted billing intensity, specialty norm concentration,
and pattern shift detection framework.

---

## Outputs Produced

| File | Location | Description | Tag |
|---|---|---|---|
| `specialty_pattern_flags_v1.parquet` | `data/processed/` | Specialty-level anomaly concentration and pathway stratification | D |
| `specialty_baseline_v1.parquet` | `data/processed/` | Track 2 baseline metrics by Provider Type specialty | A |
| `specialty_concentration_v1.png` | `outputs/figures/` | Specialty anomaly concentration by pathway | D |
| `specialty_pathway_distribution_v1.png` | `outputs/figures/` | OIG vs TPE specialty distribution | D |
| `specialty_baseline_v1.png` | `outputs/figures/` | Track 2 baseline visualization for future comparison | A |
| `D-POL-2_recommendation_memo_v1.md` | `outputs/decision_briefs/` | Stakeholder-ready specialty pattern memo | I |
| Evidence log entries E-DPOL2-01 through E-DPOL2-XX | `decisions/D-POL-2_specialty_shift/` | O/D/I/A tagged analytical outputs | — |

---

## Decision Impact Pathway

D-POL-1 code scrutiny flags (OIG + TPE providers) (O)
  -> Specialty-level anomaly concentration scoring — Track 1 (D)
    -> Pathway-stratified specialty profiles (D)
      -> Disproportionate specialty pattern identification (I)
        -> CPI specialty-level policy guidance targets (I)
          -> MAC Medical Review specialty-focused TPE protocols (D)
          -> D-OPS-1 Monitoring Placement specialty inputs (D)
  -> Specialty norm baseline establishment — Track 2 (A)
    -> Shift detection framework — ready for multi-year data (A)
      -> Future: Pattern shift detection resolves to (D)

---

## Notebook Dependencies

| Notebook | Role | Location |
|---|---|---|
| `D-POL-2_specialty_shift.ipynb` | Specialty pattern scoring and policy output | `notebooks/decision/` |

**Upstream dependencies:**
- D-PI-1 signal engine outputs (`provider_tiered_v1.parquet`,
  `provider_rollup_v1.parquet`)
- D-PI-2 escalation outputs (`escalation_pathway_v1.parquet`)
- D-POL-1 code scrutiny outputs (`code_scrutiny_flags_v1.parquet`,
  `top_codes_by_frequency_v1.parquet`,
  `top_codes_by_exposure_v1.parquet`)

**Downstream consumers:**
- D-OPS-1 Monitoring Placement (consumes `specialty_pattern_flags_v1.parquet`)
- CPI policy guidance development (consumes
  `D-POL-2_recommendation_memo_v1.md`)
- MAC TPE review protocol alignment (consumes
  `specialty_pattern_flags_v1.parquet`)
- Future temporal shift detection (consumes `specialty_baseline_v1.parquet`)

---

## Governance Footnote

This charter reflects decision framing established February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
UPIC and MAC operational structure references sourced from CMS Program
Integrity Manual, Chapter 4, and OIG UPIC evaluation reports.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.

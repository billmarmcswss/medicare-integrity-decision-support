# D-POL-2 Specialty Pattern Shift — Recommendation Memo

**To:** Medical Review Director / CPI Policy Analyst
**From:** Medicare Integrity Decision Support Framework
**Date:** February 2026
**Re:** Specialty-Level Billing Pattern Analysis — Data Year 2023
**Classification:** Analytical Output — Statistical Patterns Only

---

## Legal and Use Disclaimer

This memo is part of an independent analytical portfolio project. It is not
affiliated with, endorsed by, or produced in cooperation with CMS, HHS, OIG,
or any federal, state, or local government agency. All data is publicly
available. No outputs constitute actionable findings, referrals, or
recommendations under any federal statute. Analytical outputs describe
statistical patterns in publicly available billing data only.

---

## Purpose

This memo summarizes specialty-level billing pattern findings from D-POL-2
Specialty Pattern Shift, the fourth module of the Medicare Integrity Decision
Support Framework. It identifies which Provider Type specialties exhibit
disproportionate concentration of anomalous code billing patterns among the
3,182 providers assigned to OIG/DOJ Referral and Targeted Probe and Educate
pathways in D-PI-2, and documents a Data Year 2023 baseline for future
temporal shift detection.

---

## Analytical Framework

Two analytical tracks were executed:

**Track 1 — Cross-Specialty Pattern Comparison (Data Year 2023)**
Fully executable from D-POL-1 outputs. Two dimensions:
- S1 Specialty Anomaly Concentration — flagged provider share by specialty
- S2 Pathway Stratification — OIG vs TPE distribution by specialty

**Track 2 — Intra-Specialty Temporal Shift Framework (Baseline Only)**
Data Year 2023 establishes the reference baseline for future temporal shift
detection. All Track 2 outputs are tagged Assumed (A) — data-constrained.
Framework is complete and parameterized. Resolves to Derived (D) when
Data Year 2024 or later becomes available.

---

## Input Population

| Source | Count |
|---|---|
| OIG/DOJ Referral providers (from D-PI-2) | 655 |
| Targeted Probe and Educate providers (from D-PI-2) | 2,527 |
| Total input population | 3,182 |
| Unique Provider Type specialties represented | 74 |

---

## Track 1 Findings — Cross-Specialty Pattern Comparison

### S1 — Specialty Anomaly Concentration

Nine specialties exceeded the S1 concentration threshold of 0.75%
(flagged providers as share of total specialty population).
Threshold derived from natural break in observed distribution. Tag D.

| Specialty | Flagged Providers | Total Specialty | Flagged Share |
|---|---|---|---|
| Hospitalist | 241 | 17,729 | 1.36% |
| General Surgery | 178 | 15,640 | 1.14% |
| Mass Immunizer Roster Biller | 260 | 26,981 | 0.96% |
| Interventional Radiology | 18 | 2,141 | 0.84% |
| Dermatology | 94 | 11,865 | 0.79% |
| Pulmonary Disease | 76 | 9,743 | 0.78% |
| Diagnostic Radiology | 237 | 30,684 | 0.77% |
| Colorectal Surgery (Proctology) | 11 | 1,441 | 0.76% |
| Otolaryngology | 61 | 8,088 | 0.75% |

### S2 — Pathway Stratification by Specialty

Eleven reliable specialties exceeded the S2 OIG share threshold of 35%
(OIG/DOJ Referral providers as share of total flagged providers per specialty).
Threshold derived from natural break in observed distribution. Tag D.

| Specialty | OIG Count | TPE Count | Total | OIG Share |
|---|---|---|---|---|
| Pathology | 15 | 9 | 24 | 62.5% |
| Vascular Surgery | 8 | 6 | 14 | 57.1% |
| Urology | 25 | 23 | 48 | 52.1% |
| Clinical Laboratory | 6 | 6 | 12 | 50.0% |
| Otolaryngology | 29 | 32 | 61 | 47.5% |
| Clinical Cardiac Electrophysiology | 6 | 7 | 13 | 46.2% |
| Interventional Cardiology | 9 | 11 | 20 | 45.0% |
| Mass Immunizer Roster Biller | 106 | 154 | 260 | 40.8% |
| Diagnostic Radiology | 91 | 146 | 237 | 38.4% |
| Centralized Flu | 15 | 26 | 41 | 36.6% |
| Cardiology | 21 | 37 | 58 | 36.2% |

### Priority Finding — Both Dimensions Firing

Three specialties exhibit both disproportionate concentration (S1) and
disproportionate OIG routing (S2) simultaneously. These represent the
highest policy priority targets from this analysis.

| Specialty | Flagged Share | OIG Share |
|---|---|---|
| Mass Immunizer Roster Biller | 0.96% | 40.8% |
| Diagnostic Radiology | 0.77% | 38.4% |
| Otolaryngology | 0.75% | 47.5% |

**Analytical note — Hospitalist:** Highest S1 concentration at 1.36% but
low OIG share at 6.6%. High volume of anomalous patterns but predominantly
routing to Provider Education Letter pathway — consistent with education-
appropriate intervention rather than law enforcement referral.

**Analytical note — Physical Therapist in Private Practice:** Largest
absolute flagged count at 297 providers but low concentration share at
0.45% relative to 65,881 total PT providers. Predominantly TPE routing
at 85.9%. High volume, education and review appropriate.

---

## Track 2 Findings — Specialty Norm Baseline

### Data Year 2023 Baseline Established

Specialty-level baseline metrics established for 101 reliable Provider Type
specialties (minimum 10 providers). Three metrics captured:

- Median allowed dollars per beneficiary
- Median services per beneficiary
- Median top-1 HCPCS code concentration share

**Selected baseline highlights (tag A — data-constrained):**

| Specialty | Median Allowed/Bene | Median Services/Bene | Median Top-1 Share |
|---|---|---|---|
| Ambulatory Surgical Center | $751.57 | 1.44 | 41.7% |
| Cardiac Surgery | $397.64 | 1.02 | 51.8% |
| Opioid Treatment Program | $172.85 | 19.70 | 82.6% |
| Ambulance Service Provider | $243.79 | 9.89 | 68.8% |

**Note:** These baseline values reflect legitimate specialty billing
characteristics, not anomalies. High services per beneficiary for Opioid
Treatment Program and Ambulance Service Provider reflect the nature of
those services. Baseline values are reference points for future temporal
shift detection only.

### Temporal Shift Detection Framework

The detect_specialty_shift() function is defined, parameterized, and
operationally ready. Shift magnitude threshold set provisionally at 20%
change between data years. Framework activates when Data Year 2024 or
later becomes available. All Track 2 outputs remain tagged A until
multi-year data is incorporated.

---

## Outputs Produced

| File | Location | Tag |
|---|---|---|
| `specialty_pattern_flags_v1.parquet` | `data/processed/` | D |
| `specialty_baseline_v1.parquet` | `data/processed/` | A |
| `specialty_pathway_detail_v1.parquet` | `data/processed/` | D |
| `specialty_concentration_v1.png` | `outputs/figures/` | D |
| `specialty_pathway_distribution_v1.png` | `outputs/figures/` | D |
| `specialty_baseline_v1.png` | `outputs/figures/` | A |

---

## Limitations and Assumptions

- No patient-level clinical or risk data available — specialty billing
  intensity differences may partly reflect case mix (tag A)
- No prior enforcement history — cannot distinguish first-offense from
  repeat patterns at specialty level (tag A)
- Provider Type classification may aggregate distinct sub-specialties —
  granular sub-specialty analysis not available in this dataset (tag A)
- Single data year — temporal shift detection data-constrained (tag A)
- Lagged data — Data Year 2023 patterns may have evolved since
  reporting period (tag A)

---

## Downstream Consumers

| Module | Consumes |
|---|---|
| D-OPS-1 Monitoring Placement | `specialty_pattern_flags_v1.parquet` |
| CPI policy guidance development | Memo findings |
| MAC TPE review protocol alignment | S2 pathway stratification findings |
| Future temporal shift detection | `specialty_baseline_v1.parquet` |

---

## Evidence Tags Applied

| Tag | Meaning |
|---|---|
| O | Observed — directly in the data |
| D | Derived — mathematically constructed from observed data |
| I | Inferred — evidence-based pattern interpretation |
| A | Assumed — declared gap-filling for decision support |

No output in this memo asserts provider intent, fraud, waste, or abuse.
All findings describe statistical patterns in publicly available billing
data relative to specialty peer norms.

---

## Governance Footnote

This memo reflects analytical framing established February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
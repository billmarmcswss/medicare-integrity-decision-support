# D-POL-1 Code Scrutiny — Recommendation Memo
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

Which HCPCS codes are driving anomalous billing patterns among the 3,182
providers assigned to OIG/DOJ Referral and Targeted Probe and Educate
pathways in D-PI-2, and do those codes represent statistically significant
deviations from specialty peer norms warranting policy-level scrutiny?

---

## Decision Owner

**Primary:**
Center for Program Integrity (CPI) — Policy Analysis

**Supporting:**
Medicare Administrative Contractors (MAC) — Medical Review

**Secondary:**
Center for Medicare (CM) — Payment Policy

**Decision Owner Role:**
CPI Policy Analyst / Medical Review Director

---

## Analytical Approach

This recommendation is based on a four-dimension code-level scrutiny model
applied to 17,676 provider × HCPCS rows across 3,182 OIG + TPE providers.

| Dimension | Description | Status |
|---|---|---|
| C1 — Code Concentration | Top-3 code share vs. Provider Type peer median | Non-discriminating — excluded |
| C2 — Peer Deviation by HCPCS | Allowed per bene vs. peer median Z-score >= 2.0 | Active |
| C3 — High-Concern Signal Overlap | C2 deviation firing on provider top-1 HCPCS code | Active |
| C4 — Specialty Norm Comparison | Allowed per bene above national p90 by Provider Type x HCPCS | Active |

**C1 Note:** Top-3 code share median is 0.980 (OIG) and 1.000 (TPE).
Peer median similarly near 1.0 — threshold non-discriminating for this
population. Retained for future analysis. Tag: A.

---

## Provider Population Summary

| Pathway | Providers | Share |
|---|---|---|
| OIG/DOJ Referral | 655 | 20.6% |
| Targeted Probe and Educate | 2,527 | 79.4% |
| **Total** | **3,182** | **100%** |

---

## Dimension Flag Results

| Dimension | Rows Flagged | Providers Flagged | Share of 3,182 |
|---|---|---|---|
| C2 — Peer Deviation | 3,237 | 878 | 27.6% |
| C3 — Signal Overlap | 577 | 577 | 18.1% |
| C4 — National Norm | 5,054 | 1,500 | 47.1% |

---

## Provider Flag Summary (Active Dimensions C2, C3, C4)

| Dimensions Flagged | Provider Count | Share |
|---|---|---|
| 0 | 1,645 | 51.7% |
| 1 | 673 | 21.2% |
| 2 | 310 | 9.7% |
| 3 | 554 | 17.4% |
| **Total** | **3,182** | **100%** |

| Dimensions Flagged | OIG Count | TPE Count |
|---|---|---|
| 0 | 236 | 1,409 |
| 1 | 210 | 463 |
| 2 | 105 | 205 |
| 3 | 104 | 450 |

---

## Top Anomalous HCPCS Codes — By Provider Flag Frequency

| Rank | HCPCS Code | Provider Type | Providers Flagged |
|---|---|---|---|
| 1 | 99223 | Hospitalist | 100 |
| 2 | 78815 | Diagnostic Radiology | 98 |
| 3 | K1034 | Mass Immunizer Roster Biller | 89 |
| 4 | 97110 | Physical Therapist in Private Practice | 82 |
| 5 | 78816 | Diagnostic Radiology | 72 |
| 6 | 76937 | Diagnostic Radiology | 51 |
| 7 | 99291 | Internal Medicine | 48 |
| 8 | 99222 | Hospitalist | 48 |
| 9 | 97161 | Physical Therapist in Private Practice | 46 |
| 10 | 99223 | Internal Medicine | 46 |

---

## Top Anomalous HCPCS Codes — By Allowed Dollar Exposure

| Rank | HCPCS Code | Provider Type | Total Allowed ($M) |
|---|---|---|---|
| 1 | 81519 | Clinical Laboratory | ~$85M |
| 2 | 78815 | Diagnostic Radiology | ~$48M |
| 3 | 81479 | Clinical Laboratory | ~$22M |
| 4 | 81521 | Clinical Laboratory | ~$9M |
| 5 | 37243 | Diagnostic Radiology | ~$8M |

---

## Analytical Notes

**Clinical Laboratory dollar dominance:**
Code 81519 (multi-gene cancer panel) represents the single largest
allowed dollar exposure at approximately $85M despite not appearing
in the top frequency list. High unit cost drives dollar exposure
independent of provider count. Clinical Laboratory codes 81479 and
81521 also appear in the top exposure list — a cluster warranting
focused policy attention.

**Diagnostic Radiology breadth:**
Diagnostic Radiology dominates both the frequency and exposure panels.
Codes 78815 and 78816 (PET scan oncology imaging) appear in both
panels — high frequency and high dollar exposure. 12 of the top 20
frequency codes are Diagnostic Radiology.

**Hospitalist admission code pattern:**
Code 99223 (high-complexity inpatient admission) appears for both
Hospitalists (100 providers) and Internal Medicine (46 providers).
This code is associated with known upcoding risk in program integrity
literature.

**Physical Therapy cluster:**
Three PT codes appear in the top frequency list (97110, 97161, 97140)
suggesting a specialty-level pattern warranting further analysis under
D-POL-2 Specialty Pattern Shift.

**Frequency vs. exposure divergence:**
The codes driving the highest provider flag frequency are not the same
codes driving the highest dollar exposure. CPI should apply both
rankings when prioritizing policy guidance targets.

**Zero-dimension providers (1,645 — 51.7%):**
Providers with no code-level flags are not falling through the cracks.
Their anomaly patterns from D-PI-1/D-PI-2 are likely driven by broadly
distributed billing intensity rather than specific code concentration.
These providers are candidates for D-POL-2 Specialty Pattern Shift
analysis.

---

## Known Limitations and Assumptions

| Item | Description | Tag |
|---|---|---|
| C1 non-discriminating | Top-3 code share near 1.0 for both pathways — threshold unreachable | A |
| MIN_PEER_SIZE = 5 | 567 provider x HCPCS rows assigned neutral score | A |
| C4 threshold = p90 | National percentile threshold provisional — recalibrate in future cycle | A |
| Avg_ dollar columns | Total allowed derived as Avg_Mdcr_Alowd_Amt x Tot_Benes | D |
| No claims-level detail | Cannot assess medical necessity or documentation at code level | A |
| No patient complexity | Code-level intensity differences may partly reflect case mix | A |
| Lagged data | CMS PUF reflects Data Year 2023 — patterns may have evolved | A |

---

## Output Files

| File | Location | Description |
|---|---|---|
| hcpcs_scored_v1.parquet | data/processed/ | Full provider x HCPCS scored dataset — 17,676 rows, 47 cols |
| code_scrutiny_flags_v1.parquet | data/processed/ | Provider-level flag summary — 3,182 rows, 10 cols |
| top_codes_by_frequency_v1.parquet | data/processed/ | HCPCS codes ranked by provider flag frequency — 1,886 rows |
| top_codes_by_exposure_v1.parquet | data/processed/ | HCPCS codes ranked by dollar exposure — 1,886 rows |
| code_concentration_v1.png | outputs/figures/ | Concentration distribution by pathway |
| top_flagged_codes_v1.png | outputs/figures/ | Top anomalous codes dual panel |

---

## Recommendation

This analytical output is provided to support the decision of the
CPI Policy Analyst / Medical Review Director regarding code-level
scrutiny priorities for D-PI-2 escalation pathway providers.

No output in this memo constitutes a finding of fraud, waste, or abuse.
All outputs describe statistical patterns in publicly available billing
data relative to peer providers under defined grouping logic.

Code-level findings reflect statistical patterns only. Final policy
guidance, TPE focus list construction, and OIG referral package
specificity decisions rest entirely with the named Decision Owner
and appropriate operational and legal review processes.

---

## Governance Footnote

This memo reflects analytical work completed February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
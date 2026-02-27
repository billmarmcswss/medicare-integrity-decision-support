# Decision Charter — D-POL-1 Code Scrutiny

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

Which HCPCS codes are driving anomalous billing patterns among the 3,182
providers assigned to OIG/DOJ Referral and Targeted Probe and Educate
pathways in D-PI-2, and do those codes represent statistically significant
deviations from specialty peer norms warranting policy-level scrutiny?

---

## Decision Owner

**Primary:**
Center for Program Integrity (CPI) — Policy Analysis
Responsible for identifying billing pattern anomalies at the code level
and determining whether patterns warrant policy guidance, educational
intervention, or referral for further enforcement action.

**Supporting:**
Medicare Administrative Contractors (MAC) — Medical Review
Responsible for executing Targeted Probe and Educate reviews at the
claims level. Consumes D-POL-1 code-level findings to focus medical
review on the specific HCPCS codes identified as anomalous.

**Secondary:**
Center for Medicare (CM) — Payment Policy
Informed of code-level patterns that may indicate systemic billing
practice issues requiring payment policy review or LCD/NCD guidance.

---

## Decision Actor Tag

`POL` — Policy

---

## Decision Owner Role

CPI Policy Analyst / Medical Review Director

---

## Cadence

- Operational: Quarterly code pattern review aligned with D-PI-2
  escalation queue refresh cycle
- Strategic: Annual review of high-concern code list and peer deviation
  thresholds

---

## Decision Context

### Why This Decision Exists

D-PI-1 identified 22,400 Tier 1 providers with anomalous billing patterns
relative to peers across eight independent signals. D-PI-2 routed 3,182 of
those providers — 655 to OIG/DOJ Referral and 2,527 to Targeted Probe and
Educate — based on composite escalation scores reflecting signal breadth,
type, dollar exposure, and persistence.

Neither D-PI-1 nor D-PI-2 identified which specific HCPCS codes are driving
those anomalous patterns. The signals established that anomalies exist and
how severe they are — but not where in the provider's billing portfolio the
anomaly originates.

D-POL-1 answers that question. Without code-level scrutiny, CPI and MAC
Medical Review cannot:

- Focus TPE medical review on the specific codes most likely to reveal
  documentation or medical necessity deficiencies
- Identify whether anomalous patterns are concentrated in a small number
  of high-value codes or distributed across a provider's billing portfolio
- Distinguish between providers whose anomalies are driven by legitimate
  specialty coding complexity and those whose patterns reflect systematic
  code-level deviation from peer norms
- Develop targeted policy guidance or educational materials addressing
  the specific codes generating the highest program integrity risk

### What Happens If Wrong

**False positive code identification (flagging codes that are not anomalous):**
- MAC Medical Review focuses TPE review on codes that do not reflect
  actual billing irregularities
- Provider burden increases without corresponding program integrity benefit
- CPI policy guidance targets codes that do not warrant intervention
- Credibility of the analytical framework weakens with operational partners

**False negative code identification (missing codes that are anomalous):**
- Actual anomalous codes escape scrutiny during TPE review cycle
- Improper payment patterns continue undetected at the code level
- OIG referral packages lack specificity, reducing law enforcement utility
- Systemic code-level patterns remain unaddressed through policy cycle

**Misattribution of code anomalies to wrong specialty drivers:**
- Policy guidance targets wrong provider types
- MAC review protocols misaligned with actual risk
- Downstream D-POL-2 specialty pattern analysis receives incorrect inputs

### What Happens If Delayed

- TPE reviews proceed without code-level analytical support
- OIG referral packages lack specificity on billing code patterns
- D-POL-2 specialty pattern analysis cannot begin without D-POL-1 outputs
- Policy guidance cycle delayed by one quarter minimum

---

## Primary Uncertainties

| ID | Uncertainty | Decision Impact |
|---|---|---|
| U1 | Are code-level anomalies concentrated in a small number of HCPCS codes or broadly distributed? | Determines whether targeted or broad code scrutiny is warranted |
| U2 | Do high-concern signal flags (S5a, S5b top-code concentration) map consistently to the same HCPCS codes across providers? | Affects whether a cross-provider code list can be constructed |
| U3 | Are peer group sizes at the Provider Type × HCPCS level sufficient for reliable deviation scoring? | Affects reliability of code-level peer comparison |
| U4 | Do OIG and TPE providers share the same anomalous code patterns, or do their code profiles differ meaningfully? | Determines whether pathway-stratified analysis is required |
| U5 | Does HCPCS code specificity vary across Provider Types in ways that confound peer comparison? | Affects peer grouping design at the code level |
| U6 | Are the top anomalous codes stable across quarterly review cycles? | Affects confidence in code-level policy guidance |

---

## Analytical Approach — Four Dimensions

D-POL-1 constructs a code-level scrutiny profile for each of the 3,182
OIG + TPE providers using four independent analytical dimensions applied
at the provider × HCPCS level.

### Dimension 1 — Code Concentration (C1)
Share of each provider's total allowed dollars attributable to their
top-N HCPCS codes. Identifies whether a provider's billing is
disproportionately concentrated in a small number of codes relative
to specialty peers.

- Source: Provider × HCPCS services file joined to escalation pathway list
- Peer comparison: Provider Type peer group median code concentration
- Flag condition: Provider top-code share exceeds peer median by
  defined threshold (parameterized)
- Evidence tag: Derived (D)

### Dimension 2 — Peer Deviation by HCPCS Code (C2)
For each HCPCS code billed by a provider, allowed dollars per beneficiary
compared to Provider Type peer group median for that code. Identifies
specific codes where the provider's billing intensity is anomalous
relative to peers billing the same code.

- Source: Provider × HCPCS services file; peer group medians by
  Provider Type × HCPCS
- Flag condition: Allowed per bene for code exceeds peer median by
  defined Z-score threshold (parameterized; inherited from D-PI-1
  CORE_Z_THRESHOLD=2.0)
- Minimum peer group size enforced (parameterized)
- Evidence tag: Derived (D)

### Dimension 3 — High-Concern Signal Code Overlap (C3)
Identifies whether the specific HCPCS codes flagged in Dimension 2
correspond to the codes driving the S5a and S5b signal flags from
D-PI-1 (top HCPCS allowed share and top HCPCS payment share).
Confirms whether code-level peer deviations are the same codes
responsible for the provider's original D-PI-1 signal flags.

- Source: Z-score columns from provider_signal_scores_v1.parquet
  cross-referenced with C2 flagged codes
- Flag condition: C2 flagged code matches provider's top HCPCS code
  as identified in D-PI-1 S5a/S5b signal construction
- Evidence tag: Derived (D) / Inferred (I) for pattern interpretation

### Dimension 4 — Specialty Norm Comparison (C4)
For the highest-deviation codes identified in Dimension 2, compares
the provider's code-level billing pattern to the full national
distribution for that Provider Type. Distinguishes between providers
whose anomalous code billing is at the extreme tail of their specialty
distribution versus those whose deviation is moderate relative to
national peers.

- Source: Full provider_rollup_v1.parquet and services file
  (national Provider Type population as reference)
- Flag condition: Provider code-level allowed per bene falls above
  defined national percentile threshold (parameterized)
- Evidence tag: Derived (D)

---

## Evidence Required

- 3,182 OIG + TPE provider NPIs from escalation_pathway_v1.parquet
- Provider × HCPCS services-level data from CMS PUF services file
- Provider Type peer group median allowed per beneficiary by HCPCS code
- S5a and S5b signal construction detail from provider_signal_scores_v1.parquet
- National Provider Type distribution for top anomalous codes
- Code-level sensitivity analysis across peer deviation thresholds
- Pathway-stratified code profile comparison (OIG vs. TPE)

---

## Data Dependencies

**Primary Input Datasets:**

| File | Source | Content |
|---|---|---|
| `escalation_pathway_v1.parquet` | D-PI-2 output | 3,182 OIG + TPE provider NPIs and pathway assignments |
| `escalation_scored_v1.parquet` | D-PI-2 output | Full composite scores and dimension scores |
| `provider_signal_scores_v1.parquet` | D-PI-1 output | S5a/S5b signal flags and Z-scores |
| `provider_tiered_v1.parquet` | D-PI-1 output | Full provider population for peer reference |
| CMS PUF Services File | Raw data | Provider × HCPCS allowed dollars, payments, service counts, bene counts |

**Reference Documentation:**
- Data Dictionary: `MUP_PHY_RY25_20250312_DD_PRV_SVC_508.pdf`
- Technical Specifications: `MUP_PHY_RY25_20250312_Technical_Specifications_508.pdf`
- Methodology: `MUP_PHY_RY25_20250312_Methodology_508.pdf`

**Known Data Gaps:**

| Gap | Impact | Tag |
|---|---|---|
| No claims-level detail | Cannot assess medical necessity or documentation at code level | A |
| No patient complexity data | Code-level intensity differences may partly reflect case mix | A |
| No prior enforcement history | Cannot distinguish first-offense from repeat patterns at code level | A |
| Peer group sizes at Provider Type × HCPCS may be small | Reliability of C2 peer deviation scores varies by code rarity | A |
| Lagged data (Data Year 2023) | Code-level patterns may have evolved since reporting period | A |

---

## Analytical Requirements

- New data prep cell required: join 3,182 provider NPIs to CMS PUF
  services file to produce provider × HCPCS working dataset
- All dimension thresholds fully parameterized with documented rationale
- Minimum peer group size enforced at Provider Type × HCPCS level
- Pathway-stratified output: OIG and TPE providers analyzed jointly
  and separately
- Top anomalous codes ranked by frequency (how many providers flagged)
  and by dollar exposure (total allowed dollars at risk)
- Sensitivity analysis across C2 peer deviation threshold assumptions
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
Language is restricted to: anomalous code-level patterns, statistical
deviation, peer-adjusted billing intensity, top-code concentration,
and code-level scrutiny flags.

---

## Outputs Produced

| File | Location | Description | Tag |
|---|---|---|---|
| `hcpcs_scored_v1.parquet` | `data/processed/` | Provider × HCPCS scored dataset with C1–C4 dimension scores | D |
| `code_scrutiny_flags_v1.parquet` | `data/processed/` | Code-level flag summary by provider and pathway | D |
| `top_codes_by_frequency_v1.parquet` | `data/processed/` | HCPCS codes ranked by provider flag frequency | D |
| `top_codes_by_exposure_v1.parquet` | `data/processed/` | HCPCS codes ranked by total allowed dollar exposure | D |
| `code_concentration_v1.png` | `outputs/figures/` | Code concentration distribution by pathway | D |
| `top_flagged_codes_v1.png` | `outputs/figures/` | Top anomalous codes by frequency and dollar exposure | D |
| `D-POL-1_recommendation_memo_v1.md` | `outputs/decision_briefs/` | Stakeholder-ready code scrutiny memo | I |
| Evidence log entries E-DPOL1-01 through E-DPOL1-XX | `decisions/D-POL-1_code_scrutiny/` | O/D/I/A tagged analytical outputs | — |

---

## Decision Impact Pathway

D-PI-2 escalation pathway assignments (OIG + TPE) (O)
  -> Provider × HCPCS services file join (D)
    -> Four-dimension code scrutiny scoring (D)
      -> Top anomalous codes by provider, frequency, and exposure (D)
        -> Persistent code-level anomaly pattern (I)
          -> CPI policy guidance targets (I)
            -> MAC Medical Review TPE code focus list (D)
            -> OIG referral package code specificity (D)
            -> D-POL-2 specialty pattern analysis inputs (D)

---

## Notebook Dependencies

| Notebook | Role | Location |
|---|---|---|
| `D-POL-1_code_scrutiny.ipynb` | Code-level scrutiny scoring and policy output | `notebooks/decision/` |

**Upstream dependencies:**
- D-PI-1 signal engine outputs (provider_signal_scores_v1.parquet)
- D-PI-2 escalation outputs (escalation_pathway_v1.parquet,
  escalation_scored_v1.parquet)

**Downstream consumers:**
- D-POL-2 Specialty Pattern Shift (consumes top_codes_by_frequency_v1.parquet)
- OIG referral packages (consumes code_scrutiny_flags_v1.parquet)
- MAC TPE review protocols (consumes code_scrutiny_flags_v1.parquet)

---

## Governance Footnote

This charter reflects decision framing established February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
UPIC and MAC operational structure references sourced from CMS Program
Integrity Manual, Chapter 4, and OIG UPIC evaluation reports.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
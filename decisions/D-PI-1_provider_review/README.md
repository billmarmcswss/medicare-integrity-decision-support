# Decision Charter — D-PI-1 Provider Review Entry

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

Which providers should enter the program integrity review queue based on
defensible, evidence-supported signals of anomalous utilization or billing
patterns relative to peer providers?

---

## Decision Owner

**Primary:**
Center for Program Integrity (CPI) — Analytics and Enforcement units
responsible for identifying and acting on provider-level program integrity risks.

**Supporting:**
Office of Enterprise Data and Analytics (OEDA) — Responsible for data
governance, analytical standards, and measurement methodology that informs
integrity signals.

**Secondary:**
Center for Medicare (CM) — Payment policy context and provider type
classification standards that define peer grouping logic.

---

## Decision Actor Tag

`PI` — Program Integrity

---

## Decision Owner Role

Investigations Lead / Program Integrity Analytics Manager

---

## Cadence

- Operational: Monthly review queue refresh
- Strategic: Quarterly threshold calibration review

---

## Operational Action

Select and prioritize providers (identified by NPI) for one of the following
actions based on evidence-supported anomaly signals:

- Tier 1 — Initiate focused medical review or audit
- Tier 2 — Place under active monitoring
- Tier 3 — Flag for passive surveillance
- No Flag — No action warranted at this time

---

## Decision Context

### Why This Decision Exists

CMS program integrity resources are finite. The Medicare Part B program covers
over one million rendering providers billing across thousands of procedure codes
nationally. Manual review of all providers is operationally impossible.

A structured, reproducible, evidence-based method is required to identify which
providers warrant review based on objective signals rather than reactive
complaint-driven selection.

Without this decision support, enforcement resources risk being:
- Misallocated to low-risk providers
- Delayed in reaching high-risk patterns
- Inconsistent across review cycles
- Indefensible under audit or legal scrutiny

### What Happens If Wrong

**False Positives (flagging legitimate providers):**
- Unnecessary administrative burden on providers
- Erosion of provider trust in CMS oversight
- Consumption of enforcement capacity on low-yield reviews
- Potential legal or reputational exposure

**False Negatives (missing high-risk providers):**
- Continued improper payment exposure
- Delayed identification of systemic billing patterns
- Reduced program integrity effectiveness
- Financial harm to the Medicare Trust Fund

### What Happens If Delayed

- Anomalous billing patterns persist undetected
- Payment exposure compounds over reporting cycles
- Systemic patterns become entrenched before intervention
- Enforcement credibility weakens

---

## Primary Uncertainties

| ID | Uncertainty | Decision Impact |
|---|---|---|
| U1 | What constitutes meaningful anomaly vs legitimate specialty variation? | Changes who gets flagged |
| U2 | How much variation is explained by patient mix vs provider behavior? | Affects false positive rate |
| U3 | Which signals persist across metrics vs appearing in only one? | Drives tier assignment confidence |
| U4 | What thresholds balance detection with operational review capacity? | Determines queue size |
| U5 | Are outliers artifacts of data suppression or real patterns? | Affects ranking reliability |

---

## Evidence Required

- Peer-adjusted utilization patterns (services per beneficiary)
- Payment intensity deviations (allowed and payment per beneficiary)
- Submitted-to-allowed charge ratios
- Code concentration signals (top HCPCS share)
- Allowed vs payment gap signals
- Multi-signal agreement across independent metrics
- Minimum volume reliability filters

---

## Data Dependencies

**Primary Dataset:**
CMS Medicare Part B Public Use File — Provider and Service Level
`MUP_PHY_R25_P05_V20_D23_Prov_Svc.csv`
Reporting Year 2025 / Data Year 2023

**Reference Documentation:**
- Data Dictionary: `MUP_PHY_RY25_20250312_DD_PRV_SVC_508.pdf`
- Technical Specifications: `MUP_PHY_RY25_20250312_Technical_Specifications_508.pdf`
- Methodology: `MUP_PHY_RY25_20250312_Methodology_508.pdf`

**Known Data Gaps:**
- No patient-level clinical or risk data
- No claims-level detail
- No intent or compliance indicators
- No real-time billing data (lagged reporting cycle)
- Patient complexity proxied via service intensity (Assumed — A)

---

## Analytical Requirements

- Peer grouping by Provider Type (primary) with State and RUCA
  as secondary diagnostic controls
- Outlier detection via peer-adjusted Z-scores across 8 independent signals
- Multi-signal agreement framework (Tier 1 requires >= 3 signals flagged)
- Minimum beneficiary volume filter (Tot_Benes >= 11)
- Prioritization by allowed dollars within each tier

---

## Output Tagging Standard

All outputs carry one of the following evidence tags:

| Tag | Meaning |
|---|---|
| O | Observed — directly in the data |
| D | Derived — mathematically constructed from observed data |
| I | Inferred — evidence-based pattern interpretation |
| A | Assumed — declared gap-filling for decision support |

**No output may assert provider intent, fraud, or wrongdoing.**
Language is restricted to: anomalous patterns, statistical deviation,
billing intensity signals, and peer-adjusted outliers.

---

## Outputs Produced

- `provider_review_queue_v1.parquet` — Ranked Tier 1 provider list (D)
- `provider_tiered_v1.parquet` — Full provider tier assignments (D)
- `provider_signal_scores_v1.parquet` — Peer-adjusted Z-scores (D)
- `signal_validation_v1.png` — Visual validation of signal distribution (D)
- Evidence log entries E-DPI1-00 through E-DPI1-04

---

## Decision Impact Pathway

Observed utilization (O)
  -> Derived peer-adjusted signals (D)
    -> Persistent multi-signal anomalies (I)
      -> Tiered review prioritization (D)
        -> Operational investigation queue
          -> Audit / Monitor / Education / No Action

---

## Notebook Dependencies

| Notebook | Role | Cell IDs |
|---|---|---|
| `00_data_intake_and_validation.ipynb` | Ingestion + parquet foundation | DPI1-00-* |
| `01_peer_grouping_design.ipynb` | Peer group definition + lock | DPI1-01-* |
| `02_signal_engine.ipynb` | Signal computation + tiering | DPI1-02-* |

---

## Governance Footnote

This charter reflects decision framing established February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.

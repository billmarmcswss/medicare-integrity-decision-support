# Decision Charter — D-OPS-1 Monitoring Placement

**Version:** 1.0
**Date:** March 2026
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

Which providers in the D-PI-1 Tier 1 review queue — including those assigned
to all three D-PI-2 escalation pathways — warrant placement on an active
monitoring roster, and at what monitoring intensity level, based on their
composite risk profile, escalation pathway assignment, and code-level scrutiny
findings from prior modules?

---

## Decision Owner

**Primary:**
Unified Program Integrity Contractors (UPIC) — Operations units responsible
for maintaining active monitoring rosters, coordinating surveillance activity,
and tracking provider billing behavior between formal review cycles.

**Supporting:**
Center for Program Integrity (CPI) — Monitoring Unit responsible for
oversight of UPIC monitoring activity, escalation triggers, and monitoring
roster governance.

**Secondary:**
Medicare Administrative Contractors (MAC) — Medical Review units that
execute TPE reviews and report findings back to UPIC for monitoring
roster updates.

---

## Decision Actor Tag

`OPS` — Operations

---

## Decision Owner Role

UPIC Operations Manager / CPI Monitoring Unit Lead

---

## Cadence

- Operational: Monthly monitoring roster review aligned with D-PI-1
  review queue refresh cycle
- Strategic: Quarterly monitoring intensity threshold calibration review

---

## Monitoring Intensity Levels

Providers placed on the monitoring roster are assigned one of three
intensity levels based on their composite monitoring placement score:

- **Level 1 — Active Surveillance:** Highest monitoring intensity.
  Providers assigned to OIG/DOJ Referral pathway in D-PI-2 who remain
  under active billing review pending investigation outcome. Billing
  patterns tracked monthly. Anomaly recurrence triggers escalation notice
  to UPIC Case Development.

- **Level 2 — Periodic Review:** Mid-level monitoring intensity.
  Providers assigned to Targeted Probe and Educate pathway in D-PI-2
  whose TPE review is in progress or recently completed. Billing patterns
  tracked quarterly. Material change in pattern triggers re-entry to
  escalation scoring.

- **Level 3 — Passive Surveillance:** Lowest monitoring intensity.
  Providers assigned to Provider Education Letter pathway in D-PI-2
  and D-PI-1 Tier 2 providers not routed through D-PI-2. Billing patterns
  tracked semi-annually. Sustained anomaly pattern triggers promotion
  to Level 2.

---

## Decision Context

### Why This Decision Exists

D-PI-1 identified 22,400 Tier 1 providers with anomalous billing patterns.
D-PI-2 routed 3,182 of those providers to OIG/DOJ Referral or Targeted
Probe and Educate pathways. D-POL-1 identified the specific HCPCS codes
driving anomalous patterns among those 3,182 providers.

None of those modules answers what happens next — operationally — to the
providers who have been identified, escalated, and code-scrutinized. Program
integrity enforcement is not a single event. Providers whose patterns prompted
escalation require ongoing monitoring to:

- Detect whether anomalous billing patterns persist, resolve, or worsen
  following intervention
- Ensure that OIG referral cases under active investigation are tracked
  and not lost in operational handoffs
- Identify providers whose Education Letter response is non-compliant —
  sustained anomaly patterns following education warrant promotion to TPE
- Provide UPIC Operations with a structured, prioritized roster rather
  than an unmanaged list of flagged providers

Without monitoring placement, the upstream analytical work in D-PI-1
through D-POL-1 produces a one-time snapshot with no operational continuity.

### What Happens If Wrong

**Over-placement (assigning too many providers to active monitoring):**
- UPIC monitoring capacity consumed on providers who have resolved
  their anomalous billing patterns
- Monitoring roster becomes unmanageable, reducing effectiveness
  of surveillance for highest-risk providers
- Administrative burden on MAC and UPIC staff without corresponding
  program integrity benefit

**Under-placement (missing providers who warrant monitoring):**
- Anomalous billing patterns persist undetected between formal review cycles
- OIG referral cases lose operational continuity during investigation period
- Education Letter providers with non-compliant patterns escape re-escalation
- Program integrity enforcement becomes episodic rather than continuous

**Misclassification of monitoring intensity:**
- Level 1 resources deployed to providers requiring only passive surveillance
- Level 3 passive surveillance applied to providers requiring active tracking
- Monitoring roster stratification loses operational meaning

### What Happens If Delayed

- D-PI-2 escalation routing produces pathway assignments with no
  downstream operational follow-through
- Providers flagged for OIG referral exit the analytical pipeline
  without structured monitoring continuity
- Monthly review queue refresh cycles occur without reference to
  prior-cycle provider behavior
- Enforcement credibility weakens as identified providers are not
  systematically tracked

---

## Primary Uncertainties

| ID | Uncertainty | Decision Impact |
|---|---|---|
| U1 | Does escalation pathway assignment alone adequately stratify monitoring intensity, or do composite score and code-level findings add independent information? | Determines whether D-PI-2 pathway is sufficient or a new composite score is needed |
| U2 | What monitoring intensity level is appropriate for D-PI-1 Tier 2 providers not routed through D-PI-2? | Affects roster scope — Tier 2 inclusion vs. Tier 1 only |
| U3 | How should providers with all three D-OPS-1 dimensions firing be differentiated from those with only one? | Drives monitoring intensity tier boundary design |
| U4 | Are code-level findings from D-POL-1 sufficiently discriminating to affect monitoring placement independent of escalation pathway? | Affects whether D-POL-1 outputs are incorporated as a scoring dimension |
| U5 | What monitoring roster size is operationally sustainable for UPIC monthly review? | Determines whether scope must be bounded by capacity constraints |

---

## Composite Monitoring Placement Score — Three Dimensions

The monitoring placement score is a parameterized weighted composite of
three independent analytical dimensions. All dimension weights and threshold
values are parameterized with documented rationale.

### Dimension 1 — Escalation Pathway Weight (M1)
Escalation pathway assignment from D-PI-2 expressed as a normalized score.
Higher pathway severity = higher M1 contribution.

- Source: `escalation_pathway_v1.parquet` — pathway column
- Scoring: OIG/DOJ Referral = 1.0 | TPE = 0.67 | Education Letter = 0.33
  | Not in D-PI-2 (Tier 2 only) = 0.10
- Evidence tag: Derived (D)

### Dimension 2 — Composite Escalation Score Carry-Forward (M2)
D-PI-2 composite escalation score carried forward directly as a monitoring
dimension. Captures the full multi-dimensional risk signal from D-PI-2
without re-deriving it.

- Source: `escalation_scored_v1.parquet` — composite_score column
- For Tier 2 providers not in D-PI-2: assign NEUTRAL_SCORE = 0.50
- Evidence tag: Derived (D) / Assumed (A) for Tier 2 neutral assignments

### Dimension 3 — Code-Level Scrutiny Intensity (M3)
Count of active D-POL-1 dimensions firing (C2, C3, C4) for providers
in the OIG + TPE population, normalized to [0, 1]. For providers not
in D-POL-1 scope (Education Letter and Tier 2), assign NEUTRAL_SCORE = 0.50.

- Source: `code_scrutiny_flags_v1.parquet` — dimensions_flagged column
- Normalization: dimensions_flagged / 3 (max active dimensions)
- For out-of-scope providers: NEUTRAL_SCORE = 0.50
- Evidence tag: Derived (D) / Assumed (A) for out-of-scope neutral assignments

---

## Evidence Required

- D-PI-1 Tier 1 and Tier 2 provider lists with tier assignments
- D-PI-2 escalation pathway assignments and composite scores
- D-POL-1 code-level scrutiny flag counts by provider
- Composite monitoring placement score distribution analysis
- Monitoring intensity level threshold calibration from distribution
- Roster size estimates by monitoring intensity level
- Dollar exposure by monitoring intensity level

---

## Data Dependencies

**Primary Input Datasets:**

| File | Source | Content |
|---|---|---|
| `provider_tiered_v1.parquet` | D-PI-1 output | Full tier assignments including Tier 2 |
| `escalation_pathway_v1.parquet` | D-PI-2 output | Pathway assignments for Tier 1 providers |
| `escalation_scored_v1.parquet` | D-PI-2 output | Composite escalation scores |
| `code_scrutiny_flags_v1.parquet` | D-POL-1 output | Provider-level dimension flag counts |

**Known Data Gaps:**

| Gap | Impact | Tag |
|---|---|---|
| No prior enforcement history | Cannot assess whether provider is repeat vs. first-time flag | A |
| No TPE review outcome data | Cannot confirm whether TPE resolved or escalated the anomaly | A |
| No real-time billing data | Monitoring is retrospective to Data Year 2023 | A |
| Tier 2 providers lack D-PI-2 composite scores | Neutral score assignment required | A |
| Education Letter providers lack D-POL-1 code scrutiny data | Neutral score assignment required | A |

---

## Analytical Requirements

- Composite monitoring placement score constructed from three parameterized
  dimensions with documented weights and rationale
- Tier 2 provider inclusion requires explicit scope decision and
  documented rationale
- Monitoring intensity thresholds calibrated from score distribution
  (histogram + percentile table)
- Roster size and dollar exposure reported by monitoring intensity level
- Sensitivity analysis across monitoring intensity threshold assumptions
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
Language is restricted to: monitoring placement score, monitoring intensity
level, anomalous billing pattern persistence, escalation pathway carry-forward,
and code-level scrutiny signal.

---

## Outputs Produced

| File | Location | Description | Tag |
|---|---|---|---|
| `monitoring_scored_v1.parquet` | `data/processed/` | Full provider population with M1–M3 scores and composite monitoring score | D |
| `monitoring_roster_v1.parquet` | `data/processed/` | Monitoring roster with intensity level assignments | D |
| `monitoring_distribution_v1.png` | `outputs/figures/` | Composite monitoring score distribution by intensity level | D |
| `monitoring_roster_summary_v1.png` | `outputs/figures/` | Roster composition by intensity level — provider count and dollar exposure | D |
| `D-OPS-1_recommendation_memo_v1.md` | `outputs/decision_briefs/` | Stakeholder-ready monitoring placement memo | I |
| Evidence log entries E-DOPS1-01 through E-DOPS1-XX | `decisions/D-OPS-1_monitoring_placement/` | O/D/I/A tagged analytical outputs | — |

---

## Decision Impact Pathway

D-PI-1 tier assignments + D-PI-2 pathway assignments + D-POL-1 code flags (O/D)
  -> Three-dimension composite monitoring placement score (D)
    -> Monitoring intensity level assignment (D)
      -> Structured monitoring roster (D)
        -> Level 1 — Active Surveillance (OIG referral tracking)
        -> Level 2 — Periodic Review (TPE follow-through)
        -> Level 3 — Passive Surveillance (Education Letter compliance)

---

## Notebook Dependencies

| Notebook | Role | Location |
|---|---|---|
| `D-OPS-1_monitoring_placement.ipynb` | Monitoring score + roster assignment | `notebooks/decision/` |

**Upstream dependencies:**
- D-PI-1 signal engine outputs (provider_tiered_v1.parquet)
- D-PI-2 escalation outputs (escalation_pathway_v1.parquet, escalation_scored_v1.parquet)
- D-POL-1 code scrutiny outputs (code_scrutiny_flags_v1.parquet)

**Downstream consumers:**
- D-OPS-2 Resource Allocation (consumes monitoring_roster_v1.parquet)

---

## Governance Footnote

This charter reflects decision framing established March 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
UPIC operational structure references sourced from CMS Program Integrity
Manual, Chapter 4, and OIG UPIC evaluation reports.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
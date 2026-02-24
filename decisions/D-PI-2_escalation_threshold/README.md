# Decision Charter — D-PI-2 Escalation Threshold

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

Which providers in the D-PI-1 Tier 1 review queue meet the threshold for
escalation to one of three industry-standard CMS/UPIC enforcement pathways,
based on a composite score derived from four independent analytical dimensions?

---

## Decision Owner

**Primary:**
Unified Program Integrity Contractors (UPIC) — Case Development units
responsible for investigating, developing, and routing program integrity
cases to appropriate enforcement channels.

**Supporting:**
Center for Program Integrity (CPI) — Enforcement units responsible for
authorizing administrative actions and coordinating with law enforcement
referral pathways.

**Secondary:**
Office of Inspector General (OIG) — Referral recipient for cases meeting
the highest escalation threshold warranting law enforcement consideration.

---

## Decision Actor Tag

`PI` — Program Integrity

---

## Decision Owner Role

Investigations Lead / UPIC Case Development Manager

---

## Cadence

- Operational: Monthly escalation queue refresh aligned with D-PI-1 review
  queue refresh cycle
- Strategic: Quarterly composite score threshold calibration review

---

## Escalation Pathways

Providers in the D-PI-1 Tier 1 review queue are routed to one of three
industry-standard CMS/UPIC enforcement pathways based on composite
escalation score:

- **OIG/DOJ Referral** — Case development for law enforcement referral.
  Highest composite score threshold. Reserved for providers with persistent,
  multi-dimensional anomaly patterns and significant dollar exposure relative
  to peers. UPIC prepares detailed investigative report for OIG or DOJ.

- **Targeted Probe and Educate (TPE)** — Focused medical review of specific
  claims conducted by MAC Medical Review staff. Mid-level composite score
  threshold. Provider billing records reviewed for medical necessity,
  documentation, and coding compliance. Results inform further action
  or education.

- **Provider Education Letter** — Education and monitoring. Lowest composite
  score threshold within Tier 1. Provider notified of anomalous billing
  patterns relative to peers. No formal audit initiated. Monitoring
  continues through subsequent review cycles.

---

## Decision Context

### Why This Decision Exists

The D-PI-1 Provider Review Entry decision produces a ranked queue of 500
Tier 1 providers representing $1.77 billion in allowed dollars. Program
integrity enforcement resources — UPIC investigators, MAC medical reviewers,
and OIG case development capacity — are finite and cannot be deployed equally
across all 500 providers simultaneously.

A structured, reproducible, evidence-based method is required to route each
provider to the appropriate enforcement pathway based on the weight, breadth,
and persistence of their anomaly signals — not solely on dollar exposure rank.

Without this escalation decision support, enforcement resources risk being:
- Deployed to OIG referral for providers whose patterns do not warrant
  law enforcement consideration
- Underdeployed to TPE for providers whose billing patterns require
  focused medical review
- Applied inconsistently across review cycles
- Indefensible under appeal or legal scrutiny

### What Happens If Wrong

**Over-escalation (routing providers to OIG/DOJ Referral incorrectly):**
- Consumption of scarce OIG and DOJ investigative capacity on low-yield cases
- Unnecessary administrative and legal burden on providers
- Erosion of referral credibility with law enforcement partners
- Potential legal exposure from unsupported referrals

**Under-escalation (routing providers to Provider Education Letter incorrectly):**
- High-risk billing patterns continue undetected through enforcement cycle
- Continued improper payment exposure to Medicare Trust Fund
- Systemic patterns become entrenched before intervention
- Enforcement credibility weakens with CPI and OIG oversight

**Misrouting to TPE incorrectly:**
- MAC medical review capacity consumed on providers requiring either
  stronger or weaker enforcement response
- Review results may not align with actual risk level
- Downstream routing decisions compromised

### What Happens If Delayed

- D-PI-1 review queue providers remain unrouted pending escalation decision
- Enforcement capacity sits idle or is deployed without analytical support
- Payment exposure compounds over reporting cycles
- Systemic patterns persist undetected

---

## Primary Uncertainties

| ID | Uncertainty | Decision Impact |
|---|---|---|
| U1 | Are behavioral signals (S4, S5) truly more indicative of anomalous patterns than intensity signals (S1, S2)? | Changes signal type composition weighting |
| U2 | Does signal persistence across peer subgroups materially improve escalation accuracy? | Affects Dimension 4 weight in composite score |
| U3 | What composite score thresholds correctly separate OIG/DOJ Referral from TPE? | Determines pathway boundary cutpoints |
| U4 | Does peer-relative dollar exposure add independent information beyond raw allowed dollars? | Affects Dimension 3 construction |
| U5 | Are subgroup peer comparisons stable enough across State and RUCA classifications? | Affects Dimension 4 reliability |
| U6 | How sensitive is pathway routing to composite score weight assumptions? | Drives sensitivity analysis requirements |

---

## Composite Escalation Score — Four Dimensions

The escalation score is a parameterized weighted composite of four
independent analytical dimensions. All dimension weights and threshold
values are parameterized with documented rationale. No hardcoded values
without explicit annotation.

### Dimension 1 — Signal Breadth (D1)
Count of signals flagged at or above the core Z-score threshold across
all 8 independent peer-adjusted signals from D-PI-1.

- Source: `signals_flagged` field in `provider_tiered_v1.parquet`
- Range for Tier 1 providers: 3 to 8
- Higher signal count = higher D1 contribution to composite score

### Dimension 2 — Signal Type Composition (D2)
Count of high-concern behavioral signals flagged among those in Dimension 1.

High-concern signals reflect billing behavior choices rather than volume
patterns and are considered more analytically significant for escalation
purposes:

| Signal | Type | Escalation Concern Level |
|---|---|---|
| S4_submitted_to_allowed_ratio | Pricing Behavior | High |
| S5a_top1_allowed_share | Coding Pattern | High |
| S5b_top1_payment_share | Coding Pattern | High |
| S1_allowed_per_bene | Payment Intensity | Moderate |
| S2_payment_per_bene | Payment Intensity | Moderate |
| S3_services_per_bene | Utilization | Moderate |
| S6_payment_to_allowed_ratio | Gap Signal | Moderate |
| S7_allowed_minus_payment_per_bene | Gap Signal | Moderate |

- Source: Z-score columns in `provider_signal_scores_v1.parquet`
- Range: 0 to 3 high-concern signals flagged
- Higher high-concern signal count = higher D2 contribution

### Dimension 3 — Peer-Relative Dollar Exposure (D3)
Provider allowed dollars expressed as a ratio to their Provider Type
peer group median allowed dollars. Normalizes for specialty-level
billing differences.

- Source: `allowed_dollars_sum` and peer group median derived from
  `provider_rollup_v1.parquet`
- Constructed as: provider allowed dollars / peer group median allowed dollars
- Higher peer-relative exposure = higher D3 contribution
- Evidence tag: Derived (D)

### Dimension 4 — Signal Persistence Across Peer Subgroups (D4)
Count of peer subgroup classifications under which the provider remains
anomalous. Tests whether anomaly patterns persist when peer grouping
controls shift from Provider Type alone to State and RUCA classifications.

Subgroup classifications tested:
- Primary: Provider Type (inherited from D-PI-1)
- Secondary: State (Rndrng_Prvdr_State_Abrvtn)
- Tertiary: RUCA classification (Rndrng_Prvdr_RUCA)

- Source: `provider_signal_scores_v1.parquet` with subgroup re-scoring
- Range: 0 to 2 additional subgroup classifications confirming anomaly
- Higher persistence = higher D4 contribution
- Evidence tag: Derived (D)

---

## Evidence Required

- D-PI-1 Tier 1 provider list with signal scores and tier assignments
- Peer-adjusted Z-scores across all 8 signals
- Provider Type peer group median allowed dollars
- State and RUCA subgroup peer re-scoring
- Composite score sensitivity analysis across weight assumptions
- Pathway distribution analysis (how many providers route to each pathway)
- Dollar exposure by pathway (what share of $1.77B routes to each pathway)

---

## Data Dependencies

**Primary Input Datasets (from D-PI-1 outputs):**
- `data\processed\provider_review_queue_v1.parquet` — Ranked Tier 1 providers
- `data\processed\provider_signal_scores_v1.parquet` — Peer-adjusted Z-scores
- `data\processed\provider_tiered_v1.parquet` — Full tier assignments
- `data\processed\provider_rollup_v1.parquet` — Provider-level rollup with signals

**Reference Documentation:**
- Data Dictionary: `MUP_PHY_RY25_20250312_DD_PRV_SVC_508.pdf`
- Technical Specifications: `MUP_PHY_RY25_20250312_Technical_Specifications_508.pdf`
- Methodology: `MUP_PHY_RY25_20250312_Methodology_508.pdf`

**Known Data Gaps:**
- No patient-level clinical or risk data
- No claims-level detail
- No intent or compliance indicators
- No real-time billing data (lagged reporting cycle)
- No prior enforcement history for cross-referencing
- Subgroup peer groups may be small for rare Provider Types in low-population
  states or rural RUCA classifications (Assumed — A)

---

## Analytical Requirements

- Composite escalation score constructed from four parameterized dimensions
- All dimension weights fully parameterized with documented rationale
- Sensitivity analysis required across composite score weight assumptions
- Pathway boundary cutpoints parameterized and documented
- Subgroup re-scoring for Dimension 4 using State and RUCA classifications
- Pathway distribution output showing provider count and dollar exposure
  by escalation pathway
- Colorblind-accessible visualizations throughout using IBM Carbon
  accessible palette

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
billing intensity signals, peer-adjusted outliers, and composite
escalation score thresholds.

---

## Outputs Produced

- `escalation_scores_v1.parquet` — Composite escalation scores by provider (D)
- `escalation_routing_v1.parquet` — Pathway assignments for Tier 1 providers (D)
- `subgroup_persistence_v1.parquet` — Dimension 4 subgroup re-scoring results (D)
- `escalation_sensitivity_v1.png` — Sensitivity analysis visualization (D)
- `escalation_pathway_distribution_v1.png` — Pathway routing distribution (D)
- `D-PI-2_recommendation_memo_v1.md` — Stakeholder-ready escalation memo (I)
- Evidence log entries E-DPI2-00 through E-DPI2-05

---

## Decision Impact Pathway

D-PI-1 Tier 1 review queue (O)
  -> Composite escalation score across 4 dimensions (D)
    -> Persistent multi-dimensional anomaly pattern (I)
      -> Pathway routing assignment (D)
        -> OIG/DOJ Referral
        -> Targeted Probe and Educate (TPE)
        -> Provider Education Letter

---

## Notebook Dependencies

| Notebook | Role | Cell IDs |
|---|---|---|
| `notebooks\decision\D-PI-2_escalation_threshold.ipynb` | Composite score + pathway routing | DPI2-01-* |

---

## Governance Footnote

This charter reflects decision framing established February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
UPIC operational structure references sourced from CMS Program Integrity
Manual, Chapter 4, and OIG UPIC evaluation reports.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.

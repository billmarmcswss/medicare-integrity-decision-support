# Decision Charter — D-OPS-2 Resource Allocation

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

Across the 61,416 providers on the D-OPS-1 monitoring roster — stratified
simultaneously by monitoring intensity level (L1/L2/L3) and escalation
pathway (OIG/TPE/EdLetter/Tier2Only) — how should program integrity
investigative and review resources be allocated to maximize enforcement
impact per unit of operational capacity?

The decision is expressed across four resource denominator views:
Review Hours, FTE Capacity, Case Slots, and Abstract Capacity Units.
Each view produces an independent prioritization ranking of level × pathway
cells. The Decision Owner selects the denominator view appropriate to their
operational context.

---

## Decision Owner

**Primary:**
Center for Program Integrity (CPI) — Director / Program Integrity Resource
Manager responsible for UPIC contract oversight, resource allocation guidance,
and enforcement priority-setting across MAC and UPIC operational units.

**Supporting:**
Unified Program Integrity Contractors (UPIC) — Operations units responsible
for executing monitoring activity and managing investigative caseloads within
allocated capacity.

**Secondary:**
Medicare Administrative Contractors (MAC) — Medical Review units that
execute TPE reviews; capacity constraints at MAC level affect L2 cell
throughput and must be factored into allocation decisions.

---

## Decision Actor Tag

`OPS` — Operations

---

## Decision Owner Role

CPI Director / Program Integrity Resource Manager

---

## Cadence

- Operational: Quarterly resource allocation review aligned with UPIC
  contract performance cycle
- Strategic: Annual allocation model recalibration when new data year
  becomes available

---

## Decision Context

### Why This Decision Exists

D-OPS-1 produced a structured monitoring roster of 61,416 providers
assigned to three monitoring intensity levels:

- Level 1 — Active Surveillance: 1,162 providers | $0.51B allowed
- Level 2 — Periodic Review: 1,613 providers | $0.26B allowed
- Level 3 — Passive Surveillance: 58,641 providers | $6.35B allowed

The roster answers where each provider sits in the monitoring hierarchy.
It does not answer how much investigative capacity to direct to each
level, or how to prioritize within levels when pathway composition differs.

Program integrity resources — investigative hours, analyst FTE, formal
review case slots — are finite. The 1,162 Level 1 providers alone
represent a material operational commitment. Allocating capacity without
analytical grounding risks concentrating resources on high-count,
lower-risk cells while under-resourcing high-risk, lower-count cells,
and ignoring the pathway composition within each level (e.g., the 556
TPE-spillover providers in Level 1 have different operational needs
than the 606 OIG-pathway Level 1 providers).

D-OPS-2 bridges that gap by producing a data-driven resource allocation
model that simultaneously accounts for monitoring level and pathway
composition, with outputs expressed in four operationally meaningful
denominator views.

### Level × Pathway Cell Structure

The allocation model operates on level × pathway cells — the intersection
of monitoring intensity level and escalation pathway assignment. Nine
analytically meaningful cells exist:

| Cell         | Level   | Pathway            | Note                                              |
|--------------|---------|--------------------|---------------------------------------------------|
| L1-OIG       | Level 1 | OIG/DOJ Referral   | Primary investigative commitment                  |
| L1-TPE       | Level 1 | TPE                | TPE spillover into L1 — documented in DL-DOPS1-10|
| L1-EdLetter  | Level 1 | Education Letter   | Edge case — score-driven placement                |
| L2-OIG       | Level 2 | OIG/DOJ Referral   | OIG providers below L1 threshold — DL-DOPS1-09   |
| L2-TPE       | Level 2 | TPE                | Core TPE monitoring population                    |
| L2-EdLetter  | Level 2 | Education Letter   | Elevated EdLetter providers                       |
| L3-OIG       | Level 3 | OIG/DOJ Referral   | Low-score OIG tail                                |
| L3-TPE       | Level 3 | TPE                | Low-score TPE tail                                |
| L3-EdLetter/Tier2 | Level 3 | EdLetter + Tier2Only | Passive surveillance mass                    |

Cell counts and dollar exposure are derived from monitoring_roster_v1.parquet
in the D-OPS-2 notebook.

### What Happens If Wrong

**Over-allocation to lower-risk cells:**
- Investigative capacity consumed on Level 3 passive surveillance providers
  at the expense of Level 1 active surveillance follow-through
- OIG referral case momentum lost
- TPE review cycle throughput constrained by MAC capacity diverted to
  lower-priority cells

**Under-allocation to high-risk cells:**
- Level 1 OIG providers tracked without adequate investigative staffing
- TPE-spillover Level 1 providers treated as standard TPE when composite
  scores indicate elevated risk
- Program integrity enforcement yield reduced relative to available capacity

**Ignoring pathway composition within levels:**
- L1 resource allocation undifferentiated between OIG and TPE-spillover
  providers — operationally distinct populations with distinct capacity needs
- L2 OIG providers (49 providers per DL-DOPS1-09) receive standard periodic
  review capacity rather than case-development supplemental allocation

### What Happens If Delayed

- D-OPS-1 monitoring roster becomes an unactioned list
- UPIC and MAC operational units allocate resources by internal heuristic
  rather than composite-score-driven analytical guidance
- CPI contract oversight lacks quantitative basis for UPIC performance
  benchmarking
- Downstream D-EXT-1 Safe Reporting outputs lack resource context for
  prioritization framing

---

## Primary Uncertainties

| ID | Uncertainty                                                                 | Decision Impact                                                                 |
|----|-----------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| U1 | What is the actual per-provider review burden at each monitoring level?     | Drives review hours and FTE denominator assumptions — must be asserted (A)      |
| U2 | How many formal case slots does UPIC sustain per contract cycle?            | Drives case slot denominator capacity ceiling — must be asserted (A)            |
| U3 | Does the level × pathway cell structure capture operationally distinct populations? | Determines whether nine-cell model is right granularity or cells should merge |
| U4 | How should dollar exposure weight relative to provider count?               | Drives R3 dimension weight                                                      |
| U5 | Are ACUs interpretable for Decision Owner use without operational mapping?  | Determines whether ACU view requires a translation note                         |

---

## Resource Allocation Model — Four-Denominator Architecture

### Stage 1 — Core Allocation Engine

A parameterized composite allocation score is constructed for each
level × pathway cell using three dimensions. The core score is
denominator-agnostic and feeds all four Stage 2 views.

#### Dimension 1 — Monitoring Intensity Weight (R1)
Cell monitoring intensity level expressed as a normalized weight.

- Source: monitoring_roster_v1.parquet — monitoring_level column
- Scoring: Level 1 = 1.00 | Level 2 = 0.67 | Level 3 = 0.33
- Evidence tag: Derived (D)
- Rationale: Mirrors M1 pathway weight structure from D-OPS-1 for
  internal consistency across modules.

#### Dimension 2 — Provider Count Concentration (R2)
Share of total roster providers in each cell, normalized to [0, 1]
relative to the maximum cell count.

- Source: monitoring_roster_v1.parquet — cell-level provider count
- Normalization: cell_count / max_cell_count across all cells
- Evidence tag: Derived (D)
- Rationale: Larger provider populations generate proportionally
  greater review burden regardless of intensity level.

#### Dimension 3 — Dollar Exposure Concentration (R3)
Share of total monitored allowed dollars in each cell, normalized to
[0, 1] relative to the maximum cell exposure.

- Source: monitoring_roster_v1.parquet — allowed dollars by cell
- Normalization: cell_allowed / max_cell_allowed across all cells
- Evidence tag: Derived (D)
- Rationale: Dollar exposure reflects program integrity financial
  stakes independent of provider count.

**Composite Allocation Score:**
Weighted sum of R1, R2, R3.
Default weights: R1=0.50, R2=0.25, R3=0.25.
Weights parameterized in CONFIG — rationale documented in decision log.

---

### Stage 2 — Four Denominator Views

#### View A — Review Hours
Estimated investigative review hours per provider by monitoring level,
multiplied by cell provider count.

- Default assumptions: L1 = 8.0 hrs | L2 = 4.0 hrs | L3 = 0.5 hrs
- Evidence tag: Assumed (A)
- Output: Total estimated review hours per cell; percentage share of
  total estimated review burden

#### View B — FTE Capacity
Review hours translated into FTE equivalents.

- FTE translation: total_cell_hours / ANNUAL_PRODUCTIVE_HOURS
- Default assumption: ANNUAL_PRODUCTIVE_HOURS = 1,800
- Evidence tag: Assumed (A)
- Output: Estimated FTE requirement per cell; recommended FTE share
  of total modeled FTE demand

#### View C — Case Slots
Formal review actions per cell bounded by total UPIC case slot capacity.

- Case slot translation: cell_providers × CASE_SLOT_RATE_BY_LEVEL
- Default assumptions: L1 = 1.00 | L2 = 0.50 | L3 = 0.05
- UPIC_TOTAL_CASE_CAPACITY default: 2,500 per cycle
- Evidence tag: Assumed (A)
- Output: Estimated case slots consumed per cell; recommended case
  slot share of total capacity

#### View D — Abstract Capacity Units (ACU)
Denominator-free relative burden index. No operational assumptions required.

- ACU = (cell_composite_score / sum_all_composite_scores) × 100
- Evidence tag: Derived (D)
- Output: ACU share per cell; cumulative ACU by priority rank
- Use: Recommended primary view for external users or Decision Owners
  who cannot validate operational assumptions in Views A–C

---

## Decision Notes by Denominator View

Each denominator view is accompanied in the recommendation memo by a
brief Decision Note stating the recommended allocation. Tagged (I).

Format:
> Based on [denominator] analysis, we recommend allocating [X]% of
> [resource unit] to [top N cells], with [cell name] receiving the
> highest priority allocation at [value]. Assumptions are declared
> in CONFIG and tagged (A). Decision Owner should validate assumptions
> against operational actuals before implementation.

---

## Evidence Required

- monitoring_roster_v1.parquet — level assignments, pathway assignments,
  allowed dollar amounts
- Cell-level provider counts and dollar exposure aggregations
- Composite allocation score distribution across nine cells
- Four-view output tables with priority rankings
- Sensitivity analysis across R1/R2/R3 weight assumptions
- Sensitivity analysis across View A–C operational assumptions

---

## Data Dependencies

**Primary Input Dataset:**

| File                          | Source      | Content                                                              |
|-------------------------------|-------------|----------------------------------------------------------------------|
| monitoring_roster_v1.parquet  | D-OPS-1     | 61,416 providers — monitoring level, pathway, allowed dollars, score |

No additional upstream files required.

**Known Data Gaps:**

| Gap                              | Impact                                              | Tag |
|----------------------------------|-----------------------------------------------------|-----|
| No empirical review hours data   | Hours per provider by level must be assumed         | A   |
| No UPIC contract capacity data   | FTE and case slot ceilings must be assumed          | A   |
| No MAC throughput data           | Case slot rate at L2 cannot be empirically calibrated | A |
| No enforcement outcome data      | Cannot weight cells by enforcement yield probability | A  |
| Single data year                 | Dollar exposure reflects Data Year 2023 only        | A   |

---

## Analytical Requirements

- Nine-cell level × pathway structure from monitoring_roster_v1.parquet
- Composite allocation score from three parameterized dimensions
- Four denominator views produced from single core allocation engine
- Each view accompanied by a Decision Note (tagged I)
- Sensitivity analysis across dimension weights and operational assumptions
- Priority ranking table per denominator view
- Cumulative coverage chart across all four views
- Colorblind-accessible visualizations using IBM Carbon palette throughout

---

## Output Tagging Standard

| Tag | Meaning                                          |
|-----|--------------------------------------------------|
| O   | Observed — directly in the data                  |
| D   | Derived — mathematically constructed             |
| I   | Inferred — evidence-based pattern interpretation |
| A   | Assumed — declared gap-filling                   |

**No output may assert provider intent, fraud, or wrongdoing.**
Language is restricted to: resource allocation score, review burden
estimate, capacity unit share, level × pathway cell priority rank,
and composite allocation weight.

---

## Outputs Produced

| File                              | Location               | Description                                          | Tag |
|-----------------------------------|------------------------|------------------------------------------------------|-----|
| allocation_cells_v1.parquet       | data/processed/        | Nine cells with R1–R3 scores and aggregations        | D   |
| allocation_views_v1.parquet       | data/processed/        | Four-denominator view outputs by cell                | A/D |
| allocation_priority_v1.png        | outputs/figures/       | Cell priority ranking heatmap                        | D   |
| allocation_burden_v1.png          | outputs/figures/       | Four-panel denominator view bar charts               | A/D |
| allocation_cumulative_v1.png      | outputs/figures/       | Cumulative resource coverage by priority rank        | A/D |
| D-OPS-2_recommendation_memo_v1.md | outputs/decision_briefs/ | Stakeholder memo with four Decision Notes          | I   |

---

## Decision Impact Pathway

D-OPS-1 monitoring roster — 61,416 providers by level × pathway (D)
  -> Nine-cell level × pathway structure (D)
    -> Three-dimension composite allocation score per cell (D)
      -> Four denominator view translations (A/D)
        -> View A — Review Hours (A)
        -> View B — FTE Capacity (A)
        -> View C — Case Slots (A)
        -> View D — Abstract Capacity Units (D)
          -> Priority ranking by denominator view (D/I)
            -> Decision Notes — recommended allocation per view (I)
              -> CPI Director resource allocation guidance (I)

---

## Notebook Dependencies

| Notebook                          | Role                              | Location              |
|-----------------------------------|-----------------------------------|-----------------------|
| D-OPS-2_resource_allocation.ipynb | Allocation scoring + four-view output | notebooks/decision/ |

**Upstream dependencies:** monitoring_roster_v1.parquet (D-OPS-1)

**Downstream consumers:**
- D-EXT-1 Safe Reporting
- CPI Director resource planning (D-OPS-2_recommendation_memo_v1.md)
- UPIC Operations (allocation_views_v1.parquet)

---

## Governance Footnote

This charter reflects decision framing established March 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
UPIC and MAC operational structure references sourced from CMS Program
Integrity Manual, Chapter 4, and OIG UPIC evaluation reports.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
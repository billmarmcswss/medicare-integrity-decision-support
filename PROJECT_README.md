# Medicare Integrity Decision Support

**Repository:** `medicare-integrity-decision-support`
**Version:** 1.0
**Date:** February 2026
**Status:** Active — Module 1 Complete, Modules 2–7 In Development

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this repository are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Decision authority patterns described remain analytically applicable regardless
> of subsequent organizational changes. Data references are based on CMS Medicare
> Part B Public Use File, Reporting Year 2025, Data Year 2023 (RY25/D23).
> Readers should verify current organizational alignment independently.

---

## Project Purpose

This repository provides structured, evidence-based analytical support for
Medicare program integrity decisions. It is not a decision-making system.
Every module produces ranked signals, parameterized outputs, and documented
uncertainties intended to inform human judgment by named decision owners
within CMS program integrity structures.

No output in this repository asserts provider intent, fraud, waste, or abuse.
All analytical outputs describe statistical patterns only, relative to peer
providers under defined grouping logic.

---

## Legal and Use Disclaimer

This repository is an independent analytical portfolio project. It is not
affiliated with, endorsed by, or produced in cooperation with the Centers
for Medicare and Medicaid Services (CMS), the Department of Health and Human
Services (HHS), the Office of Inspector General (OIG), or any federal, state,
or local government agency.

All data used in this project is publicly available and sourced directly from
CMS data publication programs. No proprietary, confidential, or non-public
data has been used.

No outputs in this repository are intended for operational use in any federal
or state program integrity process. No outputs constitute actionable findings,
referrals, or recommendations under any federal statute, including but not
limited to the False Claims Act, the Anti-Kickback Statute, or the Social
Security Act.

Analytical outputs describe statistical patterns in publicly available billing
data only. They do not establish, imply, or suggest provider wrongdoing,
fraudulent intent, or violation of any law, regulation, or program requirement.

This project is presented solely for the purpose of demonstrating analytical
methodology, decision-support framework design, and data science portfolio work.

---

## Why This Project Exists

CMS program integrity resources are finite. The Medicare Part B program covers
over one million rendering providers billing across thousands of procedure codes
nationally. Across four decision domains — Program Integrity, Policy, Operations,
and External Reporting — structured analytical support is required to ensure
that review, escalation, monitoring, and reporting decisions are:

- Evidence-based and reproducible
- Defensible under audit or legal scrutiny
- Consistent across review cycles
- Transparent in their assumptions and limitations

This repository addresses that need through a seven-module decision-support
framework spanning those four domains.

---

## Decision Framework Architecture

Each module follows a standardized Decision Charter structure including:

- Decision statement and ownership
- Decision context (why it exists, consequences of error or delay)
- Primary uncertainties and evidence requirements
- Data dependencies and known gaps
- Analytical requirements and output tagging
- Governance footnote with data and organizational source dates

All outputs are tagged using a four-category evidence standard:

| Tag | Meaning |
|---|---|
| O | Observed — directly in the data |
| D | Derived — mathematically constructed from observed data |
| I | Inferred — evidence-based pattern interpretation |
| A | Assumed — declared gap-filling for decision support |

---

## Module Roadmap

### Program Integrity Decisions (D-PI)

| Module | Decision | Status |
|---|---|---|
| D-PI-1 | Provider Review Entry | **Complete** |
| D-PI-2 | Escalation Threshold | In Development |

### Policy Decisions (D-POL)

| Module | Decision | Status |
|---|---|---|
| D-POL-1 | Code Scrutiny | In Development |
| D-POL-2 | Specialty Shift | In Development |

### Operational Decisions (D-OPS)

| Module | Decision | Status |
|---|---|---|
| D-OPS-1 | Monitoring Placement | In Development |
| D-OPS-2 | Resource Allocation | In Development |

### External Decisions (D-EXT)

| Module | Decision | Status |
|---|---|---|
| D-EXT-1 | Safe Reporting | In Development |

---

## Completed Module — D-PI-1 Provider Review Entry

**Decision:** Which providers should enter the program integrity review queue
based on defensible, evidence-supported signals of anomalous utilization or
billing patterns relative to peer providers?

**Key Results (Data Year 2023):**
- Provider population analyzed: 970,848
- High-priority candidates identified (Tier 1): 500
- Allowed dollar exposure represented: $1.77 billion (2.6% of total)
- Provider population share: 0.05%
- Signals used: 8 independent peer-adjusted metrics
- Tier 1 threshold: Multi-signal agreement (3 or more signals flagged)
- Minimum volume filter: 50 beneficiaries (eliminates low-volume ratio inflation)

**Notebooks:**
- `00_data_intake_and_validation.ipynb`
- `01_peer_grouping_design.ipynb`
- `02_signal_engine.ipynb`

See `D-PI-1_provider_review/README.md` for full Decision Charter.

---

## Primary Data Source

**CMS Medicare Part B Public Use File — Provider and Service Level**
`MUP_PHY_R25_P05_V20_D23_Prov_Svc.csv`
Reporting Year 2025 / Data Year 2023

Source: [CMS Data Research](https://www.cms.gov/data-research)

---

## Analytical Standards

- Peer grouping by Provider Type with State and RUCA as secondary controls
- Outlier detection via peer-adjusted Z-scores
- Multi-signal agreement frameworks to reduce false positives
- Parameterized thresholds with documented rationale — no hardcoded values
  without explicit annotation and justification
- Colorblind-accessible visualizations throughout
- Language restricted to: anomalous patterns, statistical deviation,
  billing intensity signals, and peer-adjusted outliers

---

## Repository Structure

```
medicare-integrity-decision-support/
│
├── .gitignore
├── DECISION_INVENTORY.md
├── README.md                                    <- This file
├── requirements.txt
│
├── assumptions\                                 <- In Development
│
├── data\
│   ├── interim\
│   │   ├── .gitkeep
│   │   └── partb_provider_service.parquet\      <- Partitioned Parquet dataset (Dask)
│   │       ├── part_00000.parquet
│   │       ├── ...
│   │       └── part_00038.parquet
│   ├── processed\
│   │   ├── .gitkeep
│   │   ├── provider_review_queue_v1.parquet
│   │   ├── provider_rollup_v1.parquet
│   │   ├── provider_signal_scores_v1.parquet
│   │   └── provider_tiered_v1.parquet
│   └── raw\
│       ├── .gitkeep
│       └── MUP_PHY_R25_PD5_V20_D23_prov_svc.csv
│
├── decisions\
│   ├── D-PI-1_provider_review\                  <- Complete
│   │   ├── README.md                            <- Decision Charter
│   │   ├── decision_log.md
│   │   ├── evidence_log.md
│   │   └── notebook_map.md
│   ├── D-PI-2_escalation_threshold\             <- In Development
│   │   └── README.md
│   ├── D-POL-1_code_scrutiny\                   <- In Development
│   │   └── README.md
│   ├── D-POL-2_specialty_shift\                 <- In Development
│   │   └── README.md
│   ├── D-OPS-1_monitoring_placement\            <- In Development
│   │   └── README.md
│   ├── D-OPS-2_resource_allocation\             <- In Development
│   │   └── README.md
│   └── D-EXT-1_safe_reporting\                  <- In Development
│       └── README.md
│
├── docs\                                        <- In Development
│
├── notebooks\
│   ├── core\
│   │   ├── 00_data_intake_and_validation.ipynb
│   │   ├── 01_peer_grouping_design.ipynb
│   │   ├── 02_signal_engine.ipynb
│   │   └── D1_provider_review_queue.ipynb
│   └── decision\                               <- In Development
│
└── outputs\
    ├── decision_briefs\
    │   ├── .gitkeep
    │   └── D-PI-1_recommendation_memo_v1.md
    ├── figures\
    │   ├── .gitkeep
    │   ├── decision_sensitivity_v1.png
    │   └── signal_validation_v1.png
    └── tables\
        └── .gitkeep
```

---

## Governance Footnote

This repository reflects decision framing established February 2026.
CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
This is an active, ongoing project. Module status indicators reflect
current development state and will be updated as modules are completed.

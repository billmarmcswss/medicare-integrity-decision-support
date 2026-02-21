# Decision Notebooks

**Status:** Active — D-PI-2 In Development

---

## Purpose

This folder contains decision-specific analytical notebooks for the Medicare
Integrity Decision Support framework. Each notebook in this folder consumes
outputs produced by the core signal engine (notebooks\core) and applies
decision-specific logic to produce a structured recommendation for a named
decision owner within CMS program integrity structures.

## Distinction from notebooks\core

| notebooks\core | notebooks\decision |
|---|---|
| Shared infrastructure | Decision-specific analysis |
| Produces data used across multiple decisions | Consumes core outputs for one specific decision |
| Signal computation, peer grouping, tiering | Escalation logic, routing, recommendation production |
| Runs once per data refresh cycle | Runs per decision cycle for named decision owner |

## Naming Convention

Decision notebooks follow the module naming convention established in the
decisions\ folder:

```
D-{DOMAIN}-{NUMBER}_{decision_name}.ipynb
```

Examples:
- `D-PI-2_escalation_threshold.ipynb`
- `D-POL-1_code_scrutiny.ipynb`
- `D-OPS-1_monitoring_placement.ipynb`

## Current Notebooks

| Notebook | Decision | Status |
|---|---|---|
| D-PI-2_escalation_threshold.ipynb | D-PI-2 Escalation Threshold | In Development |
| D-POL-1_code_scrutiny.ipynb | D-POL-1 Code Scrutiny | In Development |
| D-POL-2_specialty_shift.ipynb | D-POL-2 Specialty Shift | In Development |
| D-OPS-1_monitoring_placement.ipynb | D-OPS-1 Monitoring Placement | In Development |
| D-OPS-2_resource_allocation.ipynb | D-OPS-2 Resource Allocation | In Development |
| D-EXT-1_safe_reporting.ipynb | D-EXT-1 Safe Reporting | In Development |

## Input Dependencies

All notebooks in this folder depend on outputs from notebooks\core:

- `data\processed\provider_tiered_v1.parquet`
- `data\processed\provider_signal_scores_v1.parquet`
- `data\processed\provider_review_queue_v1.parquet`
- `data\processed\provider_rollup_v1.parquet`

## Analytical Standards

All notebooks in this folder adhere to the following project-wide standards:

- No hardcoded variables without explicit annotation and documented rationale
- Colorblind-accessible visualizations using IBM Carbon accessible palette
- Output tagging standard (O/D/I/A) applied to all outputs
- Language restricted to anomalous patterns, statistical deviation,
  billing intensity signals, and peer-adjusted outliers
- No output may assert provider intent, fraud, or wrongdoing

---

## Governance Footnote

This folder is part of the Medicare Integrity Decision Support framework
established February 2026. All analytical outputs describe statistical
patterns only. No outputs constitute findings of fraud, waste, or abuse.

See repository root `README.md` for full project architecture and roadmap.

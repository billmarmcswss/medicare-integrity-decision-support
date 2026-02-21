# Notebook Map — D-PI-1 Provider Review Entry

**Version:** 1.0
**Date:** February 2026

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This document maps each notebook to its role in supporting D-PI-1.
No notebook exists in this project without a decision justification.

---

## Core Notebooks (Shared Infrastructure)

| Notebook | Role | Key Cell IDs | Outputs |
|---|---|---|---|
| `00_data_intake_and_validation.ipynb` | Raw CSV ingestion, schema validation, parquet conversion | DPI1-00-INGEST-01 through DPI1-00-CHARTER-01 | `data/interim/partb_provider_service.parquet` |
| `01_peer_grouping_design.ipynb` | Peer group definition and lock (Provider Type primary) | DPI1-01-LOAD-01 through DPI1-01-LOCK-01 | `peer_group_contract` dict |
| `02_signal_engine.ipynb` | Signal computation, Z-scoring, tiering, review queue | DPI1-02-SIGNALS-01 through DPI1-02-EVIDENCE-01 | `provider_review_queue_v1.parquet` |

---

## Decision Notebooks (D-PI-1 Specific)

| Notebook | Role | Status |
|---|---|---|
| `D1_provider_review_queue.ipynb` | Frames Tier 1 queue as operational recommendation for PI actor | Pending |

---

## Dependency Order

Notebooks must be executed in this order per session:

1. `00_data_intake_and_validation.ipynb`
2. `01_peer_grouping_design.ipynb`
3. `02_signal_engine.ipynb`
4. `D1_provider_review_queue.ipynb` (decision notebook — pending)

---

## Governing Rule

A notebook is added to this map if and only if:

- A decision exists that requires it
- That decision has a blocking uncertainty
- That uncertainty requires unique evidence
- That evidence cannot be produced by an existing notebook

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.

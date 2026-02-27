# Decision Log — D-POL-1 Code Scrutiny

**Version:** 1.0
**Date:** February 2026

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Purpose

This log records analytical and architectural decisions made during the
development of D-POL-1 support infrastructure. It captures what was decided,
why, what evidence was used, and whether any decision was later reversed.

This is not a log of provider-level findings.
It is a log of analytical design decisions.

---

## Entries

| ID | Date | Decision Made | Evidence Used | Confidence | Reversed? | Notes |
|---|---|---|---|---|---|---|
| DL-DPOL1-01 | 2026-02 | Input scope set to OIG + TPE combined (3,182 providers) | Both pathways warrant code-level scrutiny; excluding OIG would reduce analytical coverage at highest-risk population | High | No | EdLetter providers excluded — code-level scrutiny not warranted at that pathway |
| DL-DPOL1-02 | 2026-02 | Four dimensions selected: C1 Concentration, C2 Peer Deviation, C3 Signal Overlap, C4 National Norm | Dimensions capture concentration, deviation, signal convergence, and national context independently | High | No | C1 subsequently determined non-discriminating — see DL-DPOL1-06 |
| DL-DPOL1-03 | 2026-02 | CORE_Z_THRESHOLD set to 2.0 for C2 peer deviation flagging | Inherited from D-PI-1 signal engine; industry-standard anomaly detection threshold | High | No | Not provisional — accepted industry constant |
| DL-DPOL1-04 | 2026-02 | MIN_PEER_SIZE set to 5 for Provider Type x HCPCS peer groups | Code-level peer groups structurally smaller than provider-level groups; minimum of 5 balances reliability against coverage loss | Medium | Pending review | PROVISIONAL — tag A; 567 rows assigned neutral score |
| DL-DPOL1-05 | 2026-02 | Services file uses Avg_Mdcr_Alowd_Amt — derived totals computed as Avg x Tot_Benes | CMS PUF provider-service file publishes average dollar amounts not totals; multiplication by Tot_Benes produces reliable total estimate | High | No | Tag D; consistent with PUF methodology documentation |
| DL-DPOL1-06 | 2026-02 | C1 concentration excluded from active dimensions — non-discriminating | DPOL1-06-DIAG-01: OIG median top-3 share=0.980, TPE median=1.000; peer median near 1.0 makes 1.5x threshold unreachable; no discriminating signal | High | No | Tag A; annotated for future review — options include top-1 share, absolute threshold, geographic stratification |
| DL-DPOL1-07 | 2026-02 | C3 overlap flag defined as C2 deviation firing on provider top-1 HCPCS code | Convergence of peer deviation and top-code concentration on same code strengthens analytical warrant | High | No | Tag D/I — deviation is derived; convergence interpretation is inferred |
| DL-DPOL1-08 | 2026-02 | C4 national percentile threshold set to p90 | Providers above p90 for their specialty on a given code are considered extreme-tail outliers | Medium | Pending review | PROVISIONAL — tag A; recalibrate after national distribution review |
| DL-DPOL1-09 | 2026-02 | Active dimensions for dimensions_flagged: C2, C3, C4 only | C1 excluded per DL-DPOL1-06; three remaining dimensions provide peer deviation, signal convergence, and national norm independently | High | No | 554 providers with all three dimensions firing |
| DL-DPOL1-10 | 2026-02 | Top-N display set to 20 codes for visualization and output ranking | 20 codes provides actionable policy target list without overwhelming visualization or memo | Medium | No | Tag A; adjustable in CONFIG |

---

## Reversal Protocol

If any decision above is reversed in a future session:

1. Update the Reversed? field to Yes
2. Add a new entry documenting the replacement decision
3. Update the evidence log with new supporting evidence
4. Update notebook_map.md if notebook dependencies change
5. Re-run affected notebook cells in dependency order

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, Data Year 2023.
All decisions documented here reflect analytical design choices only.
No entries constitute findings of fraud, waste, or abuse.
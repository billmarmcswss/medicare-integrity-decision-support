# Decision Log — D-PI-2 Escalation Threshold

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
development of D-PI-2 support infrastructure. It captures what was decided,
why, what evidence was used, and whether any decision was later reversed.

This is not a log of provider-level findings.
It is a log of analytical design decisions.

---

## Log Format

| ID | Date | Decision Made | Evidence Used | Confidence | Reversed? | Notes |
|---|---|---|---|---|---|---|

---

## Entries

| ID | Date | Decision Made | Evidence Used | Confidence | Reversed? | Notes |
|---|---|---|---|---|---|---|
| DL-DPI2-01 | 2026-02 | Four dimensions selected for composite score: D1 Signal Breadth, D2 Signal Type Composition, D3 Peer-Relative Dollar Exposure, D4 Signal Persistence | D-PI-1 signal engine design; CMS/OIG program integrity prioritization guidance | High | No | Dimensions capture breadth, type, magnitude, and persistence of anomalous patterns independently |
| DL-DPI2-02 | 2026-02 | Dimension weights set: D1=0.25, D2=0.30, D3=0.25, D4=0.20 | D2 highest weight reflects CMS/OIG prioritization of high-concern signal types S4, S5a, S5b | High | No | Weights sum to 1.0; assert in CONFIG |
| DL-DPI2-03 | 2026-02 | High-concern signals defined as S4, S5a, S5b weighted 2x standard signals in D2 | S4 (submitted-to-allowed ratio), S5a/S5b (top HCPCS concentration) most strongly associated with program integrity concerns per CMS/OIG guidance | High | No | D2_MAX=11.0; standard signals weighted 1x |
| DL-DPI2-04 | 2026-02 | CORE_Z_THRESHOLD set to 2.0 for signal flagging in D2 | Industry-standard statistical threshold for anomaly detection; inherited from D-PI-1 signal engine | High | No | Accepted industry constant; not provisional |
| DL-DPI2-05 | 2026-02 | D3 peer comparison uses Provider Type peer group median allowed per beneficiary | Provider Type is the stable primary peer grouping established in D-PI-1; consistent with peer_group_contract v1 | High | No | allowed_dollars_sum / Tot_Benes_sum used as allowed per bene proxy |
| DL-DPI2-06 | 2026-02 | D3_CAP set to 5.0 provisionally | Providers billing 5x peer median as reasonable ceiling; compresses extreme outliers without losing resolution in 1x-5x range | Medium | Pending review | PROVISIONAL — tag A; recalibrate after distribution analysis |
| DL-DPI2-07 | 2026-02 | D4 persistence evaluated against State x RUCA subgroups using Rndrng_Prvdr_RUCA | ruca_cat was never written to processed files; raw RUCA code used directly — consistent with peer_group_contract v1 secondary controls | High | No | 801 providers received NEUTRAL_SCORE=0.50 due to subgroup < MIN_SUBGROUP_SIZE=10 |
| DL-DPI2-08 | 2026-02 | State and RUCA joined from interim parquet rather than processed files | Rndrng_Prvdr_State_Abrvtn and Rndrng_Prvdr_RUCA not present in any processed parquet; interim parquet is existing dependency from D-PI-1 | High | No | F_INTERIM added to CONFIG; 1,175,281 unique NPIs in geo lookup |
| DL-DPI2-09 | 2026-02 | THRESHOLD_OIG set to 0.50, THRESHOLD_TPE set to 0.40 | Natural breaks identified in composite score distribution histogram (DPI2-07-VIZ-01); break at 0.40 separates main body from shoulder; break at 0.50 separates shoulder from isolated right tail | High | No | Tag D (Derived); replaces provisional values of 0.75 and 0.50; p90=0.4084, p99=0.5729 confirmed boundaries |
| DL-DPI2-10 | 2026-02 | Three-pathway routing: OIG/DOJ Referral, Targeted Probe and Educate, Provider Education Letter | CMS/UPIC operational pathway structure; aligns with standard program integrity escalation framework | High | No | OIG: 655 (2.9%) | TPE: 2,527 (11.3%) | EdLetter: 19,218 (85.8%) |

---

## Reversal Protocol

If any decision above is reversed in a future session:

1. Update the Reversed? field to Yes
2. Add a new entry documenting the replacement decision
3. Update the evidence
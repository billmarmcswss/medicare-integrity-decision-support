# Recommendation Memo — D-EXT-1 Safe Reporting

**To:** Compliance Officer / OIG Hotline Coordination Lead
**From:** Program Integrity Analytics
**Date:** March 2026
**Re:** Safe Reporting Framework — External Provider Population and Tip Intake Infrastructure
**Classification:** Internal — Analytical Support Document

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Disclaimer

This memo describes statistical patterns in publicly available CMS Medicare
Part B billing data only. No finding in this document constitutes evidence
of fraud, waste, abuse, or any violation of law or program requirement.
All outputs are advisory only. Decision authority is retained by the
Compliance Officer / OIG Hotline Coordination Lead.

---

## Purpose

This memo summarizes the analytical outputs of D-EXT-1 Safe Reporting —
the final module of the seven-module Medicare program integrity
decision-support framework. It provides the Compliance Officer with:

1. A summary of the external reporting population and its dollar exposure
2. Guidance on the safe output files produced for external coordination use
3. Documentation of the tip intake cross-reference infrastructure

---

## Background

Six upstream modules (D-PI-1 through D-OPS-2) produced provider-level
analytical outputs across program integrity, policy, operational, and
resource dimensions using CMS Medicare Part B PUF data for 970,848
providers in Data Year 2023. D-EXT-1 governs the boundary between
those internal analytical outputs and external reporting.

Two flows are addressed:

- **Outbound** — Safe summary outputs for OIG hotline coordination
  and oversight bodies
- **Inbound** — Tip intake cross-reference infrastructure for
  integrating whistleblower and hotline signals into the analytical record

---

## External Reporting Population

The external reporting population consists of providers routed to the
OIG/DOJ Referral and Targeted Probe and Educate (TPE) pathways by D-PI-2.
Education Letter providers and the full monitoring roster are excluded
as inappropriate for external reporting context.

| Pathway | Providers | Allowed Dollars | % of Dollars |
|---|---|---|---|
| OIG/DOJ Referral | 655 | — | — |
| Targeted Probe and Educate | 2,527 | — | — |
| **Total** | **3,182** | **$798.55M** | **100%** |

---

## Population by Pathway and Monitoring Level

| Pathway | Monitoring Level | Providers | % of Total | Allowed Dollars | % of Dollars | Mean Composite Score |
|---|---|---|---|---|---|---|
| OIG/DOJ Referral | Level 1 — Active Surveillance | 606 | 19.0% | $363.69M | 45.5% | 0.570 |
| OIG/DOJ Referral | Level 2 — Periodic Review | 49 | 1.5% | $5.83M | 0.7% | 0.506 |
| TPE | Level 1 — Active Surveillance | 556 | 17.5% | $143.83M | 18.0% | 0.430 |
| TPE | Level 2 — Periodic Review | 1,564 | 49.2% | $255.40M | 32.0% | 0.436 |
| TPE | Level 3 — Passive Surveillance | 407 | 12.8% | $29.79M | 3.7% | 0.403 |

---

## Key Observations

**Dollar concentration in OIG/DOJ Level 1:**
606 OIG/DOJ Referral providers assigned to Level 1 Active Surveillance
represent 19.0% of the external population by count but 45.5% of all
allowed dollars — $363.69M. This concentration reflects the composite
score profile of the OIG referral population and warrants priority
attention in external coordination planning.

**TPE Level 2 is the largest cell by count:**
1,564 TPE providers in Level 2 Periodic Review represent 49.2% of the
external population. Their mean composite score (0.436) is lower than
the OIG population, consistent with the TPE pathway's role as a
structured education and review mechanism rather than direct referral.

**49 OIG providers assigned to Level 2:**
Consistent with findings documented in D-OPS-1, 49 OIG/DOJ Referral
providers score into Level 2 rather than Level 1. This is an analytical
signal for Decision Owner awareness — it does not alter their OIG/DOJ
referral status. External reporting for these providers should reflect
their referral pathway, not their monitoring level assignment.

**No OIG providers in Level 3:**
All 655 OIG/DOJ Referral providers are assigned to either Level 1 or
Level 2 — consistent with their escalation score profile throughout
the framework.

---

## Safe Output Files

Two files have been produced for external coordination use:

**safe_reporting_providers_v1.parquet**
Provider-level file — 3,182 rows, 9 columns. Contains NPI, provider
type, state, pathway, monitoring level, allowed dollars, composite score,
disclaimer, and evidence tag. Raw dimension sub-scores are excluded.
Every row carries an embedded disclaimer and evidence tag.

**safe_reporting_summary_v1.parquet**
Aggregate summary — 5 rows, 10 columns. Summarizes provider counts,
allowed dollars, and mean composite scores by pathway and monitoring
level. Suitable for executive briefing and oversight body reporting.

Both files were round-trip verified. All assertions passed.

---

## Tip Intake Cross-Reference Infrastructure

The cross_reference_tip() function provides the integration pattern
for inbound whistleblower and OIG hotline tips. When a tip NPI is
received it is matched against the safe provider file and routed as follows:

- **Matched** — provider flagged for Decision Owner awareness with
  full analytical context: pathway, monitoring level, composite score,
  and allowed dollars
- **Unmatched** — provider routed to D-PI-1 intake for potential
  future queue entry

No live tip data exists in the RY25/D23 analytical cycle. The
infrastructure is validated and ready for operational use when tips
are received. All tip convergence flags are advisory only — the
Compliance Officer retains full authority over tip handling and
any resulting enforcement referral.

---

## Recommendations

1. **Distribute safe_reporting_providers_v1.parquet** to OIG hotline
   coordination staff for use in external reporting and referral
   coordination. Confirm data use agreements are in place before
   distribution.

2. **Use safe_reporting_summary_v1.parquet** for oversight body
   briefings and executive summaries. The aggregate format is
   appropriate for audiences without direct data access.

3. **Flag the 49 OIG Level 2 providers** for Compliance Officer
   review. Their referral pathway status is unchanged — monitoring
   level assignment is an operational signal only.

4. **Activate tip intake infrastructure** when inbound tips are
   received. The cross_reference_tip() function is validated and
   ready. NPPES lookup capability should be confirmed for tips
   that reference providers by name or address rather than NPI.

5. **Rerun D-EXT-1** when Data Year 2024 becomes available to
   capture temporal shifts in the external reporting population.

---

## Framework Completio
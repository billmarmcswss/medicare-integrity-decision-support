# Decision Charter — D-EXT-1 Safe Reporting

**Module:** D-EXT-1
**Decision:** Safe Reporting
**Version:** 1.0
**Date:** March 2026
**Status:** In Development

---

## Organizational Reference Notice

> Organizational roles and structures referenced in this document are based on the
> CMS Organizational Chart dated April 22, 2025. Structure is subject to change.
> Data references are based on CMS Medicare Part B PUF, RY25/D23.

---

## Decision Statement

Which analytical findings from the Medicare program integrity decision-support
framework can be safely packaged for external reporting to the OIG and oversight
bodies, and how should inbound tips from whistleblowers and hotline reporters
be integrated into the analytical record — without asserting wrongdoing,
compromising enforcement actions, or misrepresenting statistical patterns
as legal findings?

---

## Decision Owner

- **Primary Owner:** Office of Inspector General (OIG) — Hotline and Compliance
- **Supporting Owner:** Center for Program Integrity (CPI) — External Coordination
- **Decision Owner Role:** Compliance Officer / OIG Hotline Coordination Lead
- **Actor Tag:** COM + EXT

---

## Decision Context

The six upstream modules (D-PI-1 through D-OPS-2) produce provider-level
analytical outputs across program integrity, policy, operational, and
resource dimensions. Those outputs are intended for internal CMS/UPIC
decision-making. D-EXT-1 governs the boundary between internal analytical
work and external reporting.

Two flows require governance:

**Outbound — Safe External Reporting:**
Analytical findings about OIG referral providers (n=655) and TPE-routed
providers (n=2,527) may be shared with OIG hotline coordination staff and
oversight bodies. Safe reporting requires that outputs are:
- Aggregated or filtered to remove operationally sensitive details
- Tagged to distinguish observed data from derived or assumed outputs
- Accompanied by explicit disclaimers precluding legal interpretation
- Scoped to publicly available CMS PUF data only

**Inbound — Tip Intake Integration:**
Whistleblower and OIG hotline tips may reference providers already present
in the analytical record. D-EXT-1 establishes how inbound tip signals
are documented, cross-referenced against existing provider routing, and
flagged for Decision Owner awareness — without the analytical system
itself acting on unverified tip content.

---

## Consequences of Error or Delay

| Error Type | Consequence |
|---|---|
| Releasing operationally sensitive provider details externally | May compromise active UPIC or DOJ enforcement actions |
| Asserting fraud or wrongdoing in external reports | Legal and reputational exposure for CMS/OIG |
| Failing to cross-reference inbound tips against analytical record | Missed signal convergence — tip + data pattern in same provider |
| Acting on unverified tip content without analytical corroboration | Introduces bias and due process risk into enforcement pipeline |

---

## Primary Uncertainties

| Uncertainty | Impact | Tag |
|---|---|---|
| Whether a provider in the OIG referral list has an active enforcement action | Cannot confirm without UPIC case system access — not available in PUF | A |
| Whether inbound tips reference providers by NPI or by name/address only | NPI matching may require NPPES lookup for non-NPI tip submissions | A |
| Whether tip volume in any reporting period is sufficient for pattern analysis | Tip intake is event-driven — volume unknown in advance | A |
| Whether external recipients have appropriate data use agreements in place | Assumed yes for OIG coordination; cannot verify from PUF data alone | A |

---

## Scope

**In Scope:**
- OIG referral providers: 655 providers routed via D-PI-2
- TPE-routed providers: 2,527 providers routed via D-PI-2
- Outbound safe summary file construction and column selection
- Inbound tip cross-reference logic against analytical record
- Disclaimer and evidence tag documentation for all external outputs

**Out of Scope:**
- Full monitoring roster (61,416 providers) — not appropriate for external reporting
- Education Letter providers — insufficient signal strength for external referral context
- Legal determination of reportability — retained by Decision Owner
- Active enforcement case status — not available in PUF data

---

## Data Dependencies

| File | Source | Content |
|---|---|---|
| `escalation_pathway_v1.parquet` | D-PI-2 output | Pathway assignments — OIG and TPE populations |
| `escalation_scored_v1.parquet` | D-PI-2 output | Composite escalation scores |
| `monitoring_roster_v1.parquet` | D-OPS-1 output | Monitoring intensity levels for OIG/TPE providers |
| `resource_allocation_v1.parquet` | D-OPS-2 output | Resource context for external summary |

---

## Analytical Requirements

- Safe output file constructed with explicit column allowlist (no operationally
  sensitive derived fields exposed without aggregation)
- Aggregate summary statistics by pathway and monitoring level
- Tip cross-reference logic documented as placeholder infrastructure
  (tip intake is event-driven; framework documents the integration pattern)
- All outputs tagged O/D/I/A
- Explicit external disclaimer block included in all output files and memo

---

## Output Tagging Standard

| Tag | Meaning |
|---|---|
| O | Observed — directly in the data |
| D | Derived — mathematically constructed from observed data |
| I | Inferred — evidence-based pattern interpretation |
| A | Assumed — declared gap-filling for decision support |

**No output may assert provider intent, fraud, or wrongdoing.**

---

## Governance Footnote

CMS organizational references sourced from CMS Org Chart, April 22, 2025.
Data sourced from CMS Medicare Part B PUF, RY25/D23.
All analytical outputs describe statistical patterns only.
No outputs constitute findings of fraud, waste, or abuse.
# SESSION_CONTEXT.md
# Medicare Integrity Decision Support — Active Session Log
# Location: C:\Users\billm\Projects\Medicare\medicare-program-integrity\
# PURPOSE: Paste the CURRENT STATE section at the top of every new chat. That's it.
# Do NOT paste the full file — just the Current State block.

---

## HOW TO USE THIS FILE

1. At the end of every session, update the CURRENT STATE block below.
2. When starting a new chat, paste only the CURRENT STATE block as your opening message.
3. Keep the FULL HISTORY section below for your own reference.

The Current State block is designed to be small — under 30 lines — so it costs
minimal tokens and leaves maximum room for actual work in the new chat.

---

## ==================== CURRENT STATE ====================
## (PASTE THIS BLOCK — AND ONLY THIS BLOCK — TO START A NEW CHAT)

**Project:** medicare-integrity-decision-support  
**Local path:** `C:\Users\billm\Projects\Medicare\medicare-integrity-decision-support\`  
**GitHub:** https://github.com/billmarmcswss/medicare-integrity-decision-support  

**Project architecture:** 7-module Medicare program integrity decision-support framework  
using CMS Part B 2023 public data (MUP_PHY_R25_PD5_V20_D23_prov_svc.csv).  
970,848 providers processed. No hardcoded variables. CVD-compliant visuals (IBM Carbon palette).  
All outputs tagged O/D/I/A. No assertions of fraud or wrongdoing anywhere in project.

**Seven modules:**
- D-PI-1 Provider Review Entry — COMPLETE, on GitHub
- D-PI-2 Escalation Threshold — NOTEBOOK COMPLETE, memo pending
- D-POL-1 Code Scrutiny — placeholder README on GitHub
- D-POL-2 Specialty Shift — placeholder README on GitHub
- D-OPS-1 Monitoring Placement — placeholder README on GitHub
- D-OPS-2 Resource Allocation — placeholder README on GitHub
- D-EXT-1 Safe Reporting — placeholder README on GitHub

**D-PI-1 outputs (in data/processed/):**
- provider_tiered_v1.parquet — 970,848 providers; 22,400 Tier 1 (flagged ≥3 signals)
- provider_signal_scores_v1.parquet — 8 signals per provider (S1–S7, S5a/S5b)
- provider_rollup_v1.parquet — allowed dollars, payments, bene counts
- provider_review_queue_v1.parquet — 22,400 Tier1_Review providers, dollar-ranked
- Top Tier 1 entry: IDTF, $132M allowed, 5/8 signals flagged

**D-PI-2 design (four dimensions, three CMS/UPIC pathways):**
- D1 Signal Breadth (weight 0.25) — signals_flagged / 8
- D2 Signal Type Composition (weight 0.30) — weighted high-concern signals S4, S5a, S5b (2x) vs standard (1x); D2_MAX=11.0
- D3 Peer-Relative Dollar Exposure (weight 0.25) — allowed_per_bene vs. Provider Type peer median; D3_CAP=5.0 PROVISIONAL
- D4 Signal Persistence (weight 0.20) — Z-scores re-evaluated vs. State × RUCA subgroups; 801 neutral assignments (subgroup < 10)
- Pathways: OIG/DOJ Referral | Targeted Probe and Educate (TPE) | Provider Education Letter
- CORE_Z_THRESHOLD = 2.0 (industry standard, inherited from D-PI-1)
- THRESHOLD_OIG = 0.50 (CALIBRATED — natural break from distribution analysis)
- THRESHOLD_TPE = 0.40 (CALIBRATED — natural break from distribution analysis)
- Min subgroup size: 10 providers (neutral score 0.50 if below)

**D-PI-2 outputs (in data/processed/):**
- escalation_scored_v1.parquet — 22,400 rows, 34 columns; full scoring audit trail
- escalation_pathway_v1.parquet — 22,400 rows, 14 columns; lean operational file

**D-PI-2 pathway results:**
- OIG/DOJ Referral        :   655 providers  (2.9%)
- Targeted Probe & Educate:  2,527 providers (11.3%)
- Provider Education Letter: 19,218 providers (85.8%)

**D-PI-2 notebook cells completed (all passing clean):**
- DPI2-00-HEADER-00, DPI2-00-CONFIG-01
- DPI2-01-LOAD-01, DPI2-02-VALIDATE-01
- DPI2-03-PREP-00, DPI2-03-PREP-00a, DPI2-03-PREP-01
- DPI2-05-SCORE-01
- DPI2-07-VIZ-01
- DPI2-06-PATHWAY-01
- DPI2-08-OUTPUT-01
- DPI2-09-EVIDENCE-01

**LAST COMPLETED:**
Session: 2026-02-24
D-PI-2 notebook complete. Evidence log written to
decisions/D-PI-2_escalation_threshold/evidence_log.md.

**NEXT ACTION:**
1. GitHub push — commit all D-PI-2 work
2. Build D-PI-2 recommendation memo →
   outputs/decision_briefs/D-PI-2_recommendation_memo_v1.md
3. Revisit D3_CAP=5.0 (still PROVISIONAL) before module marked COMPLETE
## ==================== END CURRENT STATE ====================

---

## FULL SESSION HISTORY
## (For your reference only — do NOT paste this into new chats)

---

### Session: 2026-02-24
**Chat title:** [New Chat 2026-02-24 0805 Continuing our Medicare Project with updated SESSION_CONTEXT.md]
**Accomplished:**
- Built DPI2-03-PREP-00, PREP-00a, PREP-01 — df_work and D1–D4 features
- Built DPI2-05-SCORE-01 — composite scores for 22,400 providers
- Built DPI2-07-VIZ-01 — distribution histogram, thresholds calibrated
- Built DPI2-06-PATHWAY-01 — three-pathway routing complete
- Built DPI2-08-OUTPUT-01 — two parquet files written and verified
- Built DPI2-09-EVIDENCE-01 — six evidence entries, all tag D
- Written evidence_log.md to decisions/D-PI-2_escalation_threshold/
- D-PI-2 notebook complete

**Ended because:** Proceeding to GitHub push

### Session: 2026-02-20 ~1724
**Chat title:** New Chat 2026-02-20 1724 Medicare project naming conventions issues  
**Accomplished:**
- Established full 7-module architecture and folder structure
- Named repository: medicare-integrity-decision-support
- Built root README.md with legal disclaimer and full module roadmap
- Built DECISION_INVENTORY.md v2 with Decision Owner role mapping per module
- Built placeholder README.md for all 6 non-D-PI-1 decision folders
- Built README.md for assumptions/, docs/, notebooks/decision/
- Resolved .gitignore to expose output figures and decision brief markdown
- Completed first GitHub push (47 objects, clean)
- Completed second GitHub push (placeholder READMEs, notebooks/decision/)
- Confirmed full folder structure rendering on GitHub
- D-PI-2 architecture designed: 4 dimensions, 3 CMS/UPIC pathways
- D-PI-2 Decision Charter (README.md) built
- D-PI-2 notebook built: 9 cells (DPI2-00-CONFIG-01 through DPI2-09-EVIDENCE-01)

**Ended because:** Context window bogged down

---

### Session: 2026-02-21 ~0756
**Chat title:** New Chat 2026-02-21 0756 Medicare program integrity workflow continuation  
**Accomplished:**
- Recovered full project context from transcript paste
- Built decision_log.md for D-PI-2_escalation_threshold (8 entries)
- Built evidence_log.md for D-PI-2_escalation_threshold (6 entries, O/D/I/A tagged)
- Built notebook_map.md for D-PI-2_escalation_threshold (full cell map, I/O table)
- Built SESSION_CONTEXT.md (this file)

**Pending before next push:**
- Confirm D-PI-2 notebook survived from prior session or rebuild it
- Place decision_log.md, evidence_log.md, notebook_map.md into decisions/D-PI-2_escalation_threshold/
- Place D-PI-2_escalation_threshold.ipynb into notebooks/decision/
- git add . → git commit → git push
- Then build D-PI-2 recommendation memo → outputs/decision_briefs/D-PI-2_recommendation_memo_v1.md

### Session: 2026-02-24
**Chat title:** New Chat 2026-02-24 0805 Continuing our Medicare Project with updated SESSION_CONTEXT.md
**Accomplished:**
- Added F_INTERIM to CONFIG for interim parquet access
- Added CORE_Z_THRESHOLD and D3_CAP to CONFIG SCORING PARAMETERS block
- Built DPI2-03-PREP-00 (markdown header)
- Built DPI2-03-PREP-00a (df_work merge — queue + state/RUCA from interim parquet)
- Built DPI2-03-PREP-01 (D1–D4 feature engineering — all four dimensions clean)
- Resolved FutureWarning on D4 groupby apply with include_groups=False
- Resolved 22 D4 null edge cases with post-apply fillna(NEUTRAL_SCORE)

**Ended because:** Session context update — continuing to DPI2-05-SCORE-01

**Ended because:** [update when session ends]

## STANDING PROJECT RULES (apply to every session, every module)

1. No hardcoded variables — all thresholds and weights in CONFIG cell with rationale blocks
2. CVD-compliant visualizations — IBM Carbon palette:
   - Tier1/OIG:     #DC267F  (magenta)
   - Tier2/TPE:     #FFB000  (amber)
   - Tier3/EdLetter:#648FFF  (blue)
   - No Flag/Other: #BBBBBB  (gray)
3. All outputs tagged O/D/I/A (Observed/Derived/Inferred/Assumed)
4. No output may assert provider intent, fraud, or wrongdoing
5. Decision Owner role differs per module — see DECISION_INVENTORY.md
6. Notebook naming: DPI2-00-CONFIG-01 style (module prefix, section, cell number)
7. Every decision folder gets: README.md, decision_log.md, evidence_log.md, notebook_map.md
8. Commit messages are descriptive — no "update files" style commits
9. Random state = 42 for reproducibility
10. Minimum beneficiary threshold = 50 (inherited from D-PI-1, parameterized)

---

## MODULE DECISION OWNER REFERENCE

| Module | Decision Owner Role |
|---|---|
| D-PI-1 Provider Review Entry | Investigations Lead / Program Integrity Analytics Manager |
| D-PI-2 Escalation Threshold | Investigations Lead / UPIC Case Development Manager |
| D-POL-1 Code Scrutiny | Medical Review Director / MAC Medical Director |
| D-POL-2 Specialty Shift | Medical Review Director / CPI Policy Analyst |
| D-OPS-1 Monitoring Placement | UPIC Operations Manager / CPI Monitoring Unit Lead |
| D-OPS-2 Resource Allocation | CPI Director / Program Integrity Resource Manager |
| D-EXT-1 Safe Reporting | Compliance Officer / OIG Hotline Coordination Lead |

---

## REPOSITORY FOLDER STRUCTURE (as of 2026-02-21)

```
medicare-integrity-decision-support/
├── .gitignore
├── DECISION_INVENTORY.md
├── README.md
├── requirements.txt
├── SESSION_CONTEXT.md          ← THIS FILE (not pushed to GitHub)
│
├── assumptions/
│   └── README.md
│
├── data/
│   ├── interim/
│   │   ├── .gitkeep
│   │   └── partb_provider_service.parquet/
│   │       └── part_00000.parquet ... part_00038.parquet
│   ├── processed/
│   │   ├── .gitkeep
│   │   ├── provider_review_queue_v1.parquet
│   │   ├── provider_rollup_v1.parquet
│   │   ├── provider_signal_scores_v1.parquet
│   │   └── provider_tiered_v1.parquet
│   └── raw/
│       ├── .gitkeep
│       └── MUP_PHY_R25_PD5_V20_D23_prov_svc.csv
│
├── decisions/
│   ├── D-PI-1_provider_review/
│   │   ├── README.md
│   │   ├── decision_log.md
│   │   ├── evidence_log.md
│   │   └── notebook_map.md
│   ├── D-PI-2_escalation_threshold/
│   │   ├── README.md
│   │   ├── decision_log.md      ← built 2026-02-21, needs placement
│   │   ├── evidence_log.md      ← built 2026-02-21, needs placement
│   │   └── notebook_map.md      ← built 2026-02-21, needs placement
│   ├── D-POL-1_code_scrutiny/
│   │   └── README.md
│   ├── D-POL-2_specialty_shift/
│   │   └── README.md
│   ├── D-OPS-1_monitoring_placement/
│   │   └── README.md
│   ├── D-OPS-2_resource_allocation/
│   │   └── README.md
│   └── D-EXT-1_safe_reporting/
│       └── README.md
│
├── docs/
│   └── README.md
│
├── notebooks/
│   ├── core/
│   │   ├── 00_data_intake_and_validation.ipynb
│   │   ├── 01_peer_grouping_design.ipynb
│   │   ├── 02_signal_engine.ipynb
│   │   └── D1_provider_review_queue.ipynb
│   └── decision/
│       ├── README.md
│       └── D-PI-2_escalation_threshold.ipynb  ← built prior session, confirm exists
│
└── outputs/
    ├── decision_briefs/
    │   ├── .gitkeep
    │   └── D-PI-1_recommendation_memo_v1.md
    ├── figures/
    │   ├── .gitkeep
    │   ├── decision_sensitivity_v1.png
    │   └── signal_validation_v1.png
    └── tables/
        └── .gitkeep
```

---
*Add SESSION_CONTEXT.md to .gitignore so it never gets pushed to GitHub.*
*It's a local working tool, not a project deliverable.*
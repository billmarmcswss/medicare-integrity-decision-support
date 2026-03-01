PROJECT_STANDARDS.md
Medicare Integrity Decision Support
PURPOSE: Single source of truth for all project-wide standards including
naming conventions, coding rules, visualization requirements,
threshold design policy, and output integrity rules.
Upload to Claude Project knowledge base as PROJECT_STANDARDS.md
Last updated: 2026-02-23

PROJECT-WIDE CODING STANDARDS
These rules apply to every notebook, every module, every session.
No exceptions. When in doubt, refer here first.
Rule 1 — No Hardcoded Variables
No literal values embedded directly in code logic. Every threshold, weight,
ratio, minimum count, palette color, file path, or tuning parameter must be
defined in the CONFIG cell with an accompanying rationale block.
Correct:
python# DPI2-00-CONFIG-01
# Minimum beneficiary threshold — inherited from D-PI-1
# Rationale: CMS statistical reliability standard; suppress providers
#            with fewer than 50 beneficiaries to avoid small-sample distortion
MIN_BENE_THRESHOLD = 50

# Composite score pathway thresholds — PROVISIONAL
# Rationale: Placeholder values pending score distribution analysis in DPI2-07-VIZ
# Action required: Recalibrate after running histogram in cell DPI2-07-VIZ-01
THRESHOLD_OIG    = 0.75   # >= this → OIG/DOJ Referral
THRESHOLD_TPE    = 0.50   # >= this and < OIG → Targeted Probe and Educate
                          # <  this → Provider Education Letter
Incorrect:
python# Hardcoded — never do this
df['pathway'] = df['composite_score'].apply(
    lambda x: 'OIG' if x >= 0.75 else ('TPE' if x >= 0.50 else 'EdLetter')
)
Exceptions — values that may appear inline without CONFIG definition:

Commonly accepted industry/statistical constants (e.g., random_state=42,
confidence=0.95, p_value=0.05)
Pandas/numpy/matplotlib method defaults that are not project decisions
(e.g., figsize=(12, 6) used as a layout convenience, not a standard)

Even for exceptions, add a brief inline comment explaining the value.

Rule 2 — Provisional Thresholds Must Be Labeled
Any threshold set before data distribution analysis must be marked clearly:
pythonTHRESHOLD_OIG = 0.75   # PROVISIONAL — recalibrate after DPI2-07-VIZ-01
Provisional thresholds must be resolved before the module is marked COMPLETE.
The evidence_log.md must record when and how final thresholds were set, tagged A
until calibrated, then updated to D once derived from distribution analysis.

Rule 3 — CVD-Compliant Visualizations (IBM Carbon Palette)
All visualizations must be readable by individuals with color vision deficiency.
Use only the following project palette. No exceptions for "just a quick plot."
python# DPI2-00-CONFIG-01  (paste this block into every notebook CONFIG cell)
CVD_PALETTE = {
    'tier1_oig'   : '#DC267F',   # Magenta  — OIG/DOJ Referral
    'tier2_tpe'   : '#FFB000',   # Amber    — Targeted Probe and Educate
    'tier3_edltr' : '#648FFF',   # Blue     — Provider Education Letter
    'no_flag'     : '#BBBBBB',   # Gray     — No flag / neutral
}
Do not use red/green combinations. Do not use color as the sole differentiator —
always pair color with shape, pattern, or label where possible.

Rule 4 — No Assertions of Provider Intent or Wrongdoing
No cell output, markdown, visualization title, axis label, memo, or log entry
may assert that a provider committed fraud, intended to defraud, or acted
with wrongdoing. This applies under every O/D/I/A tag.
Correct language:

"anomalous billing pattern"
"elevated composite risk score"
"flagged for program integrity review"
"billing volume exceeds peer median by X%"

Prohibited language:

"fraudulent provider"
"provider committed fraud"
"intentional overbilling"
"confirmed abuse"


Rule 5 — Threshold Design Process
Thresholds that route providers to escalation pathways must follow this process:

Set provisional values in CONFIG with # PROVISIONAL label
Build scoring engine and run against full population
Analyze score distribution (histogram + percentile table in VIZ section)
Identify natural breaks or set deliberate percentile-based cutoffs
Update CONFIG with final values, remove # PROVISIONAL label
Document rationale in decision_log.md and evidence_log.md (tag: D)
Module cannot be marked COMPLETE until all provisionals are resolved


Rule 6 — Reproducibility
pythonRANDOM_STATE = 42   # Industry-standard reproducibility seed
Use RANDOM_STATE everywhere a random seed is required. Never use a literal
42 in code logic — always reference the CONFIG variable.

Rule 7 — Minimum Subgroup Size
pythonMIN_SUBGROUP_SIZE = 10   # Minimum providers for peer group validity
                         # Rationale: Below this, peer comparison is statistically
                         # unreliable; assign neutral score 0.50 instead
Any peer group calculation falling below MIN_SUBGROUP_SIZE receives a neutral
score of 0.50 rather than a computed score. Document all neutral assignments
in the evidence_log.md tagged A.

Rule 9 — Opening Markdown Cell Required
Every notebook must begin with a markdown cell before any code cell.
This cell carries the ID [MODULE]-00-HEADER-00 and must include:

Module name and full title
Decision type and Decision Owner role
Current status and last updated date
Purpose statement (with no-wrongdoing disclaimer)
Inputs table (file, source, description)
Outputs table (file, description)
Scoring dimensions or key logic summary (if applicable)
Escalation pathways or decision outcomes (if applicable)
Provisional threshold notice (if applicable)
O/D/I/A tag reference table
Data source citation

No code may precede this markdown cell. No exceptions.

Rule 8 — Commit Message Standards
Every git commit message must be descriptive and action-oriented.
Correct:
Add DPI2 CONFIG cell with dimension weights and provisional pathway thresholds
Incorrect:
update files
fix stuff
wip

NOTEBOOK CELL ID FORMAT
Every code cell and every markdown cell in every notebook carries a unique,
stable ID as the first comment line (code cells) or first line (markdown cells).
Format
[MODULE]-[SECTION_NUM]-[SECTION_NAME]-[CELL_NUM]
ComponentDescriptionExampleMODULEModule prefix (see table below)DPI2SECTION_NUMTwo-digit section number03SECTION_NAMESection vocabulary word (see table below)SCORINGCELL_NUMTwo-digit cell number within section02
Full example
DPI2-03-SCORING-02
Meaning: Module D-PI-2, Section 3, Scoring logic, second cell in that section.
In a code cell
python# DPI2-03-SCORING-02
# Composite score calculation — weighted sum of D1–D4 dimensions
In a markdown cell
markdown<!-- DPI2-03-SCORING-02 -->
## Composite Score Calculation

MODULE PREFIX REFERENCE
ModulePrefixD-PI-1 Provider Review EntryDPI1D-PI-2 Escalation ThresholdDPI2D-POL-1 Code ScrutinyDPOL1D-POL-2 Specialty ShiftDPOL2D-OPS-1 Monitoring PlacementDOPS1D-OPS-2 Resource AllocationDOPS2D-EXT-1 Safe ReportingDEXT1Core notebooks (shared)CORE

SECTION NAME VOCABULARY
Standardized across all modules. Use only these names.
CodeMeaningTypical contentCONFIGConfiguration and parametersWeights, thresholds, palette, paths — all with rationale blocksLOADData loadingRead parquet/CSV, confirm row countsVALIDATEData validationSchema checks, null counts, range assertionsPREPFeature engineering / preparationDerived columns, peer group joins, normalizationPEERPeer group calculationsSubgroup medians, min-size enforcementSCOREScoring logicDimension scores D1–D4, composite scorePATHWAYPathway assignmentOIG/DOJ, TPE, Education Letter threshold logicVIZVisualizationsAll matplotlib/seaborn plots, CVD-compliantOUTPUTOutput generationWrite parquet, CSV, markdown tablesEVIDENCEEvidence loggingO/D/I/A tagged summary of cell outputs

SECTION NUMBERING CONVENTION
Section numbers are fixed per module type to maintain consistency across modules.
Section NumSection NameNotes00CONFIGAlways first cell in every notebook01LOADLoad inputs from prior module outputs02VALIDATEValidate loaded data before processing03PREPFeature engineering04PEERPeer group work (if applicable)05SCORECore scoring logic06PATHWAYPathway/tier assignment07VIZVisualizations08OUTPUTWrite outputs09EVIDENCEEvidence log summary cell
Not every module uses every section. Skip numbers that don't apply —
do not renumber to fill gaps.

FILE NAMING CONVENTIONS
Notebooks
[module-slug].ipynb

Examples:
  D-PI-2_escalation_threshold.ipynb
  D-POL-1_code_scrutiny.ipynb
Output data files
[descriptor]_v[version].parquet

Examples:
  provider_tiered_v1.parquet
  escalation_scored_v1.parquet
Output figures
[descriptor]_v[version].png

Examples:
  decision_sensitivity_v1.png
  pathway_distribution_v1.png
Decision brief memos
[module-slug]_recommendation_memo_v[version].md

Examples:
  D-PI-1_recommendation_memo_v1.md
  D-PI-2_recommendation_memo_v1.md
Decision folder documents
Every decision folder contains exactly these four files, named exactly:
README.md
decision_log.md
evidence_log.md
notebook_map.md

CLAUDE PROJECT KNOWLEDGE BASE UPLOAD NAMING
Files uploaded to the Claude Project knowledge base must have unique names.
Several files share generic names across modules (README.md, decision_log.md, etc.).
Rename copies before uploading — do NOT rename the files on disk or in the repo.
Root and module README files
File on diskUpload to Project asREADME.md (project root)PROJECT_README.mddecisions/D-PI-1_.../README.mdDPI1_decision_README.mddecisions/D-PI-2_.../README.mdDPI2_decision_README.mddecisions/D-POL-1_.../README.mdDPOL1_decision_README.mddecisions/D-POL-2_.../README.mdDPOL2_decision_README.mddecisions/D-OPS-1_.../README.mdDOPS1_decision_README.mddecisions/D-OPS-2_.../README.mdDOPS2_decision_README.mddecisions/D-EXT-1_.../README.mdDEXT1_decision_README.md
Decision folder documents
File on diskUpload to Project asD-PI-1_.../decision_log.mdDPI1_decision_log.mdD-PI-1_.../evidence_log.mdDPI1_evidence_log.mdD-PI-1_.../notebook_map.mdDPI1_notebook_map.mdD-PI-2_.../decision_log.mdDPI2_decision_log.mdD-PI-2_.../evidence_log.mdDPI2_evidence_log.mdD-PI-2_.../notebook_map.mdDPI2_notebook_map.mdD-POL-1_.../decision_log.mdDPOL1_decision_log.mdD-POL-1_.../evidence_log.mdDPOL1_evidence_log.mdD-POL-1_.../notebook_map.mdDPOL1_notebook_map.md(continue pattern for remaining modules)
Rule
Files on disk and in GitHub keep their original names always.
Only the copies uploaded to the Claude Project get the prefixed names.
This keeps the repo structure clean and the Project knowledge base unambiguous.

TROUBLESHOOTING BY CELL ID
When reporting an error, always reference the cell ID, not the line number.
Correct:

"Cell DPI2-05-SCORE-01 is throwing a KeyError on column 'composite_score'"

Incorrect:

"Line 47 is throwing a KeyError"

Line numbers differ between local Jupyter and Claude's environment.
Cell IDs are stable regardless of environment.

O/D/I/A OUTPUT TAGGING
All outputs — whether in evidence_log.md, markdown cells, or memos — are
tagged with one of four classifications:
TagMeaningExampleOObservedDirect count or value from the dataDDerivedCalculated from observed valuesIInferredReasoned from patterns, not directly measuredAAssumedDesign decision made without data confirmation
No output may assert provider intent, fraud, or wrongdoing under any tag.

Rule 10 — No Emojis or Special Characters in Code or Print Statements
Use plain text only in all code cells and print statements.
Approved status indicators:

PASS
FAIL
WARNING
NOTE
COMPLETE

Prohibited:

Checkmarks, cross marks, warning symbols (e.g. ✓ ✗ ⚠)
Arrows or decorative Unicode characters (e.g. → ← ★)
Emoji of any kind

Rationale: Special characters cause encoding issues across environments
and are unprofessional in a program integrity context.

Rule 11 — Null Z-Score Handling
Null z-scores in signal columns are treated as 0 — signal did not fire.
pythondf['signal_col'] = df['signal_col'].fillna(0)
# Rationale: null z-score means signal could not be calculated,
# typically due to zero denominator. Does not mean signal fired
# negatively. Treatment: no contribution to score. Tag: A

Fill must occur in the VALIDATE section, not SCORE
Document null counts and fill action in evidence_log.md tagged A
If null count changes from baseline, flag as WARNING in validate output


Rule 12 — Diagnostic Cells Are Temporary
Cells prefixed with DIAG are temporary troubleshooting tools only.

Use format: # [MODULE]-DIAG-[nn] (e.g. # DPI2-DIAG-01)
Delete all DIAG cells before committing notebook to GitHub
Never include DIAG cells in a final notebook
DIAG cells do not appear in notebook_map.md


Rule 13 — Confirm Population Counts From Data
Never assume population counts, tier distributions, or file contents
from documentation alone. Always confirm from the actual data before
writing asserts, comments, or session notes.
python# Always run this pattern when loading a new file
print(f"Row count: {len(df):,}")
print(df['tier_column'].value_counts().sort_index())

SESSION_CONTEXT.md descriptions may lag behind actual file contents
If actual counts differ from documented counts, correct the
documentation before proceeding — do not adjust the assert to
match wrong documentation
Confirm and correct in this order: data first, then asserts,
then comments, then SESSION_CONTEXT.md


This file is part of the project knowledge base.
Upload to the Medicare Integrity Decision Support Claude Project.
Do NOT push to GitHub — internal working document.

Rule 12 — Notebook Cell Execution Protocol
Notebooks are built and executed one cell at a time. No exceptions.
The sequence for every cell is:

Claude produces one cell — markdown or code — and nothing else
User copies cell into Jupyter and runs it
User confirms output or reports any errors
Claude addresses errors if present before proceeding
Only after confirmation does Claude produce the next cell

This protocol applies to every notebook in every module. Building multiple cells at once 
prevents error isolation, makes troubleshooting impossible, and removes the 
user's ability to verify analytical outputs as they are produced.
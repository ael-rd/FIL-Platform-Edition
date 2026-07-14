# FIL Framework — Changelog
> Current version: **3.4.1**

---

## V3.4.1 — June 2026 · Versioned File Saves (PATCH)

### Problem fixed
LOG_ERRORS and SOP-06_DOMAIN were overwritten at every Step 7.
No Drive history · risk of data loss on corruption.
DYNAMIC had TIMESTAMP prefix but inconsistent with other files.

### Solution: append-only versioned naming for 3 files

**NAMING CONVENTION:**
  YYYYMMDD_[PROJECT]_[FILETYPE]_[N].md
  N = sequential within day · starts at 1 · resets daily
  Never overwrite · Never delete

**Versioned files:**
  YYYYMMDD_[PROJECT]_DYNAMIC_N.md     (every session · was [TIMESTAMP]_...)
  YYYYMMDD_[PROJECT]_LOG_ERRORS_N.md    (if errors logged · was flat overwrite)
  YYYYMMDD_[PROJECT]_SOP-06_DOMAIN_N.md (if domain updated · was flat overwrite)

**SESSION_INDEX stays flat (pointer):**
  [PROJECT]_SESSION_INDEX.md → DYNAMIC_FILE · LAST_LOG_ERRORS_FILE · LAST_SOP06_FILE

**New SESSION_INDEX fields:**
  LAST_LOG_ERRORS_FILE · LAST_SOP06_FILE (pointers to most recent versioned files)

**New SOPs in SOP_TEMPLATE:**
  SOP-FILE-RESOLVE : load most recent versioned files at boot
  SOP-FILE-SAVE    : append-only save at Step 7

**SOP-00C updated:**
  First session creates all 3 initial versioned files on Drive

**PE instruction files:**
  VERSIONED FILE MECHANICS block added

**Date source:** system context (available in Claude · operator confirms if absent)

---

## V3.4.0 — June 2026 · Governance Depth + Onboarding (MINOR)

### New files
- `FIL_QUICKSTART.md` (AXE 3) — single entry point for new users
  → FIL = "protocole de gouvernance persistante" (not "mémoire")
  → README.md updated to point here first
- `LIMITATIONS.md` (AXE 10) — documented structural limits
  → Inter-session fidelity · inter-LLM fidelity · context window · anti-injection
  → Manual editing coherence · simultaneous collaboration · prerequisites
- `DYNAMIC_LITE_TEMPLATE.md` (AXE 9) — minimal FIL for solo short projects
  → Steps 0/1/2/7 only · no SOP · no NCGL · no QC · no Handoff
  → Fully compatible with FIL FULL · no migration on upgrade

### STABLE_TEMPLATE (AXE 4 + AXE 8)
- AXE 8: two config sections merged → single ## SYSTEM CONFIGURATION
  Subsections: Persistence · Operators · Namespaces · Handoff · Quality Control
  QC fields integrated (were orphaned in STABLE)
- AXE 4: STABLE_VERSION + STABLE_MODIFIED fields added
  Incremented by LLM at every STABLE modification in session

### SESSION_INDEX_TEMPLATE (AXE 4)
- STABLE_VERSION_AT_LOAD field added
  Step 0 compares loaded STABLE version → divergence → warn operator

### DYNAMIC_TEMPLATE (AXE 4 + 5 + 6 + 7)
- AXE 4: Step 7 ⑩ — STABLE_VERSION increment on modification
- AXE 5: Step 4 ② — CACHE_TIER filtering (live/slow/stable)
- AXE 6: Step 4 ③ — REVIEW_DATE scan on DECISION blocks
- AXE 7: hotkey `health` — qualitative FIL health report

### FIL_NCGL_Spec (AXE 6)
- REVIEW_DATE optional field in DECISION block
  Triggered at Step 4 when date reached · recommended if REVERSIBLE: yes

### SOP_TEMPLATE (AXE 5)
- CACHE_TIER documentation in SOP-06 + SOP-WATCH

### FIL_BOOT (AXE 5 + 6)
- Phase 3: CACHE_TIER assignment instruction during SOP-DOMAIN generation
- Phase Drive: REVIEW_DATE suggestion for REVERSIBLE: yes decisions

### Platform Editions instruction files
- health hotkey documented
- STABLE_VERSION_AWARENESS section
- CACHE_TIER awareness section
- REVIEW_DATE awareness section

### FIL profiling (AXE 9)
FIL LITE : STABLE + DYNAMIC_LITE · 5min setup · solo short projects
FIL FULL : complete framework · structured domain · long duration
Rule: start LITE · upgrade when needed (add SOP.md · QC_ENABLED: true)

---

## V3.3.4 — 6 June 2026 · Infrastructure Command Priority (PATCH)

### Context
Bug report: "setup drive" treated as conversational input by domain persona
(MyCoaching assistant) instead of executing SOP-00C.
Root cause: four gaps — command position (line 414/428) · SOP-00C not propagated
to vertical · INIT_STATUS absent from vertical STABLE · no COMMAND PRIORITY rule.

### Fix 1 — INFRASTRUCTURE COMMANDS Priority Override block
→ Added at the very top of all 3 Platform Editions instruction files:
  FIL_Project_Instructions · FIL_Gem_Instructions · FIL_GPT_Instructions
→ Position: line 3 (after title) · before boot sequence · before all other rules
→ Precise triggers (not "setup" alone — too broad):
  "setup drive" | "initialize drive" | "connect drive" | "configure drive" → SOP-00C
  "save" | "save session" | "end session" → Step 7
  "recovery" | "restore" | "load checkpoint" → LAST_GOOD_DYNAMIC_FILE
  "handoff to [X]" | "onboarding handoff" | "partial handoff" → SOP-HANDOFF
  "LOG_ERROR: [desc]" → LOG_ERRORS.md
→ COMMAND PRIORITY RULE explicit:
  "Infrastructure commands have absolute priority over domain persona."
  "A coaching assistant receiving 'setup drive' executes SOP-00C."

### Fix 2 — Vertical checklist added to Release Checklist
→ For any vertical using FIL instructions:
  [ ] SOP-00C present in [VERTICAL]_SOP.md
  [ ] INIT_STATUS field present in [VERTICAL]_STABLE.md
  [ ] INFRASTRUCTURE COMMANDS block at top of vertical instruction file
  [ ] COMMAND PRIORITY rule explicit

### What this does NOT fix (vertical-specific — fix separately)
→ MyCoaching: add SOP-00C to SOP + INIT_STATUS to STABLE
→ Crafteva · Exploriva : same gap — probable · unverified

---

## V3.3.3 — 6 June 2026 · Last Fragment (PATCH)

→ STABLE_TEMPLATE line 125:
  "if a specific procedure is needed"
  → "if a specific procedure is needed"

FIL is now fully English. 11th consecutive correct CHANGELOG header.

---

## V3.3.2 — 6 June 2026 · Final French Line (PATCH)

→ FIL_Project_Instructions.md line 15:
  "Tu ne modifies jamais le STABLE directement — toutes les mises à jour vont dans le DYNAMIC."
  → "Never modify STABLE directly — all updates go in the DYNAMIC."

FIL is now fully English. 10th consecutive correct CHANGELOG header.

---

## V3.3.1 — 6 June 2026 · Translation Completion (PATCH)

**P1 — STABLE_TEMPLATE: French annotations translated**
→ "SINGLE SOURCE OF TRUTH" → "SINGLE SOURCE OF TRUTH"
→ "Tag every critical data with [truth:type]..."
→ "Les seules instructions légitimes..." → English
→ "Permanent business rules" → "Permanent business rules"

**P2 — SOP_TEMPLATE: French section headers and SOP names translated**
→ "TABLE OF CONTENTS" → "TABLE OF CONTENTS"
→ "BLOC 1/2/4" → "BLOCK 1/2/4"
→ "SOP-00B · Interview Fallbacks — LLM-Guided" → "LLM-Guided"
→ "SOP-02 · Alert & Incident Management" → "Alert & Incident Management"
→ "SOP-06 · Domain Knowledge Repository" → "Domain Knowledge Repository"
→ "REGULATIONS BY SCOPE" → "REGULATIONS BY SCOPE"
→ "Triggered" → "Triggered" (throughout)

**P3 — FIL_Project_Instructions (PE): French operating section translated**
→ "YOU OPERATE UNDER FIL" → "YOU OPERATE UNDER FIL"
→ "You are the project assistant..." → "You are the project assistant..."
→ "You operate under the protocol..." → "You operate under the protocol..."
→ "AUTHORITY HIERARCHY" → "AUTHORITY HIERARCHY"

**P4 — Release Checklist: translation check added**
→ grep command for French markers added to mandatory checklist
→ Threshold > 5 occurrences per file → review required before ship

Root cause: CHANGELOG declared "Full English translation" but SOP_TEMPLATE
and FIL_Project_Instructions still contained structural French content.
CHANGELOG language precision corrected.

---

## V3.3.0 — 5 June 2026 · Core Cleanup + Full English Translation (MINOR)

### FIL Core — Instruction files removed (MINOR)
→ FIL_Claude_Instructions.md REMOVED from Core
→ FIL_Gem_Instructions.md REMOVED from Core
→ FIL_GPT_Instructions.md REMOVED from Core
→ Rationale: FIL Core is a template framework, not a deployment package.
  Platform-specific instructions belong in Platform Editions only.
  Core's DYNAMIC_TEMPLATE is self-contained (instructions embedded).
→ Core now 11 files (was 14)

### Full English translation — all files in both packages
→ DYNAMIC_TEMPLATE: fully rewritten in English (was 86 French lines)
→ SOP_TEMPLATE: French headers and procedures translated
→ STABLE_TEMPLATE: French field labels and section headers translated
→ FIL_NCGL_Spec: French terminology translated
→ SESSION_INDEX_TEMPLATE: remaining French translated
→ FIL_DYNAMIC_DataOnly_Template: French section headers translated
→ FIL_Project_Instructions: French phrases translated
→ README_Platform_Editions: French translated
→ FIL_Gem_Instructions (editions): French translated
→ FIL_GPT_Instructions (editions): French translated

---

## V3.2.2 — 5 June 2026 · Release Checklist (PATCH)

**Root cause of the recurring patch pattern formally resolved.**

Three consecutive MINOR releases (V3.0.0 · V3.1.0 · V3.2.0) each required
an immediate patch because mechanics were added to templates/SOPs but
not propagated to the 6 instruction files (the L1/L2 execution layer).

**Fix: mandatory RELEASE CHECKLIST added to UPDATE PROTOCOL**
→ If any mechanic added to DYNAMIC · SOP · STABLE:
  Must appear in all 6 instruction files before zipping.
  [ ] FIL_Claude_Instructions.md · FIL_Gem_Instructions.md · FIL_GPT_Instructions.md
  [ ] FIL_Project_Instructions.md · FIL_Gem_Instructions.md (PE) · FIL_GPT_Instructions.md (PE)
  Unchecked box = do not ship.

No functional changes. CHANGELOG UPDATE PROTOCOL updated only.

---

## V3.2.1 — 5 June 2026 · Full V3.2.0 Runtime Propagation (PATCH)

**Canonical phrase propagated to all files:**
"Never infer logical ownership from physical location.
 Always infer ownership from namespace + OP-ID.
 Physical location resolved only after PERSISTENCE_MODE is read."

**P0 — resolve_filename() defined and injected into all 6 instruction files**
→ Single resolution function covering: SESSION_INDEX · DYNAMIC · LOG_ERRORS · HANDOFF
→ Resolves: op_suffix (single/multi) · base filename · storage location · search scope
→ All instructions reference resolver, never hardcode paths

**P0 — Step 0A "Namespace & Persistence Resolution" in all 6 instruction files**
→ Read PROJECT_NAME · OPERATOR_MODE · OPERATOR_ID · PERSISTENCE_MODE from STABLE
→ Resolve all filenames before any Drive operation
→ Determine search scope by PERSISTENCE_MODE, not platform

**P0 — SESSION_INDEX_TEMPLATE**
→ Added: PERSISTENCE_MODE · LOGICAL_NAMESPACE · OPERATOR_NAMESPACE fields
→ Search protocol: resolver-based (not platform-centric)
→ Platform-specific examples replaced with resolve_filename() references

**P2 — STABLE canonical section PERSISTENCE & NAMESPACE CONFIGURATION**
→ SHARED_NAMESPACE_FILES · SOVEREIGN_NAMESPACE_FILES · HANDOFF_NAMESPACE_FILES
→ Single authoritative declaration of namespace membership per file type

**P2 — HANDOFF_TEMPLATE namespace fields**
→ SOURCE_NAMESPACE · TARGET_NAMESPACE · SOURCE_PERSISTENCE_MODE · TARGET_PERSISTENCE_MODE
→ IMPORT_REQUIRED: yes (explicit invariant reminder)

**P2 — IMPORT STAGING namespace annotation**
→ "Namespace: STAGING · Authority: L4 · Never merged automatically"

**P3 — "Drive-First Architecture" renamed to "Persistence-Abstraction Architecture"**
**P3 — Official persistence mode matrix added (Claude/Gemini/GPT · default ≠ limitation)**

---

## V3.2.0 — 5 June 2026 · Persistence Independence + Namespace Semantics (MINOR)

### No new mechanics — conceptual formalization only

**PERSISTENCE_MODE in STABLE**
→ folder | flat | manual  (read from STABLE at every boot)
→ Flat is first-class strategy, not a Gemini fallback
→ All three modes produce identical cognitive protocol behavior
→ Claude may use flat mode · Gemini may use folder mode if available

**FIL NAMESPACE SEMANTICS (SOP_TEMPLATE)**
→ Four namespaces formally defined:
  SHARED    : STABLE · SOP — read-only · all operators
  SOVEREIGN : SESSION_INDEX · DYNAMIC · LOG_ERRORS — one operator · OP-ID suffix
  HANDOFF   : transit objects between two operators
  STAGING   : IMPORT STAGING section · receiving operator · until validated
→ OP-ID suffix = formal sovereignty marker (not just naming convention)
→ In flat mode: SHARED vs SOVEREIGN distinguished by OP-ID suffix alone

**PERSISTENCE ABSTRACTION BLOCK in all 6 instruction files**
→ Instructions now branch on PERSISTENCE_MODE from STABLE
→ Removed: "gdrive_search in project folder" as primary pattern
→ Added: mode-aware paths (folder | flat | manual)

**Formal separation**
→ logical_topology ≠ physical_persistence_topology
→ Namespace semantics are persistence-independent
→ cognitive_protocol(S) identical regardless of PERSISTENCE_MODE

**Root cause addressed**
→ V3.1.x treated folder = Claude (primary) · flat = Gemini (exception)
→ V3.4.1: folder and flat are equal valid strategies
→ FIL is no longer implicitly Claude-native in its persistence model

---

## V3.1.1 — 5 June 2026 · Runtime Propagation Patch (PATCH)

**P1 — Operator runtime block added to all 6 instruction files**
→ FIL_Claude_Instructions · FIL_Gem_Instructions · FIL_GPT_Instructions
→ FIL_Project_Instructions (Editions) + Gem + GPT Editions
→ Each now contains: OPERATOR_ID · OPERATOR_MODE · file naming rules
   handoff triggers · IMPORT STAGING boot check · new operator detection

**P2 — SESSION_INDEX naming convention updated in instructions**
→ Single mode: [PROJECT]_SESSION_INDEX.md (no suffix · backward-compatible)
→ Multi mode:  [PROJECT]_SESSION_INDEX_[OPERATOR_ID].md
→ DYNAMIC: [TIMESTAMP]_[PROJECT]_DYNAMIC[_OP-ID if multi].md
→ Shared files (STABLE · SOP): never get OP-ID suffix

Root cause: V3.4.1 propagated mechanics to templates and SOPs
but not to instruction files (L1/L2 layer). Instructions are the
execution layer — if they don't mention handoff, the LLM won't execute.

---

## V3.1.0 — 5 June 2026 · Flat Handoff + Parallel Operators (MINOR)

### Context
V3.0.1 introduced Handoff Protocol (sequential transfer).
V3.4.1 adds: flat-file compatibility (Gemini) · parallel operator support
(Scenario B) · onboarding handoff · partial exchange · import staging.

### New mechanics

**Flat-file handoff filename (platform-agnostic)**
→ [TIMESTAMP]_[PROJECT]_HANDOFF_[OP-A]_TO_[OP-B].md
→ Onboarding: [TIMESTAMP]_[PROJECT]_HANDOFF_ONBOARDING_[OP-A]_TO_[OP-B].md
→ Partial:    [TIMESTAMP]_[PROJECT]_HANDOFF_PARTIAL_[OP-A]_TO_[OP-B].md
→ Gemini: flat at Drive root · no folders required · same format

**OPERATOR_MODE: single | multi (STABLE)**
→ single: one operator · backward-compatible with V3.0.x
→ multi: several operators · files distinguished by OP-ID suffix
→ DEFAULT_OPERATOR_ID: OP-PRIMARY (initialized at project creation)
→ SHARED_DRIVE_FOLDER: true when multi

**OP-ID naming convention (multi mode)**
→ [PROJECT]_SESSION_INDEX_[OP-ID].md  (sovereign per operator)
→ [TIMESTAMP]_[PROJECT]_DYNAMIC_[OP-ID].md  (sovereign per operator)
→ [PROJECT]_LOG_ERRORS_[OP-ID].md  (sovereign per operator)
→ [PROJECT]_STABLE.md + [PROJECT]_SOP.md  (shared · no OP-ID)

**IMPORT STAGING section (DYNAMIC Hot Zone)**
→ Temporary zone for handoff imports pending individual validation
→ accept → PREVENTION ACTIVE or DECISIONS · reject → LOG_ERRORS · defer → [v:date]

**SOP-HANDOFF: two new procedures**
→ Onboarding handoff: existing operator generates bootstrap for new joiner
   Existing operator continues · new operator starts in parallel
→ Partial handoff: exchange a specific pattern between parallel operators
   Not a full transfer · no obligation to accept

**FIL_BOOT Phase 0: existing project detection**
→ Detects STABLE in Drive at startup
→ Offers: fresh start (STABLE+SOP only) OR onboarding handoff request
→ Asks for OPERATOR_ID before creating SESSION_INDEX

### Backward compatibility
→ Single-operator projects: zero migration required
→ SESSION_INDEX without OP-ID suffix: still valid (single mode)
→ STABLE without OPERATOR_MODE: treated as OPERATOR_MODE: single

---

## V3.0.1 — 5 June 2026 · Handoff Integration Fix (PATCH)

**P1 — STABLE_TEMPLATE: OPERATOR_ID fields added**
→ OPERATOR_ID          : unique operator identifier · used in PROVENANCE tags
→ HANDOFF_ENABLED      : true | false
→ HANDOFF_SEVERITY_CAP : high  (imported [critical] capped at [high] by default)

**P2 — DYNAMIC_TEMPLATE: Step 7 handoff trigger added**
→ If user requested "handoff" or "transfer project" in session:
  Generate [TIMESTAMP]_[PROJECT_NAME]_HANDOFF.md via SOP-HANDOFF Export
  OPERATOR_ID from STABLE → OPERATOR_SOURCE in TRUST METADATA
  Save to Drive · Confirm with import instructions

**Root cause**: replacement strings in build script didn't match actual file content.
Both gaps were coherent — DYNAMIC trigger had no OPERATOR_ID to reference.

---

## V3.0.0 — 5 June 2026 · FIL Handoff Protocol (MAJOR)

### New paradigm: first multi-operator capability

FIL has been single-operator since V1.0.0.
V3.4.1 introduces the first governed mechanism for transferring
a project between two operators while preserving cognitive stability.

### New file: HANDOFF_TEMPLATE.md

Structured handoff object containing:
→ Section 1: HANDOFF CONTEXT (narrative · not a DYNAMIC dump)
→ Section 2: Exported DYNAMIC state (Hot Zone condensed)
→ Section 3: PREVENTION ACTIVE (with [source: OP-A] provenance tags)
→ Section 4: Active DECISIONS (with HANDOFF_STATUS per decision)
→ Section 5: QC STATUS (error log overview)
→ Section 6: TRUST METADATA (L4 authority · export date · QC pass status)
→ Section 7: Import instructions for Operator B

### New SOP: SOP-HANDOFF

Export procedure (Operator A):
→ Generate HANDOFF_TEMPLATE.md from current state
→ Abstraction constraint: PREVENTION ACTIVE notes must retain domain signal
→ Save to Drive: [TIMESTAMP]_[PROJECT_NAME]_HANDOFF.md

Import procedure (Operator B):
→ Validate PREVENTION ACTIVE one by one: accept | reject | conflict
→ Validate DECISIONS one by one: accept | reject | review
→ Create own SESSION_INDEX fresh — never copy Operator A's
→ Full sovereignty preserved

SEVERITY capping rule:
→ All imported content enters at L4
→ Imported [critical] → capped at [high] until Operator B confirms recurrence
→ SOP-QC Recurrence Detection elevates automatically if pattern recurs

What is NOT transferred (by design):
→ DYNAMIC · SESSION_INDEX · L1/L2 governance · Full LOG_ERRORS · NCGL blocks

### STABLE updates
→ OPERATOR_ID field (used in PROVENANCE tags)
→ HANDOFF_ENABLED · HANDOFF_SEVERITY_CAP config

### DYNAMIC / DataOnly updates
→ Step 7: handoff trigger if user requested handoff in session

---

## V2.4.0 — 5 June 2026 · Abstraction Drift Constraint (MINOR)

**Design principle: no new governance mechanics in V2.4**
→ V2.4 refines existing mechanics — does not add governance layers
→ 8th design principle: governance must resist its own complexity

**Abstraction drift constraint on macro-consolidation**
→ Macro-notes must retain ≥1 domain-specific signal
→ Test: would this macro have caught each individual error it replaces?
→ Too-abstract macros rejected (e.g. "verify data before statements")
→ Valid: retain domain scope, specific data type, specific action
→ Prevents consolidation from degrading operational precision over time

**Four-level governance hierarchy formalized**
→ Error → Prevention → Macro-Prevention → Arbitration
→ Patterns now compete: rival · eliminate (SUPERSEDED) · replace (macro)
   · escalate (severity elevation) · inherit (SUPERSEDED_BY chain)
→ "Adaptive selection" replaces "behavioral accumulation"

**8th design principle**
→ "Governance must resist its own complexity"
→ Each version should add fewer mechanics than the previous
→ The system should govern less as it matures, not more

**Theoretical reframing**
→ "attentional regulation system" → "operational theory of LLM cognitive stability"
→ 5-step question evolution documented in conclusion:
   memory → governance → attention → selection → self-correction

---

## V2.3.0 — 5 June 2026 · Contradiction Management (MINOR)

**New: SOP-QC Contradiction Management**
→ Detect semantic conflicts in PREVENTION ACTIVE (same domain + opposing guidance)
→ 4 arbitration rules: SEVERITY → DATE → recurring → CONFLICT_UNRESOLVED (user)
→ CONFLICT_UNRESOLVED: user consulted — never resolved silently

**New: SUPERSEDED status**
→ Losing pattern: condensed note removed from PREVENTION ACTIVE
→ Full entry: RESOLVED ERRORS (never deleted) · SUPERSEDED_BY field for traceability
→ Reactivatable if winning pattern is later resolved or deprecated

**LOG_ERRORS_TEMPLATE V3.4.1**
→ CONFLICTS section added (format + status tracking)
→ CONFLICTS_DETECTED / CONFLICTS_RESOLVED counters in QC STATS

**Tripartite detection**
→ Step 4 (boot) · Step 7 (save) · LOG_ERROR: hotkey — continuous, not periodic

**Theoretical reframing**
→ "cognitive stabilization layer" → "attentional regulation system"
→ FIL's core question: "what to keep active in attention" not "what to remember"

---

## V2.2.0 — 5 June 2026 · Compression & Error Weighting (MINOR)

**SEVERITY becomes operational (not decorative)**
→ [critical] → Step 1 · every session · never compressed
→ [high]     → Step 4 active scan · every session · never compressed
→ [medium]   → Step 4 silent · compression after 90d resolved
→ [low]      → Step 4 silent · compression after 60d resolved

**PREVENTION ACTIVE — condensed scan layer**
→ Step 4 scans condensed notes (1 line each) not full entries
→ scan_cost: O(N×lines) V2.1.0 → O(N) V3.4.1
→ Full entries remain in RESOLVED ERRORS (never deleted)

**Compression protocol**
→ Eligible: severity∈{low,medium} AND age_resolved > threshold
→ Ineligible: critical · high · recurring
→ Checked at Step 7 every session

**Recurrence detection**
→ Match in PREVENTION ACTIVE → fetch full entry → STATUS: recurring
→ SEVERITY elevated one level on recurrence
→ Detection guaranteed regardless of time since compression

**Phase 3B: SEVERITY assigned at domain category generation**

---

## V2.1.0 — 22 May 2026 · Quality Control & Error Logging (MINOR)

**New: LOG_ERRORS_TEMPLATE.md**
→ Two-level: FIL-level (7 pre-populated) · Domain-level (Phase 3B generated)
→ LOG_ERROR: hotkey always active regardless of QC_ENABLED

**New: SOP-QC**
→ SYSTEMATIC / PREFERENCE / PUNCTUAL classification
→ Error entry: DESCRIPTION · CAUSE · CORRECTION · PREVENTION
→ Session scan (boot · save · or both)

**STABLE: QC configuration section**
→ QC_ENABLED · LOG_ERRORS_FILE · QC_TRIGGER_IMPLICIT · QC_SESSION_SCAN

**DYNAMIC / DataOnly**
→ LOG_ERROR: added to hotkeys
→ Step 4: QC scan · Step 7: QC check before save
→ Rule QC: implicit error signal detection

**FIL_BOOT Phase 3B**
→ Generates domain-specific error categories from SOP-DOMAIN context
→ Initializes [PROJECT_NAME]_LOG_ERRORS.md on Drive

---

## V2.0.0 — 22 May 2026 · FIL-NCGL (MAJOR)

**Breaking changes**
→ SESSION_INDEX: 4 new NCGL fields (NCGL_STATUS · NCGL_LAST_VALIDATION · NCGL_BLOCKS_HOT · NCGL_BLOCKS_WARNINGS)
→ DYNAMIC format: new NCGL BLOCKS ACTIVE section in Hot Zone
→ Boot: Step 0B added (silent if no blocks)

**FIL-NCGL V1.0.0**
→ 6 block types: TASK · FACT · ALERT · WORKFLOW · WATCH · DECISION
→ Soft validation: OK · WARN · REVIEW · BLOCK
→ Golden rule: "structure only if it improves reliability without reducing readability"
→ Usage threshold: 2+ of 4 conditions (PRIORITY · EXPECTED_BEHAVIOR · VALIDITY · multi-session)
→ Fully backward-compatible with inline [truth:*] [v:*]

**New: FIL_NCGL_Spec.md**

**Full English translation · zero vertical mentions**
→ [PROJECT_NAME] placeholders throughout · generic examples only

---

## V1.7.1 — 22 May 2026 · Phase 4 Verify & Launch · Quick Starts (PATCH)

**Phase 4 — FIL_BOOT**
→ 4A: Drive MCP test (discover problems before session 2)
→ 4B: Session 2 annotated preview (not live demo on empty data)
→ 4C: First Drive save → SESSION_INDEX created → proof
→ 4D: Handoff → exact instructions for session 2 · 3-session framework

**Quick Starts (new files per vertical)**
→ 5 questions · ~3 minutes · FIL invisible
→ Crafteva_BOOT_QuickStart.md · Exploriva_BOOT_QuickStart.md 
→ Post-QuickStart: never mention FIL · speak business language only

---

## V1.7.0 — 22 May 2026 · Drive-First Architecture (MINOR)

**Project Knowledge: 2 files only**
→ STABLE.md (contains Drive folder ID — bootstrap · must stay)
→ SOP.md (universal procedures)

**Drive (auto at boot)**
→ SESSION_INDEX · DYNAMIC · [active context per vertical]

**Drive (on demand)**
→ FIL_BOOT · SESSION_INDEX_TEMPLATE · domain files

**Why STABLE stays in Knowledge**: chicken-and-egg — contains Drive ID needed to access Drive

---

## V1.6.x — 22 May 2026 · Hotkeys · SESSION_INDEX · SOP-DOMAIN · Persistence

**V1.6.2** — Audit fixes: truncated line (DYNAMIC Step 7) · STABLE LAST_SESSION_TIMESTAMP removed · chronic versioning (README · FIL_BOOT · CHANGELOG_TEMPLATE)

**V1.6.1** — SESSION_INDEX minimal runtime pointer: DYNAMIC_FILE · LAST_GOOD_DYNAMIC_FILE · LAST_SAVE_STATUS · SESSION_MODE · search+update protocol (never create alone)

**V1.6.0** — Hotkeys PIN/ARCHIVE/FORGET + CAPTURE IN PROGRESS buffer · SOP-06 DOMAIN pattern (Phase 3) · SOP-WATCH (domain watch · [v:refresh] scan) · Platform-differentiated persistence (Claude Drive+Gmail · Gemini root · GPT degraded)

---

## V1.5.x — 21 May 2026 · Platform Editions · Instructions · Audit Fixes

Drive timestamp-prefix (Gemini compatible) · FIL Platform Editions packaging · Claude/Gem/GPT instruction files · versioning checklist · 6 audit cycles on Crafteva and Exploriva

---

## V1.4.0 — 20 May 2026 · Hot/Warm/Cold Memory Architecture

3-zone memory system with explicit eviction policies and temporal validity tags [v:type]

---

## V1.3.x — 20 May 2026 · Anti-Injection Security

8 security mechanisms: L1-L5 hierarchy · pattern scan · Drive sandboxing · zone isolation · non-substitution · injection logging · STABLE warning · SOP-07

---

## V1.0.0 → V1.2.0 — 14-20 May 2026 · Foundation

Creation · fallbacks · Truth Protocol [truth:*] · FIL_BOOT interview · Live Data Layer · timestamped Drive persistence

---

## UPDATE PROTOCOL

```
Before packaging any version:
1. IDENTIFY: MAJOR / MINOR / PATCH
2. UPDATE this file (header + single entry per version)
3. VERSION CHECK — MANDATORY:
   → Check first line of every file
   → Replace 'VX.Y.Z' AND '**X.Y.Z**' (both forms — lesson from V1.5.6 bug)
   → rm -f target zip before creating (prevent silent duplicates)

4. RELEASE CHECKLIST — MANDATORY FOR MINOR AND MAJOR:
   If any mechanic was added to DYNAMIC, SOP, or STABLE →
   it MUST be referenced in all 6 instruction files before zipping.
   Check each file explicitly:
   → [ ] FIL_Claude_Instructions.md       (Core)
   → [ ] FIL_Gem_Instructions.md          (Core)
   → [ ] FIL_GPT_Instructions.md          (Core)
   → [ ] FIL_Project_Instructions.md      (Platform Editions)
   → [ ] FIL_Gem_Instructions.md          (Platform Editions)
   → [ ] FIL_GPT_Instructions.md          (Platform Editions)
   If any box is unchecked → DO NOT ship · propagate first.
   Root cause of V3.0.1 · V3.1.1 · V3.4.1 patches: this checklist was missing.

5. GENERATE FIL_Framework_VX.Y.Z.zip
6. ANNOUNCE: "📦 FIL_Framework_VX.Y.Z.zip generated"

DO NOT replace version numbers inside historical changelog entries.
Only replace in headers, footers, and first lines.
```

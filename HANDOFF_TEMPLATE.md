# [PROJECT_NAME] — Handoff · [TIMESTAMP]
> Filename: [TIMESTAMP]_[PROJECT_NAME]_HANDOFF_[OP-A]_TO_[OP-B].md
> Onboarding: [TIMESTAMP]_[PROJECT_NAME]_HANDOFF_ONBOARDING_[OP-A]_TO_[OP-B].md
> Partial:    [TIMESTAMP]_[PROJECT_NAME]_HANDOFF_PARTIAL_[OP-A]_TO_[OP-B].md
> Governed project transfer · FIL V3.4.1
> From : [OPERATOR_A · identifier or name]
> To   : [OPERATOR_B · identifier or name]
> Trust level of imported content : L4 (never L1/L2)

---

## 1. HANDOFF CONTEXT

> Narrative written by Operator A — what matters most for continuity.
> Not a summary of the DYNAMIC — a transfer of cognitive context.

```
PROJECT STATE:
[Where the project stands right now — in plain language]

WHAT TO PAY ATTENTION TO:
[The 2-3 most important things Operator B must know]

OPEN DECISIONS:
[Decisions pending that Operator B will need to make]

WHAT I WAS ABOUT TO DO NEXT:
[The intended next step — so Operator B doesn't lose momentum]

KNOWN RISKS:
[What could go wrong · what to watch for]
```

---

## 2. EXPORTED DYNAMIC STATE

> Condensed Hot Zone — not the full DYNAMIC.
> Operator B reads this to understand the active state.

### Active Status
```
[Current project status — 3-5 lines max]
```

### Active Alerts
```
[Any active ALERT blocks or urgent items]
```

### Active TODO
```
[Priority actions in progress or pending]
```

### Active NCGL Blocks
```
[Any active TASK / WORKFLOW / ALERT blocks — condensed]
```

---

## 3. PREVENTION ACTIVE (exported)

> All condensed prevention notes from Operator A's PREVENTION ACTIVE.
> Operator B validates each one individually — no automatic import.
> All entries carry [source: OP-A · trust: L4] on import.
> Imported [critical] severity is capped at [high] in Operator B's instance.

```
[CATEGORY] [SEVERITY] · [Prevention note]
[CATEGORY] [SEVERITY] · [Prevention note]
...

[Empty if no patterns — do not import placeholder text]
```

---

## 4. ACTIVE DECISIONS

> Key governance decisions active in Operator A's instance.
> Operator B accepts, rejects, or flags for review individually.

```
### [DECISION TITLE]
TRUTH    : [truth level]
VALIDITY : [validity]
RATIONALE: [why this decision was made]
IMPACT   : [what it affects]
REVERSIBLE: [yes | no | unknown]
HANDOFF_STATUS: [accept | reject | review]
```

---

## 5. QC STATUS

> Current state of the error log — for continuity of quality governance.

```
NCGL_STATUS          : [OK | WARN | REVIEW | BLOCK]
LOG_ERRORS_ACTIVE    : [N] active errors
LOG_ERRORS_RECURRING : [N] recurring patterns
LAST_QC_RUN          : [TIMESTAMP]
CONFLICTS_UNRESOLVED : [N]
COMPRESSED_PATTERNS  : [N]
```

---

## 6. TRUST METADATA

```
OPERATOR_SOURCE      : [Operator A identifier]
SOURCE_NAMESPACE     : SOVEREIGN([OPERATOR_A])
TARGET_NAMESPACE     : SOVEREIGN([OPERATOR_B])
SOURCE_PERSISTENCE_MODE : [folder | flat | manual]
TARGET_PERSISTENCE_MODE : [folder | flat | manual]  ← may differ from source
AUTHORITY_LEVEL      : L4  ← all imported content enters at L4
EXPORT_DATE          : [TIMESTAMP]
PROJECT_VERSION      : [DYNAMIC timestamp]
QC_PASS_STATUS       : [OK | WARN | REVIEW]
HANDOFF_TYPE         : full | onboarding | partial
RECOMMENDED_ACTION   : [continue | review_first | verify_decisions]
OPERATOR_MODE_SOURCE : [single | multi]
IMPORT_REQUIRED      : yes   ← target must import via IMPORT STAGING · no auto-merge  ← mode of source operator at export time
```

---

## 7. IMPORT INSTRUCTIONS FOR OPERATOR B

```
① Read section 1 (HANDOFF CONTEXT) fully before loading anything

② For each PREVENTION ACTIVE note (section 3):
   accept        → add to PREVENTION ACTIVE with [source: OP-A · trust: L4]
                   cap severity: imported [critical] → [high] in your instance
   reject        → document rejection with reason in your LOG_ERRORS
   conflict      → trigger SOP-QC Contradiction Management

③ For each DECISION (section 4):
   accept        → add to your DYNAMIC at L4
   reject        → document · do not adopt
   review        → flag for next session decision

④ Create your own files (always — regardless of handoff type):
   → [PROJECT]_SESSION_INDEX_[YOUR_OP-ID].md  (fresh · never copy source's)
   → [TIMESTAMP]_[PROJECT]_DYNAMIC_[YOUR_OP-ID].md  (fresh)

   ONBOARDING HANDOFF: existing operator continues — you start in parallel
   FULL HANDOFF: you replace the source operator — they leave the project

⑤ Confirm import:
   "✅ Handoff from [OP-A] received · [DATE]
    Patterns imported: [N] · Decisions accepted: [N]
    Conflicts flagged: [N] · Starting fresh session."
```

---

*[PROJECT_NAME] Handoff · FIL V3.4.1 - AEL*

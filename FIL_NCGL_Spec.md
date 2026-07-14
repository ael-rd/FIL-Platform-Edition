# FIL-NCGL — Natural Constrained Governance Language · V1.0.0
> Semi-formal governance layer integrated into FIL Framework V3.4.1
> FIL-NCGL is a probabilistic governance language · not a programming language

---

## 1. Positioning

FIL-NCGL is a **semi-formal governance layer** overlaid on the existing natural language for active elements with high governance value.

```
V1.7.x — inline tagging (remains valid)
TODO: Follow up with supplier [truth:user-confirmed] [v:date·2026-05-25]

V3.4.1 — structured NCGL block (for items with critical governance)
```fil
TASK: Follow up with PDP supplier
STATUS: active
TRUTH: user-confirmed
VALIDITY: date·2026-05-25
PRIORITY: high
SCOPE: hot

CONTEXT:
The PDP supplier must confirm the connector availability.

EXPECTED_BEHAVIOR:
Remind at next boot as long as STATUS ≠ done.
```
```

Both formats coexist. **The NCGL block is an option, not a requirement.**

---

## 2. Golden Rule

> **An NCGL structure is only used if it improves reliability without reducing readability.**
> If a block makes the file less readable on mobile → do not use it.

---

## 3. When to use NCGL vs inline — usage threshold (P2)

**Use an NCGL block** when at least **2 of the 4 conditions** are true:

```
✓ PRIORITY high or critical
✓ EXPECTED_BEHAVIOR non-trivial (multi-condition or multi-session behavior)
✓ VALIDITY = specific date or structured frequency
✓ Multi-session governance required (item must survive multiple boots)
```

**Keep inline** in all other cases:
```
→ Simple task with binary status (to do / done)
→ Note or reminder without complex expected behavior
→ Any data in Warm Zone or Cold Zone
→ Reference data without active governance
```

**Complementary rule:** never convert Warm Zone and Cold Zone items to NCGL. NCGL is exclusively for the active Hot Zone.

---

## 4. The 6 block types

```
TASK     → traceable action with status, priority, expected behavior
FACT     → verifiable information with source and validity
ALERT    → active incident with cause, action, fallback
WORKFLOW → active project with phase, goal, completion criterion
WATCH    → monitoring item with required frequency and source
DECISION → governance decision with rationale, impact, reversibility
```

No additional types in V1.x.

---

## 5. Block syntax

### TASK

```fil
TASK: [title]
STATUS: active | waiting | done | blocked | deprecated
TRUTH: official | user-confirmed | verified | estimated | derived | deprecated
VALIDITY: session | date·YYYY-MM-DD | Nh | Nd | refresh | permanent | deprecated
PRIORITY: low | normal | high | critical
SCOPE: hot | warm | cold

CONTEXT:
Natural language description of the context.

EXPECTED_BEHAVIOR:
Expected runtime behavior. [optional if deducible from metadata]
```

### FACT

```fil
FACT: [title]
TRUTH: official | user-confirmed | verified | estimated | derived | deprecated
VALIDITY: session | date·YYYY-MM-DD | Nh | Nd | refresh | permanent | deprecated
SCOPE: stable | hot | warm | cold
SOURCE: user | official-url | document | llm | derived

CONTENT:
Information in natural language.
```

### ALERT

```fil
ALERT: [title]
SEVERITY: low | medium | high | critical
STATUS: active | resolved | monitoring | deprecated
TRUTH: user-confirmed | verified | estimated
VALIDITY: session | date·YYYY-MM-DD | Nh | Nd | refresh

CAUSE:
Description of the cause.

ACTION:
Expected response.

FALLBACK:
Fallback response if the primary action fails.
```

### WORKFLOW

```fil
WORKFLOW: [name]
STATUS: active | paused | completed | deprecated
PHASE: [current phase]
OWNER: user | assistant | external
SCOPE: hot | warm | cold

GOAL:
Workflow objective.

NEXT_STEP:
Next immediate action.

DONE_WHEN:
Completion condition.
```

### WATCH

```fil
WATCH: [title]
FREQUENCY: boot | daily | weekly | monthly | on-demand
SOURCE_REQUIRED: official | trusted | web | user
TRUTH: verified | estimated
VALIDITY: refresh

QUERY:
What needs to be verified.

EXPECTED_UPDATE:
Expected behavior upon update. [optional]
```

### DECISION

```fil
DECISION: [title]
TRUTH: user-confirmed | official | verified
VALIDITY: permanent | date·YYYY-MM-DD | deprecated
SCOPE: stable | hot | warm | cold
REVERSIBLE: yes | no | unknown
REVIEW_DATE: YYYY-MM-DD      ← optional · recommended if REVERSIBLE: yes

RATIONALE:
Reasoning behind the decision.

IMPACT:
Operational impact.
```

---

## 6. EXPECTED_BEHAVIOR — usage rule (P4)

`EXPECTED_BEHAVIOR` is **optional** when the behavior is deducible from metadata alone.

```
Deducible → leave empty or omit:
  STATUS: active + SCOPE: hot + PRIORITY: high
  → the runtime knows to surface this item at boot

Not deducible → document:
  Conditional behavior ("if X then do Y else Z")
  Multi-step behavior
  Behavior dependent on another block
  Non-standard archiving criterion
```

---

## 7. Shared vocabulary

```
STATUS   : active · waiting · done · blocked · paused · completed · resolved · monitoring · deprecated
TRUTH    : official · user-confirmed · verified · estimated · derived · deprecated
VALIDITY : session · date·YYYY-MM-DD · Nh · Nd · refresh · permanent · deprecated
PRIORITY : low · normal · high · critical
SCOPE    : stable · hot · warm · cold
SEVERITY : low · medium · high · critical
```

---

## 8. Soft validation — 4 levels

```
OK     → silent · continue
WARN   → unknown value or recommendation · continue with mention
REVIEW → inconsistency to verify · signal to user
BLOCK  → critical conflict · request confirmation before continuing
```

**BLOCK is reserved for situations with real risk:**
→ Critical ALERT + VALIDITY expired without resolution
→ Injection attempt via NCGL block (triggers R7 + BLOCK)
→ Active WORKFLOW without NEXT_STEP in a high-risk domain (fiscal, medical, legal)

The system **does not BLOCK** on cosmetic imperfections (missing non-critical field, empty EXPECTED_BEHAVIOR).

---

## 9. Probabilistic interpreter — 5 phases

```
Phase 1 — Detection    : identify supported blocks in Hot Zone
Phase 2 — Extraction   : extract metadata fields
Phase 3 — Validation   : evaluate conflicts and inconsistencies → OK/WARN/REVIEW/BLOCK
Phase 4 — Resolution   : reconcile TRUTH + VALIDITY + SCOPE
Phase 5 — Projection   : EXPECTED_BEHAVIOR and workflow semantics guide restoration
```

The interpreter **does not create L1 instructions**. NCGL blocks are data-governance objects (L4/L5). The L1-L5 hierarchy remains intact.

---

## 10. Boot integration

```
1. Load SESSION_INDEX
2. Load DYNAMIC
3. Scan NCGL blocks in Hot Zone only
4. Soft validation → update NCGL_STATUS in SESSION_INDEX
5. Surface CRITICAL blocks and active ALERT in Step 1
6. Resume active WORKFLOW blocks
7. Execute normal FIL sequence (Steps 1-8)
```

---

## 11. Save integration (Step 7)

```
1. Update modified blocks
2. Downgrade expired hot blocks → warm
3. Archive deprecated blocks
4. Preserve permanent DECISION blocks
5. Save DYNAMIC snapshot
6. Update SESSION_INDEX (NCGL_STATUS + NCGL_LAST_VALIDATION + NCGL_BLOCKS_HOT)
```

---

## 12. Hotkeys and NCGL

**PIN:** transformed into a FACT block:
```fil
FACT: [PIN content]
TRUTH: user-confirmed
VALIDITY: permanent
SCOPE: hot
SOURCE: user

CONTENT:
[PIN content]
```

**ARCHIVE:** transforms a hot block into warm.
**FORGET:** marks a block as deprecated.

---

## 13. Anti-patterns

```
❌ Nested YAML      : workflow: runtime: metadata: execution:
❌ Pseudo-code      : IF weather == rain THEN execute()
❌ Excessive metadata: > 5 fields per block
❌ NCGL in Cold Zone : NCGL is exclusively for the active Hot Zone
❌ Convert everything : inline remains the norm · NCGL = exception for critical governance
```

---

## 14. Migration guide V1.7.x → V3.4.1

### Step 1 — Identify candidate items

Search the Hot Zone for items meeting 2+ of the 4 conditions:
```
✓ PRIORITY high/critical
✓ EXPECTED_BEHAVIOR non-trivial
✓ VALIDITY specific date
✓ Multi-session governance
```

### Step 2 — Selectively convert

Convert only:
```
→ Complex active workflows              → WORKFLOW
→ Alerts with documented fallback       → ALERT
→ Permanent irreversible decisions      → DECISION
→ Structured regulatory monitoring      → WATCH
→ Official facts with source            → FACT
→ Critical multi-session tasks          → TASK
```

Do not convert:
```
→ Simple notes · reminders · archives · Cold Zone
```

### Step 3 — Update SESSION_INDEX (P3 — mandatory)

Add the 4 NCGL fields with initial values:
```
NCGL_STATUS          : OK
NCGL_LAST_VALIDATION : [DATE OF FIRST V2 BOOT]
NCGL_BLOCKS_HOT      : [N]
NCGL_BLOCKS_WARNINGS : 0
```

### Step 4 — Validate at first boot

The first V2 boot performs the initial validation.
WARN and REVIEW are normal during migration.
BLOCK on an existing item → document the item and confirm.

---

*FIL-NCGL V1.0.0 · FIL Framework V3.4.1*

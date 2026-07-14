# FIL Framework — Platform Editions · V3.4.1
> Three platforms · one framework · unified architecture.

---

## Versioning Convention

```
FIL Framework V3.4.1           → version of the underlying framework
Claude Projects Edition V3.4.1 → version of this specific edition

Both evolve independently.
FIL Framework increments with every core framework improvement.
The edition increments with every change to the deployment mode itself.
```

---

## Interest Analysis per Platform

> **Important note:** FIL is a template meta-framework.
> The value of platform editions is higher for **verticals**
> ([PROJECT_NAME], [PROJECT_NAME]) than for FIL core itself.
> For FIL, the main value is **packaging and consistency**,
> not token savings (no large domain files reloaded each session).

| | Claude Project | Gemini Gem | Custom GPT |
|---|---|---|---|
| **Interest for FIL** | ⭐⭐ Moderate | ⭐⭐ Moderate | ⭐ Limited |
| **Interest for verticals** | ⭐⭐⭐ High | ⭐⭐ Moderate | ⭐ Limited |
| **Permanent STABLE** | ✅ Knowledge | ✅ Knowledge | ✅ Knowledge |
| **Permanent SOP** | ✅ Knowledge | ✅ Knowledge | ✅ Knowledge |
| **Permanent FIL_BOOT** | ✅ Knowledge | ✅ Knowledge | ✅ Knowledge |
| **Auto DYNAMIC** | ✅ Drive MCP | ✅ Drive root | ❌ manual upload |
| **Drive folder creation** | ✅ | ❌ root only | ❌ |
| **Token savings** | ~40% | ~40% | ~20% |

---

## What this zip contains

```
README_Platform_Editions.md          ← this file

── CLAUDE PROJECT ──────────────────────────────────────────────────
FIL_Project_Instructions.md          ← system prompt · Project Instructions
FIL_DYNAMIC_DataOnly_Template.md     ← DYNAMIC data only
FIL_STABLE_Template.md               ← Project Knowledge
FIL_SOP_Template.md                  ← Project Knowledge
CHANGELOG_Template.md                ← project versioning

── GEMINI GEM ──────────────────────────────────────────────────────
FIL_Gem_Instructions.md              ← system prompt · Gem Instructions
(+ FIL_DYNAMIC_DataOnly_Template.md · FIL_STABLE_Template.md shared)

── CUSTOM GPT ──────────────────────────────────────────────────────
FIL_GPT_Instructions.md              ← system prompt · GPT Instructions
(+ FIL_DYNAMIC_DataOnly_Template.md · FIL_STABLE_Template.md shared)
```

---

## Claude Project (⭐⭐ Moderate for FIL)

**What it brings:**
FIL_BOOT.md, STABLE_TEMPLATE, SOP_TEMPLATE in Knowledge — always available.
DYNAMIC = data only (~80 lines vs ~400 in standard mode).
Drive MCP handles persistence automatically.

**When it is useful:**
If you have an ongoing FIL project managed directly via Claude — not only for creating projects via FIL_BOOT.

**Setup:**
1. Create a Claude Project → name it "[PROJECT_NAME]"
2. Project Settings → Instructions → paste `FIL_Project_Instructions.md` content
3. Project Knowledge → upload these **2 files only**:
   `[PROJECT_NAME]_STABLE.md` · `[PROJECT_NAME]_SOP.md`
4. Settings → Integrations → Google Drive → Connect
5. Upload to Drive [PROJECT_NAME]/ folder:
   Domain files · `[PROJECT_NAME]_DYNAMIC_DataOnly.md`
6. First conversation:
   → Say "setup Drive" → LLM creates SESSION_INDEX + saves to Drive
   → All subsequent sessions: everything loads automatically from Drive

**New domain context (collection / destination / case...):**
→ Add new context files to Drive
→ Project Knowledge: never touched

PROMPTS + CALENDAR = reloaded every session
→ Token savings in Claude Project: ~70%
→ The value is real and immediate

---

## Recommendation

If you use FIL to manage **an ongoing project** → Claude Project adds value.
If you use FIL to **bootstrap projects** (FIL_BOOT) → standard mode is sufficient.
For **verticals** → Claude Project as priority · Gem as second choice.

---

*FIL Framework V3.4.1 · Platform Editions - AEL*
*FIL Framework V3.4.1 · Platform Editions - AEL*

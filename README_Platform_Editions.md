# FIL Framework — Platform Editions · V3.4.1
> Trois plateformes · un framework · architecture unifiée.

---

## Convention de versioning

```
FIL Framework V3.4.1         → version du framework sous-jacent
Claude Projects Edition V3.4.1 → version de cette édition spécifique

Les deux évoluent indépendamment.
FIL Framework monte à chaque amélioration du framework core.
L'édition monte à chaque changement du mode de déploiement lui-même.
```

---

## Analyse d'intérêt per platform

> **Note importante :** FIL est un méta-framework de templates.
> L'intérêt des éditions plateforme est plus élevé pour les **verticales**
> ([PROJECT_NAME], [PROJECT_NAME]) que pour FIL core lui-même.
> Pour FIL, la valeur principale est le **packaging et la cohérence**,
> pas les gains de tokens (no large domain files reloaded chaque session).

| | Claude Project | Gemini Gem | Custom GPT |
|---|---|---|---|
| **Intérêt pour FIL** | ⭐⭐ Modéré | ⭐⭐ Modéré | ⭐ Limité |
| **Intérêt pour verticales** | ⭐⭐⭐ Élevé | ⭐⭐ Modéré | ⭐ Limité |
| **STABLE permanent** | ✅ Knowledge | ✅ Knowledge | ✅ Knowledge |
| **SOP permanent** | ✅ Knowledge | ✅ Knowledge | ✅ Knowledge |
| **FIL_BOOT permanent** | ✅ Knowledge | ✅ Knowledge | ✅ Knowledge |
| **DYNAMIQUE auto** | ✅ Drive MCP | ✅ Drive root | ❌ upload manuel |
| **Drive créer dossiers** | ✅ | ❌ racine seule | ❌ |
| **Économie tokens** | ~40% | ~40% | ~20% |

---

## Ce que contient ce zip

```
README_Platform_Editions.md          ← ce fichier

── CLAUDE PROJECT ──────────────────────────────────────────────────
FIL_Project_Instructions.md          ← system prompt · Project Instructions
FIL_DYNAMIQUE_DataOnly_Template.md   ← DYNAMIQUE data only
FIL_STABLE_Template.md               ← Project Knowledge
FIL_SOP_Template.md                  ← Project Knowledge
CHANGELOG_Template.md                ← versioning projet

── GEMINI GEM ──────────────────────────────────────────────────────
FIL_Gem_Instructions.md              ← system prompt · Gem Instructions
(+ FIL_DYNAMIQUE_DataOnly_Template.md · FIL_STABLE_Template.md communs)

── CUSTOM GPT ──────────────────────────────────────────────────────
FIL_GPT_Instructions.md              ← system prompt · GPT Instructions
(+ FIL_DYNAMIQUE_DataOnly_Template.md · FIL_STABLE_Template.md communs)
```

---

## Claude Project (⭐⭐ Modéré pour FIL)

**Ce que ça apporte :**
FIL_BOOT.md, STABLE_TEMPLATE, SOP_TEMPLATE en Knowledge — toujours availables.
DYNAMIQUE = data only (~80 lines vs ~400 en mode standard).
Drive MCP gère la persistance automatically.

**Quand c'est utile :**
Si vous avez un projet FIL en cours géré directement via Claude — pas only pour créer des projets via FIL_BOOT.

**Setup:**
1. Create a Claude Project → name it "[PROJECT_NAME]"
2. Project Settings → Instructions → paste `FIL_Project_Instructions.md` content
3. Project Knowledge → upload these **2 files only**:
   `[PROJECT_NAME]_STABLE.md` · `[PROJECT_NAME]_SOP.md`
4. Settings → Integrations → Google Drive → Connect
5. Upload to Drive [PROJECT_NAME]/ folder:
   Domain files · `[PROJECT_NAME]_DYNAMIQUE_DataOnly.md`
6. First conversation:
   → Say "setup Drive" → LLM creates SESSION_INDEX + saves to Drive
   → All subsequent sessions: everything loads automatically from Drive

**New domain context (collection / destination / case...):**
→ Add new context files to Drive
→ Project Knowledge: never touched

LES + PROMPTS + CALENDRIER = reloaded every session
→ Gain tokens en Claude Project : ~70%
→ La valeur est réelle et immédiate
```

---

## Recommandation

Si vous utilisez FIL pour gérer **un projet en cours** → Claude Project apporte de la valeur.
Si vous utilisez FIL pour **bootstrapper des projets** (FIL_BOOT) → mode standard suffit.
Pour **les verticales** → Claude Project en priority · Gem en second.

---

*FIL Framework V3.4.1 · Platform Editions - AEL*

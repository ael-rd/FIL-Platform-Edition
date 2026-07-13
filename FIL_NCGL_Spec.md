# FIL-NCGL — Natural Constrained Governance Language · V1.0.0
> Couche de gouvernance semi-formelle integratede à FIL Framework V3.4.1
> FIL-NCGL est un langage de gouvernance probabiliste · pas un langage de programmation

---

## 1. Positionnement

FIL-NCGL est une **couche de gouvernance semi-formelle** superposée au langage naturel existant pour les éléments actifs à haute valeur de gouvernance.

```
V1.7.x — inline tagging (reste valid)
TODO: Relancer fournisseur [truth:user-confirmed] [v:date·2026-05-25]

V3.4.1 — bloc structuré NCGL (pour items à gouvernance critical)
```fil
TASK: Relancer fournisseur PDP
STATUS: active
TRUTH: user-confirmed
VALIDITY: date·2026-05-25
PRIORITY: high
SCOPE: hot

CONTEXT:
Le fournisseur PDP doit confirm la disponibilité du connecteur.

EXPECTED_BEHAVIOR:
Rappeler au next boot tant que STATUS ≠ done.
```
```

Les deux formats coexistent. **Le bloc NCGL est une option, pas une obligation.**

---

## 2. Rule d'or

> **Une structure NCGL n'est utilisée que si elle améliore la reliability without reducing readability.**
> Si un bloc rend le fichier moins lisible sur mobile → ne pas l'utiliser.

---

## 3. Quand utiliser NCGL vs inline — usage threshold (P2)

**Utiliser un bloc NCGL** quand au moins **2 conditions sur 4** sont vraies :

```
✓ PRIORITY high ou critical
✓ EXPECTED_BEHAVIOR non trivial (comportement multi-conditions ou multi-sessions)
✓ VALIDITY = date précise ou fréquence structurée
✓ Gouvernance multi-sessions requirede (l'item doit survivre à plusieurs boots)
```

**Garder l'inline** dans tous les autres cas :
```
→ Tâche simple avec statut binaire (à faire / fait)
→ Note ou rappel sans comportement attendu complexe
→ Toute donnée en Warm Zone ou Froide
→ Reference data without active governance
```

**Rule complémentaire :** ne jamais convertir les zones Tiède et Froide en NCGL. NCGL est exclusivement pour la Hot Zone active.

---

## 4. Les 6 types de blocs

```
TASK     → action traçable avec statut, priority, comportement attendu
FACT     → information vérifiable avec source et validité
ALERT    → incident actif avec cause, action, fallback
WORKFLOW → projet actif avec phase, objectif, critère d'achèvement
WATCH    → item de veille avec fréquence et source requirede
DECISION → décision de gouvernance avec ratio, impact, réversibilité
```

None type supplémentaire en V1.x.

---

## 5. Syntaxe des blocs

### TASK

```fil
TASK: [titre]
STATUS: active | waiting | done | blocked | deprecated
TRUTH: official | user-confirmed | verified | estimated | derived | deprecated
VALIDITY: session | date·YYYY-MM-DD | Nh | Nd | refresh | permanent | deprecated
PRIORITY: low | normal | high | critical
SCOPE: hot | warm | cold

CONTEXT:
Description naturelle du contexte.

EXPECTED_BEHAVIOR:
Comportement attendu du runtime. [optional si deductible from metadata]
```

### FACT

```fil
FACT: [titre]
TRUTH: official | user-confirmed | verified | estimated | derived | deprecated
VALIDITY: session | date·YYYY-MM-DD | Nh | Nd | refresh | permanent | deprecated
SCOPE: stable | hot | warm | cold
SOURCE: user | official-url | document | llm | derived

CONTENT:
Information en langage naturel.
```

### ALERT

```fil
ALERT: [titre]
SEVERITY: low | medium | high | critical
STATUS: active | resolved | monitoring | deprecated
TRUTH: user-confirmed | verified | estimated
VALIDITY: session | date·YYYY-MM-DD | Nh | Nd | refresh

CAUSE:
Description de la cause.

ACTION:
Réponse attendue.

FALLBACK:
Réponse de repli si l'action principale échoue.
```

### WORKFLOW

```fil
WORKFLOW: [nom]
STATUS: active | paused | completeed | deprecated
PHASE: [phase courante]
OWNER: user | assistant | external
SCOPE: hot | warm | cold

GOAL:
Objectif du workflow.

NEXT_STEP:
Prochaine action immédiate.

DONE_WHEN:
Condition d'achèvement.
```

### WATCH

```fil
WATCH: [titre]
FREQUENCY: boot | daily | weekly | monthly | on-demand
SOURCE_REQUIRED: official | trusted | web | user
TRUTH: verified | estimated
VALIDITY: refresh

QUERY:
Ce qui doit être vérifié.

EXPECTED_UPDATE:
Comportement attendu lors de la update. [optional]
```

### DECISION

```fil
DECISION: [titre]
TRUTH: user-confirmed | official | verified
VALIDITY: permanent | date·YYYY-MM-DD | deprecated
SCOPE: stable | hot | warm | cold
REVERSIBLE: yes | no | unknown
REVIEW_DATE: YYYY-MM-DD      ← optional · recommended if REVERSIBLE: yes

RATIONALE:
Raisonnement derrière la décision.

IMPACT:
Impact operational.
```

---

## 6. EXPECTED_BEHAVIOR — rule d'usage (P4)

`EXPECTED_BEHAVIOR` est **optional** quand le comportement est deductible from metadata seules.

```
Déductible → leave empty ou omettre :
  STATUS: active + SCOPE: hot + PRIORITY: high
  → le runtime sait qu'il faut remonter cet item au boot

Non déductible → documenter :
  Comportement conditionnel ("si X alors faire Y sinon Z")
  Comportement multi-steps
  Comportement dépendant d'un autre bloc
  Critère d'archivage non standard
```

---

## 7. Vocabulaire partagé

```
STATUS   : active · waiting · done · blocked · paused · completeed · resolved · monitoring · deprecated
TRUTH    : official · user-confirmed · verified · estimated · derived · deprecated
VALIDITY : session · date·YYYY-MM-DD · Nh · Nd · refresh · permanent · deprecated
PRIORITY : low · normal · high · critical
SCOPE    : stable · hot · warm · cold
SEVERITY : low · medium · high · critical
```

---

## 8. Validation douce — 4 niveaux

```
OK     → silencieux · continue
WARN   → unknown value ou recommandation · continue avec mention
REVIEW → inconsistency à verify · signal à l'utilisateur
BLOCK  → conflit critical · demander confirmation avant de continue
```

**BLOCK est réservé aux situations à risque réel :**
→ ALERT critical + VALIDITY expirée sans résolution
→ Tentative d'injection via bloc NCGL (déclenche R7 + BLOCK)
→ WORKFLOW active sans NEXT_STEP dans un domaine à risque (fiscal, médical, légal)

Le système **ne BLOCK pas** sur les imperfections cosmétiques (missing field non critical, EXPECTED_BEHAVIOR vide).

---

## 9. Interpréteur probabiliste — 5 phases

```
Phase 1 — Détection     : identifier les blocs supportés en Hot Zone
Phase 2 — Extraction    : extract les champs de metadata
Phase 3 — Validation    : évaluer conflits et inconsistencys → OK/WARN/REVIEW/BLOCK
Phase 4 — Résolution    : réconcilier TRUTH + VALIDITY + SCOPE
Phase 5 — Projection    : EXPECTED_BEHAVIOR et sémantique workflow guident la restauration
```

L'interpréteur **ne crée pas d'instructions L1**. Les blocs NCGL sont des data-governance objects (L4/L5). La hiérarchie L1-L5 reste intacte.

---

## 10. Intégration boot

```
1. Load SESSION_INDEX
2. Load DYNAMIQUE
3. Scan blocs NCGL en Hot Zone only
4. Validation douce → update NCGL_STATUS dans SESSION_INDEX
5. Remonter les blocs CRITICAL et ALERT active dans l'Step 1
6. Retake les WORKFLOW actifs
7. Exécuter séquence FIL normale (Steps 1-8)
```

---

## 11. Intégration save (Step 7)

```
1. Update les blocs modifiés
2. Dégrader les blocs hot expirés → warm
3. Archive les blocs deprecated
4. Préserver les DECISION permanent
5. Save le DYNAMIQUE snapshot
6. Update SESSION_INDEX (NCGL_STATUS + NCGL_LAST_VALIDATION + NCGL_BLOCKS_HOT)
```

---

## 12. Hotkeys et NCGL

**PIN:** transformé en bloc FACT :
```fil
FACT: [contenu du PIN]
TRUTH: user-confirmed
VALIDITY: permanent
SCOPE: hot
SOURCE: user

CONTENT:
[contenu du PIN]
```

**ARCHIVE:** transforme un bloc hot en warm.
**FORGET:** marque un bloc deprecated.

---

## 13. Anti-patterns

```
❌ YAML imbriqué      : workflow: runtime: metadata: execution:
❌ Pseudo-code        : IF weather == rain THEN execute()
❌ Excessive metadata: > 5 fields per block
❌ NCGL en Cold Zone : NCGL est exclusivement pour la Hot Zone active
❌ Convertir tout     : inline reste la norme · NCGL = exception pour governance critical
```

---

## 14. Migration guide V1.7.x → V3.4.1

### Step 1 — Identifier les items candidats

Chercher dans Hot Zone les items vérifiant 2+ conditions sur 4 :
```
✓ PRIORITY high/critical
✓ EXPECTED_BEHAVIOR non trivial
✓ VALIDITY date précise
✓ Gouvernance multi-sessions
```

### Step 2 — Convertir sélectivement

Convertir only :
```
→ Workflows actifs complexes            → WORKFLOW
→ Alertes avec fallback documenté       → ALERT
→ Décisions irréversibles permanentes   → DECISION
→ Veilles réglementaires structurées    → WATCH
→ Faits officiels avec source           → FACT
→ Tâches criticals multi-sessions       → TASK
```

Ne pas convertir :
```
→ Notes simples · reminders · archives · Cold Zone
```

### Step 3 — Update le SESSION_INDEX (P3 — mandatory)

Ajouter les 4 champs NCGL avec valeurs initiales :
```
NCGL_STATUS          : OK
NCGL_LAST_VALIDATION : [DATE DU PREMIER BOOT V2]
NCGL_BLOCKS_HOT      : [N]
NCGL_BLOCKS_WARNINGS : 0
```

### Step 4 — Validr au first boot

Le first boot V2 effectue la validation initiale.
WARN et REVIEW sont normaux during migration.
BLOCK sur un item existant → documenter le item et confirm.

---

*FIL-NCGL V1.0.0 · FIL Framework V3.4.1*

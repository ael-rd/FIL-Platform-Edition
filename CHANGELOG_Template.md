# [PROJECT_NAME] — Changelog
> Version courante : **1.5.3**
> Convention de versioning :
> → Major (X.0.0) : changement du fonctionnement du système
> → Minor (X.Y.0) : new [domain/collection/destination/sprint]
> → Patch (X.Y.Z) : corrections, mises à jour, ajustements

---

## V1.3.1 — [DATE] · Création initiale

### Création du système
→ Fichiers créés : STABLE · DYNAMIQUE · SOP · CHANGELOG
→ Basé sur le Framework FIL V1.5.3
→ Domaine : [DOMAINE]

---

## PROTOCOLE DE MISE À JOUR

```
At session end, on request "generate zip" :

1. IDENTIFIER le type de changement de la session :
   → Changement de fonctionnement      → MAJOR (X+1.0.0)
   → New [domain/collection/etc.] → MINOR (X.Y+1.0)
   → Correction / update          → PATCH (X.Y.Z+1)

2. METTRE À JOUR ce fichier :
   → Incrémenter la version courante en tête du fichier
   → Ajouter une entrée datée avec les changements de session

3. RÉGÉNÉRER [PROJECT_NAME]_VX.Y.Z.zip :
   → STABLE + DYNAMIQUE + SOP + CHANGELOG
   → Nom : [PROJECT_NAME]_VX.Y.Z.zip

4. ANNONCER :
   "📦 [PROJECT_NAME]_VX.Y.Z.zip généré — 4 fichiers — V[X.Y.Z]"
```

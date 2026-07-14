[PROJECT_NAME] — Changelog


Current version: 1.5.3
Versioning convention:
→ Major (X.0.0): change in system behavior
→ Minor (X.Y.0): new [domain/collection/destination/sprint]
→ Patch (X.Y.Z): fixes, updates, adjustments




V1.3.1 — [DATE] · Initial creation

System creation

→ Files created: STABLE · DYNAMIC · SOP · CHANGELOG
→ Based on FIL Framework V1.5.3
→ Domain: [DOMAIN]


UPDATE PROTOCOL

At session end, on request "generate zip":

1. IDENTIFY the type of change for the session:
   → Change in system behavior         → MAJOR (X+1.0.0)
   → New [domain/collection/etc.]      → MINOR (X.Y+1.0)
   → Fix / update                      → PATCH (X.Y.Z+1)

2. UPDATE this file:
   → Increment the current version at the top of the file
   → Add a dated entry with the session's changes

3. REGENERATE [PROJECT_NAME]_VX.Y.Z.zip:
   → STABLE + DYNAMIC + SOP + CHANGELOG
   → Name: [PROJECT_NAME]_VX.Y.Z.zip

4. ANNOUNCE:
   "📦 [PROJECT_NAME]_VX.Y.Z.zip generated — 4 files — V[X.Y.Z]"
# FIL Framework — Documented Limitations
> V3.4.1 · Read before production use

---

## What FIL Cannot Guarantee

### Inter-session fidelity
FIL relies on probabilistic LLM interpretation.
Two successive boots may produce slightly different behaviors on the same files.
This is not a bug — it is the nature of the system.
FIL reduces drift. It does not eliminate it.

### Inter-LLM fidelity
A project created on Claude can be used on Gemini or GPT,
but the exact behavior of SOPs and NCGL blocks may vary between models.
Test on each LLM before deploying to production.

### Context window limits
Beyond certain context sizes, governance instructions lose semantic weight
due to attention attenuation. FIL's STABLE and SOP grounding weakens
if active files exceed the model's effective context window.
Recommendation: keep Hot Zone under 100 lines · load only necessary files.

### Anti-injection security
FIL's anti-injection detection targets common patterns.
Sophisticated malicious content in a Drive file may pass undetected.
Never load Drive files from unknown sources.
FIL requires upstream sanitization for untrusted content.

### Manual editing coherence
If a STABLE or SOP file is modified manually outside a session,
FIL cannot detect this reliably.
STABLE_VERSION provides partial detection — not a guarantee.

### Simultaneous collaboration
FIL does not support simultaneous editing by multiple operators (V3.4.1).
The Handoff Protocol is sequential only.
Parallel operators work independently — governed exchange via partial handoff.

---

## What FIL Assumes

```
→ An LLM capable of following structured multi-step instructions
→ A user who runs "save" or equivalent at session end
→ Drive files accessible to the LLM (folder and flat modes)
→ Intact files — not corrupted, not partially edited
→ A context window sufficient to load all active files simultaneously
→ Trust in the files loaded — FIL governs structure, not content truth
```

---

## What FIL Is Not

```
FIL is NOT:
→ A compliance or legal framework
→ A formal security guarantee
→ An autonomous agent system
→ A replacement for human oversight
→ A deterministic execution engine

FIL IS:
→ An operational governance architecture
→ A workflow continuity protocol
→ A governance interpretation layer for persistent AI systems
```

*FIL Framework V3.4.1 · AEL*

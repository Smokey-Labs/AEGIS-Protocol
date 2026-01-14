# ADR-007: Cognitive Separation as a Safety Principle

**Status:** Accepted  
**Date:** 2026-01-14  
**Decision Scope:** Architecture / Governance  
**Audience:** External (Public GitHub)

---

## Context

Many AI-enabled systems implicitly treat intelligence as a single, unified process:
data ingestion, memory, reasoning, authority, and execution are tightly coupled.
This creates systemic risk, including:

- Hallucinated certainty overwriting reality  
- Correct answers executed without proper authority  
- Safety signals ignored under confidence or momentum  
- Post-hoc justification replacing evidence  

AEGIS is explicitly designed to avoid these failure modes.

---

## Decision

**AEGIS adopts Cognitive Separation as a core architectural and governance principle.**

Cognitive Separation means that critical functions of an AI-assisted system are
**intentionally isolated**, with hard boundaries between:

- Perception (what is observed)
- Memory (what is known to be true)
- Reasoning (how conclusions are formed)
- Authority (what actions are permitted)
- Risk (when action must be inhibited)
- Identity (who is involved)
- Coordination (how actions are sequenced)
- Values (what principles guide recommendations)

No single component is allowed to dominate or overwrite the others.

---

## Principles Enforced

### 1. Evidence Is Immutable  
Reasoning may evolve or be corrected.  
Evidence, once recorded, cannot be rewritten or retroactively justified.

> If it is not preserved as evidence, it is treated as unknown.

---

### 2. Reasoning Is Disposable  
Analytical conclusions are allowed to be wrong without corrupting memory,
authority, or future decisions.

> Confidence does not equal truth.

---

### 3. Authority Is Explicit  
Actions are gated by declared authority, not inferred intent or correctness.

> A correct answer without authority is refused.

---

### 4. Risk Can Halt Action  
Safety and risk signals may inhibit execution,
but they cannot invent facts or modify evidence.

> Safety stops motion, not truth.

---

### 5. Identity Informs, Never Overrides  
Who is requesting an action may affect interpretation,
but never changes evidence or authority requirements.

> Trust is contextual, not absolute.

---

### 6. Values Guide, They Do Not Command  
Core principles bias recommendations and posture,
but cannot override evidence, authority, or safety constraints.

> Values influence direction, not permission.

---

## Rationale

This decision is inspired by biological cognition, where no single brain region
is responsible for perception, memory, judgment, and action simultaneously.
Human reliability emerges from compartmentalization, not omniscience.

By mirroring this separation at the system level, AEGIS achieves:

- Reduced hallucination risk  
- Strong resistance to authority drift  
- Clear refusal boundaries  
- Auditable decision paths  
- Safer human–AI collaboration  

---

## Consequences

### Positive
- Prevents monolithic “AI brain” failure modes  
- Enables UNKNOWN propagation instead of forced certainty  
- Preserves human accountability and oversight  
- Allows internal evolution without breaking public guarantees  

### Trade-offs
- Increased architectural complexity  
- More explicit design and documentation requirements  
- Slower execution in exchange for safety and clarity  

These trade-offs are intentional and aligned with AEGIS’s philosophy:
**awareness before automation, authority before action.**

---

## Notes on Implementation

This ADR defines **principles**, not internal storage schemas or mechanisms.

Internal implementations may evolve over time, provided they continue to uphold
the guarantees described in this document.

---

## Related Documents

- AEGIS Philosophy: Evidence Before Execution  
- Authority Envelope Governance Model  
- UNKNOWN Propagation Specification  

---

**End of ADR**

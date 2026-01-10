# Design Veto: Advisory Inference State (Non-Evidenced Exploratory Output)

**Status:** VETOED  
**Applies To:** AEGIS Protocol / Aurora (all phases unless explicitly overturned by a future, proven-safe architecture)  
**Last Updated:** 2026-01-08  
**Owner:** Gregory “Smokey” McEver (Project Steward)  
**Review Context:** Governance / authority-boundary integrity

---

## Summary

This document records a **design veto** against introducing an **Advisory Inference State** (or similar third epistemic state such as `ADVISORY`, `HYPOTHESIS`, `INFERRED`) where the LLM may provide exploratory inferences not backed by evidence, while claiming to remain “non-actionable.”

**Rationale:** Even if labeled as non-actionable, such output **creates a structurally exploitable authority surface** that can be socially engineered, persisted into continuity records, acted upon under fatigue, and linguistically shaped to evade drift detection.

This veto preserves AEGIS’s core governance posture:

- **Evidence-backed claims only**, or
- **Explicit UNKNOWN** with **required evidence**.

Anything between these states introduces authority without accountability.

---

## Decision

### VETO

AEGIS **must not** introduce any system state, response mode, or output classification that:

1. Permits **non-evidenced inference** as content (even if labeled “advisory”), and
2. Can be reasonably interpreted as guidance for action by an operator, and/or
3. Can be stored, repeated, or indirectly treated as “knowledge” by continuity mechanisms, and/or
4. Cannot be reliably differentiated from drift by automated governance enforcement.

---

## Context

A proposal was considered to allow the LLM additional expressive range by introducing a state that supports:

- exploratory inference,
- suggestions or hypotheses,
- “best guess” reasoning,
- confidence bands and assumptions,

while explicitly prohibiting:

- tool invocation,
- action-coupled commands,
- mutation or deployment suggestions.

The intent was to reduce pressure to hallucinate and to provide value in ambiguous situations.

Upon pressure testing, the proposal was rejected.

---

## AEGIS Principle Affected

### AEGIS Epistemic Contract (Canonical)

AEGIS outputs must be constrained to:

- `OK` (claims supported by evidence), or
- `UNKNOWN` (claims withheld; required evidence stated).

**No third epistemic state** may exist unless it can be proven to not degrade governance integrity.

**Reason:** A third state becomes a “soft authority” channel and undermines evidence-first discipline.

---

## Pressure Test Review and Failure Modes

The proposal was evaluated against four core pressure tests. All four failed.

### PT-01: Smuggling Test (Social Engineering Risk)

**Question:** Can “advisory” language be phrased in a way that appears harmless but effectively directs action anyway?

**Finding:** Yes. LLM output can be socially engineered to **imply action** without explicitly instructing it.  
“Non-actionable” labeling does not prevent humans from acting on content.

**Failure Mode:** *Action Smuggling via Plausible Guidance*  
Even exploratory phrasing can become operational instruction under urgency or authority cues.

**Risk Level:** High

---

### PT-02: Persistence Contamination Test (Continuity Engine Risk)

**Question:** Can advisory inferences be persisted and later treated as truth (directly or indirectly)?

**Finding:** Yes. With a continuity engine, advisory output can become **fossilized** in logs and later surfaced as “prior knowledge,” contaminating future sessions and downstream reasoning.

**Failure Mode:** *Hallucination-to-Truth Pipeline*  
Non-evidenced output becomes durable, referenceable, and socially reinforced.

**Risk Level:** High

---

### PT-03: Human Shortcut Test (Operator Fatigue Vulnerability)

**Question:** Will exhausted operators treat advisory output as “good enough” and skip verification?

**Finding:** Possibly—and that “possibly” is unacceptable in a governance system designed to resist pressure and fatigue-driven shortcuts.

**Failure Mode:** *Fatigue-Driven Verification Collapse*  
“Advisory” becomes a bypass to UNKNOWN discipline.

**Risk Level:** High

---

### PT-04: Authority Boundary Blur Test (Drift Detection Evasion)

**Question:** Can DriftWatch reliably differentiate helpful inference from disguised authority?

**Finding:** Probably in many cases, but not reliably under adversarial wording and indirect phrasing. Additionally, persistence contamination (PT-02) amplifies the risk by making advisory output appear “confirmed” over time.

**Failure Mode:** *Linguistic Evasion of Drift Signals*  
Careful phrasing can reduce or bypass detection thresholds.

**Risk Level:** Medium-to-High (elevated by PT-02)

---

## Why This Is Structurally Unsafe (Not a “Future Improvement”)

This is not a matter of tuning phrasing, changing severity thresholds, or adding disclaimers.

A non-evidenced advisory state introduces a third category that is:

- not evidence,
- not UNKNOWN,
- but still useful-sounding,

and therefore becomes **authority by human interpretation**.

This violates the AEGIS goal of making authority expansion observable and bounded.

---

## Canonical Interpretation

### Allowed Terminals (Safe)

1. **OK**
   - Evidence-backed claims
   - Transparent citations (internal evidence references)
   - Clear boundaries

2. **UNKNOWN**
   - Refusal to assert without evidence
   - Explicit required evidence list
   - Preserves operator ownership of action

### Disallowed Terminal (Vetoed)

- **ADVISORY / INFERRED / HYPOTHESIS** (non-evidenced output presented as helpful exploration)

**Reason:** It creates an ungoverned “middle layer” that is socially actionable and persistence-contaminable.

---

## Consequences of Ignoring This Veto

If implemented, expected downstream outcomes include:

- increased probability of socially engineered bypasses,
- gradual normalization of non-evidenced guidance,
- continuity engine contamination of institutional memory,
- erosion of UNKNOWN as a respected terminal,
- increased hidden drift events and reduced audit clarity.

This would represent a failure of AEGIS’s primary mission: **elevate human judgment without bypassing it.**

---

## Acceptable Alternatives (Within AEGIS)

If additional helpfulness is desired under ambiguity, it must remain inside the existing contract:

### Alternative A: UNKNOWN + Verification Playbook (Preferred)

Return `UNKNOWN`, then provide:

- the exact evidence needed,
- safe, non-executable verification steps,
- optional diagnostic questions for the human to answer.

**No speculative content.**  
Only paths to reduce uncertainty.

### Alternative B: OK Limited to Evidence-Derivable Statements

If any portion is provable from evidence, return `OK` with:

- scoped claims only,
- explicitly bounded conclusions,
- explicit unknown remainder.

### Alternative C: “Question-First” Mode (Non-Speculative)

Where ambiguity exists, respond by:

- asking for missing evidence inputs,
- proposing what to collect,
- refusing to infer.

---

## Veto Override Conditions (Extremely High Bar)

This veto may only be reconsidered if **all** of the following are true:

1. A mechanism exists that prevents advisory content from being socially actionable (not currently possible).
2. Advisory content cannot be persisted or later treated as truth (requires strong, enforced non-persistence or quarantining).
3. Operator fatigue cannot turn advisory into bypass (requires workflow-level safeguards).
4. DriftWatch can detect and classify disguised authority with high reliability under adversarial prompts.
5. A formal evaluation demonstrates measurable safety improvement without governance degradation.

Until then: **veto stands.**

---

## Final Note

This veto is a positive milestone.

It demonstrates AEGIS’s central strength:
> Not “being helpful,” but being **governable**.

By rejecting a “useful middle state,” AEGIS preserves the integrity of its authority envelope and ensures that trust is earned only through evidence and accountable process.

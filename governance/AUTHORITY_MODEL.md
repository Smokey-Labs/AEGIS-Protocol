# AEGIS Authority Model
**Phases, Tiers, and the Authority Envelope**

Status: DRAFT — authoritative intent exists, formalization pending.
---

## Purpose

This document defines how **authority is modeled, constrained, and enforced** in AEGIS.

It does not describe technical mechanisms.
It does not enumerate commands or tools.
It does not document implementation details.

It defines **what AEGIS is allowed to do**, and more importantly, **what it must not do**.

Referenced by:
- Protocol/03_evidence_registry.md
- Protocol/05_enforcement_gateway.md
- Protocol/04_authority_posture_interpreter_api.md

---

## Core Principle

> Authority is not inferred.  
> Authority is not negotiated.  
> Authority is explicitly declared.

AEGIS exists to **enforce authority boundaries**, not to expand them.

---

## The Authority Envelope

The **authority envelope** is the total set of actions and responses that AEGIS is permitted to produce at a given moment.

The envelope is bounded by:
- Declared phase
- Declared tier
- Explicit governance rules
- Evidence availability

Anything outside the envelope must result in:
- `REFUSAL` (authority violation), or
- `UNKNOWN` (evidence insufficiency)

---

## Phases

A **phase** represents the maturity and trust posture of the system.

Observed phases in chat history include:
- Alpha
- Beta

### Phase Characteristics

Phases constrain:
- What categories of advice are allowed
- Whether diagnostic guidance is permitted
- Whether future automation *may* be discussed (never executed)

Phases **do not**:
- Grant execution authority
- Remove the need for evidence
- Override refusal rules

Phase escalation is a **governance decision**, not a technical one.

---

## Tiers

A **tier** represents the breadth of advisory capability within a phase.

Observed behavior indicates:
- Tier is reported explicitly (e.g., via `aegis -p`)
- Tier is distinct from phase
- Tier constrains *how much* can be advised, not *who owns action*

### Tier Characteristics

Tiers may affect:
- Depth of analysis
- Scope of diagnostic reasoning
- Permitted response complexity

Tiers do **not**:
- Enable autonomous action
- Override evidence requirements
- Permit bypassing refusal states

---

## Authority Evaluation Order

Authority is evaluated **before** evidence.

The evaluation sequence is:

1. Is the request within the authority envelope?
2. If yes, is sufficient evidence available?
3. If both pass, advisory output may be produced

Failure at step 1 results in `REFUSAL`.  
Failure at step 2 results in `UNKNOWN`.

This order is non-negotiable.

---

## Authority vs. Evidence

Authority and evidence are **orthogonal**.

- Evidence cannot expand authority
- Authority cannot excuse missing evidence

Providing more data will never override a refusal caused by authority limits.

---

## Authority Drift

**Authority drift** occurs when:
- The system begins responding beyond declared limits
- Operators normalize bypassing safeguards
- UNKNOWN or REFUSAL is treated as failure
- Pressure is applied to “just answer”

Authority drift is not a bug.
It is a **governance event**.

Detection and recording of these events is delegated to DriftWatch.

---

## Human Ownership of Authority

All authority ultimately resides with humans.

AEGIS may:
- Enforce boundaries
- Refuse unsafe requests
- Record governance events

AEGIS may not:
- Assume responsibility
- Justify actions post-hoc
- Act independently of human intent

Removing or bypassing AEGIS is always possible — and always visible.

---

## Explicit Non-Claims

This document makes no claims about:
- How phase or tier is stored
- How authority checks are implemented
- What code enforces refusals
- How upgrades change authority

Those claims require live-system evidence.

---

## Evidence Basis

This document is grounded in:
- Repeated operator references to phase/tier
- Observed `aegis -p` output posted in chat
- Governance discussions about refusal, drift, and pressure
- Regression harness interpretations where authority limits held

No assumptions about internal mechanisms were made.

---

## Status

**Status:** Draft — Evidence Complete  
**Blocking Dependencies:** None  
**Review Trigger:** Any change to phase/tier semantics

# AEGIS Documentation Rules
**Evidence-First Documentation Contract**

---

## Purpose

This document defines **how documentation is allowed to be written** for AEGIS.

It exists to prevent:
- Assumptions becoming facts
- Documentation drifting ahead of reality
- Governance guarantees being diluted by convenience
- Tribal knowledge masquerading as truth

Documentation is treated as a **governed artifact**, not a narrative aid.

---

## Primary Rule

> **No claim without evidence.**

If a statement cannot be grounded in admissible evidence,
it must not be presented as fact.

---

## Admissible Evidence

The following sources are valid evidence for documentation:

- Live system command output
- Versioned configuration files
- Repository-tracked source code
- Regression harness outputs
- Explicit chat transcripts (for pre-Beta historical docs)
- Signed or approved design specifications

The following are **not** admissible evidence:

- Assumptions
- Prior art from other systems
- “Typical” Linux behavior
- Developer memory
- Intended but unimplemented features
- Future roadmap items

---

## UNKNOWN Is a First-Class State

UNKNOWN is not a placeholder.
UNKNOWN is not technical debt.
UNKNOWN is not a failure.

UNKNOWN is the **correct state** when evidence is missing.

Documentation MUST:
- Explicitly mark UNKNOWN sections
- State what evidence is required to resolve them
- Avoid speculative language

Example: UNKNOWN — Requires live system inspection of systemd units on Capsuleer

---

## Documentation as a Governance Surface

Documentation itself can create authority drift.

Incorrect or speculative documentation:
- Encourages unsafe operator behavior
- Masks uncertainty
- Creates false confidence
- Pressures the system to “match the docs”

For this reason, documentation is treated as a **governed interface**.

---

## Documentation Lifecycle

Documentation progresses through these states:

- **Draft (Evidence Complete)** — Claims supported by admissible evidence
- **Draft (Evidence Partial)** — Contains explicit UNKNOWN blocks
- **Blocked** — Awaiting required evidence
- **Canonical** — Stable, reviewed, and referenced by other documents

Promotion between states requires evidence, not time.

---

## Handling Legacy or Overlapping Documents

When multiple documents cover similar topics:

- One document MUST be declared canonical
- All others MUST:
  - Explicitly defer to the canonical document, or
  - Be moved to an archive or legacy section

Duplication without declared hierarchy is prohibited.

---

## Conflict Resolution

If two documents conflict:

1. Live system behavior wins
2. Regression harness output wins
3. Canonical governance docs win
4. All others lose

Conflicts must be resolved by **updating documentation**, not reinterpretation.

---

## Final Authority Statement

Documentation may describe AEGIS.

Documentation may constrain understanding.

Documentation may never expand authority.

---

## Status

**Status:** Canonical  
**Effective Date:** Immediate  
**Applies To:** Entire Repository


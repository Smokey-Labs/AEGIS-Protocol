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

Example:

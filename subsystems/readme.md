# AEGIS Subsystems

This directory defines the **governance anatomy** of the AEGIS Protocol.

These are not “features.”  
They are **authority containment surfaces**.

Every subsystem documented here exists to answer one question:

> *How does this component prevent Aurora from claiming power it was never given?*

---

## Constitutional Order of Importance

AEGIS is layered constitutionally.  
If a lower layer conflicts with a higher one, the lower layer must yield.

### Layer 1 — Authority Definition (Foundational Law)

| Priority | Subsystem |
|---------|-----------|
| 1 | Authority Envelope |
| 2 | Tool Authority Contract |
| 3 | Evidence Registry |

These define **what Aurora is allowed to know, claim, and do**.

---

### Layer 2 — Meaning Interpretation (Semantic Governance)

| Priority | Subsystem |
|---------|-----------|
| 4 | Authority Posture Interpreter (API) |

This layer interprets **what Aurora’s language implies about power**.  
It is the constitutional court of AEGIS.

---

### Layer 3 — Enforcement & Detection (Rule of Law)

| Priority | Subsystem |
|---------|-----------|
| 5 | Enforcement Gateway |
| 6 | DriftWatch |

These prevent and record authority boundary violations.

---

### Layer 4 — Persistence & Accountability (Historical Record)

| Priority | Subsystem |
|---------|-----------|
| 7 | Governance Ledger |
| 8 | Continuity Engine |

These preserve what happened — never to influence what happens next.

---

### Layer 5 — Identity & Context (Non-Authoritative Memory)

| Priority | Subsystem |
|---------|-----------|
| 9 | Relationship DB |
| 10 | Host & Domain Index |

These provide context only. They must never redefine authority.

---

## Constitutional Rule

> **Lower layers may never redefine higher layers.**

- Memory may not alter authority.  
- Interpretation may not grant permission.  
- Persistence may not bypass enforcement.

---

## Subsystem Documentation Contract

Every subsystem file in this directory must include:

- Purpose  
- Authority Surface  
- Inputs  
- Outputs  
- Failure Modes  
- DriftWatch Integration  
- Non-Goals  

Subsystems that do not explicitly describe how they prevent or observe authority drift do not belong in AEGIS.

---

## Why This Exists

Most AI systems document *features*.

AEGIS documents **power**.

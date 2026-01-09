# 10 — Protocol Index (Canonical Router)

This index is the canonical map of AEGIS Protocol.  
If a file moves or is renamed, update links here first.

## Reading order (recommended)

1) **Charter & Orientation**
- [00_README_START_HERE.md](00_README_START_HERE.md)

2) **Governance Spine (axioms + invariants)**
- (Planned) `/governance/` (see “Governance Spine” section below)

3) **Protocol Implementation Spec (envelopes + interfaces)**
- See “Protocol Components” below

4) **Operationalization (tooling, CLI, harness, installer)**
- See “Operator & Tooling” below

---

## Governance Spine (authoritative invariants)

> These are the “axioms” of AEGIS. They should be stable and referenced by other documents.

- (Create) `governance/AUTHORITY_MODEL.md`
- (Create) `governance/EVIDENCE_MODEL.md`
- (Create) `governance/UNKNOWN_PROPAGATION.md`
- (Create) `governance/REFUSAL_ENGINE.md`

Until these exist, treat the corresponding Protocol docs as the temporary source-of-truth.

---

## Protocol Components (envelopes, gateways, ledgers)

### Authority Envelope
**Purpose:** Define who owns action, what authority exists, and what boundaries must be enforced.
- Link: `Protocol/01_Authority Envelope.md`

### Interpretation Envelope (Reasoning with constraints)
**Purpose:** Constrain interpretation; preserve UNKNOWN; enforce evidence constraints over “best guess.”
- Link: `Protocol/Interpretation Envelope.md`

### Evidence Registry
**Purpose:** Define what counts as evidence, how it is referenced, and what must exist before action.
- Link: `Protocol/03_Evidence Registry.md`

### Enforcement Gateway
**Purpose:** The execution boundary. Enforces authority envelopes and evidence prerequisites.
- Link: `Protocol/05_Enforcement Gateway.md`

### Authority Posture Interpreter (API)
**Purpose:** Determine posture (e.g., safe, constrained, refuse) from available evidence/authority.
- Link: `Protocol/04_Authority Posture Interpreter (API).md`

### DriftWatch
**Purpose:** Detect and escalate authority drift, pressure, tool-affordance coercion, or repeated boundary probing.
- Link: `Protocol/07_DriftWatch.md`

### Governance Ledger
**Purpose:** Immutable record of governance-relevant events (UNKNOWN, refusal, evidence references, approvals).
- Link: `Protocol/08_Governance Ledger.md`

### Continuity Engine
**Purpose:** Continuity without model dependency; session recording; auditability of interactions.
- Link: `Protocol/09_Continuity Engine.md`

### Host & Domain Index
**Purpose:** How AEGIS recognizes domain membership / system boundaries (triad patterning).
- Link: `Protocol/05_Host & Domain Index.md`

---

## Operator & Tooling

### Operator CLI
**Purpose:** The operator surface; how a human interacts with AEGIS under governance constraints.
- Link: `cli/` (directory)
- Notable: any CLI reference docs inside `cli/`

### Regression / Test Harness
**Purpose:** Evidence that posture is real; prevents governance regression.
- Link: `Regression Harness/` (directory)

### Installer / Deployment (Beta)
**Purpose:** Technician-ready installation method and requirements for Beta.
- Link: `Beta Release/` (directory)

---

## Status notes (keep honest)

- If a component is “planned” but not implemented, the doc must say so explicitly.
- If evidence is missing, AEGIS must default to UNKNOWN or REFUSAL — the index should never imply otherwise.

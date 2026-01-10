# Design Veto — Windows Platform Support

**Design Veto ID:** DV-001  
**Status:** Vetoed  
**Date:** 2026-01-10  
**Category:** Platform Architecture  
**Related Issue:** #38  

---

## Design Veto Statement

Support for Microsoft Windows as a **host platform** for AEGIS is **explicitly vetoed by design**.

AEGIS SHALL NOT run on Windows.

AEGIS is architected to operate as an independent Linux-based system within a controlled **shadow-zone**, with filesystem ingestion, evidence capture, and governance logic executed exclusively under Linux.

This veto is architectural, not provisional.

---

## Design Context

AEGIS is not a general-purpose application.  
It is a **governance and evidence system** whose correctness depends on:

- deterministic filesystem semantics  
- stable permission and ownership models  
- predictable process and service behavior  
- auditable system state transitions  

These requirements are foundational to Signal Forge, evidence preservation, and Sentinel operations.

Windows as a host platform introduces abstraction layers and behavioral variability that conflict with these design constraints.

---

## Vetoed Design

The following designs are vetoed:

- Running AEGIS directly on Windows
- Performing primary filesystem ingestion from Windows
- Treating Windows as a first-class runtime environment for AEGIS
- Designing AEGIS features that assume Windows-native execution

This includes both interactive and non-interactive AEGIS components.

---

## Approved Design

AEGIS SHALL:

- run on a dedicated Linux system (Ubuntu)
- operate within an isolated shadow-zone
- ingest filesystem and system evidence via Linux tooling
- preserve evidence using Linux-native mechanisms

Windows systems MAY be **observed indirectly** through exported artifacts, logs, or controlled interfaces, but SHALL NOT host AEGIS itself.

---

## Rationale

This design veto is based on the following architectural principles:

1. **Evidence Integrity**  
   Governance systems must observe from a position of determinism. Linux provides stronger guarantees for low-level inspection and auditability.

2. **Isolation by Design**  
   AEGIS must remain operationally independent from the systems it governs or observes. Hosting on Windows undermines shadow-zone isolation.

3. **Scope Discipline**  
   Supporting Windows materially expands surface area and complexity without providing proportional governance value.

4. **Authority Boundaries**  
   Platform ambiguity weakens the authority envelope of AEGIS. Linux-only execution strengthens enforceable guarantees.

---

## Explicit Non-Claims

This veto does **not** assert that:

- Windows is insecure or invalid
- Windows-based tooling lacks operational value
- AEGIS will never analyze Windows-derived data

This veto applies **only** to Windows as a host execution environment for AEGIS.

---

## Conditions for Reconsideration

This design veto SHALL remain in effect unless **new, concrete evidence** demonstrates that:

- Windows hosting can meet or exceed Linux determinism guarantees  
- shadow-zone isolation is preserved  
- evidence integrity is not degraded  
- overall system complexity is reduced, not increased  

Absent such evidence, this veto stands indefinitely.

---

## Closing Declaration

AEGIS design decisions are governed by **evidence and invariants**, not convenience.

Platform neutrality is explicitly rejected where it compromises system integrity.

This veto preserves architectural coherence and long-term trustworthiness.

---

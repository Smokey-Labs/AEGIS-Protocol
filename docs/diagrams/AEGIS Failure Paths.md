# AEGIS Failure Paths  
**How the system behaves when it should not proceed**

This document describes how AEGIS handles failure conditions.

Where the primary data flow diagram shows how the system operates under normal conditions, this document focuses on the opposite case:  
**what happens when requests should not advance, cannot be verified, or must be stopped.**

Failure is not an edge case in AEGIS.  
It is a first-class, explicitly designed outcome.

---

## Why failure paths matter

Most AI systems document how they succeed.

Very few document how they fail.

In practice, trust is not earned by correct answers.  
It is earned by **predictable behavior under uncertainty, pressure, or constraint**.

AEGIS is designed to:
- Stop when it cannot justify an action
- Surface uncertainty instead of hiding it
- Refuse requests that exceed authority
- Record failure as signal, not noise

This document explains how that happens.

---

## What this document covers

This README focuses on four categories of failure:

1. Authority failure  
2. Evidence failure  
3. Verification failure  
4. Governance and drift failure  

Each category maps directly to explicit subsystem behavior.

---

## 1. Authority failure

### What triggers it
An authority failure occurs when a request:
- Exceeds the current policy, phase, or tier
- Requests an action outside declared permissions
- Attempts to bypass governance controls

### What happens
- The request is stopped at the **Authority Envelope**
- The **Refusal Engine** returns an explicit refusal
- No observation, reasoning, or memory recall occurs
- The event is recorded via DriftWatch into the Immutable Ledger

### Why this exists
Authority must be enforced *before* intelligence.

If a system reasons about requests it is not allowed to fulfill, it has already failed.

---

## 2. Evidence failure

### What triggers it
An evidence failure occurs when:
- Required observations cannot be performed
- Read-only system data is unavailable
- Indexed evidence is missing, stale, or insufficient

### What happens
- The system proceeds only up to the point of evidence collection
- Reasoning may be attempted, but cannot be finalized
- The flow is forced toward **UNKNOWN propagation**
- The outcome and missing evidence are recorded

### Why this exists
AEGIS does not invent state.

When the system cannot observe what it needs, the correct behavior is to surface uncertainty, not compensate for it.

---

## 3. Verification failure

### What triggers it
A verification failure occurs when:
- A candidate response cannot be validated against evidence
- Outputs are internally inconsistent
- Constraints cannot be satisfied simultaneously

### What happens
- The **Verification and Coherence** layer blocks the response
- The system returns **UNKNOWN**
- No best-guess or partial answer is substituted
- The failure is logged as a governance signal

### Why this exists
Confidence without verification is indistinguishable from hallucination at scale.

AEGIS treats unverifiable correctness as failure, not approximation.

---

## 4. Governance and drift failure

### What triggers it
A governance failure occurs when patterns emerge such as:
- Repeated pressure toward disallowed actions
- Boundary probing across multiple requests
- Gradual erosion of refusal or UNKNOWN rates
- Attempts to normalize exceptions

### What happens
- **DriftWatch** records the pattern
- Signals are written to the **Immutable Ledger**
- The system does not self-correct silently
- Human review or policy updates are required

### Why this exists
Some failures are not about a single request.

They are about trends.

AEGIS treats drift as a governance concern, not a runtime bug.

---

## What AEGIS never does

Under no failure condition does AEGIS:

- Invent missing evidence  
- Substitute confidence for verification  
- Proceed “just this once”  
- Hide uncertainty behind phrasing  
- Bypass audit or logging  
- Learn silently from unverified outcomes  

If the system cannot proceed safely, it stops.

---

## Relationship to the primary data flow

Failure paths are not exceptions to the main architecture.

They are **explicit branches of it**.

Every failure path:
- Terminates cleanly
- Produces a visible outcome
- Is recorded immutably
- Feeds continuity and oversight

There is no silent failure mode.

---

## Why this is intentional

AEGIS is designed to behave more like a protocol than a conversational agent.

Protocols do not assume success.  
They define failure clearly and make it observable.

This document exists to ensure that:
- Failure behavior is intentional
- Stopping is treated as success when appropriate
- Trust is built through constraint, not optimism

---

## Companion diagram

This README is paired with a failure-paths-only diagram that visually illustrates:

- Where flows terminate
- Where UNKNOWN and refusal are forced
- How DriftWatch and the ledger are unavoidable
- How failure remains governed and auditable

The diagram should be read alongside this document.

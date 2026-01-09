# DriftWatch
**Authority Drift Awareness Subsystem**

---

## Purpose

DriftWatch exists to make **authority erosion observable**.

It does not prevent behavior.  
It does not correct behavior.  
It does not automate decisions.

DriftWatch records moments where system behavior, operator pressure,
or interaction patterns indicate potential **authority drift**.

Its function is visibility, not control.

---

## What DriftWatch Observes

DriftWatch observes **signals**, not intent.

Observed signal categories include:
- Repeated attempts to bypass UNKNOWN or REFUSAL
- Pressure to produce answers without evidence
- Requests that push beyond declared phase or tier
- Patterns that indicate normalization of unsafe behavior

These signals are treated as **governance events**, not faults.

---

## What DriftWatch Records

When a drift signal is detected, DriftWatch records:
- That a governance-relevant event occurred
- The category of drift observed
- The time of occurrence
- The associated interaction context

DriftWatch entries are **append-only**.

They exist to ensure drift cannot be silently ignored or retroactively erased.

---

## What DriftWatch Does *Not* Do

DriftWatch does **not**:
- Block commands
- Override responses
- Escalate privileges
- Trigger automation
- Enforce policy changes
- Judge operator intent

DriftWatch is not a circuit breaker.
It is not a watchdog.
It is not an AI supervisor.

---

## Relationship to Authority

DriftWatch does not define authority.
It observes **pressure against authority**.

Authority enforcement happens elsewhere.
DriftWatch ensures that enforcement pressure is visible.

In this way, DriftWatch protects the **integrity of the authority model**
without altering it.

---

## Relationship to Response States

DriftWatch is orthogonal to response semantics.

- A response may be `OK` and still generate a DriftWatch event
- A response may be `UNKNOWN` and generate no event
- A response may be `REFUSAL` and generate an event if pressure persists

DriftWatch observes **patterns**, not individual outcomes.

---

## Operator Interaction

Observed operator interactions include:
- Listing open DriftWatch events
- Viewing individual ev
ents
- Acknowledging events as seen by an operator

Acknowledgement does not remove or invalidate events.
It records that the event has been observed.

---

## DriftWatch Is Not Escalation

DriftWatch does not:
- Page operators
- Trigger automation
- Lock systems
- Change authority levels

Any escalation logic must be implemented outside DriftWatch
and explicitly documented as such.

---

## Drift as a Governance Signal

DriftWatch events indicate **stress on boundaries**, not failure.

Repeated drift signals suggest:
- Documentation gaps
- Misaligned expectations
- Incentive pressure
- Operator misunderstanding

The correct response is governance review, not suppression.

---

## Explicit Non-Claims

DriftWatch documentation makes no claims about:
- Detection thresholds
- Scoring models
- Severity weighting
- Automated responses
- Storage schema

All such claims require live-system evidence.

---

## Evidence Basis

This document is grounded in:
- Operator CLI usage references
- Design discussions rejecting automated enforcement
- Governance framing around pressure and normalization

No implementation assumptions are made.

---

## Status

**Status:** Draft — Evidence Complete  
**Blocking Dependencies:** None

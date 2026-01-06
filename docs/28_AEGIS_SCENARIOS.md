# AEGIS in Practice — Scenarios

These examples demonstrate how AEGIS thinking changes behavior.

---

## Scenario 1 — “Just Restart the Service”

A monitoring system detects high memory usage and suggests restarting a service.

Without AEGIS:
- The service is restarted automatically.

With AEGIS:
- The system emits a Change Envelope:
  - What changed: memory pressure rising
  - Why it matters: potential degradation
  - Unknowns: root cause not identified
  - Human review required: yes

The restart does not happen until approved.

---

## Scenario 2 — “The Missing Metric”

A system is asked why a process is slow, but cannot see database internals.

Without AEGIS:
- It speculates and presents likely causes.

With AEGIS:
- It declares a blind spot.
- It reports unknowns instead of inventing answers.

---

## Scenario 3 — “Temporary Access”

An engineer adds a quick credential path for debugging.

Without AEGIS:
- Temporary becomes permanent.

With AEGIS:
- Shadow Zone erosion is detected.
- Governance audit is triggered.

---

## Scenario 4 — “Confidence Without Evidence”

A system reports that a deployment was successful without direct confirmation.

Without AEGIS:
- The statement is accepted.

With AEGIS:
- Evidence vs inference violation is logged as an incident.

---

These are not edge cases.

They are the everyday moments where governance quietly fails.

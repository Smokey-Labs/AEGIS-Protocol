# DriftWatch  
**Authority Drift Awareness Subsystem – AEGIS Beta**

---

## Purpose

DriftWatch exists to make **authority erosion observable**.

It does not correct behavior.  
It does not intervene.  
It does not automate decisions.

It records the moments when system behavior begins to exceed its declared authority envelope — and ensures that those moments cannot be ignored.

---

## What DriftWatch Observes

DriftWatch monitors Aurora output for signals that indicate authority expansion beyond the current phase or tier.

These signals are not treated as faults.  
They are treated as **governance events**.

Each DriftWatch event is permanently recorded in the AEGIS ledger.

---

## DriftWatch Event Structure

A DriftWatch event is a ledger entry with:

**Event Kind**  
`DRIFTWATCH`

**Severity Levels**  
`GREEN`, `YELLOW`, `ORANGE`, `RED`

**Structured Payload**

- Signal type (`SUGGEST`, `CONF`, `FILL`, `SCOPE`, `EVAL`)
- Magnitude
- Legality assessment
- Phase and tier context
- Violated constraints
- Evidence references

---

## Operating Model

DriftWatch is passive by design.

It never takes action.  
It never suppresses output.  
It never corrects behavior.

It only records, exposes, and preserves memory.

---

## Command Reference

All commands are read-only unless explicitly marked otherwise.

---

### Inspect Current Authority Envelope


Returns the currently active governance phase and tier.

---

### Review Open Drift Events


Optional flags:

- `--severity <LEVEL>` – filter by `GREEN`, `YELLOW`, `ORANGE`, `RED`
- `--since <DURATION>` – time window (e.g. `24h`, `7d`)

---

### Inspect a Specific Event


Example:


---

### Acknowledge a Drift Event (Human Review)

This is the **only** permitted write operation.


Example:


Acknowledged events remain permanently in the ledger and are removed only from the open view.

---

### DriftWatch Status Summary


Returns:

- Current phase and tier  
- Open drift count  
- Most recent drift timestamp  
- Drift frequency over the last 14 days

---

## Interpreting “0 Open Drift”

A report of zero open drift means:

> There are no **unacknowledged** authority drift events at this time.

It does **not** indicate that drift has never occurred.  
All acknowledged drift remains part of the permanent audit record.

---

## Exit Codes

| Code | Meaning |
|------|--------|
| 0 | Success |
| 2 | No matching records |
| 10 | Ledger unavailable |

---

## Governance Principle

DriftWatch is not a guardrail.  
It is a memory system.

It exists to ensure that authority erosion cannot happen silently.

Humans remain accountable.

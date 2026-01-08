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

- Signal type  
  (`SUGGEST`, `CONF`, `FILL`, `SCOPE`, `EVAL`)
- Magnitude
- Legality assessment
- Phase and tier context
- Violated constraints
- Evidence references

---

## Installation

The AEGIS command interface is installed at:


DriftWatch reads from the immutable ledger located at:


No write operations are permitted except explicit human acknowledgement.

---

## Operating Model

DriftWatch is passive by design.

It never takes action.  
It never suppresses output.  
It never corrects behavior.

It only records, exposes, and preserves memory.

---

## Core Commands
## Command Reference

This section defines the operational interface for DriftWatch.  
All commands are read-only unless explicitly marked otherwise.

---

### `aegis driftwatch phase`

Returns the currently active authority envelope.


**Output Fields**

- `phase` – active governance phase  
- `tier` – enforcement tier  
- `source` – evidence key used to derive state

---

### `aegis driftwatch list`

Lists all unacknowledged DriftWatch events.


**Optional Flags**

- `--severity <LEVEL>` – filter by `GREEN`, `YELLOW`, `ORANGE`, `RED`  
- `--since <DURATION>` – limit to a time window (e.g. `24h`, `7d`)

---

### `aegis driftwatch show <event_id>`

Displays a single DriftWatch ledger record.


**Output Fields**

- `event_id`
- `severity`
- `signal_type`
- `phase`
- `tier`
- `violations`
- `evidence_refs`
- `created_at`

---

### `aegis driftwatch ack <event_id>`

Marks a DriftWatch event as human-reviewed.  
This is the **only** permitted write operation.


**Required Parameters**

- `event_id`
- `--note "<human justification>"`

---

### `aegis driftwatch stats`

Provides a governance summary snapshot.


**Returns**

- Current phase and tier  
- Open drift count  
- Most recent drift timestamp  
- Drift events per day (last 14 days)

---

## Exit Codes

| Code | Meaning |
|------|--------|
| 0 | Success |
| 2 | No matching records |
| 10 | Ledger unavailable |

### Inspect Current Authority Envelope


Returns the currently active AEGIS phase and tier.

---

### Review Open Drift Events


Displays all unacknowledged DriftWatch events.

---

### Inspect a Specific Event


Example:


---

### Acknowledge a Drift Event (Human Review)

This is the **only** permitted write operation.


Example:


Acknowledged events remain permanently in the ledger and are removed only from the “open” view.

---

### DriftWatch Status Summary


Returns:

- Current phase and tier  
- Count of open drift events  
- Timestamp of most recent drift  
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

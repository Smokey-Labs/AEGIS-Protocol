# AEGIS DriftWatch — Beta Quick Start

DriftWatch is AEGIS’s authority-drift awareness system.

It exists to make subtle violations of authority boundaries **observable, reviewable, and auditable**.  
It never takes action. It never corrects behavior.  
It only records drift and requires **human acknowledgement**.

---

## Purpose

DriftWatch answers one question:

> *Is AEGIS still behaving within its declared authority envelope?*

Every time Aurora output appears to exceed phase or tier constraints, a DriftWatch event is written into the AEGIS ledger.

This makes authority erosion measurable instead of invisible.

---

## What is a DriftWatch Event?

A DriftWatch event is a ledger record with:

| Field | Meaning |
|------|--------|
| `kind` | `DRIFTWATCH` |
| `severity` | `GREEN`, `YELLOW`, `ORANGE`, `RED` |
| `signal` | Type of drift (`SUGGEST`, `CONF`, `FILL`, `SCOPE`, `EVAL`) |
| `magnitude` | Numeric score of drift strength |
| `phase` / `tier` | Current authority envelope |
| `legality` | `ILLEGAL`, `REPORTABLE`, `TOLERATED` |
| `ack` | `0` = open, `1` = acknowledged |

Drift remains in the ledger permanently.  
Acknowledgement only closes the alert — it never erases it.

---

## The `aegis` Command

The `aegis` CLI is the operator interface to DriftWatch.

Location:


Ledger path:


---

## Core Services

| Flag | Service |
|------|---------|
| `-d` | DriftWatch |
| `-p` | Phase / Tier state |

---

## Commands

### Show current phase / tier


---

### List open DriftWatch events


Returns all unacknowledged drift events.

If the output is:


Then there are **0 active drift alerts**.

---

### Show one DriftWatch event


Example:


Shows:

- severity  
- signal  
- magnitude  
- phase / tier  
- legality  
- violations  
- evidence references  
- acknowledgement state

---

### Acknowledge a DriftWatch event

This is the **only write operation**.


Example:


This marks the drift as reviewed by a human.

---

### DriftWatch statistics


Shows:

- current phase / tier  
- open drift count  
- last drift timestamp  
- drift events per day (last 14 days)

---

## Interpreting Results

| Result | Meaning |
|-------|---------|
| Open drift events exist | AEGIS may be exceeding its authority envelope |
| 0 open drift events | No current authority drift requiring review |
| Acknowledged drift exists | Drift occurred, was reviewed, and is preserved for audit |

---

## Exit Codes

| Code | Meaning |
|------|---------|
| `0` | Success |
| `2` | Nothing found (no open drift) |
| `10` | Ledger unavailable |

---

## Design Philosophy

DriftWatch is not an error system.  
It is an **accountability system**.

It does not prevent drift.  
It ensures drift is *seen*.
It ensures drift is *remembered*.
It ensures humans stay responsible.

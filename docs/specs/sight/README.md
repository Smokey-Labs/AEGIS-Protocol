# AEGIS Sight v1 — Quick Start & Orientation

AEGIS Sight defines **how the system observes without acting**.

This specification exists to ensure that:
- Awareness never silently becomes action
- Detail is never extracted without explicit human intent
- Authority cannot drift through convenience or ambiguity

This document is the **human entry point** to Sight v1.

---

## Core Principle

> **Knowledge is advisory only.**  
> **Detail must be explicit.**  
> **Action without observation can lead to catastrophe.**

AEGIS separates *seeing* from *doing*.

---

## Sight Levels

### Implied Sight (Default)

Implied sight provides **general awareness** without operator gating.

It allows AEGIS to:
- Observe existence (files, services, ports, domains)
- Enumerate names (directories, units, containers)
- Read metadata (size, owner, perms, timestamps)
- Compute hashes
- Detect changes over time
- Reason about system stability

It explicitly **does not allow**:
- Reading file contents
- Reading logs
- Inspecting environment variables
- Extracting config values
- Accessing secrets

If the operator did not explicitly request detail, AEGIS will not extract it.

---

### Explicit Sight (Operator-Gated)

Explicit sight allows **bounded detail extraction**.

It requires:
- A declared **action**
- A declared **target**
- Validation against allowlists, blacklists, and bounds
- Evidence recording and redaction

Explicit sight is always:
- Intentional
- Scoped
- Auditable
- Logged

---

## The Sight Contract (Schemas)

Sight behavior is enforced by **machine-readable contracts**.

### `sight_envelope_v1.json`
Defines:
- The request shape operators must use
- The response shape AEGIS must return
- Required fields for implied vs explicit sight
- How UNKNOWN and REFUSED are represented

This is the **primary protocol contract**.

---

### `actions_v1.json`
Defines:
- The **only actions** AEGIS is allowed to perform
- Required sight level for each action
- Supported target types
- Default bounds and hard limits
- Redaction expectations

If an action is not listed here, it does not exist.

---

### `action_blacklist_v1.json`
Defines:
- Actions that are **explicitly forbidden**
- Even if accidentally implemented in the future

This file exists to protect against:
- Authority drift
- Accidental expansion
- Prompt or tooling regression

---

## Targets

Target formats and prefixes are documented in:


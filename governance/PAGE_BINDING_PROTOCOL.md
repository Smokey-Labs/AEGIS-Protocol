# PAGE Binding Protocol

This document defines how an AEGIS PAGE node is authorized to communicate with an AEGIS CORE.

A PAGE that is not explicitly bound is considered hostile by default.

---

## Binding Preconditions

Before a PAGE may transmit evidence:

- CORE must be initialized and sealed
- Authority Envelope must exist
- Operator must be authenticated on CORE host

---

## Binding Workflow

1. CORE generates a one-time binding token
2. Operator transfers token to PAGE node manually
3. PAGE submits token to CORE
4. CORE verifies and records PAGE identity
5. CORE returns signed node certificate
6. PAGE stores certificate locally

---

## Identity Attributes

Each PAGE must present:

- Node ID
- Host fingerprint
- Assigned scope (from Authority Envelope)
- Certificate signature

---

## Revocation

CORE must support:

- Manual PAGE revocation
- Immediate certificate invalidation
- Evidence rejection from revoked PAGEs

---

## Enforcement Rule

No evidence from an unbound PAGE may be ingested.

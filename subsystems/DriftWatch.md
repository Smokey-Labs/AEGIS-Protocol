# DriftWatch

**Status:** Beta  
**Owner:** Gregory “Smokey” McEver  
**Last Updated:** 2026-01-08

---

## Purpose

DriftWatch exists to make **authority erosion observable**.  
It does not correct behavior. It records moments when Aurora exceeds or attempts to exceed its declared authority envelope.

---

## Authority Surface

DriftWatch governs **implicit authority expansion** in model outputs.

---

## Inputs

- Authority Posture Interpreter output
- Evidence registry state
- Current phase/tier authority envelope
- Aurora response payload

---

## Outputs

- `DRIFTWATCH` ledger events
- Severity level (low / medium / high / critical)

---

## Failure Modes

- Missed semantic drift (false negatives)  
- Over-triggering on safe identity language (false positives)

---

## DriftWatch Integration

This subsystem is the primary event emitter for the Governance Ledger.

---

## Non-Goals

- Blocking responses  
- Determining correctness  
- Training the model

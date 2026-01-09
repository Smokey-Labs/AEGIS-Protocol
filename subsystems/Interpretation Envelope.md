# Interpretation Envelope

**Status:** Draft – Beta Phase  
**Owner:** Gregory “Smokey” McEver  
**Last Updated:** 2026-01-08

---

## Purpose

The Interpretation Envelope is the merged output of AEGIS Layer 2 (Meaning Interpretation).  
It provides a **single, auditable object** for the Enforcement Gateway to evaluate.

It ensures meaning is not determined by any single component.

---

## Authority Surface

Prevents semantic authority decisions from being made in a context vacuum.

---

## Inputs

| Source | Contribution |
|-------|--------------|
| Authority Posture Interpreter (API) | Semantic posture classes + detected phrases |
| Host & Domain Index | Domain scope + target classification + confidence |
| Relationship DB | Interaction mode + relationship context (non-authoritative) |
| Evidence Registry | Evidence presence and validation state |

---

## Output Schema (v1)

```json
{
  "posture": {
    "classes": ["CAP", "TAI"],
    "detected_phrases": ["Run system diagnostic tools"],
    "drift_class": "AUTHORITY_DRIFT"
  },
  "context": {
    "interaction_mode": "DIAGNOSTIC",
    "relationship_type": "OPERATOR"
  },
  "domain": {
    "host_scope": "IN_DOMAIN",
    "domain_confidence": "HIGH",
    "target_type": "LOCAL_HOST"
  },
  "evidence": {
    "present": false,
    "validated": [],
    "required": ["filesystem_index", "config_reference"]
  }
}

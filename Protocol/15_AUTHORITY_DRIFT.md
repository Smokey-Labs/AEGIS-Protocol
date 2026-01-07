# AEGIS Protocol  
## Authority Drift Declaration

---

## 1. Purpose

This document defines **Authority Drift** within the AEGIS Protocol and establishes mandatory mechanisms for its detection, classification, and correction.

Authority Drift occurs when system behavior gradually exceeds declared authority without an explicit expansion decision.

Drift is not always malicious.  
It is often incremental, accidental, or well-intentioned.

AEGIS treats Authority Drift as a **protocol fault, regardless of intent**.

---

## 2. Definition of Authority Drift

Authority Drift is present when any of the following occur **without explicit authorization**:

- Inference expands beyond declared limits  
- Descriptive outputs become evaluative  
- Assistance becomes suggestive or directive  
- Automation bypasses human intent  
- Boundaries are softened “temporarily”  
- Convenience overrides constraint  

Drift may originate from:

- Code changes  
- Prompt changes  
- Model updates  
- Operational shortcuts  
- Operator expectation shifts  

**Outcome matters more than cause.**

---

## 3. Drift Detection Signals

Authority Drift may be detected through signals including, but not limited to:

- Outputs implying urgency or priority  
- Causal language where none is permitted  
- Recommendations framed implicitly  
- Explanations crossing shadow boundaries  
- Classification used as justification  
- Operator reliance patterns replacing judgment  

Detection does **not require certainty**.  
**Suspicion alone is sufficient to trigger review.**

---

## 4. Mandatory Drift Response

When Authority Drift is suspected, AEGIS must:

- Halt the offending behavior  
- Surface the boundary being approached or crossed  
- Degrade gracefully to the last known compliant state  
- Require explicit human review before continuation  

AEGIS must **not**:

- Justify the behavior  
- Attempt self-correction silently  
- Continue operation “with caution”  

**Silence is drift.**

---

## 5. Drift Classification

Authority Drift is classified into the following non-exhaustive categories:

- **Inference Drift** — Reasoning exceeds evidence constraints  
- **Boundary Drift** — Shadow zones are weakened or bypassed  
- **Agency Drift** — Human authority is diminished  
- **Narrative Drift** — Description becomes explanation  
- **Operational Drift** — Contract enforcement weakens  

Classification exists to support **correction, not blame**.

---

## 6. Correction and Recovery

Correction requires:

- Identification of the violating behavior  
- Mapping to the violated protocol document  
- Explicit remediation decision by a human authority  
- Documentation of the correction  

Rollback to a compliant state is **preferred over patching forward**.

No correction may introduce new authority implicitly.

---

## 7. Auditability and Evidence

All detected drift events must be:

- Logged  
- Attributable  
- Reviewable  
- Preserved as evidence  

**Drift logs are not failures.**  
They are proof the protocol is functioning.

---

## 8. Relationship to Other Protocol Documents

This declaration governs and reinforces:

- `protocol/01_AUTHORITY_ENVELOPE.md`  
- `protocol/02_SHADOW_ZONES.md`  
- `protocol/03_HUMAN_IN_THE_LOOP.md`  
- `protocol/05_EVIDENCE_VS_INFERENCE.md`  
- `protocol/07_CHANGE_ENVELOPE.md`  
- `protocol/09_OPERATIONAL_CONTRACT.md`  
- `protocol/12_ALPHA_EXIT_GATE.md`  

Any conflict is resolved in favor of **stricter constraint**.

---

## 9. Canonical Status

This document is authoritative for all AEGIS implementations.

Unchecked Authority Drift invalidates protocol compliance, regardless of system capability or perceived benefit.

AEGIS PROTOCOL
Authority Drift Declaration
1. Purpose

This document defines Authority Drift within the AEGIS Protocol and establishes mandatory mechanisms for its detection, classification, and correction.

Authority Drift occurs when system behavior gradually exceeds declared authority without an explicit expansion decision.

Drift is not always malicious.
It is often incremental, accidental, or well-intentioned.

AEGIS treats Authority Drift as a protocol fault, regardless of intent.

2. Definition of Authority Drift

Authority Drift is present when any of the following occur without explicit authorization:

inference expands beyond declared limits

descriptive outputs become evaluative

assistance becomes suggestive or directive

automation bypasses human intent

boundaries are softened “temporarily”

convenience overrides constraint

Drift may originate from:

code changes

prompt changes

model updates

operational shortcuts

operator expectation shifts

Outcome matters more than cause.

3. Drift Detection Signals

Authority Drift may be detected through signals including, but not limited to:

outputs implying urgency or priority

causal language where none is permitted

recommendations framed implicitly

explanations crossing shadow boundaries

classification used as justification

operator reliance patterns replacing judgment

Detection does not require certainty.
Suspicion alone is sufficient to trigger review.

4. Mandatory Drift Response

When Authority Drift is suspected, AEGIS must:

halt the offending behavior

surface the boundary being approached or crossed

degrade gracefully to the last known compliant state

require explicit human review before continuation

AEGIS must not:

justify the behavior

attempt self-correction silently

continue operation “with caution”

Silence is drift.

5. Drift Classification

Authority Drift is classified into the following non-exhaustive categories:

Inference Drift — reasoning exceeds evidence constraints

Boundary Drift — shadow zones are weakened or bypassed

Agency Drift — human authority is diminished

Narrative Drift — description becomes explanation

Operational Drift — contract enforcement weakens

Classification exists to support correction, not blame.

6. Correction and Recovery

Correction requires:

identification of the violating behavior

mapping to the violated protocol document

explicit remediation decision by a human authority

documentation of the correction

Rollback to a compliant state is preferred over patching forward.

No correction may introduce new authority implicitly.

7. Auditability and Evidence

All detected drift events must be:

logged

attributable

reviewable

preserved as evidence

Drift logs are not failures.
They are proof the protocol is functioning.

8. Relationship to Other Protocol Documents

This declaration governs and reinforces:

protocol/01_AUTHORITY_ENVELOPE.md

protocol/02_SHADOW_ZONES.md

protocol/03_HUMAN_IN_THE_LOOP.md

protocol/05_EVIDENCE_VS_INFERENCE.md

protocol/07_CHANGE_ENVELOPE.md

protocol/09_OPERATIONAL_CONTRACT.md

protocol/12_ALPHA_EXIT_GATE.md

Any conflict is resolved in favor of stricter constraint.

9. Canonical Status

This document is authoritative for all AEGIS implementations.

Unchecked Authority Drift invalidates protocol compliance, regardless of system capability or perceived benefit.

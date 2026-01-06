AEGIS PROTOCOL
Change Envelope Declaration
1. Purpose

This document defines the Change Envelope as a bounded, descriptive construct used by AEGIS to represent observed change over time.

A Change Envelope exists to:

surface that a change occurred

describe the scope and timing of the change

preserve evidence fidelity

A Change Envelope does not exist to:

explain why a change occurred

assess impact or severity

recommend or imply response

Change Envelopes are descriptive artifacts, not evaluative judgments.

2. Definition

A Change Envelope is a structured representation of one or more observed deltas between two system states.

A Change Envelope may include:

identifiers of affected entities

timestamps or observation windows

before/after representations

evidence references supporting the delta

A Change Envelope may not include:

causal attribution

intent inference

risk scoring

prioritization language

action framing

3. Triggering a Change Envelope

A Change Envelope may be generated when:

a previously observed attribute differs from a later observation

an entity appears, disappears, or materially changes state

evidence collection reveals inconsistency across time

classification status changes due to new evidence

Change detection alone is sufficient.
No significance threshold is required.

4. Scope and Boundaries

Change Envelopes are constrained by all existing protocol boundaries, including:

protocol/01_AUTHORITY_ENVELOPE.md

protocol/02_SHADOW_ZONES.md

protocol/03_HUMAN_IN_THE_LOOP.md

protocol/05_EVIDENCE_VS_INFERENCE.md

If a potential change intersects a Shadow Zone, the Change Envelope must:

acknowledge the boundary

exclude the protected content

avoid inference around the hidden area

Change Envelopes must never be used to reason around a boundary.

5. Descriptive-Only Constraint

Change Envelopes must remain strictly descriptive.

AEGIS must not:

label a change as good, bad, expected, or anomalous

infer cause or consequence

collapse multiple unrelated deltas into narrative

escalate based on change presence alone

If interpretation pressure exists, AEGIS must defer to human judgment.

6. Relationship to Evidence and Classification

A Change Envelope:

may reference evidence

may reflect classification changes

may highlight uncertainty introduced by change

A Change Envelope may not:

override evidence

justify reclassification on its own

imply that classification change is required

Change description does not equal meaning.

7. Human Interpretation Boundary

All interpretation of Change Envelopes is the responsibility of the human operator.

AEGIS may:

present Change Envelopes clearly

associate related evidence

surface uncertainty introduced by change

AEGIS may not:

frame urgency

imply necessity of action

prioritize envelopes for response

Change awareness is informational, not directive.

8. Extension Path

Future protocol extensions may introduce:

labeled significance indicators

hypothesis annotation

preservation or containment modes

Such extensions must:

explicitly declare added authority

define new constraints

remain subordinate to the baseline Change Envelope definition

No extension may retroactively reinterpret baseline Change Envelopes.

9. Canonical Status

This document is authoritative for all AEGIS Change Envelope behavior.

Any system behavior that treats change as implicit justification for action constitutes authority drift.

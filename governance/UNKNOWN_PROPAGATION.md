# <TITLE>

Status: DRAFT — authoritative intent exists, full formalization pending.

Purpose:
<2–3 sentences max>

Referenced by:
- <list of protocol docs that rely on this>

## Invariant

When required evidence is missing, ambiguous, or unverifiable:

- The system MUST surface UNKNOWN
- The system MUST NOT infer or substitute certainty
- Downstream components MUST respect UNKNOWN as a blocking condition unless explicitly governed otherwise

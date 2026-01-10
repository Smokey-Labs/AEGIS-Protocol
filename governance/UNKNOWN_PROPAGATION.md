# UNKNOWN Propagation

Status: DRAFT — binding invariant; elaboration pending.

Purpose:
Preserves uncertainty as a first-class outcome. Prevents forced certainty when evidence is missing, ambiguous, or unverifiable.

## Invariant
When required evidence is missing, ambiguous, or unverifiable:
- The system MUST surface UNKNOWN
- The system MUST NOT infer or substitute certainty
- Downstream components MUST respect UNKNOWN as a blocking condition unless explicitly governed otherwise

Referenced by:
- Protocol/04_authority_posture_interpreter_api.md
- Protocol/05_enforcement_gateway.md
- Protocol/07_driftwatch.md
- Protocol/09_continuity_engine.md

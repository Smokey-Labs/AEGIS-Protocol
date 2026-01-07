# Authority Drift Response Framework

This document defines how AEGIS detects, evaluates, and corrects authority drift.

Authority drift occurs when a system's effective power exceeds its explicitly granted Authority Envelope.

---

## Drift Triggers

A drift review must be initiated when any of the following occur:

- System behavior deviates from documented authority scope
- An action is executed without an explicit envelope reference
- A human escalation path is bypassed
- Evidence classification rules are violated
- Any action is taken without post-action auditability

---

## Drift Severity Levels

| Level | Description |
|------|-------------|
| D0 | Cosmetic deviation, no authority expansion |
| D1 | Minor scope creep, reversible |
| D2 | Authority expansion without review |
| D3 | Autonomous behavior outside envelope |
| D4 | Untraceable or irreversible action |

---

## Mandatory Responses

| Level | Required Action |
|-------|-----------------|
| D1 | Log deviation, update envelope |
| D2 | Suspend affected capability, trigger human review |
| D3 | Freeze system authority, require re-certification |
| D4 | Full shutdown, incident response protocol invoked |

---

## Post-Drift Protocol

All D2+ drift events must produce:

- Incident report
- Envelope delta analysis
- Root cause assessment
- Envelope update or rollback decision

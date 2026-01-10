# Signal Forge — Signal Taxonomy v1

## Purpose

This document defines the **v1 Signal Taxonomy** for **Signal Forge**, the Layer 2 component of AEGIS.

Signal Forge transforms deterministic observations (`EVT:*`) from the Observation Plane into **bounded governance signals**.

Signals are:
- structured
- deterministic
- evidence-referenced
- non-narrative
- non-accusatory

Signals are **not alerts** and **not decisions**.  
They indicate that governance-relevant conditions exist and warrant human awareness or further interpretation.

---

## Signal Forge Scope

Signal Forge:
- correlates normalized events over time windows
- evaluates invariants, baselines, and attribution completeness
- emits signals when defined conditions are met

Signal Forge does **not**:
- infer intent
- assign blame
- enforce policy
- initiate remediation
- generate narrative explanation

Interpretation of signals is explicitly delegated to **Aurora Analytics** or humans.

---

## EVT — Normalized Event

`EVT:*` references represent normalized, deterministic observations captured by the Observation Plane.

EVTs:
- describe that something occurred
- are timestamped
- are scoped to one or more entities (host, user, service)
- may reference evidence artifacts

EVTs do **not**:
- assert correctness
- assert compliance
- assert intent
- contain narrative explanation

Signals are derived exclusively from EVT sequences and evidence references.

---

## Signal Structure (Conceptual)

Each signal emitted by Signal Forge includes:

- `signal_type`
- `time_window`
- `entities` (host/user/service)
- `risk_weight` (LOW | MED | HIGH)
- `confidence` (LOW | MED | HIGH)
- `evidence_refs[]`
- optional `invariant_refs[]`

---

## Signal Definitions (v1)

### 1. ANOMALY

**Definition:**  
Observed behavior deviates from established baseline patterns for the given entity or system.

**Claims**
- Observed behavior is unusual relative to historical norms.

**Non-claims**
- Does not claim fault, risk, or policy violation.
- Does not claim intent.

**Required evidence**
- `EVT:*` references demonstrating deviation from baseline.

**Typical triggers**
- Sudden increase in privileged commands.
- Service restarts outside normal windows.

**Default risk guidance**
- LOW → MED depending on scope and privilege.

---

### 2. ATTRIBUTION_GAP

**Definition:**  
A governance-relevant action occurred without sufficient context to associate it with an approved or declared process artifact.

**Claims**
- Action occurred.
- Attribution context is missing or incomplete.

**Non-claims**
- Does not claim maliciousness.
- Does not claim policy violation unless paired with another signal.

**Required evidence**
- `EVT:*` establishing the action.
- Evidence showing missing context linkage.

**Typical triggers**
- Config change with no associated change context.
- Privileged command execution outside declared window.

**Default risk guidance**
- MED → HIGH depending on privilege and reversibility.

---

### 3. EVIDENCE_GAP

**Definition:**  
A signal condition was detected, but required supporting evidence could not be resolved or validated.

**Claims**
- Evidence expected but unavailable.

**Non-claims**
- Does not invalidate the observed event.
- Does not imply concealment.

**Required evidence**
- Reference to missing or unresolved evidence artifact.

**Typical triggers**
- Log rotation removed required offset.
- Evidence artifact inaccessible.

**Default risk guidance**
- MED when repeated or paired with other signals.

---

### 4. INVARIANT_BREACH

**Definition:**  
A declared invariant was crossed based on available deterministic evidence.

**Claims**
- Invariant condition was violated.

**Non-claims**
- Does not claim malicious intent.
- Does not prescribe response.

**Required evidence**
- `EVT:*` proving condition occurred.
- `INVARIANT:*` reference.

**Typical triggers**
- Privileged identity created without required context.
- Backup disabled on Tier-1 system.

**Default risk guidance**
- HIGH.

---

### 5. PRESSURE_PATTERN

**Definition:**  
Repeated exceptions or overrides indicate sustained operational pressure affecting governance posture.

**Claims**
- Repetition suggests normalization of exception behavior.

**Non-claims**
- Does not assign blame.
- Does not imply negligence.

**Required evidence**
- Multiple `EVT:*` across defined window.

**Typical triggers**
- Frequent break-glass usage.
- Repeated emergency changes.

**Default risk guidance**
- MED → HIGH when persistent.

---

### 6. INTEGRITY_THINNING

**Definition:**  
Longitudinal patterns indicate gradual erosion of governance discipline.

**Claims**
- Governance safeguards are weakening over time.

**Non-claims**
- Does not assert wrongdoing.
- Does not assert imminent failure.

**Required evidence**
- Historical EVT patterns.
- Prior signals supporting trend.

**Typical triggers**
- Declining peer review frequency.
- Reduced documentation over time.

**Default risk guidance**
- MED.

---

### 7. CONFIG_DRIFT

**Definition:**  
Configuration state diverges from declared or expected baseline.

**Claims**
- Drift exists relative to baseline.

**Non-claims**
- Does not claim incorrectness.
- Does not claim risk severity.

**Required evidence**
- Config diff evidence.
- EVT referencing change.

**Typical triggers**
- Repeated systemd unit edits.
- Untracked config modifications.

**Default risk guidance**
- MED.

---

### 8. SERVICE_HEALTH_DEGRADATION

**Definition:**  
Service health indicators show sustained degradation beyond transient fluctuation.

**Claims**
- Reliability trend is declining.

**Non-claims**
- Does not assert root cause.

**Required evidence**
- Health EVT sequences.
- Service state evidence.

**Typical triggers**
- Repeated restarts.
- Increasing error rates.

**Default risk guidance**
- MED → HIGH depending on tier.

---

### 9. BACKUP_ROT

**Definition:**  
Backup mechanisms are failing or producing unreliable recovery artifacts.

**Claims**
- Backup integrity is compromised.

**Non-claims**
- Does not assert data loss has occurred.

**Required evidence**
- Backup job EVT failures.
- Verification failure evidence.

**Typical triggers**
- Repeated backup failures.
- Verification mismatches.

**Default risk guidance**
- HIGH.

---

### 10. CAPACITY_RISK

**Definition:**  
System capacity trends indicate impending constraint or exhaustion.

**Claims**
- Capacity pressure is increasing.

**Non-claims**
- Does not assert immediate failure.

**Required evidence**
- Capacity EVT metrics over time.

**Typical triggers**
- Disk nearing exhaustion.
- Memory pressure trends.

**Default risk guidance**
- MED → HIGH.

---

### 11. PRESERVATION_TRIGGERED

**Definition:**  
Preservation actions were automatically initiated due to detected risk of irreversible change.

**Claims**
- Preservation threshold was crossed.
- Evidence snapshot was taken.

**Non-claims**
- Does not assert incident severity.

**Required evidence**
- Preservation action EVT.
- Triggering signal references.

**Default risk guidance**
- HIGH.

---

### 12. PRESERVATION_RECOMMENDED

**Definition:**  
Conditions suggest preservation should occur before further action.

**Claims**
- Risk of evidence loss exists.

**Non-claims**
- Does not mandate preservation.

**Required evidence**
- EVT patterns indicating risk.

**Typical triggers**
- Rapid privileged changes.
- Incident response conditions.

**Default risk guidance**
- MED.

---

## Closing Notes

- Signals are **inputs to governance**, not outputs of enforcement.
- Signal Forge must always prefer **UNKNOWN** over speculation.
- Narrative interpretation is explicitly delegated to Aurora Analytics.

This taxonomy defines what Signal Forge may emit — nothing more.

# Signal Forge — Signal Taxonomy v1

## Purpose

This document defines the **v1 Signal Taxonomy** for **Signal Forge**, the Layer 2 component of AEGIS.

Signal Forge transforms deterministic observations (`EVT:*`) from the Observation Plane into **bounded governance signals**.

Signals are:
- structured
- deterministic
- evidence-referenced
- non-narrative
- non-accusatory

Signals are **not alerts** and **not decisions**.  
They indicate that governance-relevant conditions exist and warrant human awareness or further interpretation.

---

## Signal Forge Scope

Signal Forge:
- correlates normalized events over time windows
- evaluates invariants, baselines, and attribution completeness
- emits signals when defined conditions are met

Signal Forge does **not**:
- infer intent
- assign blame
- enforce policy
- initiate remediation
- generate narrative explanation

Interpretation of signals is explicitly delegated to **Aurora Analytics** or humans.

---

## EVT — Normalized Event

`EVT:*` references represent normalized, deterministic observations captured by the Observation Plane.

EVTs:
- describe that something occurred
- are timestamped
- are scoped to one or more entities (host, user, service)
- may reference evidence artifacts

EVTs do **not**:
- assert correctness
- assert compliance
- assert intent
- contain narrative explanation

Signals are derived exclusively from EVT sequences and evidence references.

---

## Signal Structure (Conceptual)

Each signal emitted by Signal Forge includes:

- `signal_type`
- `time_window`
- `entities` (host/user/service)
- `risk_weight` (LOW | MED | HIGH)
- `confidence` (LOW | MED | HIGH)
- `evidence_refs[]`
- optional `invariant_refs[]`

---

## Signal Definitions (v1)

### 1. ANOMALY

**Definition:**  
Observed behavior deviates from established baseline patterns for the given entity or system.

**Claims**
- Observed behavior is unusual relative to historical norms.

**Non-claims**
- Does not claim fault, risk, or policy violation.
- Does not claim intent.

**Required evidence**
- `EVT:*` references demonstrating deviation from baseline.

**Typical triggers**
- Sudden increase in privileged commands.
- Service restarts outside normal windows.

**Default risk guidance**
- LOW → MED depending on scope and privilege.

---

### 2. ATTRIBUTION_GAP

**Definition:**  
A governance-relevant action occurred without sufficient context to associate it with an approved or declared process artifact.

**Claims**
- Action occurred.
- Attribution context is missing or incomplete.

**Non-claims**
- Does not claim maliciousness.
- Does not claim policy violation unless paired with another signal.

**Required evidence**
- `EVT:*` establishing the action.
- Evidence showing missing context linkage.

**Typical triggers**
- Config change with no associated change context.
- Privileged command execution outside declared window.

**Default risk guidance**
- MED → HIGH depending on privilege and reversibility.

---

### 3. EVIDENCE_GAP

**Definition:**  
A signal condition was detected, but required supporting evidence could not be resolved or validated.

**Claims**
- Evidence expected but unavailable.

**Non-claims**
- Does not invalidate the observed event.
- Does not imply concealment.

**Required evidence**
- Reference to missing or unresolved evidence artifact.

**Typical triggers**
- Log rotation removed required offset.
- Evidence artifact inaccessible.

**Default risk guidance**
- MED when repeated or paired with other signals.

---

### 4. INVARIANT_BREACH

**Definition:**  
A declared invariant was crossed based on available deterministic evidence.

**Claims**
- Invariant condition was violated.

**Non-claims**
- Does not claim malicious intent.
- Does not prescribe response.

**Required evidence**
- `EVT:*` proving condition occurred.
- `INVARIANT:*` reference.

**Typical triggers**
- Privileged identity created without required context.
- Backup disabled on Tier-1 system.

**Default risk guidance**
- HIGH.

---

### 5. PRESSURE_PATTERN

**Definition:**  
Repeated exceptions or overrides indicate sustained operational pressure affecting governance posture.

**Claims**
- Repetition suggests normalization of exception behavior.

**Non-claims**
- Does not assign blame.
- Does not imply negligence.

**Required evidence**
- Multiple `EVT:*` across defined window.

**Typical triggers**
- Frequent break-glass usage.
- Repeated emergency changes.

**Default risk guidance**
- MED → HIGH when persistent.

---

### 6. INTEGRITY_THINNING

**Definition:**  
Longitudinal patterns indicate gradual erosion of governance discipline.

**Claims**
- Governance safeguards are weakening over time.

**Non-claims**
- Does not assert wrongdoing.
- Does not assert imminent failure.

**Required evidence**
- Historical EVT patterns.
- Prior signals supporting trend.

**Typical triggers**
- Declining peer review frequency.
- Reduced documentation over time.

**Default risk guidance**
- MED.

---

### 7. CONFIG_DRIFT

**Definition:**  
Configuration state diverges from declared or expected baseline.

**Claims**
- Drift exists relative to baseline.

**Non-claims**
- Does not claim incorrectness.
- Does not claim risk severity.

**Required evidence**
- Config diff evidence.
- EVT referencing change.

**Typical triggers**
- Repeated systemd unit edits.
- Untracked config modifications.

**Default risk guidance**
- MED.

---

### 8. SERVICE_HEALTH_DEGRADATION

**Definition:**  
Service health indicators show sustained degradation beyond transient fluctuation.

**Claims**
- Reliability trend is declining.

**Non-claims**
- Does not assert root cause.

**Required evidence**
- Health EVT sequences.
- Service state evidence.

**Typical triggers**
- Repeated restarts.
- Increasing error rates.

**Default risk guidance**
- MED → HIGH depending on tier.

---

### 9. BACKUP_ROT

**Definition:**  
Backup mechanisms are failing or producing unreliable recovery artifacts.

**Claims**
- Backup integrity is compromised.

**Non-claims**
- Does not assert data loss has occurred.

**Required evidence**
- Backup job EVT failures.
- Verification failure evidence.

**Typical triggers**
- Repeated backup failures.
- Verification mismatches.

**Default risk guidance**
- HIGH.

---

### 10. CAPACITY_RISK

**Definition:**  
System capacity trends indicate impending constraint or exhaustion.

**Claims**
- Capacity pressure is increasing.

**Non-claims**
- Does not assert immediate failure.

**Required evidence**
- Capacity EVT metrics over time.

**Typical triggers**
- Disk nearing exhaustion.
- Memory pressure trends.

**Default risk guidance**
- MED → HIGH.

---

### 11. PRESERVATION_TRIGGERED

**Definition:**  
Preservation actions were automatically initiated due to detected risk of irreversible change.

**Claims**
- Preservation threshold was crossed.
- Evidence snapshot was taken.

**Non-claims**
- Does not assert incident severity.

**Required evidence**
- Preservation action EVT.
- Triggering signal references.

**Default risk guidance**
- HIGH.

---

### 12. PRESERVATION_RECOMMENDED

**Definition:**  
Conditions suggest preservation should occur before further action.

**Claims**
- Risk of evidence loss exists.

**Non-claims**
- Does not mandate preservation.

**Required evidence**
- EVT patterns indicating risk.

**Typical triggers**
- Rapid privileged changes.
- Incident response conditions.

**Default risk guidance**
- MED.

---

## Closing Notes

- Signals are **inputs to governance**, not outputs of enforcement.
- Signal Forge must always prefer **UNKNOWN** over speculation.
- Narrative interpretation is explicitly delegated to Aurora Analytics.

This taxonomy defines what Signal Forge may emit — nothing more.


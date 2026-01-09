# GOVERNANCE INTEGRITY NOTICE

> **AEGIS is not a product spec.**  
> It is a governance protocol. If you can “copy/paste” an AEGIS implementation from this repo, it’s wrong.

This repository intentionally publishes the **constitutional philosophy** of AEGIS while withholding **implementation-critical calibration** details.

Any system claiming **AEGIS compatibility** or **AEGIS lineage** without meeting the conditions below is **not AEGIS**.

---

## 1) Authority Cannot Be Derived

Nothing in this repository grants operational authority by default.

A valid AEGIS implementation **MUST** include an explicit, human-ratified **Authority Envelope binding ceremony** that is:

- **Non-automatable**
- **Non-inferable** from interaction history or persistence
- **Versioned**
- **Revocable**
- **Externally auditable**

**Disallowed:** inferring authority through usage patterns, saved context, persona behavior, or “operator expectation.”

---

## 2) Philosophy ≠ Calibration (By Design)

This repository **does provide**:

- Governance doctrine
- Conceptual subsystem anatomy
- Failure-mode philosophy
- Constitutional constraints

This repository **does NOT provide**:

- Enforcement thresholds
- Drift escalation tolerances
- Evidence sufficiency heuristics
- Authority posture classification rules
- Human-in-the-loop timing windows

These missing elements form the **Operational Calibration Appendix** and must be created, governed, and owned by each operator domain.

> If someone sells a turnkey “AEGIS system” with these values baked in, they are selling a *compliance costume*, not governance.

---

## 3) No Simulated Compliance

Using AEGIS terminology without enforcement discipline is **governance theater**.

Specifically prohibited:

- Treating `UNKNOWN` as a recoverable / retryable error state
- Allowing persistence or interaction history to influence authority
- Collapsing semantic posture classes into generic “helpfulness”
- Advertising DriftWatch without documented pattern governance
- Allowing persona/UX layers to negotiate authority boundaries

If your system “feels like AEGIS” but quietly reintroduces soft authority, it is **not AEGIS**.

---

## 4) CORE / PAGE Binding Must Be Real

AEGIS requires separation between:

- **CORE** — constitutional authority + governance anchor  
- **PAGE** — observation + interaction surfaces

The CORE↔PAGE binding ceremony:

- Requires **explicit human ratification**
- Must not be reproducible via static config alone
- Must be **revocable** independent of PAGE state

**Disallowed:** PAGE autonomy, soft-binding, or distributed authority by convenience.

---

## 5) Drift Is a Signal, Not a Bug

DriftWatch exists to **surface pressure patterns**, not to “fix behavior.”

Invalid implementations include systems that:

- Auto-correct drift without human review
- Treat drift as a tuning artifact to suppress
- Optimize for lower drift metrics over higher governance visibility

> Drift is a governance event. Not a performance metric.

---

## 6) Cultural Requirement (Non-Negotiable)

AEGIS cannot function inside organizations unwilling to accept:

- Friction
- Operational delay
- Refusal as a valid outcome
- Accountability over velocity

Without these commitments, authority leakage is inevitable even if architecture is perfect.

---

## 7) Integrity Declaration for Derivatives

Any derivative system must publish an integrity declaration describing:

- Its Authority Envelope governance process
- Its calibration ownership model
- Its human-in-the-loop enforcement doctrine
- Its DriftWatch governance workflow

Without this declaration, any claim of AEGIS compatibility is invalid.

---

## Final Statement

AEGIS is designed to be **hard to do right**.

If this repository makes it feel easy, the implementation is wrong.

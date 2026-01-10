# Decision Record — Aurora Dual-Mode Architecture

**Decision ID:** ADR-006  
**Status:** Accepted  
**Date:** 2026-01-10  
**Scope:** Aurora behavior, user interaction, and governance boundaries

---

## Context

As AEGIS has evolved, Aurora’s role has clarified into two distinct operational realities:

1. A **passive, ambient system presence** that surfaces governance-relevant conditions.
2. An **explicitly invoked, interactive intelligence** that assists humans in exploring unknowns.

Conflating these roles introduces significant risk:
- speculative reasoning may be mistaken for truth
- discovery may leak into governance
- trust boundaries may erode

This decision formalizes a dual-mode architecture for Aurora to preserve clarity, trust, and long-term system integrity.

---

## Decision

Aurora SHALL operate in two explicitly distinct modes:

### 1. **Aurora Analytics** (The Sun)

Aurora Analytics is the **ambient, non-interactive presence** of Aurora within AEGIS.

It:
- consumes outputs from Signal Forge
- presents structured, bounded interpretations
- produces Signal Cards, digests, and escalation packets
- operates continuously or on schedule
- does not speculate
- does not explore
- does not initiate dialogue

Aurora Analytics:
- reveals what is already known
- communicates temporal truth and uncertainty
- prefers UNKNOWN over assumption
- never acts without evidence

Aurora Analytics exists whether or not a human is present.

---

### 2. **Aurora Discovery** (The Flashlight)

Aurora Discovery is the **explicitly invoked, interactive intelligence** mode.

It:
- is initiated by a human
- operates within a bounded session
- explores ambiguity and unknowns
- asks clarifying questions
- reasons, hypothesizes, and explains
- may reference evidence *when provided*

Aurora Discovery:
- is exploratory, not authoritative
- produces provisional understanding
- does not emit signals
- does not alter governance state
- does not persist output unless explicitly instructed

Aurora Discovery exists only when summoned.

---

## Core Boundary Rule

> **Aurora does not explore unless asked, and does not govern while exploring.**

This rule is absolute.

---

## Explicit Non-Goals

Aurora Discovery SHALL NOT:
- autonomously influence Signal Forge
- emit or modify signals
- silently persist findings
- act as background monitoring
- be treated as an audit source

Aurora Analytics SHALL NOT:
- ask questions
- speculate about intent
- reconstruct missing evidence
- engage in open-ended dialogue
- replace human judgement

---

## Rationale

Separating these modes:

- preserves trust by preventing speculative reasoning from being mistaken as truth
- allows deep, meaningful discovery without governance risk
- keeps Signal Forge deterministic and auditable
- enables Aurora to assist humans without becoming an authority

This distinction mirrors real-world roles:
- governance systems illuminate conditions
- humans explore causes and meaning

---

## Consequences

### Positive
- Clear scope boundaries
- Reduced risk of AI overreach
- Stronger operator trust
- Easier regulatory and audit explanations
- Discovery can go deeper without widening scope

### Trade-offs
- Two interaction models must be maintained
- Some duplication of context loading may occur
- Discovery output requires explicit promotion to governance artifacts

These trade-offs are intentional and acceptable.

---

## Future Considerations (Deferred)

- Explicit UX affordances distinguishing Analytics vs Discovery
- Session isolation and audit controls for Discovery
- Optional “promote to evidence” workflows with human approval

No future work may violate the Core Boundary Rule.

---

## Summary

Aurora is both:
- **the sun**, illuminating the kingdom continuously
- **the flashlight**, pointed by a human into the unknown

These roles are complementary—but must never be merged.

This decision ensures Aurora remains trustworthy, useful, and governable as AEGIS grows.

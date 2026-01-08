# Design Decisions

This folder documents **explicit design decisions** made during the development of AEGIS.

These are not implementation details.
They are not requirements.
They are not guarantees.

They exist to answer a simple question:

> “Why was this built *this way*?”

---

## Purpose

Complex systems fail when their constraints are forgotten.

As AEGIS evolves, contributors, users, and reviewers will encounter boundaries that may feel conservative, restrictive, or incomplete.

This folder exists to preserve **decision intent** — especially in cases where:

- Faster progress was possible but declined
- Optimization was intentionally avoided
- Capability was constrained by governance
- Refusal was chosen over helpfulness

Each document captures:
- The decision
- The reasoning behind it
- The evidence that informed it
- What alternatives were considered and rejected

---

## What These Documents Are

These documents are:

- Narrative, not prescriptive
- Grounded in observation, not theory
- Written after decisions were made
- Stable across implementations

They are meant to be read slowly.

---

## What These Documents Are Not

These documents do **not**:

- Claim universal correctness
- Serve as justification for future changes
- Replace technical specifications
- Prevent reevaluation with new evidence

They explain *why something was chosen at the time*.

---

## Reading Guidance

If a design choice feels frustrating, unclear, or overly cautious:

Read the relevant decision document first.

If the reasoning still does not hold, that is a valid signal — but the original intent should be understood before proposing change.

---

Restraint is a design choice.
These documents explain why.

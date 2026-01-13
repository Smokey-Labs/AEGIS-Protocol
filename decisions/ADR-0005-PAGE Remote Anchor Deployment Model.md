# PAGE Remote Anchor — Deployment Model Decision (Pull-Based LAN Presence)

## Decision
PAGE will operate as a **Remote Anchor** for AEGIS across the LAN using a **pull-based enrollment model**.

Participation is explicit.
Presence is verifiable.
Updates are constrained.
No host is “taken over” by the collector.

This converts PAGE from a manual tool into durable LAN substrate without expanding AEGIS authority.

## What “Remote Anchor” Means
A PAGE Remote Anchor is a small, bounded component deployed on LAN hosts that:
- provides local observation signals (metadata, health indicators, local context)
- reports proof-of-presence back to AEGIS
- enables AEGIS to correlate observed state over time

It is a witness, not an enforcer.

## Why Pull-Based
Pull-based deployment aligns with AEGIS governance:
- avoids centralized remote execution on every host
- reduces credential sprawl and blast radius
- makes enrollment an explicit human act
- supports idempotent, repeatable updates
- ensures hosts only accept approved artifacts

Human choice sets time and participation; the system does not “spread.”

## Constraints (Non-Negotiable)
PAGE Remote Anchors must:
- be **non-destructive by default**
- be **bounded to approved operations**
- be **idempotent** (safe to run repeatedly)
- verify artifact integrity (signature/checksum) before install/update
- report version + last-seen evidence to AEGIS

PAGE Remote Anchors must not:
- mutate host state beyond their installation/runtime footprint
- escalate privileges beyond what is explicitly required
- trigger remediation or enforcement actions
- deploy themselves to other hosts
- accept arbitrary “free-form intent” at runtime

## Enrollment Principle
Enrollment is explicit and auditable:
- a human authorizes a host into scope
- the host pulls and installs PAGE from an internal distributor
- AEGIS records host enrollment and first proof-of-presence

No implicit enrollment.
No silent expansion.

## Relationship to AEGIS Accountability
LAN-level accountability is achieved by correlating:
- declared state (config, service intentions, ownership claims)
- observed state (ports, services, metadata drift, logs within scope)

The Remote Anchor strengthens observability where passive network inference is insufficient, while remaining within non-destructive governance boundaries.

## Naming Note
“Remote Anchor” is a working name.
Alternatives that may fit better:
- Witness Anchor
- Presence Anchor
- LAN Anchor
- Field Node
- Sentinel Node (only if it doesn’t imply enforcement)

Final naming should avoid “watcher” or “gatekeeper” language and emphasize witnessing, evidence, and bounded scope.

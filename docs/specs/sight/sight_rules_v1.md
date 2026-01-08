# AEGIS Sight Rules v1

## Principle: Knowledge Is Advisory Only
- AEGIS outputs are advisory.
- AEGIS must not perform actions implicitly.
- AEGIS must not represent advice as execution.

## Sight Levels

### Implied Sight
Implied sight is default read awareness that does not require operator gating.

Implied sight allows:
- existence (files/units/ports present)
- enumeration (names only for directories)
- metadata (owner/perms/mtime/size)
- hashes
- topology (dependency graphs, port ownership, cgroup/unit linkage)
- diffs (hash changed, unit changed, package changed)

Implied sight forbids:
- reading file contents
- reading log bodies
- reading environment variables
- dumping process command-line args when they may contain secrets
- database row/record extraction

Implied sight language should be observational:
- "present", "observed", "exists", "metadata indicates", "hash differs"

### Explicit Sight
Explicit sight is operator-gated extraction that requires a declared action and target.

Explicit sight requires:
1) sight="explicit"
2) action=<declared verb>
3) target=<declared target>
4) action allowlisted for the current phase/tier
5) target type allowed for the action
6) bounds enforced (limit/since/timeouts/max_bytes)
7) redaction policy applied
8) ledger entry recorded for the request and resulting evidence

Explicit sight language may be definitive only when supported by explicit evidence:
- "content shows", "log states", "value is", "line indicates"

## Rule A — Detail Must Be Explicit
- Any detail that requires reading bodies (file contents, logs, env, records) is explicit-only.
- Implied sight may reference existence and safe metadata, but not bodies.

## Rule B — Action Requires Observation Safeguard (Locked)
Action without observation can lead to catastrophe.
- Requests that include action+target must be explicit sight.
- Any missing action or target must not be inferred.

Enforcement:
- If sight=explicit and either action or target is missing -> REFUSED (or UNKNOWN if you prefer).
- If sight=implied and action/target appears -> REFUSED.

## Rule C — No Write Capability in v1 (Locked)
- No write verbs exist in v1.
- Any request that attempts to write, modify, delete, patch, or execute arbitrary commands must be REFUSED.

Defense in depth:
- Maintain an explicit blacklist of not-ready actions/verbs even if allowlist is primary.

## Shadow Zones
Shadow zones are explicit-only areas even under maximum implied awareness.

Always explicit-only:
- file contents (including configs)
- log bodies
- environment variables
- private keys, tokens, credentials
- process arguments when they may contain secrets
- credential stores and sensitive system areas

Implied sight may still:
- show that these items exist
- show safe metadata (with redaction of sensitive fields where applicable)
- show hashes to support change detection

## Bounds (Non-Negotiable)
All explicit extraction must be bounded:
- max bytes for read/unit_cat/inspect
- max lines/items for tail/list/status
- max since_sec for journal windows
- timeouts for probes

If bounds are not provided:
- defaults apply from the action allowlist.

## Evidence and Provenance
- Any concrete claim must be tied to evidence references.
- If evidence is missing or prohibited -> UNKNOWN with minimal "need" fields.

## Output Shape Preference
- Prefer storing extracted explicit evidence and returning `result_ref` over dumping bodies.
- When bodies must be returned, redact and bound them and record the redaction classes applied.

---

## How to Interpret the Sight Schemas

The JSON files in this directory are **normative contracts**.
They are not explanatory documents.

This section explains how to read them correctly.

---

### `sight_envelope_v1.json`

This schema defines the **only valid shape** of operator requests and AEGIS responses.

Key interpretation rules:
- `sight="implied"` means *awareness only*
- `sight="explicit"` requires both `action` and `target`
- `ask` is advisory text only and must never be treated as an instruction
- Explicit requests without action or target must fail
- Implied requests that include action or target must fail

This schema exists to prevent **implicit authority escalation**.

---

### `actions_v1.json`

This file is a **positive allowlist**.

Interpretation rules:
- If an action is not listed, AEGIS must refuse it
- Each action declares:
  - Required sight level
  - Valid target classes
  - Default bounds
  - Hard maximum bounds
- Bounds are mandatory, even if not supplied by the operator
- Redaction expectations are enforced by the executor

This file defines **what AEGIS can do**, not what it might want to do.

---

### `action_blacklist_v1.json`

This file is **defensive governance**.

Interpretation rules:
- Any action matching this list must be refused
- Blacklist checks are applied **before** allowlist resolution
- This file exists to protect against:
  - Accidental feature exposure
  - Future overreach
  - Authority drift under pressure

Even if an implementation exists, the blacklist wins.

---

### Relationship Between Markdown and JSON

- **Markdown files** explain intent, rules, and philosophy
- **JSON files** define enforceable structure and limits

When ambiguity exists:
- JSON defines what is allowed
- Markdown explains why

When conflict exists:
- The JSON contract is authoritative for execution
- The Markdown text is authoritative for interpretation

---

### Why This Split Exists

Humans reason in language.  
Systems enforce structure.

Sight v1 requires both to remain safe.

The schemas keep AEGIS honest.  
The documentation keeps humans informed.

# AEGIS Continuity Engine + Relationship DB (v1)
**Beta Spec — Always-Check-First Continuity + Audit-Grade Chat Ledger**

AEGIS is a governance framework, not a bot.  
Aurora is an interface, not the project.

This document defines the Continuity Engine and a Relationship Database used by `aegis-ask` to:
1) **Always check first** (compile continuity before every LLM call)
2) **Replicate Aurora’s operational “life”** (top-down reconstruction per turn)
3) **Write a direct audit log** of every chat prompt/response exchange
4) Preserve AEGIS principles: evidence-first, bounded authority, replayability

---

## 1. Goals

### 1.1 Primary Goals
- Every `aegis-ask` call produces a governed, replayable exchange record:
  - what was asked
  - what was injected as continuity
  - what the model responded
  - what drift checks fired (if any)
- Continuity is **explicit artifacts**, not implicit memory.
- Continuity compilation is **mandatory** (“always check first”).

### 1.2 Non-Goals
- No autonomous action.
- No hidden cross-session memory.
- No storing raw credentials, secrets, or evidence blobs by default.
- No “vibes-based” continuity (tone carryover, implied authorization, etc).

---

## 2. Canonical Terms

### 2.1 Session
A bounded timeline of chat exchanges under a specific:
- kernel snapshot (phase/tier constraints)
- model identity
- host role

Sessions exist for audit grouping, not “persona continuity”.

### 2.2 Turn
One message from one speaker:
- user → aurora (via LLM response)
Each `aegis-ask` produces two turns: one `user`, one `aurora`.

### 2.3 Exchange
The binding of:
- compiled prompt (kernel + continuity bundle + user input)
- model response
- validation results
- included artifact references

This is the core audit unit.

### 2.4 Continuity Bundle
The explicit, sourced context injected into the prompt.  
Continuity is **constructed**, not remembered.

---

## 3. Beta Rule: Sight Requires Values

In Beta semantics:
> If an observational capability is declared available (e.g., `sys_health`), **sight must return values** or explicitly report unavailability.

This prevents “sight-by-claim” and enforces honest observation.

---

## 4. Top-Down “Always Check First” Flow

### 4.1 `aegis-ask` lifecycle (single turn)

1) **Session Resume/Open**
- Identify active session or open a new one
- Bind: phase/tier, kernel hash, host role, model id

2) **Continuity Compile (mandatory)**
- Query eligible artifacts from Relationship DB + ledger + tool outputs
- Score/select under strict caps
- Produce `context_bundle.json` (+ hash)

3) **Prompt Render**
- Render kernel + context bundle + user input into compiled prompt
- Hash compiled prompt

4) **LLM Call**
- Send compiled prompt to model
- Receive response text

5) **Validation & DriftWatch Scan**
- Validate schema expectations (if applicable)
- Scan for authority drift, evidence drift, tone drift, etc.

6) **Audit Write (mandatory)**
- Store: turns, exchange binding, artifact refs, hashes, validation results

7) **Respond to user**
- Only after write succeeds (or write fails closed, per policy)

---

## 5. Relationship DB Schema (v1)

### 5.1 Storage principles
- Store **sanitized** prompt/response by default
- Always store hashes, timestamps, and provenance
- Never store raw secrets or evidence blobs by default

### 5.2 PostgreSQL schema (reference)

#### `chat_session`
sql
CREATE TABLE IF NOT EXISTS chat_session (
  session_id      UUID PRIMARY KEY,
  started_at      TIMESTAMPTZ NOT NULL,
  ended_at        TIMESTAMPTZ,
  phase           TEXT NOT NULL,     -- e.g. "beta"
  tier            INT  NOT NULL,     -- e.g. 1/2/3
  kernel_hash     TEXT NOT NULL,
  host_role       TEXT NOT NULL,     -- e.g. "brain"
  model_id        TEXT NOT NULL,     -- e.g. "ollama llama3.1:8b"
  status          TEXT NOT NULL      -- "open" | "closed"
);
CREATE TABLE IF NOT EXISTS chat_turn (
  turn_id         UUID PRIMARY KEY,
  session_id      UUID NOT NULL REFERENCES chat_session(session_id),
  created_at      TIMESTAMPTZ NOT NULL,
  speaker         TEXT NOT NULL,     -- "user" | "aurora"
  content         TEXT NOT NULL,     -- sanitized
  content_hash    TEXT NOT NULL,     -- sha256(content)
  refusal         BOOLEAN NOT NULL DEFAULT FALSE,
  authority_state TEXT NOT NULL,     -- "ok" | "insufficient" | "blocked"
  driftwatch_id   TEXT              -- nullable external reference
);
CREATE INDEX IF NOT EXISTS idx_chat_turn_session_time ON chat_turn(session_id, created_at);
CREATE TABLE IF NOT EXISTS llm_exchange (
  exchange_id         UUID PRIMARY KEY,
  session_id          UUID NOT NULL REFERENCES chat_session(session_id),

  turn_user_id        UUID NOT NULL REFERENCES chat_turn(turn_id),
  turn_aurora_id       UUID NOT NULL REFERENCES chat_turn(turn_id),

  context_bundle_hash  TEXT NOT NULL,
  context_bundle_json  TEXT,          -- optional (store compact JSON); can be NULL if stored elsewhere

  prompt_hash          TEXT NOT NULL,
  prompt_compiled      TEXT,          -- sanitized compiled prompt (optional)

  response_hash        TEXT NOT NULL,
  response_sanitized   TEXT,          -- optional; may duplicate turn content, retained for convenience

  validator_summary    TEXT NOT NULL,  -- compact JSON or text summary
  created_at           TIMESTAMPTZ NOT NULL
);
CREATE INDEX IF NOT EXISTS idx_llm_exchange_session_time ON llm_exchange(session_id, created_at);
CREATE TABLE IF NOT EXISTS context_artifact_ref (
  ref_id            UUID PRIMARY KEY,
  exchange_id       UUID NOT NULL REFERENCES llm_exchange(exchange_id),

  artifact_type     TEXT NOT NULL,     -- "ledger_fact" | "driftwatch" | "sys_health" | "note" | "decision"
  artifact_id       TEXT NOT NULL,
  artifact_hash     TEXT,
  included_reason   TEXT NOT NULL,

  observed_at       TIMESTAMPTZ,
  freshness_s       INT,
  risk_flags        TEXT              -- compact JSON/text
);
CREATE INDEX IF NOT EXISTS idx_context_artifact_exchange ON context_artifact_ref(exchange_id);
---

## 6. Continuity Engine (Compiler)

The Continuity Engine is responsible for constructing the **Context Bundle** that is injected into every LLM invocation.

Continuity is **mandatory** and **top-down**.  
No LLM call may occur without a compiled continuity bundle.

This engine does not store memory.  
It **selects, scores, and injects explicit artifacts**.

---

### 6.1 Inputs

The Continuity Engine consumes:

- `session_id`
- `user_input`
- active kernel snapshot (phase, tier, authority rules)
- eligible artifacts:
  - recent chat turns (sanitized)
  - ratified ledger facts
  - open DriftWatch events
  - latest observational tool outputs (Beta)
  - declared session or user intent

If an artifact is not explicitly available, it must not be inferred.

---

### 6.2 Eligibility Rules (Hard Gates)

Artifacts are eligible for continuity injection **only if all conditions are met**:

- **Sourced**  
  - Must reference a `turn_id`, `ledger_id`, or tool execution id

- **Authority-Compatible**  
  - Allowed under the current phase and tier

- **Non-Sensitive by Default**  
  - No credentials, secrets, tokens, or raw `.env` content

- **Not an Evidence Blob**  
  - No raw logs, full journal dumps, or binary artifacts

Artifacts failing any gate must be excluded and recorded as exclusions.

---

### 6.3 Artifact Classes

Continuity artifacts are grouped into rings to limit drift.

#### Ring 0 — Always Include (Minimal)
- Phase and tier
- Kernel hash
- Core safety constraints
- Last observational timestamps (e.g., `sys_health`)

This ring must be **small and stable**.

---

#### Ring 1 — Situational Context
- Facts relevant to the current user input
- Open DriftWatch items related to the topic
- Current declared intent (“what this session is doing”)

Most sessions should include **Ring 0 + Ring 1 only**.

---

#### Ring 2 — Deep History (Rare)
Included **only if**:
- Explicitly requested by the user, **or**
- Required to surface a conflict or inconsistency

Ring 2 must never be injected implicitly.

---

### 6.4 Conflict Handling

If two artifacts conflict:

- Include **both**
- Mark the conflict explicitly
- Do **not** resolve by inference
- Do **not** collapse into a synthesized conclusion

Conflict visibility is a governance feature.

---

### 6.5 Artifact Scoring (Reference)

Eligible artifacts may be ranked using:

- **Relevance** to user input entities/topics
- **Recency / Freshness**
- **Authority Fit**
- **Risk** (unknowns or conflicts reduce score unless required)
- **Utility** (does this artifact materially change the response?)

Scoring influences selection only; it does not override eligibility rules.

---

### 6.6 Output: `context_bundle.json` (v1)

The Context Bundle is the **only continuity injected into the prompt**.

json
{
  "session_id": "uuid",
  "phase": "beta",
  "tier": 2,
  "kernel_hash": "sha256:…",
  "host_role": "brain",
  "model_id": "ollama llama3.1:8b",
  "compiled_at": "2026-01-08T00:00:00Z",

  "sight": {
    "sys_health": {
      "available": true,
      "observed_at": "2026-01-08T00:00:00Z",
      "freshness_s": 3,
      "values": {
        "cpu_load": 0.42,
        "mem_used_mb": 3812,
        "disk_free_pct": 71
      },
      "unknowns": ["gpu_health"]
    }
  },

  "constraints": [
    {"rule": "read_only_default", "source": "kernel"},
    {"rule": "evidence_first", "source": "kernel"},
    {"rule": "unknowns_are_risk", "source": "kernel"}
  ],

  "facts": [
    {
      "id": "ledger:000183",
      "claim": "UNKNOWN domains are treated as risk needing verification.",
      "source": "ledger",
      "observed_at": "2026-01-07T23:31:44Z",
      "confidence": "high"
    }
  ],

  "open_items": [
    {
      "id": "driftwatch:0042",
      "issue": "Sight requires sys_health values in Beta.",
      "status": "open"
    }
  ],

  "intent": [
    {
      "intent": "Answer user question under current authority constraints.",
      "source": "system"
    }
  ],

  "exclusions": [
    {
      "artifact": "raw_journal_logs",
      "reason": "evidence_blob_disallowed"
    }
  ]
}
## 7. Observational Tools vs Actuation Tools

Tool use is divided into two classes. This separation is mandatory and is a primary drift-control mechanism.

### 7.1 Tool Classes

#### Observational Tools
Read-only tools that may be executed automatically under sight rules.

**Properties**
- Read-only
- Safe to execute without human authorization (when allowed by tier)
- Must return values in Beta if declared available

**Examples**
- `sys_health`
- `index_status`
- `index_changed` (detect-only)
- `disk_usage`
- `service_status` (read-only)

#### Actuation Tools
State-changing tools that must never be executed automatically.

**Properties**
- Mutates system state
- Requires explicit authority elevation and human approval
- Must be logged as intent before any execution path exists

**Examples**
- `systemctl restart`
- package install/remove
- configuration writes
- file deletes / moves
- firewall rule changes

### 7.2 Beta Enforcement: Sight Must Return Values

If an observational tool is declared available, the continuity bundle must contain:

- `available: true`
- `observed_at`
- `freshness_s`
- `values` (actual returned values)

If values cannot be returned, the bundle must explicitly state:

- `available: false`
- `reason` in: `not_installed` | `blocked_by_tier` | `tool_error`

**Beta failure condition**
- “Sight” is declared
- The tool is available
- But no values are returned

This is classified as **completeness drift** and fails the harness under Beta semantics.

---

## 8. Sanitization Policy (Default-On)

The Relationship DB is an audit log. It must remain safe to retain and safe to replay.

### 8.1 Default Redaction Targets

By default, sanitize (strip or redact) the following from all stored content:

- secrets and tokens of any kind
- API keys
- private keys and certificates
- passwords
- full `.env` contents
- credential files
- raw evidence blobs
- raw journal/log dumps beyond minimal excerpts required for a specific claim

If a datum is not required for continuity, it must not be stored.

### 8.2 Hash Preservation (Always)

Even when content is redacted, always store cryptographic linkage:

- `content_hash` (turn content)
- `prompt_hash` (compiled prompt)
- `response_hash` (model output)
- `context_bundle_hash` (compiled continuity bundle)

Hashes preserve replayability without retaining unsafe payloads.

---

## 9. Validator and DriftWatch Integration

The validator is the enforcement gate between model output and canonical record.

### 9.1 Minimum Validation Summary

Each exchange must record a compact validation summary (stored in `llm_exchange.validator_summary`), including:

- `schema_ok` (boolean)
- `evidence_drift` (boolean)
- `authority_drift` (boolean)
- `tone_drift` (boolean)
- `completeness_drift` (boolean) **Beta critical**
- `notes` (short text)

### 9.2 Completeness Drift (Beta Critical)

Completeness drift triggers when:

- the current tier allows observational tool use, and
- the relevant observational tool is declared available, and
- the assistant refuses or returns “no access” or returns no values

This prevents “epistemic evasion” where sight is claimed but not backed by observation.

### 9.3 DriftWatch Event Emission (Reference)

If any drift condition triggers, DriftWatch must emit an event with:

- kind: `DRIFTWATCH`
- severity: tier-dependent mapping
- drift_class: `authority` | `evidence` | `tone` | `schema` | `completeness`
- exchange linkage: `exchange_id`, `prompt_hash`, `response_hash`
- summary: short, machine-parseable

---

## 10. Implementation Notes (v1)

These notes are intentionally minimal and implementation-agnostic.

### 10.1 Where Continuity Lives

Recommended Beta deployment:

- Continuity Engine runs on Capsuleer (brain host)
- Relationship DB runs locally on Capsuleer
- Tool outputs stored as compact JSON summaries (not raw dumps)
- Evidence blobs remain outside the Relationship DB and are referenced by hash/id only

### 10.2 Minimal `aegis-ask` Wiring

Each `aegis-ask` must follow this ordered pipeline:

1. `session_open_or_resume()`
2. `continuity.compile(session_id, user_input)` → `context_bundle.json`
3. `prompt.render(kernel, context_bundle, user_input)` → compiled prompt
4. `llm.invoke(compiled_prompt)` → response
5. `validator.scan(response)` → validation summary + drift flags
6. `audit.write_exchange(...)` → turns + exchange + refs
7. print response to user

**Order is mandatory:** continuity compile must occur before LLM invocation.

### 10.3 Failure Behavior

- If audit write fails, fail closed:
  - do not present output as canonical
  - return an operator-visible error that the exchange was not recorded
- If continuity compile fails, do not call the LLM

---

## 11. Acceptance Criteria (Beta)

Continuity Engine v1 is compliant if all conditions are true:

### 11.1 Mandatory Preflight Continuity
- Every `aegis-ask` compiles a continuity bundle before the LLM is called
- The compiled bundle hash is recorded in the exchange

### 11.2 Canonical Audit Record
- Every `aegis-ask` produces:
  - one session (open or reused)
  - two turns (`user`, `aurora`)
  - one exchange binding prompt + response
  - artifact refs showing what continuity was injected and why

### 11.3 Sight Returns Values (Beta)
- If `sys_health` (or any observational tool) is available and allowed:
  - the bundle contains real values
  - or explicitly declares unavailability with a reason
- “Sight without values” fails Beta harness (completeness drift)

### 11.4 Sanitization Default-On
- Secrets and evidence blobs are not stored by default
- Hash linkage is preserved

### 11.5 Replayability Linkage
- Prompt hash, response hash, and context bundle hash are recorded for each exchange

---

## 12. Future Extensions (Non-Binding)

These are not required for v1 compliance.

### 12.1 Session Summaries
- Generate compact session summary artifacts with explicit labeling:
  - facts (ratified)
  - theories (unratified)
  - decisions (explicit)

### 12.2 Conflict Graph
- Maintain a graph of contradictory facts/artifacts
- Require explicit user action to resolve conflicts

### 12.3 Retention Controls
- Policy-driven retention windows:
  - keep hashes indefinitely
  - prune sanitized content after N days if configured

### 12.4 Admin Trace Route UI
- Per exchange display:
  - continuity bundle → compiled prompt → response → validator results
  - artifact provenance and exclusions
  - drift events and severities

### 12.5 Controlled Deep-History Recall
- Only inject Ring 2 history when explicitly requested
- Provide a disclosure line showing what deep history was added and why

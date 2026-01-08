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
```sql
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

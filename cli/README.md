# AEGIS CLI Contract (Beta)

This document defines the **operator-facing CLI contract** for AEGIS during Beta.

**Core rule:** Any operator interaction (including initiating a chat) must occur through `aegis`.

---

## Design Goals

- One operator console surface: `aegis`
- Consistent logging to Continuity Engine
- Deterministic behavior (no “surprises”)
- Beta-safe defaults (read-only unless explicitly authorized)
- Flags remain terse in daily use; full names are discoverable via `aegis -h`

---

## CLI Structure

General form:

`aegis <verb> [flags...] [positionals...]`

- The `<verb>` is a full word (e.g., `prompt`, `report`, `drift`, `status`)
- All subsequent flags are **single-letter only**
- Long flag names exist **only** as reference text in `aegis -h`

---

## Flag Parsing Rules

1) **Single-letter flags only**  
   After the verb, every option flag must be a single letter (e.g., `-s`, `-n`, `-j`). Long names appear only in `aegis -h`.

2) **Primary verb is a word**  
   The first positional token after `aegis` is the verb: `prompt`, `report`, `drift`, `status`, etc.

3) **Flags may be combined only when they are valueless**  
   `-njv` is valid and equivalent to `-n -j -v` only if those flags take no values.

4) **Value-taking flags cannot be combined**  
   Any flag that expects a value must appear as its own token:  
   ✅ `-s 20260109_221233`  
   ❌ `-js 20260109_221233`

5) **Value-taking flags consume exactly one value token**  
   A value-taking flag consumes the next token as its value (even if it begins with `-`). If the value is invalid, the parser must refuse.

6) **End-of-options marker is supported**  
   `--` ends flag parsing. Everything after is positional input.

7) **Unknown flags are a hard error**  
   Unknown flags must:
   - exit non-zero
   - print a one-line error + `Run: aegis -h`
   - optionally log an operator error event

8) **Precedence & ambiguity resolution (no surprises)**  
   8.1 Flags parse left-to-right; the first non-flag token begins positionals (unless consumed as a value).  
   8.2 Mutually exclusive flags are hard errors (no “last one wins”) unless explicitly designed as an override.  
   8.3 Defaults are deterministic and documented (text output default; read-only default for Beta).  
   8.4 If intent is ambiguous, refuse + explain + point to help.  
   8.5 Provide “Did you mean …?” suggestions, but never auto-correct and execute.

---

## Commands

### `aegis prompt`
The only operator chat entry point (interactive or one-shot). Always logs via Continuity Engine.

**Flags**
- `-i` interactive REPL
- `-p <text>` one-shot prompt
- `-c` continue last session
- `-n` new session
- `-s <id>` explicit session id
- `-t <tag>` note/tag (e.g., beta, triage, experiment)
- `-j` JSON-only output
- `-x` text output
- `-r` read-only mode (default in Beta)
- `-o` ops mode (requires authorization; otherwise refuse + log)
- `-m <target>` target label (host/service/domain)

**Examples**
- `aegis prompt -i -n -t beta`
- `aegis prompt -p "return raw json only: status" -j`
- `aegis prompt -i -c`

---

### `aegis status`
Phase/tier, mode, last update time, and (optionally) evidence availability summary.

**Flags**
- `-j` JSON-only
- `-v` verbose
- `-m <target>` target scope

Examples:
- `aegis status -v`
- `aegis status -j -m capsuleer`

---

### `aegis drift`
DriftWatch viewer.

**Flags**
- `-o` open events only
- `-a` all events (history)
- `-j` JSON-only
- `-v` verbose

Examples:
- `aegis drift -o`
- `aegis drift -a -j`

---

### `aegis diag`
Diagnostics, including regression harness execution.

**Flags**
- `-h` show diag options
- `-r` run regression harness
- `-b <name>` baseline name (e.g., B0)
- `-j` JSON-only
- `-v` verbose

Examples:
- `aegis diag -r -b B0`
- `aegis diag -r -j`

---

### `aegis session`
Session tools for listing, inspecting, and exporting session data.

**Flags**
- `-l` list recent sessions
- `-s <id>` select session
- `-p` print transcript (human)
- `-j` JSON-only (metadata)
- `-e <path>` export a single session bundle

Examples:
- `aegis session -l`
- `aegis session -s 20260109_221233 -p`

---

## `aegis report` (Beta Support Bundle)

Produces a single zipped support bundle a Beta user can send to maintainers.

**Default behavior**
- Output filename: `aegis_report_<host>_<timestamp>.zip`
- Includes a root `MANIFEST.json` describing what was included and what was redacted
- Redaction is ON by default

**Flags**
- `-o <path>` output directory (default: current directory)
- `-j` print JSON summary to stdout (path + counts)
- `-v` verbose progress output
- `-s <id>` include only a specific session id
- `-d <n>` number of days of history to include (default: 7)
- `-x` extended (heavier logs; still redacted)
- `-u` unsafe/unredacted (requires explicit elevated authorization; otherwise refuse + log)

**Bundle contents (recommended)**
- Identity & version: `aegis_version.txt`, `phase_tier.json`, `MANIFEST.json`
- DriftWatch: open + history (within window)
- Sessions: index + included session transcripts
- System context: OS release, uname, hostname, time
- Service state: governed health snapshot + relevant unit statuses
- Logs: scoped, capped extracts relevant to AEGIS/Aurora
- Config fingerprints: hashes + mtimes only (never raw secret contents)

**Examples**
- `aegis report`
- `aegis report -o "C:\Users\Gregory\Desktop" -v`
- `aegis report -d 14`
- `aegis report -s 20260109_221233`

---

## Help Output Contract

`aegis -h` must document the *meaning* of each single-letter flag and show its long-name reference.

Example (help text reference only):
- `prompt`
  - `-i` interactive — long name: `--interactive`
  - `-s` session id — long name: `--session`
- `report`
  - `-o` output dir — long name: `--output`
  - `-d` days — long name: `--days`
  - `-u` unsafe/unredacted — long name: `--unsafe-unredacted`

Long names are for discoverability and documentation only; day-to-day usage remains single-letter flags.

---

# AEGIS Documentation Burndown
**Evidence-Grounded Documentation Inventory**

---

## Documentation Rules (Authoritative)

This repository follows the AEGIS documentation doctrine:

1. **No claim without evidence**
2. **Chat transcripts are admissible evidence**
3. **Live system output outranks documentation**
4. **UNKNOWN is a valid, preferred state**
5. **Speculation is prohibited**

If evidence does not exist, the documentation MUST explicitly state:
> UNKNOWN — Evidence Pending

---

## Evidence Sources Currently Allowed

| Source Type | Status |
|------------|--------|
| Chat transcripts | ✅ Allowed |
| Regression harness outputs (posted in chat) | ✅ Allowed |
| CLI output pasted into chat | ✅ Allowed |
| Live system inspection | ❌ Not available |
| Filesystem paths | ❌ Not available |
| systemd units | ❌ Not available |

---

## Documentation Inventory

### Operator Documentation

| Document | Status | Evidence Basis |
|--------|-------|----------------|
| AEGIS_OVERVIEW.md | 🟡 Draftable | Design philosophy discussions |
| RESPONSE_SEMANTICS.md | 🟢 Draftable | Harness outputs, refusal examples |
| OPERATOR_QUICKSTART.md | 🔴 Blocked | Requires live CLI evidence |
| TROUBLESHOOTING.md | 🔴 Blocked | Requires runtime failure cases |

---

### Governance & Authority

| Document | Status | Evidence Basis |
|--------|-------|----------------|
| AUTHORITY_MODEL.md | 🟢 Draftable | Phase/tier discussions, `aegis -p` outputs |
| DRIFTWATCH.md | 🟢 Draftable | Concept + observed commands |
| AEGIS_PROTOCOL.md | 🟡 Partial | Named, but full spec incomplete |

---

### Testing & Validation

| Document | Status | Evidence Basis |
|--------|-------|----------------|
| REGRESSION_HARNESS.md | 🟢 Draftable | Posted test result tables |
| BEHAVIORAL_GUARANTEES.md | 🟡 Partial | Requires formal mapping |
| RELEASE_GATING.md | 🔴 Blocked | Depends on CI/installer integration |

---

### Architecture & Operations

| Document | Status | Reason Blocked |
|--------|-------|----------------|
| COMPONENT_MAP.md | 🔴 | Requires filesystem + services |
| PATHS_AND_PERMISSIONS.md | 🔴 | Requires live host |
| SYSTEMD_SERVICES.md | 🔴 | Requires unit inspection |
| LEDGER_DB_SCHEMA.md | 🔴 | Requires sqlite schema |
| INSTALLER_SPEC.md | 🔴 | Beta-stage artifact |

---

## Explicit Non-Claims

The following are **intentionally undocumented** until evidence exists:

- Exact file paths
- Service names and dependencies
- Network listeners
- Database schema details
- Installer behavior
- Upgrade / rollback procedures

Any future documentation in these areas MUST cite:
- command output
- file contents
- or authoritative commit hashes

---

## Next Unblocking Event

**Required:** Live access to Capsuleer  
**Purpose:** Promote blocked documents to draftable

Until then, documentation work is restricted to:
- philosophy
- governance
- semantics
- behavioral guarantees

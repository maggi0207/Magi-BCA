# ADD NPP — Implementation Progress Tracker

> Use this file as the single visual progress tracker inside Cursor.
>
> **Status legend**
> - ⬜ Not Started
> - 🟡 In Analysis
> - 🔵 Waiting for Office Evidence
> - 🟣 Ready for Copilot
> - 🟠 Implementing
> - 🟢 Completed
> - 🔴 Blocked
> - ⚪ N/A

---

## Overall Progress

**Current Phase:** `0 — Architecture Discovery`

**Overall Status:** 🟡 In Analysis

**Progress:** `0 / 8 phases completed`

### Master Flow

```text
┌──────────────────────────────┐
│ 0. ARCHITECTURE DISCOVERY    │
│ ⬜                            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ 1. SEARCH FOUNDATION         │
│ ⬜                            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ 2. BEGIN NPP PAF ENTRY       │
│ ⬜                            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ 3. PAF CONSTRUCTION          │
│ ⬜                            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ 4. NO-NPI EXCEPTION          │
│ ⬜                            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ 5. ADD NPP BUSINESS FLOW     │
│ ⬜                            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ 6. AUTO-ACCEPT / CPC / CVI   │
│ ⬜                            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ 7. CACTUS + SUPPORTING       │
│ ⬜                            │
└──────────────────────────────┘
```

---

# Phase 0 — Existing Architecture Discovery

**Status:** 🟡 In Analysis

**Rule:** No code changes yet.

### Architecture

- [ ] Existing Begin PAF identified
- [ ] Existing Enforce NPI Search identified
- [ ] Existing Practitioner Search identified
- [ ] Existing Add New Practitioner identified
- [ ] Existing Add Practitioner to Facility identified
- [ ] Existing PAF creation flow identified
- [ ] Existing PAF Tasks identified
- [ ] Existing Practitioner Information Card identified
- [ ] Existing authorization/RBAC identified
- [ ] Existing CACTUS integration identified
- [ ] Existing Auto-Accept implementation identified
- [ ] Existing CPC implementation identified
- [ ] Existing CVI implementation identified
- [ ] Existing PAF PDF/history identified
- [ ] Existing audit implementation identified
- [ ] Existing reporting implementation identified
- [ ] Existing tests identified

### Office Evidence

- [ ] All required repository files requested
- [ ] File paths verified
- [ ] Classes/components verified
- [ ] Methods/functions verified
- [ ] Existing implementation behavior verified
- [ ] No invented repository details

### Exit Criteria

- [ ] Architecture map completed
- [ ] Add Practitioner to Facility reference flow understood
- [ ] Reuse points identified
- [ ] Gaps identified
- [ ] Ready to analyze Phase 1

---

# Phase 1 — Practitioner Search Foundation

**Tickets:** `53658`, `127104`

**Status:** ⬜ Not Started

### 53658 — NPP: PAF - Search

- [ ] NPI primary/required behavior verified
- [ ] 10-digit NPI validation verified
- [ ] Existing Enforce NPI Search located
- [ ] Existing search validation located
- [ ] Existing duplicate practitioner search located
- [ ] NPP search integration gap identified
- [ ] Tests identified

### 127104 — Add NPP to Facility Practitioner Search

- [ ] Existing Add New Practitioner search located
- [ ] Existing NPI search reused
- [ ] No-NPI exception connection understood
- [ ] Alternate search criteria verified
- [ ] Audit logging verified
- [ ] No duplicate search logic introduced
- [ ] Tests identified

### Exit Criteria

- [ ] Search architecture confirmed
- [ ] Reuse strategy confirmed
- [ ] Copilot prompt ready
- [ ] Copilot implementation completed
- [ ] Tests passed
- [ ] Regression checked

---

# Phase 2 — Begin NPP PAF Entry Point

**Ticket:** `136871`

**Status:** ⬜ Not Started

### Child Tickets

- [ ] `136872` Entry point / existing Begin PAF preservation
- [ ] `136873` Dashboard action
- [ ] `136876` Tooltip
- [ ] `136878` Launch existing NPI Search
- [ ] `136882` Preserve existing Begin PAF
- [ ] `136883` Security / authorization

### Verification

- [ ] Begin NPP PAF appears for authorized MSP
- [ ] Existing Begin PAF unchanged
- [ ] Tooltip exact text verified
- [ ] Existing Enforce NPI Search launched
- [ ] Unauthorized users blocked
- [ ] Direct URL security checked
- [ ] PSG/Recruitment unaffected
- [ ] Keyboard accessibility checked
- [ ] Screen reader behavior checked
- [ ] Tests passed
- [ ] Regression passed

---

# Phase 3 — PAF Construction

**Tickets:** `132255`, `132275`, `131794`, `132247`

**Status:** ⬜ Not Started

## 132255 — ADD NPP to Facility PAF Action

- [ ] Existing PAF action framework located
- [ ] ADD NPP action registration located
- [ ] Action title verified
- [ ] Action description verified
- [ ] Required status verified
- [ ] Work action behavior verified
- [ ] Navigation/open behavior verified
- [ ] Review & Submit blocking verified

## 132275 — PAF Tasks

- [ ] Task architecture located
- [ ] ADD NPP task registration verified
- [ ] Task title verified
- [ ] Required flag verified
- [ ] Task completion behavior verified
- [ ] Review & Submit dependency verified

## 131794 — Add New Practitioner Card

- [ ] Existing Add New Practitioner card located
- [ ] Net-new ADD NPP behavior identified
- [ ] Demographic task behavior verified
- [ ] Address task behavior verified
- [ ] Specialty task behavior verified
- [ ] ADD NPP task behavior verified

## 132247 — Existing Practitioner

- [ ] Existing Practitioner card located
- [ ] Active/inactive logic verified
- [ ] Optional/required tasks verified
- [ ] Delegate suppression verified
- [ ] Facility-specific question behavior verified

### Exit Criteria

- [ ] PAF structure matches requirements
- [ ] Net-new flow verified
- [ ] Existing practitioner flow verified
- [ ] Copilot implementation completed
- [ ] Tests passed
- [ ] Regression passed

---

# Phase 4 — Controlled No-NPI Exception

**Ticket:** `131785`

**Status:** ⬜ Not Started

- [ ] No-NPI entry point verified
- [ ] Exception criteria verified
- [ ] Reason field verified
- [ ] First Name validation verified
- [ ] Last Name validation verified
- [ ] State validation verified
- [ ] License Number validation verified
- [ ] Like/normalized license matching verified
- [ ] Exception audit logging verified
- [ ] Duplicate check verified
- [ ] Net-new vs existing behavior verified
- [ ] Tests passed
- [ ] Regression passed

---

# Phase 5 — ADD NPP Business Workflow

**Status:** ⬜ Not Started

## Demographics

- [ ] First Name required
- [ ] Last Name required
- [ ] Degree required
- [ ] Provider Category required
- [ ] Individual NPI required
- [ ] Email behavior verified
- [ ] Cell behavior verified
- [ ] DOB/SSN behavior verified
- [ ] Gender behavior verified
- [ ] Existing CACTUS values prepopulate

## Address

- [ ] Home address hidden
- [ ] Credentialing address hidden
- [ ] Alternate address hidden
- [ ] Active primary + active facility affiliation behavior verified
- [ ] Primary address edit restriction verified
- [ ] Add Address behavior verified
- [ ] Required address fields verified
- [ ] Optional address fields verified

## Specialty

- [ ] Net-new specialty requirement verified
- [ ] Existing practitioner optional behavior verified
- [ ] No-primary behavior verified
- [ ] Active primary read-only behavior verified
- [ ] Secondary/alternate hidden
- [ ] PPI behavior verified
- [ ] Primary specialty/PPI validation verified

## ADD NPP Workflow

- [ ] Practitioner Type = NPP only
- [ ] Active duty / VA question verified
- [ ] VA + PA question verified
- [ ] VA + PA = yes behavior verified
- [ ] VA + PA = no behavior verified
- [ ] Non-VA behavior verified
- [ ] State License visibility verified
- [ ] License PSV requirement verified
- [ ] Sanctions PSV requirement verified
- [ ] NPI PSV requirement verified

## License

- [ ] Existing licenses recalled
- [ ] Existing license selection verified
- [ ] "None of these apply" option verified
- [ ] New license duplicate check verified
- [ ] State verified
- [ ] Effective Date verified
- [ ] License Number verified
- [ ] Status verified
- [ ] Expiration Date verified
- [ ] Field of Licensure verified
- [ ] State-specific license rule verified
- [ ] Active status behavior verified
- [ ] Temporary Permit behavior verified
- [ ] Active Military behavior verified
- [ ] Active-Compact behavior verified
- [ ] Expired/matured behavior verified
- [ ] PA behavior verified

## PSV

- [ ] License PSV upload verified
- [ ] Sanctions PSV upload verified
- [ ] NPI PSV upload verified
- [ ] DOC accepted
- [ ] DOCX accepted
- [ ] PDF accepted
- [ ] JPG accepted
- [ ] TIFF accepted
- [ ] Unsupported file rejection verified
- [ ] PDF/HTML conversion verified where required
- [ ] CACTUS attachment verified
- [ ] Metadata verified
- [ ] Audit trace verified

---

# Phase 6 — Auto-Accept / CPC / CVI

**Tickets:** `132591`, `132537`

**Status:** ⬜ Not Started

## Normal Flow

- [ ] Normal ADD NPP auto-accept verified
- [ ] Completed queue behavior verified
- [ ] PAF history verified
- [ ] MSP identity verified
- [ ] HCA Corporate history verified
- [ ] PAF PDF generated
- [ ] Provider Record attachment verified

## CPC Exceptions

- [ ] Each documented CPC trigger verified
- [ ] Manual processing message verified
- [ ] CPC routing verified
- [ ] CVI created only where required
- [ ] CVI type verified
- [ ] CVI initial status verified
- [ ] Trigger notes verified
- [ ] Due date rule verified
- [ ] PAF PDF attached to CVI
- [ ] CPC completion behavior verified

---

# Phase 7 — CACTUS Integration

**Tickets:** `132600`, `131724`

**Status:** ⬜ Not Started

- [ ] Net-new provider creation verified
- [ ] Existing provider behavior verified
- [ ] First Name
- [ ] Last Name
- [ ] Suffix
- [ ] Degree
- [ ] Provider Category
- [ ] NPI
- [ ] Entity assignment
- [ ] NPP Active assignment where applicable
- [ ] Security rules
- [ ] Primary address creation
- [ ] Specialty creation
- [ ] PPI creation
- [ ] License creation/update behavior
- [ ] Existing recalled license protection verified
- [ ] NPI image
- [ ] Sanctions record
- [ ] Sanctions image
- [ ] Audit
- [ ] Integration tests passed

---

# Phase 8 — Security / CPC / Reporting

**Tickets:** `131728`, `131783`, `132687`

**Status:** ⬜ Not Started

## Security

- [ ] MSP authorization
- [ ] NPP access
- [ ] Unauthorized UI hidden/blocked
- [ ] Direct URL blocked
- [ ] PSG unaffected
- [ ] Recruitment unaffected
- [ ] Existing session/security model preserved

## CPC Dashboard

- [ ] ADD NPP PAF type appears where required
- [ ] Filtering verified
- [ ] Selection verified
- [ ] CPC access verified

## Reporting

- [ ] Monthly total received
- [ ] Total routed CPC
- [ ] Percentage routed CPC
- [ ] Reporting Team distribution
- [ ] CPC Distribution List
- [ ] Facility Undefined Practitioner CVI exclusion from MOR statistics

---

# Cross-Cutting Validation

## PAF PDF / History

- [ ] PDF shows "ADD NPP to Facility"
- [ ] Correct PAF type mapping
- [ ] Correct template
- [ ] Correct generator
- [ ] Correct attachment
- [ ] Correct history
- [ ] MSP identity captured
- [ ] HCP System User behavior verified

## Audit

- [ ] Duplicate practitioner audit
- [ ] Duplicate license audit
- [ ] Image audit
- [ ] Auto-accept history
- [ ] MSP identity
- [ ] HCA Corporate identity
- [ ] Exception search audit

## Regression

- [ ] Existing Begin PAF
- [ ] Existing NPI search
- [ ] Existing Add New Practitioner
- [ ] Existing Add Practitioner to Facility
- [ ] Existing PAF Tasks
- [ ] Existing CPC flow
- [ ] Existing PAF types
- [ ] Existing packet behavior

---

# Copilot Prompt Readiness

Before every Copilot prompt:

- [ ] Ticket understood
- [ ] Business requirement mapped
- [ ] Existing implementation inspected
- [ ] Exact file paths verified
- [ ] Exact class/component verified
- [ ] Exact method verified
- [ ] Reuse point identified
- [ ] Required change defined
- [ ] Files expected to change identified
- [ ] Acceptance criteria mapped
- [ ] Tests identified
- [ ] Regression impact identified
- [ ] No guessed architecture

**If any box is unchecked: STOP → ask user for office evidence.**

---

# Current Work Log

Update this section after each Cursor ↔ Office iteration.

### Current Ticket

`TBD`

### Current Phase

`TBD`

### Status

`⬜ Not Started`

### What Cursor Knows

- 

### What Office Evidence Is Needed

- 

### What Was Implemented

- 

### Copilot Prompt Used

- 

### Test Result

- 

### Issues / Errors

- 

### Next Action

- 

---

# Final Project Status

| Area | Status |
|---|---|
| Architecture Discovery | ⬜ |
| Practitioner Search | ⬜ |
| Begin NPP PAF | ⬜ |
| PAF Construction | ⬜ |
| No-NPI Exception | ⬜ |
| ADD NPP Business Flow | ⬜ |
| Auto-Accept / CPC / CVI | ⬜ |
| CACTUS | ⬜ |
| Security / CPC / Reporting | ⬜ |
| PDF / History | ⬜ |
| Audit | ⬜ |
| Regression | ⬜ |
| Final Review | ⬜ |

---

# Completion Rule

Do not mark a ticket `🟢 Completed` merely because code was written.

A ticket becomes **🟢 Completed** only after:

```text
Requirement understood
       ↓
Existing architecture verified
       ↓
Implementation completed
       ↓
Tests passed
       ↓
Regression checked
       ↓
Cursor review completed
       ↓
Status = 🟢 Completed
```

## Engineering Principle

> **Understand → Trace → Verify → Reuse → Minimal Change → Test → Review → Complete**

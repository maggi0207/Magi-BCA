# Office Copilot Prompt — Existing Code Flow Source of Truth

**What this is for**

Office Copilot must read the HCP repo and write **one markdown file**. That file becomes the **repository source of truth** for ADD NPP work.

Cursor will use it to fill implementation prompts. If a field is missing, Cursor **cannot** write a Copilot prompt for that ticket.

| Source of truth | What it governs |
|---|---|
| NPP V5 requirements | Business rules |
| Azure DevOps tickets | Scope, AC, ticket boundaries |
| **This generated code-flow file** | Real files, classes, methods, routes, reuse points, tests |

This is **analysis only**. No NPP coding. No refactors.

**After Copilot finishes**, copy the file to this Cursor workspace:

```text
08_Office_Repository_Evidence/HCP_Existing_PAF_NPP_Code_Flow.md
```

---

## How to run on the office laptop

1. Open `Hca.Credentialing.sln`.
2. Paste the **Copy-paste prompt** below into GitHub Copilot Chat as one message.
3. Copilot may create **only** `_analysis/HCP_Existing_PAF_NPP_Code_Flow.md`.
4. If chat truncates: `Write the complete file to disk. Do not summarize. Fill every Implementation Prompt Seed.`
5. Bring the file back to Cursor before any implementation prompt.

Do not commit `_analysis/` to the product repo unless the team wants it.

---

## What Cursor will copy out of the generated file

Every later implementation prompt needs these blocks filled from **repository evidence only**:

```text
REPOSITORY EVIDENCE:     path + class + method + current behavior
EXISTING PATTERN TO REUSE: exact extension point
SCOPE:                   files expected to change
DO NOT CHANGE:           files/behavior that must stay as-is
TESTS:                   existing test files to extend or run
```

The generated file must contain an **Implementation Prompt Seed** for each ticket below. Incomplete seed = not ready.

**Must be complete in this first pass (epic 136871):**

| Ticket | Seed must identify |
|---|---|
| 136873 | Where Begin PAF is rendered; sibling extension point; a11y pattern; dashboard tests |
| 136876 | Existing tooltip/info-icon pattern (or **Not found**) |
| 136878 | How PSG/Recruitment launch Enforce NPI Search; how Begin PAF launches its **different** search |
| 136883 | How Begin PAF is authorized (UI + route/API); direct-URL deny pattern |
| 136882 | Begin PAF click/search/validation files and tests that must not change |

**Must also be traced** (later tickets; still required in the same file):

Add New Practitioner search, PAF type/action registration, PAF tasks/cards, Add Practitioner to Facility, submit → processor → CACTUS, auto-accept/CPC/CVI, PDF/audit if on that path.

---

## Copy-paste this into office Copilot

```
You are analyzing the HCA HCP Credentialing repository (Hca.Credentialing).

ROLE:
Act as a senior engineer. Produce the REPOSITORY SOURCE OF TRUTH for existing PAF / practitioner-search / dashboard / authorization code. Another engineer (Cursor) will copy your findings into GitHub Copilot IMPLEMENTATION prompts. Your document must be complete enough that those prompts can name exact files, classes, methods, routes, and tests without guessing.

THIS DOCUMENT IS NOT:
- A business requirements spec
- An NPP design
- A proposal to rewrite PAF

THIS DOCUMENT IS:
- Verified current-state code flow
- Reuse / extension points
- Files that must not change
- Tests that lock current behavior
- Explicit Not found / Unknown where evidence is missing

TASK:
Create ONE markdown file. Analysis only.

OUTPUT FILE (only allowed write):
_analysis/HCP_Existing_PAF_NPP_Code_Flow.md

If you cannot write files, emit the FULL markdown in chat. Never a short summary. If too long, write the file to disk in one pass.

DO NOT:
- Implement Begin NPP PAF, ADD NPP, new buttons, routes, PAF types, or search
- Refactor production code
- Invent paths, types, APIs, tables, flags, roles, or routes
- Merge Begin PAF search with Enforce NPI Search if they are different
- Assume Packet modules are on the PAF path unless submit code calls them
- Treat folder names or comments as behavior — open the files
- Mark NPP as implemented without a citation
- Leave Implementation Prompt Seeds as TBD if you found the code — fill them
- Put inference in Repository Evidence fields

MODE:
Read-only except the one markdown file.

================================================================================
SEARCH TERMS (record hits with paths; if zero hits write Not found)
================================================================================

Dashboard / Begin PAF:
"Begin PAF", BeginPaf, BeginPAF, "MSP Dashboard", Dashboards

Search:
"Enforce NPI", EnforceNpi, "No NPI available", "Search without NPI",
PractitionerSearch, "Add New Practitioner", AddNewPractitioner

PAF framework:
PAFType, PafType, "PAF Type", PractitionerAction, "Add Practitioner to Facility",
AddPractitionerToFacility, "Add Practitioner"

NPP (expected often empty):
NPP, "Non-Privileged", "Non Privileged", "ADD NPP", AddNpp, BeginNpp, BeginNPP

Auth:
Authorize, AuthorizeView, policy, MSP, PSG, Recruitment, Roles, claims

Processing (trace only if on Add Practitioner to Facility or PAF submit path):
AutoAccept, Auto-Accept, CVI, CPC, CactusUpdater, paf-processing, PSV, Sanction

Look first (verify; do not assume internals):
- Hca.Credentialing/Client   Pages/Dashboards, Pages/Paf, Pages/PractitionerSearch, Pages/Queues, Security
- Hca.Credentialing/Shared   typed HTTP clients
- Hca.Credentialing.Paf.Api
- Hca.Credentialing.Paf.Processor
- Hca.Credentialing.Portal.Api
- Hca.Credentialing.Cactus.Api
- Hca.Credentialing.Api.Shared
- Hca.Credentialing.Document.Generation
- Matching *.Tests projects

IMPORTANT:
Hca.Credentialing.Tasks may be WASM MSBuild compression, NOT PAF workflow cards.
Find the REAL PAF task/card implementation (likely Paf.Api TaskController and/or Client Pages/Paf). State which.

================================================================================
RULES FOR EVERY FINDING
================================================================================

Use this block. Do not skip fields. Use "Not found" or "Unknown" instead of guessing.

### <Name>
- EvidenceClass: Repository Evidence
- File: <full repo-relative path>
- Class/Component:
- Method:
- Route or @page or API URL:  (or Not found)
- Auth (attribute/policy/claim/AuthorizeView):  (or Not found)
- CurrentBehavior:  (what the code does now)
- CallChain: `Ui.Method` → `Client.Method` → `Controller.Action` → `Service.Method`
- Tests: <test project / class / method>
- ReuseForNpp: Reuse | Extend | Configure | New | Cannot determine
- Why:
- FilesMustNotChange:  (if this is a regression boundary)
- PromptReady: yes | no
- PromptReadyGap:  (what Cursor still needs if no)

Small excerpts only (5–15 lines) when needed to prove behavior. Prefer path + method.

If two implementations exist, document BOTH and say which one Begin PAF / PSG actually use, with proof (who navigates there).

================================================================================
WRITE THE FILE WITH THIS EXACT STRUCTURE
================================================================================

# HCP Existing PAF / Search Code Flow — Repository Source of Truth

## 0. Document contract

State verbatim:
This file is the implementation source of truth for CURRENT HCP code related to PAF initiation, practitioner search, PAF actions/tasks, Add Practitioner to Facility, submit/processing, and authorization. Business rules remain in NPP V5. Ticket scope remains in Azure DevOps. Cursor may copy Implementation Prompt Seeds into Copilot prompts only when PromptReady is yes.

Include:
- AnalysisDate
- SolutionPath
- Branch
- Commit SHA if available
- Analyst: GitHub Copilot (office repo)

## 1. Prompt-readiness scoreboard

Table with every row below. Status = Ready | Partial | Not found

| Ticket or area | PromptReady | Extension point file | Tests identified | Blocking gap |
|---|---|---|---|---|
| 136873 Begin NPP PAF button | | | | |
| 136876 Tooltip | | | | |
| 136878 Route to Enforce NPI Search | | | | |
| 136883 MSP auth + direct URL | | | | |
| 136882 Preserve Begin PAF | | | | |
| 53658 / 127104 Search reuse | | | | |
| 132255 PAF action ADD NPP | | | | |
| 132275 PAF tasks | | | | |
| 131794 Add New Practitioner card | | | | |
| 132247 Existing practitioner | | | | |
| Add Practitioner to Facility baseline | | | | |
| Submit / processor / CACTUS | | | | |
| Auto-accept / CPC / CVI | | | | |

A 136871 implementation prompt is allowed only when rows 136873, 136878, 136883, 136882 are Ready (136876 Ready or Not found with a named fallback pattern).

## 2. End-to-end flows (real names only)

Three text diagrams using discovered class/route names, not generic words like "PAF API" unless that is the project name.

### 2.1 Begin PAF (MSP) — current
Dashboard → search → add/select practitioner → PAF action → tasks → submit → processor (only steps that exist)

### 2.2 Enforce NPI Search (PSG / Recruitment) — current
How they open it → search page → next step

### 2.3 Add Practitioner to Facility — current
From action selection through submit/downstream

If 2.1 search and 2.2 are different, add a one-line WARNING: Do not wire Begin NPP PAF to the Begin PAF search.

## 3. Implementation Prompt Seeds (CRITICAL — Cursor copies these)

For EACH seed, fill the fenced template with discovered values. Empty angle brackets are not allowed; use Not found.

### 3.1 Seed — 136873 Add Begin NPP PAF action (button only)

```
TICKET: 136873
REPOSITORY EVIDENCE:
- Begin PAF control File:
- Class/Component:
- Method / click handler:
- Button markup / a11y attributes (keyboard, aria/name):
- How dashboard actions are declared (markup vs config vs list):
- CurrentBehavior:

EXISTING PATTERN TO REUSE:
- Clone/extend this control as a sibling labeled "Begin NPP PAF"
- Extension point file:
- Extension point method/component:

SCOPE (files likely to change for a sibling button):
-

DO NOT CHANGE:
- Begin PAF click handler:
- Begin PAF navigation target:
- Begin PAF tests that must still pass:

TESTS:
- Existing dashboard/UI tests:
- Test project + pattern to copy:

PromptReady: yes/no
PromptReadyGap:
```

### 3.2 Seed — 136876 Tooltip

```
TICKET: 136876
REPOSITORY EVIDENCE:
- Existing tooltip or info-icon File:
- Component:
- Hover support:
- Keyboard focus support:
- CurrentBehavior:
- If Not found, closest tooltip in same Client project:

EXISTING PATTERN TO REUSE:
-

SCOPE:
-

DO NOT CHANGE:
-

TESTS:
-

PromptReady: yes/no
PromptReadyGap:
```

Approved tooltip text is owned by the ticket, not by code. Do not invent wording. Only document the UI pattern.

### 3.3 Seed — 136878 Launch existing Enforce NPI Search

```
TICKET: 136878
REPOSITORY EVIDENCE:
- Enforce NPI Search page File:
- Class/Component:
- @page / route / URL:
- How PSG currently navigates there (file + method):
- How Recruitment currently navigates there (file + method):
- Context/query/parameters passed:
- Search fields on THIS screen:
- API client + controller action for search:

BEGIN PAF SEARCH (must stay separate):
- File:
- Route:
- Search fields on THAT screen:
- Click handler from Begin PAF:
- Proof the two screens differ:

EXISTING PATTERN TO REUSE:
- Begin NPP PAF click should call the PSG/Recruitment launch path, not Begin PAF search
- Exact method/navigation helper to reuse:

SCOPE:
-

DO NOT CHANGE:
- Enforce NPI Search validation/business rules
- Begin PAF search

TESTS:
- Enforce NPI Search tests:
- Begin PAF search tests:

PromptReady: yes/no
PromptReadyGap:
```

### 3.4 Seed — 136883 Security (UI + direct URL)

```
TICKET: 136883
REPOSITORY EVIDENCE:
- Begin PAF UI visibility (AuthorizeView / role / policy) File + code:
- Begin PAF or dashboard route auth:
- Enforce NPI Search route auth:
- API auth for search/PAF create:
- Unauthorized behavior (redirect, 401, 403, access-denied page) File:
- Direct URL protection pattern used today:

EXISTING PATTERN TO REUSE:
- Same MSP model as Begin PAF; do not create new auth framework
- Claims/roles/policies FOUND IN CODE (names only if present):

SCOPE:
-

DO NOT CHANGE:
- PSG access to Enforce NPI Search
- Recruitment access to Enforce NPI Search
- Begin PAF authorization

TESTS:
- Auth tests, including direct URL if they exist:

PromptReady: yes/no
PromptReadyGap:
```

### 3.5 Seed — 136882 Preserve Begin PAF

```
TICKET: 136882
REPOSITORY EVIDENCE:
- Begin PAF click File/Method:
- Begin PAF search File/Route:
- Validation File/Method:
- Create/start PAF after search File/Method:

DO NOT CHANGE (regression boundary — list exact files):
-

TESTS THAT MUST KEEP PASSING:
-

PromptReady: yes/no
PromptReadyGap:
```

### 3.6 Seed — 53658 / 127104 Search reuse (later; still fill)

```
TICKET: 53658 / 127104
REPOSITORY EVIDENCE:
- Add New Practitioner search File/Class/Method:
- NPI validation File/Method (10-digit if present):
- No-NPI exception File/Method (or Not found):
- Duplicate practitioner check:
- Audit logging of searches:
- Shared search service vs duplicated UI:

EXISTING PATTERN TO REUSE:
-

DO NOT CHANGE:
- Begin PAF search unless it is the same component (prove it)

TESTS:
-

PromptReady: yes/no
PromptReadyGap:
```

### 3.7 Seed — PAF action / tasks / Add Practitioner to Facility (later)

```
AREA: PAF construction baseline
REPOSITORY EVIDENCE:
- PAF type/action registration File/Class:
- Add Practitioner to Facility identifier (enum/value/name in code):
- ADD NPP / NPP action: Found | Not found
- Create PAF API: controller/action/route
- Task/card registration File:
- Task requiredness/visibility File:
- Review & Submit gating File:
- Demographics / address / specialty task files:
- Add New Practitioner card File:
- Existing practitioner card File:

EXISTING PATTERN TO REUSE:
- Add Practitioner to Facility is the comparison baseline

DO NOT CHANGE:
- Other PAF types' action/task registration unless shared config requires a new row/value

TESTS:
-

PromptReady: yes/no
PromptReadyGap:
```

### 3.8 Seed — Submit / processor / CACTUS / auto-accept / CVI

```
AREA: Processing baseline
REPOSITORY EVIDENCE:
- Submit endpoint:
- Queue name (only if referenced in code):
- Processor entry:
- CACTUS update method:
- Packet handoff condition (quote the if-condition or Not on this path):
- Auto-accept:
- CPC:
- CVI:
- PDF:
- Audit:

Reuse vs New vs Cannot determine for ADD NPP: do not design NPP processing. Only record current Add Practitioner to Facility / generic PAF submit path.

PromptReady: yes/no
PromptReadyGap:
```

## 4. Detailed traces (support the seeds)

Expand each seed with the finding block format. Minimum depth:

4.1 MSP Dashboard + Begin PAF control (markup, a11y, handler, route)
4.2 Begin PAF search (fields, validation, APIs)
4.3 Enforce NPI Search (entry, fields, validation, APIs, no-NPI)
4.4 Add New Practitioner
4.5 Authorization (MSP / PSG / Recruitment) UI and server
4.6 Tooltip/info-icon patterns in Client
4.7 PAF type/action/create
4.8 PAF tasks/cards including Review & Submit
4.9 Add Practitioner to Facility full path
4.10 Submit → processor → CACTUS / packet / PDF
4.11 Existing NPP strings (or none)
4.12 Test map: Test project | Class | Method | Behavior locked | Production file

## 5. Reuse catalog

Table required by later design:

| Area | File | Class | Method | Current behavior | Reuse/Extend/Configure/New/Cannot determine | Why | Regression risk | Prompt seed section |

## 6. Conflicts and stop conditions

List anything that would block an implementation prompt:
- File not found
- Multiple candidates, winner unclear
- Begin PAF search vs Enforce NPI Search not distinguished
- Auth mechanism unclear
- Tests missing
- Code vs comments disagree

## 7. Appendix — file index

Every production and test file opened:
`path | class | methods | seed it supports`

================================================================================
QUALITY GATE (fail the document if any is false)
================================================================================

[ ] Every Prompt Seed for 136873, 136878, 136882 has real File paths or explicit Not found
[ ] Begin PAF search and Enforce NPI Search are compared with routes AND field lists
[ ] Call chains use real method names
[ ] Tests are named, not "add tests later"
[ ] No invented identifiers
[ ] Inference never labeled as Repository Evidence
[ ] File written to _analysis/HCP_Existing_PAF_NPP_Code_Flow.md

START:
1. Search all terms.
2. Open matches. Trace UI → client → API → processor where applicable.
3. Fill scoreboard + ALL Implementation Prompt Seeds first (section 1 and 3). Then write detailed traces.
4. Return in chat: output path, PromptReady scoreboard, and blocking gaps only.
```

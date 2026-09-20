# Copilot Task Prompts — 136871 Begin NPP PAF Entry Point

Office-laptop playbook. Copy **one task** into GitHub Copilot Chat in the HCA HCP repository. Do not paste the whole file at once.

Before these implementation tasks, run the analysis prompt so Copilot documents existing PAF/search code: [Copilot_Existing_PAF_NPP_Code_Flow_Prompt.md](Copilot_Existing_PAF_NPP_Code_Flow_Prompt.md).

This slice is **dashboard entry only**. It does **not** implement ADD NPP PAF type, tasks, VA/PA, license, PSV, CACTUS, CVI, or search-rule changes (`53658` / `127104`). Those wait until this epic is completed and deployed.

```text
MSP Dashboard
      |
      +------------------+
      |                  |
      v                  v
 Begin PAF        Begin NPP PAF
      |                  |
      v                  v
Existing Flow     Existing Enforce NPI Search
(unchanged)       (PSG / Recruitment workflow — reuse)
```

## How to use

1. Open the HCP repo on the **office laptop**.
2. Run **Task 0** first. Copilot must find existing source. Do not skip it.
3. Paste Task 0 output into the **REPOSITORY EVIDENCE** block of the next task before you run that task.
4. Run Tasks 1 → 5 in order. One Copilot chat per task.
5. After each task, copy Copilot’s RETURN block back to Cursor for review.
6. If Copilot cannot find an existing pattern, **stop**. Do not let it invent a new architecture.

## Hard rules for every prompt

- Reuse existing HCP patterns. Prefer clone/extend over new components.
- Do not invent file paths, class names, routes, APIs, feature flags, or roles. Discover them in the repo.
- Do not duplicate Enforce NPI Search.
- Do not change Begin PAF click behavior, search fields, or validations.
- Do not start `53658` or `127104` in these tasks.
- Match existing styling, DI, validation, logging, authorization, and tests.
- If a search string below finds nothing, say so and stop. Do not guess.

## Known investigation starting points

These are folders/names already seen on this project. They are **not** proof of internal files.

- UI: look first in Blazor/client under `Hca.Credentialing` / `Hca.Credentialing.Client`
- Portal APIs: `Hca.Credentialing.Portal.Api`
- PAF APIs: `Hca.Credentialing.Paf.Api` (not required for button-only Task 1)
- Tests: matching `*.Tests` projects; follow existing UI/component test style

Search strings Copilot should use:

```text
"Begin PAF"
BeginPaf
BeginPAF
"MSP Dashboard"
"Enforce NPI"
"No NPI available"
"Search without NPI"
```

---

# Evidence sheet (fill after Task 0)

Paste Copilot’s findings here, then reuse them in later prompts.

```text
### A. Begin PAF (dashboard)
File:
Class/Component:
Method / click handler:
Button markup / a11y attributes:
Authorization / visibility:
UI tests:
Current behavior:

### B. Begin PAF search (must NOT be used by NPP)
File / route:
How it differs from Enforce NPI Search:

### C. Enforce NPI Search (PSG / Recruitment)
File:
Class/Component:
Route / URL:
How PSG/Recruitment navigate there today:
Context / query params passed in:
Authorization:
Tests:
Current behavior:

### D. Tooltip / info-icon pattern on dashboard
File:
Component:
Hover + keyboard focus behavior:
Tests:

### E. Existing tests that must keep passing
Begin PAF dashboard:
Begin PAF search / validation:
Enforce NPI Search:
Authorization / direct URL:
```

---

# TASK 0 — Discover existing implementation (no code changes)

Copy everything inside the fence.

```
You are inspecting the HCA HCP Credentialing repository.

ROLE:
Act as a senior engineer. Discovery only. Do not modify any files.

TASK:
Find the existing implementations that ADD NPP Begin NPP PAF must reuse.

BUSINESS REQUIREMENT:
Epic 136871 / Feature 136872: MSP Dashboard needs a second action labeled "Begin NPP PAF" adjacent to existing "Begin PAF". Begin NPP PAF must route into the existing Enforce NPI Search workflow already used by PSG and Recruitment. Existing Begin PAF must remain unchanged. Tooltip and MSP authorization will be added in later tasks.

SCOPE:
Read-only search. No code changes. No new files.

SEARCH FOR:
1. Dashboard control that renders the "Begin PAF" button/action.
2. Click handler / navigation used by Begin PAF.
3. Practitioner search launched by Begin PAF (expected: NPI + SSN + First Name + Last Name — different from Enforce NPI Search).
4. Enforce NPI Search used by PSG and Recruitment (NPI required; likely includes "No NPI available" / "Search without NPI").
5. How those roles currently open Enforce NPI Search (menu, route, button).
6. Existing tooltip or info-icon pattern on the MSP Dashboard or similar actions.
7. Authorization that shows/hides Begin PAF and that protects Enforce NPI Search (UI + direct URL).
8. Existing tests for the above.

INVESTIGATION STARTING POINTS (folders only, not proven files):
- Hca.Credentialing.Client / Hca.Credentialing UI
- Hca.Credentialing.Portal.Api
- Matching *.Tests projects

DO NOT:
- Invent file paths if search finds nothing.
- Treat Begin PAF search as Enforce NPI Search.
- Propose a new search workflow.
- Implement anything.

BEFORE RESPONDING:
1. Search the repo with: "Begin PAF", BeginPaf, BeginPAF, "Enforce NPI", "No NPI available", "Search without NPI", "MSP Dashboard".
2. Open the matching files and read the actual click/route/auth code.
3. Quote real paths, class/component names, and methods.

RETURN exactly this structure:

## Discovery result
### A. Begin PAF dashboard
File:
Class/Component:
Method:
Button markup / a11y:
Authorization / visibility:
Tests:
Current behavior:
Reusable for Begin NPP PAF? (yes/no + why)

### B. Begin PAF search (do not reuse for NPP)
File / route:
Class/Component:
Current behavior:
Proof it is different from Enforce NPI Search:

### C. Enforce NPI Search (reuse target)
File:
Class/Component:
Route / URL:
How PSG/Recruitment launch it:
Context passed in:
Authorization:
Tests:
Current behavior:
Reusable for Begin NPP PAF click? (yes/no + why)

### D. Tooltip / info-icon
File:
Component:
Hover and keyboard-focus support:
Tests:
Reusable? (yes/no + why)

### E. Tests that must keep passing
- list real test files

### F. Recommended extension point for Task 1 (136873)
File to change:
Why this is the smallest safe change:
Files that must not change:

### G. Unknowns
- anything not found
```

**After Task 0:** paste the RETURN block into the Evidence sheet above. Bring the same block to Cursor if you want it reviewed before coding.

---

# TASK 1 — 136873 Add Begin NPP PAF action (button only)

Prerequisite: Task 0 complete. Paste discovery into REPOSITORY EVIDENCE.

This task is **display, layout, accessibility**. Do not implement tooltip text, routing, or new security here.

```
You are implementing a change in the HCA HCP Credentialing repository.

ROLE:
Act as a senior engineer familiar with the existing repository patterns.

TASK:
136873 — Add a new dashboard action/button labeled "Begin NPP PAF" immediately adjacent to the existing "Begin PAF" control.

BUSINESS REQUIREMENT:
As an MSP user I want a dedicated Begin NPP PAF option so that I can initiate requests for Non-Privileged Practitioners.

Acceptance criteria:
- On MSP Dashboard page load, both actions are displayed: Begin PAF and Begin NPP PAF.
- Begin NPP PAF appears immediately adjacent to Begin PAF.
- The new control is keyboard accessible, screen reader compatible, and styled consistently with existing application controls.

This story does NOT define click routing (136878), tooltip text (136876), or authorization (136883).

REPOSITORY EVIDENCE:
<PASTE TASK 0 SECTION A AND F HERE>

EXISTING PATTERN TO REUSE:
Reuse the existing Begin PAF dashboard control (same component type, styling, and a11y attributes). Add a sibling action. Do not create a new dashboard framework.

SCOPE:
Only the MSP Dashboard UI that currently renders Begin PAF, plus matching UI tests.

DO NOT CHANGE:
- Begin PAF label, layout meaning, click handler, search, or validations.
- Enforce NPI Search internals.
- PAF types, tasks, CACTUS, CPC, CVI, reporting.
- Unrelated dashboard widgets.

IMPLEMENTATION REQUIREMENTS:
1. Reuse the existing repository pattern.
2. Preserve existing behavior for other PAF types.
3. Do not create duplicate services/components.
4. Follow existing dependency injection.
5. Follow existing validation.
6. Follow existing logging/error handling.
7. Follow existing test conventions.
8. Do not invent business rules.
9. Do not modify unrelated code.
10. Button label must be exactly: Begin NPP PAF
11. If the existing control requires a click handler, do not point Begin NPP PAF at the Begin PAF handler. Leave navigation for 136878, or use a no-op that does not launch Begin PAF.

BEFORE CODING:
1. Inspect the referenced Begin PAF implementation.
2. Confirm the extension point.
3. Explain the planned changes in 3–5 bullets.
4. Then implement.

IMPLEMENTATION:
- Add Begin NPP PAF immediately adjacent to Begin PAF.
- Copy styling and accessibility attributes from Begin PAF (role/name, keyboard focus, aria as used today).
- Do not restyle Begin PAF.

TESTS:
- Dashboard shows Begin PAF and Begin NPP PAF.
- Begin NPP PAF is adjacent to Begin PAF.
- New control is keyboard reachable and has an accessible name "Begin NPP PAF" (or the same accessible-name pattern Begin PAF uses).
- Existing Begin PAF UI tests still pass.

VALIDATION:
- build
- relevant tests
- regression tests for Begin PAF dashboard
- inspect changed files
- inspect diff

RETURN:
1. Files changed
2. Classes/methods changed
3. Requirement implemented
4. Tests
5. Build/test results
6. Remaining gaps
7. Any assumptions
```

**Bring back to Cursor:** files changed, diff summary, test results.

---

# TASK 2 — 136876 Tooltip (approved text only)

Prerequisite: Task 1 button exists. Paste Task 0 section D plus the new button file from Task 1.

```
You are implementing a change in the HCA HCP Credentialing repository.

ROLE:
Act as a senior engineer familiar with the existing repository patterns.

TASK:
136876 — Display an informational tooltip when the user hovers over (and focuses) Begin NPP PAF.

BUSINESS REQUIREMENT:
As an MSP user I want guidance regarding when to use Begin NPP PAF so that I select the correct workflow.

Approved tooltip text (exact; do not rewrite):
Use this option only when submitting a request for a Non-Privileged Practitioner (NPP).

Acceptance criteria:
- Tooltip appears when the user hovers over Begin NPP PAF.
- Tooltip contains the approved instructional text above.
- Tooltip disappears when hover/focus ends.

Do NOT add this extra mockup sentence unless it already exists as approved product text (it is not in the ticket):
Selecting this option will launch the NPI Search workflow.

REPOSITORY EVIDENCE:
<PASTE TASK 0 SECTION D AND THE TASK 1 BUTTON FILE HERE>

EXISTING PATTERN TO REUSE:
Reuse the existing dashboard tooltip / info-icon pattern if one exists. If the dashboard has no tooltip, reuse the closest existing tooltip component in the same UI project. Do not add a new tooltip library.

SCOPE:
Begin NPP PAF control + existing tooltip pattern + matching UI tests.

DO NOT CHANGE:
- Begin PAF.
- Tooltip text wording.
- Routing or search.
- Unrelated components.

IMPLEMENTATION REQUIREMENTS:
1. Reuse the existing repository pattern.
2. Preserve existing behavior for other PAF types.
3. Do not create duplicate services/components.
4. Follow existing dependency injection.
5. Follow existing validation.
6. Follow existing logging/error handling.
7. Follow existing test conventions.
8. Do not invent business rules.
9. Do not modify unrelated code.
10. Use the approved sentence exactly, including capitalization and "(NPP)".

BEFORE CODING:
1. Inspect the existing tooltip / info-icon implementation.
2. Confirm how hover AND keyboard focus are handled today.
3. Explain the planned changes in 3–5 bullets.
4. Then implement.

IMPLEMENTATION:
- Attach tooltip to Begin NPP PAF using the existing pattern (mockup shows an info icon; follow existing HCP pattern if it uses ⓘ or title/tooltip on the button).
- Show on hover and focus; hide when hover/focus ends.
- Accessible name/description must not break the button’s screen-reader name from 136873.

TESTS:
- Hover shows approved text only.
- Focus shows approved text.
- Dismiss on hover/focus end.
- Text equals the approved sentence exactly.
- Begin PAF still has no new tooltip unless it already had one.

VALIDATION:
- build
- relevant tests
- regression tests
- inspect changed files
- inspect diff

RETURN:
1. Files changed
2. Classes/methods changed
3. Requirement implemented
4. Tests
5. Build/test results
6. Remaining gaps
7. Any assumptions
```

---

# TASK 3 — 136878 Route Begin NPP PAF to existing Enforce NPI Search

Prerequisite: Tasks 0–1. Paste Task 0 sections B, C, and F.

```
You are implementing a change in the HCA HCP Credentialing repository.

ROLE:
Act as a senior engineer familiar with the existing repository patterns.

TASK:
136878 — Selecting Begin NPP PAF must navigate into the existing Enforce NPI Search workflow used by PSG and Recruitment. Reuse it. Do not create a second search workflow.

BUSINESS REQUIREMENT:
As an MSP user I want Begin NPP PAF to launch the NPI Search workflow so that I can search using NPI for Non-Privileged Practitioner requests.

Acceptance criteria:
- Selecting Begin NPP PAF displays the Enforce NPI Search screen.
- Existing business rules, validations, and search functionality are reused.
- No duplicate workflow is introduced.
- Users can continue through the existing NPI Search process without modification.

Critical distinction:
- Begin PAF search (NPI + SSN + name) must stay on Begin PAF only.
- Begin NPP PAF must NOT open that Begin PAF search screen.

Do not change NPI validation rules, no-NPI exception behavior, or Add New Practitioner search. Those belong to later tickets 53658 / 127104 / 131785.

REPOSITORY EVIDENCE:
<PASTE TASK 0 SECTIONS B, C, AND THE TASK 1 BUTTON HANDLER HERE>

EXISTING PATTERN TO REUSE:
The existing Enforce NPI Search launch path used by PSG/Recruitment (route, component, navigation helper). Pass only the same kind of context that existing callers already pass, plus any existing PAF-type/context parameter if the repo already has that pattern. If no PAF-type parameter exists, do not invent one; report it as a gap.

SCOPE:
Begin NPP PAF click/navigation only. Existing Enforce NPI Search internals should not be rewritten.

DO NOT CHANGE:
- Begin PAF click handler or Begin PAF search.
- Enforce NPI Search business rules / UI fields / validation.
- Duplicate a new NPI search page.
- 53658 / 127104 search-rule work.

IMPLEMENTATION REQUIREMENTS:
1. Reuse the existing repository pattern.
2. Preserve existing behavior for other PAF types.
3. Do not create duplicate services/components.
4. Follow existing dependency injection.
5. Follow existing validation.
6. Follow existing logging/error handling.
7. Follow existing test conventions.
8. Do not invent business rules.
9. Do not modify unrelated code.

BEFORE CODING:
1. Inspect how PSG/Recruitment currently open Enforce NPI Search.
2. Inspect how Begin PAF opens its different search.
3. Confirm Begin NPP PAF will call the PSG/Recruitment path, not the Begin PAF path.
4. Explain the planned changes in 3–5 bullets.
5. Then implement.

IMPLEMENTATION:
- Wire Begin NPP PAF to the existing Enforce NPI Search navigation.
- Keep Begin PAF pointed at its current workflow.
- Do not copy search components.

TESTS:
- Begin NPP PAF click shows Enforce NPI Search (same screen PSG/Recruitment use).
- Begin PAF click still shows the original Begin PAF search.
- No new search service/component added.
- Existing Enforce NPI Search tests still pass.
- Existing Begin PAF tests still pass.

VALIDATION:
- build
- relevant tests
- regression tests
- inspect changed files
- inspect diff

RETURN:
1. Files changed
2. Classes/methods changed
3. Requirement implemented
4. Tests
5. Build/test results
6. Remaining gaps (especially any missing NPP PAF-type context)
7. Any assumptions
```

---

# TASK 4 — 136883 Security and authorization (UI + direct URL)

Prerequisite: Task 3 route exists. Paste Task 0 authorization findings.

```
You are implementing a change in the HCA HCP Credentialing repository.

ROLE:
Act as a senior engineer familiar with the existing repository patterns.

TASK:
136883 — Only authorized MSP users may access Begin NPP PAF. Unauthorized users must be blocked in the UI and by direct URL. Reuse the existing MSP security model. Do not affect PSG or Recruitment access to Enforce NPI Search.

BUSINESS REQUIREMENT:
As a System Administrator I want only authorized MSP users to access the Begin NPP PAF workflow so that unauthorized users cannot initiate NPP requests.

Acceptance criteria:
- Authorized MSP users can access Begin NPP PAF.
- Unauthorized users cannot access the workflow through the UI or direct URL.
- Existing PSG and Recruitment access is unaffected.
- Security and session management remain consistent with current application standards.

Hiding the button is not enough. Direct URL must also be blocked.

Unauthorized result should follow the existing access-denied pattern in this repo. Do not invent a new HTTP status, error page, or auth framework.

REPOSITORY EVIDENCE:
<PASTE TASK 0 AUTHORIZATION FOR BEGIN PAF AND ENFORCE NPI SEARCH, PLUS THE TASK 3 ROUTE/URL HERE>

EXISTING PATTERN TO REUSE:
Whatever currently gates Begin PAF for MSP (policy, role, claim, Authorize attribute, route guard, menu visibility). Extend that same mechanism to Begin NPP PAF. Keep existing Enforce NPI Search authorization for PSG/Recruitment intact.

SCOPE:
Visibility of Begin NPP PAF + authorization on the navigation/route it uses for MSP. Minimal change.

DO NOT CHANGE:
- Begin PAF authorization.
- PSG/Recruitment ability to open Enforce NPI Search the way they do today.
- A new RBAC platform (project ticket 131728 is out of scope).
- Session management redesign.

IMPLEMENTATION REQUIREMENTS:
1. Reuse the existing repository pattern.
2. Preserve existing behavior for other PAF types.
3. Do not create duplicate services/components.
4. Follow existing dependency injection.
5. Follow existing validation.
6. Follow existing logging/error handling.
7. Follow existing test conventions.
8. Do not invent business rules.
9. Do not modify unrelated code.
10. Do not invent role/claim names. Use names already in the repo.

BEFORE CODING:
1. Inspect MSP authorization for Begin PAF.
2. Inspect Enforce NPI Search authorization for PSG/Recruitment.
3. Confirm how unauthorized UI vs direct URL is tested today.
4. Explain the planned changes in 3–5 bullets.
5. Then implement.

IMPLEMENTATION:
- Show Begin NPP PAF only to authorized MSP users, using the existing visibility pattern.
- Block unauthorized direct navigation to the MSP Begin NPP PAF → Enforce NPI Search path.
- Do not remove or tighten PSG/Recruitment access.

TESTS:
- Authorized MSP: button visible; click reaches Enforce NPI Search.
- Unauthorized: button not available.
- Unauthorized direct URL: access denied using existing pattern.
- PSG Enforce NPI Search still works.
- Recruitment Enforce NPI Search still works.
- Begin PAF security unchanged.

VALIDATION:
- build
- relevant tests
- security regression tests
- inspect changed files
- inspect diff

RETURN:
1. Files changed
2. Classes/methods changed
3. Roles/claims/policies used (only those found in repo)
4. Tests
5. Build/test results
6. Remaining gaps
7. Any assumptions
```

---

# TASK 5 — 136882 Preserve Begin PAF (regression; no feature work)

Prerequisite: Tasks 1–4. This task should mostly be tests and verification. Change product code only if a regression was introduced.

```
You are implementing a change in the HCA HCP Credentialing repository.

ROLE:
Act as a senior engineer familiar with the existing repository patterns.

TASK:
136882 — Prove existing Begin PAF is unchanged after adding Begin NPP PAF. Fix only regressions if tests or inspection show Begin PAF was altered.

BUSINESS REQUIREMENT:
As an MSP user I want the existing Begin PAF process to remain unchanged so that standard practitioner requests continue to function normally.

Acceptance criteria:
- Selecting Begin PAF still launches the current Begin PAF workflow.
- Existing practitioner search options remain unchanged (NPI, SSN, First Name, Last Name as they work today).
- Existing validations remain unchanged.
- Regression testing confirms no unintended changes.

REPOSITORY EVIDENCE:
<PASTE TASK 0 SECTIONS A, B, E AND THE DIFF FROM TASKS 1–4 HERE>

EXISTING PATTERN TO REUSE:
Existing Begin PAF tests. Add tests only if coverage is missing, using the same test project and style.

SCOPE:
Regression verification of Begin PAF dashboard, search, and validation. Product-code edits only to restore Begin PAF if it was broken.

DO NOT CHANGE:
- Begin PAF behavior "to improve" it.
- Enforce NPI Search.
- NPP button/tooltip/route/security except to undo accidental Begin PAF breakage.

IMPLEMENTATION REQUIREMENTS:
1. Reuse the existing repository pattern.
2. Preserve existing behavior for other PAF types.
3. Do not create duplicate services/components.
4. Follow existing dependency injection.
5. Follow existing validation.
6. Follow existing logging/error handling.
7. Follow existing test conventions.
8. Do not invent business rules.
9. Do not modify unrelated code.

BEFORE CODING:
1. Diff Begin PAF files against the pre-NPP behavior.
2. List any Begin PAF files that changed in Tasks 1–4 and justify each line.
3. Run existing Begin PAF tests.
4. Only then add missing regression tests or revert accidental edits.

IMPLEMENTATION:
- Keep Begin PAF click → original search.
- Keep original search fields and validations.
- Add/adjust tests if needed to lock this behavior.

TESTS:
- Begin PAF still present and launches original workflow.
- Begin PAF search options unchanged.
- Begin PAF validations unchanged.
- Begin NPP PAF still present and still goes to Enforce NPI Search (do not break Tasks 1–4).

VALIDATION:
- build
- relevant tests
- regression tests
- inspect changed files
- inspect diff (Begin PAF files should be unchanged or change-justified)

RETURN:
1. Files changed
2. Classes/methods changed
3. Requirement implemented
4. Tests
5. Build/test results
6. Remaining gaps
7. Any assumptions
```

---

# Feature-level check (after Tasks 0–5)

Use this only as a manual QA list. Do not paste into Copilot unless you need a gap report.

| 136872 AC | Task that owns it | Pass? |
|---|---|---|
| Begin NPP PAF available on MSP Dashboard | 136873 | |
| Existing Begin PAF unchanged | 136882 | |
| Begin NPP PAF routes to existing Enforce NPI Search | 136878 | |
| Hover tooltip displayed | 136876 | |
| Existing security model maintained | 136883 | |
| Regression: no impact to current functionality | 136882 | |

Layout/a11y (adjacent, keyboard, screen reader, styling) is 136873.

---

# What to paste back to Cursor after each task

```text
Ticket:
Task:
Files changed:
Classes/methods:
Diff summary:
Tests run + result:
Build result:
Gaps / assumptions:
Did Copilot invent any new file/architecture? (yes/no)
```

---

# Do not prompt yet

Do **not** use this file for:

- `53658` NPP: PAF - Search
- `127104` Add New Practitioner search reuse
- `131785` No-NPI exception
- PAF action / tasks / CACTUS / CVI / reporting

`53658` states epic **136871 must be completed and deployed** before that search feature is developed, tested, or deployed.

When this epic is done, ask Cursor for the next Copilot prompt pack (`53658` / `127104`) using Task 0 evidence from Enforce NPI Search.

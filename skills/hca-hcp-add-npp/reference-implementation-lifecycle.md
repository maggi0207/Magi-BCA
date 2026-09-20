# ADD NPP Implementation Lifecycle

Companion to [SKILL.md](SKILL.md). Phases, status table, testing, and Copilot prompt contents.

53658 states epic **136871 must be completed and deployed** before that search feature is developed, tested, or deployed. Do not start `53658` / `127104` implementation until that gate is satisfied unless the user explicitly overrides it.

## Phase 0 — Existing Architecture Discovery

No code changes.

Identify existing HCP implementations for: Begin PAF, Enforce NPI Search, Practitioner Search, Add New Practitioner, Add Practitioner to Facility, PAF creation, PAF Tasks, practitioner information cards, authorization/security, CACTUS, auto-acceptance, CPC/CVI, PDF/history, audit, reporting.

For each finding, only after office evidence:

```text
File:
Class/Component:
Method:
Current Behavior:
Relevant Ticket:
Relevant Requirement:
Reusable:
Why:
```

## Phase 1 — Practitioner Search Foundation

Tickets: `53658`, `127104`, `131785` (title-only until captured).

Reuse existing search. Do not duplicate search component, NPI validation, search service, exception workflow, or audit unless repository evidence proves reuse is impossible.

## Phase 2 — Begin NPP PAF Entry Point

Tickets: `136871`, `136873`, `136876`, `136878`, `136883`, `136882`.

`136872` is the **feature** that contains those stories. Preserve Begin PAF is **`136882`**, not `136872`.

Target: MSP Dashboard → Begin NPP PAF → existing Enforce NPI Search → NPP Search. Existing Begin PAF unchanged.

## Phase 3 — PAF Construction

Tickets: `132255`, `132275`, `131794`, `132247` (title-only until captured).

Reuse existing PAF/task/card architecture.

## Phase 4 — ADD NPP Business Workflow

From NPP V5, not from invented rules: Practitioner Type = NPP → VA → PA where applicable → State License → PSV → Submit → Auto-Accept or CPC/CVI where required.

## Phase 5 — Auto Acceptance / CPC / CVI

Tickets: `132591`, `132537` (title-only until captured). Do not assume every exception creates a CVI.

## Phase 6 — CACTUS

Tickets: `132600`, `131724` (title-only until captured). Trace HCP → PAF → validation → auto-accept → CACTUS (provider, entity, address, specialty, PPI, license, NPI image, sanctions, audit).

## Phase 7 — Security / CPC / Reporting

Tickets: `131728`, `131783`, `132687` (title-only until captured).

## Requirement traceability

Requirement → Ticket → Existing implementation → File → Class → Method → Current behavior → Required behavior → Gap → Implementation → Test.

Status: Implemented / Partially Implemented / Not Implemented / Different Behavior / Cannot Determine.

## Copilot prompt must contain

Ticket ID/title. Exact business requirement. Verified existing implementation (path, class, method, service, API). Reuse point. Required change only. Files expected to change. Files that should not change. Acceptance criteria. Tests. Constraints: no duplicate logic, no unrelated redesign, preserve existing workflows.

## Implementation status tracking

| Area | Ticket | Requirement | Existing Code | Gap | Implementation | Tests | Status |
|---|---|---|---|---|---|---|---|
| Entry Point | 136871 | | | | | | |
| Search | 53658 | | | | | | |
| Add Practitioner Search | 127104 | | | | | | |
| No NPI | 131785 | | | | | | |
| PAF Action | 132255 | | | | | | |
| PAF Tasks | 132275 | | | | | | |
| Practitioner Card | 131794 | | | | | | |
| Existing Practitioner | 132247 | | | | | | |
| Auto Accept | 132591 | | | | | | |
| CVI | 132537 | | | | | | |
| CACTUS | 132600 | | | | | | |
| Security | 131728 | | | | | | |
| CPC | 131783 | | | | | | |
| Reporting | 132687 | | | | | | |

Do not mark Implemented without repository evidence.

## Testing

Positive: valid NPI, valid ADD NPP submit, valid license/PSV, normal auto-accept.

Negative: invalid NPI, missing data, invalid license, missing PSV, duplicate license/practitioner.

Exception: no NPI, VA/PA routing, non-active/expired license, CPC, CVI where required.

Regression: Begin PAF, practitioner search, Add Practitioner to Facility, other PAF types, CPC workflows.

## Definition of Ready

Ticket understood. Parent/child known. NPP V5 mapped. Existing implementation inspected. Code paths verified. Reuse identified. Changes defined. Tests identified. Regression understood.

## Definition of Done

Relevant 52350 children implemented or explicitly dispositioned. V5 mapped and verified. Add Practitioner to Facility architecture respected. Search, PAF/tasks, demographics/address/specialty, VA/PA, license/PSV, auto-accept/CPC/CVI, CACTUS, PDF/history, audit, reporting verified. Tests and regression pass.

## Recommended order

Phase 0 discovery (no code). Then 1 Search (respect 53658 gate on 136871). Then 2 Entry Point. Then 3 PAF Construction. Then 4 Processing. Then 5 Supporting functions. Adjust after repository analysis.

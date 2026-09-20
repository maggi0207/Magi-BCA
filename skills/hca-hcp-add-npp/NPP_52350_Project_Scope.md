# 52350 — Project - Add NPP to existing PAF Types

## What 52350 is

`52350` is the Azure DevOps **parent/project** work item for the ADD NPP initiative.

It is **project scope**, not a single implementation ticket.

Do not implement ADD NPP by treating `52350` as one coding task. Work is decomposed into epics, features, and user stories under this project.

## Distinctions

| Level | Meaning | Example |
|---|---|---|
| Project | Overall ADD NPP initiative | `52350` |
| Epic / Feature | A delivery slice | `136871` MSP dashboard entry; `53658` PAF Search |
| User Story | A bounded implementation item | `136873` dashboard button; `127104` practitioner search reuse |

Azure DevOps tickets are **implementation/decomposition evidence**.

The **business source of truth** remains:

- HCP NPP PAF Type Requirements V5
- [01_Requirements/ADD_NPP_Analysis.md](01_Requirements/ADD_NPP_Analysis.md) as working guide (not a replacement for V5)
- [SKILL.md](SKILL.md)

## Relationship to 136871

`136871` — NPP: Add Begin NPP PAF Entry Point to MSP Dashboard

- Captured as a child of `52350`.
- Covers the MSP Dashboard **Begin NPP PAF** entry point.
- Captured child feature: `136872`.
- Captured successor: `53658`.

Stories `136873`, `136876`, `136878`, `136882`, and `136883` were captured as **children of feature 136872**, not as direct children of `52350`. They belong to the `136871` entry-point slice.

## Relationship to 53658

`53658` — NPP: PAF - Search

- Captured as a **Successor** of `136871` (not merely another child of `136871`).
- Also appears on the `52350` project child list from the Azure DevOps screenshot.
- Ticket `53658` states **136871 must be completed and deployed** before this feature is developed, tested, or deployed.
- Captured child: `127104`.

## ADD NPP is decomposed across multiple areas

| Area | Known tickets (title only unless captured) |
|---|---|
| MSP dashboard entry point | `136871`, `136872`, `136873`, `136876`, `136878`, `136882`, `136883` |
| Practitioner search | `53658`, `127104` |
| No-NPI exception | `131785` (title only) |
| PAF action / type selection | `132255` (feature captured; child AC pending); `131783` (title only) |
| PAF tasks | `132275` (title only) |
| Existing practitioner handling | `132247` (title only) |
| Add New Practitioner card | `131794` (title only) |
| Security / RBAC | `131728` (title only); entry-point story `136883` is captured |
| CACTUS integration | `131724`, `132600` (title only) |
| Auto-acceptance | `132591` (title only) |
| CVI | `132537` (title only) |
| Reporting | `132687` (title only) |

Do not invent acceptance criteria for uncaptured tickets.

## Ticket inventory

Titles for uncaptured `52350` children are taken **only** from the Azure DevOps screenshot/reference list. Truncation (`...`) is preserved. Work item type is **Not captured** unless a ticket file already recorded it.

| Ticket | Title | Work Item Type | Parent/Relationship | Captured? | Notes |
|---|---|---|---|---|---|
| 52350 | Project - Add NPP to existing PAF Types | Project | — | Partial | Scope/hierarchy only |
| 136871 | NPP: Add Begin NPP PAF Entry Point to MSP Dashboard | Epic | Child of 52350 | Yes | Description captured; AC not in export |
| 136872 | NPP: Add "Begin NPP PAF" Entry Point on MSP Dashboard | Feature | Child of 136871 | Yes | Feature AC captured |
| 136873 | NPP: Add Begin NPP PAF action to MSP Dashboard | User Story | Child of 136872 | Yes | Button/layout/a11y |
| 136876 | NPP: Provide user guidance through tooltip | User Story | Child of 136872 | Yes | Approved tooltip text |
| 136878 | NPP: Launch existing Enforce NPI Search workflow | User Story | Child of 136872 | Yes | Routing/reuse |
| 136882 | NPP: Preserve existing Begin PAF functionality | User Story | Child of 136872 | Yes | Regression boundary |
| 136883 | NPP: Validate security and authorization for Begin NPP PAF | User Story | Child of 136872 | Yes | UI + direct URL |
| 144239 | NPP: Add "Begin NPP PAF" Entry Point on MSP | User Story | Related to 136872 | No | Related; no AC export |
| 53658 | NPP: PAF - Search | Feature | Successor of 136871; listed under 52350 | Yes | Blocked on 136871 |
| 127104 | NPP: Add NPP to Facility PAF Practitioner Search – Add New Practitioner | User Story | Child of 53658 | Yes | Reuse existing search |
| 131724 | NPP: Cactus Enable Maintenance and Management of New Credentialing Values for Non-... | Not captured | Child of 52350 | No | Title only. **ADO State: Done** (52350 Links screenshot 2026-09-20) |
| 131728 | NPP: Enable HCP Access and Role-Based Security for Non-Defined Practitioner (NPP) ... | Not captured | Listed under 52350 | No | Title only |
| 132600 | NPP: PAF to Cactus Credentialing Record Creation & Synchronization | Not captured | Listed under 52350 | No | Title only |
| 131783 | NPP: Add NPP to Facility PAF Type Selection and Filtering in CPC Dashboard | Not captured | Listed under 52350 | No | Title only |
| 132591 | NPP: Auto-Acceptance and Automated Processing | Not captured | Listed under 52350 | No | Title only |
| 131785 | NPP: Controlled "No NPI Available" Exception Workflow for NPP | Not captured | Listed under 52350 | No | Title only; related to 53658/127104/V5 |
| 132255 | NPP: Create "ADD NPP to Facility" PAF Action Option | Feature | Child of 52350 | Yes | Feature + all four child AC captured |
| 132256 | NPP: Display New PAF Action for MSP User - "ADD NPP to Facility" | User Story | Child of 132255 | Yes | MSP-only radio; last in Practitioner Action list |
| 132257 | NPP: Restrict Action to Eligible Practitioners | User Story | Child of 132255 | Yes | SHOW net new or existing inactive at entity; HIDE active / other |
| 132258 | NPP: Apply Existing PAF Task Determination Rules | User Story | Child of 132255 | Yes | Reuse New vs Existing determination; no NPP engine |
| 132259 | NPP: Enforce Single PAF Action Selection | User Story | Child of 132255 | Yes | Radio group + client/server single-select; reject multi-select |
| 132537 | NPP: CVI Creation | Not captured | Listed under 52350 | No | Title only |
| 131794 | NPP: Modify Add New Practitioner Card for ADD NPP to Facility PAF | Not captured | Listed under 52350 | No | Title only |
| 132687 | NPP: Reports | Not captured | Listed under 52350 | No | Title only |
| 132247 | NPP: Verify Practitioner Information Card – Existing Practitioner | Not captured | Listed under 52350 | No | Title only |
| 132275 | PAF Tasks | Not captured | Listed under 52350 | No | Title only |

## Structured ticket files

Captured tickets also have a 9-section summary under [tickets/](tickets/).

Detailed captures remain in `02_Epics`, `03_Features`, `04_User_Stories`, `05_Related`, and `06_Successors`.

## Screenshots

Treat as **UI/reference evidence**, not proof of final NPP implementation.

- [screenshots/Begin_PAF_Current](screenshots/Begin_PAF_Current) — current MSP Begin PAF
- [screenshots/NPP_Search](screenshots/NPP_Search) — existing NPI-required search + PSG chat + NPP dashboard mockup
- [screenshots/PAF_Action](screenshots/PAF_Action) — PAF Action & Facilities dialog (`132255`)
- Canonical copies also live in [10_Screenshots](10_Screenshots)

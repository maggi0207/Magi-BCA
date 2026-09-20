# ADD NPP — Master Implementation Plan

> **Status:** Control document. No application code was modified to produce this file.
> **Role:** Primary planning document for implementing ADD NPP. Supersedes `09_Cursor_Analysis/Implementation_Plan.md` (stale), `09_Cursor_Analysis/Architecture_Map.md` (stale), `09_Cursor_Analysis/Requirement_Traceability.md` (stale), and the Phase 1→2 order in `reference-implementation-lifecycle.md` / `ADD_NPP_PROGRESS_TRACKER.md` where those conflict with ticket `53658`.
> **Rule:** This document validates the gap analysis. It does **not** silently rewrite it. Disagreements are labeled. Unconfirmed items are **UNKNOWN / NEEDS OFFICE EVIDENCE**.
> **Next process step after this file:** **not** a Copilot implementation prompt. Gather the §9 evidence first. Do not generate an implementation prompt until 136873 is READY.
> **Revision (entry point):** Prior close-out treated 136873 as the next Copilot implementation (inspect-then-code in one session). That is **incorrect**. Shared `PafQueueSearchCriteria` / unread `SharedPafQueue` / dual MSP+PSG dashboard route make 136873 **not independently implementable**. See §9.

**Sources used (actual filenames in this workspace):**

| # | Requested as | Actual file used |
|---|---|---|
| 1 | `08_Office_Repository_Evidence/ADD_NPP_CODE_LEVEL_ARCHITECTURE.md` | [`08_Office_Repository_Evidence/ADD NPP — Code-Level Architecture .md`](../08_Office_Repository_Evidence/ADD%20NPP%20—%20Code-Level%20Architecture%20.md) |
| 2 | `04_GAP_ANALYSIS/ADD_NPP_EXISTING_VS_REQUIRED_GAP.md` | [`04_GAP_ANALYSIS/ADD NPP — Existing vs Required Gap Analysis.md`](../04_GAP_ANALYSIS/ADD%20NPP%20—%20Existing%20vs%20Required%20Gap%20Analysis.md) |
| 3 | `01_Requirements/HCP_NPP_PAF_Type_Requirements_V5.md` | Pointer only. Curated business text used: [`source-documents/HCP_NPP_PAF_Type_Requirements_V5.md`](../source-documents/HCP_NPP_PAF_Type_Requirements_V5.md) |
| 4 | Project scope | [`NPP_52350_Project_Scope.md`](../NPP_52350_Project_Scope.md) |
| 5 | Ticket index | [`00_Master/NPP_Ticket_Index.md`](../00_Master/NPP_Ticket_Index.md) |
| 6 | Cursor analysis | [`09_Cursor_Analysis/`](./) — Implementation_Plan, Architecture_Map, Requirement_Traceability, Copilot templates (stale except templates) |
| 7 | Lifecycle | [`reference-implementation-lifecycle.md`](../reference-implementation-lifecycle.md) |

Also used: [`00_Master/NPP_Dependency_Map.md`](../00_Master/NPP_Dependency_Map.md), captured ticket files under `tickets/`, and V5 conflicts recorded in the curated requirements file.

**Evidence taxonomy (unchanged):** Ticket Evidence · Requirements Evidence · UI Evidence · Repository Evidence · Inference · Unknown.

**Readiness values:**

| Value | Meaning |
|---|---|
| **READY** | Captured AC, exact files verified, independently implementable, and regression-safe. |
| **PARTIAL** | Files and gap known, but unresolved evidence changes *how* the change must be written. |
| **BLOCKED** | Missing evidence, missing AC, or a dependency that makes a safe independent implementation impossible. |
| **NOT AN IMPLEMENTATION UNIT** | Epic/feature container, or a regression/test constraint. Not a Copilot coding ticket. |

No ticket in the current evidence set is **READY**. **NO IMPLEMENTATION TASK IS READY.** The first *candidate* after missing evidence is still **136873**, but it cannot be implemented or tested independently with regression safety until shared-component callers are known. See **§9 Entry-point sequence (corrected)** and §12.

---

# 0. How This Document Is Used

```text
This master plan
      ↓
Cursor selects ONE ticket (see §5 sequence)
      ↓
Cursor writes ONE focused Copilot prompt (not in this document)
      ↓
Office Copilot inspects the named files and implements that ticket only
      ↓
User returns diff / build / tests / errors
      ↓
Cursor reviews against requirement, ticket, architecture, gap, regression, security, data/CACTUS, audit
      ↓
This plan + progress tracker are updated
      ↓
Next ticket
```

Copilot must treat the code-level architecture and the gap analysis as the repository knowledge base. It must not rediscover the whole repository. This plan is the **control** layer: what is validated, what is wrong or uncertain in the gap analysis, what depends on what, and which ticket is next.

---

# 1. Gap Analysis Validation

The gap analysis is a strong repository comparison. It is **not** automatically correct. The following ledger is the critical review. Nothing below is a silent correction of the gap file.

## 1.1 Validation of the nine named major gaps

### Gap 1 — `TaskType.Npp` reserved and unwired

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §7 requires a required task card titled **ADD NPP to Facility**. Ticket `132275` / `132255` are title-only. |
| Ticket relationship | **Ticket Evidence (title only).** `132275` “PAF Tasks”; `132255` “Create ADD NPP to Facility PAF Action Option.” No captured AC. |
| Repository evidence | **Fact.** Architecture + gap both report `TaskType.Npp` with `[Description("NPP PAF")]` in `Shared/Data/Model/Paf/TaskType.cs`, and zero wiring (no strategy, no `TaskDeterminationService` case, no UI, no `PafExtensions.GetCredentialingTypeRtk` case). |
| Fact vs inference | **Fact:** enum value exists and is unwired. **Inference (not proven):** that `TaskType.Npp` *is* the V5 “ADD NPP to Facility” task. The enum description is `"NPP PAF"`, not `"ADD NPP to Facility"`. The existing analog for facility association is `TaskType.AddPractitionerToFacility`. |
| Unresolved evidence | **UNKNOWN / NEEDS OFFICE EVIDENCE:** confirm `TaskType.Npp` was reserved for this initiative and is the intended task key. Confirm `PafExtensions.GetCredentialingTypeRtk` throw at the cited line. |
| Dependencies | Correctly upstream of most workflow/CACTUS work **if** NPP is a new `TaskType`. That “if” is not settled (see Gap 7). |
| Validation verdict | **Accepted as a real repository gap. Not accepted as a proven design decision.** Do not start wiring `TaskType.Npp` until AC exists and the PafType-vs-TaskType decision is explicit. |

### Gap 2 — No auto-acceptance mechanism

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §13: normal ADD NPP is auto-accepted; PAF goes to Completed. Ticket `132591` title-only. |
| Ticket relationship | Title only. Do not invent AC. |
| Repository evidence | **Fact.** Architecture full-text search: no `AutoAccept`, no `*Rule*.cs`. Acceptance today is human `CpcAction` / `CpcReviewAction` before `SubmitPafStrategy` → `paf-processing` → `CactusUpdater`. |
| Fact vs inference | **Fact:** no auto-accept engine. **Inference:** “new branch in `SubmitPafStrategy.InternalExecuteAsync()`” is a candidate, not a confirmed extension point. |
| Unresolved evidence | **UNKNOWN / NEEDS OFFICE EVIDENCE:** product resolution of V5 conflicts (below). Full AC for `132591`. |
| Dependencies | Correctly depends on being able to compute normal vs CPC-exception. Also depends on Gap 1 only if the submit path keys off `TaskType.Npp`. |
| Validation verdict | **Accepted.** Also **understated:** V5 itself conflicts on whether VA+PA=YES auto-accepts. That is a product blocker, not only a missing engine. |

**V5 conflicts the gap analysis mentions incompletely:**

1. Early V5 assumption: “CVIs will not be created for ADD NPP” vs later CVI Creation section (CVI **is** required on CPC route).
2. VA + PA = YES: one V5 paragraph says PAF **will auto accept** then create CVI; Workflow 1 says PAF **will not auto accept**; CVI Creation says on CPC-route submission the PAF **shall be automatically accepted**.
3. Do not implement both. Prefer the detailed CVI / Auto-Acceptance sections only after product confirms.

### Gap 3 — No standalone License PAF task; no CA/LA/NV/KY/TX state-match

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §12 license + state-match. No distinct captured ticket. Implied by `132255` / `132275` titles only. |
| Repository evidence | **Fact.** License UI is inside Recruitment PSV only. Five-state match not found as a generic submit block. |
| Fact vs inference | **Fact:** no standalone Manage License task. **Inference:** lift Recruitment PSV license subcomponents into an NPP task. |
| Unresolved evidence | Duplicate-license check beyond the 50-item cap: architecture already marked this a Repository Evidence Gap. |
| Validation verdict | **Accepted as a repository gap.** Ticket ownership is **title-only / implied** — do not treat `132275` as license AC. |

### Gap 4 — No VA / Active Duty / PA concept

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §12. No distinct ticket. |
| Repository evidence | **Fact.** No `ActiveDuty` / `VaPa` / `IsVa` / `IsPa` model. Unrelated `NPP - Active/Inactive/Suspend` comments in Portal.Api are CACTUS entity-assignment status labels, not this question tree. |
| Validation verdict | **Accepted.** Net-new UI + model + submit routing. Depends on an NPP task model existing (Gap 1 decision). |

### Gap 5 — No Facility Undefined Practitioner CVI type / UDP statuses

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §13. Ticket `132537` title-only. |
| Repository evidence | **Fact.** No `FacilityUndefinedPractitioner` / `UdpFacilityRequest` strings. `CommonCactusRtks` has other credentialing-type RTKs. `CrudEntityCredentialingOperation` due-date switch is a real extension pattern. |
| Unresolved evidence | **UNKNOWN / NEEDS OFFICE EVIDENCE:** actual CACTUS `CREDENTIALINGTYPE_RTK` / `CREDENTIALINGSTATUS_RTK` values. **Do not invent RTKs.** |
| Validation verdict | **Accepted. Implementation of CVI type constants is BLOCKED until office/CACTUS reference data is supplied.** Due-date “+3 business days” also has no existing “business day” helper confirmed in the architecture pass. |

### Gap 6 — No general PAF-type / CPC-routing reporting

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §15. Ticket `132687` title-only. |
| Repository evidence | **Fact.** Only `RecruitmentReportGeneration.cs`, hardcoded `IndexedTaskSelected = 13`. |
| Unresolved evidence | Whether MOR exclusion is an HCP job, a CACTUS script, or an external report. V5 also asks for a credentialing-database script to auto-close completed UDP CVIs — that may not be an HCP ticket. |
| Validation verdict | **Accepted as “no reusable general report pipeline.”** Closest analog is Recruitment reporting, not a proven extension point. |

### Gap 7 — `PafType` has no ADD NPP value

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §1 / §6: CPC wants a new **PAF type** “ADD NPP to Facility”; MSP selects that action. Ticket `132255` title-only. |
| Repository evidence | **Fact.** `PafType` has only `ManageNewPractitioner` / `ManageExistingPractitioner`. Switched in multiple files listed by the gap analysis. |
| Fact vs inference | **Inference (gap analysis itself marks Cannot Determine):** follow `AddPractitionerToFacility` and distinguish by `TaskType` only, and do **not** add a `PafType`. That is the stronger **repository analog**, but V5 language is “PAF type/action.” Ticket `131783` title is “PAF Type Selection and Filtering in CPC Dashboard,” which leans toward a visible type, not only a task. |
| Validation verdict | **Accepted as an unresolved design fork, not as “no PafType needed.”** Settling this is a prerequisite to Gap 1 wiring, PDF title, queue filters, and reporting indexes. |

### Gap 8 — `LicenseStatus` missing Temporary Permit / Active Military / Active-Compact

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §12 lists **CACTUS `STATUS_RTK`** values: Active `DRFB0IQ54S`, Temporary Permit `D2H11CCT3V`, Active Military `D3UK1E0UF6`, Active-Compact `M5890K2GGY`. |
| Repository evidence | Gap reports HCP Packet `LicenseStatus` enum (`Active`, `Expired`, `Pending`, `Invalid`, `Inactive`) from usage sites. **Defining file was not opened.** |
| Fact vs inference | **Possible mis-layering.** V5 statuses are CACTUS RTKs. They may not belong on the Packet `LicenseStatus` enum at all. Extending that shared enum is a high-regression inference, not a proven need. |
| Unresolved evidence | **UNKNOWN / NEEDS OFFICE EVIDENCE:** open the `LicenseStatus` definition; determine whether NPP license status is a CACTUS lookup (`STATUS_RTK`) bound on the NPP task, not a Packet enum change. |
| Validation verdict | **Accepted that those three labels are not in the Packet enum usage set. Not accepted that the Packet enum must be extended.** |

### Gap 9 — PPI has zero code representation

| Check | Finding |
|---|---|
| Requirement | **Requirements Evidence.** V5 §11: professional practice interest/focus, 500 chars, primary specialty **OR** PPI. CACTUS: Specialty = APP-Other, status Not Certified, `SPECIALTYSTATUS_RTK` `DS9Q12M4UO`. |
| Repository evidence | **Fact.** Literal `"PPI"` not found. |
| Unresolved evidence | **UNKNOWN / NEEDS OFFICE EVIDENCE:** APP-Other specialty RTK and `DS9Q12M4UO` in CACTUS reference data. Do not invent. |
| Validation verdict | **Accepted.** |

## 1.2 Comparison-table rows that are wrong, overstated, or conflated

These are **not** silent edits to the gap file. They are control-document corrections.

| Gap row | Gap-analysis claim | Validation |
|---|---|---|
| #3 / #6 | Begin PAF already navigates to `PractitionerSearchPage` / `PractitionerSearchForm`, and that form **is** Enforce NPI Search | **Overstated.** Repository Evidence: `NavigateToStartPaf()` → `PageUri.PractitionerSearchUri` (`/Practitioner/Search`) and `PractitionerSearchForm` has `SearchMode.NpiSearch` / `ExceptionSearch`. **UI Evidence (tickets 136878, 136882):** Begin PAF search is NPI + SSN + name; Enforce NPI Search is NPI-required with “No NPI available?”. Those are two different screens in screenshots. **UNKNOWN / NEEDS OFFICE EVIDENCE:** whether `PractitionerSearchForm` is role/mode-conditional; whether MSP Begin PAF uses a different mode on the same page; whether a second route or query/state is required so Begin NPP PAF can open Enforce NPI Search **without** changing Begin PAF. |
| #12 | `TaskType.Npp` is the ADD NPP task (partially implemented as enum placeholder) | **Inference.** Enum exists. Identity with V5 task title is unproven. |
| #13 | “ADD NPP task required for Net-New; required for Existing only if inactive” | **Incorrect relative to V5.** V5 §7: ADD NPP task **Required = Always, regardless of practitioner type**. V5 §6: the **action** is available only for net-new or existing inactive-with-entity. V5 §7 table: for Existing, Demographic / Addresses / Specialties are **optional** (specialty becomes required if no active primary). The gap row conflates **action eligibility**, **ADD NPP task requiredness**, and **other-card requiredness**. |
| #4 | 136882 “Implemented (as a constraint)” | **Accepted as current-state only.** The ticket is not done. It becomes a regression gate the moment #1 is implemented. |
| #5 / 136883 | Same destination page/role gate as Begin PAF | **Partial.** Pages are role-gated. Ticket 136883: hide is not enough; **direct URL** must be blocked; PSG/Recruitment access to Enforce NPI Search must stay. If Enforce NPI Search is already reachable by Recruiter/PSG on the same URL, an NPP-only URL gate may be impossible without a distinct route or claim. **UNKNOWN / NEEDS OFFICE EVIDENCE.** |
| #12 Review & Submit | Task `IsRequired` gates Review & Submit | **Understated mismatch.** Architecture: Review & Submit is **not** gated by “all required complete”; `PafReviewForm.IsSubmittable()` uses lock/status/role; validation runs **on click**. V5: Review & Submit **unavailable** while required items incomplete. That is **Different Behavior**, not only “set `IsRequired`.” |
| #18 / #26 | Every `Crud*Operation` needs a `TaskType.Npp` branch | **Inference** until Gap 1 / Gap 7 are decided. If NPP reuses `TaskType.AddPractitionerToFacility` with a flag, the branch shape changes. |
| Missing major gap | Delegate card and Facility Specific Questions must be **suppressed** for ADD NPP (V5 §7) | **Omitted as a numbered gap.** `AddTaskAddPractitionerToFacilityStrategy` initializes FacilityQuestions and conditionally PerfTask. Suppression is a real requirement and a regression risk if the Add-Practitioner-to-Facility path is reused carelessly. |
| Missing major gap | V5: **do not create credentialing packets**; ADD NPP is not packet determination | **Omitted as a numbered gap.** Architecture: `CactusUpdateResult.ShouldGeneratePacket` can enqueue `packet-processing`. ADD NPP submit/processor work must not turn packet generation on. |
| #21 / Gap 8 | Extend `LicenseStatus` | See §1.1 Gap 8 — possible wrong layer. |
| 136873 readiness YES | “None blocking — purely additive UI” | **Overstated.** See §1.3 and §4. `PafQueueSearchCriteria` is a **shared** component (MSP “Begin PAF” and Recruitment “Begin Request”). `CanBeginPaf` source (`SharedPafQueue.razor.cs`) was not opened. Adding a button there can leak Begin NPP PAF into Recruitment UI. |

## 1.3 Entry-point evidence the gap analysis treated as settled

**Fact (Repository Evidence)**

- MSP Dashboard: `Client/Pages/Dashboards/MspDashboardPage.razor` — `[Authorize(Roles = "Msp, Psg")]`.
- Begin PAF button lives in `Client/Pages/Queues/Components/PafQueueSearchCriteria.razor`, gated by `CanBeginPaf`, click → `NavigateToStartPaf()` → `PageUri.PractitionerSearchUri`.
- No “Begin NPP PAF” control exists.

**Unknown / NEEDS OFFICE EVIDENCE**

1. `SharedPafQueue.razor.cs` (or other caller): how `CanBeginPaf` is computed and passed.
2. Whether `PafQueueSearchCriteria` is used on Recruitment queues (“Begin Request”) and how to show Begin NPP PAF **only** on MSP Dashboard.
3. How NPP intent is carried after click (none / query / `StateContainer` flag / distinct URI). Ticket `136873` does **not** require this; `136878` and `136883` do.
4. Whether MSP Begin PAF and PSG/Recruitment Enforce NPI Search are the same component in different modes or two different UIs that happen to share a route family.
5. Existing tooltip / info-icon pattern for dashboard buttons (needed by `136876`).
6. `PractitionerPafPage` `[Authorize(Roles=...)]` (architecture listed this as unread).

## 1.4 Stale Cursor analysis files (do not use as evidence)

| File | Status | Instruction |
|---|---|---|
| `09_Cursor_Analysis/Implementation_Plan.md` | “Not started”; waits for a file name that was never produced (`HCP_Existing_PAF_NPP_Code_Flow.md`) | Superseded by this document. |
| `09_Cursor_Analysis/Architecture_Map.md` | “Not analyzed” | Superseded by the code-level architecture file. |
| `09_Cursor_Analysis/Requirement_Traceability.md` | All rows “Not provided” | Superseded by gap analysis + this validation. |
| `reference-implementation-lifecycle.md` Phase 1 Search → Phase 2 Entry | Conflicts with ticket `53658` | **Ticket Evidence wins:** `136871` must be completed and **deployed** before `53658` / `127104` development, testing, or deployment. |
| `ADD_NPP_PROGRESS_TRACKER.md` same Phase 1→2 order | Same conflict | Use §5 of this document as the sequence. |

## 1.5 What the gap analysis got right (keep)

- `TaskType.Npp` is unwired. `PafExtensions.GetCredentialingTypeRtk` is reported to throw on unhandled `TaskType`.
- No auto-accept engine exists.
- No VA/PA, no PPI string, no UDP CVI type constants.
- License UI is Recruitment-PSV-scoped.
- Enforce NPI Search **mechanism** (`SearchMode`, `DoNpiValidation`, `NpiAttribute`) exists and is the reuse target named by `53658` / `127104` / `136878`.
- Add New Practitioner form is practitioner-type-agnostic.
- CACTUS writes go through `Paf.Processor` / `CactusUpdater` / `Crud*Operation`, not `Cactus.Api` HTTP writes.
- No Blazor client test project; no `Hca.Credentialing.Paf.Api.Tests`.
- Title-only `52350` children must not be given invented AC.
- `131728` AC must not be substituted with `136883` AC.
- Exception-search fields: V5 vs ticket `127104` reuse is a recorded **Conflict**.
- `144239` is Related, not a second implementation of `136872`.

---

# 2. Implementation Streams

Organize work by capability, not by a single ticket. Title-only tickets are listed as **title-only**; V5 is used for the business rule, not as fake AC.

Legend for readiness: the stream’s **current** implementation readiness, not a wish.

## Stream summary

| Stream | Readiness | Why |
|---|---|---|
| A. Begin NPP PAF | **PARTIAL** | Captured tickets; shared-component / `CanBeginPaf` / search-mode identity unresolved. |
| B. Practitioner Search / No-NPI | **PARTIAL** (and **ticket-blocked** until `136871` deployed) | Search reuse confirmed; exception fields + exception audit call site unresolved; `53658` gate. |
| C. PAF initialization / type | **BLOCKED** | `PafType` vs `TaskType` fork; `132255` title-only. |
| D. Task determination | **BLOCKED** | Depends on C; `ITaskDeterminationService` was later read in the gap pass, but NPP branch does not exist; `132275` title-only. |
| E. Practitioner information | **BLOCKED** | `131794` / `132247` title-only; mechanism exists. |
| F. Address | **BLOCKED** | V5 rules exist; no distinct captured ticket; depends on D. |
| G. Specialty / PPI | **BLOCKED** | PPI net-new; APP-Other / `DS9Q12M4UO` not in repo. |
| H. ADD NPP to Facility | **BLOCKED** | Depends on C/D; Delegate/FSQ suppression not designed. |
| I. VA / Active Duty / PA | **BLOCKED** | No model; no ticket; V5 auto-accept conflict. |
| J. License | **BLOCKED** | Wrong-layer risk on `LicenseStatus`; no standalone task. |
| K. PSV | **PARTIAL** (pipeline) / **BLOCKED** (NPP wiring) | Upload pipeline reusable; NPP task wiring missing; which `ConvertPafAttachment` is live is unknown. |
| L. Submit / validation / routing | **BLOCKED** | Depends on I/J; Review & Submit behavior mismatch. |
| M. Auto-acceptance | **BLOCKED** | No engine; V5 conflict; `132591` title-only. |
| N. CPC | **BLOCKED** | `131783` title-only; trigger conditions do not exist. |
| O. CVI | **BLOCKED** | RTKs unknown; `132537` title-only. |
| P. CACTUS synchronization | **BLOCKED** | Depends on C + office RTKs; `132600` / `131724` title-only. |
| Q. PDF / History | **BLOCKED** | Depends on type identity + CVI-vs-provider attachment rule. |
| R. Audit | **PARTIAL** | Generic pipeline exists; exception-search and “HCA Corporate” identity untraced. |
| S. Reporting | **BLOCKED** | `132687` title-only; no general pipeline. |
| T. Security | **PARTIAL** (entry) / **BLOCKED** (131728) | `136883` captured but URL/claim model unresolved; `131728` title-only. |
| U. Regression / testing | **PARTIAL** | Constraint is known; no client/PAF.Api tests exist to lock it. |

## A. Begin NPP PAF

| Item | Content |
|---|---|
| Requirements | **Ticket (not in V5):** MSP Dashboard shows **Begin NPP PAF** adjacent to **Begin PAF**; approved tooltip; click launches existing Enforce NPI Search; Begin PAF unchanged; MSP auth including direct URL; PSG/Recruitment unaffected. V5 alignment: MSP-only submitters. |
| Tickets | Epic `136871` → Feature `136872` → Stories `136873`, `136876`, `136878`, `136882`, `136883`. Related `144239` (do not implement as a second entry). Successor `53658` is **not** this stream. |
| Existing implementation | One Begin PAF / Begin Request button in `PafQueueSearchCriteria`. Navigation to `/Practitioner/Search`. |
| Exact files | `Client/Pages/Dashboards/MspDashboardPage.razor`; `Client/Pages/Queues/Components/PafQueueSearchCriteria.razor` + `.razor.cs` (`NavigateToStartPaf`); `Client/Pages/Shared/PageUri.cs`; **not opened:** `Client/Pages/Queues/Components/SharedPafQueue.razor.cs`. |
| Gap | No second button, tooltip, NPP click path, or NPP-specific auth boundary. |
| Dependency | None from TaskType/CACTUS. Internal: button (`136873`) before tooltip (`136876`) and before routing (`136878`); routing before URL auth (`136883`); `136882` is a continuous regression constraint. |
| Implementation readiness | **PARTIAL** |
| Testing impact | No Blazor test project (Repository Evidence Gap). Manual + existing Begin PAF screenshot path. High regression if the shared criteria component is edited globally. |

## B. Practitioner Search / No-NPI

| Item | Content |
|---|---|
| Requirements | **Both:** NPI primary, valid 10-digit NPI. **Ticket:** reuse existing Enforce NPI Search / Add New Practitioner search; no new NPI logic; exception searches audit logged; Begin PAF search unchanged. **V5 (conflict):** exception fields Reason / First / Last / State License Number / State of License. **UI Evidence:** existing help text is Last Name + one of First / DOB / SSN / Email. |
| Tickets | Feature `53658` (blocked on `136871` deployed). Child `127104`. Title-only `131785`. |
| Existing implementation | `PractitionerSearchForm` (`OnNpiFieldChanged`, `SearchMode`); `ValidationRoutines.DoNpiValidation`; `NpiAttribute`; `PractitionerSearchTable.LoadPractitionersAsync` / `StartPafFrom`; `PractitionerDuplicateCheckHelper`; `AddNewPractitionerForm` / `ConfirmationWizardModal`. |
| Exact files | `Client/Pages/PractitionerSearch/Components/PractitionerSearchForm.razor` + `.cs`; `PractitionerSearchTable.razor.cs`; `Shared/Validations/ValidationRoutines.cs`; `Shared/Validations/Npi.cs`; `Shared/Helpers/PractitionerDuplicateCheckHelper.cs`; ConfirmationWizard files. |
| Gap | Exception **field list** not read in the architecture pass. Exception **audit** call site not traced. V5 vs ticket field **Conflict** unresolved. |
| Dependency | **Ticket gate:** `136871` completed and deployed. Technical: stream A must exist so “Begin NPP PAF → this search” is testable. Do not change Begin PAF search (`136882`). |
| Implementation readiness | **PARTIAL** and **must not start** until the `53658` gate is satisfied (unless the user explicitly overrides). |
| Testing impact | No unit tests found for NPI validation or search form. Regression: Begin PAF search (different UI per screenshots) and PSG/Recruitment search. |

## C. PAF initialization / type

| Item | Content |
|---|---|
| Requirements | **V5:** new PAF type/action ADD NPP to Facility; MSP only; one action at a time; available only net-new or existing inactive with entity; placed below last available PAF type. |
| Tickets | `132255` title-only. CPC filter `131783` title-only (also stream N). |
| Existing implementation | `CreatePafRequest.Type` = `PafType.ManageNewPractitioner` \| `ManageExistingPractitioner`. `PractitionerPafPage.OnParametersSetAsync` → `IPafClient.CreateNewPafAsync`. |
| Exact files | `Shared/Data/Model/Paf/Form/PafType.cs`; `CreatePafRequest`; `Client/Pages/Paf/PractitionerPafPage.razor.cs`; `Shared/Clients/Paf/PafClient.cs`; `Hca.Credentialing.Paf.Api/Controllers/PafController.cs`; switch sites listed in gap Gap 7. |
| Gap | No ADD NPP `PafType`. Design fork: new `PafType` vs `TaskType` only. |
| Dependency | Stream B produces the practitioner used to create the PAF. Must be decided before D/H/Q/S. |
| Implementation readiness | **BLOCKED** (no AC; design fork). |
| Testing impact | High if a `PafType` value is added (7+ switch sites, some without `default`). |

## D. Task determination

| Item | Content |
|---|---|
| Requirements | **V5 §7:** ADD NPP card always required; net-new required cards = Demographic, Addresses, Specialties, ADD NPP; existing required = ADD NPP only; suppress Delegate and Facility Specific Questions; Review & Submit unavailable while required incomplete. |
| Tickets | `132275` title-only. |
| Existing implementation | `TaskDeterminationService.GetAvailableTaskListAsync` (read in gap pass) branches New / Existing / Recruitment / PSG. `TaskType.Npp` has no case. `BasePractitionerActionFormTask.IsRequired`. Strategy factory `PafProcessingStrategyContext`. |
| Exact files | `Hca.Credentialing.Paf.Api/Services/TaskDeterminationService.cs`; `Hca.Credentialing.Paf.Api/Controllers/TaskController.cs`; `Hca.Credentialing.Paf.Api/Domain/Strategy/PafProcessingStrategyContext.cs`; `Shared/Data/Model/Paf/TaskType.cs`; `Client/Pages/Paf/Components/PafTaskSelection.razor.cs`; `PafTaskDisplay.razor.cs`. |
| Gap | No NPP task list / requiredness / suppression. Review & Submit UX does not match V5. |
| Dependency | Stream C decision. Upstream of E–L. |
| Implementation readiness | **BLOCKED** |
| Testing impact | No Paf.Api test project. High — task list is shared across PAF types. |

## E. Practitioner information

| Item | Content |
|---|---|
| Requirements | **V5 §8–9:** net-new required First/Last/Degree/Category/NPI (10 digits starting with 1 or 20 on the **card**, not invented as a search rule); Email/Cell/DOB/SSN/Gender optional. Existing: email read-only if present, editable if blank; CACTUS prepopulate listed fields. |
| Tickets | `131794`, `132247` title-only. |
| Existing implementation | `ManagePractitionerInformationTask.OnInitializedAsync` role/mode required flags; `PractitionerDemographicTask`; `Practitioner.IsNew`. |
| Exact files | `Client/Pages/Paf/Components/Tasks/ManagePractitionerInformation/ManagePractitionerInformationTask.razor.cs`; `Shared/Data/Model/Paf/Form/PractitionerDemographicTask.cs`; `AddNewPractitionerForm.razor.cs`. |
| Gap | No NPP-specific required-flag branch. `131794` modification unknown without AC. |
| Dependency | D. |
| Implementation readiness | **BLOCKED** |
| Testing impact | Medium. Must not change privileged Add-New-Practitioner requiredness. |

## F. Address

| Item | Content |
|---|---|
| Requirements | **V5 §10:** hide Home / Credentialing / Alternate; primary only; affiliation-based edit lock; suppress group-address note; field lengths/formats. |
| Tickets | No distinct captured ticket. Implied by `132255` / `132275`. |
| Existing implementation | `ManageAddressesTask` always manages four groups. |
| Exact files | `Client/Pages/Paf/Components/Tasks/ManageAddresses/ManageAddressesTask.razor.cs` and listed subcomponents; `Shared/Data/Model/Paf/Form/AddressTask.cs`. |
| Gap | No hide-three-groups mode. |
| Dependency | D. |
| Implementation readiness | **BLOCKED** (no ticket AC; depends on D). |
| Testing impact | Medium. Do not hide groups for other PAF types. |

## G. Specialty / PPI

| Item | Content |
|---|---|
| Requirements | **V5 §11:** required net-new; optional existing unless no/inactive primary; hide secondary/alternate; PPI 500-char alternate; CACTUS APP-Other / `DS9Q12M4UO`. |
| Tickets | Implied by `132255` / `132275`. |
| Existing implementation | `ManageSpecialtiesTask` hard-requires Primary; shows Secondary/Alternate; no PPI. |
| Exact files | `ManageSpecialtiesTask.razor.cs`; `SpecialtyTask.cs`; `CrudPractitionerSpecialtiesOperation.cs`. |
| Gap | Suppression + PPI + CACTUS mapping. |
| Dependency | D. CACTUS RTKs from office. |
| Implementation readiness | **BLOCKED** |
| Testing impact | High if CACTUS write mapping changes. |

## H. ADD NPP to Facility

| Item | Content |
|---|---|
| Requirements | **V5 §7 / §12:** task title, description, Work action, Practitioner Type = NPP only, facility association, disclaimer copy, suppress Delegate/FSQ. |
| Tickets | `132255`, `132275` title-only. |
| Existing implementation | `AddPractitionerToFacilitiesTask` + `AddTaskAddPractitionerToFacilityStrategy` + CACTUS ops keyed on `TaskType.AddPractitionerToFacility`. |
| Exact files | `Client/Pages/Paf/Components/Tasks/AddPractitionerToFacilities/AddPractitionerToFacilitiesTask.razor.cs`; `AddTaskAddPractitionerToFacilityStrategy.cs`; listed `Crud*Operation` files. |
| Gap | No NPP task UI. Reuse-vs-new-component not decided. Delegate/FSQ suppression not designed. |
| Dependency | C, D. Upstream of I–K as host card. |
| Implementation readiness | **BLOCKED** |
| Testing impact | High. This is the Add-Practitioner-to-Facility analog; regression on that task type is the main risk. |

## I. VA / Active Duty / PA

| Item | Content |
|---|---|
| Requirements | **V5 §12** question tree and routing. |
| Tickets | None captured. |
| Existing implementation | None. |
| Exact files | None today. Candidate host is the future NPP task model/UI (Gap 1). `SubmitPafStrategy.cs` for routing. |
| Gap | Entire tree. |
| Dependency | H (host). Upstream of J (license visibility) and L/M/O (routing). |
| Implementation readiness | **BLOCKED** |
| Testing impact | High. Product conflict on VA+PA=YES auto-accept must be resolved first. |

## J. License

| Item | Content |
|---|---|
| Requirements | **V5 §12:** recall existing; add new; duplicate State+Number; CA/LA/NV/KY/TX match; STATUS_RTK set; expired/inactive → CPC. |
| Tickets | None captured. |
| Existing implementation | Recruitment PSV license UI only. `PractitionerLicense` computed expiry. DEA/CDS state RTK lookups are **not** the V5 match rule. |
| Exact files | `RecruitmentStateLicenseVerificationForm.razor.cs`; `StateLicenseModal.razor.cs`; `PractitionerLicense.cs`; `CrudPractitionerLicensesOperation.cs`. `LicenseStatus` defining file **not opened**. |
| Gap | No NPP license section. Do not assume Packet `LicenseStatus` must change. |
| Dependency | I (visibility). D/H (task). |
| Implementation readiness | **BLOCKED** |
| Testing impact | High if shared Packet enum or Recruitment PSV components are modified. |

## K. PSV

| Item | Content |
|---|---|
| Requirements | **V5 §12 / §13:** License / NPI / Sanctions PSV by branch; DOC/DOCX/PDF/JPG/TIFF; attach to correct CACTUS record. |
| Tickets | None captured. Implied by workflow tickets. |
| Existing implementation | `PafFile.razor`; `PafController.UploadPafAttachmentAsync` (superset including BMP/PNG); Recruitment PSV detail shapes; two `ConvertPafAttachment` functions. |
| Exact files | `Client/Pages/Paf/Components/PafFile.razor(.cs)`; `PafController.UploadPafAttachmentAsync`; `RecruitmentPsvTask.cs`; `Hca.Credentialing.Image.Conversion/Functions.cs`; `Hca.Credentialing.Background/Functions/ImageConversionFunctions.cs`. |
| Gap | Pipeline reusable. NPP wiring missing. **UNKNOWN / NEEDS OFFICE EVIDENCE:** which ConvertPafAttachment is live. |
| Dependency | H/I/J. |
| Implementation readiness | **PARTIAL** (reuse pipeline) / **BLOCKED** (NPP-specific requiredness). |
| Testing impact | Medium. Do not tighten upload whitelist in a way that breaks other PAF types. |

## L. Submit / validation / routing

| Item | Content |
|---|---|
| Requirements | **V5:** block submit on incomplete required work and on CA/LA/NV/KY/TX mismatch; route PA=YES / non-Active / expired-matured license to CPC; MSP-only submit. |
| Tickets | None captured for NPP submit rules. Uses existing submit pipeline. |
| Existing implementation | `PafReviewForm` click-time validation; `SubmitPafStrategy` role routes MSP/CPC/Recruitment; queues `paf-processing`. |
| Exact files | `Client/Pages/Paf/Components/ReviewForm/PafReviewForm.razor.cs`; `Hca.Credentialing.Paf.Api/Domain/Strategy/SubmitPafStrategy.cs`. |
| Gap | No NPP routing predicates. Review & Submit availability ≠ V5. |
| Dependency | E–K populated. Upstream of M/N/O. |
| Implementation readiness | **BLOCKED** |
| Testing impact | High. Shared submit strategy. |

## M. Auto-acceptance

| Item | Content |
|---|---|
| Requirements | **V5 §13** + conflicts in §18 of curated V5. Ticket `132591` title-only. |
| Existing implementation | None automated. `PafStatus.AcceptedByCpc` exists as a human-set status. |
| Exact files | `Shared/Data/Model/Paf/Form/PafStatus.cs`; `CpcAction.cs`; `SubmitPafStrategy.cs`. |
| Gap | No engine. Candidate branch is Inference. |
| Dependency | L routing decision. Product conflict resolution. |
| Implementation readiness | **BLOCKED** |
| Testing impact | High. No existing auto-accept tests. |

## N. CPC

| Item | Content |
|---|---|
| Requirements | **V5:** exception path uses Add-Practitioner-to-Facility **process path** (not identical rules). Queue filter by ADD NPP type (assumption). Ticket `131783` title-only. |
| Existing implementation | `CpcDashboardPage`; `PafQueueSearch.BuildCpcStatusFilterOptions`; `CpcAction` / `PafStatus` vocabulary. |
| Exact files | `Client/Pages/Dashboards/CpcDashboardPage.razor(.cs)`; `Client/Pages/Queues/Components/PafQueueSearch.razor.cs`; `PafStatus.cs`. |
| Gap | No NPP type filter; no NPP trigger conditions. |
| Dependency | C (how type is stored) + L/M. |
| Implementation readiness | **BLOCKED** |
| Testing impact | Medium. CPC queue filters are shared. |

## O. CVI

| Item | Content |
|---|---|
| Requirements | **V5 §13:** type Facility Undefined Practitioner; statuses UDP Facility Request / Complete; +3 business days; notes = trigger; group name rule. **Also:** early V5 assumption says no CVI — product must confirm later section wins. |
| Tickets | `132537` title-only. |
| Existing implementation | `CrudEntityCredentialingOperation` RTK→due-date switch; `PacketCviUpdater` when `ShouldUpdatePacketCvis`. |
| Exact files | Both `CommonCactusRtks.cs`; both `CrudEntityCredentialingOperation.cs`; `PafExtensions.GetCredentialingTypeRtk()`; `PacketCviUpdater.cs`. |
| Gap | Constants absent. **Do not invent RTKs. Do not assume packet CVI association runs for ADD NPP** (V5: no packets). |
| Dependency | M (exception accepted). Office RTKs. |
| Implementation readiness | **BLOCKED** |
| Testing impact | `CrudEntityCredentialingOperationTests.cs` is a proven pattern **after** RTKs exist. |

## P. CACTUS synchronization

| Item | Content |
|---|---|
| Requirements | **V5 §14:** provider (net-new only), entity assignment `M67G0Q3AIP` NPP–Active with listed do-not-overwrite statuses, address, specialty/PPI, license, NPI/sanctions images. Tickets `132600`, `131724` title-only (`131724` title truncated). |
| Existing implementation | `CactusUpdater` + `Crud*Operation` for `AddPractitionerToFacility`. Portal.Api comments already mention RTKs `M67G0Q3AIP`, `M6LH0Z2G0S`, `M6LH0Z2GHZ` as NPP Active/Inactive/Suspend. |
| Exact files | `PafProcessorFunction.cs`; `CactusUpdater.cs`; `CrudPractitionerOperation.cs`; `CrudEntityAssignmentOperation.cs`; `CrudCredentialingAssignmentOperation.cs`; `CrudPractitionerAddressOperation.cs`; `CrudPractitionerSpecialtiesOperation.cs`; `CrudPractitionerLicensesOperation.cs`; Portal.Api query builders (status comment sites). |
| Gap | No `TaskType.Npp` (or other) branch. **UNKNOWN / NEEDS OFFICE EVIDENCE:** confirm Portal.Api RTKs are the V5 entity-assignment statuses; confirm `131724` actual title/AC; confirm sync reuses `CactusUpdater` (Inference, likely). |
| Dependency | C/D + finalized field mapping from E–K. Must **not** set `ShouldGeneratePacket` for ADD NPP. |
| Implementation readiness | **BLOCKED** |
| Testing impact | High. `CactusUpdaterIntegrationTests` / `CactusOperationIntegrationTests` do not match literal `AddPractitionerToFacility`. Coverage may be indirect only. |

## Q. PDF / History

| Item | Content |
|---|---|
| Requirements | **V5:** PDF type/title ADD NPP to Facility; attach to Provider Record unless CVI exists (then CVI); history user `HCP System User` / `HCP System User – MSP submitter name`. |
| Tickets | None captured. |
| Existing implementation | `PdfType` Standard/Dop/Cpc/ColoradoAddendum/TexasAddendum; `PdfGeneration.GeneratePdfFromCompletedPaf`; `CactusUpdater.CreatePafHistoryAsync`. |
| Exact files | `Hca.Credentialing.Background/Functions/PdfGeneration.cs`; `Hca.Credentialing.Document.Generation/Pdf/Operations/*`; `CactusUpdater.cs`. |
| Gap | No NPP PDF type/title. Attachment destination rule (provider vs CVI) not designed. |
| Dependency | C (type identity) + O (whether CVI exists). |
| Implementation readiness | **BLOCKED** |
| Testing impact | Medium. Template dispatch is shared. |

## R. Audit

| Item | Content |
|---|---|
| Requirements | **V5:** audit all system updates; auto-accept identity `HCP System User – MSP submitter name`; entity `HCA Corporate`. **Ticket 127104:** exception searches audit logged. |
| Tickets | `127104` (exception search). Rest V5-only. |
| Existing implementation | `ICredentialingAuditLogger` → `audit-log` queue → `AuditLogFunctions`. CACTUS field audit via `CactusAuditContext`. |
| Exact files | `Shared/Helpers/CredentialingAuditLogger.cs`; `Hca.Credentialing.Background/Functions/AuditLogFunctions.cs`; search form/table (audit call site **not traced**). |
| Gap | Exception-search call site unknown. “HCA Corporate” constant unknown. |
| Dependency | B for search audit; M/P for auto-accept identity. |
| Implementation readiness | **PARTIAL** (pipeline) / **BLOCKED** (NPP-specific identity rules). |
| Testing impact | Low for reuse; unknown for search path. |

## S. Reporting

| Item | Content |
|---|---|
| Requirements | **V5 §15:** monthly totals, CPC %, MOR exclude Facility Undefined Practitioner CVI. |
| Tickets | `132687` title-only. |
| Existing implementation | `RecruitmentReportGeneration.cs` only. |
| Exact files | `Hca.Credentialing.Background/Functions/RecruitmentReportGeneration.cs`. |
| Gap | No general PAF-type report. |
| Dependency | C/D (how NPP is indexed) + O (CVI type for MOR). Logically last. |
| Implementation readiness | **BLOCKED** |
| Testing impact | No reporting tests found. |

## T. Security

| Item | Content |
|---|---|
| Requirements | **Ticket 136883:** authorized MSP; block UI and direct URL; do not affect PSG/Recruitment; reuse MSP security model. **V5:** MSP-only ADD NPP action. **Title-only `131728`:** NPP RBAC — unknown AC. |
| Existing implementation | Role attributes on pages; `CanBeginPaf` parameter; Cactus.Api scope policy. No Begin-PAF-specific policy. |
| Exact files | `MspDashboardPage.razor`; `PractitionerSearchPage.razor`; `PafQueueSearchCriteria`; Server/Cactus/Paf `Startup.cs` / `[Authorize]` on `PafController` / `TaskController`. `PractitionerPafPage` role attribute **not confirmed**. |
| Gap | NPP-specific URL boundary unknown. `131728` unknown. |
| Dependency | A (entry) for `136883`. `131728` may be broader than entry — do not merge. |
| Implementation readiness | `136883` **PARTIAL**. `131728` **BLOCKED**. |
| Testing impact | Security regression on PSG/Recruitment search access. |

## U. Regression / testing

| Item | Content |
|---|---|
| Requirements | **Ticket 136882 / 136872 / 53658:** Begin PAF, its search, and validations unchanged. **V5:** no impact to existing PAF task cards, routing, demographics, packets unless specified. |
| Tickets | `136882` (captured constraint). Cross-cutting on every stream. |
| Existing implementation | Processor integration tests exist; **no** client tests; **no** Paf.Api tests. |
| Exact files | Begin PAF chain in stream A; Add-Practitioner-to-Facility chain in architecture §4; `Hca.Credentialing.Paf.Processor.Tests/*`. |
| Gap | Nothing locks UI regression automatically. |
| Dependency | Runs with every ticket, not only at the end. |
| Implementation readiness | **PARTIAL** (known constraint; weak automation). |
| Testing impact | This stream *is* the testing impact. |

---

# 3. Implementation Dependency Graph

Do not copy the gap-analysis numbered list as if it were the only order. Do not start at `TaskType.Npp` just because it is structurally deep. **Ticket gates and missing AC change what may be coded first.**

## 3.1 Azure DevOps relationship graph (fact)

From `NPP_Dependency_Map.md` — do not invent edges:

```text
52350  Project
  └── 136871  Epic  Begin NPP PAF entry
        ├── 136872  Feature
        │     ├── 136873  Button
        │     ├── 136876  Tooltip
        │     ├── 136878  Route to Enforce NPI Search
        │     ├── 136882  Preserve Begin PAF
        │     ├── 136883  Security / direct URL
        │     └── 144239  Related (not a prerequisite)
        └── successor 53658  Search feature
              └── 127104  Reuse Add New Practitioner search

52350 also lists (title only, no proven parent/child among themselves):
131724, 131728, 132600, 131783, 132591, 131785, 132255, 132537,
131794, 132687, 132247, 132275
```

**Ticket Evidence:** `53658` states epic `136871` must be **completed and deployed** before that search feature is developed, tested, or deployed.

## 3.2 Technical dependency graph (validated)

Two **independent** trunks exist.

```text
TRUNK 1 — Entry (captured AC, PARTIAL)
136873 button
   ├── 136876 tooltip (needs button)
   ├── 136878 routing (needs button + search-identity evidence)
   │      └── 136883 URL/auth (needs a real NPP route or mode)
   └── 136882 regression (starts the moment 136873 lands; formal after 136878/136883)
         └── 136871 / 136872 deploy gate
                └── 53658 / 127104 / 131785   [ticket-blocked until deploy]

TRUNK 2 — PAF type / task (structurally deep, BLOCKED)
Product + office decision: PafType vs TaskType.Npp vs reuse AddPractitionerToFacility
   └── Task determination + requiredness + Delegate/FSQ suppression
          └── NPP task host (ADD NPP to Facility card)
                 ├── Demographics / Address / Specialty+PPI   (parallel after host exists)
                 └── VA/PA
                        └── License / PSV
                               └── Submit validation + CPC routing
                                      └── Auto-accept  OR  CPC path
                                             └── CVI (needs office RTKs) 
                                                    └── CACTUS Crud* mapping (shape can be studied earlier; mapping needs fields)
                                                           ├── PDF / History (provider vs CVI attach)
                                                           └── Reporting (needs indexed type + CVI type)
```

**Cross-trunk links**

- Search (Trunk 1, after `136871` deploy) produces the practitioner that Trunk 2 initializes.
- Security `131728` (title-only) may cut across both trunks. Do not treat it as `136883`.
- Audit is cross-cutting: search exception (Trunk 1) and auto-accept identity (Trunk 2).
- Packet generation must stay **off** for ADD NPP wherever Trunk 2 touches `CactusUpdateResult`.

## 3.3 What was changed vs the example graph in the request

The requested example:

```text
TaskType.Npp → task determination → NPP task → data/model
  → Demographics/Address/Specialty → VA/PA → License/PSV
  → Submit → Auto-accept/CPC → CVI → CACTUS → PDF/Reporting
```

**Keep** that order **inside Trunk 2**. It matches repository coupling: an unwired `TaskType` will throw in `PafExtensions.GetCredentialingTypeRtk` if activated halfway; VA/PA must exist before license visibility; routing must exist before auto-accept; CVI needs RTKs; reporting is last.

**Change:** do **not** put `TaskType.Npp` first in the **project** sequence.

| Reason | Evidence |
|---|---|
| `TaskType.Npp` identity is Inference | Enum description `"NPP PAF"` ≠ V5 title; `PafType` fork open |
| Owning tickets have no AC | `132255`, `132275` title-only → **BLOCKED** |
| Entry point has captured AC and no Trunk 2 dependency | `136873` family |
| Search is ticket-gated on entry deploy | `53658` |

**Change vs lifecycle / progress tracker:** Search is **not** Phase 1 before entry. Entry is first. Search is after `136871` deploy.

**Change vs gap-analysis step 1 “resolve TaskType.Npp first”:** that is the first **design** decision for Trunk 2, not the first **implementable** ticket. It can be analyzed in parallel. It cannot be coded first.

## 3.4 Parallelism that is actually safe

| Can proceed in parallel | Cannot |
|---|---|
| Inspect `SharedPafQueue` / search-mode / tooltip pattern (evidence gathering) while this plan is used | Implement `53658` before `136871` deploy |
| Analyze Trunk 2 design (`PafType` vs `TaskType`) while implementing Trunk 1 | Wire `TaskType.Npp` without AC and without the type decision |
| Capture title-only ticket AC from Azure DevOps at any time | Invent CACTUS RTKs, PPI RTKs, or auto-accept behavior while V5 conflicts |
| `136882` regression checks with every Trunk 1 change | Modify Begin PAF handler to “share” NPP routing |

---

# 4. Implementation Readiness by Ticket

| Ticket | Implementation Area | Readiness | Blocking Evidence | Exact Files | Dependencies |
|---|---|---|---|---|---|
| 136871 | Entry epic | **NOT AN IMPLEMENTATION UNIT** | Epic container. No AC in export. Track completion via `136872` children + deploy for `53658`. | N/A | Children of `136872` |
| 136872 | Entry feature | **NOT AN IMPLEMENTATION UNIT** | Feature grouping. Feature AC exists but is decomposed to stories. Do not code the feature as one change set. | Same as children | `136873`–`136883` |
| **136873** | Begin NPP PAF button | **BLOCKED** | Callers of `PafQueueSearchCriteria` unread; `CanBeginPaf` unread; same `MspDashboardPage` is also `/Psg/Dashboard`; Recruitment “Begin Request”; `SharedPafQueue` also used on CPC dashboard. Cannot add the button independently without leak risk. | `PafQueueSearchCriteria.razor` + `.cs`; `MspDashboardPage.razor`; **need** `SharedPafQueue.razor.cs` and **all** callers | None from Trunk 2. Minimum MSP-only visibility is required to meet this story without violating V5. Full URL auth is `136883`. |
| 136876 | Tooltip | **BLOCKED** | Depends on a real Begin NPP PAF control. Tooltip/info-icon pattern not confirmed. Extra mockup sentence not approved. | Same criteria component; pattern file **UNKNOWN / NEEDS OFFICE EVIDENCE** | `136873` |
| 136878 | Route to Enforce NPI Search | **BLOCKED** | `/Practitioner/Search` is **not** proven to be Enforce NPI Search. UI Evidence: two different search UIs both appear at that URL. `SearchMode` exists but which mode Begin PAF vs PSG uses is unread. | `NavigateToStartPaf`; `PageUri.cs`; `PractitionerSearchForm.razor(.cs)`; `PractitionerSearchPage.razor` | `136873`. Design-coupled with `136883`. Must not break `136882`. |
| 136882 | Preserve Begin PAF | **NOT AN IMPLEMENTATION UNIT** | Regression constraint + test/UAT requirement. Tasks are regression/functional/UAT — not a feature change. | `NavigateToStartPaf`; Begin PAF search screenshots | Active from the first `PafQueueSearchCriteria` edit |
| 136883 | Entry security | **BLOCKED** | No NPP-specific policy exists. Auth is `[Authorize(Roles=...)]` plus `CanBeginPaf`. Direct-URL AC cannot be designed until `136878` destination is known. Do not invent a policy. Do not use `131728` AC. | `MspDashboardPage.razor`; `PractitionerSearchPage.razor`; unread `CanBeginPaf` source | `136873` (UI) + `136878` (URL). Existing search access for PSG/Recruitment must remain. |
| 144239 | Related entry | **BLOCKED** | No AC export | Unknown | Related ≠ dependency |
| 53658 | PAF Search feature | **BLOCKED** | Own text: wait for `136871` **deployed**. Exception-field Conflict + exception-mode fields unread | Search files in stream B | `136871` deployed; `136878` overlap |
| 127104 | Search reuse | **BLOCKED** | Same gate as `53658`. Exception audit call site unread. V5 field Conflict | Same as B + ConfirmationWizard | `53658` / `136871` |
| 131785 | No-NPI exception | **BLOCKED** | Title only | `PractitionerSearchForm` exception branch (unread in full) | Do not merge into `127104` AC |
| 132255 | PAF action / type | **BLOCKED** | Title only. `PafType` vs `TaskType` fork | `PafType.cs`; `TaskType.cs`; create-PAF path | Product decision |
| 132275 | PAF Tasks | **BLOCKED** | Title only | `TaskDeterminationService.cs`; `PafProcessingStrategyContext.cs`; `TaskType.cs` | `132255` decision |
| 131794 | Add New Practitioner card | **BLOCKED** | Title only — modification unknown | `AddNewPractitionerForm.razor.cs`; `ManagePractitionerInformationTask.razor.cs` | C/D |
| 132247 | Existing practitioner card | **BLOCKED** | Title only | `ManagePractitionerInformationTask.razor.cs` | C/D |
| 131728 | NPP RBAC | **BLOCKED** | Title only. Must not use `136883` AC | Authorization layer **UNKNOWN** | Unknown |
| 131783 | CPC type filter | **BLOCKED** | Title only | `PafQueueSearch.razor.cs`; `PafType` / indexed type | C |
| 132591 | Auto-acceptance | **BLOCKED** | Title only. No engine. V5 VA+PA conflict | `SubmitPafStrategy.cs`; `PafStatus.cs` | L + product |
| 132537 | CVI creation | **BLOCKED** | Title only. RTKs absent | `CommonCactusRtks.cs` (both); `CrudEntityCredentialingOperation.cs` (both) | M + office RTKs |
| 132600 | PAF→CACTUS sync | **BLOCKED** | Title only. Reuse of `CactusUpdater` is Inference | `CactusUpdater.cs`; `Crud*Operation` | C/D + field mapping |
| 131724 | CACTUS credentialing values | **BLOCKED** | Title truncated + no AC | `CommonCactusRtks.cs` (candidate only) | Office title + CACTUS reference data |
| 132687 | Reports | **BLOCKED** | Title only | `RecruitmentReportGeneration.cs` (analog only) | C + O |

**No row is READY.**

---

# 5. Recommended Ticket Sequence

Sequence is **technical dependency + ticket gates + readiness**. It is not “easiest first.”

## Sequence table

| Order | Ticket | Why this position | What must already exist | Files that will probably change | Regression risk | Tests required |
|---|---|---|---|---|---|---|
| 0 | **Evidence pull (not a ticket)** | 136873 is first *candidate* but **not READY**. Do not code until callers / `CanBeginPaf` / MSP vs PSG vs Recruitment vs CPC usage of `PafQueueSearchCriteria` are known | — | Inspect only. No application change | — | Return evidence to Cursor |
| 1 | **136873** | First coding story **after** evidence: display/layout/a11y AC; tooltip, click, and URL auth have nothing to attach to without this control. Not first because it is the lowest child ID | Evidence from row 0 | `PafQueueSearchCriteria.razor` (+ `.cs` only if a visibility parameter is required). Caller markup only if needed | Leak onto PSG `/Psg/Dashboard`, Recruitment “Begin Request”, or CPC `SharedPafQueue`; restyling Begin PAF | Keyboard, screen reader, adjacent layout; Begin PAF unchanged; NPP button absent on Recruitment/PSG/CPC unless evidence says otherwise |
| 2 | **136876** | Tooltip AC is known; needs a control to attach to | Button from 136873; existing tooltip pattern | Criteria `.razor`; existing tooltip component **if found** | Extra mockup sentence must not be added; hover/focus must not break the button | Hover + keyboard focus show/hide; exact approved sentence |
| 3 | **136878** | Click must reuse Enforce NPI Search; this is the load-bearing entry behavior | Button; **search-identity evidence** (same page/mode vs different UI) | `PafQueueSearchCriteria.razor.cs`; possibly `PageUri.cs` / `StateContainer` **only if evidence shows a context flag is required** | **Highest Trunk 1 risk:** accidentally pointing Begin PAF at NPI-required search, or duplicating a search workflow | Click Begin NPP PAF → Enforce NPI Search; Begin PAF path unchanged; no new search component |
| 4 | **136883** | URL/auth cannot be designed until the NPP URL/mode exists | 136878 destination | Unknown until route/mode is known. Must not invent a new IDP policy without evidence | Blocking PSG/Recruitment from their existing search; thinking hide-button is enough | Authorized MSP; unauthorized UI + direct URL; PSG/Recruiter regression |
| 5 | **136882** | Formal regression ticket after the entry slice exists. Informally enforced from step 1 | 136873–136883 landed | **Should not change application files** unless a regression is found | This ticket *is* the regression gate | Begin PAF click, search fields, validations, UAT support per ticket tasks |
| 6 | **136872 / 136871** | Feature/epic close and **deploy** — `53658` gate | Stories 136873–136883 | Deploy, not a code ticket | — | Feature AC checklist on `136872` |
| 7 | **53658** then **127104** | Successor search slice. Ticket forbids starting before 136871 deploy | Deployed entry; exception-field and exception-audit evidence | Prefer **no new search logic**. Possible small audit-hook **only if** existing exception path has no audit and `127104` requires it | Changing shared NPI validation or exception criteria (V5 Conflict) | 10-digit NPI; invalid NPI does not search; exception min-criteria; audit; Begin PAF search unchanged |
| 8 | **131785** | Separate no-NPI ticket. Do not implement from V5 fields under this ID until AC exists | `127104` reuse decision | Unknown | Replacing existing exception fields with V5 | Per future AC |
| 9 | **132255** then **132275** | Trunk 2 start. Type/action then tasks | Product decision + full AC | `PafType` and/or `TaskType` + factory + `TaskDeterminationService` + task UI | Exhaustive enum switches; Add-Practitioner-to-Facility; Delegate/FSQ | Task list new vs existing; suppression; other PAF types unchanged |
| 10 | **131794** / **132247** | Cards after task host exists | 132275 wiring | Demographic / Add New Practitioner files | Changing requiredness for non-NPP PAFs | Net-new vs existing email/optional rules |
| 11 | Address / Specialty / ADD NPP card / VA-PA / License / PSV | V5-owned; **no captured story IDs** for most | Do not start as “tickets” until ADO AC exists or the user assigns V5 slices to `132275` / `132255` | Stream F–K files | Shared task components | Per V5 positive/negative/exception |
| 12 | **132591** | Auto-accept after routing predicates exist | Product resolution of V5 conflicts; submit path | `SubmitPafStrategy` (candidate only) | Completing PAFs without CPC when they should route, or the reverse | Normal complete-queue; exception not silently completed if product says no |
| 13 | **131783** | CPC dashboard filter after type is stored | C | `PafQueueSearch.razor.cs` / indexed type | CPC filters for other types | Filter/select ADD NPP |
| 14 | **132537** | CVI after exception accept | Office RTKs | Both `CommonCactusRtks` + both `CrudEntityCredentialingOperation` + `GetCredentialingTypeRtk` | Wrong CVI type; packet CVI updater running (forbidden unless evidence says so) | Due date +3 business days; type/status; no packet |
| 15 | **132600** / **131724** | CACTUS sync / reference values | Field mapping + RTKs | `CactusUpdater` + `Crud*` | Provider overwrite; packet generation; wrong entity status | Net-new vs existing; NPP–Active rules; image names |
| 16 | PDF / History / Audit identity | V5; no ticket IDs | Type identity + CVI-or-provider rule | `PdfGeneration.cs`; history/audit helpers | Wrong PDF title on other types | Title; attachment target; HCP System User |
| 17 | **132687** | Reporting last | Indexed NPP + CVI type | New timer analog or confirmed external report | Hardcoding another `IndexedTaskSelected` incorrectly | Monthly counts; MOR exclusion |
| 18 | **131728** | Place when AC arrives; may move earlier if AC shows it blocks entry | Full AC from Azure DevOps | Unknown | Over-grant/over-block NPP access | Per future AC |

**Do not generate Copilot implementation prompts in this section.** The next prompt is for order 1 only, in a later Cursor step.

---

# 6. Copilot Execution Rules

Copilot must:

- work on **one ticket at a time**
- inspect the **exact files named for that ticket** before changing them
- make **minimal** changes
- **reuse** existing HCP patterns
- never redesign unrelated architecture
- never invent CACTUS RTKs
- never modify unrelated PAF flows
- run **targeted** tests/build after implementation
- report **changed files**
- report **tests executed**
- report **unresolved issues**

Copilot must not:

- repeatedly rediscover the entire repository
- implement from a whole-project prompt
- treat the gap analysis as automatically correct (use this plan’s validation ledger)
- invent file paths, policies, feature flags, or RTKs
- “complete” sibling tickets in the same session
- change Begin PAF navigation, search fields, or validations while implementing NPP
- enable packet determination / packet build for ADD NPP
- merge `131728` into `136883`
- replace existing no-NPI exception fields with V5 fields because V5 lists them

**Repository knowledge base for Copilot (do not rediscover):**

1. [`08_Office_Repository_Evidence/ADD NPP — Code-Level Architecture .md`](../08_Office_Repository_Evidence/ADD%20NPP%20—%20Code-Level%20Architecture%20.md)
2. [`04_GAP_ANALYSIS/ADD NPP — Existing vs Required Gap Analysis.md`](../04_GAP_ANALYSIS/ADD%20NPP%20—%20Existing%20vs%20Required%20Gap%20Analysis.md)
3. **This file** (control: what is validated, blocked, or overstated)

If Copilot must open a file that is **UNKNOWN / NEEDS OFFICE EVIDENCE** (for example `SharedPafQueue.razor.cs`), it inspects **that file only**, then implements. It does not restart architecture discovery.

Prompt shape when Cursor later writes one: [`Copilot_Prompt_Template.md`](Copilot_Prompt_Template.md). Quality gate in `SKILL.md` §13 still applies.

---

# 7. Implementation Loop

# Implementation Loop

For every ticket:

1. Cursor selects the ticket from §5 (do not skip gates).
2. Cursor reads the requirement (V5 curated file and/or captured ticket AC).
3. Cursor reads the gap analysis **and** §1 of this plan (validation overrides).
4. Cursor identifies exact repository files (architecture file index + this plan).
5. Cursor verifies dependencies (this plan §3 / §4 / §9). If readiness is **BLOCKED** or **NOT AN IMPLEMENTATION UNIT**, stop. Do **not** inspect-and-implement in one session when visibility callers are unread. Request office evidence first.
6. Cursor creates **one** focused Copilot prompt.
7. Copilot inspects those files (and only the listed evidence-gap files).
8. Copilot implements the ticket only.
9. Copilot builds / runs targeted tests.
10. User brings diff / errors / results back to Cursor.
11. Cursor reviews implementation against:
    - requirement (V5)
    - ticket AC
    - architecture (reuse, no new architecture)
    - gap + this validation ledger
    - regression (Begin PAF, other PAF types, packets)
    - security (MSP vs PSG/Recruitment; hide ≠ URL)
    - data / CACTUS (no invented RTKs; no packet)
    - audit
12. Cursor updates this plan’s readiness notes and `ADD_NPP_PROGRESS_TRACKER.md`.
13. Move to the next ticket.

**Stop conditions (from the skill — still in force):** multiple implementations and the correct one is unclear; ticket and code conflict; V5 conflicts (auto-accept / CVI / exception fields); file path unverified; proposed change needs new architecture; change may significantly affect existing PAF behavior.

---

# 8. Copilot Credit Strategy

Optimize for limited Copilot AI credits.

**Rules**

- No whole-repository implementation prompts.
- No repeated architecture discovery.
- No “understand the entire ADD NPP project” prompts.
- **One ticket per Copilot implementation session.**
- Keep implementation prompts focused on **verified files**.
- Continue the **same** Copilot conversation for implementation → build → targeted fix when useful.
- Start a **fresh** Copilot conversation when moving to a different ticket.
- Prefer deterministic repository inspection and normal coding for simple changes (for example: adding a second button next to a known button once `CanBeginPaf` / caller parameters are visible).
- Use Copilot reasoning primarily where it adds value (shared-component visibility, search-mode identity, strategy-factory exhaustiveness, CACTUS branch analysis).
- Do not ask Copilot to generate this master plan, the gap analysis, or new RTKs.
- Do not attach the full architecture document to every prompt. Cite the 5–15 files for that ticket.
- Evidence-gathering that is a single unread file (`SharedPafQueue.razor.cs`, exception-mode field list, `LicenseStatus` enum definition) can be the **first instruction** in the same ticket conversation. That is cheaper than a new “discover the repo” chat.
- Title-only tickets: do **not** spend Copilot credits implementing from titles. Spend human/ADO time capturing AC first.

**Credit-expensive work to defer**

- Auto-accept design (`132591`) until product resolves V5 conflicts.
- CVI constants until office RTKs exist.
- `PafType` enum expansion until the type-vs-task decision is written down.
- Reporting (`132687`) until NPP is indexed in real data.

---

# 9. Entry-point sequence (corrected)

This section **replaces** the prior “first implementation candidate = 136873 PARTIAL, inspect-then-code in one Copilot session” close-out.

## 9.1 Hierarchy (Ticket Evidence)

```text
52350  Project
  └── 136871  Epic — no AC in export; not a coding ticket
        ├── 136872  Feature — has feature AC; implementation is children
        │     ├── 136873  Story — button display / layout / a11y
        │     ├── 136876  Story — tooltip on that button
        │     ├── 136878  Story — click launches existing Enforce NPI Search
        │     ├── 136882  Story — Begin PAF unchanged (regression/UAT)
        │     └── 136883  Story — MSP auth; hide is not enough; direct URL
        └── successor 53658  Feature — blocked until 136871 completed AND deployed
```

| ID | Captured type | Independently implementable story? | Track implementation at |
|---|---|---|---|
| 136871 | Epic | No | Child feature `136872` stories; epic done when those are done **and deployed** (`53658` Scenario 6) |
| 136872 | Feature | No. Feature AC is the integration checklist for the five stories | Child stories |
| 136873–136883 | User stories | See §9.3 | Each story, except `136882` which is a constraint |

ADO Predecessor was **not captured** on the five stories. Order below is from AC, shared-component impact, navigation, security, and regression — not ticket number.

## 9.2 Verdict

**NO IMPLEMENTATION TASK IS READY.**

136873 is the first *candidate* after evidence, because:

- 136871 / 136872 are not coding units.
- 136876 has no control to attach to until 136873 exists.
- 136878 has no NPP action to wire until 136873 exists, and the Enforce NPI Search target is unproven.
- 136883 cannot design a URL gate until 136878’s destination exists.
- 136882 is a constraint, not a first change.
- 53658 is blocked by 136871 deploy.

136873 is **not** first because it is the lowest child ID. It is **BLOCKED** (not PARTIAL) until the evidence in §9.5 is returned to Cursor. Do **not** inspect-and-implement in one Copilot session.

Do **not** combine 136873 with 136876 (tooltip pattern would block the button). Do **not** combine 136873 with 136878 (search-identity would block the button). 136878 and 136883 are **design-coupled** (same URL cannot be blocked for unauthorized users without breaking PSG/Recruitment) but remain separate ADO stories.

## 9.3 Entry-point classification

| ID | Title | Type | Code Change? | Dependency | Evidence | Readiness |
|---|---|---|---|---|---|---|
| 136871 | Begin NPP PAF Entry Point | Epic / parent container | No | Children of 136872 | Ticket: Epic; no AC | **NOT AN IMPLEMENTATION UNIT** |
| 136872 | Begin NPP PAF on MSP Dashboard | Feature grouping | No (not as one change set) | Children | Ticket: Feature AC = sum of stories | **NOT AN IMPLEMENTATION UNIT** |
| 136873 | Add Begin NPP PAF action | Independently implementable **after** visibility evidence | Yes — additive UI | None from other stories; minimum MSP-only visibility required by V5 / 136873 AC (“on the MSP Dashboard”) | Repo: button in `PafQueueSearchCriteria`; `CanBeginPaf` unread; `MspDashboardPage` is also `/Psg/Dashboard`; `SharedPafQueue` also on CPC | **BLOCKED** |
| 136876 | Tooltip | Must follow 136873; separate task | Yes — on the new control | 136873 | Approved text captured. Tooltip pattern **UNKNOWN**. Extra mockup sentence not approved | **BLOCKED** |
| 136878 | Launch Enforce NPI Search | Must follow 136873; design-coupled with 136883 | Yes — click/navigation only | 136873. Must not change Begin PAF (`136882`) | Begin PAF → `/Practitioner/Search`. UI: two different search screens at that URL. `SearchMode` exists; mode used by MSP Begin PAF vs PSG **not proven**. Do not assume that URL is Enforce NPI Search | **BLOCKED** |
| 136882 | Preserve Begin PAF | Regression constraint + tests/UAT | No, unless a regression is found | Active from first shared-component edit | `NavigateToStartPaf`; screenshots NPI+SSN+name | **NOT AN IMPLEMENTATION UNIT** |
| 136883 | Security / direct URL | Must follow 136878 destination | Maybe — only if existing role gates cannot satisfy AC. Do not invent a policy | 136873 (UI) + 136878 (URL) | Roles `[Authorize]`; no Begin-PAF policy; `CanBeginPaf` unread. Same search page allows Msp, Psg, Recruiter, RecruiterManager | **BLOCKED** |
| 53658 | PAF Search | Feature — later slice | Later | **136871 completed and deployed** (ticket Scenario 6). Overlaps 136878 on routing | Ticket successor of 136871 | **BLOCKED** |

## 9.4 Dependency graph (evidence)

```text
136871  (epic container)
   └── 136872  (feature grouping / integration AC)
          ├── 136873  (button)     ──► 136876  (tooltip)
          │         │
          │         └──► 136878  (route)  ──► 136883  (URL/auth)
          │                     │
          └── 136882  (constraint on every change above)
                 │
                 ▼
           136871 complete + deployed
                 │
                 ▼
               53658
```

| Edge | Meaning | Evidence |
|---|---|---|
| 136871 → 136872 | Epic contains feature | Ticket: 136872 parent is 136871 |
| 136872 → 136873, 136876, 136878, 136882, 136883 | Feature contains stories | Ticket children list |
| 136873 → 136876 | Tooltip requires the Begin NPP PAF control | 136876 AC: hover over Begin NPP PAF; 136876 dependencies: button 136873 |
| 136873 → 136878 | Routing requires the action that is clicked | 136878 AC: user selects Begin NPP PAF; tasks: wire dashboard button |
| 136878 → 136883 | Direct-URL gate needs a known destination; if destination is the existing shared search URL, a new route/mode is required or the AC cannot be met without breaking PSG/Recruitment | 136883 AC: UI **or** direct URL; PSG/Recruitment access unaffected. Repo: `PractitionerSearchPage` already allows Msp, Psg, Recruiter, RecruiterManager |
| 136882 on all | Begin PAF click, search fields, validations unchanged | 136882 AC; 136872 AC; 53658 Scenario 5 |
| **136871 → 53658** | Epic must be completed **and deployed** before search-feature work | 53658 Scenario 6 and Dependency section. **Not changed.** |

## 9.5 Missing evidence before 136873 can become READY

**UNKNOWN / NEEDS OFFICE EVIDENCE** (inspect only; return to Cursor; do not implement):

| Evidence | File | Why it blocks independent implementation |
|---|---|---|
| `CanBeginPaf` computation and all pass-sites | `Client/Pages/Queues/Components/SharedPafQueue.razor.cs` (stated caller; confirm). **All** callers of `PafQueueSearchCriteria` | Architecture: `SharedPafQueue` is also used on `CpcDashboardPage`. Recruitment uses “Begin Request” on the same criteria component. Guessing visibility leaks NPP onto the wrong queue |
| How `MspDashboardPage` distinguishes `/Msp/Dashboard` vs `/Psg/Dashboard` | `Client/Pages/Dashboards/MspDashboardPage.razor` | Same component, `[Authorize(Roles = "Msp, Psg")]`. 136873 AC is MSP Dashboard only. V5: MSP-only submit |
| Existing Begin PAF button markup / a11y | `PafQueueSearchCriteria.razor` | Match styling and accessibility; do not invent a control type |
| (For 136878 later, not 136873) Search-mode identity | `PractitionerSearchForm.razor(.cs)`; query/state into `/Practitioner/Search` | UI Evidence: Begin PAF search is NPI+SSN+name; Enforce NPI Search is NPI-required. Both screenshots can show `/Practitioner/Search`. Repo has `SearchMode` but does **not** prove which mode MSP Begin PAF uses |
| (For 136876 later) Tooltip/info-icon pattern | Unknown file | 136876 task “Create tooltip component” must not invent one if a pattern exists |

## 9.6 Files for the first *candidate* (136873) — inspect now, change only after §9.5

**Inspect**

- `Client/Pages/Queues/Components/PafQueueSearchCriteria.razor`
- `Client/Pages/Queues/Components/PafQueueSearchCriteria.razor.cs`
- `Client/Pages/Queues/Components/SharedPafQueue.razor.cs`
- `Client/Pages/Dashboards/MspDashboardPage.razor`
- Any other file that renders `<PafQueueSearchCriteria` or sets `CanBeginPaf`

**Must not change while gathering evidence / when later implementing 136873**

- `NavigateToStartPaf` semantics for Begin PAF
- `PageUri.PractitionerSearchUri` (136878)
- Practitioner search forms/validation
- IDP policies / new roles
- `TaskType` / PAF strategies / CACTUS

## 9.7 Tests when 136873 later becomes READY

No Blazor client test project (Repository Evidence). Manual:

- `/Msp/Dashboard`: Begin PAF and Begin NPP PAF adjacent; a11y
- Begin PAF unchanged (136882)
- NPP button absent on Recruitment “Begin Request”, `/Psg/Dashboard`, and CPC `SharedPafQueue` unless evidence shows it should appear

## 9.8 Next Cursor action

**Not** a Copilot implementation prompt.

1. Office inspect of §9.5 files (read-only).
2. Cursor reviews that evidence.
3. If visibility can be limited to MSP Dashboard without guessing, 136873 becomes READY.
4. Then one focused Copilot prompt for 136873 only.

---

# 10. Office Evidence Still Required (backlog)

Do not invent answers. Bring these from the office laptop / Azure DevOps / CACTUS when the matching ticket is next.

| Priority | Evidence | Blocks |
|---|---|---|
| **Now — before any implementation prompt** | `SharedPafQueue.razor.cs`; **all** `PafQueueSearchCriteria` callers; how `MspDashboardPage` splits MSP vs PSG | 136873 cannot become READY |
| Before 136876 prompt | Existing tooltip/info-icon component used on dashboard/queue actions | Tooltip pattern |
| Before 136878 prompt | Full `PractitionerSearchForm` modes: what MSP Begin PAF shows vs PSG/Recruitment Enforce NPI Search; any query/state | Routing without breaking Begin PAF |
| Before 136883 prompt | Exact URL after 136878; how unauthorized direct URL is handled today | URL gate |
| Before 53658 / 127104 | Exception-mode field list + validations; `QueueAuditLogEntry` on exception search | Reuse vs V5 Conflict |
| Before Trunk 2 coding | Full AC for `132255`, `132275`; written PafType vs TaskType decision | Type/task wiring |
| Before VA/PA / auto-accept | Product decision on V5 CVI-assumption conflict and VA+PA=YES auto-accept conflict | `132591` / `132537` |
| Before CVI / PPI / license RTKs | CACTUS reference values (UDP type/statuses; APP-Other; `DS9Q12M4UO`; license STATUS_RTKs already listed in V5) | Do not invent |
| Before PSV image work | Which `ConvertPafAttachment` function is deployed | Avoid editing dead code |
| Before 131728 / 131724 / 132600 / 132687 / 131783 / 131785 / 131794 / 132247 | Full Azure DevOps AC (and untruncated `131724` title) | Those tickets stay BLOCKED |
| Confirm | Portal.Api `M67G0Q3AIP` / `M6LH0Z2G0S` / `M6LH0Z2GHZ` vs V5 entity-assignment statuses | De-risks CACTUS assignment |
| Confirm | `LicenseStatus` defining file — and whether NPP should use CACTUS lookup instead | Gap 8 layer |

---

# 11. Explicit Non-Goals (carry forward)

From V5 and captured tickets — still binding:

- Do not merge ADD NPP PAFs.
- Do not create credentialing packets or run packet determination for ADD NPP.
- Do not require Delegate or Facility Specific Questions cards.
- Do not change Begin PAF.
- Do not duplicate Enforce NPI Search / Add New Practitioner search.
- Do not treat ADD NPP as privileged-practitioner credentialing.
- Do not assume every exception creates a CVI beyond V5 CPC-route cases.
- Do not overwrite existing CACTUS provider/address/active-at-another-facility license unless V5 says so.
- Do not invent CACTUS RTKs.
- Do not implement title-only tickets from titles.

---

# 12. Close-Out for This Planning Step

## 1. 136871

**Epic / parent container.** Not an independently implementable story. No AC in the export. Track via `136872` children. Epic is the **53658 deploy gate**.

## 2. 136872

**Feature grouping.** Has independently *testable* feature AC, but those AC are the sum of the five stories. Do not implement 136872 as one coding task.

## 3–7. Stories

See §9.3. Code changes: **136873, 136876, 136878**, and **maybe 136883**. **136882** is regression/UAT, not a feature change.

## 8. Dependency graph

`136873 → 136876`; `136873 → 136878 → 136883`; `136882` constrains all; **`136871 → 53658` unchanged.**

## 9. First implementation task

**NO IMPLEMENTATION TASK IS READY.**

First *candidate* after evidence: **136873**.

## 10. Files for that candidate

Inspect (do not change yet): `PafQueueSearchCriteria.razor` / `.cs`; `SharedPafQueue.razor.cs`; `MspDashboardPage.razor`; all `PafQueueSearchCriteria` callers.

## 11. Remaining evidence gaps

`CanBeginPaf` source; all callers (including CPC); MSP vs PSG split on the shared dashboard page; later: search-mode identity for 136878; tooltip pattern for 136876.

## 12. Master plan updated?

**Yes.** Entry-point sequence, readiness, and next step were corrected. Unrelated Trunk 2 sections were not rewritten.

```text
MASTER PLAN  (this file — corrected)
    → OFFICE EVIDENCE  (§9.5, read-only)
    → CURSOR REVIEWS VISIBILITY
    → IF 136873 IS READY: ONE FOCUSED COPILOT PROMPT
    → COPILOT IMPLEMENTS 136873 ONLY
```

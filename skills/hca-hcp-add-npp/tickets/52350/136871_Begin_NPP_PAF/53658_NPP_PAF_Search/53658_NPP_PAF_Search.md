# 53658 â€” NPP: PAF - Search

Full capture: [../../../../06_Successors/53658_NPP_PAF_Search/53658.md](../../../../06_Successors/53658_NPP_PAF_Search/53658.md)

## 1. Azure DevOps Metadata

- Ticket ID: 53658
- Work item type: Feature
- Title: NPP: PAF - Search
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Products Epics Features
- State: To Do
- Tags: HCP, NPP PAF, Search Options, Sprint 6.0 2026
- Parent: Successor of 136871; also listed under 52350
- Children: 127104 â€” NPP: Add NPP to Facility PAF Practitioner Search â€“ Add New Practitioner
- Predecessor: 136871 (must be completed and deployed first)
- Successor: Not captured
- Related items: Not captured

This work item is a **Successor** of `136871`, not merely another child of `136871`.

## 2. User Story

Not written as â€œAs aâ€¦â€ on the captured feature. Intent: Practitioner Search for NPP uses NPI as primary required criterion, with a controlled exception when NPI is unavailable, by leveraging existing Enforce NPI Search after Begin NPP PAF.

## 3. Description

The Practitioner Search workflow within HCP enforces the National Provider Identifier (NPI) as the primary and required search criterion.

Users must enter a valid 10-digit NPI before a standard practitioner search can be performed. A controlled exception workflow is available when an NPI is unavailable.

This feature will leverage the existing Enforce NPI Search workflow when users select **Begin NPP PAF** from the MSP Dashboard, ensuring a consistent practitioner search experience for Non-Privileged Practitioner (NPP) requests.

## 4. Acceptance Criteria

- Users selecting Begin NPP PAF are routed to existing Enforce NPI Search.
- Practitioner Search requires a valid 10-digit NPI before standard search.
- Users without an NPI use the existing controlled exception workflow.
- Existing Enforce NPI Search is reused; no duplicate search logic.
- Existing Begin PAF functionality remains unchanged.
- Epic 136871 must be completed and deployed prior to implementation and testing of this feature.

## 5. Tasks

Not listed as a development-task checklist on the captured feature. Child `127104` has development/QA tasks.

## 6. Dependencies

**Epic 136871 must be completed before development, testing, or deployment of this feature.**

Overlaps `136878` on routing/reuse. `136878` is part of the entry-point epic. This feature is the later search slice.

`131785` (No NPI exception) is title-only under 52350. Do not merge its unknown AC into this feature.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- NPI-required standard search; existing exception workflow; reuse; Begin PAF unchanged; blocked on 136871.

**Existing UI behavior observed in screenshots**

- Begin PAF search is not NPI-only.
- NPI-required search screen exists with No-NPI link.

**Inference**

- Exact exception fields must come from existing implementation / `127104` / V5 comparison, not from this feature ticket alone.

## 8. Screenshots / References

- [../../../../screenshots/NPP_Search](../../../../screenshots/NPP_Search)
- [../../../../screenshots/Begin_PAF_Current](../../../../screenshots/Begin_PAF_Current)

## 9. Implementation Investigation Questions

- Where is Enforce NPI Search implemented?
- Does an exception workflow already exist, and what are its minimum criteria?
- How is Facility PAF / ADD NPP search currently routed?
- What must wait until 136871 is deployed?

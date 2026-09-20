# 136871 â€” NPP: Add Begin NPP PAF Entry Point to MSP Dashboard

Full capture: [../../../02_Epics/136871_Begin_NPP_PAF_Entry_Point/136871.md](../../../02_Epics/136871_Begin_NPP_PAF_Entry_Point/136871.md)

Project scope: [../../../NPP_52350_Project_Scope.md](../../../NPP_52350_Project_Scope.md)

## 1. Azure DevOps Metadata

- Ticket ID: 136871
- Work item type: Epic
- Title: NPP: Add Begin NPP PAF Entry Point to MSP Dashboard
- Area: Not captured in the epic export
- Iteration: Not captured in the epic export
- State: To Do
- Tags: MSP Dashboard; NPP PAF; Sprint 7.0 2026; UI; Workflow
- Parent: 52350 â€” Project - Add NPP to existing PAF Types
- Children: 136872 â€” NPP: Add "Begin NPP PAF" Entry Point on MSP Dashboard
- Predecessor: Not captured
- Successor: 53658 â€” NPP: PAF - Search
- Related items: Not captured
- Priority: 2

## 2. User Story

Not labeled as a user story on the captured epic. Intent from the description:

MSP Dashboard must provide a dedicated entry point to initiate a PAF for Non-Privileged Practitioners, in addition to existing Begin PAF.

## 3. Description

As part of the NPP (Non-Privileged Practitioner) initiative, the MSP Dashboard must provide users with a dedicated entry point to initiate a PAF for Non-Privileged Practitioners.

Today, users only have the Begin PAF option, which launches the standard practitioner workflow. To support NPP requests, a second option will be introduced that routes users into the existing Enforce NPI Search workflow currently used by PSG and Recruitment user groups.

This enhancement will leverage existing functionality, minimize new development, and preserve the current Begin PAF experience.

## 4. Acceptance Criteria

Not provided in the captured epic export. Feature/story AC lives on `136872` and its children.

## 5. Tasks

Not provided in the captured epic export.

## 6. Dependencies

- Successor `53658` depends on this epic being completed and deployed (stated on `53658`).
- Child feature `136872` and its stories implement the entry point.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Dedicated NPP entry point on MSP Dashboard.
- Route into existing Enforce NPI Search used by PSG and Recruitment.
- Preserve current Begin PAF experience.
- Minimize new development; reuse existing functionality.

**Existing UI behavior observed in screenshots**

- Current local MSP Dashboard shows only **Begin PAF** (`screenshots/Begin_PAF_Current/01_MSP_Dashboard_Begin_PAF.png`).
- Target mockup shows Begin PAF + Begin NPP PAF + tooltip (`screenshots/NPP_Search/01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png`).

**Inference**

- Child stories under `136872` are the implementation breakdown of this epic. Not a repository fact.

## 8. Screenshots / References

- [screenshots/Begin_PAF_Current](../../../screenshots/Begin_PAF_Current)
- [screenshots/NPP_Search](../../../screenshots/NPP_Search)

## 9. Implementation Investigation Questions

Do not answer until office source is inspected.

- Which dashboard page/component renders Begin PAF?
- How is MSP Dashboard authorization applied?
- Which route launches the existing Enforce NPI Search for PSG/Recruitment?
- How should Begin NPP PAF pass PAF type/context into that search?
- Which tests cover Begin PAF so they can prove no regression?

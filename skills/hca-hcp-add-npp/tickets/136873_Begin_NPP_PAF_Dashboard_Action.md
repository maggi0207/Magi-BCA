# 136873 — NPP: Add Begin NPP PAF action to MSP Dashboard

Full capture: [../04_User_Stories/136873_Begin_NPP_PAF_Dashboard_Action/136873.md](../04_User_Stories/136873_Begin_NPP_PAF_Dashboard_Action/136873.md)

## 1. Azure DevOps Metadata

- Ticket ID: 136873
- Work item type: User Story
- Title: NPP: Add Begin NPP PAF action to MSP Dashboard
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Ready for Dev
- State: To Do
- Tags: MSP Dashboard, NPP PAF, Sprint 7.0 2026, UI
- Parent: 136872
- Children: Not captured
- Predecessor: Not captured
- Successor: Not captured
- Related items: Not captured
- Assigned To: Bathula Mahendra

## 2. User Story

As an MSP user

I want a dedicated Begin NPP PAF option

So that I can initiate requests for Non-Privileged Practitioners.

## 3. Description

Add a new action/button labeled **Begin NPP PAF** immediately adjacent to the existing **Begin PAF** button.

The new control should match the application's existing styling and accessibility standards.

## 4. Acceptance Criteria

**Scenario 1 — Display**

Given the user is on the MSP Dashboard. When the page loads. Then both actions are displayed: Begin PAF and Begin NPP PAF.

**Scenario 2 — Layout**

Given both actions are displayed. Then **Begin NPP PAF** appears immediately adjacent to **Begin PAF**.

**Scenario 3 — Accessibility**

The new button must be keyboard accessible, screen reader compatible, and styled consistently with existing application controls.

This story does **not** define click routing (`136878`), tooltip text (`136876`), or authorization (`136883`).

## 5. Tasks

- Add new dashboard button
- Apply styling
- Add accessibility support
- UI regression testing

## 6. Dependencies

Parent `136872`. Related children of the same parent: `136882`, `136876`, `136878`, `136883`.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- New Begin NPP PAF control adjacent to Begin PAF; match styling and a11y.

**Existing UI behavior observed in screenshots**

- Current dashboard has only Begin PAF.
- Mockup shows both buttons adjacent, NPP button with info icon.

**Inference**

None as repository fact.

## 8. Screenshots / References

- [../screenshots/Begin_PAF_Current/01_MSP_Dashboard_Begin_PAF.png](../screenshots/Begin_PAF_Current/01_MSP_Dashboard_Begin_PAF.png)
- [../screenshots/NPP_Search/01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png](../screenshots/NPP_Search/01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png)

## 9. Implementation Investigation Questions

- Which existing dashboard component renders Begin PAF?
- What button/control pattern and a11y attributes does Begin PAF use?
- Where is the dashboard action list defined?
- Which UI tests cover MSP Dashboard actions?

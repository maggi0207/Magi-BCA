# 132256 — NPP: Display New PAF Action for MSP User - "ADD NPP to Facility"

Full capture: [../../../../04_User_Stories/132256_Display_PAF_Action_MSP/132256.md](../../../../04_User_Stories/132256_Display_PAF_Action_MSP/132256.md)

Parent feature: [132255.md](132255.md)

## 1. Azure DevOps Metadata

- Ticket ID: 132256
- Work item type: User Story
- Title: NPP: Display New PAF Action for MSP User - "ADD NPP to Facility"
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Ready for Dev
- State: To Do
- Tags: ADD NPP, NPP PAF, Sprint 7.0 2026, UI
- Parent: 132255 — NPP: Create "ADD NPP to Facility" PAF Action Option
- Project: 52350
- Children: Not captured
- Predecessor: Not captured
- Successor: Not captured
- Related items: Not captured

ADO title uses **Display** New PAF Action (not “Add New”).

## 2. User Story

As an MSP user, I want to see an **"ADD NPP to Facility"** PAF action option so that I can initiate facility assignment for eligible practitioners.

## 3. Description

Create a new radio-button option labeled **ADD NPP to Facility** within the **Practitioner Action** section.

## 4. Acceptance Criteria

**Scenario 1 — MSP User Access**

Given the user has the MSP role. When the Practitioner Action section is displayed. Then the **"ADD NPP to Facility"** radio button is visible.

**Scenario 2 — Non-MSP User Access**

Given the user does not have the MSP role. When the Practitioner Action section is displayed. Then the **"ADD NPP to Facility"** radio button is not visible.

**Scenario 3 — Placement of Option**

Given the Practitioner Action list is displayed. When the list loads. Then the **"ADD NPP to Facility"** option appears directly below the last existing PAF Action option.

**Exception handling (ticket text, not a numbered scenario)**

If role information cannot be retrieved: do not display **"ADD NPP to Facility"**; log role validation failures.

## 5. Tasks

- Add new radio button control.
- Implement MSP role validation.
- Position control according to UI requirements.
- Update UI styling and alignment.
- Unit testing.

## 6. Dependencies

- Parent `132255`.
- **132257** adds practitioner eligibility (this story is role visibility only).
- **132259** single-select; screenshot already uses radios.
- **132258** runs after the action is submitted.
- **136883** is dashboard/NPP search URL security, not this dialog.
- **131728** is NPP RBAC. Do not invent a policy name for “MSP role”.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Exact label **ADD NPP to Facility**.
- Existing Practitioner Action radio list.
- MSP on / non-MSP off.
- Last in the list (below the last existing action).
- Role lookup failure → hide + log.

**Existing UI behavior observed in screenshot**

- Dialog **PAF Action & Facilities**, left column **Practitioner Action**.
- Radios already present; **ADD NPP to Facility** shown at the bottom with a **New** badge.
- Badge is **UI Evidence only** — not in this AC.
- In that shot the option sits under **Add Privilege MSS-18**. Treat that as the current last existing action **in the screenshot**, not as AC naming that row.

**Inference (not repository fact)**

- Same dialog as Add Practitioner to Facility. Extension of the existing radio list, not a new page.

This story does **not** hide the option for ineligible practitioners; that is **132257**.

## 8. Screenshots / References

- [../../../../screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png](../../../../screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png)

## 9. Implementation Investigation Questions

Do not answer until office source is inspected.

- Which component renders Practitioner Action radios?
- Where is the action list defined (enum, markup, or config)?
- How is MSP role evaluated on this dialog today? Same helper as 136883 dashboard, or different?
- What happens today if role lookup fails?
- Which unit tests cover this radio list?
- Must other actions stay visible and styled unchanged?

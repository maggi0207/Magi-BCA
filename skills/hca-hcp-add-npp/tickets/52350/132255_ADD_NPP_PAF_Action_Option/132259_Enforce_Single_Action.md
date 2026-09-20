# 132259 — NPP: Enforce Single PAF Action Selection

Full capture: [../../../../04_User_Stories/132259_Enforce_Single_PAF_Action/132259.md](../../../../04_User_Stories/132259_Enforce_Single_PAF_Action/132259.md)

Parent feature: [132255.md](132255.md)

## 1. Azure DevOps Metadata

- Ticket ID: 132259
- Work item type: User Story
- Title: NPP: Enforce Single PAF Action Selection
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

## 2. User Story

As an MSP user, I want to select only one PAF action at a time so that conflicting requests cannot be submitted.

## 3. Description

The Practitioner Action list must **continue** single-selection **radio-button** behavior, including **ADD NPP to Facility**.

Adding the new action must not allow multiple PAF Actions to be selected at once.

## 4. Acceptance Criteria

**Scenario 1 — Select New Action**

Given no PAF Action is selected. When the user selects **"ADD NPP to Facility"**. Then it becomes the only selected option.

**Scenario 2 — Switch Between Actions**

Given another PAF Action is selected. When the user selects **"ADD NPP to Facility"**. Then the previously selected action is deselected.

**Scenario 3 — Existing Behavior Maintained**

Given multiple PAF Actions exist. When a user makes a selection. Then only one action can remain selected at any time.

**Exception handling (ticket text, not a numbered scenario)**

Prevent submission if multiple selections are detected due to UI or browser issues. Log selection validation errors.

## 5. Tasks

- Verify radio-button grouping.
- Validate client-side selection logic.
- Validate server-side submission logic.
- Perform regression testing.

## 6. Dependencies

- Parent `132255`.
- **132256** adds the radio that must join the existing group.
- **132257** eligibility is separate (visibility), not multi-select.
- **132258** runs after a single valid submit.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Keep radio single-select, including ADD NPP.
- Switching deselects the previous action.
- Other actions keep the same one-at-a-time behavior.
- Client **and** server must not accept multiple selections.
- Log validation errors.

**Existing UI behavior observed in screenshot**

- Practitioner Action is already a radio list. UI Evidence that grouping likely exists. This story still requires **verify grouping** plus a **server-side** guard — not “UI only.”

**Inference (not repository fact)**

- Implementation may be mostly grouping the new radio with existing ones, plus a submit check if the payload can be tampered. Confirm in office code. Do not invent the endpoint.

## 8. Screenshots / References

- [../../../../screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png](../../../../screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png)

## 9. Implementation Investigation Questions

Do not answer until office source is inspected.

- Are Practitioner Action options already one radio group?
- What is posted on Next/submit (single action value vs list)?
- Is there already server validation that only one action is selected?
- Where would a multi-select be logged and rejected?
- Which tests cover switching between Add Practitioner to Facility and other actions?

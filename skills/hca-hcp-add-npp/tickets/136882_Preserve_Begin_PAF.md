# 136882 — NPP: Preserve existing Begin PAF functionality

Full capture: [../04_User_Stories/136882_Preserve_Begin_PAF/136882.md](../04_User_Stories/136882_Preserve_Begin_PAF/136882.md)

## 1. Azure DevOps Metadata

- Ticket ID: 136882
- Work item type: User Story
- Title: NPP: Preserve existing Begin PAF functionality
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

**Bathula Mahendra** is the assignee, not a technical term.

## 2. User Story

As an MSP user

I want the existing **Begin PAF** process to remain unchanged

So that standard practitioner requests continue to function normally.

## 3. Description

Ensure the addition of **Begin NPP PAF** has no impact on the existing PAF initiation workflow.

## 4. Acceptance Criteria

**Scenario 1**

Given the existing MSP Dashboard is displayed. When the user selects **Begin PAF**. Then the current Begin PAF workflow is launched.

**Scenario 2**

Existing practitioner search options remain unchanged.

**Scenario 3**

Existing validations remain unchanged.

**Scenario 4**

Regression testing confirms there are no unintended changes.

## 5. Tasks

- Regression testing
- Functional validation
- UAT support

## 6. Dependencies

Parent `136872` introduces Begin NPP PAF. This story constrains that change.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Begin PAF, its search options, and its validations must remain the current workflow.

**Existing UI behavior observed in screenshots**

- Dashboard Begin PAF button.
- Begin PAF search: NPI, SSN, First Name, Last Name.
- No results + Add New Practitioner.
- Add New Practitioner modal with required DOB/Gender/SSN/Email/Cell and Verify & Start PAF.

**Inference**

- That Begin PAF search is not the Enforce NPI Search screen. Confirmed as two different UIs in screenshots; routes/classes still unknown.

## 8. Screenshots / References

- [../screenshots/Begin_PAF_Current](../screenshots/Begin_PAF_Current)

## 9. Implementation Investigation Questions

- Which handler runs when Begin PAF is clicked?
- Which search page does Begin PAF open?
- Which tests lock current Begin PAF search and validation behavior?
- What must not change when adding Begin NPP PAF beside it?

# 132258 — NPP: Apply Existing PAF Task Determination Rules

Full capture: [../../../../04_User_Stories/132258_Apply_Task_Determination/132258.md](../../../../04_User_Stories/132258_Apply_Task_Determination/132258.md)

Parent feature: [132255.md](132255.md)

## 1. Azure DevOps Metadata

- Ticket ID: 132258
- Work item type: User Story
- Title: NPP: Apply Existing PAF Task Determination Rules
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Ready for Dev
- State: To Do
- Tags: Eligibility, NPP PAF, Sprint 7.0 2026
- Parent: 132255 — NPP: Create "ADD NPP to Facility" PAF Action Option
- Project: 52350
- Children: Not captured
- Predecessor: Not captured
- Successor: Not captured
- Related items: Not captured

## 2. User Story

As an MSP user, I want the system to use existing PAF task determination rules so that facility additions follow established workflows.

## 3. Description

When **"ADD NPP to Facility"** is selected, run the **same** task determination logic already used for new practitioners and for existing practitioners.

Reuse the existing PAF task determination engine. Do **not** create a separate NPP-specific determination mechanism.

## 4. Acceptance Criteria

**Scenario 1 — Net New Practitioner Task Generation**

Given a Net New Practitioner is selected. When the user submits **"ADD NPP to Facility"**. Then the system creates tasks using the existing **New Practitioner PAF determination rules**.

**Scenario 2 — Existing Inactive Practitioner Task Generation**

Given an Existing Inactive Practitioner is selected. When the user submits **"ADD NPP to Facility"**. Then the system creates tasks using the existing **Existing Practitioner PAF determination rules**.

**Scenario 3 — Rule Processing Success**

Given valid practitioner data exists. When task determination executes. Then all applicable workflow tasks are generated successfully.

**Exception handling (ticket text, not a numbered scenario)**

If task determination fails: display an error; prevent submission; log workflow processing failures; prevent creation of partial task sets.

## 5. Tasks

- Reuse existing task determination engine.
- Map the new action type to existing workflows.
- Validate task creation.
- Perform regression testing.

## 6. Dependencies

- Parent `132255`. Sibling display `132256` should land first so the action exists.
- Eligibility `132257` defines who can select the action; this story runs **after** submit of that action.
- **132275** still owns NPP-specific task cards / suppressions. This story maps the action onto **existing** New vs Existing rules, not a new NPP engine.
- Do not treat this as `131783` (CPC type filter) or `136883` (entry security).

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- One engine, two existing rule sets: New Practitioner vs Existing Practitioner.
- ADD NPP action is mapped onto those workflows.
- Failure is hard-stop: error, no submit, log, no partial tasks.

**Existing UI behavior**

- PAF Action & Facilities screenshot is on the parent. This story is post-**Next**/submit determination, not the radio label.

**Inference (not repository fact)**

- “Map the new action type” likely means the selected Practitioner Action chooses which existing determination path runs. It does **not** prove a new `PafType` or `TaskType.Npp` value.

The ADO paste’s flowchart stopped after Net New. Existing Inactive flow is Scenario 2 only, not extra pasted diagram text.

## 8. Screenshots / References

- [../../../../screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png](../../../../screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png) (parent dialog; not determination output)

## 9. Implementation Investigation Questions

Do not answer until office source is inspected.

- Where do New Practitioner PAF determination rules run today?
- Where do Existing Practitioner PAF determination rules run today?
- Where does the selected PAF Action choose between those paths?
- How are task sets created atomically (no partial set)?
- How are determination failures shown and logged today?
- Which tests lock current New vs Existing task generation so ADD NPP mapping does not change other actions?

# 136872 â€” NPP: Add "Begin NPP PAF" Entry Point on MSP Dashboard

Full capture: [../../../../03_Features/136872_Begin_NPP_PAF_Entry_Point/136872.md](../../../../03_Features/136872_Begin_NPP_PAF_Entry_Point/136872.md)

Note: Preserve Begin PAF is ticket **136882**, not 136872. See [136882_Preserve_Begin_PAF.md](136882_Preserve_Begin_PAF.md).

## 1. Azure DevOps Metadata

- Ticket ID: 136872
- Work item type: Feature
- Title: NPP: Add "Begin NPP PAF" Entry Point on MSP Dashboard
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Products Epics Features
- State: To Do
- Tags: NPI Search, NPP PAF, Routing, Sprint 7.0 2026, UI
- Parent: 136871
- Children: 136882, 136873, 136878, 136876, 136883
- Predecessor: Not captured
- Successor: Not captured on this ticket (epic successor is 53658)
- Related items: 144239 (Related â€” not a dependency)
- Priority: Not shown in captured ticket details

## 2. User Story

Feature description (not a classic â€œAs aâ€¦â€ on this ticket):

Enhance the MSP Dashboard by adding **Begin NPP PAF** adjacent to existing **Begin PAF**. Selection directs users to existing **Enforce NPI Search**. Existing Begin PAF remains unchanged. Hover tooltip explains when to use the option.

Dashboard button layout/accessibility details are specified on **136873**.

## 3. Description

Enhance the MSP Dashboard by adding a new action labeled **Begin NPP PAF** adjacent to the existing **Begin PAF** button.

When selected, users will be directed to the existing **Enforce NPI Search** workflow, enabling NPI-based practitioner searches for Non-Privileged Practitioner requests.

The existing **Begin PAF** workflow will remain unchanged.

An informational tooltip will be displayed when users hover over the new action to ensure they understand when the option should be used.

## 4. Acceptance Criteria

Captured on this feature:

- New **Begin NPP PAF** action is available on the MSP Dashboard.
- Existing **Begin PAF** functionality is unchanged.
- **Begin NPP PAF** routes users into the existing **Enforce NPI Search** workflow.
- Hover tooltip is displayed.
- Existing security model is maintained.
- Regression testing confirms no impact to current functionality.

Layout/a11y (adjacent, keyboard, screen reader, styling) are captured on **136873**, not restated here as if they were 136872-only AC.

## 5. Tasks

Not listed as development tasks on this feature export. Child stories own tasks.

## 6. Dependencies

Implemented through children: 136873 (button), 136876 (tooltip), 136878 (routing), 136882 (Begin PAF unchanged), 136883 (security).

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Second dashboard action, reuse Enforce NPI Search, preserve Begin PAF, tooltip, existing security.

**Existing UI behavior observed in screenshots**

- Current dashboard: Begin PAF only.
- Mockup: both buttons + tooltip.

**Inference**

None as repository fact.

## 8. Screenshots / References

- [../../../../screenshots/Begin_PAF_Current/01_MSP_Dashboard_Begin_PAF.png](../../../../screenshots/Begin_PAF_Current/01_MSP_Dashboard_Begin_PAF.png)
- [../../../../screenshots/NPP_Search/01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png](../../../../screenshots/NPP_Search/01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png)

## 9. Implementation Investigation Questions

- Which dashboard component renders Begin PAF?
- How are dashboard actions registered?
- What context is passed when launching Enforce NPI Search?
- How is existing MSP security applied to dashboard actions?

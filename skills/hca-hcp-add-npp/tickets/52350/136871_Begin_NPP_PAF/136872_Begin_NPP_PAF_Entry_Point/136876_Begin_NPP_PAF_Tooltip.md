# 136876 â€” NPP: Provide user guidance through tooltip

Full capture: [../../../../04_User_Stories/136876_NPP_Tooltip/136876.md](../../../../04_User_Stories/136876_NPP_Tooltip/136876.md)

## 1. Azure DevOps Metadata

- Ticket ID: 136876
- Work item type: User Story
- Title: NPP: Provide user guidance through tooltip
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Ready for Dev
- State: To Do
- Tags: MSP Dashboard, NPP PAF, Sprint 7.0 2026, Tool Tip, UI
- Parent: 136872
- Children: Not captured
- Predecessor: Not captured
- Successor: Not captured
- Related items: Not captured
- Assigned To: Bathula Mahendra

## 2. User Story

As an MSP user

I want guidance regarding when to use **Begin NPP PAF**

So that I select the correct workflow.

## 3. Description

Display an informational tooltip when the user hovers over the **Begin NPP PAF** button.

## 4. Acceptance Criteria

**Approved tooltip text (ticket):**

> Use this option only when submitting a request for a Non-Privileged Practitioner (NPP).

- Tooltip appears when the user hovers over Begin NPP PAF.
- Tooltip contains the approved instructional text.
- Tooltip disappears when hover/focus ends.

## 5. Tasks

- Create tooltip component
- Apply approved messaging
- Cross-browser validation

## 6. Dependencies

Parent `136872`. Button placement `136873`.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Hover/focus tooltip with the approved sentence above.

**Existing UI behavior observed in screenshots**

- Mockup shows tooltip on the Begin NPP PAF info icon.
- Mockup also shows extra sentence: â€œSelecting this option will launch the NPI Search workflow.â€

**Inference**

- Extra mockup sentence is **not** the approved ticket text unless product confirms it. Implement the approved sentence.

## 8. Screenshots / References

- [../../../../screenshots/NPP_Search/01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png](../../../../screenshots/NPP_Search/01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png)

## 9. Implementation Investigation Questions

- What existing tooltip / info-icon pattern does the dashboard use?
- Is hover and keyboard focus already supported on similar controls?
- Where should the tooltip attach relative to Begin NPP PAF?

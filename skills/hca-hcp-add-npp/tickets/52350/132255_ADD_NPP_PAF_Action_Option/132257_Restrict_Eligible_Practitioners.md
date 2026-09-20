# 132257 — NPP: Restrict Action to Eligible Practitioners

Full capture: [../../../../04_User_Stories/132257_Restrict_Eligible_Practitioners/132257.md](../../../../04_User_Stories/132257_Restrict_Eligible_Practitioners/132257.md)

Parent feature: [132255.md](132255.md)

## 1. Azure DevOps Metadata

- Ticket ID: 132257
- Work item type: User Story
- Title: NPP: Restrict Action to Eligible Practitioners
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Ready for Dev
- State: To Do
- Tags: ADD NPP, Eligibility, NPP PAF, Sprint 7.0 2026
- Parent: 132255 — NPP: Create "ADD NPP to Facility" PAF Action Option
- Project: 52350
- Children: Not captured
- Predecessor: Not captured
- Successor: Not captured
- Related items: Not captured

## 2. User Story

As an MSP user, I want the **"ADD NPP to Facility"** option available only for eligible practitioners so that invalid facility assignments cannot be initiated.

## 3. Description

Show **"ADD NPP to Facility"** only when the practitioner is:

- a **Net New Practitioner**, or
- an **Existing Practitioner with an inactive relationship to the selected entity**

Do not display it when eligibility is not met.

## 4. Acceptance Criteria

**Scenario 1 — Net New Practitioner**

Given the practitioner is identified as Net New. When the Practitioner Action section loads. Then **"ADD NPP to Facility"** is displayed.

**Scenario 2 — Existing Inactive Practitioner**

Given the practitioner exists in the system and is inactive with the entity. When the Practitioner Action section loads. Then **"ADD NPP to Facility"** is displayed.

**Scenario 3 — Active Practitioner**

Given the practitioner is active with the entity. When the Practitioner Action section loads. Then **"ADD NPP to Facility"** is not displayed.

**Scenario 4 — Ineligible Practitioner Status**

Given the practitioner does not meet eligibility requirements. When the Practitioner Action section loads. Then the option is hidden.

**Eligibility logic (from ticket)**

```text
                    ┌── Net New Practitioner ───────────→ SHOW
                    │
Practitioner ───────┤
                    │
                    └── Existing + Inactive with Entity → SHOW
                    │
                    └── Existing + Active with Entity ─→ HIDE
                    │
                    └── Other Ineligible Status ───────→ HIDE
```

## 5. Tasks

Not provided in the captured paste.

## 6. Dependencies

- Parent `132255`. **132256** owns MSP role visibility; this story owns practitioner eligibility. Both hide/show the same radio.
- **132258** uses net new vs existing inactive **after** submit; this story gates whether the action is offered.
- **132259** is single-select of whatever is visible.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- SHOW: net new, or existing inactive with entity.
- HIDE: existing active with entity, or any other ineligible status.
- Evaluated when Practitioner Action **loads**.

**Existing UI behavior**

- Screenshot shows the radio in the list. It does **not** prove eligibility logic. Facilities on the right are UI Evidence of an entity list; tying eligibility to a checked facility is **Inference** until office code confirms. Parent 132255 said **selected entity**.

**Unknown (do not invent)**

- Net-new flag / add-new path.
- Inactive vs active relationship field.
- Which entity is “the entity” at load time if no facility is checked yet.

**Combined with 132256 (not extra AC)**

MSP role known **and** eligible → show. Otherwise hide. Role lookup failure already hides (132256).

## 8. Screenshots / References

- [../../../../screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png](../../../../screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png)

## 9. Implementation Investigation Questions

Do not answer until office source is inspected.

- How is Net New vs existing known on PAF Action & Facilities?
- Where is practitioner–entity active/inactive stored?
- Is eligibility computed at dialog open, or after a facility is selected?
- Do any existing Practitioner Actions already hide by practitioner/entity status?
- Which tests cover action-list visibility by practitioner state?

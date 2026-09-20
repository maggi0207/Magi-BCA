# 136878 — NPP: Launch existing Enforce NPI Search workflow

Full capture: [../04_User_Stories/136878_Enforce_NPI_Search/136878.md](../04_User_Stories/136878_Enforce_NPI_Search/136878.md)

## 1. Azure DevOps Metadata

- Ticket ID: 136878
- Work item type: User Story
- Title: NPP: Launch existing Enforce NPI Search workflow
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Ready for Dev
- State: To Do
- Tags: MSP Dashboard, NPI Search, NPP PAF, Sprint 7.0 2026, UI
- Parent: 136872
- Children: Not captured
- Predecessor: Not captured
- Successor: Not captured on this story (epic successor is 53658)
- Related items: Not captured
- Assigned To: Bathula Mahendra

## 2. User Story

As an MSP user

I want **Begin NPP PAF** to launch the **NPI Search** workflow

So that I can search using NPI for Non-Privileged Practitioner requests.

## 3. Description

Selecting **Begin NPP PAF** should navigate users into the existing **Enforce NPI Search** workflow currently available for PSG and Recruitment user groups.

The solution should reuse the existing implementation rather than creating a new workflow.

## 4. Acceptance Criteria

- Selecting Begin NPP PAF displays the Enforce NPI Search screen.
- Existing business rules, validations, and search functionality are reused.
- No duplicate workflow is introduced.
- Users can continue through the existing NPI Search process without modification.

## 5. Tasks

- Wire dashboard button to routing
- Reuse existing workflow
- Validate search behavior
- Integration testing

## 6. Dependencies

Parent `136872`. Button `136873`. Authorization `136883`. Later search feature `53658` / `127104` is blocked on epic `136871`.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Reuse existing Enforce NPI Search. Do not create a second search workflow.

**Existing UI behavior observed in screenshots**

- Begin PAF search (`screenshots/Begin_PAF_Current`) is NPI + SSN + name — **different** screen.
- NPI-required search (`screenshots/NPP_Search/01_Enforce_NPI_Search_NPI_Required.png`) has NPI * and “No NPI available? Search without NPI”.
- Chat note that PSG has this search and it may be used for MSP — supporting context only.

**Inference**

- The NPI-required screenshot is likely the Enforce NPI Search UI. That is UI evidence, not a confirmed class/route until office code is inspected.

## 8. Screenshots / References

- [../screenshots/Begin_PAF_Current](../screenshots/Begin_PAF_Current)
- [../screenshots/NPP_Search](../screenshots/NPP_Search)

## 9. Implementation Investigation Questions

- Which route launches Enforce NPI Search for PSG/Recruitment?
- Which component renders that search?
- How do those roles navigate to it today?
- How does Begin PAF launch its different search, so it is not reused by mistake?
- What PAF type/context must be passed into the existing search?

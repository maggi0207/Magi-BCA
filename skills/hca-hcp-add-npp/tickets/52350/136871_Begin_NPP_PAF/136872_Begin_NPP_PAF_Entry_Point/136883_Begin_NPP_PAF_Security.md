# 136883 â€” NPP: Validate security and authorization for Begin NPP PAF

Full capture: [../../../../04_User_Stories/136883_Security_Authorization/136883.md](../../../../04_User_Stories/136883_Security_Authorization/136883.md)

## 1. Azure DevOps Metadata

- Ticket ID: 136883
- Work item type: User Story
- Title: NPP: Validate security and authorization for Begin NPP PAF
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Ready for Dev
- State: To Do
- Tags: MSP Dashboard, NPI Search, NPP PAF, Sprint 7.0 2026, UI
- Parent: 136872
- Children: Not captured
- Predecessor: Not captured
- Successor: Not captured
- Related items: Not captured
- Assigned To: Bathula Mahendra

## 2. User Story

As a System Administrator

I want only authorized MSP users to access the **Begin NPP PAF** workflow

So that unauthorized users cannot initiate NPP requests.

## 3. Description

The new entry point must follow the existing MSP security model and enforce authorization before allowing access to the **Enforce NPI Search** workflow.

## 4. Acceptance Criteria

- Authorized MSP users can access **Begin NPP PAF**.
- Unauthorized users cannot access the workflow through the UI or direct URL.
- Existing PSG and Recruitment access is unaffected.
- Security and session management remain consistent with current application standards.

Hiding the button is not enough. Direct URL must also be blocked.

## 5. Tasks

- Validate role-based access
- Test direct URL access
- Verify authorization logic
- Security regression testing

## 6. Dependencies

Parent `136872`. Button `136873`. Route `136878`. Begin PAF unchanged `136882`.

Project-level RBAC ticket `131728` is title-only and must not be treated as this storyâ€™s AC.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Reuse existing MSP security model. Authorize before Enforce NPI Search. Do not affect PSG/Recruitment access.

**Existing UI behavior observed in screenshots**

- None that prove authorization.

**Inference**

- Exact role claims, URL, and access-denied page are unknown. Do not invent an auth mechanism.

## 8. Screenshots / References

None specific to authorization.

## 9. Implementation Investigation Questions

- Where is MSP authorization enforced for Begin PAF?
- Which roles/claims identify MSP vs PSG vs Recruitment?
- Which route is Enforce NPI Search, and is it already authorized?
- How are unauthorized UI vs direct URL cases tested today?

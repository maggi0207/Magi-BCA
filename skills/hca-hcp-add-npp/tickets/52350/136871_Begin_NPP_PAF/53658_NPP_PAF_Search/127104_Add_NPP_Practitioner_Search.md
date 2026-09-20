# 127104 â€” NPP: Add NPP to Facility PAF Practitioner Search â€“ Add New Practitioner

Full capture: [../../../../04_User_Stories/127104_NPP_Practitioner_Search/127104.md](../../../../04_User_Stories/127104_NPP_Practitioner_Search/127104.md)

## 1. Azure DevOps Metadata

- Ticket ID: 127104
- Work item type: User Story (also shown as New Feature on the captured ticket)
- Title: NPP: Add NPP to Facility PAF Practitioner Search â€“ Add New Practitioner
- Area: Parallon-Credentialing
- Iteration: Parallon-Credentialing\Ready for Dev
- State: To Do
- Tags: ADD NPP, HCP, NPP PAF, PAF, Practitioner Search, Sprint 7.0 2026
- Parent: 53658
- Children: Not captured
- Predecessor: 136871 â†’ 53658 â†’ 127104
- Successor: Not captured
- Related items: Not captured
- Assigned To: Bathula Mahendra
- Business Line: CPC

## 2. User Story

Not written as a classic â€œAs aâ€¦â€ on the captured story. Intent: Facility PAF Practitioner Search must leverage the existing Add New Practitioner search workflow used by other PAF types.

## 3. Description

Enable Facility PAF Practitioner Search to reuse existing Add New Practitioner search:

- NPI is the primary search criterion.
- Valid 10-digit NPI required for standard search.
- Controlled exception when NPI is unavailable.
- Minimum alternate criteria for exception searches.
- Audit log exception searches.
- Reuse existing search criteria and validation.
- Preserve existing business rules.

**Architectural statement from the ticket:**

> No new NPI search logic is being introduced.

## 4. Acceptance Criteria

- Standard Add New Practitioner search workflow is reused (same criteria, validation, workflow as other PAF types).
- Standard search is blocked when NPI is missing; user is prompted for a valid NPI or the exception workflow.
- NPI that is not exactly 10 numeric digits is rejected with a validation error; search does not execute.
- Valid 10-digit NPI executes search and returns matching results.
- Exception workflow is available when NPI is unavailable.
- Exception search without minimum criteria is blocked with a validation message.
- Exception searches are audit logged using existing audit standards.
- Behavior is consistent across PAF types.

Out of scope: changes to NPI validation requirements; changes to audit architecture/schema.

## 5. Tasks

Development: reuse existing Add New Practitioner search; verify NPI-required and 10-digit validation; enable exception workflow; ensure exception audit logging; align UI labels/messages.

QA: standard NPI, invalid NPI, exception workflow, minimum criteria, audit, no-results, existing search unchanged.

## 6. Dependencies

Parent `53658`, which is blocked on epic `136871`. Reuse existing Add New Practitioner search used by other PAF types. `131785` is a separate title-only no-NPI ticket.

## 7. UI / Architecture Notes

**Explicit ticket requirement**

- Integration/reuse. Do not create Facility-specific NPI search, a second validation implementation, a new exception architecture, or a new audit schema.

**Existing UI behavior observed in screenshots**

- NPI * required; Search; **No NPI available? Search without NPI**; results grid.
- Help text: exception search with Last Name and at least one of First Name, DOB, SSN, or Email.
- Chat: PSG has this search; consider same for MSP (supporting context only).

**Inference**

- NPP V5 exception fields (Reason, First Name, Last Name, State License Number, State of License) differ from that UI help text. Ticket 127104 says reuse existing criteria. Do not silently replace existing exception fields with V5.

## 8. Screenshots / References

- [../../../../screenshots/NPP_Search/01_Enforce_NPI_Search_NPI_Required.png](../../../../screenshots/NPP_Search/01_Enforce_NPI_Search_NPI_Required.png)
- [../../../../screenshots/NPP_Search/02_PSG_Search_Reuse_Chat.png](../../../../screenshots/NPP_Search/02_PSG_Search_Reuse_Chat.png)

## 9. Implementation Investigation Questions

- Where is the existing Add New Practitioner search implemented?
- Which PAF types currently use it?
- Which UI component, service, and API perform the search?
- Where is 10-digit NPI validation implemented?
- Where is the no-NPI exception workflow, and what are the actual minimum criteria?
- Where is exception-search audit logging implemented?
- How is Facility PAF currently routed to practitioner search?
- Can the existing search component be reused directly?
- What tests already cover this, and what Begin PAF regression risk exists?

Do not generate a Copilot prompt until office files answering these are supplied.

# ADD NPP to Facility — Project Knowledge & Implementation Guide

## 1. Purpose

This document consolidates the available business and architecture context for the HCP **ADD NPP to Facility** feature.

**Primary business source of truth:** HCP NPP PAF Type Requirements V5 – 06/2026 – Credentialing Product Development.

Use this document as a working implementation guide. It is not a replacement for the approved business requirements.

---

## 2. Business Context

The feature introduces a new PAF type:

**ADD NPP to Facility**

The business goal is to allow an MSP user to submit a Non-Privileged Practitioner (NPP) request when the facility is responsible for performing the required verification activities instead of CPC performing the normal NPP verification process.

Normal ADD NPP submissions are intended to be auto-accepted and result in CACTUS updates. Defined exception scenarios are routed to CPC for manual processing.

ADD NPP should not automatically be treated as a normal privileged-practitioner credentialing workflow.

---

## 3. Important Terms

### HCP
The application/workflow platform used by MSP users to search practitioners, initiate PAFs, complete task cards, upload documents, validate information, and submit requests.

### MSP
Medical Staff Professional. The user role responsible for submitting ADD NPP PAF requests.

### CPC
Centralized credentialing/processing team. CPC handles defined exception/manual-processing scenarios.

### CACTUS
The credentialing database/system where practitioner, facility assignment, address, specialty, license, NPI, and sanctions-related records are created or updated.

### PAF
Provider Action Form. The workflow/request type used to perform provider-related actions.

### NPP
Non-Privileged Practitioner.

### PSV
Primary Source Verification evidence/documentation.

### CVI
Credential Verification Issue used for CPC processing when an ADD NPP request meets defined exception criteria.

### Net New Practitioner
A practitioner who does not currently reside in CACTUS.

### Existing Active Practitioner
A practitioner in CACTUS with at least one active facility where the practitioner holds privileges.

### Existing Inactive Practitioner
A practitioner in CACTUS with no active facilities.

---

## 4. High-Level ADD NPP Flow

```text
MSP
 |
 v
Practitioner Search
 |
 +--> Net New
 |
 +--> Existing Inactive
 |
 v
ADD NPP to Facility PAF
 |
 +--> Demographic
 +--> Manage Addresses
 +--> Specialties
 +--> ADD NPP to Facility
 |
 v
License + PSV Validation
 |
 v
Submit
 |
 +-------------------------+
 |                         |
 v                         v
Normal                    Exception
 |                         |
 v                         v
Auto-Accept               Auto-Accept
 |                         |
 v                         v
CACTUS Updates             CVI
 |                         |
 v                         v
Completed Queue           CPC Processing
```

The normal flow is intended to be automated. Exception flows are explicitly defined by the requirements.

---

## 5. Existing HCP Architecture Context

The existing HCP system uses an MSP/PSG-driven PAF flow.

Conceptually:

```text
MSP / PSG
    |
    v
Create PAF
    |
    v
Search Practitioner
    |
    +--> Existing
    |
    +--> New
    |
    v
Select Facility
    |
    v
Complete PAF Tasks
    |
    v
Review & Submit
    |
    v
CPC
    |
    v
Existing Credentialing / Packet Processing
```

The existing walkthrough explains that facility selection is important to downstream processing. Existing packet behavior can depend on facility/state/entity, credentialing type, packet type, anchor packet rules, and resource configuration.

**Important:** this existing packet/credentialing behavior must not automatically be applied to ADD NPP. The ADD NPP requirements explicitly state that ADD NPP is not part of normal packet determination, packet creation, or PAF merging except where the requirements explicitly require CPC processing behavior.

---

# 6. Repository Analysis Strategy

Before changing code:

1. Locate the PAF framework.
2. Locate PAF type definitions.
3. Locate PAF action definitions.
4. Locate task/card configuration.
5. Locate practitioner action selection.
6. Locate routing.
7. Locate submission.
8. Locate completion.
9. Locate PDF generation.
10. Locate PAF history.
11. Locate auto-acceptance.
12. Locate the existing **Add Practitioner to Facility** implementation.
13. Compare that implementation against ADD NPP requirements.
14. Reuse existing patterns where appropriate.

Do not immediately create a new architecture.

---

# 7. MSP Authorization and Eligibility

ADD NPP to Facility must be:

- Available only to MSP users.
- Selectable as one PAF action at a time.
- Available only for:
  - Net-new practitioners, or
  - Existing practitioners who are inactive with the entity.

When analyzing code, identify:

- Role/permission checks.
- Practitioner eligibility checks.
- PAF action availability.
- Practitioner status determination.

---

# 8. Practitioner Search

## Normal NPI Search

NPI is the primary and required search criterion.

Requirements:

- Valid 10-digit NPI.
- NPI validation before normal search.
- Duplicate checking against CACTUS records.

## No-NPI Search

The UI must provide:

**“No NPI available? Search without NPI”**

Required fields:

- Reason
- First Name
- Last Name
- State License Number
- State of License

Validation:

- Reason selected.
- First name contains at least 1 alpha character.
- Last name contains at least 2 alpha characters.
- State selected.
- State license number has at least 2 characters.

License lookup must support the normalized/like matching behavior specified by the requirements.

Trace:

```text
UI
 -> Validation Schema
 -> API
 -> Search Service
 -> Lookup Source
 -> Audit Logging
```

---

# 9. Practitioner Type Branching

## Net New

A practitioner who does not exist in CACTUS.

Required task cards:

- Demographic
- Manage Addresses
- Specialties
- ADD NPP to Facility

## Existing

For an existing practitioner:

- Demographic: Optional
- Manage Addresses: Optional
- Specialties: Optional unless no primary specialty exists
- ADD NPP to Facility: Required

Delegate and Facility Specific Questions cards must be suppressed for ADD NPP.

---

# 10. ADD NPP Task Card

The task card must:

- Be titled **ADD NPP to Facility**
- Be visible in PAF Tasks.
- Be Required.
- Have a Work action.
- Open the ADD NPP workflow.
- Prevent Review & Submit while required work remains incomplete.

Expected description:

> Upload and complete required Non-Privileged Practitioner (NPP) verification information and supporting PSV documentation.

Trace:

```text
Task Registration
      |
      v
Task Visibility
      |
      v
Requiredness
      |
      v
Completion State
      |
      v
Navigation
      |
      v
Submit Gating
```

---

# 11. Demographic Rules

## ADD NPP Fields

Optional:

- Email
- Cell
- DOB/SSN
- Gender

Required:

- First Name
- Last Name
- Degree
- Provider Category
- Individual NPI

For existing practitioners:

- Existing email is read-only.
- Blank email is editable and optional.
- Email format validation applies only when a value is entered.

If CACTUS data exists, demographic values should prepopulate from corresponding provider fields.

Existing values should not be editable unless explicitly permitted by the requirement.

---

# 12. Address Rules

Do not show:

- Home address
- Credentialing address
- Alternate address

## Active Primary Address + Active Facility Affiliation

Show the address and disable Add Address.

Expected message:

> The practitioner currently holds privileges at one or more HCA facilities. The primary address cannot be changed.

## No Primary Address

Allow Add Address.

## Active Primary Address but No Other Active Affiliations

Allow Add Address.

Required address fields:

- Address
- City
- State
- Zip Code
- Country
- Phone
- Fax

Optional:

- Contact
- Address Line 2
- Phone Extension
- Fax Extension

---

# 13. Specialty Rules

## Net New

Specialty is required.

## Existing

Specialty is optional unless:

- No primary specialty exists, or
- Existing primary specialty is inactive.

If an active primary specialty exists:

- Do not allow editing.
- Show the required explanatory hover behavior.

Secondary and alternate specialties must not be displayed.

## Professional Practice Interest

If the user cannot find an appropriate specialty, provide a professional practice interest/focus comment field.

At least one is required:

- Primary specialty
OR
- Professional practice interest/focus

Expected validation:

> A primary specialty or professional practice interest or focus is required. Please provide either a primary specialty or a professional practice interest or focus.

---

# 14. ADD NPP Business Workflow

The ADD NPP card communicates that the MSP is adding an NPP and uploading PSV evidence.

## Practitioner Type

- NPP only.
- Required.
- User cannot select another practitioner type.

Then ask:

**“Is the practitioner an active duty military member or practicing at the VA?”**

---

# 15. VA / PA Branching

## VA = YES

Ask:

**“Is the Practitioner a PA?”**

### VA = YES + PA = YES

Requirements:

- Hide State License section.
- Require Sanctions PSV.
- Require NPI PSV.
- Route to CPC.
- Follow specified CPC-processing behavior.

### VA = YES + PA = NO

Require:

- State License
- License PSV
- Sanctions PSV
- NPI PSV

## VA = NO

Skip the PA question.

Automatically show State License.

Require:

- State License
- License PSV
- Sanctions PSV
- NPI PSV

---

# 16. Existing Practitioner License Workflow

For an existing practitioner:

- Recall active/existing licenses.
- Display State.
- Display License Number.
- Display Expiration Date.
- Allow selection.

Provide:

**“None of these apply. Add New State License Instead.”**

For a new license:

- Compare state + license number against existing displayed licenses.
- Do not create duplicates.
- Show the defined duplicate message.
- Return to existing license selection where specified.

Existing recalled licenses can bypass standard validation rules where the requirements permit it.

If selected license is inactive or expired/matured:

- Route to CPC.
- Display the defined informational message.

---

# 17. State License Validation

License fields:

- State
- Effective Date
- License Number
- Status
- Expiration Date
- Field of Licensure

For facilities/entities in:

- CA
- LA
- NV
- KY
- TX

The practitioner license state must match the entity/facility state.

If it does not match, block submission.

Expected error:

> A state license in the state of ‘XX’ is required for PAF submission.

Trace:

```text
Facility / Entity
      |
      v
Entity State
      |
      +------ Compare ------+
                             |
                       License State
                             |
                   +---------+---------+
                   |                   |
                 Match              Mismatch
                   |                   |
                   v                   v
                Allow              Block
```

---

# 18. License Status and Routing

Defined STATUS_RTK values include:

- Active
- Temporary Permit
- Active Military
- Active-Compact

Routing requirements include:

- PA = YES → CPC
- License status not Active → CPC
- Expired/matured license → CPC
- Net-new licenses → normal validation
- Existing recalled licenses → existing-practitioner rules

Do not assume every non-Active status has identical handling. Verify each exact business rule.

---

# 19. PSV Uploads

License PSV is required wherever the workflow requires a license.

Accepted upload types:

- DOC
- DOCX
- PDF
- JPG
- TIFF

System responsibilities:

- Reject unsupported types.
- Convert/store as PDF/HTML as required.
- Attach image to appropriate CACTUS record.
- Populate required image metadata.
- Audit image creation.

Trace separately:

- License PSV
- NPI PSV
- Sanctions PSV

---

# 20. Auto-Acceptance

Normal ADD NPP submissions should be auto-accepted.

After acceptance:

- Perform CACTUS updates.
- Move PAF to Completed queue.
- PAF history uses HCP System User as specified.
- Audit logs use MSP submitter identity and HCA Corporate as specified.
- PAF PDF attaches to Provider Record unless a CVI is created.

If CVI criteria apply:

- PAF PDF goes to the CVI instead of Provider Record.

---

# 21. CPC / CVI Processing

When CPC processing is explicitly required:

1. PAF is auto-accepted as specified.
2. User receives the manual-processing message.
3. CVI is created.
4. CVI Type = Facility Undefined Practitioner.
5. Initial status = UDP Facility Request.
6. CVI notes identify triggering criteria.
7. Due date = PAF auto-acceptance date + 3 business days.
8. CVI remains open until CPC completes processing.
9. Completion status = UDP Facility Request Complete.
10. PAF PDF is attached to CVI.

Trace both:

```text
PAF path
   +
CVI path
```

---

# 22. CACTUS Updates

For every CACTUS update identify:

- API/service
- Request model
- Mapping layer
- Database/repository
- Error handling
- Transaction behavior
- Duplicate checking
- Audit logging

## Provider

For net-new:

- First Name
- Last Name
- Suffix
- Degree
- Provider Category
- NPI

Existing provider records should not change unless explicitly required.

## Entity Assignment

For net-new:

- Create entity assignment using existing logic.
- Status should become NPP – Active where applicable.
- Follow specified security rules.

## Address

Create primary address if no existing record.

## Specialty

For net-new/inactive:

- Add primary specialty when selected.
- Specialty type = Primary.
- Status = Active.

PPI:

- Specialty = APP-Other.
- Status = Not Certified.

## State License

Check existing license using:

- License type
- License number
- License state

Do not update an existing recalled license in normal auto-acceptance cases.

Create a new license when appropriate.

## NPI

Attach NPI image to Provider Record using required naming/type/notes rules.

## Sanctions

Find/create Sanction Check record as specified and attach sanctions image.

---

# 23. Audit and Data Integrity

Always verify:

- Duplicate practitioner detection.
- Duplicate license detection.
- Audit logging.
- Image audit logging.
- Auto-accept history.
- Correct MSP submitter identity.
- Correct entity.

System updates must generate audit logs.

---

# 24. PAF PDF and History

Generated PAF PDF must show:

**ADD NPP to Facility**

Trace:

```text
PAF Type
   |
   v
PDF Template
   |
   v
PDF Generation Service
   |
   v
Attachment Destination
   |
   v
History Generation
```

PDF history must record the required HCP System User/MSP submitter identity.

---

# 25. Reporting

Monthly report requirements:

- Total ADD NPP requests received.
- Total ADD NPP requests routed to CPC.
- Percentage routed to CPC, where applicable.

Distribution:

- Reporting Team
- CPC Distribution List

CVIs with:

**CVI Type = Facility Undefined Practitioner**

must be excluded from MOR statistical calculations.

---

# 26. Key Architectural Distinctions

## ADD NPP vs Add Practitioner to Facility

ADD NPP is a new PAF type.

It may reuse existing framework/routing patterns, but its business rules are different.

## Net New vs Existing Active vs Existing Inactive

These determine:

- Requiredness
- Editability
- CACTUS update behavior

## Normal vs Exception

Normal:

```text
Submit
  ↓
Auto-Accept
  ↓
CACTUS
  ↓
Completed
```

Exception:

```text
Submit
  ↓
Auto-Accept as specified
  ↓
CVI
  ↓
CPC
```

## Existing License vs New License

Existing recalled licenses follow different validation/update behavior from newly entered licenses.

## HCP vs CACTUS

HCP is the workflow/application layer.

CACTUS is the credentialing data system being updated.

---

# 27. Existing Packet Architecture — Important Boundary

The existing HCP architecture includes:

- Facility selection
- State/entity determination
- RFC/RRFC
- Full/DOP packets
- Anchor packets
- DOP + DQs
- DOP + Forms
- Packet determination
- Merge packets
- Resource management
- CPC packet processing

This context is useful for understanding the existing application, but **ADD NPP must not automatically inherit these behaviors**.

The ADD NPP requirements explicitly state not to assume:

- ADD NPP creates credentialing packets.
- ADD NPP participates in PAF merging.
- Facility Specific Questions are required.
- Delegate card is required.

Only apply such behavior when the ADD NPP requirements explicitly require it.

---

# 28. Recommended Repository Investigation Order

```text
1. PAF Framework
       ↓
2. PAF Type / Action
       ↓
3. Add Practitioner to Facility
       ↓
4. Practitioner Search
       ↓
5. Eligibility / Role
       ↓
6. Task/Card Framework
       ↓
7. Demographic
       ↓
8. Address
       ↓
9. Specialty
       ↓
10. ADD NPP Workflow
       ↓
11. License
       ↓
12. PSV
       ↓
13. Validation
       ↓
14. Routing
       ↓
15. Auto-Accept
       ↓
16. CVI / CPC
       ↓
17. CACTUS
       ↓
18. PDF / History
       ↓
19. Audit
       ↓
20. Reporting
       ↓
21. Tests
```

---

# 29. Search Strategy

Start broad and then narrow.

Useful repository search terms:

```text
ADD NPP
Add NPP
NPP
Non Privileged
Non-Privileged
PAF
PAFType
PAF Type
Practitioner Action
Add Practitioner to Facility
MSP
Facility Undefined Practitioner
UDP Facility Request
CVI
Auto Accept
Auto-Accept
CACTUS
Provider
Provider Specialty
Provider License
Sanction
NPI
PSV
State License
Entity Assignment
STATUS_RTK
NPDSTATEx
NPDBSTATEx
```

Also search the existing **Add Practitioner to Facility** implementation.

---

# 30. Requirement Traceability Template

Use this table during repository analysis:

| Requirement | Repository Location | Status | Evidence | Gap |
|---|---|---|---|---|
| MSP-only action | TBD | TBD | TBD | TBD |
| NPI validation | TBD | TBD | TBD | TBD |
| No-NPI search | TBD | TBD | TBD | TBD |
| ADD NPP task | TBD | TBD | TBD | TBD |
| Net-new branching | TBD | TBD | TBD | TBD |
| Existing practitioner rules | TBD | TBD | TBD | TBD |
| Address rules | TBD | TBD | TBD | TBD |
| Specialty rules | TBD | TBD | TBD | TBD |
| VA/PA workflow | TBD | TBD | TBD | TBD |
| License validation | TBD | TBD | TBD | TBD |
| PSV upload | TBD | TBD | TBD | TBD |
| Auto acceptance | TBD | TBD | TBD | TBD |
| CVI routing | TBD | TBD | TBD | TBD |
| CACTUS Provider update | TBD | TBD | TBD | TBD |
| CACTUS Entity update | TBD | TBD | TBD | TBD |
| CACTUS License update | TBD | TBD | TBD | TBD |
| NPI image | TBD | TBD | TBD | TBD |
| Sanctions image | TBD | TBD | TBD | TBD |
| PAF PDF | TBD | TBD | TBD | TBD |
| Audit | TBD | TBD | TBD | TBD |
| Reporting | TBD | TBD | TBD | TBD |

Never mark a requirement as implemented without repository evidence.

---

# 31. Analysis Output Format

For every important finding:

### Business Requirement
What the requirement says.

### Existing Implementation
- File path
- Class/component
- Method/function
- Existing behavior

### Status
One of:

- Implemented
- Partially implemented
- Not implemented
- Different behavior
- Cannot determine

### Impact
Which part of the ADD NPP flow is affected.

### Recommended Implementation
Only after identifying the existing implementation pattern.

---

# 32. Safety / Interpretation Rule

If code contradicts the requirements, report:

**Requirement says:** X

**Code currently does:** Y

**Impact:** Z

**Recommendation:** determine whether Y is intentional legacy behavior or requires change.

If the requirement is ambiguous, state:

> The source requirement does not provide enough information to determine this.

Do not invent business rules.

---

# 33. Implementation Philosophy

Use this workflow:

```text
UNDERSTAND
    ↓
TRACE EXISTING CODE
    ↓
COMPARE WITH NPP REQUIREMENTS
    ↓
IDENTIFY GAP
    ↓
FIND EXISTING REUSABLE PATTERN
    ↓
DESIGN MINIMAL CHANGE
    ↓
IMPLEMENT
    ↓
UNIT / INTEGRATION TEST
    ↓
REGRESSION TEST
    ↓
REVIEW AGAINST REQUIREMENTS
```

The objective is to extend the existing architecture where appropriate, minimize duplication, preserve established behavior, and prevent ADD NPP-specific rules from accidentally affecting existing PAF types.

---

# 34. Core Takeaway

The central mental model for this project is:

```text
                 HCP
                  |
             MSP / PSG
                  |
                 PAF
                  |
        Practitioner Search
                  |
        Net New / Existing
                  |
          ADD NPP Action
                  |
        +---------+---------+
        |         |         |
   Demographic Address  Specialty
        |         |         |
        +---------+---------+
                  |
             ADD NPP Task
                  |
        VA / PA Determination
                  |
              License
                  |
                PSV
                  |
              Validation
                  |
               Submit
                  |
          +-------+-------+
          |               |
       Normal          Exception
          |               |
     Auto-Accept       Auto-Accept
          |               |
       CACTUS             CVI
          |               |
     Completed            CPC
          |               |
          +-------+-------+
                  |
             History/PDF
                  |
                Audit
                  |
              Reporting
```

This is the working architecture model to use when analyzing and implementing the ADD NPP feature.

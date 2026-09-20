# NPP Azure DevOps Ticket Structure & Dependency Map

## Purpose

This document gives Cursor the known Azure DevOps work-item structure for the HCA NPP initiative.

Use it together with:

- HCP NPP PAF Type Requirements
- `add-npp-to-facility-analysis.SKILL.md`
- individual Azure DevOps ticket exports
- meeting notes
- office-repository evidence supplied by the user

This document describes the **ticket relationships discovered from Azure DevOps visualizations**.

It does **not** contain the full acceptance criteria or implementation details of each ticket unless those details are provided separately.

---

# 1. Important Cursor Rule

Do not treat an individual NPP ticket as an isolated requirement.

First understand:

```text
Parent
  ↓
Epic / Feature
  ↓
Child Work Items
  ↓
Related Work
  ↓
Successors / Dependencies
  ↓
Actual Business Requirement
  ↓
Repository Implementation
```

Preserve the Azure DevOps relationship type.

Do not interpret a `Related` relationship as a dependency.

Do not interpret a `Successor` as merely another child.

Do not infer implementation order from ticket titles alone.

---

# 2. Known NPP Work-Item Graph

The currently identified structure is:

```text
52350
Project - Add NPP to existing PAF Types
│
├── 136871
│   NPP: Add Begin NPP PAF Entry Point to MSP Dashboard
│   │
│   ├── 136872
│   │   NPP: Add "Begin NPP PAF" Entry Point on MSP
│   │   │
│   │   ├── 136882 — Child
│   │   │   NPP: Preserve existing Begin PAF functionality
│   │   │
│   │   ├── 136883 — Child
│   │   │   NPP: Validate security and authorization for Begin NPP PAF
│   │   │
│   │   ├── 136876 — Child
│   │   │   NPP: Provide user guidance through tooltip
│   │   │
│   │   ├── 144239 — Related
│   │   │   NPP: Add "Begin NPP PAF" Entry Point on MSP
│   │   │
│   │   ├── 136878 — Child
│   │   │   NPP: Launch existing Enforce NPI Search workflow
│   │   │
│   │   └── 136873 — Child
│   │       NPP: Add Begin NPP PAF action to MSP Dashboard
│   │
│   └── 53658 — Successor
│       NPP: PAF - Search
│       │
│       └── 127104 — Child
│           NPP: Add NPP to Facility PAF Practitioner Search
```

---

# 3. Work Item Inventory

| ID | Title | Relationship | Known Parent/Context | Current Known State |
|---|---|---|---|---|
| 52350 | Project - Add NPP to existing PAF Types | Parent/project | Top-level context | To Do |
| 136871 | NPP: Add Begin NPP PAF Entry Point to MSP Dashboard | Epic/feature | 52350 | To Do |
| 136872 | NPP: Add "Begin NPP PAF" Entry Point on MSP | Child of 136871 | 136871 | To Do |
| 136882 | NPP: Preserve existing Begin PAF functionality | Child | 136872 | To Do |
| 136883 | NPP: Validate security and authorization for Begin NPP PAF | Child | 136872 | To Do |
| 136876 | NPP: Provide user guidance through tooltip | Child | 136872 | To Do |
| 144239 | NPP: Add "Begin NPP PAF" Entry Point on MSP | Related | 136872 | To Do |
| 136878 | NPP: Launch existing Enforce NPI Search workflow | Child | 136872 | To Do |
| 136873 | NPP: Add Begin NPP PAF action to MSP Dashboard | Child | 136872 | To Do |
| 53658 | NPP: PAF - Search | Successor | Related to 136871 | To Do |
| 127104 | NPP: Add NPP to Facility PAF Practitioner Search | Child | 53658 | To Do |

---

# 4. Relationship Definitions

## Parent

Example:

```text
52350
  ↓
136871
```

Meaning:

`52350` provides broader project/feature context for `136871`.

Cursor should use the parent to understand scope.

Do not assume every parent requirement belongs directly in the child implementation.

---

## Child

Example:

```text
136872
  ↓
136873
```

A child is part of the implementation breakdown of the parent work item.

Cursor should inspect child requirements when determining the complete scope of the parent feature.

---

## Related

Example:

```text
136872
  ── Related ──> 144239
```

Related does not automatically mean:

- prerequisite
- child
- successor
- implementation dependency

Cursor must read the related ticket before deciding how it affects implementation.

---

## Successor

Example:

```text
136871
  ↓ Successor
53658
```

A successor indicates a sequencing/dependency relationship in Azure DevOps.

For this graph, the known relationship is:

```text
136871
NPP: Add Begin NPP PAF Entry Point to MSP Dashboard

        ↓ Successor

53658
NPP: PAF - Search
```

Cursor should inspect both tickets and determine the actual dependency from their acceptance criteria and implementation context.

Do not infer more than the ticket relationship proves.

---

# 5. Begin NPP Entry-Point Area

The first major feature group currently identified is:

```text
136871
NPP: Add Begin NPP PAF Entry Point to MSP Dashboard
```

Its child/context work includes:

### 136872

`NPP: Add "Begin NPP PAF" Entry Point on MSP`

Children:

- 136882 — Preserve existing Begin PAF functionality
- 136883 — Validate security and authorization
- 136876 — Provide user guidance through tooltip
- 136878 — Launch existing Enforce NPI Search workflow
- 136873 — Add Begin NPP PAF action to MSP Dashboard

Related:

- 144239 — Add "Begin NPP PAF" Entry Point on MSP

Cursor should treat these as a connected feature area.

---

# 6. Existing Begin PAF Compatibility

### 136882

Title:

> NPP: Preserve existing Begin PAF functionality

This indicates that the NPP entry-point change has a compatibility/regression concern around existing Begin PAF behavior.

Important Cursor rule:

Do not assume what "Mahendra" means technically.

Obtain the actual ticket description and repository implementation before generating a Copilot prompt.

Cursor should investigate:

- existing Begin PAF behavior
- existing Begin PAF action
- existing MSP dashboard behavior
- authorization
- navigation
- regression tests

Then compare NPP behavior against the existing behavior.

---

# 7. Security / Authorization

### 136883

Title:

> NPP: Validate security and authorization for Begin NPP PAF

This is a separate child work item.

Cursor should not invent an authorization mechanism.

First obtain:

1. Ticket acceptance criteria.
2. Existing Begin PAF authorization implementation.
3. Existing MSP authorization pattern.
4. Existing tests.
5. Any NPP-specific security requirement.

Then generate the Copilot prompt.

---

# 8. Tooltip / User Guidance

### 136876

Title:

> NPP: Provide user guidance through tooltip

This is a UI-focused child.

Cursor should obtain:

- exact tooltip text
- display conditions
- UI component
- accessibility expectations
- existing tooltip pattern
- tests

Do not invent tooltip wording.

---

# 9. Existing Enforce NPI Search Workflow

### 136878

Title:

> NPP: Launch existing Enforce NPI Search workflow

This ticket is particularly important.

The title explicitly references an **existing workflow**.

Cursor must therefore follow:

```text
NPP Begin PAF
      ↓
Existing Enforce NPI Search workflow
```

Do NOT create a second NPI search implementation unless repository evidence proves the existing workflow cannot be reused.

Before generating the Copilot implementation prompt, Cursor should ask the user for the office-repository implementation of:

- Enforce NPI Search
- related API/service
- relevant UI/component
- routing/navigation
- tests

If those files have not been supplied, stop and request them.

---

# 10. Begin NPP PAF Dashboard Action

### 136873

Title:

> NPP: Add Begin NPP PAF action to MSP Dashboard

This likely concerns the MSP Dashboard entry point, but the exact implementation must come from the ticket and repository.

Cursor should investigate:

- existing Begin PAF dashboard action
- dashboard action registration
- UI component
- authorization
- navigation
- PAF type/action passed to backend
- existing tests

Do not assume exact implementation.

---

# 11. PAF Search Area

### 53658

Title:

> NPP: PAF - Search

This is identified as the **Successor** to `136871`.

It has child:

### 127104

> NPP: Add NPP to Facility PAF Practitioner Search

Therefore:

```text
136871
Begin NPP PAF Entry Point
       ↓
     Successor
       ↓
53658
NPP: PAF - Search
       ↓
     Child
       ↓
127104
NPP: Add NPP to Facility PAF Practitioner Search
```

This should be treated as a separate but connected feature area.

---

# 12. Practitioner Search Area

### 127104

Title:

> NPP: Add NPP to Facility PAF Practitioner Search

This is especially important because practitioner search is a major part of the ADD NPP requirements.

Cursor should map this ticket to the business requirements for:

- NPI search
- no-NPI search
- practitioner lookup
- duplicate checking
- Net New vs Existing classification
- existing active/inactive behavior
- MSP eligibility

But Cursor must use the ticket acceptance criteria and repository implementation to determine exactly what `127104` covers.

---

# 13. Ticket Graph vs Business Requirement

Do not confuse:

```text
Azure DevOps structure
```

with:

```text
NPP business process
```

The ticket graph tells us how work is organized.

The NPP Requirements document tells us what the business behavior must be.

Use both.

Example:

```text
Azure DevOps
    ↓
127104
NPP Practitioner Search

NPP Requirements
    ↓
NPI Search
No-NPI Search
Duplicate Check
Practitioner Classification
Eligibility

Repository
    ↓
Actual implementation
```

Cursor must reconcile these three sources.

---

# 14. Required Ticket Analysis for Every Work Item

For each NPP work item, Cursor should extract:

```text
Work Item ID
Title
Type
State
Parent
Children
Related Items
Successors
Predecessors
Business Objective
Description
Acceptance Criteria
UI Requirements
API Requirements
Validation Requirements
Security Requirements
Integration Requirements
Dependencies
Regression Requirements
Tests
Attachments
Comments
```

If a field is unavailable, mark it:

```text
Not provided
```

Do not invent it.

---

# 15. Required NPP Ticket Matrix

As ticket exports are provided, Cursor should maintain:

| ID | Title | Relationship | Business Area | Dependency | Repository Area | Status |
|---|---|---|---|---|---|---|
| 52350 | Project - Add NPP to existing PAF Types | Parent | Overall NPP | — | TBD | To Do |
| 136871 | Begin NPP PAF Entry Point | Feature | MSP Dashboard | Leads to 53658 | TBD | To Do |
| 136872 | Begin NPP PAF Entry Point on MSP | Child | MSP | 136871 | TBD | To Do |
| 136882 | Preserve existing Begin PAF | Child | Regression | 136872 | TBD | To Do |
| 136883 | Security/Authorization | Child | Security | 136872 | TBD | To Do |
| 136876 | Tooltip | Child | UI | 136872 | TBD | To Do |
| 136878 | Existing Enforce NPI Search | Child | Search | 136872 | TBD | To Do |
| 136873 | Begin NPP PAF Dashboard Action | Child | UI | 136872 | TBD | To Do |
| 144239 | Begin NPP PAF Entry Point | Related | MSP | Related | TBD | To Do |
| 53658 | NPP PAF Search | Successor | Search | 136871 | TBD | To Do |
| 127104 | Add NPP to Facility PAF Practitioner Search | Child | Search | 53658 | TBD | To Do |

Update this matrix as new ticket information becomes available.

---

# 16. Ticket Collection Strategy

Do not require the user to manually provide every ticket immediately.

As the user provides:

- screenshots
- copied ticket descriptions
- exported Markdown
- Excel
- PDF
- comments
- acceptance criteria

Cursor should progressively enrich the map.

When information is missing, request only the next relevant ticket.

Example:

> We have the relationship for `136878`, but not its acceptance criteria. Please export/copy work item 136878 so I can determine exactly what the existing Enforce NPI Search workflow must do.

---

# 17. Office Repository Evidence Boundary

The ticket graph is available on the personal laptop.

The actual repository is on the office laptop.

Therefore:

```text
Ticket evidence
        ↓
Cursor understands WHAT is required
        ↓
Repository evidence needed
        ↓
User supplies office files
        ↓
Cursor determines HOW existing HCP implements it
        ↓
Cursor generates Copilot prompt
```

Never skip the repository-evidence step for implementation-specific recommendations.

---

# 18. Cursor's First Job After Loading This File

When this document is loaded, Cursor should NOT immediately generate code.

It should respond with:

### NPP Ticket Graph Understanding

Summarize the known graph.

### Missing Ticket Details

List tickets for which the full description/acceptance criteria are not yet available.

### Business Requirements Available

Identify which requirements are already known from the NPP requirements document.

### Repository Evidence Needed

Identify the minimum office-repository files required for the first implementation area.

### Recommended Next Investigation

Choose the next investigation based on dependency evidence, not arbitrary preference.

### Exact File Request

Ask the user for the specific office files needed.

---

# 19. Important Current Knowledge Boundary

From the visualizations, the following is known:

- `52350` is the parent project context.
- `136871` is the Begin NPP PAF Entry Point feature.
- `136872` is a child of `136871`.
- `136872` has children `136882`, `136883`, `136876`, `136878`, and `136873`.
- `144239` is related to `136872`.
- `136871` has `53658` as a successor.
- `53658` has `127104` as a child.
- `53658` is `NPP: PAF - Search`.
- `127104` is `NPP: Add NPP to Facility PAF Practitioner Search`.

Anything beyond those relationships must be supported by actual ticket content or repository evidence.

---

# 20. Do Not Over-Interpret Ticket Titles

A ticket title such as:

> NPP: Launch existing Enforce NPI Search workflow

does not by itself establish:

- the exact API
- exact UI component
- exact service
- exact route
- exact PAF type
- exact implementation
- exact test class

Those must be discovered from the ticket details and office repository.

---

# 21. Goal

The purpose of this ticket structure document is to help Cursor answer:

> "What is the complete NPP work breakdown, how are the tickets related, what depends on what, and what repository evidence do I need before generating the next Copilot implementation prompt?"

The final implementation workflow is:

```text
NPP Ticket Graph
       ↓
Ticket Requirements
       ↓
Business Requirement Mapping
       ↓
Office Repository Evidence
       ↓
Existing Architecture
       ↓
Gap Analysis
       ↓
Focused Implementation Task
       ↓
Exact Copilot Prompt
       ↓
Office Implementation
       ↓
Cursor Review
       ↓
Next Task
```

The goal is **controlled, evidence-based implementation**, not blind code generation.

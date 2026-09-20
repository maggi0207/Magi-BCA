# Exact Copilot Prompt Template

First, have office Copilot write the repository source of truth: [Copilot_Existing_PAF_NPP_Code_Flow_Prompt.md](Copilot_Existing_PAF_NPP_Code_Flow_Prompt.md). Copy the result to `08_Office_Repository_Evidence/HCP_Existing_PAF_NPP_Code_Flow.md`.

Fill the blocks below from that file’s **Implementation Prompt Seeds**. If a seed is `PromptReady: no`, do not generate an implementation prompt — ask for the listed gap.

Implementation task pack (after seeds are Ready): [Copilot_Task_Prompts_136871_Begin_NPP_PAF.md](Copilot_Task_Prompts_136871_Begin_NPP_PAF.md).

Every implementation task must end with an exact copy-paste prompt.

Use:

```
You are implementing a change in the HCA HCP Credentialing repository.

ROLE:
Act as a senior engineer familiar with the existing repository patterns.

TASK:
<one focused implementation task>

BUSINESS REQUIREMENT:
<exact NPP requirement>

REPOSITORY EVIDENCE:
<paste from HCP_Existing_PAF_NPP_Code_Flow.md Implementation Prompt Seed for this ticket>

EXISTING PATTERN TO REUSE:
<paste seed EXISTING PATTERN TO REUSE>

SCOPE:
<paste seed SCOPE — files expected to change>

DO NOT CHANGE:
<paste seed DO NOT CHANGE — especially Begin PAF and unrelated PAF types>

IMPLEMENTATION REQUIREMENTS:
1. Reuse the existing repository pattern.
2. Preserve existing behavior for other PAF types.
3. Do not create duplicate services/components.
4. Follow existing dependency injection.
5. Follow existing validation.
6. Follow existing logging/error handling.
7. Follow existing test conventions.
8. Do not invent business rules.
9. Do not modify unrelated code.

BEFORE CODING:
1. Inspect the referenced implementation.
2. Confirm the extension point.
3. Explain the planned changes briefly.
4. Then implement.

IMPLEMENTATION:
<precise implementation>

TESTS:
<paste seed TESTS plus ticket acceptance scenarios>

VALIDATION:
- build
- relevant tests
- regression tests
- inspect changed files
- inspect diff

RETURN:
1. Files changed
2. Classes/methods changed
3. Requirement implemented
4. Tests
5. Build/test results
6. Remaining gaps
7. Any assumptions
```

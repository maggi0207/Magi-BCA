# NPP Dependency Map

Currently known Azure DevOps relationship graph.

Do **not** invent relationships beyond this map.

```text
52350
Project - Add NPP to existing PAF Types
  |
  └── 136871
       NPP: Add Begin NPP PAF Entry Point to MSP Dashboard
       |
       ├── Child → 136872
       |            NPP: Add "Begin NPP PAF" Entry Point on MSP
       |            |
       |            ├── Child → 136882
       |            |            NPP: Preserve existing Begin PAF functionality
       |            ├── Child → 136883
       |            |            NPP: Validate security and authorization for Begin NPP PAF
       |            ├── Child → 136876
       |            |            NPP: Provide user guidance through tooltip
       |            ├── Related → 144239
       |            |              NPP: Add "Begin NPP PAF" Entry Point on MSP
       |            ├── Child → 136878
       |            |            NPP: Launch existing Enforce NPI Search workflow
       |            └── Child → 136873
       |                         NPP: Add Begin NPP PAF action to MSP Dashboard
       |
       └── Successor → 53658
                         NPP: PAF - Search
                         |
                         └── Child → 127104
                                      NPP: Add NPP to Facility PAF Practitioner Search – Add New Practitioner
```

`52350` also lists additional children in the Azure DevOps screenshot. Titles only; do not invent parent/child links beyond “listed under 52350”:

- 131724, 131728, 132600, 131783, 132591, 131785, 132255, 132537, 131794, 132687, 132247, 132275

Full inventory: [../NPP_52350_Project_Scope.md](../NPP_52350_Project_Scope.md)

## Relationship types

| Type | Meaning | Do not treat as |
|---|---|---|
| Parent | Broader project/feature context | A requirement that must all be implemented in the child |
| Child | Part of the parent work-item breakdown | A successor/prerequisite by itself |
| Related | Linked work; read the ticket before deciding impact | Prerequisite, child, successor, or implementation dependency |
| Successor | Azure DevOps sequencing/dependency relationship | Merely another child |

## Investigation sequence implied by this graph only

1. `52350` — project context
2. `136871` — Begin NPP PAF entry-point epic
3. `136872` and its children (`136882`, `136883`, `136876`, `136878`, `136873`)
4. `144239` — related; inspect before assuming impact
5. `53658` — successor search area
6. `127104` — child of the search successor

Do not infer implementation order from titles alone. Confirm from ticket acceptance criteria and office-repository evidence.

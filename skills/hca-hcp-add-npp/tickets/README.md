# Ticket summaries

Nine-section summaries for **captured** Azure DevOps tickets, grouped by **parent hierarchy**. Detailed files remain in `02_Epics`, `03_Features`, `04_User_Stories`, `05_Related`, and `06_Successors`.

ASCII flowchart: [ASCII_FLOWCHART.md](ASCII_FLOWCHART.md)  
Deep ADO graph (from `download (1).png`): [ADO_LINK_GRAPH_HIERARCHY.md](ADO_LINK_GRAPH_HIERARCHY.md)  
Implementation order (done / next / epic-by-epic): [IMPLEMENTATION_ORDER.md](IMPLEMENTATION_ORDER.md)

Project inventory (including title-only 52350 children): [../NPP_52350_Project_Scope.md](../NPP_52350_Project_Scope.md)

Do not interpret `Related` as a dependency. Do not interpret `Successor` as merely another child.

```text
52350  Project - Add NPP to existing PAF Types
│
├── 136871  Epic — Begin NPP PAF Entry Point
│   │
│   ├── 136872  Feature — Begin NPP PAF on MSP Dashboard
│   │   ├── 136873  Child — Add Begin NPP PAF action
│   │   ├── 136876  Child — Tooltip
│   │   ├── 136878  Child — Launch Enforce NPI Search
│   │   ├── 136882  Child — Preserve Begin PAF
│   │   ├── 136883  Child — Security / direct URL
│   │   └── 144239  Related — not a child (no summary in this folder)
│   │
│   └── 53658  Successor — NPP: PAF - Search   [HELD]
│       └── 127104  Child — Add NPP practitioner search
│
├── 132255  Feature — ADD NPP to Facility PAF Action   [AC on feature; child AC pending]
│   ├── 132256  Child — Display action for MSP [AC captured]
│   ├── 132257  Child — Restrict eligible practitioners [AC captured]
│   ├── 132258  Child — Apply existing task determination [AC captured]
│   └── 132259  Child — Enforce single action selection [AC captured]
│
└── (title-only, listed under 52350 — no captured summaries here)
    131724, 131728, 131783, 131785, 131794, 132247,
    132275, 132537, 132591, 132600, 132687
```

## Folder tree

| Path | ID | Relationship |
|---|---|---|
| [52350/136871_Begin_NPP_PAF/136871_Begin_NPP_PAF.md](52350/136871_Begin_NPP_PAF/136871_Begin_NPP_PAF.md) | 136871 | Child of 52350 |
| [52350/136871_Begin_NPP_PAF/136872_Begin_NPP_PAF_Entry_Point/136872_Begin_NPP_PAF_Entry_Point.md](52350/136871_Begin_NPP_PAF/136872_Begin_NPP_PAF_Entry_Point/136872_Begin_NPP_PAF_Entry_Point.md) | 136872 | Child of 136871 |
| […/136872_Preserve_Begin_PAF.md](52350/136871_Begin_NPP_PAF/136872_Begin_NPP_PAF_Entry_Point/136872_Preserve_Begin_PAF.md) | — | Pointer only — Preserve is 136882 |
| […/136873_Begin_NPP_PAF_Dashboard_Action.md](52350/136871_Begin_NPP_PAF/136872_Begin_NPP_PAF_Entry_Point/136873_Begin_NPP_PAF_Dashboard_Action.md) | 136873 | Child of 136872 |
| […/136876_Begin_NPP_PAF_Tooltip.md](52350/136871_Begin_NPP_PAF/136872_Begin_NPP_PAF_Entry_Point/136876_Begin_NPP_PAF_Tooltip.md) | 136876 | Child of 136872 |
| […/136878_Launch_Enforce_NPI_Search.md](52350/136871_Begin_NPP_PAF/136872_Begin_NPP_PAF_Entry_Point/136878_Launch_Enforce_NPI_Search.md) | 136878 | Child of 136872 |
| […/136882_Preserve_Begin_PAF.md](52350/136871_Begin_NPP_PAF/136872_Begin_NPP_PAF_Entry_Point/136882_Preserve_Begin_PAF.md) | 136882 | Child of 136872 |
| […/136883_Begin_NPP_PAF_Security.md](52350/136871_Begin_NPP_PAF/136872_Begin_NPP_PAF_Entry_Point/136883_Begin_NPP_PAF_Security.md) | 136883 | Child of 136872 |
| [52350/136871_Begin_NPP_PAF/53658_NPP_PAF_Search/53658_NPP_PAF_Search.md](52350/136871_Begin_NPP_PAF/53658_NPP_PAF_Search/53658_NPP_PAF_Search.md) | 53658 | **Successor** of 136871 |
| […/127104_Add_NPP_Practitioner_Search.md](52350/136871_Begin_NPP_PAF/53658_NPP_PAF_Search/127104_Add_NPP_Practitioner_Search.md) | 127104 | Child of 53658 |
| [52350/132255_ADD_NPP_PAF_Action_Option/132255.md](52350/132255_ADD_NPP_PAF_Action_Option/132255.md) | 132255 | Child of 52350 |
| […/132256_Add_PAF_Action_MSP.md](52350/132255_ADD_NPP_PAF_Action_Option/132256_Add_PAF_Action_MSP.md) | 132256 | Child of 132255; **AC captured** |
| […/132257_Restrict_Eligible_Practitioners.md](52350/132255_ADD_NPP_PAF_Action_Option/132257_Restrict_Eligible_Practitioners.md) | 132257 | Child of 132255; **AC captured** |
| […/132258_Apply_Task_Determination.md](52350/132255_ADD_NPP_PAF_Action_Option/132258_Apply_Task_Determination.md) | 132258 | Child of 132255; **AC captured** |
| […/132259_Enforce_Single_Action.md](52350/132255_ADD_NPP_PAF_Action_Option/132259_Enforce_Single_Action.md) | 132259 | Child of 132255; **AC captured** |

Related 144239 (not a child): [../05_Related/144239_Begin_NPP_PAF_Entry_Point/144239.md](../05_Related/144239_Begin_NPP_PAF_Entry_Point/144239.md)

Do not implement from one ticket in isolation. Place it in the `52350` hierarchy first.

ASCII ticket hierarchy for ADD NPP.

Full graph (every feature’s hanging stories from `download (1).png`): [ADO_LINK_GRAPH_HIERARCHY.md](ADO_LINK_GRAPH_HIERARCHY.md)  
Implementation order: [IMPLEMENTATION_ORDER.md](IMPLEMENTATION_ORDER.md)

Source of 52350 children: Azure DevOps Links on **52350** (screenshot 2026-09-20). Child **order matches ADO**.

Do not treat Related as a dependency. Do not treat Successor as merely another child.
Title-only children have **no captured AC** — do not implement from the title.

Legend:
  [ADO Done] / [ADO To do]  = Azure DevOps state on 52350
  [IMPL]                    = implemented in office repo
  [HELD]                    = parked (search reuse done; exception audit deferred)
  [TITLE]                   = no AC export

```
52350  PRODUCT  [ADO To do]
|      Project - Add NPP to existing PAF Types
|
|  -- direct children of 52350 (ADO Links / Child) --
|
+-- 131724  [ADO Done]  [TITLE]
|         NPP: Cactus Enable Maintenance and Management of
|         New Credentialing Values for Non-...
|
+-- 131728  [ADO To do]  [TITLE]
|         NPP: Enable HCP Access and Role-Based Security for
|         Non-Defined Practitioner (NPP) ...
|         (do not use as 136883 AC)
|
+-- 132600  [ADO To do]  [TITLE]  Feature
|         NPP: PAF to Cactus Credentialing Record Creation
|         & Synchronization
|
+-- 136871  [ADO To do]  [IMPL]  Epic
|   |     NPP: Add Begin NPP PAF Entry Point to MSP Dashboard
|   |
|   +-- Child --> 136872  [IMPL]  Feature
|   |             NPP: Add "Begin NPP PAF" Entry Point on MSP Dashboard
|   |             |
|   |             +-- Child --> 136873  [IMPL]  Button
|   |             +-- Child --> 136876  [IMPL]  Tooltip
|   |             +-- Child --> 136878  [IMPL]  Launch Enforce NPI Search
|   |             +-- Child --> 136882  [IMPL]  Preserve Begin PAF
|   |             +-- Child --> 136883  [IMPL]  Security / direct URL *
|   |             +-- Related -> 144239         Related entry (not a child)
|   |
|   +-- Successor --> 53658   (also a 52350 child, below)
|
+-- 131783  [ADO To do]  [TITLE]
|         NPP: Add NPP to Facility PAF Type Selection and
|         Filtering in CPC Dashboard Completed ...
|
+-- 132591  [ADO To do]  [TITLE]
|         NPP: Auto-Acceptance and Automated Processing
|
+-- 131785  [ADO To do]  [TITLE]
|         NPP: Controlled "No NPI Available" Exception Workflow
|         for NPP
|         (do not merge into 127104 / 53658)
|
+-- 132255  [ADO To do]  Feature  [CAPTURED]
|   |     NPP: Create "ADD NPP to Facility" PAF Action Option
|   |     Radio ADD NPP to Facility; MSP + eligible; reuse task rules
|   |
|   +-- Child --> 132256  [AC captured] Display action for MSP
|   +-- Child --> 132257  [AC captured] Restrict eligible practitioners
|   +-- Child --> 132258  [AC captured] Apply existing task determination
|   +-- Child --> 132259  [AC captured] Enforce single action selection
|
+-- 132537  [ADO To do]  [TITLE]
|         NPP: CVI Creation
|
+-- 131794  [ADO To do]  [TITLE]
|         NPP: Modify Add New Practitioner Card for ADD NPP
|         to Facility PAF
|
+-- 53658   [ADO To do]  [HELD]  Feature
|   |     NPP: PAF - Search
|   |     Child of 52350 AND Successor of 136871
|   |
|   +-- Child --> 127104  [HELD]
|                 NPP: Add NPP to Facility PAF Practitioner Search
|                 – Add New Practitioner
|
+-- 132687  [ADO To do]  [TITLE]
|         NPP: Reports
|
+-- 132247  [ADO To do]  [TITLE]
|         NPP: Verify Practitioner Information Card –
|         Existing Practitioner
|
+-- 132275  [ADO To do]  [TITLE]  Feature
          PAF Tasks
```

* 136883 is in the office repo (Msp-only button + `/Practitioner/Search/Npp` redirect to RootUri). Close after manual checks. ADO still shows 136871 To Do.

Box-down (main implemented path):

```
                    +---------------------------+
                    | 52350  PRODUCT            |
                    | Add NPP to existing PAF   |
                    +-------------+-------------+
                                  |
              +-------------------+-------------------+
              | Child                                 | Child + Successor of 136871
              v                                       v
    +---------+-----------+                 +---------+-----------+
    | 136871  EPIC [IMPL] |                 | 53658  SEARCH [HELD]|
    | Begin NPP PAF Entry |                 | NPP: PAF - Search   |
    +---------+-----------+                 +---------+-----------+
              |                                       |
              | Child                                 | Child
              v                                       v
    +---------+-----------+                 +---------+-----------+
    | 136872  FEATURE     |                 | 127104  [HELD]      |
    | MSP Dashboard entry |                 | Add NPP search      |
    +--+---+---+---+---+--+                 +---------------------+
       |   |   |   |   |
       v   v   v   v   v
    +-----+ +-----+ +-----+ +-----+ +-----+     +--------+
    |873  | |876  | |878  | |882  | |883  |     |144239  |
    |Btn  | |Tip  | |NPI  | |PAF  | |Auth |     |Related |
    |IMPL | |IMPL | |IMPL | |IMPL | |IMPL |     |not child
    +-----+ +-----+ +-----+ +-----+ +-----+     +--------+
```


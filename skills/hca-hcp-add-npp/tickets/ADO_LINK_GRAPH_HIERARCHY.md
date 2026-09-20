# 52350 — Deep ticket hierarchy (from ADO graph)

**Source image:** [`../download (1).png`](../download%20(1).png)  
**Size:** 24077×1377 (too wide to read as one picture)  
**What the PNG is:** Azure DevOps **directed link graph** (Child **and** Successor **and** Related). It is **not** a pure parent-only tree.

**Authoritative parent list** for 52350 remains the **Links → Child** tab (screenshot 2026-09-20). Use that for “who reports to 52350”. Use this file for **children under those features**, which the graph shows.

Titles below are **read from the graph cards**. Truncated text is marked `…`. Assignee on most stories: Bathula Mahendra unless noted. ADO state on cards is **To do** unless noted.

Do **not** implement from titles. Do **not** treat Successor/Related as Parent.

---

## How to read

```
52350                    ← Project (yellow in graph)
 ├── Feature / Epic      ← orange or purple boxes (direct 52350 children)
 │    └── User Story     ← blue-left cards hanging under a feature
 └── (dashed/cross edges) Successor / Related — listed separately
```

`136871` appears in the graph as a **single orange node** under a line from `53658`. That is the **Successor** link (53658 succeeds 136871), **not** “136871 is a child of 53658”. Both are children of 52350.

---

## Level 0 — Project

```
52350  Project - Add NPP to existing PAF Types    [ADO To do]
```

---

## Level 1 — Direct children of 52350

Order matches the ADO Links tab.

| ID | Title (graph / Links) | Kind in graph | Notes |
|---|---|---|---|
| 131724 | Cactus Enable Maintenance and Management of New Credentialing Values for Non-… | Feature | **ADO Done** on Links tab |
| 131728 | Enable HCP Access and Role-Based Security for Non-Defined Practitioner (NPP) … | Feature | Not 136883 AC |
| 132600 | PAF to Cactus Credentialing Record Creation & Synchronization | Epic/Feature (orange) | Large CACTUS subtree |
| **136871** | Add Begin NPP PAF Entry Point to MSP Dashboard | Epic (orange) | Subtree **not expanded** in this PNG; see §136871 |
| 131783 | Add NPP to Facility PAF Type Selection and Filtering in CPC Dashboard … | Feature | |
| 132591 | Auto-Acceptance and Automated Processing | Feature | Fan of 132592–132599 |
| 131785 | Controlled “No NPI Available” Exception Workflow for NPP | Feature | Do not merge into 127104 |
| 132255 | Create “ADD NPP to Facility” PAF Action Option | Feature | Trunk 2 start (needs AC) |
| 132537 | CVI Creation | Feature | |
| 131794 | Modify Add New Practitioner Card for ADD NPP to Facility PAF | Feature | |
| **53658** | PAF - Search | Feature | Child of 52350 **and** Successor of 136871 |
| 132687 | Reports | Feature | |
| 132247 | Verify Practitioner Information Card – Existing Practitioner | Feature | |
| 132275 | PAF Tasks | Epic/Feature (orange) | Largest task/card subtree |

---

## 136871 — Begin NPP PAF Entry (not expanded in the PNG)

Captured from ticket files (not from this image):

```
136871  Epic  [office implemented]
 └── 136872  Feature  Begin NPP PAF on MSP Dashboard
      ├── 136873  Child  Button
      ├── 136876  Child  Tooltip
      ├── 136878  Child  Launch Enforce NPI Search
      ├── 136882  Child  Preserve Begin PAF
      ├── 136883  Child  Security / direct URL
      └── 144239  Related  (not a child)
```

Graph extra: a line from **53658 → 136871** (Successor / predecessor drawing).

---

## 53658 — NPP: PAF - Search  [HELD]

```
53658  NPP: PAF - Search
 └── 127104  Add NPP to Facility PAF Practitioner Search – Add New Practitioner
```

Graph also draws edges from 53658 toward some CVI stories (132539 / 132540 / 132589 / 132590). Treat those as **Related/Successor edges**, not as 53658 children, unless ADO Links on 53658 is captured later.

---

## 132537 — CVI Creation

```
132537  NPP: CVI Creation
 ├── 132539  CVI Creation Auto-Accept PAF
 ├── 132540  CVI Creation Display CPC Processing
 ├── 132589  CVI Creation Automatically Create CPC
 └── 132590  CVI Creation Attach PAF PDF to CVI Instead of …
```

Related Cactus CVI setup (under **131724**, Banks Michael, Ready for DEV):

```
131724  Cactus credentialing values  [ADO Done]
 ├── 131725  Add New CVI Type “Facility Undefined …
 ├── 131726  Cactus Add New CVI Statuses “UDP Facility …
 └── 132684  CVI Closure of …          (title truncated on card)
```

---

## 131728 — NPP RBAC

```
131728  Enable HCP Access and Role-Based Security …
 └── 131729  Configure Role-Based Access Control for …
```

---

## 131783 / 132255 — PAF type / action

```
131783  Add NPP to Facility PAF Type …
 └── 131784  Add “Add NPP to Facility” PAF Type to …

132255  Create “ADD NPP to Facility” PAF Action Option   [feature captured]
 ├── 132256  Display New PAF Action for MSP User          [AC captured]
 ├── 132257  Restrict Action to Eligible Practitioners    [AC captured]
 ├── 132258  Apply Existing PAF Task Determination Rules  [AC captured]
 └── 132259  Enforce Single PAF Action Selection          [AC captured]
```

---

## 131785 — No-NPI exception workflow

Do **not** implement these as 127104 / 53658.

```
131785  Controlled “No NPI Available” Exception Workflow for NPP
 ├── 131786  Create Exception Search Screen for Practitioners
 ├── 131787  Implement Exception Search Validation
 ├── 131788  Search Practitioner Records Using …
 ├── 131790  Implement State Lookup Integration
 └── 131791  Provide Search and Cancel Actions for Exception
```

---

## 131794 — Modify Add New Practitioner Card

```
131794  Modify Add New Practitioner Card for ADD NPP …
 ├── 131795  Configure NPP-Specific Field Requirements
 ├── 132248  Remove Email Requirement for Existing
 ├── 132250  Prevent Modification of Existing Email
 ├── 132252  Allow Entry of Existing Email Addresses   (ID read from truncated card)
 └── 132253  Validate Email Format When Entered
```

---

## 132247 — Existing practitioner card

Parent box is on the graph; story titles in this slice overlap 131794 email rules. Confirm children on the **132247 Links** tab before coding. Visible nearby cards that may belong here or to 131794:

- 132248, 132250, 132252, 132253 (email / existing practitioner)

---

## 132275 — PAF Tasks  (orange)

Mid-level task hosts hang under 132275; stories hang under those hosts.

```
132275  PAF Tasks
 ├── 132260  Task card in PAF Tasks
 │    ├── 132265  Enforce requiredness for ADD NPP to …   (ID truncated on card)
 │    ├── 132267  Prevent Submission Until Required Tasks Are …
 │    ├── 132268  Suppress Delegate and Facility Specific Questions
 │    ├── 132261  Display ADD NPP to Facility Task Card
 │    └── 132266  Apply Practitioner-Type Requiredness
 ├── 132262  Restrict Task Card Visibility Based on Security
 ├── 132263  Launch ADD NPP to Facility Workflow
 ├── 132264  Update Task Status Based on Workflow Completion
 ├── 132283  PAF Manage Address
 │    ├── 132327  Manage Addresses Allow Address Creation When No …
 │    ├── 132343  Manage Address Complete Add Address Form
 │    ├── 132342  Manage Address Suppress Group Address Selection
 │    ├── 132308  Disable Address Changes When …
 │    ├── 132344  Manage Address Validation Errors
 │    ├── 132288  Manage Addresses Display Only Primary Address
 │    └── 132341  Manage Address Allow Address Addition When …
 ├── 132352  ADD NPP to Facility Task Card
 │    ├── 132354  Create Add NPP Facility Card
 │    ├── 132355  Add NPP to Facility Card VA Workflow
 │    ├── 132351  Add NPP to Facility Routing
 │    ├── 132527  Add NPP to Facility Card State License
 │    ├── 132368  Add NPP to Facility Existing Practitioner
 │    ├── 132367  Add NPP to Facility VA = NO (Net New)
 │    ├── 132530  Add NPP to Facility Card License PSV Upload
 │    ├── 132529  Add NPP to Facility Card State Matching
 │    ├── 132365  Add NPP to Facility Existing License
 │    ├── 132372  Add NPP to Facility PA Question Logic
 │    ├── 132374  Add NPP to Facility Prevent Duplicate
 │    └── 132366  Add NPP to Facility PA = NO Path
 ├── 132345  Manage Specialties
 │    ├── 132346  Specialty Card display and edit
 │    ├── 132349  Read-only behavior and hover
 │    ├── 132347  Add unrecognized specialty
 │    └── 132348  Validate required specialty or …
 └── 132277  Demographic Card
      ├── 132282  Maintain Existing Add New …
      ├── 132279  Make Designated Demographic Fields Optional
      ├── 132281  Prepopulate Provider Demographic
      └── 132280  Maintain Validation Rules for …
```

---

## 132600 — PAF to Cactus sync  (orange)

```
132600  PAF to Cactus Credentialing Record Creation
 ├── 132601  Create Provider Records in Cactus
 │    └── 132602  Create Provider Records in Cactus
 ├── 132647  NPI Record
 │    └── 132648  NPI Record
 ├── 132678  Sanction Check
 │    ├── 132679  Sanction Check
 │    ├── 132681  Attach Sanctions Documentation
 │    └── 132680  Create Sanction Check Record
 ├── 132641  State License Records
 │    ├── 132646  Process State Licenses
 │    ├── 132643  Create State License
 │    ├── 132645  Attach State License
 │    ├── 132642  Validate State License
 │    └── 132644  Preserve Existing Active
 ├── 132631  Specialty Records
 │    ├── 132615  Create APP-Other Specialty for …
 │    ├── 132632  Create Primary Specialty for …
 │    └── 132634  Create Specialty for Existing
 ├── 132627  Primary Address
 │    ├── 132628  Record Create New Provider
 │    └── 132629  Record Preserve Existing
 └── 132608  Entity Assignment
      ├── 132622  Entity Assignment Record
      ├── 132626  Record Preserve Historical
      ├── 132624  Record – Set Default Entity
      └── 132604  Provider Record Preserve Existing …   (card truncated)
```

---

## 132591 — Auto-Acceptance

```
132591  Auto-Acceptance and Automated Processing
 ├── 132592  Auto-Acceptance and Automated Processing
 ├── 132593  Automated Processing Route
 ├── 132594  Automated Processing Record
 ├── 132595  Automated Processing Create
 ├── 132596  Automated Processing Attach
 ├── 132597  Automated Processing Attach
 ├── 132598  Automated Processing Display
 └── 132599  Automated Processing Record
```

---

## 132687 — Reports

```
132687  NPP: Reports
 └── 132688  Reports Monthly Add NPP Request Activity …
```

---

## Box-down (features only)

```
                         +-------------+
                         |    52350    |
                         +------+------+
                                |
     +----------+----------+----+----+----------+----------+
     v          v          v         v          v          v
 +-------+  +-------+  +-------+ +-------+  +-------+  +-------+
 |131724 |  |131728 |  |132600 | |136871 |  |131783 |  |132591 |
 |Cactus |  |RBAC   |  |CACTUS | |Entry  |  |CPC    |  |Auto-  |
 |values |  |       |  |sync   | |[IMPL] |  |type   |  |accept |
 +-------+  +-------+  +-------+ +-------+  +-------+  +-------+
     v          v          v         v          v          v
 +-------+  +-------+  +-------+ +-------+  +-------+  +-------+
 |131785 |  |132255 |  |132537 | |131794 |  |53658  |  |132687 |
 |No-NPI |  |PAF    |  |CVI    | |Add New|  |Search |  |Reports|
 |       |  |action |  |       | |card   |  |[HELD] |  |       |
 +-------+  +-------+  +-------+ +-------+  +-------+  +-------+
     v          v
 +-------+  +-------+
 |132247 |  |132275 |
 |Exist. |  |PAF    |
 |card   |  |Tasks  |
 +-------+  +-------+
```

---

## Cross-links that are **not** Parent (from graph geometry)

| From | To | Likely ADO link | Do not treat as |
|---|---|---|---|
| 53658 | 136871 | Successor / Predecessor | 136871 is **not** a child of 53658 |
| 53658 | 132539, 132540, 132589, 132590 | Related (same CVI stories also hang from 132537) | 53658 CVI children |
| 132537 | those CVI stories | Child | — |

---

## Uncertainties (card text cut off)

Confirm on Azure DevOps before using as AC:

- 132252 vs 13225x for “Allow Entry of Existing Email Addresses”
- 132265 vs 13226x for “Enforce requiredness for ADD NPP”
- 132604 exact title under Entity Assignment
- Whether 132248/132250/132253 are children of **131794** or **132247** (or both via Related)
- Full 136872 story list is **missing from this PNG** (136871 not expanded)

---

## What this does **not** change

- Office implementation of 136871 / 136872 stories
- Hold on 53658 / 127104 exception audit
- Rule: no Copilot implementation of title-only tickets until AC is captured

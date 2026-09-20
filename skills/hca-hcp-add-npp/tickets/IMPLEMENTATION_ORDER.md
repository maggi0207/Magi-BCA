# 52350 — Implementation order (epic by epic)

**Rule:** This is a **capture-then-implement** order. Titles are labels only. Do **not** code from a title. For each wave, paste the epic/feature **plus every child** (description, AC, tasks). Cursor maps that AC to office evidence, then one Copilot prompt.

**How we work each epic**

```text
You paste real ADO content for ONE epic
      ↓
Cursor reads AC against V5 + office evidence
      ↓
Inspect-only Copilot prompt (if files unknown)
      ↓
Office inspect → Cursor gap
      ↓
Implement Copilot prompt (one story at a time)
      ↓
You return diff / build / tests
      ↓
Next story in that epic, then next epic
```

Do not skip inspect when the office files for that epic are not already in this workspace.

---

## Status now

| Status | What |
|---|---|
| **DONE (office)** | **136871** / **136872** stories: 136873 button, 136876 tooltip, 136878 NPI search URL, 136882 preserve Begin PAF, 136883 MSP-only + NPP URL redirect |
| **ADO Done** | **131724** Cactus credentialing values (Links tab). Treat as closed unless a child still has HCP work |
| **CLOSEOUT** | 136883 **manual checks**; mark 136871/136872 **Done** in ADO after UAT; deploy remains the 53658 gate |
| **HELD** | **53658** / **127104** — NPI search reuse already satisfied by 136878; exception-search **audit** deferred. Do not merge No-NPI into 127104 |
| **NEXT** | **132255 complete (AC).** Office inspect PAF Action & Facilities, then implement **132256** first. |

Related **144239** is not a child. Ignore unless ADO says it still has unique AC.

---

## Recommended next paste

**Feature `132255` and all four children are captured.** Do not paste more 132255 AC unless ADO adds scenarios. Next is **office inspect**, not another epic.

| ID | Title | AC in workspace? |
|---|---|---|
| 132255 | Create “ADD NPP to Facility” PAF Action Option | **Yes** |
| 132256 | Display New PAF Action for MSP User – ADD NPP to Facility | **Yes** |
| 132257 | Restrict Action to Eligible Practitioners | **Yes** |
| 132258 | Apply Existing PAF Task Determination Rules | **Yes** |
| 132259 | Enforce Single PAF Action Selection | **Yes** |

**Why not code yet:** office has not inspected **PAF Action & Facilities**. Do not invent MSP role check, eligibility query, submit API, or task-determination class.

**Implement order after inspect:** 132256 (radio + MSP) → 132257 (eligibility hide) → 132259 (single-select + server guard) → 132258 (map action to New vs Existing determination).

---

## Wave list (do in this order)

```text
0  CLOSE  136871 entry          office done — manual close
1  SKIP   53658 / 127104        HELD
2  CAPTURED  132255 PAF Action     all child AC in workspace
2a NEXT      office inspect        then implement 132256 first
3         131794 + 132247       practitioner cards (net-new / existing)
4         131785                No-NPI exception (own epic; not 127104)
5         132275                PAF Tasks (largest)
6         131728                NPP RBAC (move earlier if AC blocks UI)
7         131783                CPC PAF-type filter
8         132591                Auto-accept / automated processing
9         132537                CVI Creation
10        132600                PAF → CACTUS sync
11 LAST   132687                Reports
          131724                already ADO Done — do not reopen from title
```

User journey this order follows:

```text
MSP → Begin NPP PAF → NPI search     [DONE]
         → (No NPI exception)        [wave 4]
         → Add New / Existing card   [wave 3]
         → PAF Action                [wave 2 CAPTURED / children pending]
         → PAF Tasks                 [wave 5]
         → Submit / Auto-accept / CPC
         → CVI → CACTUS → Reports
```

Wave 2 before wave 3 is **implementation** order (host action before card rules). If 132255 AC says the card is in-scope there, stay on 132255 and do not jump to 131794.

---

## Wave 0 — Close 136871 (do not recode)

| ID | Title | Status |
|---|---|---|
| 136871 | Begin NPP PAF Entry Point | Office implemented; ADO still To do |
| 136872 | Entry point on MSP Dashboard | Feature container |
| 136873 | Dashboard action / button | IMPL |
| 136876 | Tooltip | IMPL |
| 136878 | Launch Enforce NPI Search | IMPL (`/Practitioner/Search/Npp`) |
| 136882 | Preserve Begin PAF | IMPL — keep as regression gate |
| 136883 | Security / direct URL | IMPL in repo — **manual checks remaining** |

**You do:** MSP-only button, tooltip, NPP URL vs Begin PAF, unauthorized `/Practitioner/Search/Npp` → home. Then ADO Done.

---

## Wave 1 — Search (do not start)

| ID | Title | Status |
|---|---|---|
| 53658 | NPP: PAF - Search | **HELD** |
| 127104 | Practitioner Search – Add New Practitioner | **HELD** |

NPI-required search is already the 136878 path. Exception **audit** is deferred. Exception **screen** belongs to **131785**, not this wave.

Revisit 53658 only if you later want that audit story, or 127104 AC still has unique work after 131785 is captured.

---

## Wave 2 — 132255 PAF Action (feature captured)

Nine-section: [52350/132255_ADD_NPP_PAF_Action_Option/132255.md](52350/132255_ADD_NPP_PAF_Action_Option/132255.md)

Exact radio label: **ADD NPP to Facility**. MSP + (net new **or** existing inactive with selected entity). Reuse task determination. Single action.

Screenshot: `screenshots/PAF_Action/01_PAF_Action_Facilities_ADD_NPP.png` (**New** badge is UI Evidence, not AC).

Story order after child AC is in:

1. **132256** Display action for MSP — **AC captured** (inspect Practitioner Action, then implement first)  
2. **132257** Restrict to eligible practitioners — **AC captured** (SHOW net new or existing inactive at entity; HIDE active / other)  
3. **132258** Existing task-determination rules — **AC captured** (after 132256 exists + engine named)  
4. **132259** Single PAF action selection — **AC captured** (same radio group + server reject if multiple posted)

**132257 AC:** SHOW if net new or existing inactive with entity. HIDE if active with entity or other ineligible. Combined with 132256: MSP **and** eligible. How inactive/entity is computed is office evidence.

**132259 AC:** selecting ADD NPP is the only selected option; switching deselects the previous action; all actions stay single-select; prevent submit + log if multiple selections (UI/browser).

---

## Wave 3 — Practitioner cards

Capture **both** together (graph emails overlap). Implement only after 132255 identity is known.

**131794** Modify Add New Practitioner Card

| ID | Title (graph) |
|---|---|
| 131795 | Configure NPP-Specific Field Requirements |
| 132248 | Remove Email Requirement for Existing |
| 132250 | Prevent Modification of Existing Email |
| 132252 | Allow Entry of Existing Email Addresses *(ID truncated on card)* |
| 132253 | Validate Email Format When Entered |

**132247** Verify Practitioner Information Card – Existing Practitioner  

Confirm on ADO Links whether 132248/132250/132253 are children of 131794, 132247, or Related to both.

Likely story order: net-new requiredness (131795) → existing-card read-only/email rules (132247 family).

---

## Wave 4 — 131785 No-NPI exception

Own feature. Do **not** implement under 127104.

| ID | Title (graph) |
|---|---|
| 131786 | Create Exception Search Screen for Practitioners |
| 131787 | Implement Exception Search Validation |
| 131788 | Search Practitioner Records Using … |
| 131790 | Implement State Lookup Integration |
| 131791 | Provide Search and Cancel Actions for Exception |

Can be captured in parallel with wave 2 **after** 132255 is pasted, if you want two ADO exports in flight. Implement after NPI search is stable (already true).

---

## Wave 5 — 132275 PAF Tasks (largest)

Do **not** paste this until 132255 AC is mapped. Inside the epic, implement **hosts then cards** (graph hang):

1. **132260** Task card host → requiredness / suppress Delegate+FSQ / display  
2. **132262–132264** visibility, launch, status  
3. **132277** Demographic Card  
4. **132283** Manage Address  
5. **132345** Manage Specialties  
6. **132352** ADD NPP to Facility Task Card (VA/PA, license, PSV, duplicate)

**132352** is the V5 business core. Do not start it before the task host exists.

---

## Wave 6 — 131728 NPP RBAC

| ID | Title (graph) |
|---|---|
| 131728 | Enable HCP Access and Role-Based Security for NPP |
| 131729 | Configure Role-Based Access Control |

Not a substitute for 136883. Capture early if you suspect new roles; implement when AC names roles/policies. **Do not invent authorization policies.**

---

## Wave 7 — 131783 CPC filter

| ID | Title (graph) |
|---|---|
| 131783 | PAF Type Selection and Filtering in CPC Dashboard |
| 131784 | Add “Add NPP to Facility” PAF Type to … |

Needs the type/action from **132255** to already be stored and visible on CPC.

---

## Wave 8 — 132591 Auto-acceptance

| ID | Title (graph) |
|---|---|
| 132592–132599 | Route / Record / Create / Attach / Display (titles truncated) |

After submit predicates exist (wave 5). V5 still **conflicts** on VA+PA=YES auto-accept vs CPC. Product must resolve; do not pick a side from the title.

---

## Wave 9 — 132537 CVI Creation

| ID | Title (graph) |
|---|---|
| 132539 | CVI Creation Auto-Accept PAF |
| 132540 | CVI Creation Display CPC Processing |
| 132589 | CVI Creation Automatically Create CPC |
| 132590 | CVI Creation Attach PAF PDF to CVI Instead of … |

Needs **131724** values (ADO Done) and real CACTUS RTKs from office. **Do not invent RTKs.**

---

## Wave 10 — 132600 PAF → CACTUS

After CVI/submit path is real. Graph hosts then records:

Provider → NPI → Sanctions → State License → Specialty → Primary Address → Entity Assignment.

Net-new vs existing preserve rules are in those child titles. Confirm AC before any `CactusUpdater` change.

---

## Wave 11 — 132687 Reports (last)

| ID | Title (graph) |
|---|---|
| 132688 | Reports Monthly Add NPP Request Activity … |

Needs an indexed NPP type (wave 2/7) and CVI type (wave 9).

---

## What not to do

- Do not implement **132275** from titles while 132255 **child** AC is missing.  
- Do not treat **131785** as leftover 127104 work.  
- Do not treat **131728** as 136883.  
- Do not start **132591 / 132537 / 132600** before the PAF can submit.  
- Do not reopen **131724** from the truncated title (ADO Done).  
- Do not invent RTKs, audit actions, or authorization policies.

---

## Checklist — 132255 capture complete

- [x] 132255 description + AC
- [x] 132256 full AC + tasks
- [x] 132257 full AC (no Tasks section in paste)
- [x] 132258 full AC + tasks
- [x] 132259 full AC + tasks

Next: office inspect of PAF Action & Facilities. No Copilot **implement** prompt until that evidence is back.

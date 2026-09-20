# ADD NPP to Facility — Final Requirements (Source of Truth)

**Working business source of truth** for ADD NPP implementation.

This is **not** a dump of the V5 Word file and **not** a dump of Azure DevOps. It is the curated requirement set after reading V5 and comparing captured `52350` tickets.

| Layer | Governs | Does not govern |
|---|---|---|
| This document (from V5 + captured tickets) | What the product must do | How HCP code is structured |
| Azure DevOps `52350` tickets | Scope, sequencing, UI/entry-point AC | Full NPP business rules |
| HCP source repository | Files, classes, methods, current behavior | Business rules |

Original V5 artifact (office/Cursor knowledge base):

`HCP NPP PAF Type Requirements V5 6.18.26.docx`  
Parallon HCP Non-Privileged Practitioner (NPP) PAF Requirements, Version 5 – 06/2026, Credentialing Product Development.

Do not invent repository paths, APIs, or existing code behavior from this file.

---

## 0. How to read this document

Every rule is tagged:

| Tag | Meaning |
|---|---|
| **V5** | Stated in the V5 requirements document |
| **Ticket** | Stated on a captured Azure DevOps work item |
| **Both** | Same rule appears in V5 and a captured ticket |
| **Conflict** | Sources disagree — do not pick a side in code without product confirmation |
| **Title only** | `52350` child exists by title; no AC captured — do not invent |
| **Not in V5** | Ticket-only; V5 does not define this UI/control |

If a later V5 section contradicts an earlier V5 assumption, both are listed under **Conflicts**. Do not silently “fix” V5.

---

## 1. Business problem and PAF type

**V5.** CPC wants a new PAF type: **ADD NPP to Facility**.

In some cases CPC is not responsible for the Non-Privileged Practitioner (NPP) process (examples in V5: facility does not use Meditech 5.x to register patients; NPP must be faster than the 3-day timeframe; state law requires validations outside current CPC scope). The facility/MSP performs the required undefined-practitioner (UDP) verifications. The MSP uses **ADD NPP to Facility** to send PSV evidence so CACTUS records can be created or updated. PSV is stored in CACTUS so CPC can monitor applicable licenses and sanctions.

**V5.** CPC does not perform credentialing verifications on ADD NPP practitioners.

**V5.** ADD NPP is not privileged-practitioner packet credentialing.

---

## 2. Assumptions that constrain design

**V5.**

- Only **MSP** HCP roles submit ADD NPP PAF type work items.
- Submitters understand when this PAF selection applies.
- **Not** part of PAF merging.
- **Not** part of packet determination or packet building.
- Credentialing packets are **not** created for ADD NPP.
- Two routing branches: existing vs net-new practitioner.
- **Net new:** not in CACTUS.
- **Existing Active:** in CACTUS with at least one active facility where the practitioner holds privileges.
- **Existing Inactive:** in CACTUS with no active facilities.
- CPC routing of submitted ADD NPP follows the **same path/logic as Add Practitioner to Facility** (process path — not identical business rules).
- Audit logs for all system updates.
- Duplicate checks against CACTUS.
- PAF will be auto-accepted (see Conflicts for exception wording).
- Open/Completed queues: ideally filter by PAF type ADD NPP.

**V5 assumption vs later V5 requirement — Conflict:** early Assumptions say “CVIs will not be created for ADD NPP to Facility work items.” The later **CVI Creation** section requires a CVI when the workflow routes to CPC. Treat later CVI rules as the detailed requirement; do not delete the assumption — product should confirm.

---

## 3. End-to-end business flow

```text
MSP only
  → entry (Ticket: Begin NPP PAF on MSP Dashboard)
  → practitioner search (Both: NPI required; Ticket: reuse Enforce NPI Search)
  → net-new OR existing inactive with entity (V5)
  → select ADD NPP to Facility action (V5)
  → PAF tasks (V5)
  → ADD NPP card: Practitioner Type = NPP; VA / PA; license; PSV (V5)
  → Review & Submit (V5: blocked while required work incomplete)
  → auto-accept and CACTUS updates  OR  CPC / CVI path (V5; see Conflicts)
  → PAF PDF + history + audit (V5)
  → monthly reporting (V5)
```

Ticket **53658** says epic **136871 must be completed and deployed** before the search feature is developed, tested, or deployed.

---

## 4. MSP Dashboard entry (not in V5)

V5 assumes an MSP can start an ADD NPP PAF. It does **not** specify a dashboard button named Begin NPP PAF.

Captured tickets under epic **136871** / feature **136872** own this slice.

| Rule | Source | Requirement |
|---|---|---|
| Add **Begin NPP PAF** adjacent to existing **Begin PAF** | Ticket 136873 | Both actions visible on MSP Dashboard; NPP control matches existing styling; keyboard and screen-reader accessible |
| Approved tooltip | Ticket 136876 | Exact text: `Use this option only when submitting a request for a Non-Privileged Practitioner (NPP).` Show on hover/focus; hide when hover/focus ends. Extra mockup sentence about launching NPI Search is **not** approved unless product confirms |
| Click routes to **existing Enforce NPI Search** (PSG/Recruitment workflow) | Ticket 136878, 136872, 53658 | Reuse; do not build a second search workflow |
| Existing **Begin PAF** unchanged (click, search fields, validations) | Ticket 136882, 136872, 53658 | Regression boundary |
| Only authorized MSP users; hide is not enough; block direct URL | Ticket 136883, 136872 | Reuse existing MSP security; do not affect PSG/Recruitment access to Enforce NPI Search |
| Related 144239 | Title / related only | Do not treat as a second implementation of 136872 |

**V5 alignment:** MSP-only submission; submitters understand when the option applies. Tooltip text is ticket-owned, not V5.

---

## 5. Practitioner search

### 5.1 Standard NPI search — Both

- NPI is the primary and required search criterion.
- Standard search requires a **valid 10-digit NPI**.
- Invalid length/format: reject; search does not run (**Ticket 127104**).
- Reuse existing search — **no new NPI search logic** (**Ticket 127104**, **53658**).
- Existing Begin PAF search must stay unchanged (**Ticket 136882**, **53658**).

**V5 (Add New Practitioner card, Individual NPI field):** required; 10 digits starting with **1 or 20**; duplicate check. That field rule is for the **card**, not a restatement of search validation. Do not invent a “starts with 1 or 20” search rule unless V5 search text is later found to say so.

### 5.2 Controlled no-NPI exception — V5 (detail) vs tickets (reuse)

**V5.** Link: `No NPI available? Search without NPI`. Routes to exception search.

**V5 exception fields**

| Field | Rule |
|---|---|
| Reason | Required. Dropdown includes: Practitioner does not have NPI |
| Last Name | Required. Minimum **2** alpha characters |
| First Name | Required. Minimum **1** alpha character |
| State License Number | Alphanumeric + special characters; HCP-to-VC lookup may ignore special characters; **like** match, not exact character-for-character; minimum **2** characters |
| State of License | Required. Source `LOOKUP_STATE`. Same state lookup as other HCP state fields |

**V5.** Search stays disabled until Reason, First Name, Last Name, State, and License Number meet the rules above. Actions: Search; Cancel returns to Practitioner Search.

**Ticket 53658 / 127104.** Use the **existing** controlled exception workflow; exception search without minimum criteria is blocked; exception searches are **audit logged**; do not change NPI validation or audit schema.

**Ticket 131785** — title only: Controlled “No NPI Available” Exception Workflow for NPP. Do not invent extra AC.

**Conflict — exception criteria:** V5 names Reason + First + Last + State License Number + State. Ticket **127104** says reuse existing Add New Practitioner / Enforce NPI Search criteria and not introduce new search logic. Existing UI help (screenshot evidence, not V5) mentions Last Name plus at least one of First Name, DOB, SSN, or Email. **Do not replace existing exception fields with V5 fields, and do not ignore V5, until repository evidence plus product confirm which set is already implemented.** Implementation default from tickets: **reuse existing**. Record V5 fields as the business exception spec if a gap is confirmed.

---

## 6. Eligibility and PAF action — V5

- New PAF action radio: **ADD NPP to Facility**, **MSP only**.
- Available only if practitioner is **net new** or **existing and currently inactive with the entity**.
- Same PAF-task determination criteria as adding a practitioner to a facility (new vs existing), except ADD NPP-specific cards below.
- Action appears **directly below the last available PAF type** in the Practitioner Action list.
- MSP may select **only one** PAF action at a time.

**Title only:** `132255` Create “ADD NPP to Facility” PAF Action Option. Use V5 above; do not invent extra AC from the title.

---

## 7. PAF tasks — V5

New required task card **ADD NPP to Facility**.

| Item | Value |
|---|---|
| Title | ADD NPP to Facility |
| Description | Upload and complete required Non-Privileged Practitioner (NPP) verification information and supporting PSV documentation. |
| Required | Always, regardless of practitioner type |
| Action | Work — opens ADD NPP workflow |
| Review & Submit | Unavailable while required items incomplete |
| Visibility | Only MSP users who can use the ADD NPP action |

**Suppress for this PAF type:** Delegate card; Facility Specific Questions card.

| Practitioner | Required cards | Optional cards |
|---|---|---|
| Net new | Demographic, Manage Addresses, Specialties, ADD NPP to Facility | — |
| Existing | ADD NPP to Facility | Demographic, Manage Addresses, Specialties (specialty becomes required if no primary or primary inactive — §9) |

Placement/styling: same as existing PAF task cards; next available grid position.

**No impact (V5):** existing PAF task cards; existing task routing; existing demographic workflows; existing packet workflows unless specified.

**Title only:** `132275` PAF Tasks. Use V5; do not invent extra AC.

---

## 8. Add New Practitioner / Verify Practitioner cards — V5

### Net new — ADD New Practitioner

Existing functionality remains except:

**Optional:** Email, Cell, DOB/SSN, Gender.

**Required**

| Field | Requirement |
|---|---|
| First Name | Required, 20 characters |
| Last Name | Required, 35 characters |
| Suffix | Not required, 5 characters |
| Degree | Required, 35 characters |
| Provider Category | Required, dropdown `Provider.Category_RTK` |
| Individual NPI | Required, 10 digits starting with 1 or 20; duplicate check |

**Title only:** `131794` Modify Add New Practitioner Card.

### Existing — Verify Practitioner Information

- Email remains optional.
- If email already exists: **read-only**; cannot edit, overwrite, or remove.
- If blank: editable and optional; format validation only when a value is entered; blank does not block **Start PAF**.
- “Update Email Address” section remains visible.
- New-practitioner email behavior unchanged unless specified here.

**Title only:** `132247` Verify Practitioner Information Card – Existing Practitioner.

---

## 9. Demographic task card — V5

Same optional fields as §8 (Email, Cell, DOB/SSN, Gender). If an optional field is completed, apply the same validation as when that field is required.

If present in CACTUS, prepopulate:

| Field | V5 source |
|---|---|
| First Name | PROVIDERS.FIRSTNAME |
| Middle Name | PROVIDERS.MIDDLENAME |
| Last Name | PROVIDERS.LASTNAME |
| Suffix | PROVIDERS.SUFFIX |
| Degree | PROVIDERS.DISPLAYDEGREES_SHORT |
| Provider Category | PROVIDERS.CATEGORY_RTK |
| Individual NPI | PROVIDERS.NPI |

All other demographic logic follows ADD New Practitioner PAF unless called out here.

---

## 10. Addresses — V5

Do **not** show Home, Credentialing, or Alternate addresses.

**Primary address**

| Situation | Behavior |
|---|---|
| Active primary address **and** affiliated with one or more facilities | Show address; disable Add Address; message: `The practitioner currently holds privileges at one or more HCA facilities. The primary address cannot be changed.` |
| No primary address | Show Add Address |
| Active primary address but **no other** active affiliations | Show Add Address |

When adding an address, **suppress** the group-address note (group addresses are not available for this PAF type).

**Add Address fields**

| Field | Requirement |
|---|---|
| Contact | Optional, 60 |
| Address | Required, 50 |
| Address Line 2 | Not required, 50 |
| City | Required, 40 |
| State | Required, dropdown |
| Zip Code | Required, 11 |
| Country | Required, dropdown |
| Phone | Required, `(nnn)nnn-nnnn` |
| Phone Extension | Not required, 10 |
| Fax | Required, `(nnn)nnn-nnnn` |
| Fax Extension | Not required, 10 |

Validation display follows **Add Practitioner to Facility**.

---

## 11. Specialties — V5

| Practitioner | Card |
|---|---|
| Net new (new to CACTUS) | Required |
| Existing | Optional, **unless** no Primary Specialty **or** `PROVIDERSPECIALTIES` active = False |

If a primary specialty is present and active: **no edits**. Hover on disabled field: `The practitioner currently holds privileges at one or more HCA facilities. The primary specialty cannot be changed.`

Display `PROVIDERSPECIALTIES.SPECIALTY_RTK` only. **Do not display** secondary or alternate specialties. Otherwise suppress Select Specialty so the user cannot edit.

**Unrecognized specialty / PPI**

- Prompt: `Please list non-specialty/board areas of professional practice interest or focus (HIV/AIDS, etc.).`
- Comment box, **500** characters, directly below primary specialty.
- Extra instruction: `If you are unable to locate the specialty under primary specialty, please enter the professional practice interest in the comment box below.`
- User must provide **primary specialty OR** professional practice interest.
- If both blank: `A primary specialty or professional practice interest or focus is required. Please provide either a primary specialty or a professional practice interest or focus.`

**CACTUS specialties**

- Existing **Active** practitioner: no specialty updates unless no primary specialty record exists.
- Net new or inactive: if primary selected — Specialty_RTK = first selected; type Primary `PRSPTYPRIM`; Active.
- If PPI: specialty **APP-Other**; status **Not Certified** (`SPECIALTYSTATUS_RTK` `DS9Q12M4UO` in V5).

---

## 12. ADD NPP to Facility card — V5

Verbiage:

- `You have requested to ADD a Non Privileged Practitioner (NPP) at this facility, please upload the following primary source verifications (PSV) requested below.`
- `DISCLAIMER: ADD Non Privileged Practitioner (NPP) Provider Action Form (PAF) should only be used when the CPC is not currently performing the NPP verifications for this provider at your facility.`

**Practitioner Type:** NPP only; required; one option; validation `This field is required.`

### VA / PA

Question: `Is the practitioner an active duty military member or practicing at the VA?` Yes/No.

| VA | Next |
|---|---|
| YES | Ask `Is the Practitioner a PA?` (required Yes/No) |
| NO | Hide PA question; show State License |

**VA = YES and PA = YES**

- Hide State License.
- Require Sanctions PSV and NPI PSV.
- Route to CPC; create CVI (CVI section).
- **Conflict in V5:** one paragraph says PAF **will auto accept** then route/create CVI; Workflow 1 says PAF **will not auto accept**. CVI Creation later says on CPC-route submission the PAF **shall be automatically accepted**. Do not implement both; confirm with product. Prefer the detailed CVI/Auto-Acceptance sections unless product says otherwise.

**VA = YES and PA = NO**

- Show State License; all license fields required.
- Date must be ≥ today; message: `Date must be greater than or equal to today`.
- Require License PSV, Sanctions PSV, NPI PSV.

**VA = NO (V5 heading: net new providers)**

- Skip PA question; show State License; same date and PSV rules as VA+PA=NO.

**Existing practitioner licenses**

- Recall active/existing licenses; show State, License Number, Expiration Date.
- Link: `None of these apply. Add New State License Instead.` → then Workflow 2 (new license) rules.
- Recalled existing records **bypass standard validation**.
- If selected license is not Active **or** expiration is expired/matured: route to CPC; message: `You have selected a license that is either inactive or expired and will require review by the CPC. Once the CPC has reviewed the license, it will be updated accordingly. Please do not add another license.`
- New license State + License Number matching a displayed existing license: block create; message: `A license with this State and License Number already exists for this practitioner. Please select the existing license record displayed above.`; return to the list. No duplicate State + License Number.

### State match for CA, LA, NV, KY, TX

For Workflow 2 and 3: license state must match entity/facility state. Else block submit: `A state license in the state of ‘XX’ is required for PAF submission.`

### License fields

| Field | Requirement |
|---|---|
| State | Required, dropdown, 2 characters |
| Effective Date | Required, MM/DD/YYYY |
| License Number | Required, 20 characters |
| Status | `STATUS_RTK` values only |
| Expiration Date | Required, MM/DD/YYYY |
| Field of Licensure | Required |

**STATUS_RTK (V5 codes)**

| Label | V5 RTK |
|---|---|
| Active | DRFB0IQ54S |
| Temporary Permit | D2H11CCT3V |
| Active Military | D3UK1E0UF6 |
| Active-Compact | M5890K2GGY |

Hover for Active Military: `Use this type for civilian practitioners working for the VA.`

### PSV uploads

Required where the workflow requires that PSV. Accepted: **DOC, DOCX, PDF, JPG, TIFF**. Reject unsupported types. V5: store/attach in PDF (HTML) form; attach to the correct CACTUS record; audit image entries.

### Routing (V5)

- PA = YES → CPC.
- License status not Active → CPC.
- Expiration expired/matured → CPC.
- Net-new licenses follow all validation rules.
- Recalled existing licenses bypass standard validation.

---

## 13. Auto-acceptance, CPC, CVI — V5

**Normal (unless a CPC path applies):** CPC does not review/approve after MSP submit; use Auto-Acceptance; after CACTUS updates, PAF goes to **Completed** queue.

**History / audit**

- Completed PAF history user: `HCP System User`.
- CACTUS audit for auto-accepted updates: user `HCP System User – MSP submitter name`; entity `HCA Corporate`.
- PDF history: `HCP System User – MSP submitter name`.

**PDF destination**

- Attach PAF PDF to **Provider Record**, unless a CVI is created — then attach PDF to the **CVI**, not the Provider Record.
- PDF Type displays **ADD NPP to Facility**.

**When V5 says route to CPC / CPC processing** (including military PA and inactive/expired license; V5 also lists NP, Resident, missing credential as examples in the CVI paragraph):

1. On submission, PAF is automatically accepted (see VA+PA Conflict).
2. MSP message: `This PAF requires manual processing by the CPC due to a missing or expired credential.`
3. Create CVI:

| Field | Value |
|---|---|
| CVI Type | Facility Undefined Practitioner |
| Initial Status | UDP Facility Request |
| Closure status | UDP Facility Request Complete |
| Started / Received | Date/time of PAF auto-acceptance |
| CVI Notes | The PAF criteria that triggered CPC processing |
| MSO Due Date | Auto-acceptance date + **3 business days** |
| CVI Group Name | NPP Date of PAF Auto Acceptance |

CVI stays UDP Facility Request until CPC finishes, then UDP Facility Request Complete.

**Title only:** `132591` Auto-Acceptance; `132537` CVI Creation. Use V5; do not invent extra AC.

**V5 script request:** auto-close CVIs with status UDP Facility Request Complete (credentialing database script — not an HCP UI story unless a ticket says so).

---

## 14. CACTUS updates — V5 (what, not how)

Do not invent HCP APIs from these table names. They are V5 data requirements.

**Provider — net new:** First Name, Last Name, Suffix, Degree, Provider Category, Individual NPI (PROVIDERS.* as in V5).  
**Provider — existing:** no changes.

**Entity assignment (facility the MSP selected)**

- Net new: existing assignment logic unless noted.
- Security: Internal Use Only unless the NPP had existing privileges at that entity (inactive entity with start and end date — leave as is).
- Status **NPP – Active** (`STATUS_RTK` / `ENTITYASSIGNMENTS` `M67G0Q3AIP` in V5) if status is not currently set.
- Do **not** update status if it is other than: None or N/A; NPP – Inactive; NPP – Suspend; Inactive.

**Address:** if no existing record, create from PAF (primary). Existing address: no changes.

**License:** match existing on type (state), number, state. Do not update prepopulated license on accept when practitioner is active at another facility. If existing and currently **inactive**, follow net-new license create rules. New license: duplicate check; V5 flags including PersistentVerification, Type_RTK `NPDSTATEx`, NPDBPrimaryLicense, LEMM enrollment status “Requesting – Ready to enroll in LEMM”. Image name pattern: first 13 of last + first 3 of first + state + `_Lic_` + MMDDYY (submit date). Type Medical License. Notes: `Uploaded via auto PAF for ADD NPP to Facility`.

**NPI image:** V5 heading says attach to Provider Record. Name: first 13 last + first 3 first + `_NPI_` + MMDDYY. Type NPI. Same notes. (One V5 line says “Attach license image” under NPI — treat as wording error; image type is NPI.)

**Sanctions:** find/create Sanction Check on V5 license RTK; attach sanctions image. Name: `..._Sanctions_MMDDYY`. Type Sanction Checks.

**Title only:** `132600` PAF to Cactus sync; `131724` Cactus credentialing values. Use V5; do not invent extra AC.

---

## 15. Reporting — V5

Monthly, beginning of each month, to Reporting Team and CPC Distribution List:

- Total ADD NPP requests received
- Total routed to CPC
- Percentage routed to CPC (if applicable)

**MOR:** exclude CVI Type **Facility Undefined Practitioner** from MOR statistical CVI counts.

**Title only:** `132687` Reports.

---

## 16. Ticket map (captured vs this document)

| Ticket | Owns | In V5? |
|---|---|---|
| 52350 | Project container | — |
| 136871 / 136872 | Begin NPP PAF entry feature | Not in V5 (MSP-only is) |
| 136873 | Dashboard button + a11y | Not in V5 |
| 136876 | Tooltip sentence | Not in V5 |
| 136878 | Route to existing Enforce NPI Search | Search reuse implied; route is ticket |
| 136882 | Preserve Begin PAF | Not in V5 |
| 136883 | MSP auth + direct URL | MSP-only is V5; URL rule is ticket |
| 144239 | Related entry story | Not captured |
| 53658 | Search feature; blocked on 136871 | NPI + exception overlap V5 |
| 127104 | Reuse Add New Practitioner search | Overlap + exception **Conflict** |
| 131785, 132255, 132275, 131794, 132247, 132591, 132537, 132600, 131724, 132687, 131728, 131783 | Title only under 52350 | Do not invent AC |

**Title only security/CPC dashboard:** `131728` RBAC; `131783` CPC dashboard PAF type filter. V5 already requires MSP-only action and ADD NPP as a PAF type. Do not invent UI/filter AC.

---

## 17. Explicit non-goals

**V5 / tickets.**

- Do not merge ADD NPP PAFs.
- Do not create credentialing packets or run packet determination.
- Do not require Delegate or Facility Specific Questions cards.
- Do not change Begin PAF.
- Do not duplicate Enforce NPI Search / Add New Practitioner search (**tickets**).
- Do not treat ADD NPP as privileged-practitioner credentialing.
- Do not assume every exception creates a CVI beyond V5 CPC-route cases.
- Do not overwrite existing CACTUS provider/address/active-at-another-facility license unless V5 says so.

---

## 18. Conflicts and stop conditions

Resolve with product (and repository evidence for search) before coding the affected area:

1. **V5 Assumptions:** “CVIs will not be created” vs **V5 CVI Creation** when routing to CPC.
2. **VA + PA = YES:** auto-accept vs will not auto-accept vs CVI section “PAF shall be automatically accepted.”
3. **No-NPI exception fields:** V5 field list vs Ticket 127104 “reuse existing criteria / no new search logic.”
4. Uncaptured `52350` children: titles only — no AC in this file.

---

## 19. Implementation note (not a requirement)

Existing **Add Practitioner to Facility** is the primary HCP pattern to **inspect and reuse** where it already matches this document. Matching the pattern does not override a V5 or captured-ticket rule.

If the HCP repository cannot confirm a rule, report **Repository Evidence Gap**. Do not invent implementation.

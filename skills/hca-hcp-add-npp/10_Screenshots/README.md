# Screenshots

UI captures for NPP reference. These are **current application screens**, not office-repository source files.

Do not treat a screenshot as proof of class names, APIs, or file paths.

## Begin_PAF_Current

Current **Begin PAF** flow captured from local HCP (`localhost:50001`) on 19 September 2026.

This is the workflow that `136882` requires to remain unchanged.

| File | Screen | Visible URL / action | What it shows |
|---|---|---|---|
| [01_MSP_Dashboard_Begin_PAF.png](Begin_PAF_Current/01_MSP_Dashboard_Begin_PAF.png) | MSP Dashboard | `/Msp/Dashboard` | PAFs queue, filters, single **Begin PAF** button. No **Begin NPP PAF** action yet. |
| [02_Practitioner_Search.png](Begin_PAF_Current/02_Practitioner_Search.png) | Practitioner Search | after Begin PAF | Search by NPI, SSN, First Name, Last Name. Help text allows full 9-digit SSN, full 10-digit NPI, or 1-character first name and 2-character last name. |
| [03_Practitioner_Search_No_Results.png](Begin_PAF_Current/03_Practitioner_Search_No_Results.png) | Practitioner Search — no results | same search | No matching practitioner. **Add New Practitioner** is available. |
| [04_Add_New_Practitioner.png](Begin_PAF_Current/04_Add_New_Practitioner.png) | Add New Practitioner modal | `/Practitioner/Search` | Required First Name, Last Name, DOB, Gender, SSN, Email, Cell, Degree, Category. NPI present. **Verify & Start PAF**. |

## Notes from these screens only

- MSP Dashboard currently has **one** initiation action: **Begin PAF**.
- Current Begin PAF search is **not** NPI-only. SSN and name search are available.
- NPP V5 and tickets `136871` / `136872` / `136878` require **Begin NPP PAF** to use the existing **Enforce NPI Search** workflow. That is a different search than the current Begin PAF search shown here.
- Screenshot 04 is the **current Begin PAF** Add New Practitioner form. NPP V5 Add New rules (optional email/cell/DOB/SSN/gender; required NPI) are not proven by this screen.

## 136876_Tooltip

Mockup for `136876` (Begin NPP PAF tooltip). This is a **target mockup**, not the current local dashboard.

| File | Screen | What it shows |
|---|---|---|
| [01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png](136876_Tooltip/01_MSP_Dashboard_Begin_NPP_PAF_Tooltip_Mockup.png) | MSP Dashboard mockup | **Begin PAF** and **Begin NPP PAF** adjacent. Info icon on NPP button. Tooltip visible. |

Approved ticket text:

> Use this option only when submitting a request for a Non-Privileged Practitioner (NPP).

Mockup also shows:

> Selecting this option will launch the NPI Search workflow.

Treat the extra sentence as mockup-only until product confirms it.

## 127104_Practitioner_Search

Existing **Enforce NPI / Add New Practitioner** search UI for `127104` (and reuse target for `136878` / `53658`). This is **not** the current MSP Begin PAF search.

| File | Screen | Visible URL / action | What it shows |
|---|---|---|---|
| [01_Enforce_NPI_Search_NPI_Required.png](127104_Practitioner_Search/01_Enforce_NPI_Search_NPI_Required.png) | Practitioner Search | `https://localhost:5001/Practitioner/Search` | Required **NPI ***. Search disabled-looking until NPI. Link **No NPI available? Search without NPI**. Empty results. Help text: Last Name plus one of First Name, DOB, SSN, or Email. |
| [02_PSG_Search_Reuse_Chat.png](127104_Practitioner_Search/02_PSG_Search_Reuse_Chat.png) | Chat / supporting context | — | “Psg has this search functionality” / “I think we can use the same for msp”. Supporting context only, not ticket AC. |

Office source files are still not in this folder.

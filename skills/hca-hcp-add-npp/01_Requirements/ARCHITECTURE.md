# HCA Credentialing 2.0 — Architecture & Project Guide

> **Audience:** Any developer, architect, or stakeholder joining the project with no prior knowledge.  
> **Goal:** Understand what the system does, how it is structured, how data flows, and how the pieces connect.

---

## Table of Contents

1. [What the System Does (Business Context)](#1-what-the-system-does)
2. [High-Level Architecture Overview](#2-high-level-architecture-overview)
3. [Solution Structure](#3-solution-structure)
4. [Project-by-Project Breakdown](#4-project-by-project-breakdown)
5. [Data Flow — End-to-End Scenarios](#5-data-flow--end-to-end-scenarios)
6. [Authentication & Authorization](#6-authentication--authorization)
7. [Databases](#7-databases)
8. [Messaging — Azure Service Bus Queues](#8-messaging--azure-service-bus-queues)
9. [Azure Functions Summary](#9-azure-functions-summary)
10. [Shared Libraries](#10-shared-libraries)
11. [Key Technologies & Libraries](#11-key-technologies--libraries)
12. [Build & Run](#12-build--run)
13. [Glossary](#13-glossary)

---

## 1. What the System Does

**HCA Credentialing 2.0** is an enterprise medical-staff credentialing platform for **HCA Healthcare**.  
It manages the full lifecycle of credentialing and privileging for practitioners (physicians, nurses, etc.) across HCA facilities.

Core capabilities:

| Capability | Description |
|---|---|
| **Practitioner Profiles** | Store and manage practitioners' personal, license, board, insurance, and specialty data pulled from the CACTUS credentialing database. |
| **PAF (Practitioner Application Form)** | Digital application form that practitioners fill out when applying for privileges at a facility. |
| **Packet (Privilege Packet)** | A set of privilege forms sent to a practitioner for a credentialing cycle. Drives the CVI (Credentialing Verification Items) workflow. |
| **Recredentialing** | Periodic re-verification of practitioner credentials on a committee-driven schedule. |
| **Portal Integration** | Sends links and messages to an external HCA practitioner portal (legacy system) so practitioners can access their forms. |
| **Document Generation** | Renders PDF documents (PAF, Packet, Portal documents) using Aspose. |
| **Image Conversion** | Converts attachments (Word docs, images) to PDF/TIFF and stores them in OnBase (HCA's document management system). |
| **Notifications** | Sends email (via Microsoft Graph) and SMS text messages (via Raven) to practitioners and delegates. |
| **MIP Processing** | Medical Identity Protection — handles special identity-sensitive document workflows. |
| **Audit Logging** | Captures all meaningful user actions across the system. |
| **User Management** | Internal user accounts with roles; also supports HCA AD (federated) login via OpenID Connect. |
| **Delegate Management** | A practitioner may designate a delegate (assistant) who fills out forms on their behalf. |

---

## 2. High-Level Architecture Overview

```
┌────────────────────────────────────────────────────────────────────────────┐
│                         Browser (Practitioner / MSP / Admin)               │
└───────────────────────────────────┬────────────────────────────────────────┘
                                    │ HTTPS
                    ┌───────────────▼──────────────┐
                    │  Blazor WebAssembly Client    │  (Hca.Credentialing.Client)
                    │  Hosted by ASP.NET Core       │
                    │  Server (Hca.Credentialing.   │
                    │  Server)                      │
                    └────────────┬─────────────────┘
                                 │ HTTP (JWT Bearer Tokens)
          ┌──────────────────────┼──────────────────────────┐
          │                      │                          │
          ▼                      ▼                          ▼
  ┌───────────────┐   ┌──────────────────┐   ┌───────────────────────┐
  │ Cactus API    │   │   PAF API        │   │   Packet API          │
  │ (REST)        │   │   (REST)         │   │   (REST)              │
  └───────┬───────┘   └────────┬─────────┘   └──────────┬────────────┘
          │                    │                         │
          │           ┌────────▼──────┐       ┌──────────▼────┐
          │           │  PAF Processor │       │Packet Processor│
          │           │ (Azure Fn)    │       │(Azure Fn)     │
          │           └───────────────┘       └───────────────┘
          │
  ┌───────▼──────────────────────────────────────────┐
  │           Azure Service Bus (HCO Service Bus)     │
  │  Queues: paf-processing, packet-processing,       │
  │  send-email, send-text, paf-attachment-image-     │
  │  conversion, packet-pdf-generation-document,      │
  │  portal-pdf-generation-document, …                │
  └──────────────────────────────────────────────────┘
          │                    │                        │
          ▼                    ▼                        ▼
  ┌──────────────┐  ┌──────────────────────┐  ┌────────────────────────┐
  │  Background  │  │ Document Generation  │  │  Image Conversion      │
  │  Functions   │  │ (Azure Fn)           │  │  (Azure Fn)            │
  │ (Azure Fn)   │  │ Renders PDFs         │  │ Converts to PDF/TIFF   │
  │ Notifications│  │ via Aspose           │  │ Uploads to OnBase/DMS  │
  │ Cleanup, MIP │  └──────────────────────┘  └────────────────────────┘
  │ Recredentialing                                                      │
  └──────────────┘

  External Dependencies:
  ─────────────────────
  • CACTUS DB (SQL Server) — practitioner master record system
  • OnBase / HCO DMS       — document management system
  • Microsoft Graph API    — email via M365
  • Raven API              — SMS text messages
  • Azure Blob Storage     — temporary file storage, email bodies
  • HCA Identity Provider  — Active Directory federated login (OpenID Connect)
  • Portal API             — legacy HCA practitioner portal
```

---

## 3. Solution Structure

The entire solution lives at:

```
Credentialing-2.0/
└── Hca.Credentialing/
    ├── Hca.Credentialing.sln          ← Main solution file
    ├── global.json                    ← .NET SDK version pin
    ├── nuget.config                   ← NuGet feed configuration
    │
    ├── Hca.Credentialing/             ← Main web application (Blazor)
    │   ├── Client/                    ← Blazor WASM frontend
    │   ├── Server/                    ← ASP.NET Core host + IDP server
    │   └── Shared/                    ← Shared models & client HTTP wrappers
    │
    ├── Hca.Credentialing.Api.Shared/  ← Cross-cutting shared library for all APIs
    ├── Hca.Credentialing.Cactus.Api/  ← REST API over CACTUS database
    ├── Hca.Credentialing.Paf.Api/     ← REST API for PAF forms
    ├── Hca.Credentialing.Packet.Api/  ← REST API for Privilege Packets
    ├── Hca.Credentialing.Portal.Api/  ← REST API for Portal integration
    ├── Hca.Credentialing.Background/  ← Azure Functions: notifications, cleanup, MIP, PDF
    ├── Hca.Credentialing.Document.Generation/  ← Azure Function: PDF builder (Aspose)
    ├── Hca.Credentialing.Image.Conversion/     ← Azure Function: image/doc to PDF/TIFF
    ├── Hca.Credentialing.Paf.Processor/        ← Azure Function: async PAF processing
    ├── Hca.Credentialing.Packet.Processor/     ← Azure Function: async Packet processing
    │
    ├── Hca.Credentialing.Tasks/       ← Build-time MSBuild task (WASM compression)
    │
    └── *.Tests/                       ← Test projects mirroring each main project
```

---

## 4. Project-by-Project Breakdown

### 4.1 `Hca.Credentialing.Client` — Blazor WebAssembly Frontend

**Type:** Blazor WebAssembly (WASM) SPA  
**Role:** The user-facing application.

Key pages (under `Pages/`):

| Page Area | Purpose |
|---|---|
| `Admin/` | User management, audit log, alert configuration, error log |
| `Dashboards/` | Role-specific dashboards: Practitioner, Delegate, MSP (Medical Staff Professionals), CPC, CSS, Recruitment |
| `CredentialingItems/` | View and track credentialing items for a practitioner |
| `Paf/` | Fill out and submit PAF (application form), print PAF |
| `Packet/` | Review and submit privilege packet, print privileges |
| `PractitionerManagement/` | Manage practitioner profiles |
| `PractitionerSearch/` | Search across practitioners |
| `Delegate/` & `DelegateManagement/` | Delegate workflows — view/manage assigned practitioners |
| `Facilities/` | Facility-level views |
| `Queues/` | Work queues for MSP and CSS roles |
| `MessageCenter/` | In-app messaging with the portal |
| `Recruitment/` | Recruitment-specific dashboards and reports |
| `Tools/` | Miscellaneous admin tools |

Client-side HTTP calls go to the **Shared** project's typed clients (e.g., `CactusClient`, `PafClient`, `PacketClient`).

**Authentication:** The client uses OpenID Connect (OIDC) with silent token refresh. The `Security/` folder handles session management, token handling, and custom user factory to populate claims.

---

### 4.2 `Hca.Credentialing.Server` — ASP.NET Core Host & Identity Provider

**Type:** ASP.NET Core (Blazor hosted server)  
**Role:** Serves the Blazor WASM, acts as the **Identity Provider (IDP)** using **OpenIddict**, and exposes a small set of controllers.

Controllers:

| Controller | Purpose |
|---|---|
| `AdminController` | Admin operations (user search, role management) |
| `AuditController` | User audit log queries |
| `ErrorLogController` | System error log queries |
| `UserAccountController` | Account actions: login, password reset, TOTP setup |
| `OidcConfigurationController` | Exposes OIDC discovery endpoint |
| `KeepAliveController` | Session keep-alive ping |

**IDP Features:**
- Acts as an OAuth 2.0 / OpenID Connect Authorization Server using **OpenIddict**.
- Issues JWT access tokens with scopes: `api-cactus`, `api-paf`, `api-packet`.
- Federates with HCA's external identity provider (Azure AD / IDF) via OpenID Connect.
- Supports Two-Factor Authentication (TOTP — Google Authenticator compatible).
- Rate-limits the `/connect/*` endpoints (60 req/min unauthenticated, 240 req/min general) to prevent brute-force attacks.

**Database:** Uses **Entity Framework Core** with SQL Server (`UserDb`) for:
- `AspNetUsers` — application user accounts
- `AspNetRoles` — roles
- OpenIddict tables (applications, tokens, authorizations, scopes)
- `AuditLog` — user action audit trail
- `ErrorLog` — system errors

---

### 4.3 `Hca.Credentialing.Cactus.Api` — CACTUS REST API

**Type:** ASP.NET Core Web API  
**Authentication:** JWT Bearer token (scope: `api-cactus`)  
**Database:** SQL Server — CACTUS database (read/write)

**Role:** The primary data API. CACTUS is HCA's legacy practitioner credentialing system. This API wraps the CACTUS SQL database with a modern REST interface.

Controllers and what they expose:

| Controller | Data |
|---|---|
| `PractitionerController` | Search, retrieve, update practitioner profiles |
| `CredentialingController` | Credentialing items (licenses, board certs, training, etc.) |
| `FacilityController` | HCA facility data |
| `LicenseController` | State licensure records |
| `PractitionerLicenseController` | Practitioner-to-license associations |
| `PractitionerBoardController` | Board certification records |
| `PractitionerSpecialtyController` | Specialty & subspecialty records |
| `PractitionerInsuranceController` | Malpractice insurance records |
| `PractitionerFacilityController` | Practitioner-facility privileges |
| `PractitionerImagesController` | Document/image records in CACTUS |
| `DelegateController` | Delegate assignments |
| `CollaboratingPractitionerController` | CPC (Collaborating Practitioner/CNP) relationships |
| `GroupsController` | Credentialing groups |
| `InstitutionsController` | Training institution (residency/fellowship) records |
| `PeerController` | Peer references |
| `CactusUserController` | CACTUS-side user lookups |

Uses **Dapper** (micro-ORM) and raw SQL for performance against the large CACTUS database.

---

### 4.4 `Hca.Credentialing.Paf.Api` — PAF REST API

**Type:** ASP.NET Core Web API  
**Authentication:** JWT Bearer token (scope: `api-paf`)  
**Database:** SQL Server — PAF database (separate from CACTUS)

**Role:** Manages the Practitioner Application Form lifecycle.

Controllers:

| Controller | Purpose |
|---|---|
| `PafController` | CRUD for PAF forms — create, search, submit, withdraw, merge, unlock |
| `TaskController` | Task/facility list management for PAF workflow |

**Domain Patterns:**
- Uses the **Strategy pattern** (`Domain/Strategy/`) to handle different PAF processing scenarios.
- Database migrations managed via **FluentMigrator**.
- Publishes to `paf-processing` Service Bus queue when a PAF is submitted for backend processing.

---

### 4.5 `Hca.Credentialing.Packet.Api` — Packet REST API

**Type:** ASP.NET Core Web API  
**Authentication:** JWT Bearer token (scope: `api-packet`)  
**Databases:** SQL Server — Packet DB and PAF DB

**Role:** Manages privilege packet lifecycle for practitioners.

Controllers:

| Controller | Purpose |
|---|---|
| `PacketController` | CRUD for packets — create, search, submit, update, reset |
| `DmsController` | Document Management System (DMS/OnBase) document search |
| `DisclosuresController` | Disclosure questions/answers in the packet |

**Domain Patterns:**
- Strategy pattern for different packet building/processing scenarios.
- Database migrations via **FluentMigrator**.
- Publishes to `packet-processing` Service Bus queue.

---

### 4.6 `Hca.Credentialing.Portal.Api` — Portal Integration REST API

**Type:** ASP.NET Core Web API  
**Authentication:** JWT Bearer token (scope: `api-portal`)  
**Databases:** SQL Server — Portal DB, User DB, PAF DB

**Role:** Acts as the integration bridge to the external HCA practitioner portal (legacy system). The portal sends practitioners links to complete their forms online.

Controllers:

| Controller | Purpose |
|---|---|
| `MessagesController` | Portal messages inbox/outbox |
| `ItemsController` | Outstanding credentialing items from portal perspective |
| `DocumentController` | Portal documents |
| `AlertController` | Notification alerts |
| `RecredentialingGroupsController` | Recredentialing committee groups |
| `RecredentialingUtilityController` | Admin utilities for recredentialing |
| `AffiliationVerificationController` | Verify practitioner affiliations |
| `LinkController` | Generate secure links for practitioners |
| `ContactInfoController` | Practitioner contact info management |
| `MipController` | Medical Identity Protection workflows |
| `UserManagementController` | Portal-side user management |

---

### 4.7 `Hca.Credentialing.Background` — Background Azure Functions

**Type:** Azure Functions (in-process, Durable Functions support)  
**Role:** Handles all background work that should not block the user's request.

Functions:

| Function | Trigger | Purpose |
|---|---|---|
| `SendText` | Service Bus: `send-text` | Send SMS via Raven API |
| `SendEmail` | Service Bus: `send-email` | Send email via Microsoft Graph |
| `CleanupPortalMessages` | Timer (daily 1:30 AM) | Purge old portal messages |
| `CleanupPafAttachments` | Timer | Remove orphaned PAF attachments from Blob storage |
| `CleanupPacketAttachments` | Timer | Remove orphaned Packet attachments |
| `PdfGeneration` | Service Bus / HTTP | Generate HTML-to-PDF for PAF/Packet print pages |
| `BatchRrfcGeneration` | Timer / HTTP | Batch generate RRFC (Re-credentialing Request For Committee) reports |
| `MipFunctions` | Service Bus / HTTP + Durable | MIP document processing orchestration |
| `RecredentialingGroupFunctions` | Timer / HTTP + Durable | Manage recredentialing committee cycles |
| `PractitionerDelegateBulkAuditLog` | Service Bus | Emit bulk audit log entries for delegate changes |
| `DeleteOldRecredentialingCommitteeFunction` | Timer | Purge stale recredentialing committee data |
| `CacheFunctions` | HTTP | Invalidate/refresh caches |
| `CopyCviUpdates` | Timer | Propagate CVI updates across facilities |
| `UnlockTasks` | Timer | Auto-unlock expired locked tasks |
| `RecruitmentReportGeneration` | Timer | Generate recruitment activity reports |

Uses **Durable Functions** for long-running orchestrated workflows (MIP, recredentialing batches).

---

### 4.8 `Hca.Credentialing.Paf.Processor` — PAF Async Processor

**Type:** Azure Function  
**Trigger:** Service Bus queue: `paf-processing`

**Role:** Processes PAF form submissions asynchronously. When a practitioner submits a PAF:

1. PAF API pushes a `PafProcessingRequest` to the `paf-processing` queue.
2. This function picks it up and runs `CactusUpdater.UpdateCactusFromPafAsync()`.
3. CACTUS database is updated with the submitted practitioner data.
4. If successful and a packet should be generated, it pushes a `PacketProcessingRequest` to the `packet-processing` queue.
5. Also notifies the legacy portal client.

---

### 4.9 `Hca.Credentialing.Packet.Processor` — Packet Async Processor

**Type:** Azure Function  
**Trigger:** Service Bus queue: `packet-processing`

**Role:** Handles all packet lifecycle operations asynchronously.

Handlers:

| Handler | What it does |
|---|---|
| `PacketBuilder` | Builds a new privilege packet from a submitted PAF |
| `PacketDelegateUpdater` | Propagates delegate changes to all open packets |
| `PacketCviUpdater` | Syncs CVI (credentialing verification) status with CACTUS |
| `PacketResetUpdater` | Resets a packet back to its initial state |
| `MsoDueDateUpdater` | Updates MSO (Medical Staff Office) due dates |
| Notification Functions | Sends emails for account unlock, DOB change, address change, delegate change |

---

### 4.10 `Hca.Credentialing.Document.Generation` — PDF Builder

**Type:** Azure Function  
**Trigger:** Multiple Service Bus queues  

**Role:** Generates PDF documents using **Aspose.PDF**. Triggered by:

| Queue | Document Generated |
|---|---|
| `portal-pdf-generation-document` | Portal-facing practitioner PDF (delegation letter, etc.) |
| `packet-pdf-generation-document` | Privilege packet PDF |

After generating a PDF, stores it in OnBase (HCO DMS) via the `IDmsService`.

---

### 4.11 `Hca.Credentialing.Image.Conversion` — Image/Document Converter

**Type:** Azure Function  
**Trigger:** Multiple Service Bus queues  

**Role:** Converts documents (Word, images) uploaded as PAF/Packet attachments into PDF or TIFF format suitable for storage in OnBase. Uses **Aspose.Words** for Word-to-PDF conversion. Connects to **OnBase** via the `OnBaseService` (Unity API).

Functions:

| Function | Queue | Purpose |
|---|---|---|
| `ConvertPafAttachment` | `paf-attachment-image-conversion` | Convert PAF attachment and upload to OnBase |
| `ConvertPacketAttachment` | `packet-attachment-image-conversion` | Convert Packet attachment and upload to OnBase |

---

### 4.12 `Hca.Credentialing.Api.Shared` — API Shared Library

**Role:** Common code shared by all API and Azure Function projects.

Contents:

| Folder | Contents |
|---|---|
| `Authentication/` | JWT claims helpers, HTTP message handlers for inter-service auth |
| `Config/ConfigConstants.cs` | All configuration key constants and Service Bus queue name constants |
| `Config/ServiceBusSettings.cs` | Service Bus connection settings |
| `Data/` | Dapper configuration, generic repository base classes |
| `Helpers/` | JSON converters, retry policy builder |
| `Services/` | `ServiceBusService` (publish messages), `HcoDmsService` (OnBase DMS client), `IDmsService` |

---

### 4.13 `Hca.Credentialing.Shared` — Frontend/Backend Shared Library

**Role:** Shared between the Blazor Client, Server, and all backend projects.

Contents:

| Folder | Contents |
|---|---|
| `Clients/Cactus/` | Typed HTTP client for Cactus API — `CactusClient`, `PractitionerClient`, `FacilityClient`, etc. |
| `Clients/Paf/` | Typed HTTP client for PAF API — `PafClient` |
| `Clients/Packet/` | Typed HTTP client for Packet API — `PacketClient`, `DisclosuresClient` |
| `Clients/Portal/` | Typed HTTP client for Portal API |
| `Clients/Idp/` | Typed HTTP client for IDP (used by background functions to get tokens) |
| `Data/Model/` | All domain model classes (Practitioner, Paf, Packet, Facility, Group, etc.) |
| `Helpers/` | Shared utility helpers |
| `Validations/` | FluentValidation validators for shared models |

---

## 5. Data Flow — End-to-End Scenarios

### Scenario A: Practitioner Submits a PAF

```
Practitioner (Browser)
  │
  ▼ 1. Fills PAF form on PractitionerPafPage.razor
  │    (Blazor WASM — Hca.Credentialing.Client)
  │
  ▼ 2. HTTP POST to PAF API /api/paf/submit
  │    (Hca.Credentialing.Paf.Api — JWT scope: api-paf)
  │
  ▼ 3. PAF API saves PAF record to PAF DB
  │    Publishes PafProcessingRequest → Service Bus queue: "paf-processing"
  │
  ▼ 4. PAF Processor Function picks up message
  │    (Hca.Credentialing.Paf.Processor)
  │    - Calls CactusUpdater → updates CACTUS DB with submitted data
  │    - If packet needed: publishes PacketProcessingRequest → "packet-processing"
  │    - Notifies Portal API
  │
  ▼ 5. Packet Processor Function picks up message
  │    (Hca.Credentialing.Packet.Processor)
  │    - PacketBuilder creates Privilege Packet in Packet DB
  │    - Publishes PDF generation request → "packet-pdf-generation-document"
  │
  ▼ 6. Document Generation Function picks up message
       (Hca.Credentialing.Document.Generation)
       - Aspose builds PDF
       - Uploads to OnBase via DMS service
```

### Scenario B: PAF/Packet Attachment Uploaded

```
User uploads file (image/Word doc)
  │
  ▼ PAF API / Packet API saves attachment record + raw file to Blob Storage
  │  Publishes message → "paf-attachment-image-conversion" or "packet-attachment-image-conversion"
  │
  ▼ Image Conversion Function picks up message
  │  (Hca.Credentialing.Image.Conversion)
  │  - Downloads from Blob Storage
  │  - Converts Word/image → PDF/TIFF via Aspose
  │  - Uploads converted file to OnBase (via OnBase Unity API)
  │  - Updates attachment status in DB
  │
  ▼ Practitioner can now see final document in the application
```

### Scenario C: Email or SMS Notification

```
Any API or Processor
  │
  ▼ Publishes SendEmailRequest → "send-email" queue
  │  or
  ▼ Publishes SendTextRequest → "send-text" queue
  │
  ▼ Background.Notifications function picks up message
    - SendEmail: calls Microsoft Graph API (Graph Mail Service)
    - SendText: calls Raven API (via Durable Functions for token management)
```

### Scenario D: Recredentialing Committee Batch

```
Timer trigger (scheduled)
  │
  ▼ RecredentialingGroupFunctions.BatchRrfcGenerationOrchestrator
  │  (Durable Orchestrator in Hca.Credentialing.Background)
  │
  ▼ Fans out to activity functions for each practitioner group
    - Fetches data from CACTUS DB and Portal DB
    - Generates RRFC report
    - Stores result in Azure Blob Storage
    - Sends summary email to configured address
```

---

## 6. Authentication & Authorization

### Identity Flow

```
Browser                   Server (IDP - OpenIddict)         Client App
  │                              │                               │
  ├─── 1. Login page ────────────►│                               │
  │                              │                               │
  ├─── 2. Username/Password ─────►│ (or federate to HCA AD IDP)  │
  │                              │                               │
  │◄── 3. Authorization code ─────┤                               │
  │                              │                               │
  ├─── 4. Token exchange ─────────►│                               │
  │                              │                               │
  │◄── 5. Access token (JWT) ─────┤                               │
  │                              │                               │
  └─── 6. API calls with Bearer token ─────────────────────────► │
```

### Key Authentication Components

| Component | Location | Role |
|---|---|---|
| **OpenIddict** | `Hca.Credentialing.Server` | OAuth2/OIDC authorization server |
| **External IDP** | HCA Azure AD (IDF) | Federated login for HCA employees |
| **JWT Bearer** | All APIs | Validates tokens issued by the server |
| **AccessTokenHandler** | `Api.Shared/Authentication/` | Auto-attaches tokens for inter-service HTTP calls |
| **CustomUserFactory** | `Client/Security/` | Maps JWT claims to Blazor `AuthenticationState` |
| **ClaimsPrincipalHelper** | `Api.Shared/Authentication/` | Extract user identity from claims in APIs |

### Roles

Roles are stored as `AspNetRoles` and attached as JWT claims. Common roles visible in dashboards:
- **Practitioner** — fills out own PAF/Packet
- **Delegate** — fills forms on behalf of practitioner
- **MSP** (Medical Staff Professional) — processes forms, manages queues
- **CSS** (Credentialing Support Specialist) — credentialing reviews
- **CPC** (Credentialing/Privileges Coordinator)
- **Admin** — user management, system configuration
- **Recruitment** — recruitment reporting

### API Scopes

| Scope | API |
|---|---|
| `api-cactus` | Cactus API |
| `api-paf` | PAF API |
| `api-packet` | Packet API |
| `api-portal` | Portal API |

Each API validates that the incoming token contains the correct scope.

### Rate Limiting (Server)

- `/connect/*` endpoints (OIDC): **60 requests/minute per IP** (unauthenticated)
- All other endpoints: **240 requests/minute per IP**
- Returns `HTTP 429` with an HTML message on rejection.

---

## 7. Databases

| Database | Config Key | Owner Project | ORM | Purpose |
|---|---|---|---|---|
| **UserDb** | `ConnectionStrings:DefaultConnection` | Server | EF Core + Dapper | User accounts, roles, OpenIddict, audit log |
| **CactusDb** | `ConnectionStrings:CactusDb` | Cactus API | Dapper | Practitioner master data (legacy CACTUS system) |
| **PafDb** | `ConnectionStrings:PafDb` | PAF API | Dapper + FluentMigrator | PAF forms, attachments, history |
| **PacketDb** | `ConnectionStrings:PacketDb` | Packet API | Dapper + FluentMigrator | Privilege packets, disclosures, attachments |
| **PortalDB** | `ConnectionStrings:PortalDB` | Portal API | Dapper + FluentMigrator | Portal messages, items, affiliations |

All SQL Server databases. Database schema evolution:
- `UserDb`: Entity Framework Core migrations (files in `Server/Migrations/`)
- `PafDb`, `PacketDb`, `PortalDB`: FluentMigrator (embeded SQL migration scripts in each API project)

---

## 8. Messaging — Azure Service Bus Queues

All async communication uses **Azure Service Bus** (connection string key: `HcoServiceBus`).

| Queue Name | Producer | Consumer | Purpose |
|---|---|---|---|
| `paf-processing` | PAF API | PAF Processor | PAF submission processing |
| `packet-processing` | PAF Processor, Packet API | Packet Processor | Packet build/update processing |
| `send-email` | Any | Background (Notifications) | Send email |
| `send-text` | Any | Background (Notifications) | Send SMS |
| `paf-attachment-image-conversion` | PAF API | Image Conversion | Convert PAF attachment |
| `packet-attachment-image-conversion` | Packet API | Image Conversion | Convert Packet attachment |
| `packet-pdf-generation-document` | Packet Processor | Document Generation | Generate Packet PDF |
| `portal-pdf-generation-document` | Portal API | Document Generation | Generate Portal PDF |
| `packet-pdf-generation-background` | Background | Background | Internal PDF job tracking |
| `portal-pdf-generation-conversion` | Document Generation | Image Conversion | Convert portal PDF for OnBase |
| `paf-pdf-generation-background` | Background | Background | PAF PDF job tracking |
| `completed-paf-image-conversion` | Image Conversion | Background | Notify completion of PAF image conversion |
| `portal-upload-item-dmi` | Portal API | Background | Upload portal item to DMS |
| `packet-upload-attachment-dmi` | Packet API | Background | Upload Packet attachment to DMS |

---

## 9. Azure Functions Summary

| Project | Trigger Types | Key Libraries |
|---|---|---|
| `Background` | Service Bus, Timer, HTTP, Durable | Aspose.PDF, SkiaSharp, Microsoft Graph, Polly, Durable Task |
| `Document.Generation` | Service Bus | Aspose.PDF |
| `Image.Conversion` | Service Bus, HTTP | Aspose.Words, OnBase Unity API |
| `Paf.Processor` | Service Bus | Dapper |
| `Packet.Processor` | Service Bus, HTTP | Dapper |

All Azure Functions use the **isolated worker model** (`ConfigureFunctionsWebApplication()` or `ConfigureFunctionsWorkerDefaults()`).

---

## 10. Shared Libraries

### `Hca.Credentialing.Api.Shared`
Used by: all API projects and Azure Functions  
Key classes:
- `ConfigConstants` — centralized key names for `IConfiguration` and queue names
- `ServiceBusService` / `IServiceBusService` — publish messages to any queue
- `HcoDmsService` — HTTP client for OnBase DMS
- `DefaultErrorProcessor` — standardized error handling/logging
- Dapper type handlers and extension methods

### `Hca.Credentialing.Shared`
Used by: Client, Server, all API projects, all Azure Functions  
Key namespaces:
- `Shared.Clients.*` — typed HttpClient wrappers for each internal API
- `Shared.Data.Model.*` — all domain entity classes (Paf, Packet, Practitioner, Facility, etc.)
- `Shared.Helpers.*` — date/time, string, JSON utilities
- `Shared.Extensions.*` — Central Time extension, JSON extension, etc.

---

## 11. Key Technologies & Libraries

| Technology | Version/Notes | Used For |
|---|---|---|
| **ASP.NET Core** | .NET (see global.json) | Server host, all REST APIs |
| **Blazor WebAssembly** | Hosted model | Frontend SPA |
| **OpenIddict** | OAuth 2.0 / OIDC server | Identity Provider |
| **Entity Framework Core** | SQL Server provider | UserDb schema & migrations |
| **Dapper** | Micro-ORM | All other database access (CACTUS, PAF, Packet, Portal) |
| **FluentMigrator** | DB schema migrations | PAF, Packet, Portal databases |
| **Azure Service Bus** | Messaging | Async inter-service communication |
| **Azure Functions** | Isolated worker model | All background processing |
| **Durable Functions** | Orchestration | MIP, Recredentialing batches |
| **Azure Blob Storage** | Azure Storage | Temporary attachment storage, email body blobs |
| **Aspose.PDF** | Commercial | PDF rendering (Document Generation) |
| **Aspose.Words** | Commercial | Word-to-PDF conversion (Image Conversion) |
| **SkiaSharp** | Graphics | Image manipulation in Background functions |
| **Microsoft Graph** | v1.0 | Email sending (M365), user lookup |
| **Polly** | Resilience | Retry policies for token fetch and HTTP calls |
| **Ant Design Blazor** | UI component library | Frontend UI components |
| **WebOptimizer** | Asset pipeline | JS/CSS minification |
| **Application Insights** | Azure Monitor | Telemetry and logging |

---

## 12. Build & Run

### Prerequisites
- .NET SDK (version pinned in `Hca.Credentialing/global.json`)
- Azure Functions Core Tools
- Azurite (local Azure Storage emulator) or Azure Storage connection string
- SQL Server (local or Azure)
- Access to Azure Service Bus namespace

### Build Tasks (VS Code)

| Task | Command |
|---|---|
| `build` | Builds Server + Cactus API + PAF API |
| `build site` | Builds only the main web host |
| `build paf api` | Builds PAF API only |
| `build cactus api` | Builds Cactus API only |
| `build background functions` | Cleans + builds Background Azure Functions |
| `build paf processor` | Cleans + builds PAF Processor function |
| `Run Background Functions` | Starts the Background Functions locally |

### Configuration Files

Each project has:
- `appsettings.json` — default/template values (connection strings left blank)
- `appsettings.Development.json` — local developer overrides (git-ignored)
- `appsettings.Development.json-example` — template showing what Development settings look like

### Important Config Values (Server)

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "<UserDb SQL connection string>",
    "StorageConnection": "UseDevelopmentStorage=true"
  },
  "HcoServiceBus": "<Azure Service Bus connection string>",
  "Credentialing": {
    "Env": "local",
    "CactusApiUrl": "<url to Cactus API>",
    "Idp": {
      "IssuerUri": "<this server's URL>",
      "ExternalIdpAuthority": "<HCA Azure AD URL>",
      "ExternalIdpClientId": "<Azure AD app ID>",
      "ExternalIdpClientSecret": "<secret>"
    }
  }
}
```

---

## 13. Glossary

| Term | Meaning |
|---|---|
| **CACTUS** | HCA's proprietary practitioner credentialing database/system (legacy) |
| **PAF** | Practitioner Application Form — the digital form a practitioner fills out to apply or reapply for privileges |
| **Packet** | Privilege Packet — a set of privilege request forms for a given credentialing cycle at one or more facilities |
| **CVI** | Credentialing Verification Item — an item that must be verified as part of credentialing (license, board cert, etc.) |
| **DMS / OnBase** | Document Management System — HCA's OnBase repository for scanned/generated documents |
| **RRFC** | Re-credentialing Request for Committee — the report submitted to the credentialing committee |
| **MIP** | Medical Identity Protection — a sensitive workflow protecting high-risk practitioner identity data |
| **MSP** | Medical Staff Professional — staff who process credentialing requests |
| **CSS** | Credentialing Support Specialist |
| **CPC** | Credentialing/Privileges Coordinator |
| **IDP / IdP** | Identity Provider — the OpenIddict server embedded in `Hca.Credentialing.Server` |
| **HCA AD / IDF** | HCA Healthcare's enterprise Azure Active Directory, used as the federated external IdP |
| **Raven** | HCA's SMS text messaging platform |
| **Graph** | Microsoft Graph API — used to send emails via Microsoft 365 |
| **Durable Functions** | Azure Durable Functions — provides stateful orchestration for long-running workflows |
| **Service Bus** | Azure Service Bus — the messaging backbone used for async communication between services |
| **Delegate** | A person (assistant/agent) authorized to fill out credentialing forms on behalf of a practitioner |
| **Portal** | A legacy HCA web portal where practitioners log in to access their application links |
| **Aspose** | A commercial .NET library used to generate and manipulate PDF and Word documents |
| **FluentMigrator** | A .NET database migration framework used to evolve SQL schemas without EF Core |
| **OpenIddict** | An open-source OAuth 2.0 / OpenID Connect server library for ASP.NET Core |

# Office Repository Investigation Map

## Known Office Repository Structure

The user has provided the following solution structure from the office repository:

```
Credentialing-2.0/
└── Hca.Credentialing/
    ├── .vs/
    ├── Hca.Credentialing/
    ├── Hca.Credentialing.Api.Shared/
    ├── Hca.Credentialing.Background/
    ├── Hca.Credentialing.Background.Tests/
    ├── Hca.Credentialing.Cactus.Api/
    ├── Hca.Credentialing.Cactus.Api.Tests/
    ├── Hca.Credentialing.Document.Generation/
    ├── Hca.Credentialing.Document.Generation.Tests/
    ├── Hca.Credentialing.Image.Conversion/
    ├── Hca.Credentialing.Image.Conversion.Tests/
    ├── Hca.Credentialing.Packet.Api/
    ├── Hca.Credentialing.Packet.Api.Tests/
    ├── Hca.Credentialing.Packet.Processor/
    ├── Hca.Credentialing.Packet.Processor.Tests/
    ├── Hca.Credentialing.Paf.Api/
    ├── Hca.Credentialing.Paf.Processor/
    ├── Hca.Credentialing.Paf.Processor.Tests/
    ├── Hca.Credentialing.Portal.Api/
    ├── Hca.Credentialing.Tasks/
    └── packages/
```

This structure is repository evidence only.

Folder names alone do not prove what a module does internally.

Before assigning implementation work to a module, inspect its actual files and symbols.

## Initial Architecture Mapping

Use the known solution structure as the starting investigation map.

Hca.Credentialing

Potentially the core application/domain area.

Do NOT assume exact responsibilities.

Ask for relevant files when needed.

Hca.Credentialing.Api.Shared

Investigate for:

shared API models

common DTOs

API contracts

shared request/response types

common API behavior

Do not assume these exist until inspected.

Hca.Credentialing.Background

Investigate for:

background processing

scheduled work

asynchronous processing

queues

workers

Hca.Credentialing.Cactus.Api

Investigate for:

CACTUS integration

provider updates

license updates

specialty updates

address updates

entity assignment

sanctions

images

Confirm through actual code.

Hca.Credentialing.Document.Generation

Investigate for:

PAF PDF generation

document templates

document rendering

PAF document type mapping

attachment generation

Hca.Credentialing.Image.Conversion

Investigate for:

PSV file conversion

document/image conversion

PDF/HTML conversion

supported image/document formats

Hca.Credentialing.Packet.Api

Investigate for:

packet APIs

packet retrieval

packet submission

packet-related endpoints

Do not assume ADD NPP uses this module.

Hca.Credentialing.Packet.Processor

Investigate for:

packet processing

packet workflow

packet state changes

Do not assume ADD NPP belongs here.

Hca.Credentialing.Paf.Api

This is a high-priority investigation area for ADD NPP.

Inspect for:

PAF APIs

PAF actions

PAF types

PAF requests

submit endpoints

validation endpoints

task/card endpoints

routing endpoints

Hca.Credentialing.Paf.Processor

This is another high-priority investigation area.

Inspect for:

PAF business processing

validation

routing

auto acceptance

CPC/CVI

PAF state transitions

CACTUS orchestration

history

audit

Do not assume exact responsibilities until files are inspected.

Hca.Credentialing.Portal.Api

Investigate for:

MSP-facing APIs

practitioner search

portal operations

task/card retrieval

practitioner-related endpoints

Hca.Credentialing.Tasks

High-priority investigation area for:

task definitions

task registration

task completion

task requiredness

task visibility

task navigation

Review & Submit gating

Tests

Always inspect corresponding test projects before creating new tests.

Prefer:

Implementation project
        ↓
Existing test project
        ↓
Existing test pattern
        ↓
New/updated test

Do not invent a testing framework or pattern.

## High-Priority ADD NPP Investigation Order

When beginning repository analysis, use this order:

1. Hca.Credentialing.Paf.Api
2. Hca.Credentialing.Paf.Processor
3. Hca.Credentialing.Tasks
4. Hca.Credentialing.Portal.Api
5. Hca.Credentialing.Cactus.Api
6. Hca.Credentialing.Api.Shared
7. Hca.Credentialing.Document.Generation
8. Hca.Credentialing.Image.Conversion
9. Existing test projects
10. Packet modules only if repository evidence shows ADD NPP interaction
11. Background only if repository evidence shows asynchronous ADD NPP processing

This is an investigation priority, NOT a claim about the architecture.

## First Repository Request

If the user has not yet supplied repository code, start with a focused request.

Ask the user to bring the following from the office laptop:

A. PAF implementation

PAF API project structure

PAF Processor project structure

PAF type/action registration

Add Practitioner to Facility implementation

B. Tasks

task definitions

task registration

task completion

task requiredness/visibility

C. Practitioner search

NPI search

no-NPI search

practitioner classification

D. Related tests

Add Practitioner to Facility tests

PAF processor tests

task tests

Do not ask for the entire repository if smaller targeted files are sufficient.

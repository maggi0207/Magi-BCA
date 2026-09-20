# ADD NPP Requirement Baseline

The implementation must account for:

## MSP

MSP-only action

one PAF action at a time

eligible practitioner states according to requirements

## Search

NPI:

valid 10-digit NPI

validation before search

duplicate checking

No NPI:

No NPI available? Search without NPI

Fields:

Reason

First Name

Last Name

State License Number

State of License

Validation:

reason selected

first name ≥ 1 alphabetic character

last name ≥ 2 alphabetic characters

state selected

license number ≥ 2 characters

## Task

ADD NPP to Facility

Required

Work action

visible in PAF Tasks

opens ADD NPP workflow

blocks Review & Submit while incomplete

Description:

Upload and complete required Non-Privileged Practitioner (NPP) verification information and supporting PSV documentation.

## Demographics

Required:

First Name

Last Name

Degree

Provider Category

Individual NPI

Optional:

Email

Cell

DOB/SSN

Gender

## Address

Hide:

Home

Credentialing

Alternate

Required:

Address

City

State

Zip

Country

Phone

Fax

Optional:

Contact

Address Line 2

Phone Extension

Fax Extension

Primary-address editability depends on active primary address and active facility affiliation.

## Specialty

Net-new:

required

Existing:

optional unless no active primary specialty exists

Hide:

secondary

alternate

PPI can satisfy the requirement where applicable.

## VA / PA

Question:

Is the practitioner an active duty military member or practicing at the VA?

VA = Yes:

Is the Practitioner a PA?

VA + PA:

hide State License

require Sanctions PSV

require NPI PSV

CPC routing

VA + non-PA:

State License

License PSV

Sanctions PSV

NPI PSV

VA = No:

skip PA question

State License

License PSV

Sanctions PSV

NPI PSV

## License

Trace existing-license recall and new-license creation.

Fields:

State

Effective Date

License Number

Status

Expiration Date

Field of Licensure

Statuses:

Active

Temporary Permit

Active Military

Active-Compact

Special state validation includes:

CA

LA

NV

KY

TX

where the license must match the applicable entity/facility state.

## PSV

Accepted:

DOC

DOCX

PDF

JPG

TIFF

Trace:

validation

conversion

storage

CACTUS image

metadata

audit

## Routing

Trace criteria such as:

PA = Yes → CPC

non-Active status → CPC

expired/matured → CPC

normal new license → normal validation

recalled existing license → existing-license behavior

Do not generalize beyond the requirement.

## Auto acceptance

Normal ADD NPP:

auto accepted

CACTUS updates

Completed queue

PAF history

audit

PAF PDF

## CPC / CVI

Where explicitly required:

Facility Undefined Practitioner

UDP Facility Request

trigger notes

due date +3 business days

completion status UDP Facility Request Complete

PAF PDF attached to CVI

## CACTUS

Trace:

Provider

Entity Assignment

Address

Specialty

License

NPI image

Sanctions

## PDF / history / audit

Trace:

PAF type

template

generator

attachment

history

MSP identity

HCP System User

audit

## Reporting

Trace:

total received

total routed CPC

percentage CPC

distribution

MOR exclusion for Facility Undefined Practitioner CVI where required

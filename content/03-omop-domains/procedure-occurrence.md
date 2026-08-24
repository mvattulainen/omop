---
title: "PROCEDURE_OCCURRENCE Transformation"
type: omop-domain
status: stable
omop_domains: [PROCEDURE_OCCURRENCE]
source_entities: [procedure]
engineering_stages: [vocabulary-mapping, omop-transformation]
tags: [omop, ehr, procedure]
---

# PROCEDURE_OCCURRENCE Transformation

## Problem

Represent performed procedures in the domain chosen by the standard concept, without treating plans/cancellations as events.

## Source Semantics

SRC-R7001 says a local 12-lead ECG procedure was performed during E1003.

## Minimal Raw EHR Example

| source_record_id | patient_id | encounter_id | code | system | performed_at | status |
|---|---|---|---|---|---|---|
| SRC-R7001 | P002 | E1003 | ECG12 | LOCAL-PROC | 2026-02-03 22:30+02 | performed |

## Target OMOP Representation

### `PROCEDURE_OCCURRENCE`

| procedure_occurrence_id | person_id | procedure_concept_id | procedure_datetime | procedure_type_concept_id | visit_occurrence_id | procedure_source_value |
|---:|---:|---|---|---|---:|---|
| 91001 | 2 | `<standard 12-lead ECG procedure>` | 2026-02-03 22:30+02 | `<EHR procedure record type>` | 50003 | ECG12 |

## Transformation Rules

1. Include only performed rows.
2. Map code at event date and route by standard concept domain.
3. Create this row only when the domain is Procedure; otherwise route to the correct domain or quarantine.

## Field Mapping

| Source | OMOP | Rule |
|---|---|---|
| patient/encounter | person_id/visit_occurrence_id | crosswalk lookups |
| code/system | concept/source fields | semantic mapping and direct source preservation |
| performed_at | procedure datetime/date | direct normalized copy/derived date |
| source_record_id | lineage | direct technical link |

## Mapping Decisions

The source table name does not override standard concept domain. Cancelled/planned rows remain in exclusion lineage.

## Validation

- Included rows have performed status/time and valid Procedure-domain concept or explicit zero fallback.
- Eligible performed inputs equal routed targets plus quarantine.

## Failure Modes

- Plan becomes occurrence; domain is forced; performed time is replaced by order time; repeated messages duplicate an event.

## Related Pages

[Procedures](../01-source-ehr/procedures.md) · [domain assignment](../03-Challenges/domain-assignment.md) · [timestamp selection](../03-Challenges/timestamp-selection.md) · [[06-omop-transformation]]

---
title: "CONDITION_OCCURRENCE Transformation"
type: omop-domain
status: stable
omop_domains: [CONDITION_OCCURRENCE]
source_entities: [diagnosis, problem_list]
engineering_stages: [vocabulary-mapping, omop-transformation]
tags: [omop, ehr, condition]
---

# CONDITION_OCCURRENCE Transformation

## Problem

Preserve the distinction between encounter diagnosis, problem-list assertion, and billing duplication while mapping clinical meaning.

## Source Semantics

SRC-D5001 is a confirmed encounter diagnosis recorded during E1001.

## Minimal Raw EHR Example

| source_record_id | patient_id | encounter_id | dx_code | code_system | dx_type | clinical_at |
|---|---|---|---|---|---|---|
| SRC-D5001 | P001 | E1001 | E11.9 | ICD10CM | encounter | 2026-01-14 09:42+02 |

## Target OMOP Representation

### `CONDITION_OCCURRENCE`

| condition_occurrence_id | person_id | condition_concept_id | condition_start_date | condition_start_datetime | condition_type_concept_id | visit_occurrence_id | condition_source_value | condition_source_concept_id |
|---:|---:|---|---|---|---|---:|---|---|
| 90001 | 1 | `<standard SNOMED type 2 diabetes>` | 2026-01-14 | 2026-01-14 09:42+02 | `<EHR encounter diagnosis type>` | 50001 | E11.9 | `<source ICD-10-CM E11.9>` |

## Transformation Rules

1. Include confirmed encounter diagnoses and qualifying active problem assertions; exclude cancelled entries.
2. Resolve source and standard concepts at the clinical date.
3. Assign type from source semantics and link the encounter crosswalk when present.
4. Deduplicate only demonstrable representations of the same assertion; retain a lineage bridge to all contributing records.

## Field Mapping

| Source field | OMOP field | Rule |
|---|---|---|
| patient_id | person_id | lookup |
| code/system | source/standard concept fields | vocabulary resolution/mapping |
| clinical_at | start datetime/date | direct normalized copy/derived date |
| dx_type | condition_type_concept_id | semantic mapping |
| encounter_id | visit_occurrence_id | crosswalk lookup |
| dx_code | condition_source_value | direct copy |

## Mapping Decisions

Recorded time is used only because the source declares it clinical for encounter diagnoses. No condition end is inferred for diabetes. Provenance retains source table/type, record, value, source concept, mapping snapshot, and rule.

## Validation

- Standard concept is valid and Condition-domain or zero by explicit fallback.
- Visit belongs to the person; included/excluded/deduplicated counts reconcile to eligible source rows.

## Failure Modes

- Billing copy becomes another condition; problem-list date is treated as onset; invalid mapping targets another domain; source code or type is discarded.

## Related Pages

[Diagnoses](../01-source-ehr/diagnoses.md) · [vocabulary mapping](../02-engineering/05-vocabulary-mapping.md) · [duplicate events](../03-Challenges/duplicate-events.md) · [[example-outpatient-diabetes]]

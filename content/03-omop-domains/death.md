---
title: "DEATH Transformation"
type: omop-domain
status: draft
omop_domains: [DEATH]
source_entities: [patient_death, diagnosis]
engineering_stages: [normalization, omop-transformation]
tags: [omop, ehr, death]
---

# DEATH Transformation

## Problem

Select one best-supported death fact per person while preserving conflicting evidence.

## Source Semantics

The extension fixture contains an explicitly verified death record; discharge disposition alone is supporting evidence, not sufficient by itself under this policy.

## Minimal Raw EHR Example

| source_record_id | patient_id | death_at | verification | cause_code | code_system |
|---|---|---|---|---|---|
| SRC-X9001 | P003 | 2026-03-08 04:12+02 | registry-confirmed | I21.9 | ICD10CM |

## Target OMOP Representation

### `DEATH`

| person_id | death_date | death_datetime | death_type_concept_id | cause_concept_id | cause_source_value | cause_source_concept_id |
|---:|---|---|---|---|---|---|
| 3 | 2026-03-08 | 2026-03-08 04:12+02 | `<registry-confirmed death type>` | `<standard mapped cause>` | I21.9 | `<source ICD-10-CM I21.9>` |

## Transformation Rules

1. Rank evidence by configured reliability and select one fact per person.
2. Preserve conflicts in lineage; do not average dates.
3. Map cause separately and allow null/zero when cause is unknown under CDM policy.

## Field Mapping

| Source | OMOP | Rule |
|---|---|---|
| patient_id | person_id | lookup |
| death_at | date/datetime | selected verified value/derived date |
| verification | death_type_concept_id | semantic mapping |
| cause code/system | cause fields | vocabulary mapping/source copy |

## Mapping Decisions

Only explicit verified sources qualify in the base policy. Evidence ranking and every rejected candidate are versioned.

## Validation

- At most one DEATH per person; death is not before birth; cause mapping is valid for snapshot.

## Failure Modes

- Disposition is treated as certain death; conflicting dates create multiple rows; unsupported cause is fabricated.

## Related Pages

[Patients](../01-source-ehr/patients.md) · [identity resolution](../03-Challenges/patient-identity-resolution.md) · [timestamp selection](../03-Challenges/timestamp-selection.md) · [[08-clinical-validation]]

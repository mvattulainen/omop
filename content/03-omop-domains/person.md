---
title: "PERSON Transformation"
type: omop-domain
status: stable
omop_domains: [PERSON]
source_entities: [patient, patient_alias, patient_merge]
engineering_stages: [normalization, omop-transformation]
tags: [omop, ehr, person]
---

# PERSON Transformation

## Problem

Create one persistent analytical person from potentially mergeable EHR registrations without inventing demographics.

## Source Semantics

`patient_id` is a registration identity. Explicit merge records, not demographic similarity, establish equivalence.

## Minimal Raw EHR Example

| source_record_id | patient_id | birth_date | administrative_sex |
|---|---|---|---|
| SRC-P001 | P001 | 1980-04-12 | F |

## Target OMOP Representation

### `PERSON`

| person_id | gender_concept_id | year_of_birth | month_of_birth | day_of_birth | person_source_value | gender_source_value |
|---:|---|---:|---:|---:|---|---|
| 1 | `<standard concept for female>` | 1980 | 4 | 12 | P001 | F |

## Transformation Rules

1. Resolve the winning registration through explicit merge history.
2. Look up or create one persistent `person_id` for the winner.
3. Populate only known birth parts; map administrative sex using the approved release rule.
4. Preserve winning and losing identifiers in the identity crosswalk.

## Field Mapping

| Source field | OMOP field | Rule |
|---|---|---|
| patient_id | person_id | persistent crosswalk lookup |
| birth_date | birth parts | calculated extraction; no imputation |
| administrative_sex | gender_concept_id | semantic mapping |
| patient_id | person_source_value | direct copy |

## Mapping Decisions

Demographic resemblance never triggers a merge. Unknown sex maps to the approved unknown/zero representation and retains the source value. Technical lineage is `person_id → winning source record + merge chain`.

## Validation

- One PERSON per winning identity; every clinical fact resolves to one person.
- Birth precedes all clinical dates; crosswalk identifiers remain stable between releases.

## Failure Modes

- False merge combines two people; missed merge duplicates one person; partial birth dates are silently imputed; merge lineage is lost.

## Related Pages

[Patients](../01-source-ehr/patients.md) · [identity resolution](../03-Challenges/patient-identity-resolution.md) · [incremental loads](../02-engineering/09-incremental-loads.md) · [[example-patient-001]]

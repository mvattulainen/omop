---
title: "MEASUREMENT Transformation"
type: omop-domain
status: stable
omop_domains: [MEASUREMENT]
source_entities: [laboratory_result, observation]
engineering_stages: [normalization, vocabulary-mapping, omop-transformation]
tags: [omop, ehr, measurement]
---

# MEASUREMENT Transformation

## Problem

Represent numeric and categorical test results with correct clinical time, unit, value type, and provenance.

## Source Semantics

SRC-L6001 is a final HbA1c result collected during E1001; collection time is the clinical event time and result time is audit provenance.

## Minimal Raw EHR Example

| source_record_id | lab_code | value_num | unit | collected_at | resulted_at | status |
|---|---|---:|---|---|---|---|
| SRC-L6001 | HBA1C-LOCAL | 7.4 | % | 2026-01-14 09:50+02 | 2026-01-14 12:30+02 | final |

## Target OMOP Representation

### `MEASUREMENT`

| measurement_id | person_id | measurement_concept_id | measurement_datetime | measurement_type_concept_id | value_as_number | unit_concept_id | visit_occurrence_id | measurement_source_value | unit_source_value |
|---:|---:|---|---|---|---:|---|---:|---|---|
| 93001 | 1 | `<standard HbA1c measurement>` | 2026-01-14 09:50+02 | `<EHR laboratory result type>` | 7.4 | `<standard percent unit>` | 50001 | HBA1C-LOCAL | % |

## Transformation Rules

1. Select the latest valid final/corrected version per result key.
2. Route by standard concept domain and use collection time, falling back by the documented timestamp hierarchy.
3. Put numeric results in `value_as_number`; categorical results use `value_as_concept_id`; never populate both for one result without explicit rule.
4. Preserve original value/unit; store normalized value/unit only after a compatible versioned conversion.

## Field Mapping

| Source | OMOP | Rule |
|---|---|---|
| patient/encounter | person_id/visit_occurrence_id | crosswalk lookup |
| lab code/system | measurement concept/source fields | mapping/direct copy |
| collected_at | measurement datetime/date | selected normalized timestamp/derived date |
| value_num | value_as_number | direct or versioned calculated value |
| unit | unit concept/source value | semantic unit mapping/direct copy |

## Mapping Decisions

HbA1c percent is not converted unless the release defines a verified conversion target. Reference ranges remain source-compatible. Lineage retains all source versions, collection/result times, original value/unit, conversion formula, and vocabulary versions.

## Validation

- Concept domain is Measurement; unit is compatible with test.
- Plausible range alerts do not silently delete values; result versions and eligible counts reconcile.

## Failure Modes

- Result time replaces collection time; wrong unit conversion changes magnitude; categorical text is dropped; preliminary and corrected results both load.

## Related Pages

[Laboratory results](../01-source-ehr/laboratory-results.md) · [unit normalization](../03-Challenges/lab-unit-normalization.md) · [timestamp selection](../03-Challenges/timestamp-selection.md) · [[example-outpatient-diabetes]]

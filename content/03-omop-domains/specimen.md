---
title: "SPECIMEN Transformation"
type: omop-domain
status: draft
omop_domains: [SPECIMEN, MEASUREMENT]
source_entities: [specimen, laboratory_result]
engineering_stages: [vocabulary-mapping, omop-transformation]
tags: [omop, ehr, specimen]
---

# SPECIMEN Transformation

## Problem

Represent collected material once and link laboratory provenance without confusing the specimen with its measurements.

## Source Semantics

One specimen can support multiple results. Accession identifiers identify laboratory workflow, not the patient.

## Minimal Raw EHR Example

| source_record_id | specimen_id | patient_id | encounter_id | type_code | collected_at | accession |
|---|---|---|---|---|---|---|
| SRC-S4001 | S4001 | P001 | E1001 | BLOOD | 2026-01-14 09:50+02 | SYN-ACC-001 |

## Target OMOP Representation

### `SPECIMEN`

| specimen_id | person_id | specimen_concept_id | specimen_type_concept_id | specimen_datetime | specimen_source_id | specimen_source_value |
|---:|---:|---|---|---|---|---|
| 97001 | 1 | `<standard blood specimen>` | `<EHR specimen record type>` | 2026-01-14 09:50+02 | SYN-ACC-001 | BLOOD |

## Transformation Rules

1. Deduplicate by stable specimen business key, not result count.
2. Map material type and copy collection time.
3. Retain result-to-specimen association in lineage/FACT_RELATIONSHIP if the implementation uses it.

## Field Mapping

| Source | OMOP | Rule |
|---|---|---|
| specimen_id | specimen_id | persistent crosswalk |
| patient_id | person_id | lookup |
| type_code | concept/source value | mapping/direct copy |
| collected_at | specimen datetime/date | direct/derived |
| accession | specimen_source_id | direct copy |

## Mapping Decisions

Accession is preserved but not used as person identity. Result and specimen timestamps may differ and keep their own semantics.

## Validation

- One specimen target per eligible source specimen; all linked results have the same person; material concept is valid.

## Failure Modes

- One specimen is duplicated per result; accession becomes person key; result time replaces collection time; linkage is lost.

## Related Pages

[Laboratory results](../01-source-ehr/laboratory-results.md) · [MEASUREMENT](measurement.md) · [timestamp selection](../03-Challenges/timestamp-selection.md) · [[source-provenance]]

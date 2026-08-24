---
title: "NOTE Transformation"
type: omop-domain
status: draft
omop_domains: [NOTE]
source_entities: [clinical_document]
engineering_stages: [normalization, omop-transformation]
tags: [omop, ehr, note]
---

# NOTE Transformation

## Problem

Represent eligible synthetic clinical text with correct version, authoring time, encoding, and document provenance.

## Source Semantics

A clinical document can be amended; status/version controls which text is current. The fixture contains synthetic text only.

## Minimal Raw EHR Example

| source_record_id | patient_id | encounter_id | document_type | authored_at | version | status | text |
|---|---|---|---|---|---:|---|---|
| SRC-N9101 | P001 | E1001 | progress | 2026-01-14 10:10+02 | 2 | final | Synthetic diabetes follow-up note. |

## Target OMOP Representation

### `NOTE`

| note_id | person_id | note_date | note_datetime | note_type_concept_id | note_class_concept_id | note_title | note_text | encoding_concept_id | visit_occurrence_id | note_source_value |
|---:|---:|---|---|---|---|---|---|---|---:|---|
| 96001 | 1 | 2026-01-14 | 2026-01-14 10:10+02 | `<EHR note type>` | `<progress note class>` | Progress note | Synthetic diabetes follow-up note. | `<UTF-8 encoding>` | 50001 | progress |

## Transformation Rules

1. Select latest final non-deleted version per document key.
2. Normalize encoding without altering clinical text.
3. Map document type/class and link person/visit.

## Field Mapping

| Source | OMOP | Rule |
|---|---|---|
| patient/encounter | person/visit IDs | lookup |
| authored_at | note date/datetime | direct/derived |
| type | note type/class/source | semantic mapping/direct copy |
| text | note_text | direct copy after verified decoding |

## Mapping Decisions

Audit update time never replaces authored time. Superseded text remains in restricted lineage but not the current NOTE row.

## Validation

- One current note per document key; valid encoding; note datetime plausibly relates to visit.

## Failure Modes

- Amendments duplicate notes; text decoding corrupts content; audit time distorts chronology; non-synthetic data enters the public fixture.

## Related Pages

[Staging](../02-engineering/03-staging-model.md) · [timestamp selection](../03-Challenges/timestamp-selection.md) · [provenance](../03-Challenges/source-provenance.md) · [[duplicate-events]]

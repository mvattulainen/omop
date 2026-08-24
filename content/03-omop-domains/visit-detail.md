---
title: "VISIT_DETAIL Transformation"
type: omop-domain
status: stable
omop_domains: [VISIT_DETAIL, VISIT_OCCURRENCE]
source_entities: [encounter_segment]
engineering_stages: [omop-transformation]
tags: [omop, ehr, visit-detail]
---

# VISIT_DETAIL Transformation

## Problem

Represent nested care-setting intervals without duplicating the containing visit.

## Source Semantics

E1004 is an ICU segment inside inpatient encounter E1003 and shares its patient.

## Minimal Raw EHR Example

| source_record_id | encounter_id | parent_encounter_id | class | start_at | end_at |
|---|---|---|---|---|---|
| SRC-E1004 | E1004 | E1003 | ICU | 2026-02-04 01:00+02 | 2026-02-05 09:00+02 |

## Target OMOP Representation

### `VISIT_DETAIL`

| visit_detail_id | person_id | visit_detail_concept_id | visit_detail_start_datetime | visit_detail_end_datetime | visit_occurrence_id | visit_detail_source_value |
|---:|---:|---|---|---|---:|---|
| 51001 | 2 | `<standard intensive-care visit detail>` | 2026-02-04 01:00+02 | 2026-02-05 09:00+02 | 50003 | ICU |

## Transformation Rules

1. Require an eligible parent visit crosswalk.
2. Create one detail per non-duplicate completed segment.
3. Copy normalized boundaries and map segment class.

## Field Mapping

| Source field | OMOP field | Rule |
|---|---|---|
| encounter_id | visit_detail_id | persistent crosswalk |
| parent_encounter_id | visit_occurrence_id | derived relationship lookup |
| class | visit_detail_concept_id | semantic mapping |
| times | visit detail times/dates | direct normalized copy/derived dates |

## Mapping Decisions

Segments never broaden parent bounds. Segment record, hierarchy edge, and mapping version provide lineage.

## Validation

- Detail start/end fall within parent; person matches parent.
- Sibling overlaps are reported for review; identical retransmissions deduplicate.

## Failure Modes

- ICU becomes an extra visit; detail links to wrong parent/person; rounding makes it exceed parent; repeated segment messages duplicate it.

## Related Pages

[VISIT_OCCURRENCE](visit-occurrence.md) · [visit reconstruction](../03-Challenges/visit-reconstruction.md) · [duplicate events](../03-Challenges/duplicate-events.md) · [[example-emergency-to-inpatient]]

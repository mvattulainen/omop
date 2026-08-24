---
title: "VISIT_OCCURRENCE Transformation"
type: omop-domain
status: stable
omop_domains: [VISIT_OCCURRENCE, VISIT_DETAIL]
source_entities: [encounter]
engineering_stages: [normalization, omop-transformation]
tags: [omop, ehr, visit]
---

# VISIT_OCCURRENCE Transformation

## Problem

Represent source contacts without collapsing distinct ED/inpatient phases or duplicating nested segments.

## Source Semantics

Top-level encounter rows are administrative contacts. ICU is a nested segment, not an additional top-level visit.

## Minimal Raw EHR Example

| source_record_id | encounter_id | patient_id | parent | class | start_at | end_at |
|---|---|---|---|---|---|---|
| SRC-E1002 | E1002 | P002 | NULL | ED | 2026-02-03 18:10+02 | 2026-02-03 22:00+02 |
| SRC-E1003 | E1003 | P002 | NULL | IP | 2026-02-03 22:00+02 | 2026-02-08 14:00+02 |

## Target OMOP Representation

### `VISIT_OCCURRENCE`

| visit_occurrence_id | person_id | visit_concept_id | visit_start_datetime | visit_end_datetime | visit_source_value | preceding_visit_occurrence_id |
|---:|---:|---|---|---|---|---:|
| 50002 | 2 | `<standard emergency visit>` | 2026-02-03 18:10+02 | 2026-02-03 22:00+02 | ED | NULL |
| 50003 | 2 | `<standard inpatient visit>` | 2026-02-03 22:00+02 | 2026-02-08 14:00+02 | IP | 50002 |

## Transformation Rules

1. Create a visit for each eligible top-level completed encounter.
2. Map class using versioned encounter mapping.
3. Preserve source boundaries; link chronologically adjacent transfers with `preceding_visit_occurrence_id` rather than collapsing.
4. Route child segments to VISIT_DETAIL.

## Field Mapping

| Source field | OMOP field | Rule |
|---|---|---|
| encounter_id | visit_occurrence_id | persistent crosswalk lookup |
| patient_id | person_id | identity lookup |
| class | visit_concept_id | semantic mapping |
| start_at/end_at | visit datetimes/dates | direct normalized copy/derived date |
| encounter_id | visit_source_value | direct copy or configured class value; identifier retained in lineage |

## Mapping Decisions

ED and inpatient remain two visits to preserve administrative semantics. Exact concept IDs await the pinned snapshot. Row lineage includes `SRC-E1002/SRC-E1003`, rule and mapping versions.

## Validation

- End ≥ start; crosswalk is one-to-one for eligible top-level encounters.
- Child segments do not create extra visits; preceding visit belongs to the same person.

## Failure Modes

- Entire episode is incorrectly collapsed; ICU duplicates inpatient visit; open encounter gets fabricated end time; source encounter cannot be traced.

## Related Pages

[Encounters](../01-source-ehr/encounters.md) · [visit reconstruction](../03-Challenges/visit-reconstruction.md) · [VISIT_DETAIL](visit-detail.md) · [[example-emergency-to-inpatient]]

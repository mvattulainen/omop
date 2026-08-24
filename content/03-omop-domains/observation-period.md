---
title: "OBSERVATION_PERIOD Transformation"
type: omop-domain
status: stable
omop_domains: [OBSERVATION_PERIOD]
source_entities: [encounter, clinical_events]
engineering_stages: [omop-transformation]
tags: [omop, ehr, observation-period]
---

# OBSERVATION_PERIOD Transformation

## Problem

Represent intervals during which the source can support meaningful longitudinal observation, not merely the span between any two dates.

## Source Semantics

The imaginary EHR has no enrollment table. Coverage is inferred from eligible completed encounters and clinical facts under an explicitly documented gap rule.

## Minimal Raw EHR Example

| patient_id | event_id | event_date | eligible |
|---|---|---|---|
| P001 | E1001 | 2026-01-14 | true |
| P001 | O2001-end | 2026-04-13 | true |

## Target OMOP Representation

### `OBSERVATION_PERIOD`

| observation_period_id | person_id | observation_period_start_date | observation_period_end_date | period_type_concept_id |
|---:|---:|---|---|---|
| 40001 | 1 | 2026-01-14 | 2026-04-13 | `<concept for EHR-derived observation period>` |

## Transformation Rules

1. Select eligible event dates after identity resolution.
2. Sort by person and join dates separated by no more than the configured 365-day gap.
3. Set boundaries to actual earliest/latest supported dates; do not pad them.

## Field Mapping

| Source | OMOP field | Rule |
|---|---|---|
| eligible event dates | start/end date | calculated min/max per gap-defined interval |
| patient_id | person_id | identity crosswalk lookup |

## Mapping Decisions

The 365-day rule is a transformation policy, not an EHR fact. It is versioned. Source events and rule version provide lineage for both boundaries.

## Validation

- Start ≤ end and periods for a person do not overlap.
- Every included clinical event lies within a period or has a documented exception.

## Failure Modes

- Using patient registration creates unsupported coverage; future appointments extend the period; arbitrary padding distorts time-at-risk.

## Related Pages

[Encounters](../01-source-ehr/encounters.md) · [OMOP transformation](../02-engineering/06-omop-transformation.md) · [late data](../03-Challenges/late-arriving-data.md) · [[example-outpatient-diabetes]]

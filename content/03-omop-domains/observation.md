---
title: "OBSERVATION Transformation"
type: omop-domain
status: stable
omop_domains: [OBSERVATION]
source_entities: [observation]
engineering_stages: [vocabulary-mapping, omop-transformation]
tags: [omop, ehr, observation]
---

# OBSERVATION Transformation

## Problem

Represent structured non-measurement assertions without using OBSERVATION as a catch-all for failed mappings.

## Source Semantics

SRC-B8001 asserts that P001's recorded smoking status was never-smoker at the encounter time.

## Minimal Raw EHR Example

| source_record_id | patient_id | encounter_id | code | value_code | observed_at |
|---|---|---|---|---|---|
| SRC-B8001 | P001 | E1001 | SMOKE | NEVER | 2026-01-14 09:38+02 |

## Target OMOP Representation

### `OBSERVATION`

| observation_id | person_id | observation_concept_id | observation_datetime | observation_type_concept_id | value_as_concept_id | visit_occurrence_id | observation_source_value | value_source_value |
|---:|---:|---|---|---|---|---:|---|---|
| 94001 | 1 | `<standard smoking-status observation>` | 2026-01-14 09:38+02 | `<EHR observation type>` | `<standard never-smoker answer>` | 50001 | SMOKE | NEVER |

## Transformation Rules

1. Map question and answer separately.
2. Load only when the standard question belongs to Observation; otherwise route by domain.
3. Preserve an unmapped answer as source value with the CDM-compatible fallback.

## Field Mapping

| Source | OMOP | Rule |
|---|---|---|
| patient/encounter | person_id/visit_occurrence_id | lookups |
| question/value codes | concept/value concept fields | semantic mappings |
| observed_at | observation datetime/date | direct normalized copy/derived date |

## Mapping Decisions

OBSERVATION is not an overflow table. Technical/vocabulary provenance includes both source code/value and mapping versions.

## Validation

- Question concept domain is Observation; coded value mapping is valid at event date.
- Source eligible rows reconcile to routed domain, exclusion, or quarantine.

## Failure Modes

- Numeric lab is misclassified; unknown code is dumped here; question mapping succeeds but answer meaning is lost.

## Related Pages

[Source observations](../01-source-ehr/observations.md) · [domain assignment](../03-Challenges/domain-assignment.md) · [unmapped concepts](../03-Challenges/unmapped-concepts.md) · [[05-vocabulary-mapping]]

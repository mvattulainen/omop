---
title: "Unmapped Concepts and concept_id 0"
type: challenge
status: stable
omop_domains: [ALL]
source_entities: [coded_events]
engineering_stages: [vocabulary-mapping, data-quality]
tags: [omop, vocabulary, unmapped]
---

# Unmapped Concepts and `concept_id = 0`

## Problem

Unknown local codes cannot be safely guessed, but silently dropping them loses source facts.

## Source Semantics

An unresolved source code still proves the source recorded something; its precise standardized meaning is not yet established.

## Minimal Raw EHR Example

| source_record_id | code_system | code | display |
|---|---|---|---|
| SRC-L6999 | LOCAL-LAB | NEW-ASSAY | New assay label |

## Incorrect Transformation

Map by display-name similarity without review, drop the row, or use zero without preserving/monitoring the source value.

## Adopted Rule

Attempt valid source/standard resolution. If unresolved and the CDM field permits it, use `concept_id=0`, preserve source code/display and any source concept, record mapping status/reason, and add the code to a prioritized backlog. Reject only when a required structural field prevents a conformant row.

## Target OMOP Representation

| measurement_id | measurement_concept_id | measurement_source_value | lineage status |
|---:|---:|---|---|
| 93999 | 0 | NEW-ASSAY | UNMAPPED_LOCAL_CODE |

## Mapping Decisions

Zero means no matching concept in the release context, not “clinically absent.” A future approved mapping updates the same persistent target identity in a remap release.

## Validation

- Zero rates/counts are reported by source system/code/domain and compared with thresholds/baseline.
- Every zero has non-empty source value and reason; mappings never fabricate domain.

## Failure Modes

- Vocabulary failure is hidden; source value disappears; zero is interpreted as negative finding; later mapping creates a duplicate target row.

## Related Pages

[Vocabulary mapping](../02-engineering/05-vocabulary-mapping.md) · [local codes](local-code-mapping.md) · [domain assignment](domain-assignment.md) · [[conformance-checks]]

---
title: "Source Provenance and Lineage"
type: challenge
status: stable
omop_domains: [ALL]
source_entities: [all]
engineering_stages: [ingestion, normalization, omop-transformation, release]
tags: [omop, provenance, lineage]
---

# Source Provenance and Lineage

## Problem

OMOP rows often combine identity, visit, vocabulary, and value rules; source-value fields alone cannot explain the result.

## Source Semantics

Clinical provenance is the care assertion, technical lineage is the processing path, and vocabulary provenance is the mapping context.

## Minimal Raw EHR Example

| source_record_id | patient_id | encounter_id | code | clinical_at |
|---|---|---|---|---|
| SRC-D5001 | P001 | E1001 | E11.9 | 2026-01-14 09:42+02 |

## Incorrect Transformation

Store only `condition_source_value=E11.9`; the source table, record, visit resolution, rule, and vocabulary snapshot become unknowable.

## Adopted Rule

Maintain row lineage keyed by `(target_table, target_id)` with contributing source system/table/record/version/batch, transformation rule/version, crosswalks, mapping/version/snapshot, run, and inclusion/exclusion action. Keep source values in native CDM fields too.

## Target OMOP Representation

| target | source | rule | identity/visit crosswalk | vocabulary |
|---|---|---|---|---|
| CONDITION_OCCURRENCE:90001 | diagnosis:SRC-D5001 | condition-v1 | P001→1; E1001→50001 | ICD10CM→SNOMED, snapshot `<pinned>` |

## Mapping Decisions

Lineage tables are release artifacts outside core CDM tables. Many source records may contribute to one row, and exclusions are first-class actions.

## Validation

- Sample every domain and round-trip target → source bytes → rule/mapping.
- Every target has at least one lineage source; every eligible canonical input has target or exclusion action.

## Failure Modes

- Broken lineage; only last source version survives; mapping snapshot missing; many-to-one contributors vanish; deleted target leaves no audit trail.

## Related Pages

[Raw ingestion](../02-engineering/02-raw-ingestion.md) · [OMOP transformation](../02-engineering/06-omop-transformation.md) · [release process](../02-engineering/10-release-process.md) · [[source-vs-omop-reconciliation]]

---
title: "Duplicate Event Handling"
type: challenge
status: stable
omop_domains: [CONDITION_OCCURRENCE, DRUG_EXPOSURE, MEASUREMENT]
source_entities: [diagnosis, medication_administration, laboratory_result]
engineering_stages: [staging, normalization, omop-transformation]
tags: [omop, ehr, deduplication]
---

# Duplicate Event Handling

## Problem

Retransmissions, corrections, billing copies, and genuinely repeated clinical events can look alike.

## Source Semantics

Same source business key/version lineage means one evolving record. Similar code/person/time without shared identity may be distinct facts.

## Minimal Raw EHR Example

| source record | representation | code | time | version |
|---|---|---|---|---:|
| SRC-D5001 | clinical | E11.9 | 09:42 | 2 |
| SRC-D5001 | retransmission | E11.9 | 09:42 | 2 |
| SRC-BILL-1 | billing | E11.9 | 09:42 | 1 |

## Incorrect Transformation

Load all three, or collapse every same-day diabetes record regardless of provenance.

## Adopted Rule

First collapse byte/business-key retransmissions. Next select the current corrected version. Collapse cross-table representations only under an approved semantic rule with evidence; keep a many-source-to-one lineage bridge. Never deduplicate solely on a broad time window.

## Target OMOP Representation

| target | contributing source | excluded reason |
|---|---|---|
| CONDITION_OCCURRENCE 90001 | SRC-D5001 version 2 | retransmission; billing representation linked as duplicate evidence |

## Mapping Decisions

Deterministic fingerprint is `(source system, entity, business key, version)` for technical duplicates. Semantic duplicate rules are entity-specific and versioned.

## Validation

- No identical current source identity produces multiple target rows.
- Source count = target facts + technical duplicates + semantic duplicates + other exclusions; repeat-event test cases remain distinct.

## Failure Modes

- Data duplication; aggressive window collapse loses true recurrence; corrected and original both load; lineage names only one contributor.

## Related Pages

[Staging](../02-engineering/03-staging-model.md) · [CONDITION_OCCURRENCE](../03-omop-domains/condition-occurrence.md) · [source provenance](source-provenance.md) · [[source-vs-omop-reconciliation]]

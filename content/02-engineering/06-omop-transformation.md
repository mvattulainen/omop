---
title: "OMOP Domain Transformation"
type: engineering
status: stable
engineering_stages: [omop-transformation]
tags: [omop, ehr, etl]
---

# OMOP Domain Transformation

## Responsibility

Materialize CDM 5.4 rows and persistent crosswalks from normalized, vocabulary-routed events.

## Inputs

- Canonical events, mapping decisions, identity and visit crosswalks

## Outputs and artifacts

- OMOP clinical tables, crosswalks, exclusion ledger, and row-level lineage

## Example

| Canonical event | Target |
|---|---|
| diagnosis `SRC-D5001`, P001, E1001, E11.9 | CONDITION_OCCURRENCE `90001`, person `1`, visit `50001` |

## Execution contract

Dependencies load in order: PERSON, OBSERVATION_PERIOD/VISIT, clinical facts, then relationships. Surrogate identifiers come from persistent crosswalks and are never `MAX(id)+1`. Each target row links to source record, rule version, mapping version, and run.

## Incremental behavior and errors

New facts obtain identifiers; corrections update the same target identity; tombstones retire/delete per release policy; reruns are idempotent. Missing person/required concept/date is rejected. Optional unresolved visit links are measured and documented.

## Quality gates

Required fields are populated, foreign keys resolve, concept domains conform, target keys are unique, and eligible inputs reconcile to output plus exclusion.

Related: [domain index](../03-omop-domains/index.md) · [[source-provenance]] · [[duplicate-events]]

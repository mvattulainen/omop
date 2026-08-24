---
title: "Semantic Normalization"
type: engineering
status: stable
engineering_stages: [normalization]
tags: [omop, ehr, normalization]
---

# Semantic Normalization

## Responsibility

Convert source-specific shapes into canonical clinical events while preserving source meaning.

## Inputs

- Typed current/history staging and controlled-source dictionaries

## Outputs and artifacts

- Canonical patient, encounter, diagnosis, procedure, drug event, lab event, and observation views
- Identity/encounter candidate relationships and normalized value pairs

## Example

| Source | Canonical event |
|---|---|
| administration `A3002`, status `held` | medication event `outcome=NOT_GIVEN`; `eligible_drug_exposure=false` |

## Execution contract

Rules are deterministic and versioned. Direct copies remain distinguishable from calculations. Original timestamps, values, units, statuses, and source record identifiers remain in lineage. No vocabulary concept or OMOP surrogate key is assigned here.

## Incremental behavior and errors

Changed keys recompute their dependent canonical events. Unknown statuses/codes are quarantined, not guessed. A normalized event is idempotently keyed by source system, entity, and source record identifier.

## Quality gates

Every eligible staging record yields exactly one canonical event or an explicit exclusion/rejection reason; value and status distributions reconcile.

Related: [[03-staging-model]] · [[05-vocabulary-mapping]] · [[medication-order-vs-administration]]

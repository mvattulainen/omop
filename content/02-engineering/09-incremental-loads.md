---
title: "Incremental Loads"
type: engineering
status: stable
engineering_stages: [incremental-loads]
tags: [omop, incremental, change-data-capture]
---

# Incremental Loads

## Responsibility

Apply new, changed, deleted, late-arriving, and merge events without changing stable OMOP identities.

## Inputs

- Ordered source changes, prior snapshot/crosswalks, current rules/mappings

## Outputs and artifacts

- New candidate snapshot, change ledger, impacted-key graph, reconciliation delta

## Rules

1. Inserts create a crosswalk once.
2. Corrections update the target row tied to the source key.
3. Tombstones remove/retire the derived fact and record the action.
4. Late facts reopen affected patient/encounter partitions.
5. Patient merges re-parent facts to the winner while retaining aliases.
6. Vocabulary changes create an explicit remap release; they are not ordinary source changes.

Identifiers are persistent across releases. Full rebuild and ordered replay MUST yield equivalent business content and identical crosswalk keys. Failed batches commit nothing and resume from the previous checkpoint.

## Quality gates

No change is skipped or applied twice; delta counts reconcile; replay/rebuild comparison passes; all re-parented foreign keys resolve.

Related: [[late-arriving-data]] · [[patient-identity-resolution]] · [[source-provenance]]

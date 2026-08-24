---
title: "Raw Ingestion"
type: engineering
status: stable
engineering_stages: [ingestion]
tags: [omop, ehr, ingestion, lineage]
---

# Raw Ingestion

## Responsibility

Capture source bytes and extraction context without semantic alteration.

## Inputs

- Accepted source batch and source manifest

## Outputs and artifacts

- Immutable raw partition by source/table/batch
- Manifest with checksum, row count, extraction interval, schema fingerprint, and received time

## Example

| source row | raw additions |
|---|---|
| `SRC-D5001,P001,E1001,E11.9` | `_batch_id=B001`, `_ingested_at=2026-08-23T07:00Z`, `_row_hash=<stable hash>` |

## Execution contract

No source column is renamed or coerced in raw storage. Re-delivery with the same batch/checksum is a no-op; the same batch identifier with a different checksum is rejected. Deletes arrive as explicit tombstones. Errors quarantine the batch atomically.

## Quality gates and release implications

Manifest count equals parsed count, checksum matches, and no undeclared file is admitted. Raw partitions are persistent release dependencies and support replay.

Related: [[01-source-profiling]] · [[03-staging-model]] · [[source-provenance]]

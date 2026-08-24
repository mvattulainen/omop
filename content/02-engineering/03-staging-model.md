---
title: "Staging Model"
type: engineering
status: stable
engineering_stages: [staging]
tags: [omop, ehr, staging]
---

# Staging Model

## Responsibility

Produce typed, lossless history and current-state views without assigning OMOP meaning.

## Inputs

- Immutable raw batches and source schemas

## Outputs and artifacts

- Typed `stg_*_history`, deduplicated `stg_*_current`, rejection table, and source-key audit

## Example

| Raw | Typed staging |
|---|---|
| `event_time='2026-01-14T09:42:00+02:00'` | `event_ts_utc=2026-01-14 07:42Z`, `event_tz_offset='+02:00'`, original text preserved |

## Execution contract

Types, trim rules, and null tokens are declared per field. Current state chooses the greatest `(updated_at, batch_sequence)` per source key; ties with differing payloads are rejected. The same history produces the same current view.

## Incremental behavior and errors

Only affected keys are recomputed, but a full rebuild must be equivalent. Parse errors retain source row, field, error code, and batch. No rejected row reaches normalization.

## Quality gates

Keys are unique in current views; typed/current plus rejected counts reconcile to eligible raw rows; all foreign-key orphans are reported.

Related: [[04-normalization]] · [[late-arriving-data]] · [[timestamp-selection]]

---
title: "Source Profiling"
type: engineering
status: stable
engineering_stages: [profiling]
source_entities: [all]
tags: [omop, ehr, profiling]
---

# Source Profiling

## Responsibility

Measure shape and semantic drift before transformation rules run.

## Inputs

- Versioned source extracts and data dictionary
- Previous accepted profile baseline

## Outputs and artifacts

- Per-table counts, null rates, uniqueness, value frequencies, ranges, and orphan counts
- Timestamp/code-system inventories and drift findings keyed by `run_id`

## Example

| Metric | Source result | Gate |
|---|---:|---|
| diagnosis rows | 1 | informational |
| diagnosis patient orphans | 0 | must equal 0 |
| lab unit null rate | 0% | must be below approved threshold |

## Execution contract

Profiling is read-only and idempotent. Incremental batches are profiled alone and cumulatively. Schema removal, new undeclared status, broken keys, or material distribution drift raises a quarantine finding; it never silently changes rules.

## Quality gates and release implications

All primary keys must meet the source contract, all code systems must be declared, and every timestamp field must have a documented meaning/time zone. Critical drift blocks ingestion promotion.

Related: [source model](../01-source-ehr/source-model-overview.md) · [[02-raw-ingestion]] · [[source-vs-omop-reconciliation]]

---
title: "Late-Arriving Data"
type: challenge
status: stable
omop_domains: [OBSERVATION_PERIOD, VISIT_OCCURRENCE, ALL_CLINICAL]
source_entities: [all]
engineering_stages: [incremental-loads]
tags: [omop, incremental, late-data]
---

# Late-Arriving Data

## Problem

A clinically old event can arrive after a release and change visit links, observation periods, deduplication, or mappings.

## Source Semantics

Extraction/arrival time describes delivery; clinical time determines chronology. Late does not mean invalid.

## Minimal Raw EHR Example

| source_record_id | clinical_at | arrived_at | affected encounter |
|---|---|---|---|
| SRC-L6000 | 2026-01-14 09:40+02 | 2026-03-01 08:00Z | E1001 |

## Incorrect Transformation

Ignore records older than the last cutoff, or insert the fact without recomputing its patient/visit/observation-period dependencies.

## Adopted Rule

Order changes by arrival ledger, then reopen the affected source key, patient, encounter, deduplication group, and observation-period partition. Recompute deterministically with current approved rules. Preserve existing target IDs for existing source facts and allocate one ID for the new fact.

## Target OMOP Representation

| impact | before | after |
|---|---|---|
| new measurement | absent | persistent new MEASUREMENT row linked to visit 50001 |
| observation period | boundary/reconciliation evaluated | updated only if eligibility rule changes boundary |

## Mapping Decisions

Clinical event date—not arrival date—populates OMOP. The release manifest discloses the new cutoff and changed historical partitions.

## Validation

- Ordered incremental replay equals full rebuild for affected data and stable identifiers.
- Late-event counts/age are reported; visit links and observation-period coverage revalidate.

## Failure Modes

- Event is lost; arrival date distorts clinical time; dependent rows remain stale; existing IDs churn; same late event applies twice.

## Related Pages

[Incremental loads](../02-engineering/09-incremental-loads.md) · [timestamp selection](timestamp-selection.md) · [OBSERVATION_PERIOD](../03-omop-domains/observation-period.md) · [[source-provenance]]

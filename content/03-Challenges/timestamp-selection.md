---
title: "Clinical Timestamp Selection"
type: challenge
status: stable
omop_domains: [VISIT_OCCURRENCE, CONDITION_OCCURRENCE, DRUG_EXPOSURE, MEASUREMENT]
source_entities: [all_clinical_events]
engineering_stages: [staging, normalization]
tags: [omop, ehr, time]
---

# Clinical Timestamp Selection

## Problem

Source rows expose ordered, collected, performed, resulted, recorded, and updated times with different meanings.

## Source Semantics

For a lab, collection time is the clinical event; result and update times describe workflow/audit.

## Minimal Raw EHR Example

| record | ordered_at | collected_at | resulted_at | updated_at |
|---|---|---|---|---|
| SRC-L6001 | 2026-01-14 09:45+02 | 2026-01-14 09:50+02 | 2026-01-14 12:30+02 | 2026-01-14 12:35+02 |

## Incorrect Transformation

Use `updated_at` for every OMOP datetime because it is consistently populated.

## Adopted Rule

Choose by event semantics: encounter boundaries for visits, clinical diagnosis time for conditions, administration time for given dose, collection time for lab, performed time for procedure. Apply a source-specific fallback hierarchy only when primary time is absent and record which fallback was used.

## Target OMOP Representation

| target row | selected datetime | provenance alternatives |
|---|---|---|
| MEASUREMENT 93001 | 2026-01-14 09:50+02 | ordered 09:45; resulted 12:30; updated 12:35 |

## Mapping Decisions

Normalize to UTC for processing while retaining offset and original text. Derive OMOP date from selected clinical datetime in the declared source time zone.

## Validation

- Fallback-use rates are measured; impossible sequences are flagged.
- Selected time remains stable on audit-only updates and falls plausibly relative to visit.

## Failure Modes

- Temporal distortion from result/update time; date shifts during UTC conversion; null primary time silently becomes extraction time; daylight-saving ambiguity is ignored.

## Related Pages

[Staging](../02-engineering/03-staging-model.md) · [MEASUREMENT](../03-omop-domains/measurement.md) · [VISIT_OCCURRENCE](../03-omop-domains/visit-occurrence.md) · [[late-arriving-data]]

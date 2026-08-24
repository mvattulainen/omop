---
title: "DRUG_EXPOSURE Transformation"
type: omop-domain
status: stable
omop_domains: [DRUG_EXPOSURE]
source_entities: [medication_order, medication_administration]
engineering_stages: [normalization, vocabulary-mapping, omop-transformation]
tags: [omop, ehr, medication]
---

# DRUG_EXPOSURE Transformation

## Problem

Represent outpatient prescribing intent and inpatient given doses without inflating exposure from held or merely ordered doses.

## Source Semantics

O2001 is an outpatient prescription interval. A3001 is a given inpatient dose; A3002 is held.

## Minimal Raw EHR Example

| record | context | medication | clinical time | amount | status |
|---|---|---|---|---:|---|
| SRC-O2001 | outpatient order | MET500 | 2026-01-14 to 2026-04-13 | 500 mg | active |
| SRC-A3001 | inpatient administration | CEF1G | 2026-02-04 02:15+02 | 1 g | given |
| SRC-A3002 | inpatient administration | CEF1G | 2026-02-04 10:15+02 | 1 g | held |

## Target OMOP Representation

### `DRUG_EXPOSURE`

| drug_exposure_id | person_id | drug_concept_id | drug_exposure_start_datetime | drug_exposure_end_datetime | drug_type_concept_id | quantity | visit_occurrence_id | drug_source_value |
|---:|---:|---|---|---|---|---:|---:|---|
| 92001 | 1 | `<standard metformin clinical drug/ingredient per mapping>` | 2026-01-14 00:00 | 2026-04-13 23:59:59 | `<EHR prescription type>` | `<derived only if directions sufficient>` | 50001 | MET500 |
| 92002 | 2 | `<standard ceftriaxone clinical drug/ingredient per mapping>` | 2026-02-04 02:15+02 | 2026-02-04 02:15+02 | `<EHR administration type>` | 1 | 50003 | CEF1G |

## Transformation Rules

1. Outpatient active prescriptions create order-based exposure using explicit start/end; never infer quantity from insufficient directions.
2. Inpatient administrations create dose-level exposure only when status is `given`.
3. Held/refused/cancelled administrations create no exposure and retain exclusion lineage.
4. Map formulation with strength/unit metadata and assign type from evidence source.

## Field Mapping

| Source | OMOP | Rule |
|---|---|---|
| medication code/system | drug concept/source fields | vocabulary mapping/direct source copy |
| order start/end | exposure start/end | direct/derived dates for outpatient orders |
| administered_at | start/end datetime | direct copy for instantaneous given dose |
| dose/unit | quantity | normalize only under mapped formulation rule |
| encounter | visit_occurrence_id | crosswalk lookup |

## Mapping Decisions

Orders and administrations receive different type concepts. A held dose is not represented as exposure. Each row retains source record, order relationship, local code, mapping evidence, and rule version.

## Validation

- No held/refused/cancelled row maps to DRUG_EXPOSURE.
- Start ≤ end; administration occurs within/near linked visit; eligible evidence reconciles to exposure plus exclusion.

## Failure Modes

- Every scheduled dose becomes exposure; order and administration double-count; days supply or quantity is fabricated; grams are stored as milligrams without conversion.

## Related Pages

[Medication orders](../01-source-ehr/medications.md) · [administrations](../01-source-ehr/medication-administrations.md) · [order versus administration](../03-Challenges/medication-order-vs-administration.md) · [[example-medication-administration]]

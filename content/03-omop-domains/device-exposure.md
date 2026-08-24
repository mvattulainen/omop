---
title: "DEVICE_EXPOSURE Transformation"
type: omop-domain
status: draft
omop_domains: [DEVICE_EXPOSURE]
source_entities: [device_event, procedure]
engineering_stages: [vocabulary-mapping, omop-transformation]
tags: [omop, ehr, device]
---

# DEVICE_EXPOSURE Transformation

## Problem

Represent documented patient exposure to a device, not inventory movement or a mere procedure code.

## Source Semantics

The extension fixture describes a device applied to a patient with UDI and start/end times.

## Minimal Raw EHR Example

| source_record_id | patient_id | encounter_id | local_device_code | udi | applied_at | removed_at | status |
|---|---|---|---|---|---|---|---|
| SRC-V8101 | P002 | E1003 | CVC-3L | (01)SYNTHETIC | 2026-02-04 03:00+02 | 2026-02-06 09:00+02 | applied |

## Target OMOP Representation

### `DEVICE_EXPOSURE`

| device_exposure_id | person_id | device_concept_id | device_exposure_start_datetime | device_exposure_end_datetime | device_type_concept_id | unique_device_id | visit_occurrence_id | device_source_value |
|---:|---:|---|---|---|---|---|---:|---|
| 95001 | 2 | `<standard central venous catheter device>` | 2026-02-04 03:00+02 | 2026-02-06 09:00+02 | `<EHR device record type>` | (01)SYNTHETIC | 50003 | CVC-3L |

## Transformation Rules

1. Include applied/implanted patient events only.
2. Map device code to a Device-domain standard concept.
3. Preserve UDI as supplied and do not derive it from local inventory identifiers.

## Field Mapping

| Source | OMOP | Rule |
|---|---|---|
| patient/encounter | person/visit IDs | lookups |
| device code | concept/source fields | mapping/direct copy |
| applied/removed times | start/end | direct normalized copy |
| udi | unique_device_id | direct copy |

## Mapping Decisions

Inventory scans without patient exposure are excluded with reason. Lineage includes device record and application status.

## Validation

- Device concept domain is Device; end ≥ start; exposure is linked to the correct person.

## Failure Modes

- Inventory item becomes exposure; procedure and device rows are conflated; UDI is fabricated; removal before application is accepted.

## Related Pages

[Procedures](../01-source-ehr/procedures.md) · [domain assignment](../03-Challenges/domain-assignment.md) · [OMOP transformation](../02-engineering/06-omop-transformation.md) · [[source-provenance]]

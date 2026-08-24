---
title: "Source EHR Summary"
type: source
status: stable
source_entities:
  - patient
  - encounter
  - diagnosis
  - procedure
  - medication_order
  - medication_administration
  - laboratory_result
  - observation
  - code_catalogue
tags:
  - omop
  - ehr
  - source
  - summary
---

# Source EHR Summary

The source topics are ordered from the shared source contract and patient identity through encounter-linked clinical records and their code systems.

## 1. Source model

The imaginary source is an append/change-captured relational EHR extract in which business keys remain stable, corrections arrive with later update timestamps and deletions are represented as tombstones. Clinical timestamps retain their offsets, extraction timestamps use UTC, every event must resolve to a canonical patient and unresolved encounter relationships are quarantined rather than guessed.

## 2. Patients

A source patient row represents an EHR registration identity rather than automatically proving a unique biological person. Patient aliases and explicit merge records determine identity resolution, unknown birth-date components are never fabricated, administrative sex retains its source value and every clinical event must resolve through a persistent patient-to-PERSON crosswalk.

## 3. Encounters

Source encounters represent administrative contacts, while encounter segments represent nested locations or care settings within a parent contact. Adjacent emergency and inpatient encounters remain distinct source facts, ICU segments remain within their parent admission and all intervals must have valid chronology, consistent patients and resolvable hierarchy relationships.

## 4. Diagnoses

Encounter diagnoses, longitudinal problem-list entries and billing diagnoses represent different kinds of assertions and must retain that provenance. Confirmed and otherwise eligible records can support `CONDITION_OCCURRENCE`, corrected versions supersede earlier versions and source type, code system, status, clinical time and encounter relationship determine mapping, deduplication and condition type.

## 5. Procedures

A procedure record may describe a performed, planned or cancelled activity, but only performed events are eligible for occurrence tables. The clinical performance time and status must be valid, and the mapped standard concept—not the name of the source table—determines whether the event belongs in `PROCEDURE_OCCURRENCE`, `MEASUREMENT`, `DEVICE_EXPOSURE` or another supported domain.

## 6. Medication orders

A medication order represents prescribing intent rather than proof that a medicine was taken or administered. Eligible outpatient orders may support exposure intervals when explicit start, end, dose and formulation information exists, whereas inpatient orders usually provide context for administrations and must not independently inflate `DRUG_EXPOSURE` counts.

## 7. Medication administrations

Medication-administration rows record dose-level outcomes linked to an order, patient and encounter. A `given` outcome is evidence of exposure, while held, refused and cancelled doses remain clinically useful negative facts in lineage but do not create `DRUG_EXPOSURE`; duplicate messages must collapse to one administration identity.

## 8. Laboratory results

A laboratory result combines the test, specimen context, typed result, unit, reference interval, status and multiple workflow timestamps. Collection time is normally the clinical measurement time, corrected final versions supersede preliminary results and the original value and unit remain available even when an approved normalized derivative is calculated.

## 9. Observations

Source observations are structured assertions such as smoking status or housing circumstances, but the source table name does not force the OMOP target domain. The question and value codes must be validated against the local catalogue, the mapped standard concept determines routing and unsupported values remain traceable rather than being discarded or forced into `OBSERVATION`.

## 10. Code systems

Every coded source value is interpreted together with its code system and validity interval, never by display text alone. Standard systems such as ICD-10-CM resolve through OMOP vocabulary relationships, local laboratory, drug, procedure, encounter and observation codes use reviewed versioned mappings, and unresolved codes retain their source values while entering the mapping backlog and release metrics.

## Related pages

- [Detailed source model](source-model-overview.md)
- [Engineering pipeline summary](../02-engineering/engineering-pipeline-summary.md)
- [Transformation challenges summary](../03-Challenges/challenge-summary.md)

Obsidian: [[source-model-overview]] · [[engineering-pipeline-summary]] · [[challenge-summary]]

---
title: "Vault Home"
type: meta
status: stable
tags: [omop, ehr, navigation]
---

# EHR to OMOP Transformation Vault

Follow the transformation chain from clinical meaning to a validated, reproducible OMOP release.

## Section summaries

- [Source EHR summary](01-source-ehr/source-ehr-summary.md)
- [Engineering pipeline summary](02-engineering/engineering-pipeline-summary.md)
- [Transformation challenges summary](03-Challenges/challenge-summary.md)
- [Data quality summary](04-Quality/quality-summary.md)

## Learn the pipeline

Start with the [10-step engineering pipeline summary](02-engineering/engineering-pipeline-summary.md), then explore the detailed stages:

1. [Understand the source](01-source-ehr/source-model-overview.md)
2. [Profile and ingest it](02-engineering/01-source-profiling.md)
3. [Normalize and map vocabulary](02-engineering/04-normalization.md)
4. [Build OMOP rows](02-engineering/06-omop-transformation.md)
5. [Validate and release](02-engineering/07-data-quality.md)

## Browse OMOP domains

[PERSON](03-omop-domains/person.md) · [OBSERVATION_PERIOD](03-omop-domains/observation-period.md) · [VISIT_OCCURRENCE](03-omop-domains/visit-occurrence.md) · [VISIT_DETAIL](03-omop-domains/visit-detail.md) · [CONDITION_OCCURRENCE](03-omop-domains/condition-occurrence.md) · [PROCEDURE_OCCURRENCE](03-omop-domains/procedure-occurrence.md) · [DRUG_EXPOSURE](03-omop-domains/drug-exposure.md) · [MEASUREMENT](03-omop-domains/measurement.md) · [OBSERVATION](03-omop-domains/observation.md) · [DEVICE_EXPOSURE](03-omop-domains/device-exposure.md) · [DEATH](03-omop-domains/death.md) · [NOTE](03-omop-domains/note.md) · [SPECIMEN](03-omop-domains/specimen.md)

## Solve a transformation challenge

Begin with the [ranked transformation challenges summary](03-Challenges/challenge-summary.md), or go directly to a specific problem:

- Identity and time: [patient identity](03-Challenges/patient-identity-resolution.md), [visit reconstruction](03-Challenges/visit-reconstruction.md), [timestamp selection](03-Challenges/timestamp-selection.md)
- Meaning and vocabulary: [local codes](03-Challenges/local-code-mapping.md), [domain assignment](03-Challenges/domain-assignment.md), [unmapped concepts](03-Challenges/unmapped-concepts.md)
- Event integrity: [orders versus administrations](03-Challenges/medication-order-vs-administration.md), [duplicate events](03-Challenges/duplicate-events.md), [late-arriving data](03-Challenges/late-arriving-data.md)

## Follow a patient story

- [P001: outpatient diabetes](05-examples/example-outpatient-diabetes.md)
- [P002: emergency to inpatient](05-examples/example-emergency-to-inpatient.md)
- [P002: inpatient medication](05-examples/example-medication-administration.md)

## Assurances

- All records are synthetic.
- Source values and lineage are preserved.
- Vocabulary-dependent identifiers are not fabricated.
- Each adopted rule has validation and documented failure modes.

Obsidian: [[source-model-overview]] · [[engineering-overview]] · [[challenge-index]] · [[quality-framework]]

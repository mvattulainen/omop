---
title: "Imaginary EHR Source Model"
type: source
status: stable
source_entities: [patient, encounter, diagnosis, procedure, medication_order, medication_administration, laboratory_result, observation]
tags: [omop, ehr, source]
---

# Imaginary EHR Source Model

The source is an append/change-captured relational EHR extract. A record's business key is stable, while corrections arrive with a later `updated_at`. Deletes are tombstones. Clinical timestamps are stored with offsets; extraction timestamps are UTC.

```text
patient ─┬─ encounter ─┬─ diagnosis
         │             ├─ procedure
         │             ├─ medication_order ─ medication_administration
         │             ├─ laboratory_result ─ specimen
         │             └─ observation
         └─ patient_alias / patient_merge
```

## Canonical scenarios

| Scenario | Patient | Source records | Primary OMOP targets |
|---|---|---|---|
| Outpatient diabetes | P001 | E1001, D5001, L6001, O2001 | VISIT_OCCURRENCE, CONDITION_OCCURRENCE, MEASUREMENT, DRUG_EXPOSURE |
| ED to inpatient/ICU | P002 | E1002, E1003, E1004 | VISIT_OCCURRENCE, VISIT_DETAIL |
| Inpatient medication | P002 | O2002, A3001, A3002 | DRUG_EXPOSURE; held dose retained only in lineage |

The canonical flat fixture is [synthetic-ehr.csv](../05-Meta/assets/data/synthetic-ehr.csv). Page tables are authoritative semantic views of that fixture, not a second dataset.

## Source-wide invariants

- Every clinical row resolves to one canonical patient after merge handling.
- Encounter-linked events may arrive before the encounter extract and are quarantined until resolvable.
- `recorded_at` is audit time, not automatically clinical event time.
- Local codes have meaning only with their declared code system and mapping version.

## Navigation

[Patients](patients.md) · [Encounters](encounters.md) · [Diagnoses](diagnoses.md) · [Procedures](procedures.md) · [Medications](medications.md) · [Administrations](medication-administrations.md) · [Laboratory results](laboratory-results.md) · [Observations](observations.md) · [Code systems](code-systems.md)

Related: [[03-staging-model]] · [[example-outpatient-diabetes]]

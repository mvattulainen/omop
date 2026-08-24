---
title: "Worked Example: Outpatient Diabetes"
type: example
status: stable
omop_domains: [PERSON, OBSERVATION_PERIOD, VISIT_OCCURRENCE, CONDITION_OCCURRENCE, MEASUREMENT, DRUG_EXPOSURE]
source_entities: [patient, encounter, diagnosis, laboratory_result, medication_order]
tags: [omop, example, diabetes]
---

# Worked Example: Outpatient Diabetes

## Clinical Story

P001 attends an outpatient visit, receives a type 2 diabetes diagnosis, has HbA1c 7.4% collected, and receives an outpatient metformin prescription.

## Source Records

| record | entity | clinical meaning |
|---|---|---|
| SRC-P001 | patient P001 | born 1980-04-12, administrative sex F |
| SRC-E1001 | encounter E1001 | AMB, 09:30–10:20+02 |
| SRC-D5001 | diagnosis | E11.9 ICD10CM, confirmed at 09:42 |
| SRC-L6001 | lab | HBA1C-LOCAL, 7.4%, collected 09:50 |
| SRC-O2001 | order | MET500 500 mg, 2026-01-14 through 2026-04-13 |

## Transformation Sequence

1. Identity crosswalk maps P001 → person 1.
2. Encounter class maps E1001 → outpatient visit 50001.
3. E11.9 resolves to its source concept and standard Condition concept.
4. Local HbA1c and percent unit map to standard Measurement/unit concepts; collection time is selected.
5. Outpatient order maps to a prescription-type exposure; no administration is claimed.

## Generated OMOP Records

| table | id | essential representation |
|---|---:|---|
| PERSON | 1 | birth 1980-04-12; gender `<female>`; source P001 |
| OBSERVATION_PERIOD | 40001 | 2026-01-14 to 2026-04-13 under documented event-span rule |
| VISIT_OCCURRENCE | 50001 | `<outpatient>`, 09:30–10:20+02 |
| CONDITION_OCCURRENCE | 90001 | `<type 2 diabetes>`, E11.9, visit 50001 |
| MEASUREMENT | 93001 | `<HbA1c>`, 7.4 `<percent>`, visit 50001 |
| DRUG_EXPOSURE | 92001 | `<metformin>`, prescription 2026-01-14–04-13, visit 50001 |

## Vocabulary Decisions

The transformation chain is source value → source vocabulary concept → standard concept → standard domain. Placeholders are resolved only against the pinned release snapshot. Local codes additionally require approved mapping evidence.

## Validation Results

| Check | Result |
|---|---|
| all person/visit foreign keys resolve | pass |
| condition domain/type/source preserved | pass |
| HbA1c value/unit and collection time preserved | pass |
| order represented as prescription, not administration | pass |
| five eligible clinical/source entities reconcile | pass |

## Related Pages

[Source model](../01-source-ehr/source-model-overview.md) · [CONDITION_OCCURRENCE](../03-omop-domains/condition-occurrence.md) · [MEASUREMENT](../03-omop-domains/measurement.md) · [DRUG_EXPOSURE](../03-omop-domains/drug-exposure.md) · [[local-code-mapping]]

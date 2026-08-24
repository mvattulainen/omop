---
title: "Example Patient P001"
type: example
status: stable
omop_domains: [PERSON, VISIT_OCCURRENCE, CONDITION_OCCURRENCE, MEASUREMENT, DRUG_EXPOSURE]
source_entities: [patient, encounter, diagnosis, laboratory_result, medication_order]
tags: [omop, example, synthetic-data]
---

# Example Patient P001

P001 is a wholly synthetic patient used as the stable outpatient vertical slice.

## Source timeline

| Time | Record | Meaning |
|---|---|---|
| 1980-04-12 | SRC-P001 | synthetic birth date |
| 2026-01-14 09:30+02 | SRC-E1001 | outpatient visit starts |
| 09:42 | SRC-D5001 | encounter diagnosis E11.9 |
| 09:50 | SRC-L6001 | HbA1c collected, 7.4% |
| 10:05 | SRC-O2001 | outpatient metformin order |

## Crosswalks

| Source | OMOP |
|---|---|
| P001 | PERSON 1 |
| E1001 | VISIT_OCCURRENCE 50001 |
| SRC-D5001 | CONDITION_OCCURRENCE 90001 |
| SRC-L6001 | MEASUREMENT 93001 |
| SRC-O2001 | DRUG_EXPOSURE 92001 |

## Assertions

- Every target row belongs to person 1 and links to visit 50001 when appropriate.
- Selected timestamps order the story correctly; the lab uses collection time.
- No authoritative vocabulary IDs are asserted before a vocabulary snapshot is pinned.

Full trace: [outpatient diabetes](example-outpatient-diabetes.md) · [source fixture](../05-Meta/assets/data/synthetic-ehr.csv) · [[source-provenance]]

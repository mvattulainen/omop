---
title: "Worked Example: Emergency to Inpatient"
type: example
status: stable
omop_domains: [PERSON, VISIT_OCCURRENCE, VISIT_DETAIL]
source_entities: [patient, encounter, encounter_segment]
tags: [omop, example, visit]
---

# Worked Example: Emergency to Inpatient

## Clinical Story

P002 enters the emergency department, transfers to inpatient care, spends part of the admission in ICU, and is discharged.

## Source Records

| record | encounter | hierarchy/class | interval |
|---|---|---|---|
| SRC-E1002 | E1002 | top-level ED | Feb 3 18:10–22:00+02 |
| SRC-E1003 | E1003 | top-level IP | Feb 3 22:00–Feb 8 14:00+02 |
| SRC-E1004 | E1004 | child of E1003, ICU | Feb 4 01:00–Feb 5 09:00+02 |

## Transformation Sequence

1. P002 maps to person 2.
2. The two top-level encounters map to visits 50002 and 50003.
3. Adjacency and transfer semantics set 50002 as preceding 50003; rows are not collapsed.
4. Child E1004 maps to visit detail 51001 within visit 50003.

## Generated OMOP Records

| table | id | concept | interval | relationship |
|---|---:|---|---|---|
| VISIT_OCCURRENCE | 50002 | `<emergency>` | Feb 3 18:10–22:00 | preceding of 50003 |
| VISIT_OCCURRENCE | 50003 | `<inpatient>` | Feb 3 22:00–Feb 8 14:00 | contains detail 51001 |
| VISIT_DETAIL | 51001 | `<intensive care>` | Feb 4 01:00–Feb 5 09:00 | parent visit 50003 |

## Crosswalks and Vocabulary

E1002→50002, E1003→50003, E1004→51001. Class mappings are versioned semantic placeholders pending the release snapshot.

## Validation Results

- Two top-level sources = two visits; one child = one detail.
- All rows belong to person 2; detail boundaries fit the parent.
- No source interval is broadened, duplicated, or lost.

## Related Pages

[VISIT_OCCURRENCE](../03-omop-domains/visit-occurrence.md) · [VISIT_DETAIL](../03-omop-domains/visit-detail.md) · [visit reconstruction](../03-Challenges/visit-reconstruction.md) · [[timestamp-selection]]

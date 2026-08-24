---
title: "Worked Example: Medication Administration"
type: example
status: stable
omop_domains: [DRUG_EXPOSURE, VISIT_OCCURRENCE]
source_entities: [medication_order, medication_administration]
tags: [omop, example, medication]
---

# Worked Example: Medication Administration

## Clinical Story

During P002's inpatient visit, a ceftriaxone order produces one given dose and one held dose before the order is discontinued.

## Source Records

| record | kind | time | dose | status |
|---|---|---|---|---|
| SRC-O2002 | order O2002 | Feb 4 02:00 | 1 g | discontinued |
| SRC-A3001 | administration A3001 | Feb 4 02:15 | 1 g | given |
| SRC-A3002 | administration A3002 | Feb 4 10:15 | 1 g | held |

## Transformation Sequence

1. Map local medication/formulation using the order catalogue.
2. Join administrations to O2002 and verify patient/visit consistency.
3. Create exposure 92002 from A3001 only.
4. Retain O2002 as context and A3002 as a negative-outcome exclusion in lineage.

## Generated OMOP Records

| table | id | evidence | time | quantity | visit |
|---|---:|---|---|---:|---:|
| DRUG_EXPOSURE | 92002 | A3001 given | 2026-02-04 02:15+02 | 1 (unit/formulation mapped) | 50003 |

## Crosswalks and Vocabulary

A3001→92002. A3002 has no target exposure and an explicit `HELD_NOT_EXPOSURE` action. The exact ceftriaxone and type/unit concepts await the pinned snapshot.

## Validation Results

| Assertion | Result |
|---|---|
| given administrations produce exposure | 1/1 pass |
| held administrations produce exposure | 0/1 pass |
| inpatient order adds separate exposure | 0 pass |
| exposure time lies in visit 50003 | pass |

## Related Pages

[DRUG_EXPOSURE](../03-omop-domains/drug-exposure.md) · [order vs administration](../03-Challenges/medication-order-vs-administration.md) · [duplicate events](../03-Challenges/duplicate-events.md) · [[source-provenance]]

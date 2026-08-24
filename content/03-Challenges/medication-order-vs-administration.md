---
title: "Medication Order vs Administration"
type: challenge
status: stable
omop_domains: [DRUG_EXPOSURE]
source_entities: [medication_order, medication_administration]
engineering_stages: [normalization, omop-transformation]
tags: [omop, ehr, medication]
---

# Medication Order vs Administration

## Problem

An order is intent; an administration outcome is evidence about whether a dose was given. Treating both identically inflates exposure.

## Source Semantics

O2002 schedules ceftriaxone. A3001 was given and A3002 was held.

## Minimal Raw EHR Example

| record | kind | order_id | time | status |
|---|---|---|---|---|
| SRC-O2002 | inpatient order | O2002 | 02:00 | discontinued |
| SRC-A3001 | administration | O2002 | 02:15 | given |
| SRC-A3002 | administration | O2002 | 10:15 | held |

## Incorrect Transformation

Create exposure from the order and both scheduled administrations, yielding three exposures for one given dose.

## Adopted Rule

For inpatient therapy, given administrations create dose-level DRUG_EXPOSURE. Held/refused/cancelled outcomes create no exposure. The order supplies linkage/formulation context but does not add a second row. Outpatient prescriptions use an explicitly different order-based rule.

## Target OMOP Representation

| drug_exposure_id | evidence | start | quantity | excluded evidence |
|---:|---|---|---:|---|
| 92002 | SRC-A3001 given | 2026-02-04 02:15+02 | 1 | SRC-O2002 as separate exposure; SRC-A3002 held |

## Mapping Decisions

Type concept identifies administration versus prescription. Exclusion lineage retains negative outcomes, status, order link, and rule version.

## Validation

- Given administration count equals administration-based exposures plus documented exclusions for mapping/required-field failures.
- Zero held/refused/cancelled rows become exposures; order/admin pairs do not double-count.

## Failure Modes

- Semantic inflation; held dose appears given; order and administration duplicate; formulation from order is joined to wrong administration.

## Related Pages

[Orders](../01-source-ehr/medications.md) · [Administrations](../01-source-ehr/medication-administrations.md) · [DRUG_EXPOSURE](../03-omop-domains/drug-exposure.md) · [[example-medication-administration]]

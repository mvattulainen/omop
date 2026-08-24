---
title: "Source vs OMOP Reconciliation"
type: quality
status: stable
tags: [omop, data-quality, reconciliation]
---

# Source vs OMOP Reconciliation

Reconciliation uses semantic eligibility, not raw table count alone:

```text
eligible source assertions = target facts + explicit exclusions/quarantine
```

## Canonical release reconciliation

| Flow | Eligible source | OMOP target | Explicit exclusion | Expected status |
|---|---:|---:|---:|---|
| winning patients → PERSON | 2 | 2 | 0 | pass |
| top-level encounters → VISIT_OCCURRENCE | 3 | 3 | 0 | pass |
| encounter segments → VISIT_DETAIL | 1 | 1 | 0 | pass |
| confirmed encounter diagnoses → CONDITION_OCCURRENCE | 1 | 1 | 0 | pass |
| final lab results → MEASUREMENT | 1 | 1 | 0 | pass |
| eligible outpatient orders → DRUG_EXPOSURE | 1 | 1 | 0 | pass |
| given inpatient administrations → DRUG_EXPOSURE | 1 | 1 | 0 | pass |
| held inpatient administrations → DRUG_EXPOSURE | 0 | 0 | 1 retained negative outcome | pass |

## Distribution checks

Compare source versus OMOP by patient, source system, event month, encounter class, diagnosis code group, medication evidence type, lab test/unit, mapping status, and rule version. Count equality alone cannot detect domain/timestamp/value distortion.

## Identifier and lineage checks

Every source group contributing to a target is traceable; every exclusion has a reason. Incremental delta plus previous release reconciles to the candidate, and affected-partition replay equals rebuild.

## Failure handling

Unexplained difference is an error. Tolerances are allowed only for a named, measured, approved semantic reason and expire at a specified release.

Related: [source fixture](../05-Meta/assets/data/synthetic-ehr.csv) · [outpatient example](../05-examples/example-outpatient-diabetes.md) · [medication example](../05-examples/example-medication-administration.md) · [[duplicate-events]]

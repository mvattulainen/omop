---
title: "Plausibility Checks"
type: quality
status: stable
tags: [omop, data-quality, plausibility]
---

# Plausibility Checks

| Check ID | Assertion | Behavior |
|---|---|---|
| PLA-001 | Event/death dates are not before birth | error |
| PLA-002 | Visit/detail/exposure end is not before start | critical |
| PLA-003 | Visit detail falls inside its visit and shares person | critical |
| PLA-004 | Linked event occurs within an approved encounter tolerance | warning/error by entity |
| PLA-005 | Measurement value is within broad physically possible range | warning; retain value |
| PLA-006 | Result is not implausibly before collection | warning/error |
| PLA-007 | Given administration is not represented from held/refused status | critical |

## Example timeline

For P002: ED ends exactly when inpatient starts; ICU begins/ends inside inpatient; the given dose at 02:15 on Feb 4 lies inside inpatient; the held dose creates no exposure.

## Clinical review

Statistical alerts identify records for review; they do not silently truncate, delete, or replace clinical values. Thresholds are broad, versioned, analyte/context-aware, and reported with numerator/denominator.

## Failure handling

Deterministic impossibilities block release. Context-sensitive outliers require review and documented disposition.

Related: [[08-clinical-validation]] · [[timestamp-selection]] · [[lab-unit-normalization]]

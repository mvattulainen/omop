---
title: "Source Laboratory Results"
type: source
status: stable
source_entities: [laboratory_result, specimen]
omop_domains: [MEASUREMENT, SPECIMEN]
tags: [omop, ehr, laboratory]
---

# Source Laboratory Results

A laboratory result identifies the test, specimen/collection context, result, unit, reference interval, status, and audit times. Collection time is the preferred clinical time; result time is provenance.

| source_record_id | patient_id | encounter_id | lab_code | code_system | value_num | unit | collected_at | resulted_at | status |
|---|---|---|---|---|---:|---|---|---|---|
| SRC-L6001 | P001 | E1001 | HBA1C-LOCAL | LOCAL-LAB | 7.4 | % | 2026-01-14 09:50+02:00 | 2026-01-14 12:30+02:00 | final |

Corrected final results supersede preliminary/current versions sharing the source result key. The original unit/value remain available even when a normalized value is calculated.

Quality checks: exactly one typed result representation is selected; unit is compatible with test; result/collection chronology is plausible.

Downstream: [MEASUREMENT](../03-omop-domains/measurement.md) · [unit normalization](../03-Challenges/lab-unit-normalization.md) · [[timestamp-selection]]

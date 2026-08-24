---
title: "Source Procedures"
type: source
status: stable
source_entities: [procedure]
omop_domains: [PROCEDURE_OCCURRENCE, MEASUREMENT, DEVICE_EXPOSURE]
tags: [omop, ehr, procedure]
---

# Source Procedures

A procedure row records a performed, cancelled, or planned activity. Only performed events are eligible for occurrence tables. Target domain follows the mapped standard concept, not the source table name.

| source_record_id | patient_id | encounter_id | procedure_code | code_system | performed_at | status |
|---|---|---|---|---|---|---|
| SRC-R7001 | P002 | E1003 | ECG12 | LOCAL-PROC | 2026-02-03 22:30+02:00 | performed |

Quality checks: status is from the controlled list; performed rows have a clinical timestamp; mapping yields a supported target domain or an explicit quarantine reason.

Downstream: [PROCEDURE_OCCURRENCE](../03-omop-domains/procedure-occurrence.md) · [domain assignment](../03-Challenges/domain-assignment.md) · [[06-omop-transformation]]

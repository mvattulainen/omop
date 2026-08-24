---
title: "Source Encounters"
type: source
status: stable
source_entities: [encounter, encounter_segment]
omop_domains: [VISIT_OCCURRENCE, VISIT_DETAIL]
tags: [omop, ehr, encounter]
---

# Source Encounters

An encounter is an administrative contact. A segment is a location/care-setting interval nested within a parent encounter. ED and inpatient rows may represent adjacent phases of one clinical episode but remain distinct source facts.

| source_record_id | encounter_id | patient_id | parent_encounter_id | class | start_at | end_at |
|---|---|---|---|---|---|---|
| SRC-E1001 | E1001 | P001 | NULL | AMB | 2026-01-14 09:30+02:00 | 2026-01-14 10:20+02:00 |
| SRC-E1002 | E1002 | P002 | NULL | ED | 2026-02-03 18:10+02:00 | 2026-02-03 22:00+02:00 |
| SRC-E1003 | E1003 | P002 | NULL | IP | 2026-02-03 22:00+02:00 | 2026-02-08 14:00+02:00 |
| SRC-E1004 | E1004 | P002 | E1003 | ICU | 2026-02-04 01:00+02:00 | 2026-02-05 09:00+02:00 |

Quality checks: end is not before start; segments fit within parents; patient is consistent through a hierarchy.

Downstream: [VISIT_OCCURRENCE](../03-omop-domains/visit-occurrence.md) · [VISIT_DETAIL](../03-omop-domains/visit-detail.md) · [[visit-reconstruction]]

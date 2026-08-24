---
title: "Source Diagnoses"
type: source
status: stable
source_entities: [diagnosis, problem_list]
omop_domains: [CONDITION_OCCURRENCE]
tags: [omop, ehr, diagnosis]
---

# Source Diagnoses

`diagnosis` records encounter assertions; `problem_list` records longitudinal clinician-maintained problems. Billing rows are evidence of coding, not automatically separate clinical conditions.

| source_record_id | patient_id | encounter_id | dx_code | code_system | dx_type | clinical_at | status |
|---|---|---|---|---|---|---|---|
| SRC-D5001 | P001 | E1001 | E11.9 | ICD10CM | encounter | 2026-01-14 09:42+02:00 | confirmed |

Corrections supersede the same source record; they are not extra diagnoses. Source type determines condition type and deduplication policy.

Quality checks: declared code system exists; clinical time falls plausibly near the linked encounter; deleted/cancelled rows do not remain active.

Downstream: [CONDITION_OCCURRENCE](../03-omop-domains/condition-occurrence.md) · [duplicate events](../03-Challenges/duplicate-events.md) · [[05-vocabulary-mapping]]

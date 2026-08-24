---
title: "Source Observations"
type: source
status: stable
source_entities: [observation]
omop_domains: [OBSERVATION, MEASUREMENT]
tags: [omop, ehr, observation]
---

# Source Observations

Source observations are structured assertions such as smoking status or housing situation. Numeric tests and vital signs are not routed here merely because the source table is named observation.

| source_record_id | patient_id | encounter_id | observation_code | code_system | value_code | observed_at | status |
|---|---|---|---|---|---|---|---|
| SRC-B8001 | P001 | E1001 | SMOKE | LOCAL-OBS | NEVER | 2026-01-14 09:38+02:00 | final |

Quality checks: code/value pair is valid for the local catalogue; mapped standard concept determines domain; unsupported values remain traceable.

Downstream: [OBSERVATION](../03-omop-domains/observation.md) · [domain assignment](../03-Challenges/domain-assignment.md) · [[local-code-mapping]]

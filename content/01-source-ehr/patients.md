---
title: "Source Patients"
type: source
status: stable
source_entities: [patient, patient_alias, patient_merge]
omop_domains: [PERSON]
tags: [omop, ehr, patient]
---

# Source Patients

One `patient` row is an EHR registration identity, not necessarily a unique biological person. `patient_alias` supplies identifiers; `patient_merge` declares that a losing registration is superseded.

| source_record_id | patient_id | birth_date | administrative_sex | status |
|---|---|---|---|---|
| SRC-P001 | P001 | 1980-04-12 | F | active |
| SRC-P002 | P002 | 1972-09-03 | M | active |

Null birth parts are preserved as unknown. The pipeline never fabricates day or month. Administrative sex is mapped to the release-approved OMOP representation and the source value is preserved.

Quality checks: patient business key is unique in the current view; birth date is not in the future; every event resolves through the persistent identity crosswalk.

Downstream: [PERSON](../03-omop-domains/person.md) · [identity resolution](../03-Challenges/patient-identity-resolution.md) · [[01-source-profiling]]

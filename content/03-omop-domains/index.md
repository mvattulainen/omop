---
title: "OMOP Domain Index"
type: meta
status: stable
tags: [omop, domains, navigation]
---

# OMOP Domain Index

| Domain page | Primary source | Canonical scenario |
|---|---|---|
| [PERSON](person.md) | patient | all |
| [OBSERVATION_PERIOD](observation-period.md) | eligible event coverage | all |
| [VISIT_OCCURRENCE](visit-occurrence.md) | encounter | A, B |
| [VISIT_DETAIL](visit-detail.md) | encounter segment | B |
| [CONDITION_OCCURRENCE](condition-occurrence.md) | diagnosis/problem | A |
| [PROCEDURE_OCCURRENCE](procedure-occurrence.md) | performed procedure | extension fixture |
| [DRUG_EXPOSURE](drug-exposure.md) | order/administration | A, C |
| [MEASUREMENT](measurement.md) | laboratory/vital | A, D |
| [OBSERVATION](observation.md) | structured assertion | extension fixture |
| [DEVICE_EXPOSURE](device-exposure.md) | device event | extension fixture |
| [DEATH](death.md) | verified death facts | extension fixture |
| [NOTE](note.md) | clinical document | extension fixture |
| [SPECIMEN](specimen.md) | specimen | extension fixture |

All target examples use semantic concept placeholders until the [vocabulary policy](../05-Meta/omop-version-policy.md) pins and verifies identifiers.

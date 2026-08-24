---
title: "Source Medication Administrations"
type: source
status: stable
source_entities: [medication_administration]
omop_domains: [DRUG_EXPOSURE]
tags: [omop, ehr, medication-administration]
---

# Source Medication Administrations

Administration rows record dose-level outcomes. `given` is evidence of exposure; `held`, `refused`, and `cancelled` are clinically useful negative facts but not drug exposures.

| source_record_id | administration_id | order_id | patient_id | encounter_id | medication_code | administered_at | dose | unit | status |
|---|---|---|---|---|---|---|---:|---|---|
| SRC-A3001 | A3001 | O2002 | P002 | E1003 | CEF1G | 2026-02-04 02:15+02:00 | 1 | g | given |
| SRC-A3002 | A3002 | O2002 | P002 | E1003 | CEF1G | 2026-02-04 10:15+02:00 | 1 | g | held |

Quality checks: given doses have time and amount; order/patient associations agree; duplicate message identifiers collapse to one administration.

Downstream: [DRUG_EXPOSURE](../03-omop-domains/drug-exposure.md) · [[medication-order-vs-administration]] · [[duplicate-events]]

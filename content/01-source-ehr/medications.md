---
title: "Source Medication Orders"
type: source
status: stable
source_entities: [medication_order]
omop_domains: [DRUG_EXPOSURE]
tags: [omop, ehr, medication]
---

# Source Medication Orders

An order represents prescribing intent. Outpatient active orders can support exposure intervals; inpatient orders alone do not prove administration.

| source_record_id | order_id | patient_id | encounter_id | medication_code | dose | unit | ordered_at | start_at | end_at | status |
|---|---|---|---|---|---:|---|---|---|---|---|
| SRC-O2001 | O2001 | P001 | E1001 | MET500 | 500 | mg | 2026-01-14 10:05+02:00 | 2026-01-14 | 2026-04-13 | active |
| SRC-O2002 | O2002 | P002 | E1003 | CEF1G | 1 | g | 2026-02-04 02:00+02:00 | 2026-02-04 | NULL | discontinued |

Quality checks: dose unit is declared; end is not before start; local drug code resolves or is counted as unmapped.

Downstream: [DRUG_EXPOSURE](../03-omop-domains/drug-exposure.md) · [order versus administration](../03-Challenges/medication-order-vs-administration.md) · [[example-medication-administration]]

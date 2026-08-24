---
title: "OMOP Domain Assignment"
type: challenge
status: stable
omop_domains: [MEASUREMENT, OBSERVATION, PROCEDURE_OCCURRENCE, DEVICE_EXPOSURE]
source_entities: [procedure, laboratory_result, observation]
engineering_stages: [vocabulary-mapping, omop-transformation]
tags: [omop, vocabulary, domain]
---

# OMOP Domain Assignment

## Problem

Source table names do not reliably identify the OMOP target domain.

## Source Semantics

A source `observation` may be a numeric test, while a source `procedure` code may map to Measurement or Device.

## Minimal Raw EHR Example

| source table | code | meaning | mapped standard domain |
|---|---|---|---|
| observation | WEIGHT | body weight measurement | Measurement |
| procedure | ECG12 | electrocardiographic procedure | Procedure |

## Incorrect Transformation

Send every observation row to OBSERVATION and every procedure row to PROCEDURE_OCCURRENCE.

## Adopted Rule

Resolve source and standard concept first, then route by the valid standard concept's OMOP domain. Unsupported/ambiguous multi-maps quarantine; they are not coerced.

## Target OMOP Representation

| source code | standard concept | target table |
|---|---|---|
| WEIGHT | `<standard body weight measurement>` | MEASUREMENT |
| ECG12 | `<standard 12-lead ECG procedure>` | PROCEDURE_OCCURRENCE |

## Mapping Decisions

The mapping artifact records the chosen domain and snapshot. A vocabulary upgrade may reroute rows and therefore requires an impact/reconciliation report.

## Validation

- Standard concept domain matches target table.
- Eligible source events reconcile across all routed tables plus quarantine; no event is loaded twice due to routing.

## Failure Modes

- Source-table forcing misclassifies events; an event is loaded into two domains; a vocabulary domain change is ignored; unmapped events are dumped into OBSERVATION.

## Related Pages

[Vocabulary mapping](../02-engineering/05-vocabulary-mapping.md) · [PROCEDURE_OCCURRENCE](../03-omop-domains/procedure-occurrence.md) · [OBSERVATION](../03-omop-domains/observation.md) · [[unmapped-concepts]]

---
title: "Source Code Systems"
type: source
status: stable
source_entities: [code_catalogue, mapping_table]
tags: [omop, ehr, vocabulary]
---

# Source Code Systems

| Source code system | Use | Resolution policy |
|---|---|---|
| ICD10CM | Diagnoses | Resolve source concept, then Maps to standard concept |
| LOCAL-LAB | Laboratory catalogue | Versioned curated mapping to standard measurement concept |
| LOCAL-DRUG | Medication catalogue | Resolve ingredient/clinical drug using formulation metadata |
| LOCAL-PROC | Performed procedures | Curated mapping; target domain follows standard concept |
| EHR-ENCOUNTER | Encounter class | Curated mapping to standard visit concept |
| LOCAL-OBS | Structured observations | Curated question and answer mappings |

A code is keyed by `(code_system, code, valid_start, valid_end)`. Display text alone is never a mapping key. Every mapping stores reviewer, evidence, mapping version, source/standard concept, relationship, target domain, validity dates, and status.

Unresolved codes retain their source value and use `concept_id = 0` only where the CDM permits; they also enter the mapping backlog and release metrics.

Related: [vocabulary mapping](../02-engineering/05-vocabulary-mapping.md) · [local codes](../03-Challenges/local-code-mapping.md) · [[unmapped-concepts]]

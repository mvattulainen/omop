---
title: "Vocabulary Mapping"
type: engineering
status: stable
engineering_stages: [vocabulary-mapping]
tags: [omop, vocabulary, mapping]
---

# Vocabulary Mapping

## Responsibility

Resolve `source value → source concept → standard concept → target domain` against one declared vocabulary snapshot.

## Inputs

- Canonical events, source catalogues, OMOP vocabularies, curated local mappings

## Outputs and artifacts

- Versioned source/standard concept decisions, domain route, mapping evidence, and unmapped backlog

## Minimal example

| source system | source value | source concept | standard concept | domain |
|---|---|---|---|---|
| ICD10CM | E11.9 | `<source ICD-10-CM E11.9>` | `<standard SNOMED type 2 diabetes>` | Condition |
| LOCAL-LAB | HBA1C-LOCAL | `<local source concept or 0>` | `<standard HbA1c measurement>` | Measurement |

## Execution contract

Standard mappings use valid `Maps to` relationships at the event date. Local mappings require reviewer/evidence. Multi-maps follow documented concept-specific rules or quarantine. Target table follows standard concept domain. Unmapped rows preserve source value and source concept and use zero only as CDM-compatible fallback.

## Incremental behavior and errors

Mapping/vocabulary upgrades re-evaluate affected source codes and can reroute domains; impacts are versioned and reconciled. Invalid, expired, or domain-incompatible mappings block affected rows.

## Quality gates

Coverage and `concept_id=0` rates meet approved per-entity thresholds; standard concepts are standard and valid; domains match target tables; every decision records snapshot and mapping version.

Related: [[local-code-mapping]] · [[domain-assignment]] · [[unmapped-concepts]]

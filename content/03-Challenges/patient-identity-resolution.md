---
title: "Patient Identity Resolution"
type: challenge
status: stable
omop_domains: [PERSON]
source_entities: [patient, patient_alias, patient_merge]
engineering_stages: [normalization, incremental-loads]
tags: [omop, ehr, identity]
---

# Patient Identity Resolution

## Problem

Registrations can merge, but names/demographics are not safe proof that two registrations are one person.

## Source Semantics

Only an explicit `patient_merge(loser, winner, effective_at)` authorizes consolidation. Aliases identify registrations, not independent persons.

## Minimal Raw EHR Example

| record | losing_patient_id | winning_patient_id | effective_at |
|---|---|---|---|
| SRC-M001 | P001-OLD | P001 | 2026-01-10 08:00+02 |

## Incorrect Transformation

Create two PERSON rows forever, or merge registrations because birth date/sex match.

## Adopted Rule

Follow explicit merge chains to a terminal winner, reject cycles, and use a persistent winner-to-person crosswalk. Re-parent derived facts on the next load while retaining every alias and merge edge.

## Target OMOP Representation

| person_id | person_source_value | lineage aliases |
|---:|---|---|
| 1 | P001 | P001, P001-OLD |

## Mapping Decisions

Demographics never create identity. `person_id=1` remains stable after the merge. The winner supplies `person_source_value`; losing IDs remain in auditable lineage.

## Validation

- Merge graph is acyclic and each registration has one terminal winner.
- All facts resolve to the winner; no duplicate PERSON remains for a merged identity.

## Failure Modes

- False positive demographic merge; merge cycle; facts split between winner/loser; person ID changes every release; alias evidence disappears.

## Related Pages

[Patients](../01-source-ehr/patients.md) · [PERSON](../03-omop-domains/person.md) · [incremental loads](../02-engineering/09-incremental-loads.md) · [[source-provenance]]

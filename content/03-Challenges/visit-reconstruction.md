---
title: "Visit Reconstruction"
type: challenge
status: stable
omop_domains: [VISIT_OCCURRENCE, VISIT_DETAIL]
source_entities: [encounter, encounter_segment]
engineering_stages: [normalization, omop-transformation]
tags: [omop, ehr, visit]
---

# Visit Reconstruction

## Problem

Adjacent ED/inpatient contacts and nested ICU segments make both over-aggregation and duplication tempting.

## Source Semantics

E1002 (ED) and E1003 (inpatient) are distinct top-level encounters; E1004 (ICU) is a child of E1003.

## Minimal Raw EHR Example

| encounter | parent | class | start | end |
|---|---|---|---|---|
| E1002 | NULL | ED | Feb 3 18:10 | Feb 3 22:00 |
| E1003 | NULL | IP | Feb 3 22:00 | Feb 8 14:00 |
| E1004 | E1003 | ICU | Feb 4 01:00 | Feb 5 09:00 |

## Incorrect Transformation

Collapse all three into one inpatient visit, or create three VISIT_OCCURRENCE rows.

## Adopted Rule

Create two visits for top-level contacts, link ED as preceding inpatient, and create one VISIT_DETAIL for ICU. Never infer grouping solely from time proximity when source hierarchy/class is available.

## Target OMOP Representation

| table | id | concept | parent/preceding |
|---|---:|---|---:|
| VISIT_OCCURRENCE | 50002 | `<emergency>` | NULL |
| VISIT_OCCURRENCE | 50003 | `<inpatient>` | preceding 50002 |
| VISIT_DETAIL | 51001 | `<intensive care>` | visit 50003 |

## Mapping Decisions

Source encounter class maps to standard visit concepts; exact IDs await the pinned snapshot. Encounter crosswalk preserves each source-to-target relationship.

## Validation

- Two top-level encounters reconcile to two visits and one child to one detail.
- Detail lies inside parent and all rows share person 2.

## Failure Modes

- Invalid aggregation; detail becomes visit; transfer chain crosses patients; open-ended record gets a fabricated discharge; event links to the wrong phase.

## Related Pages

[Encounters](../01-source-ehr/encounters.md) · [VISIT_OCCURRENCE](../03-omop-domains/visit-occurrence.md) · [VISIT_DETAIL](../03-omop-domains/visit-detail.md) · [[example-emergency-to-inpatient]]

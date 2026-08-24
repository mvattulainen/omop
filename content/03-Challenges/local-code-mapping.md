---
title: "Local Code Mapping"
type: challenge
status: stable
omop_domains: [MEASUREMENT, DRUG_EXPOSURE, OBSERVATION]
source_entities: [code_catalogue, mapping_table]
engineering_stages: [vocabulary-mapping]
tags: [omop, vocabulary, local-codes]
---

# Local Code Mapping

## Problem

Local strings have institution- and version-specific meaning that display-name matching cannot safely recover.

## Source Semantics

`HBA1C-LOCAL` means HbA1c only within the declared LOCAL-LAB catalogue version and includes result/unit constraints.

## Minimal Raw EHR Example

| code_system | code | display | valid_start | value_type | allowed_unit |
|---|---|---|---|---|---|
| LOCAL-LAB | HBA1C-LOCAL | Glycated Hb | 2025-01-01 | numeric | % |

## Incorrect Transformation

Fuzzy-match the display name on every run and accept the top candidate without evidence or versioning.

## Adopted Rule

Curate a versioned mapping keyed by system/code/validity. Store reviewer, evidence, source/standard concept, relationship, domain, constraints, and approval. Unapproved rows remain unmapped.

## Target OMOP Representation

| source value | source concept | standard concept | domain | mapping version |
|---|---|---|---|---|
| HBA1C-LOCAL | `<local source concept or 0>` | `<standard HbA1c measurement>` | Measurement | 0.1.0-draft |

## Mapping Decisions

Unit/value-type compatibility is evidence, not a substitute for the code catalogue. Original value always populates a source-value field and lineage.

## Validation

- Mapping is valid at event date and target concept is standard/in expected domain.
- Coverage and newly seen codes are reported per release; reviewer/evidence fields are non-null for approved local mappings.

## Failure Modes

- Text similarity selects wrong analyte; code reuse across versions is ignored; target domain is forced; mapping changes without audit.

## Related Pages

[Code systems](../01-source-ehr/code-systems.md) · [vocabulary mapping](../02-engineering/05-vocabulary-mapping.md) · [MEASUREMENT](../03-omop-domains/measurement.md) · [[unmapped-concepts]]

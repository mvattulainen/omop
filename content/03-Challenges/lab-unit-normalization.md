---
title: "Laboratory Unit Normalization"
type: challenge
status: stable
omop_domains: [MEASUREMENT]
source_entities: [laboratory_result]
engineering_stages: [normalization, vocabulary-mapping]
tags: [omop, ehr, laboratory, units]
---

# Laboratory Unit Normalization

## Problem

The same analyte may use compatible units with different scales—or units that look similar but represent a different quantity.

## Source Semantics

The source value/unit pair is the reported clinical fact. Conversion creates an analytical derivative and must never overwrite its provenance.

## Minimal Raw EHR Example

| result | test | value | unit |
|---|---|---:|---|
| SRC-L6001 | HBA1C-LOCAL | 7.4 | % |

## Incorrect Transformation

Replace `%` with `mmol/mol` but leave `7.4` unchanged, or convert using an unverified generic scale factor.

## Adopted Rule

Map the source unit, verify dimensional/test compatibility, and apply only a versioned analyte-specific formula when a canonical target is required. Retain original value/unit and formula/version. Otherwise keep the original compatible representation.

## Target OMOP Representation

| measurement_id | value_as_number | unit_concept_id | unit_source_value | lineage calculation |
|---:|---:|---|---|---|
| 93001 | 7.4 | `<standard percent unit>` | % | none |

## Mapping Decisions

This vault keeps HbA1c 7.4% unchanged. A later release may add a verified IFCC conversion, but that is a rule-version change. Unmapped units use the permitted zero fallback while preserving `unit_source_value`.

## Validation

- Unit concept is compatible with the measurement concept.
- Round-trip/conversion test passes within declared tolerance; original and normalized distributions are compared.

## Failure Modes

- Magnitude changes without value conversion; incompatible dimensions convert; original unit disappears; rounding silently changes clinical category.

## Related Pages

[Laboratory results](../01-source-ehr/laboratory-results.md) · [MEASUREMENT](../03-omop-domains/measurement.md) · [local codes](local-code-mapping.md) · [[plausibility-checks]]

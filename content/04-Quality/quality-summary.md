---
title: "Data Quality Summary"
type: quality
status: stable
engineering_stages:
  - data-quality
  - clinical-validation
  - release
tags:
  - omop
  - data-quality
  - summary
---

# Data Quality Summary

The quality model progresses from the governing framework through structural, conformance and plausibility checks to semantic source-to-OMOP reconciliation.

## 1. Quality framework

Every candidate OMOP release is evaluated at four complementary levels—structural integrity, CDM and vocabulary conformance, clinical plausibility and source reconciliation—because passing any one level does not prove overall correctness. Each versioned check records its scope, numerator, denominator, threshold, severity, result, sample locator, run and release; critical findings cannot be waived, while error exceptions require an owner, rationale, expiry and disclosed impact.

## 2. Structural checks

Structural validation verifies that primary identifiers are populated and unique, required fields are present, clinical `person_id` values resolve to `PERSON`, optional foreign keys resolve when populated, crosswalk targets exist and every target row has source lineage. Critical failures quarantine the candidate and must be corrected upstream rather than patched by the validation process.

## 3. Conformance checks

Conformance validation verifies that standard concept fields contain valid standard concepts or a permitted zero, concepts belong to the expected target domain, vocabulary relationships are valid at the event date, measurement units are compatible and type concepts match the evidence source. Vocabulary upgrades compare validity, routing, mapping coverage, multi-map behavior and zero rates, while invalid mappings return to quarantine or the mapping backlog instead of being guessed.

## 4. Plausibility checks

Plausibility validation tests whether the transformed clinical and temporal story is credible, including events occurring after birth, ends not preceding starts, visit details remaining inside their parent visits, linked events occurring near the appropriate encounter, measurements remaining physically plausible and held medication doses not becoming exposures. Deterministic impossibilities block release, whereas context-dependent outliers are retained for clinical review with a documented disposition.

## 5. Source-to-OMOP reconciliation

Reconciliation demonstrates that eligible source meaning survives according to the equation `eligible source assertions = target facts + explicit exclusions or quarantine`. Counts and distributions are compared by patient, source system, time, encounter class, diagnosis group, medication evidence, laboratory test and unit, mapping status and rule version, while every unexplained difference is an error and every accepted tolerance must have a named, measured and expiring semantic justification.

## Related pages

- [Detailed quality framework](quality-framework.md)
- [Engineering pipeline summary](../02-engineering/engineering-pipeline-summary.md)
- [Transformation challenges summary](../03-Challenges/challenge-summary.md)

Obsidian: [[quality-framework]] · [[engineering-pipeline-summary]] · [[challenge-summary]]

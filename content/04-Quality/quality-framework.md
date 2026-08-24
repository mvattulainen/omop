---
title: "Quality Framework"
type: quality
status: stable
engineering_stages: [data-quality, clinical-validation, release]
tags: [omop, data-quality]
---

# Quality Framework

Quality is evaluated at four complementary levels. Passing conformance does not prove clinical plausibility, and matching row counts does not prove correct meaning.

| Level | Question | Example gate |
|---|---|---|
| [Structural](structural-checks.md) | Is the database internally complete and connected? | required foreign keys resolve |
| [Conformance](conformance-checks.md) | Does content obey CDM/vocabulary rules? | standard concept has target domain |
| [Plausibility](plausibility-checks.md) | Is the clinical/temporal story credible? | visit end ≥ start |
| [Reconciliation](source-vs-omop-reconciliation.md) | Did eligible source meaning survive? | given administrations = exposure + explicit exclusions |

## Finding contract

Every check records `check_id`, version, level, scope, numerator, denominator, threshold, severity, result, sample locator, run, and release. Critical findings cannot be waived. Error exceptions require owner, rationale, expiry, and disclosed release impact.

## Execution

Incremental loads run affected-row and invariant checks. Candidate releases run the complete suite against an immutable snapshot. Results never modify data. Baselines are versioned so threshold changes are reviewable.

## Canonical scenario gate

- P001 produces one outpatient visit, condition, HbA1c measurement, and outpatient prescription exposure.
- P002 produces two visits, one ICU detail, one given-dose exposure, and zero held-dose exposures.

Related: [data quality execution](../02-engineering/07-data-quality.md) · [clinical validation](../02-engineering/08-clinical-validation.md) · [[10-release-process]]

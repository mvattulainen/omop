---
title: "Clinical Validation"
type: engineering
status: stable
engineering_stages: [clinical-validation]
tags: [omop, clinical-validation]
---

# Clinical Validation

## Responsibility

Verify that technically conformant OMOP data still represents the intended clinical story.

## Inputs

- Quality-passing snapshot, scenario traces, distributions, mapping change report

## Outputs and artifacts

- Versioned review record, approved exceptions, mapping feedback, and sign-off

## Review method

Reviewers compare source and OMOP timelines for stable synthetic scenarios, then inspect stratified aggregates for visit types, diagnoses, measurements/units, and medications. They review semantics—not real patient records.

| Scenario assertion | Expected result |
|---|---|
| P001 outpatient diabetes | one visit, diagnosis, HbA1c, outpatient metformin order exposure |
| P002 inpatient medication | one given exposure; held dose absent from DRUG_EXPOSURE but present in lineage |

The review is repeatable for identical artifacts. New mappings, rule changes, or distribution alerts trigger focused re-review. Clinical rejection returns the candidate release to the responsible engineering stage.

Related: [[example-outpatient-diabetes]] · [[example-medication-administration]] · [[plausibility-checks]]

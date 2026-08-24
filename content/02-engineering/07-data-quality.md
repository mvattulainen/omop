---
title: "Data Quality Execution"
type: engineering
status: stable
engineering_stages: [data-quality]
tags: [omop, data-quality, testing]
---

# Data Quality Execution

## Responsibility

Run deterministic structural, conformance, plausibility, and reconciliation checks and turn findings into release decisions.

## Inputs

- Candidate OMOP snapshot, source/canonical counts, manifests, thresholds

## Outputs and artifacts

- Check-level results, offending-row samples, summary metrics, exceptions, and release-gate status

## Quality gates

| Severity | Behavior |
|---|---|
| Critical | blocks release; no waiver |
| Error | blocks unless time-bounded owner-approved exception exists |
| Warning | reviewed and disclosed |
| Information | trend only |

Checks are versioned and idempotent. Incremental runs execute affected-row checks plus global invariants; releases always execute the full suite. A check failure never mutates data.

## Required checks

- No unresolved required foreign keys
- End dates/times are not before starts
- Standard concept/domain conformance
- Source eligibility equals target plus documented exclusions
- Canonical scenario assertions pass

Related: [quality framework](../04-Quality/quality-framework.md) · [[structural-checks]] · [[source-vs-omop-reconciliation]]

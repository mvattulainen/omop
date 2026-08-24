---
title: "OMOP Release Process"
type: engineering
status: stable
engineering_stages: [release]
tags: [omop, release, reproducibility]
---

# OMOP Release Process

## Responsibility

Promote a quality-passing candidate into an immutable, reproducible OMOP release.

## Inputs

- Candidate database, ETL build, manifests, mappings, quality/clinical approvals

## Release package

| Required item | Example |
|---|---|
| OMOP snapshot | `omop-0.1.0-draft` |
| ETL/rule version | `0.1.0-draft` |
| Source cutoff/version | declared timestamp / `1.0.0` |
| CDM/vocabulary | `5.4` / pinned snapshot required |
| Mapping version | `0.1.0-draft` |
| Reports | quality, reconciliation, clinical validation |
| Governance | known issues, release notes, checksums, approvers |

## Execution contract

The release manifest names every input and artifact checksum. Packaging is idempotent: the same manifest yields the same content. Promotion is atomic; failed packaging leaves the prior release intact. Artifacts and raw dependencies are retained according to policy.

## Quality gates

All critical/error checks pass or have permitted exceptions; clinical review is signed; vocabulary is pinned; lineage samples round-trip; disaster-restore test succeeds. `UNPINNED-DOCUMENTATION` can never be promoted as production.

Related: [version policy](../05-Meta/omop-version-policy.md) · [release history](../05-Meta/release-history.md) · [[quality-framework]]

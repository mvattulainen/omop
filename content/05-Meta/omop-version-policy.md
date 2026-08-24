---
title: "OMOP Version Policy"
type: meta
status: stable
tags: [omop, vocabulary, release]
---

# OMOP Version Policy

| Component | Declared value |
|---|---|
| OMOP CDM | 5.4 |
| Vocabulary snapshot | `UNPINNED-DOCUMENTATION`; pin an Athena snapshot date before production release |
| Transformation release | `0.1.0-draft` |
| Synthetic source-data version | `1.0.0` |
| Mapping version | `0.1.0-draft` |

Concept identifiers are vocabulary-version-dependent. Documentation uses semantic placeholders unless a concept was verified against the pinned snapshot. A release MUST fail if `vocabulary_snapshot = UNPINNED-DOCUMENTATION` outside documentation/test environments.

Vocabulary upgrades are treated as mapping changes: rerun source-concept resolution, standard mappings, domain assignment, and affected conformance/reconciliation tests. Never silently carry an identifier across snapshots.

Related: [vocabulary mapping](../02-engineering/05-vocabulary-mapping.md) · [release process](../02-engineering/10-release-process.md) · [[unmapped-concepts]]

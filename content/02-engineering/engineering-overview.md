---
title: "Engineering Lifecycle"
type: engineering
status: stable
engineering_stages: [profiling, ingestion, staging, normalization, vocabulary-mapping, omop-transformation, data-quality, clinical-validation, incremental-loads, release]
tags: [omop, ehr, etl]
---

# Engineering Lifecycle

```text
versioned source snapshot → immutable raw → typed staging → normalized semantics
→ vocabulary decisions → OMOP tables → quality and clinical review → release manifest
```

| Stage | Persistent result | Release-blocking gate |
|---|---|---|
| [Profiling](01-source-profiling.md) | profile report | source contract understood |
| [Raw ingestion](02-raw-ingestion.md) | immutable batches | checksums and counts match |
| [Staging](03-staging-model.md) | typed current/history views | valid keys and types |
| [Normalization](04-normalization.md) | canonical events | semantic rules deterministic |
| [Vocabulary mapping](05-vocabulary-mapping.md) | versioned mappings | coverage/domain thresholds pass |
| [OMOP transformation](06-omop-transformation.md) | CDM and crosswalks | keys and lineage resolve |
| [Data quality](07-data-quality.md) | machine-readable findings | critical checks pass |
| [Clinical validation](08-clinical-validation.md) | signed review | scenarios plausible |
| [Incremental loads](09-incremental-loads.md) | change ledger | replay equals rebuild |
| [Release](10-release-process.md) | immutable package | manifest complete |

Every stage is parameterized by `run_id`, source cutoff, rule version, mapping version, CDM version, and vocabulary snapshot. Rerunning identical inputs MUST produce identical business rows and crosswalk identifiers.

Related: [[quality-framework]] · [[source-provenance]] · [[release-history]]

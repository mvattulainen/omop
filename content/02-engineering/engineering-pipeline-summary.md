---
title: "Engineering Pipeline Summary"
type: engineering
status: stable
engineering_stages:
  - profiling
  - ingestion
  - staging
  - normalization
  - vocabulary-mapping
  - omop-transformation
  - data-quality
  - clinical-validation
  - incremental-loads
  - release
tags:
  - omop
  - ehr
  - etl
  - summary
---

# Engineering Pipeline Summary

## 1. Source profiling

Profile versioned EHR extracts before transformation by measuring table counts, null rates, key uniqueness, value distributions, timestamp semantics, code systems and orphaned relationships. Schema changes, undeclared values or material drift produce findings or quarantine rather than silently changing transformation rules.

## 2. Raw ingestion

Capture accepted source batches exactly as received in immutable raw partitions. Record checksums, row counts, extraction intervals, schema fingerprints, batch identifiers and ingestion timestamps so every load can be verified and replayed without altering source values.

## 3. Staging

Convert raw fields into declared data types while preserving their original representations and maintaining both historical and current-state views. Deduplicate source versions deterministically, normalize timestamps, record parsing failures and ensure staged, rejected and raw row counts reconcile.

## 4. Semantic normalization

Transform source-specific records into canonical patient, encounter, diagnosis, procedure, medication, laboratory and observation events. Apply deterministic semantic rules—such as distinguishing a held medication from an administered dose—while preserving original values, statuses, timestamps and source identifiers in lineage.

## 5. Vocabulary mapping

Resolve each coded event through the chain `source value → source concept → standard concept → OMOP domain` using a declared vocabulary snapshot and versioned local mappings. Invalid, expired, ambiguous or unmapped codes are preserved and reported rather than guessed, and the standard concept's domain determines the destination table.

## 6. OMOP transformation

Materialize OMOP CDM 5.4 rows from normalized, vocabulary-routed events, loading PERSON and visit structures before dependent clinical facts and relationships. Persistent crosswalks provide stable identifiers, while every target row retains links to its source record, transformation rule, mapping version and run.

## 7. Data-quality validation

Run structural, conformance, plausibility and source-to-OMOP reconciliation checks against the candidate database. Critical findings block release, errors require correction or an approved time-limited exception, and validation never modifies the data it evaluates.

## 8. Clinical validation

Confirm that technically valid OMOP records still represent the intended clinical meaning by comparing source and OMOP timelines, canonical patient scenarios and stratified distributions. Reviewers verify visit structures, diagnoses, measurements, units and medication semantics and provide a versioned sign-off or return problems to the responsible stage.

## 9. Incremental loading

Apply inserted, corrected, deleted, late-arriving and patient-merge records without changing established OMOP identities. Recompute affected partitions, retain change and exclusion history, and verify that ordered incremental replay produces the same business content and crosswalk identifiers as a full rebuild.

## 10. Release

Promote a validated candidate into an immutable, reproducible OMOP release containing the database snapshot, ETL and mapping versions, source cutoff, CDM and vocabulary versions, quality reports, clinical approval, known issues, release notes and checksums. Promotion is atomic and permitted only after required gates pass and the vocabulary snapshot has been pinned.

## Related pages

- [Detailed engineering lifecycle](engineering-overview.md)
- [Quality framework](../04-Quality/quality-framework.md)
- [Release process](10-release-process.md)

Obsidian: [[engineering-overview]] · [[quality-framework]] · [[10-release-process]]

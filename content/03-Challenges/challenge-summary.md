---
title: "Transformation Challenges Summary"
type: challenge
status: stable
omop_domains:
  - ALL
source_entities:
  - all
engineering_stages:
  - normalization
  - vocabulary-mapping
  - omop-transformation
  - incremental-loads
tags:
  - omop
  - ehr
  - challenges
  - summary
---

# Transformation Challenges Summary

The challenges are ordered by cross-domain impact, patient-safety implications and the severity of errors they can introduce into downstream analysis.

## 1. Patient identity resolution

Multiple EHR registrations may represent the same patient, but demographic similarity alone is unsafe evidence for merging them. The vault follows only explicit merge relationships to a terminal winning identity, rejects merge cycles, assigns a persistent `person_id`, re-parents derived records to the winner and retains all losing identifiers in auditable lineage.

## 2. Source provenance and lineage

An OMOP row often combines source facts, identity resolution, visit reconstruction, transformation rules and vocabulary mappings, so source-value fields alone are insufficient. Every target row must link to its contributing source records and versions, ingestion batches, crosswalks, transformation and mapping versions, vocabulary snapshot, run identifier and inclusion or exclusion decision.

## 3. Visit reconstruction

Adjacent emergency and inpatient encounters and nested care segments can cause invalid aggregation or duplication. The adopted rule preserves eligible top-level encounters as separate `VISIT_OCCURRENCE` rows, links related transfers chronologically and represents nested units such as ICU stays as `VISIT_DETAIL` rows within the appropriate parent visit.

## 4. Local-code mapping

Institution-specific codes cannot be reliably standardized through display-name matching because their meanings and validity can change between catalogue versions. Local mappings must therefore be curated, reviewed and versioned by code system, value and validity interval, with supporting evidence, target concept, OMOP domain, constraints and approval status retained.

## 5. OMOP domain assignment

The source table containing an event does not necessarily determine its OMOP destination; for example, an EHR observation may actually be a measurement. The pipeline first maps the source value to a valid standard concept and then routes the event according to that concept's OMOP domain, quarantining unsupported or ambiguous mappings rather than forcing them into a convenient table.

## 6. Clinical timestamp selection

EHR records may contain ordered, performed, collected, resulted, recorded and updated timestamps that represent different moments. The pipeline chooses the timestamp according to clinical meaning—such as collection time for laboratory measurements and administration time for given medication doses—while retaining alternative timestamps, original offsets and any documented fallback rule.

## 7. Duplicate-event handling

Retransmissions, corrected versions, billing copies and genuinely repeated clinical events may appear deceptively similar. Technical duplicates are collapsed using stable source identities and versions, corrected records supersede earlier versions and cross-table semantic duplicates are combined only under approved evidence-based rules, while true recurring events remain separate.

## 8. Unmapped concepts and `concept_id = 0`

An unresolved code still proves that the source recorded an event even when its standardized meaning is unknown. When permitted by the CDM, the pipeline uses `concept_id = 0`, preserves the original code and display value, records the mapping failure reason, measures unmapped rates and adds the code to a mapping backlog instead of guessing or discarding it.

## 9. Late-arriving data

A clinically old record may arrive after an OMOP release and alter visit relationships, observation-period boundaries, deduplication groups or reconciliation results. The incremental pipeline reopens every affected patient, encounter and dependency partition, applies the event using its clinical rather than arrival time and preserves all existing target identifiers while verifying that replay matches a full rebuild.

## 10. Medication order versus administration

A medication order represents prescribing intent, whereas a completed administration provides evidence that a dose was actually given. For inpatient treatment, only administrations with a `given` status generate dose-level `DRUG_EXPOSURE` rows; held, refused or cancelled doses remain in exclusion lineage, and the related order supplies context without creating a duplicate exposure.

## 11. Laboratory-unit normalization

Laboratory values can be seriously distorted when similar-looking units use different dimensions or scales. The pipeline maps and checks the source unit against the measurement concept, applies only verified analyte-specific conversions, retains the original value and unit with the conversion formula and version, and otherwise keeps the original compatible representation unchanged.

## Related pages

- [Challenge index](challenge-index.md)
- [Engineering pipeline summary](../02-engineering/engineering-pipeline-summary.md)
- [Quality framework](../04-Quality/quality-framework.md)

Obsidian: [[challenge-index]] · [[engineering-pipeline-summary]] · [[quality-framework]]

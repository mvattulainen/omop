---
title: "EHR to OMOP Transformation Vault"
type: meta
status: stable
tags: [omop, ehr, transformation]
---

# EHR to OMOP Transformation Vault

This public, Obsidian-friendly vault is executable documentation for transforming a wholly synthetic EHR into an OMOP CDM release. It connects source meaning, deterministic ETL rules, vocabulary decisions, target rows, and validation.

No record in this repository describes a real person. Exact vocabulary concept identifiers are intentionally represented by semantic placeholders until they are resolved against the declared vocabulary snapshot.

## Start here

- [Vault home](../index.md)
- [Source model](../01-source-ehr/source-model-overview.md)
- [Engineering lifecycle](../02-engineering/engineering-overview.md)
- [OMOP domains](../03-omop-domains/index.md)
- [Transformation challenges](../03-Challenges/challenge-index.md)
- [Worked examples](../05-examples/index.md)
- [Quality framework](../04-Quality/quality-framework.md)
- [OMOP version policy](omop-version-policy.md)
- [Vault conventions](vault-conventions.md)

## Reproducible release

The current documentation release is `0.1.0-draft`, synthetic source version `1.0.0`, targeting OMOP CDM `5.4`. A production vocabulary date is not asserted; see the [version policy](omop-version-policy.md) and [release process](../02-engineering/10-release-process.md).

## Repository layout

| Area | What it answers |
|---|---|
| `01-source-ehr` | What does the imaginary EHR record mean? |
| `02-engineering` | How is the pipeline executed and released? |
| `03-omop-domains` | How is each OMOP table populated? |
| `03-Challenges` | How are ambiguous transformation problems resolved? |
| `04-Quality` | How is structural, conformance, plausibility, and reconciliation quality tested? |
| `05-examples` | What does an end-to-end patient story look like? |
| `05-Meta/assets/data` | What stable synthetic records underpin the examples? |

Obsidian wikilinks are included for graph navigation. Relative Markdown links provide GitHub navigation.

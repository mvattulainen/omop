---
title: "Vault Conventions"
type: meta
status: stable
tags: [omop, documentation, conventions]
---

# Vault Conventions

## Pages and links

Files use lowercase kebab-case. Each substantive page has YAML frontmatter and a single H1. Relative Markdown links support GitHub; wikilinks support Obsidian discovery.

## Data notation

- `NULL` is an intentional database null; blank means omitted from a display-only example.
- `<semantic placeholder>` requires resolution against the release vocabulary.
- Timestamps are ISO 8601 local EHR time with explicit `Europe/Helsinki` interpretation unless an offset is shown.
- Dates are derived only after timestamp normalization.
- All identifiers follow the [synthetic-data principles](synthetic-data-principles.md).

## Rule language

`MUST` is release-blocking, `SHOULD` requires documented exception, and `MAY` is optional. Transformation mappings label direct copy, lookup, semantic mapping, calculation, derived relationship, default, intentional null, or unsupported.

## Provenance

Clinical provenance describes who/what/when in care. Technical lineage links a target row to source record and rule version. Vocabulary provenance records source vocabulary, standard mapping, and vocabulary snapshot.

## Status

`draft` is incomplete or unverified; `stable` is internally reviewed; `deprecated` is retained only for historical navigation.

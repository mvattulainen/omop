---
title: "Synthetic Data Principles"
type: meta
status: stable
tags: [omop, synthetic-data, privacy]
---

# Synthetic Data Principles

All names, identifiers, dates, clinical stories, and source records are invented for this vault. They are not de-identified real records.

## Stable identifiers

Patients use `P001` and `P002`; encounters use `E1001` onward; orders use `O2001`; administrations use `A3001`; specimens use `S4001`; source records use `SRC-*`. OMOP examples use stable integers recorded in crosswalks.

## Coherence rules

- Birth precedes every event.
- Events link only to declared patients and encounters.
- Held medication doses do not become exposures.
- Encounter segments remain within the parent encounter.
- The same source record keeps the same meaning wherever reused.

Canonical records live in [synthetic-ehr.csv](assets/data/synthetic-ehr.csv) and are summarized in [source model overview](../01-source-ehr/source-model-overview.md).

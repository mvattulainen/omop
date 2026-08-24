---
title: "Transformation Challenge Index"
type: meta
status: stable
tags: [omop, challenges, navigation]
---

# Transformation Challenge Index

| Challenge | Adopted principle | Primary stage |
|---|---|---|
| [Patient identity](patient-identity-resolution.md) | Explicit merges only; persistent winner crosswalk | normalization |
| [Visit reconstruction](visit-reconstruction.md) | Preserve top-level contacts; details for segments | OMOP transformation |
| [Local codes](local-code-mapping.md) | Versioned evidence-backed mapping | vocabulary mapping |
| [Domain assignment](domain-assignment.md) | Standard concept domain routes the event | vocabulary mapping |
| [Order vs administration](medication-order-vs-administration.md) | Intent and given evidence are distinct | normalization |
| [Lab units](lab-unit-normalization.md) | Convert only compatible, verified units | normalization/mapping |
| [Timestamps](timestamp-selection.md) | Choose by clinical meaning, retain alternatives | normalization |
| [Duplicate events](duplicate-events.md) | Deduplicate source representations, not similar facts | normalization |
| [Unmapped concepts](unmapped-concepts.md) | Preserve source and measure zero; never guess | vocabulary mapping |
| [Source provenance](source-provenance.md) | Clinical, technical, vocabulary lineage all survive | all |
| [Late-arriving data](late-arriving-data.md) | Reopen impacted partitions, keep IDs stable | incremental loads |

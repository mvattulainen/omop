---
title: "Release History"
type: meta
status: stable
tags: [omop, release, changelog]
---

# Release History

| Vault release | Date | Source | CDM | Vocabulary | Notes |
|---|---|---|---|---|---|
| `0.1.0-draft` | 2026-08-23 | `1.0.0` | 5.4 | unpinned documentation snapshot | Initial complete vault implementation; semantic concept placeholders intentionally remain. |

## Known issues

- Exact source and standard concept identifiers await a pinned vocabulary snapshot.
- Executable ETL is outside this documentation release; SQL-like rules describe required behavior.
- Device, death, note, and specimen are documented but not populated by the three canonical patient journeys.
